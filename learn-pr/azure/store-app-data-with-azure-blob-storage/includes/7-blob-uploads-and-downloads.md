<span data-ttu-id="f8018-101">Dopo aver ottenuto un riferimento a un blob, è possibile caricare e scaricare i dati.</span><span class="sxs-lookup"><span data-stu-id="f8018-101">Once we have a reference to a blob, we can upload and download data.</span></span> <span data-ttu-id="f8018-102">Gli oggetti `ICloudBlob` dispongono di metodi `Upload` e `Download` che supportano le matrici di byte, i flussi e i file come origini e destinazioni.</span><span class="sxs-lookup"><span data-stu-id="f8018-102">`ICloudBlob` objects have `Upload` and `Download` methods that support byte arrays, streams, and files as sources and targets.</span></span> <span data-ttu-id="f8018-103">Altri tipi specifici dispongono di metodi aggiuntivi per motivi di praticità &mdash;, ad esempio, `CloudBlockBlob` supporta il caricamento e download di stringhe con `UploadTextAsync` e `DownloadTextAsync`.</span><span class="sxs-lookup"><span data-stu-id="f8018-103">Specific types have additional methods for convenience &mdash; for example, `CloudBlockBlob` supports uploading and downloading strings with `UploadTextAsync` and `DownloadTextAsync`.</span></span>

## <a name="creating-new-blobs"></a><span data-ttu-id="f8018-104">Creazione di nuovi blob</span><span class="sxs-lookup"><span data-stu-id="f8018-104">Creating new blobs</span></span>

<span data-ttu-id="f8018-105">Per creare un nuovo blob, chiamare uno dei metodi `Upload` su un blob che non esiste.</span><span class="sxs-lookup"><span data-stu-id="f8018-105">To create a new blob, you call one of the `Upload` methods on a blob that doesn't exist.</span></span> <span data-ttu-id="f8018-106">Ciò comporta due operazioni: crea il blob e carica i dati.</span><span class="sxs-lookup"><span data-stu-id="f8018-106">This does two things: creates the blob and uploads the data.</span></span> 

## <a name="moving-data-to-and-from-blobs"></a><span data-ttu-id="f8018-107">Spostamento dei dati da e verso i blob</span><span class="sxs-lookup"><span data-stu-id="f8018-107">Moving data to and from blobs</span></span>

<span data-ttu-id="f8018-108">Lo spostamento dei dati da e verso un blob è un'operazione di rete che richiede tempo.</span><span class="sxs-lookup"><span data-stu-id="f8018-108">Moving data to and from a blob is a network operation that takes time.</span></span> <span data-ttu-id="f8018-109">Nell'archivio SDK di Azure per .NET Core, tutti i metodi che richiedono attività di rete restituiscono `Task`s, in modo da assicurarsi che i metodi del controller vengano `async` in modo appropriato e che si sta `await`ando le chiamate dei metodi e non `Wait`ando su di esse.</span><span class="sxs-lookup"><span data-stu-id="f8018-109">In the Azure Storage SDK for .NET Core, all methods that require network activity return `Task`s, so make sure your controller methods are `async` as appropriate and that you are `await`ing method calls and not `Wait`ing on them.</span></span>

<span data-ttu-id="f8018-110">Un consiglio comune quando si lavora con gli oggetti dati di grandi dimensioni consiste nell'usare flussi anziché strutture in memoria, ad esempio stringhe o matrici di byte.</span><span class="sxs-lookup"><span data-stu-id="f8018-110">A common recommendation when working with large data objects is to use streams instead of in-memory structures like byte arrays or strings.</span></span> <span data-ttu-id="f8018-111">Questo evita di memorizzare nel buffer l'intero contenuto in memoria prima di inviarlo alla destinazione.</span><span class="sxs-lookup"><span data-stu-id="f8018-111">This avoids buffering the full content in memory before sending it to the target.</span></span> <span data-ttu-id="f8018-112">ASP.NET Core supporta la lettura e la scrittura di flussi da richieste e risposte.</span><span class="sxs-lookup"><span data-stu-id="f8018-112">ASP.NET Core supports reading and writing streams from requests and responses.</span></span>

