<span data-ttu-id="3a570-101">Di seguito è riportato il flusso di lavoro tipico per le app che usano l'archiviazione BLOB di Azure:</span><span class="sxs-lookup"><span data-stu-id="3a570-101">The following is the typical workflow for apps that use Azure Blob storage:</span></span>

1. <span data-ttu-id="3a570-102">**Recuperare la configurazione**: all'avvio, caricare la configurazione dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="3a570-102">**Retrieve configuration**: At startup, load the storage account configuration.</span></span> <span data-ttu-id="3a570-103">Solitamente si tratta della stringa di connessione dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="3a570-103">This is typically a storage account connection string.</span></span>
1. <span data-ttu-id="3a570-104">**Inizializzare i client**: usare la stringa di connessione per inizializzare la libreria client di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="3a570-104">**Initialize client**: Use the connection string to initialize the Azure Storage client library.</span></span> <span data-ttu-id="3a570-105">Verranno creati gli oggetti che l'app userà per collaborare con l'API di archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="3a570-105">This creates the objects the app will use to work with the Blob storage API.</span></span>
1. <span data-ttu-id="3a570-106">**Usare**: consentire alle chiamate API con la libreria client di operare su contenitori e blob.</span><span class="sxs-lookup"><span data-stu-id="3a570-106">**Use**: Make API calls with the client library to operate on containers and blobs.</span></span>

## <a name="configure-your-connection-string"></a><span data-ttu-id="3a570-107">Configurare la stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="3a570-107">Configure your connection string</span></span>

<span data-ttu-id="3a570-108">Prima di scrivere qualsiasi codice, è necessario avere la stringa di connessione per l'account di archiviazione che verrà usato.</span><span class="sxs-lookup"><span data-stu-id="3a570-108">Before writing any code, you'll need the connection string for the storage account you will use.</span></span>

<span data-ttu-id="3a570-109">Le stringhe di connessione dell'account di archiviazione includono la chiave dell'account.</span><span class="sxs-lookup"><span data-stu-id="3a570-109">Storage account connection strings include the account key.</span></span> <span data-ttu-id="3a570-110">La chiave dell'account viene considerata un segreto e deve essere archiviata in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="3a570-110">The account key is considered a secret and should be stored securely.</span></span> <span data-ttu-id="3a570-111">In questo caso la stringa di connessione verrà archiviata in un'impostazione dell'applicazione del servizio app.</span><span class="sxs-lookup"><span data-stu-id="3a570-111">Here, we will store the connection string in an App Service application setting.</span></span> <span data-ttu-id="3a570-112">Un'impostazione dell'applicazione del servizio app è una posizione sicura per i segreti dell'applicazione, ma non supporta lo sviluppo locale e non è una soluzione affidabile end-to-end di per sé.</span><span class="sxs-lookup"><span data-stu-id="3a570-112">App Service application settings are a secure place for application secrets, but this design does not support local development and is not a robust, end-to-end solution on its own.</span></span>

> [!WARNING]
> <span data-ttu-id="3a570-113">**Non inserire chiavi dell'account di archiviazione nel codice o nei file di configurazione non protetti.**</span><span class="sxs-lookup"><span data-stu-id="3a570-113">**Do not place storage account keys in code or in unprotected configuration files.**</span></span> <span data-ttu-id="3a570-114">Le chiavi dell'account di archiviazione abilitano l'accesso completo all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="3a570-114">Storage account keys enable full access to your storage account.</span></span> <span data-ttu-id="3a570-115">La perdita di una chiave può comportare danni irreversibili e fatture molto consistenti.</span><span class="sxs-lookup"><span data-stu-id="3a570-115">Leaking a key can result in unrecoverable damage and large bills.</span></span> <span data-ttu-id="3a570-116">Vedere la sezione Altre informazioni al termine di questo modulo per una guida sull'archiviazione e consigli su come eseguire il ripristino da una chiave persa.</span><span class="sxs-lookup"><span data-stu-id="3a570-116">See the Further Reading section at the end of this module for storage guidance and advice about how to recover from a leaked key.</span></span>

## <a name="initialize-the-blob-storage-object-model"></a><span data-ttu-id="3a570-117">Inizializzare il modello oggetto di archiviazione BLOB</span><span class="sxs-lookup"><span data-stu-id="3a570-117">Initialize the Blob storage object model</span></span>

