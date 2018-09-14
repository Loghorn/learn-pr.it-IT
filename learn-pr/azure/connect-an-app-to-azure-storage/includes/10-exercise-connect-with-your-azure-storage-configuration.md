::: pivot zona = "csharp" aggiungere codice per recuperare la stringa di connessione dalla configurazione e usarlo per connettersi all'account di archiviazione di Azure.

## <a name="retrieve-the-connection-string"></a>Recuperare la stringa di connessione

1. Selezionare **Program.cs** per aprirlo nell'editor del codice.

1. Aggiungere un `using` all'inizio del file a cui fare riferimento l'istruzione di `Microsoft.WindowsAzure.Storage` dello spazio dei nomi:

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. In fondo il `Main` metodo, aggiungere la riga seguente per recuperare la stringa di connessione di account di archiviazione di Azure dal file di configurazione. Il valore passato _key_ deve corrispondere al nome usato nelle **appSettings. JSON** file.

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>Creare un client BLOB

1. Usare il metodo statico `CloudStorageAccount.TryParse` metodo per creare un `CloudStorageAccount` dell'oggetto - accetta la stringa di connessione e un `out` parametro per restituire l'oggetto creato. Restituisce un `bool` valore che indica se analizzato correttamente la stringa.
    - In caso contrario, un messaggio nella console di output e il metodo restituisce.

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString, 
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. Utilizzare l'oggetto restituito `CloudStorageAccount` oggetto per creare un client di blob.

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Successivamente, usare il client di blob per recuperare un riferimento a un contenitore denominato "photoblobs". Molto, ad esempio i nomi di account, i nomi di contenitore Blob devono essere composto da lettere e numeri e lettere minuscole.

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. Usare il `CreateIfNotExistsAsync` metodo per creare il contenitore. Restituisce un `bool` che indica se il contenitore è stato creato. Questa Store in una variabile denominata `created`.
    - Si noti che questo è un **async** metodo - che significa che verrà eseguita una chiamata di rete effettivi.
    - È necessario usare il `await` parole chiave per ottenere il `bool` risultato.

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. Poiché si sta usando la `await` parola chiave, proseguire e modificare la firma per il `Main` metodo sia `async` e restituire un `Task`.

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

1. L'output, infine, se è stato creato il contenitore Blob.

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. Salvare il file.

Il file finale dovrebbe essere simile al seguente se si desidera controllare il lavoro.

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;

namespace PhotoSharingApp
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();
            var connectionString = configuration["StorageAccountConnectionString"];

            if (!CloudStorageAccount.TryParse(connectionString, out CloudStorageAccount storageAccount))
            {
                Console.WriteLine("Unable to parse connection string");
                return;
            }

            var blobClient = storageAccount.CreateCloudBlobClient();
            var blobContainer = blobClient.GetContainerReference("photoblobs");
            bool created = await blobContainer.CreateIfNotExistsAsync();

            Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
        }
    }
}
```

## <a name="use-c-71-to-build-our-app"></a>Usare c# 7.1 per compilare l'App nell'app

Supporto per `async` e `await` su `Main` metodi è stato aggiunto al linguaggio c# 7.1. Ciò potrebbe non essere la versione predefinita del compilatore in uso. Verifichiamo che si usa tale versione del compilatore mediante l'impostazione nel file di configurazione.

1. Aprire il `PhotoSharingApp.csproj` e aggiungere `<LangVersion>7.1</LangVersion>` per il `PropertyGroup` che specifica il `TargetFramework`. Quando hai finito, dovrebbe essere simile al seguente:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <LangVersion>7.1</LangVersion> 
        <TargetFramework>netcoreapp2.0</TargetFramework>
      </PropertyGroup>
      ...
    </Project>
    ```

1. Salvare il file.

## <a name="run-the-app"></a>Esecuzione dell'app

1. Compilare ed eseguire l'applicazione. **Nota:** assicurarsi di essere nella directory di lavoro corretta.

    ```bash
    dotnet run
    ```

::: zone-end

::: pivot zona = "javascript" aggiungere codice per la connessione all'account di archiviazione di Azure usando la stringa di connessione archiviate. La libreria client di Azure userà automaticamente il **AZURE_STORAGE_CONNECTION_STRING** variabile di ambiente per ottenere la stringa di connessione.

