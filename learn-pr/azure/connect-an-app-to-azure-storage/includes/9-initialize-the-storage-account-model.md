<span data-ttu-id="07973-101">La libreria client di Archiviazione di Azure offre un modello a oggetti usato per interagire con account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="07973-101">The Azure Storage client library provides an object model that is used to interact with Azure storage accounts.</span></span> <span data-ttu-id="07973-102">Questo modello viene usato per connettersi rapidamente a un account di archiviazione di Azure e usare le API dei servizi di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="07973-102">It's used to quickly connect to an Azure storage account and use the Azure Storage service APIs.</span></span> 

## <a name="azure-storage-client-library-object-model"></a><span data-ttu-id="07973-103">Modello a oggetti della libreria client di Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="07973-103">Azure Storage client library object model</span></span>

<span data-ttu-id="07973-104">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="07973-104">::: zone pivot="csharp"</span></span>

<span data-ttu-id="07973-105">Alla base del modello a oggetti per l'account di archiviazione nella libreria client .NET Core vi è la classe `CloudStorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="07973-105">The foundation of the storage account object model in the .NET Core client library is the class `CloudStorageAccount`.</span></span> <span data-ttu-id="07973-106">Il modo più semplice per inizializzare il modello a oggetti consiste nell'usare `CloudStorageAccount.Parse` o `CloudStorageAccount.TryParse` per analizzare la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="07973-106">The simplest way to initialize the object model is to use `CloudStorageAccount.Parse` or `CloudStorageAccount.TryParse` to parse the connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="07973-107">La libreria client non tenterà di connettersi finché non viene richiamata un'operazione che la richiede.</span><span class="sxs-lookup"><span data-stu-id="07973-107">The client library will not attempt to connect until an operation is invoked that requires it.</span></span> <span data-ttu-id="07973-108">`Parse()` e `TryParse()` garantiscono solo che la stringa di connessione abbia un formato corretto, ma non verificano che l'account sia presente o che la chiave sia corretta.</span><span class="sxs-lookup"><span data-stu-id="07973-108">`Parse()` and `TryParse()` only guarantee that the connection string is well-formatted; they don't verify that the account exists or that the key is correct.</span></span> 

<span data-ttu-id="07973-109">L'istanza `CloudStorageAccount` risultante restituita dalla chiamata al metodo `Parse()` o `TryParse()` espone metodi per creare un oggetto client per l'accesso ai servizi di archiviazione BLOB, file, code e tabelle.</span><span class="sxs-lookup"><span data-stu-id="07973-109">The resulting `CloudStorageAccount` instance returned from the `Parse()` or `TryParse()` method call exposes methods to create a client objects to access the Azure Blob, Files, Queue and Table storage services.</span></span> 