<span data-ttu-id="3a570-118">In Archiviazione di Azure e SDK per .NET Core, il modello standard per l'uso dell'archiviazione BLOB prevede i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3a570-118">In the Azure Storage SDK for .NET Core, the standard pattern for using Blob storage consists of the following steps:</span></span>

1. <span data-ttu-id="3a570-119">Chiamare `CloudStorageAccount.Parse` (o `TryParse`) con la stringa di connessione per ottenere `CloudStorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="3a570-119">Call `CloudStorageAccount.Parse` (or `TryParse`) with your connection string to get a `CloudStorageAccount`.</span></span>
1. <span data-ttu-id="3a570-120">Chiamare `CreateCloudBlobClient` in `CloudStorageAccount` per ottenere `CloudBlobClient`.</span><span class="sxs-lookup"><span data-stu-id="3a570-120">Call `CreateCloudBlobClient` on the `CloudStorageAccount` to get a `CloudBlobClient`.</span></span>
1. <span data-ttu-id="3a570-121">Chiamare `GetContainerReference` in `CloudBlobClient` per ottenere `CloudBlobContainer`.</span><span class="sxs-lookup"><span data-stu-id="3a570-121">Call `GetContainerReference` on the `CloudBlobClient` to get a `CloudBlobContainer`.</span></span>
1. <span data-ttu-id="3a570-122">Usare metodi per ottenere un elenco di blob e/o ottenere riferimenti a singoli blob per caricare e scaricare i dati nei contenitori.</span><span class="sxs-lookup"><span data-stu-id="3a570-122">Use methods on the container to get a list of blobs and/or get references to individual blobs to upload and download data.</span></span>

<span data-ttu-id="3a570-123">Nel codice i passaggi 1&ndash;3 sono simili a:</span><span class="sxs-lookup"><span data-stu-id="3a570-123">In code, steps 1&ndash;3 look like this:</span></span>

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

<span data-ttu-id="3a570-124">Nessuno di tali codici di inizializzazione esegue le chiamate sulla rete.</span><span class="sxs-lookup"><span data-stu-id="3a570-124">None of this initialization code makes calls over the network.</span></span> <span data-ttu-id="3a570-125">Ciò significa che alcune eccezioni che si verificano a causa di informazioni non corrette verranno generate solo in un momento successivo.</span><span class="sxs-lookup"><span data-stu-id="3a570-125">This means that some exceptions that occur because of incorrect information won't be thrown until later.</span></span> <span data-ttu-id="3a570-126">Ad esempio, la chiamata a `CloudStorageAccount.Parse` genera un'eccezione immediatamente se la stringa di connessione non è formattata correttamente, ma non verrà generata alcuna eccezione se l'account di archiviazione a cui fa riferimento una stringa di connessione non esiste.</span><span class="sxs-lookup"><span data-stu-id="3a570-126">For example, the call to `CloudStorageAccount.Parse` will throw an exception immediately if the connection string is formatted incorrectly, but no exception will be thrown if the storage account that a connection string points to doesn't exist.</span></span>

## <a name="create-containers-at-startup"></a><span data-ttu-id="3a570-127">Creare contenitori all'avvio</span><span class="sxs-lookup"><span data-stu-id="3a570-127">Create containers at startup</span></span>

<span data-ttu-id="3a570-128">La chiamata di `CreateIfNotExistsAsync` su un `CloudBlobContainer` è il modo migliore per creare un contenitore quando l'applicazione si avvia o quando tenta di usarlo per la prima volta.</span><span class="sxs-lookup"><span data-stu-id="3a570-128">Calling `CreateIfNotExistsAsync` on a `CloudBlobContainer` is the best way to create a container when your application starts or when it first tries to use it.</span></span>

<span data-ttu-id="3a570-129">`CreateIfNotExistsAsync` non genera un'eccezione se il contenitore esiste già, ma esegue una chiamata di rete ad Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="3a570-129">`CreateIfNotExistsAsync` won't throw an exception if the container already exists, but it does make a network call to Azure Storage.</span></span> <span data-ttu-id="3a570-130">La chiamata viene eseguita una volta durante l'inizializzazione, non ogni volta che si prova a usare un contenitore.</span><span class="sxs-lookup"><span data-stu-id="3a570-130">Call it once during initialization, not every time you try to use a container.</span></span>

## <a name="exercise"></a><span data-ttu-id="3a570-131">Esercizio</span><span class="sxs-lookup"><span data-stu-id="3a570-131">Exercise</span></span>

### <a name="clone-and-explore-the-unfinished-app"></a><span data-ttu-id="3a570-132">Clonare ed esplorare l'app non completata</span><span class="sxs-lookup"><span data-stu-id="3a570-132">Clone and explore the unfinished app</span></span>

<span data-ttu-id="3a570-133">Prima di tutto, è possibile clonare l'app iniziale da GitHub.</span><span class="sxs-lookup"><span data-stu-id="3a570-133">First, let's clone the starter app from GitHub.</span></span> <span data-ttu-id="3a570-134">Nel terminale Cloud Shell eseguire il comando seguente per ottenere una copia del codice sorgente e aprirla nell'editor:</span><span class="sxs-lookup"><span data-stu-id="3a570-134">In the Cloud Shell terminal, run the following command to get a copy of the source code and open it in the editor:</span></span>

<span data-ttu-id="3a570-135">**Aggiornamento TODO a URL archivio finale**</span><span class="sxs-lookup"><span data-stu-id="3a570-135">**TODO update to final repo URL**</span></span>

```console
git clone https://github.com/nickwalkmsft/FileUploader.git
cd FileUploader
code .
```

<span data-ttu-id="3a570-136">Aprire il file `Controllers/FilesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="3a570-136">Open the file `Controllers/FilesController.cs`.</span></span> <span data-ttu-id="3a570-137">Non vi è alcuna operazione da eseguire in questo passaggio, ma è necessario verificare il funzionamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="3a570-137">There's no work to do here, but we're going to have a quick look at what the app does.</span></span>

