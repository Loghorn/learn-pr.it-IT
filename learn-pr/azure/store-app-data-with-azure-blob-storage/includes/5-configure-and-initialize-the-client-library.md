Di seguito è riportato il tipico flusso di lavoro per le app che usano l'archiviazione BLOB di Azure:

1. **Recuperare la configurazione**: all'avvio, caricare la configurazione, ad esempio la stringa di connessione con la chiave dell'account. Questa operazione è necessaria per autenticare le chiamate API.
1. **Inizializzare i client**: usare la stringa di connessione per inizializzare la libreria client di Archiviazione di Azure. Verranno creati gli oggetti che l'app userà per collaborare con l'API di archiviazione BLOB.
1. **Usare**: consentire alle chiamate API con la libreria client di operare su contenitori e blob.

## <a name="configure-your-connection-string"></a>Configurare la stringa di connessione

Prima di scrivere qualsiasi codice, è necessario disporre della stringa di connessione per l'account di archiviazione che verrà usato. 

La stringa di connessione include la chiave dell'account. La chiave dell'account viene considerata un segreto e deve essere archiviata in modo sicuro. Archivieremo la stringa di connessione in un'impostazione dell'applicazione del Servizio app. Un'impostazione dell'applicazione è una posizione sicura per i segreti dell'applicazione, ma non supporta lo sviluppo locale e non è una soluzione affidabile end-to-end di per sé.

> [!WARNING]
> **Non inserire chiavi dell'account di archiviazione nel codice o nei file di configurazione non protetti.** Le chiavi dell'account di archiviazione abilitano l'accesso completo all'account di archiviazione. La verifica della perdita di una chiave può comportare danni irreversibili e fatture molto consistenti. Vedere la sezione relativa alle risorse aggiuntive al termine di questo modulo per altre informazioni sull'archiviazione e consigli su come eseguire il ripristino da una chiave persa.

## <a name="initialize-the-blob-storage-object-model"></a>Inizializzare il modello oggetto di archiviazione BLOB

In Archiviazione di Azure e SDK per .NET Core, il modello standard per l'uso dell'archiviazione BLOB prevede i passaggi seguenti:

1. Chiamare `CloudStorageAccount.Parse` (o `TryParse`) con la stringa di connessione per ottenere `CloudStorageAccount`.
1. Chiamare `CreateCloudBlobClient` in `CloudStorageAccount` per ottenere `CloudBlobClient`.
1. Chiamare `GetContainerReference` in `CloudBlobClient` per ottenere `CloudBlobContainer`.
1. Usare metodi per ottenere un elenco di blob e/o ottenere riferimenti a singoli blob per caricare e scaricare i dati nei contenitori.

Nel codice, i passaggi da 1&ndash;3 sono simili a:

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

Nessuno di tali codici di inizializzazione effettua le chiamate sulla rete. Ciò significa che non vengono generate eccezioni che si verificano a partire da informazioni non corrette fino a un momento successivo, ad esempio, la chiamata a `GetContainerReference` avrà esito positivo indipendentemente dal fatto che il contenitore è effettivamente presente nell'account.

## <a name="create-containers-at-startup"></a>Creare contenitori all'avvio

Una procedura consigliata per le applicazioni è la creazione di contenitori necessari nel codice, anche se è assodato che i contenitori verranno anticipati. La chiamata di `CreateIfNotExistsAsync` in `CloudBlobContainer` è il modo migliore per eseguire questa operazione e dovrebbe essere usata per creare ogni contenitore che sappiamo essere necessario prima di usarlo.

`CreateIfNotExistsAsync` *non*  effettua una chiamata di rete ad Archiviazione di Azure. La procedura consigliata consiste nel chiamarlo una sola volta all'avvio e non ogni volta che si accede a un contenitore.

## <a name="exercise"></a>Esercizio

### <a name="clone-and-explore-the-unfinished-app"></a>Clonare ed esplorare l'app non completata

Prima di tutto, è possibile clonare l'app iniziale da GitHub. Nel terminale Cloud Shell, eseguire il comando seguente per ottenere una copia del codice sorgente e aprirla nell'editor:

```console
git clone TODO
cd TODO
code .
```

Aprire il file `Controllers/FilesController.cs`.

Questo controller implementa un'API con tre azioni:

* **Indice** (GET /api/Files) restituisce un elenco di URL, uno per ogni file che è stato caricato. Il front-end di app chiama questo metodo per compilare un elenco di collegamenti ipertestuali nei file caricati.
* **Caricamento** (POST /api/Files) riceve un file caricato e lo salva.
* **Download** (GET/api/Files/{file-name}) scarica un singolo file in base al nome.

Ogni metodo usa un'istanza `IStorage` denominata `storage` per l'esecuzione delle operazioni. In `Models/BlobStorage.cs` è presente un'implementazione di incompleta di `IStorage`.

### <a name="add-the-nuget-package"></a>Aggiungere il pacchetto NuGet

In primo luogo, aggiungere un riferimento all'SDK di Archiviazione di Azure. Eseguire i comandi seguenti dal terminale:

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

Ciò ci consentirà di sapere che stiamo usando la versione più recente della libreria del client di archiviazione BLOB.

### <a name="configure"></a>Configurare

L'app iniziale include già le operazioni di base di configurazione necessarie. Il parametro del costruttore `IOptions<AzureStorageConfig>` in `BlobStorage` ha due proprietà: la stringa di connessione dell'account di archiviazione e il nome del contenitore in cui l'app archivierà i blob. Nel metodo `ConfigureServices` di `Startup.cs` è presente un codice che carica i valori dalla configurazione all'avvio dell'app.

In questo esercizio, eseguiremo l'app nel Servizio app di Azure, aggiungeremo quindi i valori di configurazione alle impostazioni dell'applicazione di Servizio app in un secondo momento. Per il momento, non dobbiamo eseguire operazioni correlate alla configurazione.

### <a name="initialize"></a>Inizializzare

Aprire `BlobStorage.cs`.

Percorso del metodo `Initialize`. Quando viene usato per la prima volta, l'app chiamerà questo metodo. Per curiosità, è possibile esaminare `ConfigureServices` in `Startup.cs` per visualizzare informazioni su questa procedura. 

`Initialize` è la posizione in cui si vuole creare il contenitore se non ancora esistente. Compilare `Initialize` con il codice seguente e salvare il lavoro:

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```