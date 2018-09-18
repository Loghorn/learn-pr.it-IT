La libreria client di Archiviazione di Azure offre un modello a oggetti usato per interagire con account di archiviazione di Azure. Questo modello viene usato per connettersi rapidamente a un account di archiviazione di Azure e usare le API dei servizi di Archiviazione di Azure. 

## <a name="azure-storage-client-library-object-model"></a>Modello a oggetti della libreria client di Archiviazione di Azure

::: zone pivot="csharp"

Alla base del modello a oggetti per l'account di archiviazione nella libreria client .NET Core vi è la classe `CloudStorageAccount`. Il modo più semplice per inizializzare il modello a oggetti consiste nell'usare `CloudStorageAccount.Parse` o `CloudStorageAccount.TryParse` per analizzare la stringa di connessione.

> [!NOTE]
> La libreria client non tenterà di connettersi finché non viene richiamata un'operazione che la richiede. `Parse()` e `TryParse()` garantiscono solo che la stringa di connessione abbia un formato corretto, ma non verificano che l'account sia presente o che la chiave sia corretta. 

L'istanza `CloudStorageAccount` risultante restituita dalla chiamata al metodo `Parse()` o `TryParse()` espone metodi per creare un oggetto client per l'accesso ai servizi di archiviazione BLOB, file, code e tabelle. 

Il frammento di codice seguente mostra un esempio di creazione di un client per l'uso dell'archiviazione BLOB:

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient()
```

`CloudStorageAccount` e gli oggetti client sono oggetti leggeri e possono essere creati on demand o inizialmente per essere poi condivisi nell'applicazione. Un approccio standard consiste nell'usare `CloudStorageAccount.TryParse()` accanto al punto di ingresso dell'applicazione per creare un'istanza e renderla disponibile nell'applicazione per creare istanze client.

Dopo aver creato un oggetto client per un tipo di archiviazione specifico, è possibile usare metodi per eseguire le operazioni effettive. I metodi che effettuano chiamate di rete sono intenzionalmente asincroni. In .NET vengono usate le parole chiave `async` e `await` per lavorare con questi metodi in modo efficiente.

Ad esempio, è possibile usare `CloudBlobClient` per creare un _contenitore BLOB_ e caricare un file nell'archiviazione blob.

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

::: zone-pivot="javascript"

Alla base del modello a oggetti per l'account di archiviazione nella **libreria client di Archiviazione di Microsoft Azure per Node.js e JavaScript** c'è l'oggetto `azurestorage`. Viene creato aggiungendo il modulo **azure-storage**all'app mediante un'istruzione `require`.

```javascript
const storage = require('azure-storage');
```

Questo oggetto fornisce una serie di metodi _factory_ che creano oggetti specifici da usare con ogni elemento di Archiviazione di Azure. Per creare ogni oggetto si chiamano metodi `createXXX`.

| Tipo | Metodo | Restituisce |
|--------|---------|-------------|
| **Blob** | `createBlobService` | `BlobService` |
| **Tabella** | `createTableService` | `TableService` |
| **Coda** | `createQueueService` | `QueueService` |
| **File** | `createFileService` | `FileService` |

> [!NOTE]
> La libreria client non tenterà di connettersi finché non viene richiamata un'operazione che la richiede. Ognuno di questi metodi `create` restituisce un oggetto leggero che rappresenta l'accesso al tipo di archiviazione e non convalida la connessione né la chiave di accesso in uso. 

Dopo aver creato un oggetto servizio per un tipo di archiviazione specifico, è possibile usare metodi per eseguire le operazioni effettive. I metodi che effettuano chiamate di rete sono intenzionalmente asincroni. Attualmente la libreria supporta _callback_ per restituire risultati asincroni. Ad esempio, ecco il codice per creare un contenitore BLOB.

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

Questo approccio funziona correttamente, ma tende a causare l'aggiunta nei callback di una grande quantità di codice, che può restare non gestito. Un approccio migliore in JavaScript consiste nell'usare _promesse_ per lavorare con questi metodi. Sono disponibili numerose ottime librerie per la conversione dei metodi di callback in promesse.

In questo caso si useranno la funzionalità `util.promisify` di Node e l'oggetto `BlobService` per creare il contenitore e caricare un file nell'archiviazione BLOB. Inoltre, si useranno le parole chiave `async` e `await` per lavorare con le promesse in modo più naturale.

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