<span data-ttu-id="f4012-101">Per usare un singolo BLOB in Azure Storage SDK per .NET Core è necessario un *riferimento a BLOB*, ossia un'istanza di un oggetto `ICloudBlob`.</span><span class="sxs-lookup"><span data-stu-id="f4012-101">Working with an individual blob in the Azure Storage SDK for .NET Core requires a *blob reference* &mdash; an instance of an `ICloudBlob` object.</span></span>

<span data-ttu-id="f4012-102">È possibile ottenere un `ICloudBlob` richiedendolo con il nome del BLOB oppure selezionandolo in un elenco dei BLOB nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="f4012-102">You can get an `ICloudBlob` by requesting it with the blob's name or selecting it from a list of blobs in the container.</span></span> <span data-ttu-id="f4012-103">In entrambi i casi è necessario un oggetto `CloudBlobContainer`, il cui recupero è stato illustrato nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="f4012-103">Both require a `CloudBlobContainer`, which we saw how to get in the last unit.</span></span>

## <a name="getting-blobs-by-name"></a><span data-ttu-id="f4012-104">Recupero di BLOB in base al nome</span><span class="sxs-lookup"><span data-stu-id="f4012-104">Getting blobs by name</span></span>

<span data-ttu-id="f4012-105">Chiamare uno dei metodi `GetXXXReference` in un `CloudBlobContainer` per ottenere un `ICloudBlob` in base al nome.</span><span class="sxs-lookup"><span data-stu-id="f4012-105">Call one of the `GetXXXReference` methods on a `CloudBlobContainer` to get an `ICloudBlob` by name.</span></span> <span data-ttu-id="f4012-106">Se si conosce il tipo di BLOB da recuperare, è preferibile usare un metodo più specifico, ossia `GetBlockBlobReference`, `GetAppendBlobReference` o `GetPageBlobReference`.</span><span class="sxs-lookup"><span data-stu-id="f4012-106">If you know the type of the blob you are retrieving, prefer using one of the more specific methods (`GetBlockBlobReference`, `GetAppendBlobReference`, or `GetPageBlobReference`).</span></span>

<span data-ttu-id="f4012-107">Nessuno di questi metodi effettua una chiamata di rete o verifica se il BLOB effettivamente esista.</span><span class="sxs-lookup"><span data-stu-id="f4012-107">None of these methods make a network call, nor do they confirm whether or not the blob actually exists.</span></span> <span data-ttu-id="f4012-108">Un metodo separato, `GetBlobReferenceFromServerAsync`, chiama l'API di archiviazione BLOB e genera un'eccezione se il BLOB non esiste.</span><span class="sxs-lookup"><span data-stu-id="f4012-108">A separate method, `GetBlobReferenceFromServerAsync`, does call the Blob storage API and will throw an exception if the blob doesn't already exist.</span></span>

## <a name="listing-blobs-in-a-container"></a><span data-ttu-id="f4012-109">Elenco dei BLOB in un contenitore</span><span class="sxs-lookup"><span data-stu-id="f4012-109">Listing blobs in a container</span></span>

<span data-ttu-id="f4012-110">È possibile ottenere un elenco dei BLOB in un contenitore usando il metodo `ListBlobsSegmentedAsync` di `CloudBlobContainer`.</span><span class="sxs-lookup"><span data-stu-id="f4012-110">You can get a list of the blobs in a container using `CloudBlobContainer`'s `ListBlobsSegmentedAsync` method.</span></span> <span data-ttu-id="f4012-111">*Segmented* fa riferimento alle pagine di risultati separate restituite. Non è mai garantito che una singola chiamata a `ListBlobsSegmentedAsync` restituisca tutti i risultati in una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="f4012-111">*Segmented* refers to the separate pages of results returned &mdash; a single call to `ListBlobsSegmentedAsync` is never guaranteed to return all the results in a single page.</span></span> <span data-ttu-id="f4012-112">Per scorrere le pagine potrebbe essere necessario ripetere la chiamata con il valore di `ContinuationToken` restituito.</span><span class="sxs-lookup"><span data-stu-id="f4012-112">We may need to call it repeatedly using the `ContinuationToken` it returns to work our way through the pages.</span></span> <span data-ttu-id="f4012-113">Il codice per elencare i BLOB diventa così un po' più complesso rispetto al codice per il caricamento o il download, ma è disponibile un modello standard che può essere usato per ottenere ogni BLOB in un contenitore:</span><span class="sxs-lookup"><span data-stu-id="f4012-113">This makes the code for listing blobs a little more complex than the code for uploading or downloading, but there's a standard pattern you can use to get every blob in a container:</span></span>

```csharp
BlobContinuationToken continuationToken = null;
BlobResultSegment resultSegment = null; 

do
{
    resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

    // Do work here on resultSegment.Results

    continuationToken = resultSegment.ContinuationToken;
} while (continuationToken != null);
```

