
## <a name="what-is-microsoft-cognitive-services"></a><span data-ttu-id="f8b29-101">Informazioni sui Servizi cognitivi Microsoft</span><span class="sxs-lookup"><span data-stu-id="f8b29-101">What is Microsoft Cognitive Services?</span></span>

<span data-ttu-id="f8b29-102">Servizi cognitivi Microsoft è un set di algoritmi di apprendimento automatico disponibile come servizio che chiunque può usare.</span><span class="sxs-lookup"><span data-stu-id="f8b29-102">Microsoft Cognitive Services is a set of machine learning algorithms available as a service for anyone to use.</span></span> <span data-ttu-id="f8b29-103">Invece di creare da zero questa funzionalità di intelligence per le app, è possibile usare questi servizi per visione artificiale, sintesi vocale, linguaggio, conoscenza e ricerca.</span><span class="sxs-lookup"><span data-stu-id="f8b29-103">Instead of building this intelligence for our apps from scratch, we can use these services for vision, speech, language, knowledge, and search.</span></span> <span data-ttu-id="f8b29-104">È possibile provare gratuitamente ogni servizio.</span><span class="sxs-lookup"><span data-stu-id="f8b29-104">You can try each service for free.</span></span> <span data-ttu-id="f8b29-105">Quando si decide di integrare un servizio nelle proprie app, è necessario registrarsi per una sottoscrizione a pagamento.</span><span class="sxs-lookup"><span data-stu-id="f8b29-105">When you decide to integrate a service into your app, you sign up for a paid subscription.</span></span> <span data-ttu-id="f8b29-106">In questo modello si approfondirà l'API Visione artificiale.</span><span class="sxs-lookup"><span data-stu-id="f8b29-106">We'll focus our attention on the Computer Vision API in this model.</span></span>

