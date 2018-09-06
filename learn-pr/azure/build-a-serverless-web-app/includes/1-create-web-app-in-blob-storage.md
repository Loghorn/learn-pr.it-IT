<span data-ttu-id="2e72c-101">In questo modulo verrà distribuita un'applicazione Web semplice che presenta un'interfaccia utente basata su HTML.</span><span class="sxs-lookup"><span data-stu-id="2e72c-101">In this module, you will deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="2e72c-102">Un back-end serverless consente all'applicazione di caricare le immagini e ottenere automaticamente le didascalie relative.</span><span class="sxs-lookup"><span data-stu-id="2e72c-102">A serverless back end enables the application to upload images and automatically get captions that describe them.</span></span>

![Esecuzione dell'app Web](../media/0-app-screenshot-finished.png)

<span data-ttu-id="2e72c-104">Il diagramma seguente illustra i servizi di Azure usati dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2e72c-104">The following diagram shows the Azure services that are used by the application.</span></span>

1. <span data-ttu-id="2e72c-105">Archiviazione BLOB di Azure gestisce il contenuto Web statico (HTML, CSS o JS) e archivia le immagini.</span><span class="sxs-lookup"><span data-stu-id="2e72c-105">Azure Blob storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="2e72c-106">Funzioni di Azure gestisce i caricamenti delle immagini, il ridimensionamento e l'archiviazione dei metadati.</span><span class="sxs-lookup"><span data-stu-id="2e72c-106">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="2e72c-107">Azure Cosmos DB archivia i metadati delle immagini.</span><span class="sxs-lookup"><span data-stu-id="2e72c-107">Azure Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="2e72c-108">App per la logica di Azure ottiene le didascalie delle immagini dall'API Visione artificiale di Servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="2e72c-108">Azure Logic Apps gets image captions from the Cognitive Services Computer Vision API.</span></span>
5. <span data-ttu-id="2e72c-109">Azure Active Directory gestisce l'autenticazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="2e72c-109">Azure Active Directory manages user authentication.</span></span>

![Diagramma dell'architettura della soluzione](../media/0-architecture.jpg)

<span data-ttu-id="2e72c-111">In questa unità si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="2e72c-111">In this unit, you will learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="2e72c-112">Configurare l'archiviazione BLOB di Azure per ospitare un sito Web statico e le immagini caricate.</span><span class="sxs-lookup"><span data-stu-id="2e72c-112">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="2e72c-113">Caricare le immagini nell'archiviazione BLOB di Azure con Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="2e72c-113">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="2e72c-114">Ridimensionare le immagini con Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="2e72c-114">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="2e72c-115">Archiviare i metadati delle immagini in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2e72c-115">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="2e72c-116">Usare l'API Visione artificiale di Servizi cognitivi per generare automaticamente le didascalie delle immagini.</span><span class="sxs-lookup"><span data-stu-id="2e72c-116">Use the Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="2e72c-117">Usare Azure Active Directory per proteggere l'app Web tramite l'autenticazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="2e72c-117">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="2e72c-118">Archiviazione BLOB di Azure è un servizio economico e altamente scalabile che può essere usato per ospitare file statici.</span><span class="sxs-lookup"><span data-stu-id="2e72c-118">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="2e72c-119">Per questa esercitazione è possibile usarlo per gestire il contenuto statico, come ad esempio HTML, JavaScript, CSS, per l'app Web che si compila.</span><span class="sxs-lookup"><span data-stu-id="2e72c-119">For this tutorial, you use it to serve static content (for example, HTML, JavaScript, CSS) for the web app that you build.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="2e72c-120">Creare un account di Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="2e72c-120">Create an Azure Storage account</span></span>

<span data-ttu-id="2e72c-121">Un account di Archiviazione di Azure è una risorsa di Azure che consente di archiviare tabelle, code, file, BLOB (oggetti) e dischi delle macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="2e72c-121">An Azure Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="2e72c-122">Selezionare il pulsante **Enter focus mode** (Accedi a modalità messa a fuoco) per avviare Azure Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="2e72c-122">Select the **Enter focus mode** button to launch Azure Cloud Shell (Bash).</span></span> <span data-ttu-id="2e72c-123">Questo pulsante si trova nella parte superiore destra o nella parte inferiore della pagina, a seconda dell'ampiezza della finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="2e72c-123">This button is at the top right or the bottom of the page, depending on how wide your browser window is.</span></span> <span data-ttu-id="2e72c-124">La modalità messa a fuoco ancora una finestra di Cloud Shell sul lato destro della finestra del browser, in modo da potere eseguire con facilità i comandi illustrati nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="2e72c-124">Focus mode docks a Cloud Shell window on the right side of your browser window, so you can easily execute commands that are shown in the tutorial.</span></span>

1. <span data-ttu-id="2e72c-125">Un gruppo di risorse di Azure è un contenitore con risorse di Azure correlate per facilitare la gestione.</span><span class="sxs-lookup"><span data-stu-id="2e72c-125">In Azure, a resource group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="2e72c-126">Creare un nuovo gruppo di risorse denominato **first-serverless-app**.</span><span class="sxs-lookup"><span data-stu-id="2e72c-126">Create a new resource group named **first-serverless-app**.</span></span>

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. <span data-ttu-id="2e72c-127">Il contenuto statico, come ad esempio file HTML, CSS e JavaScript, per questa esercitazione è ospitato nell'archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="2e72c-127">The static content (HTML, CSS, and JavaScript files) for this tutorial is hosted in Blob storage.</span></span> <span data-ttu-id="2e72c-128">L'archiviazione BLOB richiede un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="2e72c-128">Blob storage requires a Storage account.</span></span> <span data-ttu-id="2e72c-129">Creare un account di archiviazione (utilizzo generico V2) nel gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="2e72c-129">Create a Storage account (general-purpose V2) in the resource group.</span></span> <span data-ttu-id="2e72c-130">Sostituire `<storage account name>` con un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="2e72c-130">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```
    
1. <span data-ttu-id="2e72c-131">Usare la barra di ricerca nella parte superiore del [portale di Azure](https://portal.azure.com) per trovare l'account di archiviazione appena creato.</span><span class="sxs-lookup"><span data-stu-id="2e72c-131">Use the Search bar at the top of the [Azure portal](https://portal.azure.com) to find the storage account that you just created.</span></span> <span data-ttu-id="2e72c-132">Aprire l'account.</span><span class="sxs-lookup"><span data-stu-id="2e72c-132">Open the account.</span></span>

1. <span data-ttu-id="2e72c-133">Nel riquadro di spostamento a sinistra selezionare **Sito Web statico (anteprima)** per configurare un contenitore per l'hosting di siti Web statici.</span><span class="sxs-lookup"><span data-stu-id="2e72c-133">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="2e72c-134">Selezionare **Abilitato** per abilitare il sito Web statico.</span><span class="sxs-lookup"><span data-stu-id="2e72c-134">Select **Enabled** to enable a static website.</span></span>
    - <span data-ttu-id="2e72c-135">Immettere **index.html** come nome del documento di indice.</span><span class="sxs-lookup"><span data-stu-id="2e72c-135">Enter **index.html** as the index document name.</span></span> <span data-ttu-id="2e72c-136">Nella finestra è già presente *index.html* in grigio, ma si tratta solo di testo di esempio.</span><span class="sxs-lookup"><span data-stu-id="2e72c-136">The box already has *index.html* in a gray font, but this is only example text.</span></span> <span data-ttu-id="2e72c-137">È comunque necessario immettere **index.html** nella finestra.</span><span class="sxs-lookup"><span data-stu-id="2e72c-137">You still have to enter **index.html** in the box.</span></span>
    - <span data-ttu-id="2e72c-138">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="2e72c-138">Click **Save**.</span></span>
    
    ![Immettere le impostazioni del sito Web statico](../media/1-storage-static-website.png)

1. <span data-ttu-id="2e72c-140">Salvare l'**Endpoint primario** in una posizione da cui poterlo copiare facilmente durante l'esecuzione dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="2e72c-140">Save the **Primary Endpoint** in a place where you can conveniently copy it from while working through the tutorial.</span></span> <span data-ttu-id="2e72c-141">L'endpoint è l'URL dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="2e72c-141">This endpoint is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="2e72c-142">Caricare l'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="2e72c-142">Upload the web application</span></span>

1. <span data-ttu-id="2e72c-143">I file di origine per l'applicazione che si compila in questa esercitazione si trovano in un [repository GitHub](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span><span class="sxs-lookup"><span data-stu-id="2e72c-143">The source files for the application that you build in this tutorial are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="2e72c-144">Assicurarsi di trovarsi all'interno della home directory in Cloud Shell e clonare il repository.</span><span class="sxs-lookup"><span data-stu-id="2e72c-144">Make sure that you're in your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="2e72c-145">Il repository viene clonato in `/home/<username>/functions-first-serverless-web-application`.</span><span class="sxs-lookup"><span data-stu-id="2e72c-145">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`.</span></span>

1. <span data-ttu-id="2e72c-146">L'applicazione Web sul lato client si trova nella cartella **www** e viene compilata con il framework JavaScript Vue.js.</span><span class="sxs-lookup"><span data-stu-id="2e72c-146">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="2e72c-147">Passare a tale cartella ed eseguire i comandi **npm** per installare le dipendenze dell'applicazione e compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2e72c-147">Change into the folder and run **npm** commands to install the application dependencies and build the application.</span></span> <span data-ttu-id="2e72c-148">Il completamento dell'ultimo di questi comandi potrebbe richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="2e72c-148">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="2e72c-149">L'applicazione viene generata nella cartella **dist**.</span><span class="sxs-lookup"><span data-stu-id="2e72c-149">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="2e72c-150">Passare dalla directory corrente alla directory **dist** e caricare l'applicazione nel contenitore BLOB **$web**.</span><span class="sxs-lookup"><span data-stu-id="2e72c-150">Change the current directory to the **dist** folder and upload the application to the **$web** blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="2e72c-151">Per visualizzare l'applicazione, aprire l'URL dell'endpoint primario dei siti Web statici di archiviazione in un Web browser.</span><span class="sxs-lookup"><span data-stu-id="2e72c-151">To view the application, open the Storage static websites primary endpoint URL in a web browser.</span></span>

    ![Home page della prima app Web serverless](../media/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="2e72c-153">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2e72c-153">Summary</span></span>

<span data-ttu-id="2e72c-154">In questa unità è stato creato un gruppo di risorse denominato **first-serverless-app** contenente un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="2e72c-154">In this unit, you created a resource group named **first-serverless-app** that contains a Storage account.</span></span> <span data-ttu-id="2e72c-155">Un contenitore BLOB denominato **$web** nell'account di archiviazione archivia il contenuto statico per l'applicazione Web e rende il contenuto disponibile pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="2e72c-155">A blob container named **$web** in the Storage account stores the static content for your web application and makes the content publicly available.</span></span> <span data-ttu-id="2e72c-156">Si vedrà ora come usare una funzione serverless per caricare immagini nell'archiviazione BLOB da questa applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="2e72c-156">Next, you learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>