<span data-ttu-id="f4012-114">In questo modo, `ListBlobsSegmentedAsync` verrà chiamato ripetutamente finché il valore di `continuationToken` non è `null`, che indica la fine dei risultati.</span><span class="sxs-lookup"><span data-stu-id="f4012-114">This will call `ListBlobsSegmentedAsync` repeatedly until `continuationToken` is `null`, which signals the end of the results.</span></span>

### <a name="processing-list-results"></a><span data-ttu-id="f4012-115">Elaborazione dei risultati elencati</span><span class="sxs-lookup"><span data-stu-id="f4012-115">Processing list results</span></span>

<span data-ttu-id="f4012-116">L'oggetto ottenuto da `ListBlobsSegmentedAsync` contiene una proprietà `Results` di tipo `IEnumerable<IListBlobItem>`.</span><span class="sxs-lookup"><span data-stu-id="f4012-116">The object you'll get back from `ListBlobsSegmentedAsync` contains a `Results` property of type `IEnumerable<IListBlobItem>`.</span></span> <span data-ttu-id="f4012-117">Gli oggetti `IListBlobItem` contengono alcune proprietà relative al contenitore e all'URL del BLOB, ma nessun metodo di caricamento o download.</span><span class="sxs-lookup"><span data-stu-id="f4012-117">`IListBlobItem`s contain a handful of properties about the blob's container and URL, but no upload or download methods.</span></span> <span data-ttu-id="f4012-118">Ciò è dovuto al fatto che alcuni oggetti dei risultati potrebbero essere `CloudBlobDirectory` che rappresentano directory virtuali anziché singoli BLOB.</span><span class="sxs-lookup"><span data-stu-id="f4012-118">This is because some of the result objects may be `CloudBlobDirectory` objects that represent virtual directories rather than individual blobs.</span></span>

<span data-ttu-id="f4012-119">Se si è interessati solo ai singoli BLOB, si può usare il metodo `OfType<>` per filtrare i risultati.</span><span class="sxs-lookup"><span data-stu-id="f4012-119">If you are only interested in individual blobs, you can use the `OfType<>` method to filter the results.</span></span> <span data-ttu-id="f4012-120">Di seguito sono disponibili alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="f4012-120">Here are a few examples:</span></span>

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

<span data-ttu-id="f4012-121">L'uso di `OfType<>` richiederà un riferimento allo spazio dei nomi `System.Linq` (`using System.Linq;`).</span><span class="sxs-lookup"><span data-stu-id="f4012-121">Using `OfType<>` will require a reference to the `System.Linq` namespace (`using System.Linq;`).</span></span>

## <a name="exercise"></a><span data-ttu-id="f4012-122">Esercizio</span><span class="sxs-lookup"><span data-stu-id="f4012-122">Exercise</span></span>

<span data-ttu-id="f4012-123">Una delle funzionalità dell'app richiede il recupero di un elenco di BLOB dall'API.</span><span class="sxs-lookup"><span data-stu-id="f4012-123">One of the features in our app requires getting a list of blobs from the API.</span></span> <span data-ttu-id="f4012-124">Si userà il modello illustrato sopra per elencare tutti i BLOB nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="f4012-124">We'll use the pattern shown above to list all the blobs in our container.</span></span> <span data-ttu-id="f4012-125">Elaborando l'elenco, si ottiene il nome di ogni BLOB.</span><span class="sxs-lookup"><span data-stu-id="f4012-125">As we process the list, we get the name of each blob.</span></span>

<span data-ttu-id="f4012-126">Aprire `BlobStorage.cs` nell'editor e inserire il codice seguente in `GetNames`:</span><span class="sxs-lookup"><span data-stu-id="f4012-126">Open `BlobStorage.cs` in the editor and fill in `GetNames` with the following code:</span></span>

```csharp
public async Task<IEnumerable<string>> GetNames()
{
    List<string> names = new List<string>();

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);

    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    do
    {
        resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

        // Get the name of each blob.
        names.AddRange(resultSegment.Results.OfType<ICloudBlob>().Select(b => b.Name));

        continuationToken = resultSegment.ContinuationToken;
    } while (continuationToken != null);

    return names;
}
```

<span data-ttu-id="f4012-127">I nomi restituiti da questo metodo vengono elaborati da `FilesController` per la trasformazione in URL.</span><span class="sxs-lookup"><span data-stu-id="f4012-127">The names returned by this method are processed by `FilesController` to turn them into URLs.</span></span> <span data-ttu-id="f4012-128">Quando vengono restituiti al client, ne viene eseguito il rendering come collegamenti ipertestuali nella pagina.</span><span class="sxs-lookup"><span data-stu-id="f4012-128">When they are returned to the client, they are rendered as hyperlinks on the page.</span></span>