<span data-ttu-id="07973-110">Il frammento di codice seguente mostra un esempio di creazione di un client per l'uso dell'archiviazione BLOB:</span><span class="sxs-lookup"><span data-stu-id="07973-110">The code snippet below shows an example of creating a client to use for blob storage:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient()
```

<span data-ttu-id="07973-111">`CloudStorageAccount` e gli oggetti client sono oggetti leggeri e possono essere creati on demand o inizialmente per essere poi condivisi nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="07973-111">`CloudStorageAccount` and the client objects are lightweight and can be created on demand or created up front to be shared within your application.</span></span> <span data-ttu-id="07973-112">Un approccio standard consiste nell'usare `CloudStorageAccount.TryParse()` accanto al punto di ingresso dell'applicazione per creare un'istanza e renderla disponibile nell'applicazione per creare istanze client.</span><span class="sxs-lookup"><span data-stu-id="07973-112">A standard approach is to use `CloudStorageAccount.TryParse()` near the entry point of your application to create an instance, and make it available within your application for creating client instances.</span></span>

<span data-ttu-id="07973-113">Dopo aver creato un oggetto client per un tipo di archiviazione specifico, è possibile usare metodi per eseguire le operazioni effettive.</span><span class="sxs-lookup"><span data-stu-id="07973-113">Once you have a client object to a specific storage type, you can use methods to perform actual work.</span></span> <span data-ttu-id="07973-114">I metodi che effettuano chiamate di rete sono intenzionalmente asincroni. In .NET vengono usate le parole chiave `async` e `await` per lavorare con questi metodi in modo efficiente.</span><span class="sxs-lookup"><span data-stu-id="07973-114">Methods which make network calls are intentionally asynchronous - in .NET we use the `async` and `await` keywords to work with these methods efficiently.</span></span>

<span data-ttu-id="07973-115">Ad esempio, è possibile usare `CloudBlobClient` per creare un _contenitore BLOB_ e caricare un file nell'archiviazione blob.</span><span class="sxs-lookup"><span data-stu-id="07973-115">As an example, we can use the `CloudBlobClient` to create a _blob container_ and upload a file to blob storage.</span></span>

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

<span data-ttu-id="07973-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="07973-116">::: zone-end</span></span>

<span data-ttu-id="07973-117">::: zone-pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="07973-117">::: zone-pivot="javascript"</span></span>

<span data-ttu-id="07973-118">Alla base del modello a oggetti per l'account di archiviazione nella **libreria client di Archiviazione di Microsoft Azure per Node.js e JavaScript** c'è l'oggetto `azurestorage`.</span><span class="sxs-lookup"><span data-stu-id="07973-118">The foundation of the storage account object model in the **Microsoft Azure Storage Client Library for Node.js and JavaScript** is the `azurestorage` object.</span></span> <span data-ttu-id="07973-119">Viene creato aggiungendo il modulo **azure-storage**all'app mediante un'istruzione `require`.</span><span class="sxs-lookup"><span data-stu-id="07973-119">This is created by adding the **azure-storage** module to your app through a `require` statement.</span></span>

```javascript
const storage = require('azure-storage');
```

<span data-ttu-id="07973-120">Questo oggetto fornisce una serie di metodi _factory_ che creano oggetti specifici da usare con ogni elemento di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="07973-120">This object provides a series of _factory_ methods that create specific objects to work with each facet of Azure storage.</span></span> <span data-ttu-id="07973-121">Per creare ogni oggetto si chiamano metodi `createXXX`.</span><span class="sxs-lookup"><span data-stu-id="07973-121">You call `createXXX` methods to create each object.</span></span>

| <span data-ttu-id="07973-122">Tipo</span><span class="sxs-lookup"><span data-stu-id="07973-122">Type</span></span> | <span data-ttu-id="07973-123">Metodo</span><span class="sxs-lookup"><span data-stu-id="07973-123">Method</span></span> | <span data-ttu-id="07973-124">Restituisce</span><span class="sxs-lookup"><span data-stu-id="07973-124">Returns</span></span> |
|--------|---------|-------------|
| <span data-ttu-id="07973-125">**Blob**</span><span class="sxs-lookup"><span data-stu-id="07973-125">**Blob**</span></span> | `createBlobService` | `BlobService` |
| <span data-ttu-id="07973-126">**Tabella**</span><span class="sxs-lookup"><span data-stu-id="07973-126">**Table**</span></span> | `createTableService` | `TableService` |
| <span data-ttu-id="07973-127">**Coda**</span><span class="sxs-lookup"><span data-stu-id="07973-127">**Queue**</span></span> | `createQueueService` | `QueueService` |
| <span data-ttu-id="07973-128">**File**</span><span class="sxs-lookup"><span data-stu-id="07973-128">**File**</span></span> | `createFileService` | `FileService` |

> [!NOTE]
> <span data-ttu-id="07973-129">La libreria client non tenterà di connettersi finché non viene richiamata un'operazione che la richiede.</span><span class="sxs-lookup"><span data-stu-id="07973-129">The client library will not attempt to connect until an operation is invoked that requires it.</span></span> <span data-ttu-id="07973-130">Ognuno di questi metodi `create` restituisce un oggetto leggero che rappresenta l'accesso al tipo di archiviazione e non convalida la connessione né la chiave di accesso in uso.</span><span class="sxs-lookup"><span data-stu-id="07973-130">Each of these `create` methods return a lightweight object representing access to the storage type - it does not validate the connection or the access key being used.</span></span> 

<span data-ttu-id="07973-131">Dopo aver creato un oggetto servizio per un tipo di archiviazione specifico, è possibile usare metodi per eseguire le operazioni effettive.</span><span class="sxs-lookup"><span data-stu-id="07973-131">Once you have a service object to a specific storage type, you can use methods to perform actual work.</span></span> <span data-ttu-id="07973-132">I metodi che effettuano chiamate di rete sono intenzionalmente asincroni.</span><span class="sxs-lookup"><span data-stu-id="07973-132">Methods which make network calls are intentionally asynchronous.</span></span> <span data-ttu-id="07973-133">Attualmente la libreria supporta _callback_ per restituire risultati asincroni.</span><span class="sxs-lookup"><span data-stu-id="07973-133">The library current supports _callbacks_ to return asynchronous results.</span></span> <span data-ttu-id="07973-134">Ad esempio, ecco il codice per creare un contenitore BLOB.</span><span class="sxs-lookup"><span data-stu-id="07973-134">For example, here is code that creates a blob container.</span></span>

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

<span data-ttu-id="07973-135">Questo approccio funziona correttamente, ma tende a causare l'aggiunta nei callback di una grande quantità di codice, che può restare non gestito.</span><span class="sxs-lookup"><span data-stu-id="07973-135">This approach works fine, but tends to lead to a lot of code being added into the callbacks which can get unmanagement.</span></span> <span data-ttu-id="07973-136">Un approccio migliore in JavaScript consiste nell'usare _promesse_ per lavorare con questi metodi.</span><span class="sxs-lookup"><span data-stu-id="07973-136">A better approach in JavaScript is to use _promises_ to work with these methods.</span></span> <span data-ttu-id="07973-137">Sono disponibili numerose ottime librerie per la conversione dei metodi di callback in promesse.</span><span class="sxs-lookup"><span data-stu-id="07973-137">There are several great libraries which will convert callback-style methods into promises - you can pick the one you prefer.</span></span>

<span data-ttu-id="07973-138">In questo caso si useranno la funzionalità `util.promisify` di Node e l'oggetto `BlobService` per creare il contenitore e caricare un file nell'archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="07973-138">Here, we'll use the `util.promisify` feature from Node and use the `BlobService` to create the container and upload a file to blob storage.</span></span> <span data-ttu-id="07973-139">Inoltre, si useranno le parole chiave `async` e `await` per lavorare con le promesse in modo più naturale.</span><span class="sxs-lookup"><span data-stu-id="07973-139">In addition, we'll use the `async` and `await` keywords to work with the promises a bit more naturally.</span></span>

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
<span data-ttu-id="07973-140">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="07973-140">::: zone-end</span></span>

<span data-ttu-id="07973-141">Proviamo a farlo nell'app.</span><span class="sxs-lookup"><span data-stu-id="07973-141">Let's try this in our app.</span></span>