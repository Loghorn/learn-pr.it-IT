Di seguito è riportato il flusso di lavoro tipico per le app che usano l'archiviazione BLOB di Azure:

1. **Recuperare la configurazione**: all'avvio, caricare la configurazione dell'account di archiviazione. Solitamente si tratta della stringa di connessione dell'account di archiviazione.

1. **Inizializzare i client**: usare la stringa di connessione per inizializzare la libreria client di Archiviazione di Azure. Verranno creati gli oggetti che l'app userà per collaborare con l'API di archiviazione BLOB.

1. **Usare**: consentire alle chiamate API con la libreria client di operare su contenitori e blob.

## <a name="configure-your-connection-string"></a>Configurare la stringa di connessione

Prima di scrivere qualsiasi codice, è necessario avere la stringa di connessione per l'account di archiviazione che verrà usato.

Le stringhe di connessione dell'account di archiviazione includono la chiave dell'account. La chiave dell'account viene considerata un segreto e deve essere archiviata in modo sicuro. In questo caso la stringa di connessione verrà archiviata in un'impostazione dell'applicazione del servizio app. Un'impostazione dell'applicazione del servizio app è una posizione sicura per i segreti dell'applicazione, ma non supporta lo sviluppo locale e non è una soluzione affidabile end-to-end di per sé.

> [!WARNING]
> **Non inserire chiavi dell'account di archiviazione nel codice o nei file di configurazione non protetti.** Le chiavi dell'account di archiviazione abilitano l'accesso completo all'account di archiviazione. La perdita di una chiave può comportare danni irreversibili e fatture molto consistenti. Vedere la sezione Altre informazioni al termine di questo modulo per una guida sull'archiviazione e consigli su come eseguire il ripristino da una chiave persa.

## <a name="initialize-the-blob-storage-object-model"></a>Inizializzare il modello oggetto di archiviazione BLOB

In Archiviazione di Azure e SDK per .NET Core, il modello standard per l'uso dell'archiviazione BLOB prevede i passaggi seguenti:

1. Chiamare `CloudStorageAccount.Parse` (o `TryParse`) con la stringa di connessione per ottenere `CloudStorageAccount`.

1. Chiamare `CreateCloudBlobClient` in `CloudStorageAccount` per ottenere `CloudBlobClient`.

1. Chiamare `GetContainerReference` in `CloudBlobClient` per ottenere `CloudBlobContainer`.

1. Usare metodi per ottenere un elenco di blob e/o ottenere riferimenti a singoli blob per caricare e scaricare i dati nei contenitori.

Nel codice i passaggi 1&ndash;3 sono simili a:

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

Nessuno di tali codici di inizializzazione esegue le chiamate sulla rete. Ciò significa che alcune eccezioni che si verificano a causa di informazioni non corrette verranno generate solo in un momento successivo. Ad esempio, la chiamata a `CloudStorageAccount.Parse` genera un'eccezione immediatamente se la stringa di connessione non è formattata correttamente, ma non verrà generata alcuna eccezione se l'account di archiviazione a cui fa riferimento una stringa di connessione non esiste.

## <a name="create-containers-at-startup"></a>Creare contenitori all'avvio

La chiamata di `CreateIfNotExistsAsync` su un `CloudBlobContainer` è il modo migliore per creare un contenitore quando l'applicazione si avvia o quando tenta di usarlo per la prima volta.

`CreateIfNotExistsAsync` non genera un'eccezione se il contenitore esiste già, ma esegue una chiamata di rete ad Archiviazione di Azure. La chiamata viene eseguita una volta durante l'inizializzazione, non ogni volta che si prova a usare un contenitore.

## <a name="exercise"></a>Esercizio

### <a name="clone-and-explore-the-unfinished-app"></a>Clonare ed esplorare l'app non completata

Prima di tutto, è possibile clonare l'app iniziale da GitHub. Nel terminale Cloud Shell eseguire il comando seguente per ottenere una copia del codice sorgente e aprirla nell'editor:

**Aggiornamento TODO a URL archivio finale**

```console
git clone https://github.com/nickwalkmsft/FileUploader.git
cd FileUploader
code .
```

Aprire il file `Controllers/FilesController.cs`. Non vi è alcuna operazione da eseguire in questo passaggio, ma è necessario verificare il funzionamento dell'app.

Questo controller implementa un'API con tre azioni:

- **Indice** (GET /api/Files) restituisce un elenco di URL, uno per ogni file che è stato caricato. Il front-end di app chiama questo metodo per compilare un elenco di collegamenti ipertestuali nei file caricati.
- **Caricamento** (POST /api/Files) riceve un file caricato e lo salva.
- **Download** (GET/api/Files/{nomefile}) scarica un singolo file in base al nome.

Ogni metodo usa un'istanza `IStorage` denominata `storage` per l'esecuzione delle operazioni. In `Models/BlobStorage.cs` è presente un'implementazione incompleta di `IStorage` che è necessario sistemare.

### <a name="add-the-nuget-package"></a>Aggiungere il pacchetto NuGet

In primo luogo, aggiungere un riferimento all'SDK di Archiviazione di Azure. Eseguire i comandi seguenti dal terminale:

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

Ciò ci consentirà di sapere che stiamo usando la versione più recente della libreria del client di archiviazione BLOB.

### <a name="configure"></a>Configurare

I valori di configurazione necessari per eseguire l'app sono la stringa di connessione dell'account di archiviazione e il nome del contenitore dell'app che verrà usata per archiviare i file. In questa unità verrà eseguita l'app solo nel Servizio app di Azure e quindi verrà seguita la procedura consigliata del servizio app e verranno archiviati i valori nelle impostazioni applicazione del servizio app. Questa operazione verrà eseguita quando verrà creata l'istanza del servizio app, non in questo momento.

Quando sarà il momento di *usare* la configurazione, l'app iniziale includerà già le operazioni di base necessarie. Il parametro del costruttore `IOptions<AzureStorageConfig>` in `BlobStorage` ha due proprietà: la stringa di connessione dell'account di archiviazione e il nome del contenitore in cui l'app archivierà i BLOB. Nel metodo `ConfigureServices` di `Startup.cs` è presente un codice che carica i valori dalla configurazione all'avvio dell'app.

### <a name="initialize"></a>Inizializzare

Aprire `Models/BlobStorage.cs`. Aggiungere le istruzioni `using` seguenti all'inizio del file per eseguire la preparazione al codice che si vuole aggiungere durante l'esercizio.

```csharp
using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

Individuare il metodo `Initialize`. Quando `BlobStorage` viene usato per la prima volta, l'app chiamerà questo metodo. Per curiosità, è possibile esaminare `ConfigureServices` in `Startup.cs` per visualizzare informazioni su questa procedura.

`Initialize` è la posizione in cui si vuole creare il contenitore se non esiste già. Sostituire l'implementazione corrente di `Initialize` con il codice seguente e salvare il lavoro:

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```