<span data-ttu-id="3a570-138">Questo controller implementa un'API con tre azioni:</span><span class="sxs-lookup"><span data-stu-id="3a570-138">This controller implements an API with three actions:</span></span>

* <span data-ttu-id="3a570-139">**Indice** (GET /api/Files) restituisce un elenco di URL, uno per ogni file che è stato caricato.</span><span class="sxs-lookup"><span data-stu-id="3a570-139">**Index** (GET /api/Files) returns a list of URLs, one for each file that's been uploaded.</span></span> <span data-ttu-id="3a570-140">Il front-end di app chiama questo metodo per compilare un elenco di collegamenti ipertestuali nei file caricati.</span><span class="sxs-lookup"><span data-stu-id="3a570-140">The app front end calls this method to build a list of hyperlinks to the uploaded files.</span></span>
* <span data-ttu-id="3a570-141">**Caricamento** (POST /api/Files) riceve un file caricato e lo salva.</span><span class="sxs-lookup"><span data-stu-id="3a570-141">**Upload** (POST /api/Files) receives an uploaded file and saves it.</span></span>
* <span data-ttu-id="3a570-142">**Download** (GET/api/Files/{nomefile}) scarica un singolo file in base al nome.</span><span class="sxs-lookup"><span data-stu-id="3a570-142">**Download** (GET /api/Files/{filename}) downloads an individual file by its name.</span></span>

<span data-ttu-id="3a570-143">Ogni metodo usa un'istanza `IStorage` denominata `storage` per l'esecuzione delle operazioni.</span><span class="sxs-lookup"><span data-stu-id="3a570-143">Each method uses an `IStorage` instance called `storage` to do its work.</span></span> <span data-ttu-id="3a570-144">In `Models/BlobStorage.cs` è presente un'implementazione incompleta di `IStorage` che è necessario sistemare.</span><span class="sxs-lookup"><span data-stu-id="3a570-144">There is an incomplete implementation of `IStorage` in `Models/BlobStorage.cs` that we're going to fill in.</span></span>

### <a name="add-the-nuget-package"></a><span data-ttu-id="3a570-145">Aggiungere il pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="3a570-145">Add the NuGet package</span></span>

<span data-ttu-id="3a570-146">In primo luogo, aggiungere un riferimento all'SDK di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="3a570-146">First, add a reference to the Azure Storage SDK.</span></span> <span data-ttu-id="3a570-147">Eseguire i comandi seguenti dal terminale:</span><span class="sxs-lookup"><span data-stu-id="3a570-147">In the terminal, run the following:</span></span>

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

<span data-ttu-id="3a570-148">Ciò ci consentirà di sapere che stiamo usando la versione più recente della libreria del client di archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="3a570-148">This will make sure we're using the newest version of the Blob storage client library.</span></span>

