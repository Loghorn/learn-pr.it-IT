<span data-ttu-id="991c8-101">Di seguito è riportato il tipico flusso di lavoro per le app che usano l'archiviazione BLOB di Azure:</span><span class="sxs-lookup"><span data-stu-id="991c8-101">The following is the typical workflow for apps that use Azure Blob storage:</span></span>

1. <span data-ttu-id="991c8-102">**Recuperare la configurazione**: all'avvio, caricare la configurazione, ad esempio la stringa di connessione con la chiave dell'account.</span><span class="sxs-lookup"><span data-stu-id="991c8-102">**Retrieve configuration**: At startup, load the configuration, such as the connection string with the account key.</span></span> <span data-ttu-id="991c8-103">Questa operazione è necessaria per autenticare le chiamate API.</span><span class="sxs-lookup"><span data-stu-id="991c8-103">This is needed to authenticate API calls.</span></span>
1. <span data-ttu-id="991c8-104">**Inizializzare i client**: usare la stringa di connessione per inizializzare la libreria client di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="991c8-104">**Initialize client**: Use the connection string to initialize the Azure Storage client library.</span></span> <span data-ttu-id="991c8-105">Verranno creati gli oggetti che l'app userà per collaborare con l'API di archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="991c8-105">This creates the objects the app will use to work with the Blob storage API.</span></span>
1. <span data-ttu-id="991c8-106">**Usare**: consentire alle chiamate API con la libreria client di operare su contenitori e blob.</span><span class="sxs-lookup"><span data-stu-id="991c8-106">**Use**: Make API calls with the client library to operate on containers and blobs.</span></span>

## <a name="configure-your-connection-string"></a><span data-ttu-id="991c8-107">Configurare la stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="991c8-107">Configure your connection string</span></span>

<span data-ttu-id="991c8-108">Prima di scrivere qualsiasi codice, è necessario disporre della stringa di connessione per l'account di archiviazione che verrà usato.</span><span class="sxs-lookup"><span data-stu-id="991c8-108">Before writing any code, you'll need the connection string for the storage account you will use.</span></span> 

<span data-ttu-id="991c8-109">La stringa di connessione include la chiave dell'account.</span><span class="sxs-lookup"><span data-stu-id="991c8-109">The connection string includes your account key.</span></span> <span data-ttu-id="991c8-110">La chiave dell'account viene considerata un segreto e deve essere archiviata in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="991c8-110">The account key is considered a secret and should be stored securely.</span></span> <span data-ttu-id="991c8-111">Archivieremo la stringa di connessione in un'impostazione dell'applicazione del Servizio app.</span><span class="sxs-lookup"><span data-stu-id="991c8-111">We will store the connection string in an App Service application setting.</span></span> <span data-ttu-id="991c8-112">Un'impostazione dell'applicazione è una posizione sicura per i segreti dell'applicazione, ma non supporta lo sviluppo locale e non è una soluzione affidabile end-to-end di per sé.</span><span class="sxs-lookup"><span data-stu-id="991c8-112">An application setting is a secure place for application secrets, but it does not support local development and is not a robust, end-to-end solution on its own.</span></span>

> [!WARNING]
> <span data-ttu-id="991c8-113">**Non inserire chiavi dell'account di archiviazione nel codice o nei file di configurazione non protetti.**</span><span class="sxs-lookup"><span data-stu-id="991c8-113">**Do not place storage account keys in code or in unprotected configuration files.**</span></span> <span data-ttu-id="991c8-114">Le chiavi dell'account di archiviazione abilitano l'accesso completo all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="991c8-114">Storage account keys enable full access to your storage account.</span></span> <span data-ttu-id="991c8-115">La verifica della perdita di una chiave può comportare danni irreversibili e fatture molto consistenti.</span><span class="sxs-lookup"><span data-stu-id="991c8-115">Leaking a key can result in unrecoverable damage and large bills.</span></span> <span data-ttu-id="991c8-116">Vedere la sezione relativa alle risorse aggiuntive al termine di questo modulo per altre informazioni sull'archiviazione e consigli su come eseguire il ripristino da una chiave persa.</span><span class="sxs-lookup"><span data-stu-id="991c8-116">See the Additional Resources section at the end of this module for storage guidance and advice about how to recover from a leaked key.</span></span>

