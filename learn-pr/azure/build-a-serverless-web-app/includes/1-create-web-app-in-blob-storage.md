<span data-ttu-id="1d55d-101">In questo modulo verrà distribuita un'applicazione Web semplice che presenta un'interfaccia utente basata su HTML.</span><span class="sxs-lookup"><span data-stu-id="1d55d-101">In this module, you will deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="1d55d-102">Un back-end serverless consente all'applicazione di caricare le immagini e generare automaticamente le didascalie relative.</span><span class="sxs-lookup"><span data-stu-id="1d55d-102">A serverless back end enables the application to upload images and automatically generate descriptive captions.</span></span>

![Esecuzione dell'app Web](../media/0-app-screenshot-finished.png)

<span data-ttu-id="1d55d-104">L'illustrazione seguente mostra i servizi di Azure usati dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1d55d-104">The following illustration shows the Azure services that are used by the application.</span></span>

![<span data-ttu-id="1d55d-105">Un'illustrazione che mostra i diversi modi in cui vengono utilizzati dall'applicazione servizi quali l'archiviazione BLOB di Azure, le funzioni di Azure, Cosmos DB, le app per la logica di Azure e Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1d55d-105">An illustration showing how different Azure services such as, Azure Blob storage, Azure functions, Cosmos DB, Azure logic apps, and Azure active directory are used by the application.</span></span> ](../media/0-architecture.jpg)

1. <span data-ttu-id="1d55d-106">Archiviazione BLOB di Azure gestisce il contenuto Web statico (HTML, CSS o JS) e archivia le immagini.</span><span class="sxs-lookup"><span data-stu-id="1d55d-106">Azure Blob storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="1d55d-107">Funzioni di Azure gestisce i caricamenti delle immagini, il ridimensionamento e l'archiviazione dei metadati.</span><span class="sxs-lookup"><span data-stu-id="1d55d-107">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="1d55d-108">Azure Cosmos DB archivia i metadati delle immagini.</span><span class="sxs-lookup"><span data-stu-id="1d55d-108">Azure Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="1d55d-109">App per la logica di Azure recupera le didascalie delle immagini dall'API Visione artificiale di Servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="1d55d-109">Azure Logic Apps retrieves image captions from the Cognitive Services Computer Vision API.</span></span>
5. <span data-ttu-id="1d55d-110">Azure Active Directory gestisce l'autenticazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="1d55d-110">Azure Active Directory manages user authentication.</span></span>

<span data-ttu-id="1d55d-111">Archiviazione BLOB di Azure è un servizio economico e altamente scalabile che può essere usato per ospitare file statici.</span><span class="sxs-lookup"><span data-stu-id="1d55d-111">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="1d55d-112">In questo modulo l'archiviazione BLOB verrà usata per gestire contenuti statici, ad esempio HTML, JavaScript o CSS, per l'app Web che verrà compilata.</span><span class="sxs-lookup"><span data-stu-id="1d55d-112">In this module, you will use Blob storage to serve static content (for example, HTML, JavaScript, or CSS) for a web app you build.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="1d55d-113">Creare un account di Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="1d55d-113">Create an Azure Storage account</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="1d55d-114">Un account di Archiviazione di Azure è una risorsa di Azure che consente di archiviare tabelle, code, file, BLOB (oggetti) e dischi delle macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="1d55d-114">An Azure Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="1d55d-115">Il contenuto statico, ad esempio i file HTML, CSS e JavaScript, per questo modulo è ospitato nell'archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="1d55d-115">The static content (HTML, CSS, and JavaScript files) for this module is hosted in Blob storage.</span></span> <span data-ttu-id="1d55d-116">L'archiviazione BLOB richiede un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="1d55d-116">Blob storage requires a Storage account.</span></span> <span data-ttu-id="1d55d-117">Creare un account di archiviazione di uso generico v2 (GPv2) nel gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="1d55d-117">Create a general-purpose v2 (GPv2) Storage account in the resource group.</span></span> <span data-ttu-id="1d55d-118">Sostituire `<storage account name>` con un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="1d55d-118">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create \
        -n <storage account name> \
        -g <rgn>[Sandbox resource group name]</rgn> \
        --kind StorageV2 \
        --https-only true \
        --sku Standard_LRS
    ```
    
1. <span data-ttu-id="1d55d-119">Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="1d55d-119">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="1d55d-120">Usare la barra di ricerca nella parte superiore del portale per trovare l'account di archiviazione appena creato.</span><span class="sxs-lookup"><span data-stu-id="1d55d-120">Use the Search bar at the top of the portal to find the storage account that you just created.</span></span> <span data-ttu-id="1d55d-121">Aprire l'account.</span><span class="sxs-lookup"><span data-stu-id="1d55d-121">Open the account.</span></span>

1. <span data-ttu-id="1d55d-122">Nel riquadro di spostamento a sinistra selezionare **Sito Web statico (anteprima)** per configurare un contenitore per l'hosting di siti Web statici.</span><span class="sxs-lookup"><span data-stu-id="1d55d-122">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="1d55d-123">Selezionare **Abilitato** per abilitare il sito Web statico.</span><span class="sxs-lookup"><span data-stu-id="1d55d-123">Select **Enabled** to enable a static website.</span></span>
    - <span data-ttu-id="1d55d-124">Immettere **index.html** come nome del documento di indice.</span><span class="sxs-lookup"><span data-stu-id="1d55d-124">Enter **index.html** as the index document name.</span></span> <span data-ttu-id="1d55d-125">Nella finestra è già presente *index.html* in grigio, ma si tratta solo di testo di esempio.</span><span class="sxs-lookup"><span data-stu-id="1d55d-125">The box already has *index.html* in a gray font, but this is only example text.</span></span> <span data-ttu-id="1d55d-126">È comunque necessario immettere **index.html** nella finestra.</span><span class="sxs-lookup"><span data-stu-id="1d55d-126">You still have to enter **index.html** in the box.</span></span>
    - <span data-ttu-id="1d55d-127">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="1d55d-127">Click **Save**.</span></span>
    
    ![Immettere le impostazioni del sito Web statico](../media/1-storage-static-website.png)

1. <span data-ttu-id="1d55d-129">Salvare l'**endpoint primario** in una posizione da cui poterlo copiare facilmente durante l'utilizzo del modulo.</span><span class="sxs-lookup"><span data-stu-id="1d55d-129">Save the **Primary Endpoint** in a place where you can conveniently copy it from while working through the module.</span></span> <span data-ttu-id="1d55d-130">L'endpoint è l'URL dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="1d55d-130">This endpoint is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="1d55d-131">Caricare l'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="1d55d-131">Upload the web application</span></span>

1. <span data-ttu-id="1d55d-132">I file di origine per l'applicazione che si compila in questo modulo si trovano in un [repository GitHub](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span><span class="sxs-lookup"><span data-stu-id="1d55d-132">The source files for the application that you build in this module are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="1d55d-133">Passare alla home directory in Cloud Shell e clonare il repository.</span><span class="sxs-lookup"><span data-stu-id="1d55d-133">Go to your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="1d55d-134">Il repository viene clonato in `/home/<username>/functions-first-serverless-web-application`.</span><span class="sxs-lookup"><span data-stu-id="1d55d-134">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`</span></span>

1. <span data-ttu-id="1d55d-135">L'applicazione Web sul lato client si trova nella cartella **www** e viene compilata con il framework JavaScript Vue.js.</span><span class="sxs-lookup"><span data-stu-id="1d55d-135">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="1d55d-136">Aprire la cartella **www** ed eseguire i comandi **npm** per installare le dipendenze dell'applicazione e compilarla.</span><span class="sxs-lookup"><span data-stu-id="1d55d-136">Open the **www** folder and run **npm** commands to install the application dependencies and build the application.</span></span> <span data-ttu-id="1d55d-137">Il completamento dell'ultimo di questi comandi potrebbe richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="1d55d-137">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="1d55d-138">L'applicazione viene generata nella cartella **dist**.</span><span class="sxs-lookup"><span data-stu-id="1d55d-138">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="1d55d-139">Passare dalla directory corrente alla directory **dist** e caricare l'applicazione nel contenitore BLOB **$web**.</span><span class="sxs-lookup"><span data-stu-id="1d55d-139">Change the current directory to the **dist** folder and upload the application to the **$web** blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="1d55d-140">Per visualizzare l'applicazione, aprire l'URL dell'endpoint primario dei siti Web statici in un Web browser.</span><span class="sxs-lookup"><span data-stu-id="1d55d-140">To view the application, open the static website’s primary endpoint URL in a web browser.</span></span>

    ![Home page della prima app Web serverless](../media/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="1d55d-142">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1d55d-142">Summary</span></span>

<span data-ttu-id="1d55d-143">In questa unità è stato creato un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="1d55d-143">In this unit, you created a storage account.</span></span> <span data-ttu-id="1d55d-144">Un contenitore BLOB denominato **$web** nell'account di archiviazione archivia il contenuto statico per l'applicazione Web e rende il contenuto disponibile pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="1d55d-144">A blob container named **$web** in the storage account stores the static content for your web application and makes the content publicly available.</span></span> <span data-ttu-id="1d55d-145">Si vedrà ora come usare una funzione serverless per caricare immagini nell'archiviazione BLOB da questa applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="1d55d-145">Next, you will learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>