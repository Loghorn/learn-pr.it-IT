La libreria client di Archiviazione di Azure offre un modello a oggetti usato per interagire con account di archiviazione di Azure. Questo modello viene usato per connettersi rapidamente a un account di archiviazione di Azure e usare le API dei servizi di Archiviazione di Azure. 

## <a name="azure-storage-client-library-object-model"></a>Modello a oggetti della libreria client di Archiviazione di Azure

::: zone pivot="csharp"

Le basi del modello di oggetto account di archiviazione nella libreria client di .NET Core sono la classe `CloudStorageAccount`. Il modo più semplice per inizializzare il modello a oggetti consiste nell'usare `CloudStorageAccount.Parse` o `CloudStorageAccount.TryParse` per analizzare la stringa di connessione.

> [!NOTE]
> La libreria client non tenterà di connettersi finché non viene richiamata un'operazione che la richiede. `Parse()` e `TryParse()` garantiscono solo che la stringa di connessione abbia un formato corretto, ma non verificano che l'account sia presente o che la chiave sia corretta. 

L'oggetto risultante `CloudStorageAccount` istanza restituita dal `Parse()` o `TryParse()` chiamata al metodo espone i metodi per creare un client di oggetti di accedere ai servizi di archiviazione Blob di Azure, file, coda e tabella. 

Il frammento di codice seguente mostra un esempio di creazione di un client per l'uso dell'archiviazione BLOB:

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient()
```

`CloudStorageAccount` e gli oggetti client sono oggetti leggeri e possono essere creati on demand o inizialmente per essere poi condivisi nell'applicazione. Un approccio standard consiste nell'usare `CloudStorageAccount.TryParse()` accanto al punto di ingresso dell'applicazione per creare un'istanza e renderla disponibile nell'applicazione per creare istanze client.

Dopo aver creato un oggetto client per un tipo di archiviazione specifico, è possibile utilizzare i metodi per eseguire il lavoro effettivo. I metodi che effettuano chiamate di rete sono intenzionalmente asincroni, in .NET viene usato il `async` e `await` parole chiave per lavorare con questi metodi in modo efficiente.

Ad esempio, è possibile usare la `CloudBlobClient` per creare un _contenitore blob_ e caricare un file nell'archiviazione blob.

```csharp
// Create a local CloudBlobContainer object. No network call.
string containerName = "myblobcontainer";
CloudBlobContainer blobContainer = blobClient.GetContainerReference(containerName);

// This makes an actual service call to the Azure Storage service. 
// Unless this call fails, the container will have been created.
await blobContainer.CreateAsync();

// Create a local object to represent our blob. No network call.
string blobName = "myphoto";
CloudBlockBlob blob = blobContainer.GetBlockBlobReference(blobName);

// This transfers data in the file to the blob on the service.
string filename = "photo.png";
await blob.UploadFromFileAsync(fileName);
```

::: zone-end

::: zone pivot="javascript"

Le basi del modello di oggetto account di archiviazione nel **Microsoft Azure Storage Client Library per JavaScript e Node. js** è il `azurestorage` oggetto. Viene creato aggiungendo i **azure-storage** modulo all'App tramite un `require` istruzione.

```javascript
const storage = require('azure-storage');
```

Questo oggetto fornisce una serie di _factory_ i metodi che creano oggetti specifici da usare con ogni facet di archiviazione di Azure. Si chiama `createXXX` metodi per creare ogni oggetto.

| Tipo | Metodo | Restituisce |
|--------|---------|-------------|
| **Blob** | `createBlobService` | `BlobService` |
| **Tabella** | `createTableService` | `TableService` |
| **Coda** | `createQueueService` | `QueueService` |
| **File** | `createFileService` | `FileService` |

> [!NOTE]
> La libreria client non tenterà di connettersi finché non viene richiamata un'operazione che la richiede. Ognuno di questi `create` metodi restituiscono un oggetto leggero che rappresenta l'accesso per il tipo di archiviazione&mdash;non convalida la connessione o la chiave di accesso in uso.

Dopo aver creato un oggetto servizio a un tipo di archiviazione specifico, è possibile usare i metodi per eseguire il lavoro effettivo. I metodi che effettuano chiamate di rete sono intenzionalmente asincroni. La libreria supporta attualmente _richiamate_ per restituire risultati asincroni. Ad esempio, ecco il codice che crea un contenitore blob.

```javascript
var azure = require('azure-storage');
var blobService = azure.createBlobService();

blobService.createContainerIfNotExists('myblobcontainer', function(err, result, response) {
  if (!err) {
    // if result.created = true, container was created.
    // if result.created = false, container already existed.
  }
});
```

Questo approccio funziona correttamente, ma tende a portare a una grande quantità di codice aggiunto nel callback, che può ottenere ingestibile. Un approccio migliore in JavaScript consiste nell'usare _promette_ per lavorare con questi metodi. Sono disponibili numerose librerie eccezionale che convertirà i metodi di callback in stile in promesse&mdash;è possibile scegliere quello desiderato.

In questo caso, si userà il `util.promisify` funzionalità dal nodo e si usa il `BlobService` per creare il contenitore e caricare un file nell'archiviazione blob. Inoltre, si userà il `async` e `await` parole chiave per un po' più possibilità di lavorare con le promesse.

```javascript
const util = require('util');
const storage = require('azure-storage');

const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);
const uploadBlobAsync = util.promisify(blobService.createBlockBlobFromLocalFile).bind(blobService);

async function main() {
    try {
        // This makes an actual service call to the Azure Storage service. 
        // Unless this call fails, the container will have been created.
        await createContainerAsync(containerName);

        // This transfers data in the file to the blob on the service.
        var uploadResult = await uploadBlobAsync(containerName, "myphoto", "photo.png");
        if (uploadResult) {
            console.log("blob uploaded");
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

main();
```
::: zone-end

Proviamo a farlo nell'app.