### <a name="configure"></a><span data-ttu-id="3a570-149">Configurare</span><span class="sxs-lookup"><span data-stu-id="3a570-149">Configure</span></span>

<span data-ttu-id="3a570-150">I valori di configurazione necessari per eseguire l'app sono la stringa di connessione dell'account di archiviazione e il nome del contenitore dell'app che verrà usata per archiviare i file.</span><span class="sxs-lookup"><span data-stu-id="3a570-150">The configuration values we need to run the app are the storage account connection string and the name of the container the app will use to store files.</span></span> <span data-ttu-id="3a570-151">In questo esercizio l'app verrà eseguita solo nel servizio app di Azure quindi verrà seguita la procedura consigliata del servizio app e i valori verranno archiviati nelle impostazioni applicazione del servizio app.</span><span class="sxs-lookup"><span data-stu-id="3a570-151">In this exercise, we're only going to run the app in Azure App Service, so we'll follow App Service best practice and store the values in App Service application settings.</span></span> <span data-ttu-id="3a570-152">Questa operazione verrà eseguita quando verrà creata l'istanza del servizio app, non in questo momento.</span><span class="sxs-lookup"><span data-stu-id="3a570-152">We'll do that when we create the App Service instance, so there's nothing we need to do at the moment.</span></span>

<span data-ttu-id="3a570-153">Quando sarà il momento di *usare* la configurazione, l'app iniziale includerà già le operazioni di base necessarie.</span><span class="sxs-lookup"><span data-stu-id="3a570-153">When it comes to *using* the configuration, our starter app already includes the plumbing we need.</span></span> <span data-ttu-id="3a570-154">Il parametro del costruttore `IOptions<AzureStorageConfig>` in `BlobStorage` ha due proprietà: la stringa di connessione dell'account di archiviazione e il nome del contenitore in cui l'app archivierà i BLOB.</span><span class="sxs-lookup"><span data-stu-id="3a570-154">The `IOptions<AzureStorageConfig>` constructor parameter in `BlobStorage` has two properties: the storage account connection string and the name of the container our app will store blobs in.</span></span> <span data-ttu-id="3a570-155">Nel metodo `ConfigureServices` di `Startup.cs` è presente un codice che carica i valori dalla configurazione all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="3a570-155">There is code in the `ConfigureServices` method of `Startup.cs` that loads the values from configuration when the app starts.</span></span>

### <a name="initialize"></a><span data-ttu-id="3a570-156">Inizializzare</span><span class="sxs-lookup"><span data-stu-id="3a570-156">Initialize</span></span>

<span data-ttu-id="3a570-157">Aprire `Models/BlobStorage.cs`.</span><span class="sxs-lookup"><span data-stu-id="3a570-157">Open `Models/BlobStorage.cs`.</span></span> <span data-ttu-id="3a570-158">Aggiungere le istruzioni `using` seguenti all'inizio del file per eseguire la preparazione al codice che si vuole aggiungere durante l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="3a570-158">Add the following `using` statements to the top of the file to prepare it for the code you're going to add during the exercise.</span></span>

```csharp
using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="3a570-159">Individuare il metodo `Initialize`.</span><span class="sxs-lookup"><span data-stu-id="3a570-159">Locate the `Initialize` method.</span></span> <span data-ttu-id="3a570-160">Quando `BlobStorage` viene usato per la prima volta, l'app chiamerà questo metodo.</span><span class="sxs-lookup"><span data-stu-id="3a570-160">Our app will call this method when `BlobStorage` is used for the first time.</span></span> <span data-ttu-id="3a570-161">Per curiosità, è possibile esaminare `ConfigureServices` in `Startup.cs` per visualizzare informazioni su questa procedura.</span><span class="sxs-lookup"><span data-stu-id="3a570-161">If you're curious, you can look at `ConfigureServices` in `Startup.cs` to see how this is done.</span></span>

<span data-ttu-id="3a570-162">`Initialize` è la posizione in cui si vuole creare il contenitore se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="3a570-162">`Initialize` is where we want to create our container if it doesn't already exist.</span></span> <span data-ttu-id="3a570-163">Sostituire l'implementazione corrente di `Initialize` con il codice seguente e salvare il lavoro:</span><span class="sxs-lookup"><span data-stu-id="3a570-163">Replace the current implementation of `Initialize` with the following code and save your work:</span></span>

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```