## <a name="initialize-the-blob-storage-object-model"></a><span data-ttu-id="991c8-117">Inizializzare il modello oggetto di archiviazione BLOB</span><span class="sxs-lookup"><span data-stu-id="991c8-117">Initialize the Blob storage object model</span></span>

<span data-ttu-id="991c8-118">In Archiviazione di Azure e SDK per .NET Core, il modello standard per l'uso dell'archiviazione BLOB prevede i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="991c8-118">In the Azure Storage SDK for .NET Core, the standard pattern for using Blob storage consists of the following steps:</span></span>

1. <span data-ttu-id="991c8-119">Chiamare `CloudStorageAccount.Parse` (o `TryParse`) con la stringa di connessione per ottenere `CloudStorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="991c8-119">Call `CloudStorageAccount.Parse` (or `TryParse`) with your connection string to get a `CloudStorageAccount`.</span></span>
1. <span data-ttu-id="991c8-120">Chiamare `CreateCloudBlobClient` in `CloudStorageAccount` per ottenere `CloudBlobClient`.</span><span class="sxs-lookup"><span data-stu-id="991c8-120">Call `CreateCloudBlobClient` on the `CloudStorageAccount` to get a `CloudBlobClient`.</span></span>
1. <span data-ttu-id="991c8-121">Chiamare `GetContainerReference` in `CloudBlobClient` per ottenere `CloudBlobContainer`.</span><span class="sxs-lookup"><span data-stu-id="991c8-121">Call `GetContainerReference` on the `CloudBlobClient` to get a `CloudBlobContainer`.</span></span>
1. <span data-ttu-id="991c8-122">Usare metodi per ottenere un elenco di blob e/o ottenere riferimenti a singoli blob per caricare e scaricare i dati nei contenitori.</span><span class="sxs-lookup"><span data-stu-id="991c8-122">Use methods on the container to get a list of blobs and/or get references to individual blobs to upload and download data.</span></span>

<span data-ttu-id="991c8-123">Nel codice, i passaggi da 1&ndash;3 sono simili a:</span><span class="sxs-lookup"><span data-stu-id="991c8-123">In code, steps 1&ndash;3 look like this:</span></span>

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

<span data-ttu-id="991c8-124">Nessuno di tali codici di inizializzazione effettua le chiamate sulla rete.</span><span class="sxs-lookup"><span data-stu-id="991c8-124">None of this initialization code makes calls over the network.</span></span> <span data-ttu-id="991c8-125">Ciò significa che non vengono generate eccezioni che si verificano a partire da informazioni non corrette fino a un momento successivo, ad esempio, la chiamata a `GetContainerReference` avrà esito positivo indipendentemente dal fatto che il contenitore è effettivamente presente nell'account.</span><span class="sxs-lookup"><span data-stu-id="991c8-125">This means exceptions that occur from incorrect information won't be thrown until later; for example, the call to `GetContainerReference` will succeed whether or not the container actually exists in the account.</span></span>

## <a name="create-containers-at-startup"></a><span data-ttu-id="991c8-126">Creare contenitori all'avvio</span><span class="sxs-lookup"><span data-stu-id="991c8-126">Create containers at startup</span></span>

<span data-ttu-id="991c8-127">Una procedura consigliata per le applicazioni è la creazione di contenitori necessari nel codice, anche se è assodato che i contenitori verranno anticipati.</span><span class="sxs-lookup"><span data-stu-id="991c8-127">Common practice is for applications to create needed containers in code, even when we know what those containers will be up-front.</span></span> <span data-ttu-id="991c8-128">La chiamata di `CreateIfNotExistsAsync` in `CloudBlobContainer` è il modo migliore per eseguire questa operazione e dovrebbe essere usata per creare ogni contenitore che sappiamo essere necessario prima di usarlo.</span><span class="sxs-lookup"><span data-stu-id="991c8-128">Calling `CreateIfNotExistsAsync` on a `CloudBlobContainer` is the best way to do this, and we should use it to create each container we know we'll need before we use them.</span></span>

<span data-ttu-id="991c8-129">`CreateIfNotExistsAsync` *non*  effettua una chiamata di rete ad Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="991c8-129">`CreateIfNotExistsAsync` *does* make a network call to Azure Storage.</span></span> <span data-ttu-id="991c8-130">La procedura consigliata consiste nel chiamarlo una sola volta all'avvio e non ogni volta che si accede a un contenitore.</span><span class="sxs-lookup"><span data-stu-id="991c8-130">Best practice is to call it once at startup and not every time we access a container.</span></span>

## <a name="exercise"></a><span data-ttu-id="991c8-131">Esercizio</span><span class="sxs-lookup"><span data-stu-id="991c8-131">Exercise</span></span>

### <a name="clone-and-explore-the-unfinished-app"></a><span data-ttu-id="991c8-132">Clonare ed esplorare l'app non completata</span><span class="sxs-lookup"><span data-stu-id="991c8-132">Clone and explore the unfinished app</span></span>

<span data-ttu-id="991c8-133">Prima di tutto, è possibile clonare l'app iniziale da GitHub.</span><span class="sxs-lookup"><span data-stu-id="991c8-133">First, let's clone the starter app from GitHub.</span></span> <span data-ttu-id="991c8-134">Nel terminale Cloud Shell, eseguire il comando seguente per ottenere una copia del codice sorgente e aprirla nell'editor:</span><span class="sxs-lookup"><span data-stu-id="991c8-134">In the Cloud Shell terminal, run the following command to get a copy of the source code and open it in the editor:</span></span>

```console
git clone TODO
cd TODO
code .
```

<span data-ttu-id="991c8-135">Aprire il file `Controllers/FilesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="991c8-135">Open the file `Controllers/FilesController.cs`.</span></span>

