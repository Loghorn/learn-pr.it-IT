<span data-ttu-id="2f2bb-101">A questo punto, l'applicazione è una raccolta funzionale che consente di caricare e visualizzare le immagini.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-101">At this point, the application is a functional gallery that allows you to upload and view images.</span></span> <span data-ttu-id="2f2bb-102">In questo modulo viene illustrato come usare l'API Visione artificiale di Servizi cognitivi Microsoft per generare le didascalie per le immagini caricate e salvare le didascalie con i metadati delle immagini in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-102">In this module, you learn how to use the Computer Vision API from Microsoft Cognitive Services to generate captions for uploaded images and save the captions with the image metadata in Azure Cosmos DB.</span></span>

## <a name="create-a-computer-vision-account"></a><span data-ttu-id="2f2bb-103">Creare un account di Visione artificiale</span><span class="sxs-lookup"><span data-stu-id="2f2bb-103">Create a Computer Vision account</span></span>

<span data-ttu-id="2f2bb-104">Servizi cognitivi Microsoft è una raccolta di servizi che consentono agli sviluppatori di rendere le applicazioni più intelligenti.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-104">Microsoft Cognitive Services is a collection of services that are available to developers to make their applications more intelligent.</span></span> <span data-ttu-id="2f2bb-105">L'API Visione artificiale è un servizio senza server che elabora le immagini usando algoritmi avanzati e restituisce informazioni su ogni immagine.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-105">The Computer Vision API is a serverless service that processes images using advanced algorithms and returns information about each image.</span></span> <span data-ttu-id="2f2bb-106">Include un livello gratuito che offre fino a 5.000 chiamate API al mese.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-106">It has a free tier that provides up to 5,000 API calls per month.</span></span>

1. <span data-ttu-id="2f2bb-107">Verificare di essere ancora connessi a Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-107">Ensure you're still signed into Cloud Shell.</span></span> <span data-ttu-id="2f2bb-108">In caso contrario, selezionare **Accedi a modalità messa a fuoco** per aprire una finestra di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-108">If you aren't, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="2f2bb-109">Creare un nuovo account di Servizi cognitivi di tipo **ComputerVision** con un nome univoco nel gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-109">Create a new Cognitive Services account of type **ComputerVision** with a unique name in your resource group.</span></span> <span data-ttu-id="2f2bb-110">Per il livello gratuito usare **F0** come SKU.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-110">For the free tier, use **F0** as the SKU.</span></span> <span data-ttu-id="2f2bb-111">Se si ha già un account esistente di Visione artificiale, potrebbe essere necessario creare un account Standard (S1); tuttavia, ciò può comportare alcuni costi.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-111">If you already have an existing Computer Vision account, you may need to create a Standard account (S1), which may incur some costs.</span></span>

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a><span data-ttu-id="2f2bb-112">Creare le impostazioni dell'app per le funzioni per l'URL e la chiave di Visione artificiale</span><span class="sxs-lookup"><span data-stu-id="2f2bb-112">Create function app settings for Computer Vision URL and key</span></span>

<span data-ttu-id="2f2bb-113">Per chiamare l'API Visione artificiale, sono necessari un URL e una chiave.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-113">To call the Computer Vision API, a URL and key are required.</span></span>

1. <span data-ttu-id="2f2bb-114">Ottenere l'URL e la chiave API Visione artificiale e salvarli nelle variabili bash.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-114">Get the Computer Vision API key and URL and save them in Bash variables.</span></span>

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. <span data-ttu-id="2f2bb-115">Creare le impostazioni dell'app denominate rispettivamente **COMP_VISION_KEY** e **COMP_VISION_URL** nell'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-115">Create app settings named **COMP_VISION_KEY** and **COMP_VISION_URL**, respectively, in the function app.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-the-computer-vision-api-from-the-resizeimage-function"></a><span data-ttu-id="2f2bb-116">Chiamare l'API Visione artificiale dalla funzione ResizeImage</span><span class="sxs-lookup"><span data-stu-id="2f2bb-116">Call the Computer Vision API from the ResizeImage function</span></span>

<span data-ttu-id="2f2bb-117">Nei passaggi seguenti si andrà a modificare la funzione **ResizeImage** per chiamare l'API Visione artificiale per descrivere ogni immagine caricata e salvare la descrizione in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-117">In the following steps, you modify the **ResizeImage** function to call the Computer Vision API to describe each uploaded image and save the description in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="2f2bb-118">Aprire l'app per le funzioni nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-118">Open your function app in the Azure portal.</span></span>

