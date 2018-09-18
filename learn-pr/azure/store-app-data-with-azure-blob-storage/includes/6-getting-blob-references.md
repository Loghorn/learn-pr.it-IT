<span data-ttu-id="3f587-101">Per usare un singolo BLOB in Azure Storage SDK per .NET Core è necessario un *riferimento a BLOB*, ossia un'istanza di un oggetto `ICloudBlob`.</span><span class="sxs-lookup"><span data-stu-id="3f587-101">Working with an individual blob in the Azure Storage SDK for .NET Core requires a *blob reference* &mdash; an instance of an `ICloudBlob` object.</span></span>

<span data-ttu-id="3f587-102">È possibile ottenere un `ICloudBlob` richiedendolo con il nome del BLOB oppure selezionandolo in un elenco dei BLOB nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="3f587-102">You can get an `ICloudBlob` by requesting it with the blob's name or selecting it from a list of blobs in the container.</span></span> <span data-ttu-id="3f587-103">In entrambi i casi è necessario un oggetto `CloudBlobContainer`, il cui recupero è stato illustrato nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="3f587-103">Both require a `CloudBlobContainer`, which we saw how to get in the last unit.</span></span>

## <a name="getting-blobs-by-name"></a><span data-ttu-id="3f587-104">Recupero di BLOB in base al nome</span><span class="sxs-lookup"><span data-stu-id="3f587-104">Getting blobs by name</span></span>

<span data-ttu-id="3f587-105">Chiamare uno dei metodi `GetXXXReference` in un `CloudBlobContainer` per ottenere un `ICloudBlob` in base al nome.</span><span class="sxs-lookup"><span data-stu-id="3f587-105">Call one of the `GetXXXReference` methods on a `CloudBlobContainer` to get an `ICloudBlob` by name.</span></span> <span data-ttu-id="3f587-106">Se si conosce il tipo di BLOB che si sta recuperando, usare uno dei metodi specifici (`GetBlockBlobReference`, `GetAppendBlobReference`, o `GetPageBlobReference`) per ottenere un oggetto che include i metodi e le proprietà specifici per quel tipo di BLOB.</span><span class="sxs-lookup"><span data-stu-id="3f587-106">If you know the type of the blob you are retrieving, use one of the specific methods (`GetBlockBlobReference`, `GetAppendBlobReference`, or `GetPageBlobReference`) to get an object that includes methods and properties tailored for that blob type.</span></span>

<span data-ttu-id="3f587-107">Nessuno di questi metodi esegue una chiamata di rete o verifica se il BLOB di destinazione effettivamente esista.</span><span class="sxs-lookup"><span data-stu-id="3f587-107">None of these methods make network calls, nor do they confirm whether or not the targeted blob actually exists.</span></span> <span data-ttu-id="3f587-108">Creano solo un oggetto di riferimento al BLOB in locale, che può quindi essere usato per chiamare i metodi che *operano* nella rete e interagiscono con i BLOB nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="3f587-108">They only create a blob reference object locally, which can then be used to call methods that *do* operate over the network and interact with blobs in storage.</span></span> <span data-ttu-id="3f587-109">Un metodo separato, `GetBlobReferenceFromServerAsync`, chiama l'API di archiviazione BLOB e genera un'eccezione se il BLOB non esiste.</span><span class="sxs-lookup"><span data-stu-id="3f587-109">A separate method, `GetBlobReferenceFromServerAsync`, does call the Blob storage API and will throw an exception if the blob doesn't already exist.</span></span>

## <a name="listing-blobs-in-a-container"></a><span data-ttu-id="3f587-110">Elenco dei BLOB in un contenitore</span><span class="sxs-lookup"><span data-stu-id="3f587-110">Listing blobs in a container</span></span>

<span data-ttu-id="3f587-111">È possibile ottenere un elenco dei BLOB in un contenitore usando il metodo `ListBlobsSegmentedAsync` di `CloudBlobContainer`.</span><span class="sxs-lookup"><span data-stu-id="3f587-111">You can get a list of the blobs in a container using `CloudBlobContainer`'s `ListBlobsSegmentedAsync` method.</span></span> <span data-ttu-id="3f587-112">*Segmented* fa riferimento alle pagine di risultati separate restituite. Non è mai garantito che una singola chiamata a `ListBlobsSegmentedAsync` restituisca tutti i risultati in una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="3f587-112">*Segmented* refers to the separate pages of results returned &mdash; a single call to `ListBlobsSegmentedAsync` is never guaranteed to return all the results in a single page.</span></span> <span data-ttu-id="3f587-113">Per scorrere le pagine potrebbe essere necessario ripetere la chiamata con il valore di `ContinuationToken` restituito.</span><span class="sxs-lookup"><span data-stu-id="3f587-113">We may need to call it repeatedly using the `ContinuationToken` it returns to work our way through the pages.</span></span> <span data-ttu-id="3f587-114">Il codice per elencare i BLOB diventa così un po' più complesso rispetto al codice per il caricamento o il download, ma è disponibile un modello standard che può essere usato per ottenere ogni BLOB in un contenitore:</span><span class="sxs-lookup"><span data-stu-id="3f587-114">This makes the code for listing blobs a little more complex than the code for uploading or downloading, but there's a standard pattern you can use to get every blob in a container:</span></span>

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