<span data-ttu-id="991c8-136">Questo controller implementa un'API con tre azioni:</span><span class="sxs-lookup"><span data-stu-id="991c8-136">This controller implements an API with three actions:</span></span>

* <span data-ttu-id="991c8-137">**Indice** (GET /api/Files) restituisce un elenco di URL, uno per ogni file che è stato caricato.</span><span class="sxs-lookup"><span data-stu-id="991c8-137">**Index** (GET /api/Files) returns a list of URLs, one for each file that's been uploaded.</span></span> <span data-ttu-id="991c8-138">Il front-end di app chiama questo metodo per compilare un elenco di collegamenti ipertestuali nei file caricati.</span><span class="sxs-lookup"><span data-stu-id="991c8-138">The app front end calls this method to build a list of hyperlinks to the uploaded files.</span></span>
* <span data-ttu-id="991c8-139">**Caricamento** (POST /api/Files) riceve un file caricato e lo salva.</span><span class="sxs-lookup"><span data-stu-id="991c8-139">**Upload** (POST /api/Files) receives an uploaded file and saves it.</span></span>
* <span data-ttu-id="991c8-140">**Download** (GET/api/Files/{file-name}) scarica un singolo file in base al nome.</span><span class="sxs-lookup"><span data-stu-id="991c8-140">**Download** (GET /api/Files/{file-name}) downloads an individual file by its name.</span></span>

<span data-ttu-id="991c8-141">Ogni metodo usa un'istanza `IStorage` denominata `storage` per l'esecuzione delle operazioni.</span><span class="sxs-lookup"><span data-stu-id="991c8-141">Each method uses an `IStorage` instance called `storage` to do its work.</span></span> <span data-ttu-id="991c8-142">In `Models/BlobStorage.cs` è presente un'implementazione di incompleta di `IStorage`.</span><span class="sxs-lookup"><span data-stu-id="991c8-142">There is an incomplete implementation of `IStorage` in  `Models/BlobStorage.cs`.</span></span>

### <a name="add-the-nuget-package"></a><span data-ttu-id="991c8-143">Aggiungere il pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="991c8-143">Add the NuGet package</span></span>