## <a name="concurrent-access"></a><span data-ttu-id="f8018-113">Accesso simultaneo</span><span class="sxs-lookup"><span data-stu-id="f8018-113">Concurrent access</span></span>

<span data-ttu-id="f8018-114">È possibile che altri processi possano aggiungere, modificare o eliminare i blob usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="f8018-114">It is possible that other processes may be adding, changing, or deleting blobs as your app is using them.</span></span> <span data-ttu-id="f8018-115">Codificare sempre in modo sicuro e considerare i problemi causati dalla concorrenza, come i blob eliminati proprio quando si prova a scaricare o i blob il cui contenuto viene modificato quando non previsto.</span><span class="sxs-lookup"><span data-stu-id="f8018-115">Always code defensively and think about problems caused by concurrency, such as blobs that are deleted right as you try to download from them, or blobs whose contents change when you don't expect them to.</span></span> <span data-ttu-id="f8018-116">Consultare la sezione Risorse aggiuntive alla fine di questo modulo per informazioni sull'uso di lease di blob e AccessConditions per gestire l'accesso a blob simultanei.</span><span class="sxs-lookup"><span data-stu-id="f8018-116">See the Additional Resources section at the end of this module for information about using AccessConditions and blob leases to manage concurrent blob access.</span></span>

## <a name="exercise"></a><span data-ttu-id="f8018-117">Esercizio</span><span class="sxs-lookup"><span data-stu-id="f8018-117">Exercise</span></span>

<span data-ttu-id="f8018-118">È possibile completare l'app mediante l'aggiunta di codice per il caricamento e il download e quindi distribuirla nel servizio app di Azure per effettuare il test.</span><span class="sxs-lookup"><span data-stu-id="f8018-118">Let's finish our app by adding upload and download code, then deploy it to Azure App Service for testing.</span></span>

### <a name="upload"></a><span data-ttu-id="f8018-119">Caricamento</span><span class="sxs-lookup"><span data-stu-id="f8018-119">Upload</span></span>

<span data-ttu-id="f8018-120">Per caricare un blob, verrà implementato il metodo `BlobStorage.Save` che usa `GetBlockBlobReference` per ottenere un `CloudBlockBlob` dal contenitore.</span><span class="sxs-lookup"><span data-stu-id="f8018-120">To upload a blob, we'll implement the `BlobStorage.Save` method using `GetBlockBlobReference` to get a `CloudBlockBlob` from the container.</span></span> <span data-ttu-id="f8018-121">`FilesController.Upload` passa il flusso di file a `Save`, quindi è possibile usare `UploadFromStreamAsync` per eseguire il caricamento per la massima efficienza.</span><span class="sxs-lookup"><span data-stu-id="f8018-121">`FilesController.Upload` passes the file stream to `Save`, so we can use `UploadFromStreamAsync` to perform the upload for maximum efficiency.</span></span>

<span data-ttu-id="f8018-122">Aprire `BlobStorage.cs` nell'editor e inserire l'implementazione di `Save` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8018-122">Open `BlobStorage.cs` in the editor and fill in the `Save` implementation with the following code:</span></span>

```csharp
public Task Save(Stream fileStream, string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(name);
    return blockBlob.UploadFromStreamAsync(fileStream);
}
```

