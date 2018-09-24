<span data-ttu-id="b2d44-101">In qualità di sviluppatore responsabile presso Contoso Beverage Distribution, l'utente è incaricato della compilazione e gestione di un'app line-of-business che consente ai distributori di scansionare e caricare le immagini degli scaffali del magazzino nei quali si stanno rinnovando le scorte.</span><span class="sxs-lookup"><span data-stu-id="b2d44-101">As a lead developer at Contoso Beverage Distribution, you're responsible for building and maintaining a line-of-business app that lets your frontline distributors scan and upload images of store shelves where they are restocking.</span></span> 

<span data-ttu-id="b2d44-102">È necessario verificare che le immagini inviate dagli utenti rispettino le regole aziendali stabilite per i contenuti.</span><span class="sxs-lookup"><span data-stu-id="b2d44-102">You want to validate that any images posted by users respect the content rules set by your company.</span></span> <span data-ttu-id="b2d44-103">L'azienda non vuole che nei siti aziendali vengano inviati contenuti inappropriati.</span><span class="sxs-lookup"><span data-stu-id="b2d44-103">The company doesn't want inappropriate content posted to company sites.</span></span> <span data-ttu-id="b2d44-104">Dati i miglioramenti in materia di intelligenza artificiale, si decide di usare l'API Visione artificiale nell'app.</span><span class="sxs-lookup"><span data-stu-id="b2d44-104">Given the advancements in Artificial Intelligence, you decide to use the Computer Vision API in your app.</span></span> 

<span data-ttu-id="b2d44-105">Per iniziare, creare un account di Visione artificiale nella sottoscrizione di Azure e avviare il test della funzionalità di **analisi**.</span><span class="sxs-lookup"><span data-stu-id="b2d44-105">To get started, you create a Computer Vision account in your Azure subscription and start testing the **analyze** feature.</span></span>

## <a name="calling-the-computer-vision-api-to-analyze-images"></a><span data-ttu-id="b2d44-106">Chiamata dell'API Visione artificiale per analizzare le immagini</span><span class="sxs-lookup"><span data-stu-id="b2d44-106">Calling the Computer Vision API to analyze images</span></span>