<span data-ttu-id="3f587-115">In questo modo, `ListBlobsSegmentedAsync` verrà chiamato ripetutamente finché il valore di `continuationToken` non è `null`, che indica la fine dei risultati.</span><span class="sxs-lookup"><span data-stu-id="3f587-115">This will call `ListBlobsSegmentedAsync` repeatedly until `continuationToken` is `null`, which signals the end of the results.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f587-116">Non dare mai per scontato che i risultati di `ListBlobsSegmentedAsync` vengano recapitati in una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="3f587-116">Never assume that `ListBlobsSegmentedAsync` results will arrive in a single page.</span></span> <span data-ttu-id="3f587-117">Verificare sempre la presenza di un token di continuità e usarlo se è presente.</span><span class="sxs-lookup"><span data-stu-id="3f587-117">Always check for a continuation token and use it if it's present.</span></span>

### <a name="processing-list-results"></a><span data-ttu-id="3f587-118">Elaborazione dei risultati elencati</span><span class="sxs-lookup"><span data-stu-id="3f587-118">Processing list results</span></span>

<span data-ttu-id="3f587-119">L'oggetto ottenuto da `ListBlobsSegmentedAsync` contiene una proprietà `Results` di tipo `IEnumerable<IListBlobItem>`.</span><span class="sxs-lookup"><span data-stu-id="3f587-119">The object you'll get back from `ListBlobsSegmentedAsync` contains a `Results` property of type `IEnumerable<IListBlobItem>`.</span></span> <span data-ttu-id="3f587-120">L'interfaccia `IListBlobItem` include solo un numero limitato di proprietà relative al contenitore e all'URL del BLOB e non è molto utile da sola.</span><span class="sxs-lookup"><span data-stu-id="3f587-120">The `IListBlobItem` interface includes only a handful of properties about the blob's container and URL, and isn't very useful by itself.</span></span>

<span data-ttu-id="3f587-121">Per ottenere gli oggetti BLOB utili al di fuori di `Results`, è possibile usare il metodo `OfType<>` per filtrare ed eseguire il cast dei risultati per tipi di oggetti BLOB più specifici.</span><span class="sxs-lookup"><span data-stu-id="3f587-121">To get useful blob objects out of `Results`, you can use the `OfType<>` method to filter and cast the results to more specific blob object types.</span></span> <span data-ttu-id="3f587-122">Di seguito sono disponibili alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="3f587-122">Here are a few examples:</span></span>

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

> [!TIP]
> <span data-ttu-id="3f587-123">L'uso di `OfType<>` richiede un riferimento allo spazio dei nomi `System.Linq` (`using System.Linq;`).</span><span class="sxs-lookup"><span data-stu-id="3f587-123">Using `OfType<>` requires a reference to the `System.Linq` namespace (`using System.Linq;`).</span></span>

## <a name="exercise"></a><span data-ttu-id="3f587-124">Esercizio</span><span class="sxs-lookup"><span data-stu-id="3f587-124">Exercise</span></span>

<span data-ttu-id="3f587-125">Una delle funzionalità dell'app richiede il recupero di un elenco di BLOB dall'API.</span><span class="sxs-lookup"><span data-stu-id="3f587-125">One of the features in our app requires getting a list of blobs from the API.</span></span> <span data-ttu-id="3f587-126">Si userà il modello illustrato sopra per elencare tutti i BLOB nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="3f587-126">We'll use the pattern shown above to list all the blobs in our container.</span></span> <span data-ttu-id="3f587-127">Elaborando l'elenco, si ottiene il nome di ogni BLOB.</span><span class="sxs-lookup"><span data-stu-id="3f587-127">As we process the list, we get the name of each blob.</span></span>

<span data-ttu-id="3f587-128">Usando l'editor, sostituire `GetNames` in `BlobStorage.cs` con il codice seguente e salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3f587-128">Using the editor, replace `GetNames` in `BlobStorage.cs` with the following code and save your changes.</span></span>

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

> [!TIP]
> <span data-ttu-id="3f587-129">Si noti che la firma del metodo deve ora specificare `async`.</span><span class="sxs-lookup"><span data-stu-id="3f587-129">Note that the method signature now needs to specify `async`.</span></span>

<span data-ttu-id="3f587-130">I nomi restituiti da questo metodo vengono elaborati da `FilesController` per la trasformazione in URL.</span><span class="sxs-lookup"><span data-stu-id="3f587-130">The names returned by this method are processed by `FilesController` to turn them into URLs.</span></span> <span data-ttu-id="3f587-131">Quando vengono restituiti al client, ne viene eseguito il rendering come collegamenti ipertestuali nella pagina.</span><span class="sxs-lookup"><span data-stu-id="3f587-131">When they are returned to the client, they are rendered as hyperlinks on the page.</span></span>