## <a name="create-a-blob-client"></a>Creare un client BLOB

1. Aprire **index. js** nell'editor del codice.

1. Per iniziare, tra cui il **azure-storage** modulo. Il modulo di Store in una variabile denominata **archiviazione**.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();
    
    const storage = require('azure-storage');
    ```

1. Successivamente, a destra in seguito, usare il **storage** oggetto da creare il `BlobService` dell'oggetto e lo archivia in un oggetto globale denominato **blobService**. Tenere presente che questi sono oggetti leggeri che rappresenta l'accesso all'account di archiviazione.

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. Aggiungere una costante per rappresentare il contenitore a cui che si desidera creare. Denominiamo il contenitore "photoblobs".

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a>Creare un contenitore

È possibile usare il `BlobService` oggetto per lavorare con le API di blob in archiviazione di Azure. Come accennato in precedenza, tutte le API che effettuano chiamate di rete sono asincrone per mantenere reattiva l'app. Il `createContainerIfNotExists` metodo è uno di questi metodi. Useremo _promette_ per gestire il callback che contiene la risposta.

1. Uso `util.promisify` per richiedere la versione di callback di `createContainerIfNotExists` e trasformarlo in un metodo che restituiscono un oggetto promise.
    - Poiché il metodo di callback è in un oggetto, assicurarsi di aggiungere un `bind` chiamare alla fine per connetterla a tale contesto.
    - Assegnare il valore restituito a una costante nella parte superiore del file denominato `createContainerAsync`, come illustrato di seguito.

```javascript
const storage = require('azure-storage');
const blobService = storage.createBlobService();

const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';
```

1. Rimuovere il "Hello, World!" output da `main()`.

1. Chiamare il nuovo `createContainerAsync` promise.
    - Passare il **containerName** costante.
    - Applicare il `await` una parola chiave per la chiamata.
    - Il wrapping della chiamata in un `try`  /  `catch` costruire e restituire un errore.

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```
    
1. Il `createContainerAsync` suggerimento restituisce il primo valore da sottostante `createContainerIfNotExists`, che è il risultato di contenitore. Sono incluse informazioni su se è stato creato il contenitore o meno. Acquisire il risultato in una variabile e di output se è stato creato il contenitore di base di `result.created` proprietà.

    ```javascript
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. Infine, decorare il `main` utilizzabile con il `async` (parola chiave).
        
1. Salvare il file.

Il file finale dovrebbe essere simile al seguente, se si desidera controllare il lavoro.

```javascript
#!/usr/bin/env node

require('dotenv').load();

const util = require('util');

const storage = require('azure-storage');
const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';

async function run() {
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

run();
```

## <a name="run-the-app"></a>Esecuzione dell'app

1. Compilare ed eseguire l'applicazione. **Nota:** assicurarsi di essere nella directory di lavoro corretta.

    ```bash
    node index.js
    ```

::: zone-end

È necessario segnalare che è stato creato il contenitore blob. Se lo si esegue una seconda volta, dovrebbero essere che già esistente.

Per verificare il contenitore:

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Passare all'account di archiviazione. È possibile usare la **tutte le risorse** sezione per trovare l'account di archiviazione o la ricerca in base al nome dalle _casella di ricerca_ nella parte superiore della finestra del portale. 

1. Selezionare il **BLOB** voce dell'account di archiviazione nel **servizi Blob** sezione.

1. Dovrebbero vedere le **photoblobs** contenitore nel pannello dei BLOB. È possibile eliminare il contenitore con il pulsante "..." sul lato destro della voce di tentare di ricrearla con l'app.

> [!NOTE]
> Il contenitore non verrà più visualizzato dal portale molto rapidamente, ma sono necessari alcuni minuti per eliminare effettivamente. Si otterrà una risposta di errore da Azure, mentre questa viene eliminata se si prova a ricrearla.

## <a name="delete-the-app"></a>Eliminare l'app
Se si decide che non si vuole mantenere il codice sorgente dell'applicazione nell'ambiente Cloud Shell, è possibile usare il comando seguente per rimuovere la cartella e tutto il contenuto.

```bash
rm -r PhotoSharingApp/
```