> [!NOTE]
> <span data-ttu-id="f8018-123">Il codice di caricamento basato sul flusso illustrato di seguito è più efficiente rispetto alla lettura del file in una matrice di byte prima dell'invio all'archivio BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="f8018-123">The stream-based upload code shown here is more efficient than reading the file into a byte array before sending it to Azure Blob storage.</span></span> <span data-ttu-id="f8018-124">Tuttavia, la tecnica `IFormFile` usata per ottenere il file dal client non è una vera implementazione di flusso end-to-end ed è adatta solo per la gestione di caricamenti di file di piccole dimensioni.</span><span class="sxs-lookup"><span data-stu-id="f8018-124">However, the `IFormFile` technique we use to get the file from the client is not a true end-to-end streaming implementation and is only appropriate for handling uploads of small files.</span></span> <span data-ttu-id="f8018-125">Consultare la sezione Risorse aggiuntive alla fine di questo modulo per informazioni sui caricamenti di file con flusso completo.</span><span class="sxs-lookup"><span data-stu-id="f8018-125">See the Additional Resources section at the end of this module for information about fully streamed file uploads.</span></span>

### <a name="download"></a><span data-ttu-id="f8018-126">Download</span><span class="sxs-lookup"><span data-stu-id="f8018-126">Download</span></span>

<span data-ttu-id="f8018-127">`BlobStorage.Load` restituisce un `Stream`, vale a dire che il codice non deve spostare fisicamente i byte da un archivio BLOB &mdash; è sufficiente restituire un riferimento al flusso del blob.</span><span class="sxs-lookup"><span data-stu-id="f8018-127">`BlobStorage.Load` returns a `Stream`, meaning that our code doesn't need to physically move the bytes from Blob storage at all &mdash; we just need to return a reference to the blob stream.</span></span> <span data-ttu-id="f8018-128">A questo scopo, usare `OpenReadAsync`.</span><span class="sxs-lookup"><span data-stu-id="f8018-128">We can do that with `OpenReadAsync`.</span></span> <span data-ttu-id="f8018-129">ASP.NET Core gestirà la lettura e la chiusura del flusso una volta compilata la risposta del client.</span><span class="sxs-lookup"><span data-stu-id="f8018-129">ASP.NET Core will handle reading and closing the stream when it builds the client response.</span></span>

<span data-ttu-id="f8018-130">Inserire `Load` con questo codice:</span><span class="sxs-lookup"><span data-stu-id="f8018-130">Fill in `Load` with this code:</span></span>

```csharp
public Task<Stream> Load(string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.GetBlobReference(name).OpenReadAsync();
}
```

### <a name="deploy-and-run-in-azure"></a><span data-ttu-id="f8018-131">Pubblicare ed eseguire l'app su Azure</span><span class="sxs-lookup"><span data-stu-id="f8018-131">Deploy and run in Azure</span></span>

<span data-ttu-id="f8018-132">L'app è stata completata &mdash; è possibile distribuirla e visualizzarne il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="f8018-132">Our app is finished &mdash; let's deploy it and see it work.</span></span> <span data-ttu-id="f8018-133">Eseguire il codice seguente sul terminale di Azure Cloud Shell per compilare il codice e distribuirla in una nuova istanza del Servizio app.</span><span class="sxs-lookup"><span data-stu-id="f8018-133">Run the following code in the Azure Cloud Shell terminal to build our code and deploy it to a new App Service instance.</span></span> <span data-ttu-id="f8018-134">È anche possibile aggiungere la stringa di connessione dell'account di archiviazione alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="f8018-134">We also add our storage account connection string to configuration.</span></span>

```console

```

<span data-ttu-id="f8018-135">...</span><span class="sxs-lookup"><span data-stu-id="f8018-135">...</span></span>

<span data-ttu-id="f8018-136">Caricare e scaricare alcuni file per testare l'app, quindi eseguire il comando seguente nella shell per visualizzare i blob che sono stati caricati:</span><span class="sxs-lookup"><span data-stu-id="f8018-136">Upload and download some files to test the app, then run the following in the shell to see the blobs that have been uploaded:</span></span>

```console

```

<span data-ttu-id="f8018-137">**Imm TODO dal portale**</span><span class="sxs-lookup"><span data-stu-id="f8018-137">**TODO pic from portal**</span></span>