1. <span data-ttu-id="2f2bb-119">**C#**</span><span class="sxs-lookup"><span data-stu-id="2f2bb-119">**C#**</span></span>

    1. <span data-ttu-id="2f2bb-120">(C#) Usare il riquadro di spostamento a sinistra per trovare la funzione **ResizeImage** e aprire la finestra del codice.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-120">(C#) Using the left navigation, locate the **ResizeImage** function and open its code window.</span></span>

    1. <span data-ttu-id="2f2bb-121">(C#) Sostituire il codice con il contenuto del file [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx).</span><span class="sxs-lookup"><span data-stu-id="2f2bb-121">(C#) Replace the code with the contents of the [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx) file.</span></span> <span data-ttu-id="2f2bb-122">Questo codice usa `HttpClient` per chiamare l'API Visione artificiale e salvare il risultato in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-122">This code uses `HttpClient` to call the Computer Vision API and save its result in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="2f2bb-123">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="2f2bb-123">**JavaScript**</span></span>

    1. <span data-ttu-id="2f2bb-124">(JavaScript) Questa funzione richiede il pacchetto `axios` da npm per effettuare una chiamata HTTP all'API Visione artificiale.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-124">(JavaScript) This function requires the `axios` package from npm to make an HTTP call to the Computer Vision API.</span></span> <span data-ttu-id="2f2bb-125">Per installare il pacchetto npm, fare clic sul nome dell'app per le funzioni nel riquadro di spostamento di sinistra e fare clic su **Funzionalità della piattaforma**.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-125">To install the npm package, click on the function app name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="2f2bb-126">(JavaScript) Fare clic su **Console** per visualizzare una finestra della console.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-126">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="2f2bb-127">(JavaScript) Eseguire il comando `npm install axios` nella console.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-127">(JavaScript) Run the command `npm install axios` in the console.</span></span> <span data-ttu-id="2f2bb-128">Il completamento dell'operazione potrebbe richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-128">It may take a few minutes to complete the operation.</span></span>

    1. <span data-ttu-id="2f2bb-129">(JavaScript) Fare clic sul nome della funzione (**ResizeImage**) nel riquadro di spostamento a sinistra per visualizzare la funzione.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-129">(JavaScript) Click on the function name (**ResizeImage**) in the left navigation to reveal the function.</span></span> <span data-ttu-id="2f2bb-130">Sostituire tutto il file **index.js** con il contenuto del file [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).</span><span class="sxs-lookup"><span data-stu-id="2f2bb-130">Replace all of the **index.js** file with the contents of the [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js) file.</span></span>

1. <span data-ttu-id="2f2bb-131">Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-131">Click **Logs** below the code window to expand the Logs panel.</span></span>

1. <span data-ttu-id="2f2bb-132">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-132">Click **Save**.</span></span> <span data-ttu-id="2f2bb-133">Verificare nel pannello dei log che la funzione sia stata salvata correttamente e che non vi siano errori.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-133">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="2f2bb-134">Test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="2f2bb-134">Test the application</span></span>

1. <span data-ttu-id="2f2bb-135">Aprire l'applicazione in un browser.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-135">Open the application in a browser.</span></span> <span data-ttu-id="2f2bb-136">Selezionare un file di immagine e caricarlo.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-136">Select an image file, and upload it.</span></span>

1. <span data-ttu-id="2f2bb-137">Dopo alcuni secondi, nella pagina verrà visualizzata l'anteprima della nuova immagine.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-137">After a few seconds, the thumbnail of the new image should appear on the page.</span></span> <span data-ttu-id="2f2bb-138">Passare il mouse sull'immagine per visualizzare la descrizione generata da Visione artificiale.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-138">Point to the image to see the description that's generated by Computer Vision.</span></span>


## <a name="summary"></a><span data-ttu-id="2f2bb-139">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2f2bb-139">Summary</span></span>

<span data-ttu-id="2f2bb-140">In questa unità è stato descritto come generare automaticamente le didascalie per ogni immagine caricata usando l'API Visione artificiale di Servizi cognitivi Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-140">In this unit, you learned how to automatically generate captions for each uploaded image using Microsoft Cognitive Services Computer Vision API.</span></span> <span data-ttu-id="2f2bb-141">Si vedrà ora come aggiungere l'autenticazione all'applicazione usando l'autenticazione del servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="2f2bb-141">Next, you learn how to add authentication to the application using Azure App Service authentication.</span></span>