<span data-ttu-id="b2d44-107">L'operazione `analyze` estrae una vasta gamma di caratteristiche visive in base al contenuto delle immagini.</span><span class="sxs-lookup"><span data-stu-id="b2d44-107">The `analyze` operation extracts a rich set of visual features based on the image content.</span></span> <span data-ttu-id="b2d44-108">L'URL della richiesta si presenta nel formato seguente:</span><span class="sxs-lookup"><span data-stu-id="b2d44-108">The request URL has the following format:</span></span>

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=<...>&details=<...>&language=<...>`

<span data-ttu-id="b2d44-109">I parametri inviati al servizio sono `visualFeatures`, `details` e `languages`.</span><span class="sxs-lookup"><span data-stu-id="b2d44-109">The parameters that are sent to the service are `visualFeatures`, `details`, and `languages`.</span></span> <span data-ttu-id="b2d44-110">Impostare il parametro `details` su `Landmarks` o `Celebrities` per identificare luoghi di interesse o celebrità.</span><span class="sxs-lookup"><span data-stu-id="b2d44-110">Set the `details` parameter to `Landmarks` or `Celebrities` to help you identify landmarks or celebrities.</span></span> <span data-ttu-id="b2d44-111">`visualFeatures` identifica la tipologia di informazioni che deve essere restituita dal servizio.</span><span class="sxs-lookup"><span data-stu-id="b2d44-111">`visualFeatures` identify what kind of information you want the service to return you.</span></span> <span data-ttu-id="b2d44-112">L'opzione `Categories` classifica il contenuto delle immagini, ad esempio alberi, edifici e così via.</span><span class="sxs-lookup"><span data-stu-id="b2d44-112">The `Categories` option will categorize the content of the images like trees, buildings, and more.</span></span> <span data-ttu-id="b2d44-113">`Faces` identifica i visi delle persone e offre indicazioni su sesso ed età.</span><span class="sxs-lookup"><span data-stu-id="b2d44-113">`Faces` will identify people's faces and give you their gender and age.</span></span>

<span data-ttu-id="b2d44-114">Tutte le operazioni nell'API Visione artificiale presentano le restrizioni seguenti relative alle immagini di cui si richiede l'elaborazione:</span><span class="sxs-lookup"><span data-stu-id="b2d44-114">All operations on the Computer Vision API have the following restrictions when it comes to the images you ask it to process:</span></span>

- <span data-ttu-id="b2d44-115">Formati di immagine supportati: JPEG, PNG, GIF, BMP.</span><span class="sxs-lookup"><span data-stu-id="b2d44-115">Supported image formats: JPEG, PNG, GIF, BMP.</span></span> 
- <span data-ttu-id="b2d44-116">Le dimensioni del file di immagine devono essere minori di 4 MB.</span><span class="sxs-lookup"><span data-stu-id="b2d44-116">Image file size must be less than  4 MB.</span></span>
- <span data-ttu-id="b2d44-117">Le dimensioni dell'immagine devono essere almeno 50 x 50.</span><span class="sxs-lookup"><span data-stu-id="b2d44-117">Image dimensions must be at least 50 x 50.</span></span>

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a><span data-ttu-id="b2d44-118">Identificare i punti di riferimento in un'immagine</span><span class="sxs-lookup"><span data-stu-id="b2d44-118">Identify landmarks in an image</span></span>

<span data-ttu-id="b2d44-119">Si inizierà con una chiamata che consente di trovare i punti di riferimento in un'immagine.</span><span class="sxs-lookup"><span data-stu-id="b2d44-119">Let's start with a call to find landmarks in an image.</span></span> <span data-ttu-id="b2d44-120">Per questo esempio si userà l'immagine seguente, ma è possibile provare lo stesso comando con URL ad altre immagini.</span><span class="sxs-lookup"><span data-stu-id="b2d44-120">We'll use the following image for this example, but you're free to try the same command with URLs to other images.</span></span> 

![Immagine di una catena montuosa con cielo blu sopra alle cime innevate.](../media/3-mountains.jpg)

<span data-ttu-id="b2d44-122">Eseguire il comando seguente in Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b2d44-122">Execute the following command in Azure Cloud Shell.</span></span> <span data-ttu-id="b2d44-123">Sostituire `<region>` nel comando con l'area dell'account di Servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="b2d44-123">Replace `<region>` in the command with the region of your cognitive services account.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

- <span data-ttu-id="b2d44-124">Questa chiamata ricerca punti di riferimento nell'immagine specificata dall'URL dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="b2d44-124">This call looks for landmarks in the image specified by the image URL.</span></span> <span data-ttu-id="b2d44-125">Per questo esercizio, l'immagine analizzata viene archiviata in un repository di GitHub.</span><span class="sxs-lookup"><span data-stu-id="b2d44-125">The image we are analyzing is stored in a GitHub repo for this exercise.</span></span> 
- <span data-ttu-id="b2d44-126">La chiamata chiede anche al servizio di restituire le informazioni sulla categoria e una descrizione dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="b2d44-126">The call also asks the service to return category information and a description of the image.</span></span> <span data-ttu-id="b2d44-127">La descrizione viene restituita come una frase inglese completa.</span><span class="sxs-lookup"><span data-stu-id="b2d44-127">The description is returned as a complete English sentence.</span></span> 
- <span data-ttu-id="b2d44-128">Come saprete, ogni chiamata all'API richiede una chiave di accesso,</span><span class="sxs-lookup"><span data-stu-id="b2d44-128">As you know, every call to the API needs an access key.</span></span> <span data-ttu-id="b2d44-129">che viene impostata nell'intestazione `Ocp-Apim-Subscription-Key` della richiesta.</span><span class="sxs-lookup"><span data-stu-id="b2d44-129">This is set in the `Ocp-Apim-Subscription-Key` header of the request.</span></span> 

> [!TIP]
> <span data-ttu-id="b2d44-130">Il risultato della richiesta è costituito dal file JSON non elaborato che descrive l'immagine in `url`.</span><span class="sxs-lookup"><span data-stu-id="b2d44-130">The result of the request is the raw JSON describing the picture in the `url`.</span></span> <span data-ttu-id="b2d44-131">È stato aggiunto ` | jq '.'` alla fine del comando per migliorare l'output del file JSON.</span><span class="sxs-lookup"><span data-stu-id="b2d44-131">We added ` | jq '.'` at the end of the command to prettify the JSON output.</span></span>

<span data-ttu-id="b2d44-132">La risposta JSON da questa chiamata restituisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b2d44-132">The JSON response from this call returns the following:</span></span>

- <span data-ttu-id="b2d44-133">Una matrice `categories` in cui sono elencate tutte le categorie di immagini rilevate, insieme a un punteggio compreso tra 0 e 1 che indica il livello di affidabilità dell'appartenenza dell'immagine alla categoria specificata.</span><span class="sxs-lookup"><span data-stu-id="b2d44-133">A `categories` array listing all image categories that were detected, along with a score between 0 and 1 of how confident the service is that the image belongs in the specified category.</span></span>
- <span data-ttu-id="b2d44-134">Una voce `description` contenente una matrice di tag o parole correlate all'immagine.</span><span class="sxs-lookup"><span data-stu-id="b2d44-134">A `description` entry containing an array of tags or words that are related to the image.</span></span>
- <span data-ttu-id="b2d44-135">Una voce `captions` con un campo di testo che descrive in inglese il contenuto dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="b2d44-135">A `captions` entry with a text field that describes in English what is in the image.</span></span> <span data-ttu-id="b2d44-136">Si noti che il testo ha anche un punteggio di certezza.</span><span class="sxs-lookup"><span data-stu-id="b2d44-136">Observe that the text also has a certainty score.</span></span> <span data-ttu-id="b2d44-137">Questo punteggio può essere utile per decidere come procedere con l'analisi.</span><span class="sxs-lookup"><span data-stu-id="b2d44-137">This score can help you decide what to do next with this analysis.</span></span>


## <a name="check-for-inappropriate-content-in-an-image"></a><span data-ttu-id="b2d44-138">Verificare la presenza di contenuti inappropriati in un'immagine</span><span class="sxs-lookup"><span data-stu-id="b2d44-138">Check for inappropriate content in an image</span></span>

<span data-ttu-id="b2d44-139">In questo esempio verrà analizzata un'immagine con contenuto per adulti.</span><span class="sxs-lookup"><span data-stu-id="b2d44-139">In this example, we'll analyze an image for adult content.</span></span> <span data-ttu-id="b2d44-140">Il punteggio di attendibilità classifica le probabilità che nell'immagine sia presente contenuto per adulti o spinto.</span><span class="sxs-lookup"><span data-stu-id="b2d44-140">The confidence score rates the likelihood that the image contains either adult or racy content.</span></span> 

<span data-ttu-id="b2d44-141">Per questo esempio si userà l'immagine seguente, ma è possibile provare lo stesso comando con URL ad altre immagini.</span><span class="sxs-lookup"><span data-stu-id="b2d44-141">We'll use the following image for this example, but you're free to try the same command with URLs to other images.</span></span> 

![Immagine di una famiglia sorridente.](../media/3-people.png)

1. <span data-ttu-id="b2d44-143">Eseguire il comando seguente in Azure Cloud Shell, sostituendo `<region>` nell'URL.</span><span class="sxs-lookup"><span data-stu-id="b2d44-143">Execute the following command in Azure Cloud Shell, replacing `<region>` in the URL.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

<span data-ttu-id="b2d44-144">In questo esempio `visualFeatures` è stato impostato su `Adult,Description`.</span><span class="sxs-lookup"><span data-stu-id="b2d44-144">In this example, we set the `visualFeatures` to `Adult,Description`.</span></span> 

<span data-ttu-id="b2d44-145">La risposta offre due punteggi di attendibilità, uno per il contenuto spinto e uno per il contenuto per adulti.</span><span class="sxs-lookup"><span data-stu-id="b2d44-145">The response gives us two confidence scores, one for racy content and one for adult content.</span></span> <span data-ttu-id="b2d44-146">Usando questi punteggi insieme alla descrizione dell'immagine e altre caratteristiche visive è possibile iniziare a contrassegnare le immagini inviate nel server.</span><span class="sxs-lookup"><span data-stu-id="b2d44-146">Using these scores plus the image description and other visual features, you can start to flag images posted to your server.</span></span>

<span data-ttu-id="b2d44-147">Gli esempi contenuti in questo esercizio danno un'idea del tipo di analisi che è possibile eseguire con l'operazione `analyze`.</span><span class="sxs-lookup"><span data-stu-id="b2d44-147">The examples we tried in this exercise give you an idea of the type of analysis that you can do with the `analyze` operation.</span></span> <span data-ttu-id="b2d44-148">Provare le analisi con immagini di diverse e diverse combinazioni di parametri visualFeatures, dettagli e linguaggi.</span><span class="sxs-lookup"><span data-stu-id="b2d44-148">Try out the analysis with different images and try different combinations of visualFeatures, details, and languages parameters.</span></span>

<span data-ttu-id="b2d44-149">Per altre informazioni sull'operazione `analyze`, vedere la documentazione di riferimento relativa all'[analisi dell'immagine](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).</span><span class="sxs-lookup"><span data-stu-id="b2d44-149">For more information about the `analyze` operation, see the [Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) reference documentation.</span></span>