> [!TIP]
> <span data-ttu-id="f8b29-107">Per visualizzare un elenco di tutti i Servizi cognitivi, vedere la [Directory di Servizi cognitivi](https://azure.microsoft.com/services/cognitive-services/directory/).</span><span class="sxs-lookup"><span data-stu-id="f8b29-107">To see a list of all Cognitive Services, check out the [Cognitive Services Directory](https://azure.microsoft.com/services/cognitive-services/directory/).</span></span> 

## <a name="what-is-the-computer-vision-api"></a><span data-ttu-id="f8b29-108">Che cos'è l'API Visione artificiale?</span><span class="sxs-lookup"><span data-stu-id="f8b29-108">What is the Computer Vision API?</span></span>

<span data-ttu-id="f8b29-109">L'API Visione artificiale fornisce algoritmi per elaborare le immagini e restituire informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="f8b29-109">The Computer Vision API provides algorithms to process images and return insights.</span></span> <span data-ttu-id="f8b29-110">È ad esempio possibile usarla per verificare se un'immagine include contenuto per soli adulti o per trovare tutti i visi in un'immagine.</span><span class="sxs-lookup"><span data-stu-id="f8b29-110">For example, you can find out if an image has mature content, or can use it to find all the faces in an image.</span></span> <span data-ttu-id="f8b29-111">Offre anche altre funzionalità, ad esempio l'identificazione del colore dominante e di quelli in primo piano, la classificazione del contenuto delle immagini e la descrizione di un'immagine con frasi complete in lingua inglese.</span><span class="sxs-lookup"><span data-stu-id="f8b29-111">It also has other features like estimating dominant and accent colors, categorizing the content of images, and describing an image with complete English sentences.</span></span> <span data-ttu-id="f8b29-112">Può anche generare anteprime intelligenti delle immagini per visualizzare le immagini di grandi dimensioni in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="f8b29-112">Additionally, it can also intelligently generate images thumbnails for displaying large images effectively.</span></span>

> [!TIP]
> <span data-ttu-id="f8b29-113">L'API Visione artificiale è disponibile in molte aree in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="f8b29-113">The Computer Vision API is available in many regions across the globe.</span></span> <span data-ttu-id="f8b29-114">Per trovare l'area più vicina, vedere [Prodotti disponibili in base all'area](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).</span><span class="sxs-lookup"><span data-stu-id="f8b29-114">To find the region nearest you, see the [Products available by region](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).</span></span>

<span data-ttu-id="f8b29-115">È possibile usare l'API Visione artificiale per:</span><span class="sxs-lookup"><span data-stu-id="f8b29-115">You can use the Computer Vision API to:</span></span>

- <span data-ttu-id="f8b29-116">Analizzare le immagini per ottenere informazioni dettagliate</span><span class="sxs-lookup"><span data-stu-id="f8b29-116">Analyze images for insight</span></span>
- <span data-ttu-id="f8b29-117">Estrarre il testo stampato dalle immagini usando il riconoscimento ottico dei caratteri (OCR).</span><span class="sxs-lookup"><span data-stu-id="f8b29-117">Extract printed text from images using optical character recognition (OCR).</span></span>
- <span data-ttu-id="f8b29-118">Riconoscere il testo stampato e scritto a mano nelle immagini</span><span class="sxs-lookup"><span data-stu-id="f8b29-118">Recognize printed and handwritten text from images</span></span>
- <span data-ttu-id="f8b29-119">Riconoscere celebrità e luoghi di interesse</span><span class="sxs-lookup"><span data-stu-id="f8b29-119">Recognize celebrities and landmarks</span></span>
- <span data-ttu-id="f8b29-120">Analizzare video</span><span class="sxs-lookup"><span data-stu-id="f8b29-120">Analyze video</span></span> 
- <span data-ttu-id="f8b29-121">Generare un'anteprima di un'immagine</span><span class="sxs-lookup"><span data-stu-id="f8b29-121">Generate a thumbnail of an image</span></span> 

## <a name="how-to-call-the-computer-vision-api"></a><span data-ttu-id="f8b29-122">Come chiamare l'API Visione artificiale</span><span class="sxs-lookup"><span data-stu-id="f8b29-122">How to call the Computer Vision API</span></span>

<span data-ttu-id="f8b29-123">Per chiamare Visione artificiale nell'applicazione, usare direttamente le librerie client dell'API REST.</span><span class="sxs-lookup"><span data-stu-id="f8b29-123">You call Computer Vision in your application using client libraries or the REST API directly.</span></span> <span data-ttu-id="f8b29-124">In questo modulo si chiamerà l'API REST.</span><span class="sxs-lookup"><span data-stu-id="f8b29-124">We'll call the REST API in this module.</span></span> <span data-ttu-id="f8b29-125">Per effettuare una chiamata:</span><span class="sxs-lookup"><span data-stu-id="f8b29-125">To make a call:</span></span>

1. <span data-ttu-id="f8b29-126">Ottenere una chiave di accesso all'API</span><span class="sxs-lookup"><span data-stu-id="f8b29-126">Get an API access key</span></span>

    <span data-ttu-id="f8b29-127">Le chiavi di accesso vengono assegnate quando ci si iscrive a un account del servizio Visione artificiale.</span><span class="sxs-lookup"><span data-stu-id="f8b29-127">You are assigned access keys when you sign up for a Computer Vision service account.</span></span> <span data-ttu-id="f8b29-128">Una chiave deve essere passata nell'intestazione di **ogni** richiesta.</span><span class="sxs-lookup"><span data-stu-id="f8b29-128">A key must be passed in the header of **every** request.</span></span> 

1. <span data-ttu-id="f8b29-129">Effettuare una chiamata POST all'API</span><span class="sxs-lookup"><span data-stu-id="f8b29-129">Make a POST call to the API</span></span>

    <span data-ttu-id="f8b29-130">Formattare l'URL come segue: **area**.api.cognitive.microsoft.com/vision/v2.0/**risorsa**/**[parametri]**</span><span class="sxs-lookup"><span data-stu-id="f8b29-130">Format the URL as follows: **region**.api.cognitive.microsoft.com/vision/v2.0/**resource**/**[parameters]**</span></span> 

    - <span data-ttu-id="f8b29-131">**area**: area in cui è stato creato l'account, ad esempio *westus*.</span><span class="sxs-lookup"><span data-stu-id="f8b29-131">**region** - the region where you created the account, for example, *westus*.</span></span>
    - <span data-ttu-id="f8b29-132">**risorsa**: risorsa di Visione artificiale da chiamare, ad esempio `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.</span><span class="sxs-lookup"><span data-stu-id="f8b29-132">**resource** - the Computer Vision resource you are calling such as `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.</span></span>

    <span data-ttu-id="f8b29-133">È possibile specificare l'immagine da elaborare come file binario dell'immagine RAW o come URL dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="f8b29-133">You can supply the image to be processed either as a raw image binary or an image URL.</span></span>

    <span data-ttu-id="f8b29-134">L'intestazione della richiesta deve contenere la chiave di sottoscrizione, che fornisce l'accesso a questa API.</span><span class="sxs-lookup"><span data-stu-id="f8b29-134">The request header must contain the subscription key, which provides access to this API.</span></span>

1. <span data-ttu-id="f8b29-135">Analizzare la risposta</span><span class="sxs-lookup"><span data-stu-id="f8b29-135">Parse the response</span></span>

    <span data-ttu-id="f8b29-136">La risposta contiene le informazioni dettagliate che l'API Visione artificiale ha raccolto sull'immagine come payload JSON.</span><span class="sxs-lookup"><span data-stu-id="f8b29-136">The response holds the insight the Computer Vision API had about your image as a JSON payload.</span></span>

<span data-ttu-id="f8b29-137">Si supponga di aver creato la sottoscrizione di Visione artificiale nell'area `westus` e di aver ottenuto una chiave di sottoscrizione per accedere all'API.</span><span class="sxs-lookup"><span data-stu-id="f8b29-137">Suppose you created my Computer Vision subscription in the `westus` region and got a subscription key to access the API.</span></span> <span data-ttu-id="f8b29-138">Assegnare alla chiave il valore fittizio `xxxx-xxxxx-xxxx-xxxx`.</span><span class="sxs-lookup"><span data-stu-id="f8b29-138">Let's give the key a fictitious value of `xxxx-xxxxx-xxxx-xxxx`.</span></span> <span data-ttu-id="f8b29-139">Si vuole ora generare un'anteprima per un'immagine che si trova in `http://example.com/images/test.jpg`.</span><span class="sxs-lookup"><span data-stu-id="f8b29-139">You now want to generate a thumbnail for an image located at `http://example.com/images/test.jpg`.</span></span> <span data-ttu-id="f8b29-140">Usando `generateThumbnail`, la richiesta sarà simile alla seguente.</span><span class="sxs-lookup"><span data-stu-id="f8b29-140">Using `generateThumbnail`, the request would look like the following.</span></span>

```
curl POST "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {xxxx-xxxxx-xxxx-xxxx}"
-d "{'url':'http://example.com/images/test.jpg'}"
```

<span data-ttu-id="f8b29-141">In questo modulo si eseguiranno tutti gli esercizi nell'interfaccia della riga di comando di Azure usando Cloud Shell integrato.</span><span class="sxs-lookup"><span data-stu-id="f8b29-141">In this module, we'll run all exercises in the Azure CLI using the integrated Cloud Shell.</span></span> <span data-ttu-id="f8b29-142">Ecco alcune informazioni aggiuntive su questa configurazione.</span><span class="sxs-lookup"><span data-stu-id="f8b29-142">Let's find out a little more about this setup.</span></span>

## <a name="what-is-the-azure-cli"></a><span data-ttu-id="f8b29-143">Che cos'è l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="f8b29-143">What is the Azure CLI</span></span>

<span data-ttu-id="f8b29-144">L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b29-144">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources.</span></span> <span data-ttu-id="f8b29-145">È disponibile per macOS, Linux e Windows oppure nel browser, tramite [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="f8b29-145">It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f8b29-146">Sono attualmente disponibili due versioni dell'interfaccia della riga di comando di Azure: 1.0 e 2.0.</span><span class="sxs-lookup"><span data-stu-id="f8b29-146">There are two versions of the Azure CLI tool available today: Azure CLI 1.0 and Azure CLI 2.0.</span></span> <span data-ttu-id="f8b29-147">In questa unità si userà l'interfaccia della riga di comando di Azure 2.0, ovvero la versione più recente e consigliata, a meno che non si debbano eseguire script legacy.</span><span class="sxs-lookup"><span data-stu-id="f8b29-147">We'll be using Azure CLI 2.0, which is the latest version and is recommended unless you're running legacy scripts.</span></span> <span data-ttu-id="f8b29-148">La versione 1.0 dell'interfaccia della riga di comando di Azure viene avviata con il comando `azure`, mentre la versione 2.0 viene avviata con il comando `az`.</span><span class="sxs-lookup"><span data-stu-id="f8b29-148">Azure CLI 1.0 is started with the `azure` command, and Azure CLI 2.0 is started with the `az` command.</span></span>

## <a name="az-cognitiveservices-commands"></a><span data-ttu-id="f8b29-149">Comandi az cognitiveservices</span><span class="sxs-lookup"><span data-stu-id="f8b29-149">az cognitiveservices commands</span></span>

<span data-ttu-id="f8b29-150">L'interfaccia della riga di comando di Azure include il comando `cognitiveservices` per gestire gli account di Servizi cognitivi in Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b29-150">The Azure CLI includes the `cognitiveservices` command to manage Cognitive Services accounts in Azure.</span></span> <span data-ttu-id="f8b29-151">È possibile fornire vari sottocomandi per eseguire attività specifiche.</span><span class="sxs-lookup"><span data-stu-id="f8b29-151">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="f8b29-152">Quelli più comuni includono:</span><span class="sxs-lookup"><span data-stu-id="f8b29-152">The most common include:</span></span>

| <span data-ttu-id="f8b29-153">Sottocomando</span><span class="sxs-lookup"><span data-stu-id="f8b29-153">Subcommand</span></span> | <span data-ttu-id="f8b29-154">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f8b29-154">Description</span></span> |
|-------------|-------------|
| `list` | <span data-ttu-id="f8b29-155">Elenca gli account di Servizi cognitivi di Azure disponibili.</span><span class="sxs-lookup"><span data-stu-id="f8b29-155">List available Azure Cognitive Services accounts.</span></span> |
| `account show` | <span data-ttu-id="f8b29-156">Ottiene i dettagli di un account di Servizi cognitivi di Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b29-156">Get the details of an Azure Cognitive Services account.</span></span> |
| `account create` | <span data-ttu-id="f8b29-157">Crea un account di Servizi cognitivi di Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b29-157">Create an Azure Cognitive Services account.</span></span> |
| `account delete` | <span data-ttu-id="f8b29-158">Elimina un account di Servizi cognitivi di Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b29-158">Delete an Azure Cognitive Services account.</span></span> |
| `keys list` | <span data-ttu-id="f8b29-159">Elenca le chiavi di un account di Servizi cognitivi di Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b29-159">List the keys of an Azure Cognitive Services account.</span></span> |

<span data-ttu-id="f8b29-160">Verrà ora creata un'istanza di Servizi cognitivi con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b29-160">Let's create a Cognitive Services with the Azure CLI.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a><span data-ttu-id="f8b29-161">Creare un account di Servizi cognitivi</span><span class="sxs-lookup"><span data-stu-id="f8b29-161">Create a Cognitive Services account</span></span>

<span data-ttu-id="f8b29-162">È necessaria una chiave di accesso all'API per effettuare le chiamate all'API Visione artificiale.</span><span class="sxs-lookup"><span data-stu-id="f8b29-162">We need an API access key to make calls to the Computer Vision API.</span></span> <span data-ttu-id="f8b29-163">Per ottenere le chiavi di accesso, è necessario un account di Servizi cognitivi per l'API Visione artificiale.</span><span class="sxs-lookup"><span data-stu-id="f8b29-163">To get access keys, we need a Cognitive Services account for the Computer Vision API.</span></span> <span data-ttu-id="f8b29-164">Si userà `az cognitiveservices create` per creare l'account nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="f8b29-164">We'll use `az cognitiveservices create` to create the account in our subscription.</span></span>

 <span data-ttu-id="f8b29-165">Il comando `az cognitiveservices create` viene usato per creare un account di Servizi cognitivi in un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="f8b29-165">The command `az cognitiveservices create` is used to create a Cognitive Services account in a resource group.</span></span>  <span data-ttu-id="f8b29-166">Quando si chiama questo comando, devono essere specificati i cinque parametri seguenti.</span><span class="sxs-lookup"><span data-stu-id="f8b29-166">The following five parameters must be supplied when calling this command.</span></span>

> [!Tip]
> <span data-ttu-id="f8b29-167">La maggior parte dei flag per i parametri dell'interfaccia della riga di comando di Azure può essere abbreviata con un carattere singolo.</span><span class="sxs-lookup"><span data-stu-id="f8b29-167">Most flags for Azure CLI parameters can be abbreviated to a single character.</span></span> <span data-ttu-id="f8b29-168">È ad esempio possibile usare `-l` invece di `--location`.</span><span class="sxs-lookup"><span data-stu-id="f8b29-168">For example, we could say `-l` instead of `--location`.</span></span> <span data-ttu-id="f8b29-169">La forma estesa viene usata per maggiore chiarezza.</span><span class="sxs-lookup"><span data-stu-id="f8b29-169">The long-form is used for clarity.</span></span>

| <span data-ttu-id="f8b29-170">Parametro</span><span class="sxs-lookup"><span data-stu-id="f8b29-170">Parameter</span></span> | <span data-ttu-id="f8b29-171">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f8b29-171">Description</span></span> |
|-----------|-------------|
| `resource-group` | <span data-ttu-id="f8b29-172">Gruppo di risorse che sarà il proprietario dell'account di Servizi Cognitivi.</span><span class="sxs-lookup"><span data-stu-id="f8b29-172">The resource group that will own the cognitive services account.</span></span> <span data-ttu-id="f8b29-173">In questa sessione sandbox interattiva si userà <rgn>[Nome gruppo di risorse sandbox]</rgn></span><span class="sxs-lookup"><span data-stu-id="f8b29-173">In this interactive sandbox session, you'll use <rgn>[Sandbox resource group name]</rgn></span></span> |
| `kind` | <span data-ttu-id="f8b29-174">Nome dell'API dell'account di Servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="f8b29-174">The API name of cognitive services account.</span></span> |
| `name` | <span data-ttu-id="f8b29-175">Nome dell'account di Servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="f8b29-175">Cognitive service account name.</span></span> |
| `sku` | <span data-ttu-id="f8b29-176">SKU dell'account di Servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="f8b29-176">The Sku of cognitive services account.</span></span>|
| `location` | <span data-ttu-id="f8b29-177">Località, ovvero area, da cui effettuare le chiamate a questa API.</span><span class="sxs-lookup"><span data-stu-id="f8b29-177">The location, or region, from which you want to make calls to this API.</span></span> <span data-ttu-id="f8b29-178">Selezionare una delle località nell'elenco seguente.</span><span class="sxs-lookup"><span data-stu-id="f8b29-178">Select one of the locations from the list below.</span></span> |

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

<span data-ttu-id="f8b29-179">Eseguire il comando seguente in Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="f8b29-179">Execute the following command in Azure Cloud Shell.</span></span> <span data-ttu-id="f8b29-180">Assicurarsi di sostituire `[location]` con una località nelle vicinanze.</span><span class="sxs-lookup"><span data-stu-id="f8b29-180">Make sure to replace `[location]` with a location near you.</span></span>

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group <rgn>[Sandbox resource group name]</rgn> \
--location [location]
```

> [!NOTE]
> <span data-ttu-id="f8b29-181">Ricordare la località selezionata.</span><span class="sxs-lookup"><span data-stu-id="f8b29-181">Remember the location you have selected.</span></span> <span data-ttu-id="f8b29-182">Tutte le chiamate all'API verranno effettuate da tale area.</span><span class="sxs-lookup"><span data-stu-id="f8b29-182">You'll make all calls to the API from that region.</span></span>

<span data-ttu-id="f8b29-183">È stato creato un account di Servizi cognitivi per l'API **Visione artificiale**.</span><span class="sxs-lookup"><span data-stu-id="f8b29-183">We've created a cognitive services account for the **ComputerVision** API.</span></span> <span data-ttu-id="f8b29-184">È stato selezionato lo SKU *S1* e l'account è stato denominato **ComputerVisionService**.</span><span class="sxs-lookup"><span data-stu-id="f8b29-184">We selected the *S1* sku and named our account **ComputerVisionService**.</span></span> <span data-ttu-id="f8b29-185">L'account è di proprietà del gruppo di risorse **<rgn>[Nome gruppo di risorse sandbox]</rgn>** e l'API verrà chiamata dalla località impostata nel parametro `--location`.</span><span class="sxs-lookup"><span data-stu-id="f8b29-185">Our account is owned by the resource group **<rgn>[Sandbox resource group name]</rgn>** and we'll call the API from the location we set in the `--location` parameter.</span></span> 

<span data-ttu-id="f8b29-186">Dopo che il comando ha completato la creazione dell'account di Servizi Cognitivi, si otterrà una risposta JSON, che include la proprietà **provisioningState** impostata su **Succeeded**.</span><span class="sxs-lookup"><span data-stu-id="f8b29-186">Once the command finishes creating the cognitive services account, you'll get a JSON response, which includes **provisioningState** property set to **Succeeded**.</span></span>

## <a name="get-an-access-key"></a><span data-ttu-id="f8b29-187">Ottenere una chiave di accesso</span><span class="sxs-lookup"><span data-stu-id="f8b29-187">Get an access key</span></span>

<span data-ttu-id="f8b29-188">Dopo aver creato l'account, è possibile recuperare le chiavi della sottoscrizione, o chiavi di accesso, per questo account.</span><span class="sxs-lookup"><span data-stu-id="f8b29-188">Once we have our account successfully created we can retrieve the subscription keys, or access keys, for this account.</span></span>

1. <span data-ttu-id="f8b29-189">Eseguire il comando seguente in Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="f8b29-189">Execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
    ```
    
    <span data-ttu-id="f8b29-190">Il comando precedente restituisce le chiavi associate all'account Servizi Cognitivi denominato **ComputerVisionService**, di proprietà del gruppo di risorse specificato.</span><span class="sxs-lookup"><span data-stu-id="f8b29-190">The above command returns the keys associated with the cognitive services account called **ComputerVisionService**, which is owned by the given resource group.</span></span> <span data-ttu-id="f8b29-191">Restituisce due chiavi, una delle quali è una chiave di riserva.</span><span class="sxs-lookup"><span data-stu-id="f8b29-191">It returns two keys - one is a spare key.</span></span> <span data-ttu-id="f8b29-192">Poiché è difficile ricordare le chiavi, la prima chiave verrà archiviata in una variabile da usare per tutte le chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="f8b29-192">The keys are difficult to remember, so we'll store the first key in a variable that we'll use for all calls to the API.</span></span>

2.  <span data-ttu-id="f8b29-193">Eseguire il comando seguente in Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="f8b29-193">Execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    key=$(az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --query key1 -o tsv)
    ```
    
    <span data-ttu-id="f8b29-194">L'interfaccia della riga di comando di Azure 2.0 usa l'argomento `--query` per eseguire una query JMESPath sui risultati dei comandi.</span><span class="sxs-lookup"><span data-stu-id="f8b29-194">The Azure CLI 2.0 uses the `--query` argument to execute a JMESPath query on the results of commands.</span></span> <span data-ttu-id="f8b29-195">JMESPath è un linguaggio di query per JSON che offre la possibilità di selezionare e presentare i dati dell'output dell'interfaccia della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="f8b29-195">JMESPath is a query language for JSON, giving you the ability to select and present data from CLI output.</span></span> <span data-ttu-id="f8b29-196">Le query vengono eseguite sull'output JSON prima di qualsiasi formattazione per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8b29-196">These queries are executed on the JSON output before any display formatting.</span></span>
    <span data-ttu-id="f8b29-197">L'argomento `--query` è supportato da tutti i comandi dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b29-197">The `--query` argument is supported by all commands in the Azure CLI.</span></span> 
    
    <span data-ttu-id="f8b29-198">In questo esempio viene eseguita una query sull'elenco di chiavi per cercare una voce denominata "key1" e il risultato viene visualizzato in formato **tsv**.</span><span class="sxs-lookup"><span data-stu-id="f8b29-198">In our example, we query the list of keys for an entry named "key1" and output the result to **tsv** format.</span></span> <span data-ttu-id="f8b29-199">Questo formato rimuove le virgolette che racchiudono il valore della stringa.</span><span class="sxs-lookup"><span data-stu-id="f8b29-199">This format removes quotations around the string value.</span></span> <span data-ttu-id="f8b29-200">Il risultato viene assegnato a una variabile **key**.</span><span class="sxs-lookup"><span data-stu-id="f8b29-200">We assign the result to a variable **key**.</span></span>
    
    > [!IMPORTANT]
    > <span data-ttu-id="f8b29-201">Poiché questa chiave verrà usata in tutto il modulo, è consigliabile salvarla in una variabile.</span><span class="sxs-lookup"><span data-stu-id="f8b29-201">We're going to use this key throughout the module, so saving it in a variable is a good idea.</span></span> <span data-ttu-id="f8b29-202">Se si perde il valore o se la variabile viene annullata, eseguire nuovamente il comando per impostarla.</span><span class="sxs-lookup"><span data-stu-id="f8b29-202">If you lose the value or the variable becomes unset, run the command again to set it.</span></span>  

3. <span data-ttu-id="f8b29-203">Per visualizzare il valore della chiave, eseguire il comando seguente in Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="f8b29-203">To see the value of our key, execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    echo $key
    ```

<span data-ttu-id="f8b29-204">Ora che sono stati creati un account e una chiave, è possibile effettuare le chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="f8b29-204">Now that we have an account and a key, it's time to make some calls to the API.</span></span>