<span data-ttu-id="991c8-144">In primo luogo, aggiungere un riferimento all'SDK di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="991c8-144">First, add a reference to the Azure Storage SDK.</span></span> <span data-ttu-id="991c8-145">Eseguire i comandi seguenti dal terminale:</span><span class="sxs-lookup"><span data-stu-id="991c8-145">In the terminal, run the following:</span></span>

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

<span data-ttu-id="991c8-146">Ciò ci consentirà di sapere che stiamo usando la versione più recente della libreria del client di archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="991c8-146">This will make sure we're using the newest version of the Blob storage client library.</span></span>

### <a name="configure"></a><span data-ttu-id="991c8-147">Configurare</span><span class="sxs-lookup"><span data-stu-id="991c8-147">Configure</span></span>

<span data-ttu-id="991c8-148">L'app iniziale include già le operazioni di base di configurazione necessarie.</span><span class="sxs-lookup"><span data-stu-id="991c8-148">Our starter app already includes the configuration plumbing we need.</span></span> <span data-ttu-id="991c8-149">Il parametro del costruttore `IOptions<AzureStorageConfig>` in `BlobStorage` ha due proprietà: la stringa di connessione dell'account di archiviazione e il nome del contenitore in cui l'app archivierà i blob.</span><span class="sxs-lookup"><span data-stu-id="991c8-149">The `IOptions<AzureStorageConfig>` constructor parameter in `BlobStorage` has two properties: the storage account connection string and the name of the container our app will store blobs in.</span></span> <span data-ttu-id="991c8-150">Nel metodo `ConfigureServices` di `Startup.cs` è presente un codice che carica i valori dalla configurazione all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="991c8-150">There is code in the `ConfigureServices` method of `Startup.cs` that loads the values from configuration when the app starts.</span></span>

<span data-ttu-id="991c8-151">In questo esercizio, eseguiremo l'app nel Servizio app di Azure, aggiungeremo quindi i valori di configurazione alle impostazioni dell'applicazione di Servizio app in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="991c8-151">In this exercise, we will run the app in Azure App Service, so we will add the configuration values to the App Service application settings later.</span></span> <span data-ttu-id="991c8-152">Per il momento, non dobbiamo eseguire operazioni correlate alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="991c8-152">For now, we don't need to do any work related to configuration.</span></span>

### <a name="initialize"></a><span data-ttu-id="991c8-153">Inizializzare</span><span class="sxs-lookup"><span data-stu-id="991c8-153">Initialize</span></span>

<span data-ttu-id="991c8-154">Aprire `BlobStorage.cs`.</span><span class="sxs-lookup"><span data-stu-id="991c8-154">Open `BlobStorage.cs`.</span></span>

<span data-ttu-id="991c8-155">Percorso del metodo `Initialize`.</span><span class="sxs-lookup"><span data-stu-id="991c8-155">Location the `Initialize` method.</span></span> <span data-ttu-id="991c8-156">Quando viene usato per la prima volta, l'app chiamerà questo metodo.</span><span class="sxs-lookup"><span data-stu-id="991c8-156">Our app will call this method when it's first used.</span></span> <span data-ttu-id="991c8-157">Per curiosità, è possibile esaminare `ConfigureServices` in `Startup.cs` per visualizzare informazioni su questa procedura.</span><span class="sxs-lookup"><span data-stu-id="991c8-157">If you're curious, you can look at `ConfigureServices` in `Startup.cs` to see how this is done.</span></span> 

<span data-ttu-id="991c8-158">`Initialize` è la posizione in cui si vuole creare il contenitore se non ancora esistente.</span><span class="sxs-lookup"><span data-stu-id="991c8-158">`Initialize` is where we want to create our container if it doesn't already exist.</span></span> <span data-ttu-id="991c8-159">Compilare `Initialize` con il codice seguente e salvare il lavoro:</span><span class="sxs-lookup"><span data-stu-id="991c8-159">Fill in `Initialize` with the following code and save your work:</span></span>

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```