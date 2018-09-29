<span data-ttu-id="ba10c-101">Si supponga di voler leggere il testo nelle immagini dello stock che i principali distributori pubblicano nel server.</span><span class="sxs-lookup"><span data-stu-id="ba10c-101">Suppose you now want to read text from the stock images that your frontline distributors post to your server.</span></span> <span data-ttu-id="ba10c-102">In particolare, si desidera individuare gli adesivi promozionali che contengono prezzi scontati.</span><span class="sxs-lookup"><span data-stu-id="ba10c-102">In particular, you want to scan product looking for promotional stickers that containing sale prices.</span></span> <span data-ttu-id="ba10c-103">La funzionalità di riconoscimento ottico dei caratteri (OCR) di Visione artificiale fa al caso nostro.</span><span class="sxs-lookup"><span data-stu-id="ba10c-103">It's time to try the optical character recognition (OCR) feature of the Computer Vision API.</span></span> 

## <a name="calling-the-computer-vision-api-to-extract-printed-text"></a><span data-ttu-id="ba10c-104">Chiamata all'API Visione artificiale per estrarre testo stampato</span><span class="sxs-lookup"><span data-stu-id="ba10c-104">Calling the Computer Vision API to extract printed text</span></span>

<span data-ttu-id="ba10c-105">Con l'operazione `ocr` è possibile rilevare il testo stampato in un'immagine ed estrarre i caratteri riconosciuti in un flusso utilizzabile da computer.</span><span class="sxs-lookup"><span data-stu-id="ba10c-105">The `ocr` operation detects text in an image and extracts the recognized characters into a machine-usable character stream.</span></span> <span data-ttu-id="ba10c-106">L'URL della richiesta ha il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="ba10c-106">The request URL has the following format:</span></span>

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr?language=<...>&detectOrientation=<...>`

<span data-ttu-id="ba10c-107">Come di consueto, tutte le chiamate devono essere effettuate all'area in cui è stato creato l'account.</span><span class="sxs-lookup"><span data-stu-id="ba10c-107">As usual, all calls must be made to the region where the account was created.</span></span> <span data-ttu-id="ba10c-108">La chiamata accetta due parametri facoltativi:</span><span class="sxs-lookup"><span data-stu-id="ba10c-108">The call accepts two optional parameters:</span></span>

- <span data-ttu-id="ba10c-109">**language**: il codice della lingua del testo rilevato nell'immagine.</span><span class="sxs-lookup"><span data-stu-id="ba10c-109">**language**: The language code of the text to be detected in the image.</span></span> <span data-ttu-id="ba10c-110">Il valore predefinito è `unk` o sconosciuto.</span><span class="sxs-lookup"><span data-stu-id="ba10c-110">The default value is `unk`,or unknown.</span></span> <span data-ttu-id="ba10c-111">Ciò lascia che il servizio rilevi automaticamente la lingua del testo nell'immagine.</span><span class="sxs-lookup"><span data-stu-id="ba10c-111">This let's the service auto detect the language of the text in the image.</span></span>
- <span data-ttu-id="ba10c-112">**detectOrientation**: se è true, il servizio tenta di rilevare l'orientamento dell'immagine e correggerla prima di ulteriori elaborazioni, ad esempio, nel caso in cui l'immagine fosse capovolta.</span><span class="sxs-lookup"><span data-stu-id="ba10c-112">**detectOrientation**: When true, the service  tries to detect the image orientation and correct it before further processing, for example, whether the image is upside-down.</span></span> 

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="extract-printed-text-from-an-image-using-ocr"></a><span data-ttu-id="ba10c-113">Estrarre testo stampato da un'immagine mediante OCR</span><span class="sxs-lookup"><span data-stu-id="ba10c-113">Extract printed text from an image using OCR</span></span>

<span data-ttu-id="ba10c-114">L'immagine che verrà usata per il riconoscimento ottico dei caratteri (OCR) è la copertina del libro *.NET Microservices: Architecture for Containerized .NET Applications* (Microservizi .NET: Architettura per un'applicazione .NET in contenitori).</span><span class="sxs-lookup"><span data-stu-id="ba10c-114">The image we're going to be using for Optical Character Recognition (OCR) is the cover from the book *.NET Microservices: Architecture for Containerized .NET Applications*.</span></span>

![Immagine della copertina del libro .NET Microservices: Architecture for Containerized .NET Applications](../media/5-ebook.png)

1. <span data-ttu-id="ba10c-116">Eseguire il comando seguente in Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="ba10c-116">Execute the following command in Cloud Shell.</span></span> <span data-ttu-id="ba10c-117">Sostituire `<region>` nel comando con l'area dell'account di Servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="ba10c-117">Replace `<region>` in the command with the region of your cognitive services account.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json"  \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/ebook.png'}" \
 | jq '.'
```

<span data-ttu-id="ba10c-118">Il documento JSON seguente è un esempio della risposta ricevuta da questa chiamata.</span><span class="sxs-lookup"><span data-stu-id="ba10c-118">The following JSON is an example of the response we get from this call.</span></span> <span data-ttu-id="ba10c-119">Alcune righe di JSON sono stati rimosse per adattare il frammento di codice alla pagina.</span><span class="sxs-lookup"><span data-stu-id="ba10c-119">Some lines of JSON have been removed to make the snippet fit better on the page.</span></span>

```json
{
  "language": "en",
  "orientation": "Up",
  "textAngle": 0,
  "regions" : [
        /*... snipped*/
        {
          "boundingBox": "766,1419,302,33",
          "words": [
            {
              "boundingBox": "766,1419,126,25",
              "text": "Microsoft"
            },
            {
              "boundingBox": "903,1420,165,32",
              "text": "Corporation"
            }
          ]
        }]
}
```

<span data-ttu-id="ba10c-120">Esaminiamo questa risposta in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="ba10c-120">Let's examine this response in more detail.</span></span> 

- <span data-ttu-id="ba10c-121">Il servizio ha identificato la lingua del testo come inglese.</span><span class="sxs-lookup"><span data-stu-id="ba10c-121">The service identified the text as being English.</span></span> <span data-ttu-id="ba10c-122">Il valore del campo `language` contiene il codice lingua BCP-47 del testo rilevato nell'immagine.</span><span class="sxs-lookup"><span data-stu-id="ba10c-122">The value of the `language` field contains the BCP-47 language code of the text detected in the image.</span></span> <span data-ttu-id="ba10c-123">In questo esempio si tratta di **en** o Inglese.</span><span class="sxs-lookup"><span data-stu-id="ba10c-123">In this example it is **en**, or English.</span></span> 
- <span data-ttu-id="ba10c-124">Il valore di `orientation` rilevato è **up**.</span><span class="sxs-lookup"><span data-stu-id="ba10c-124">The `orientation` was detected as **up**.</span></span> <span data-ttu-id="ba10c-125">Questa proprietà indica la direzione verso cui è rivolta la parte superiore del testo riconosciuto, dopo che l'immagine è stata ruotata intorno al suo centro in funzione dell'inclinazione rilevata nel testo.</span><span class="sxs-lookup"><span data-stu-id="ba10c-125">This property is the direction that the top of the recognized text is facing, after the image has been rotated around its center according to the detected text angle.</span></span> 
- <span data-ttu-id="ba10c-126">`textAngle` indica l'angolo di rotazione da imprimere al testo per farlo diventare orizzontale o verticale.</span><span class="sxs-lookup"><span data-stu-id="ba10c-126">The `textAngle` is the angle by which the text must be rotated to become horizontal or vertical.</span></span> <span data-ttu-id="ba10c-127">In questo esempio, il testo era perfettamente orizzontale, quindi il valore restituito è **0**.</span><span class="sxs-lookup"><span data-stu-id="ba10c-127">In this example, the text was perfectly horizontal, so the value returned is **0**.</span></span>  
- <span data-ttu-id="ba10c-128">La proprietà `regions` contiene un elenco di valori usati per indicare dove si trova il testo, la posizione nell'immagine e la parole trovata in quella parte dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="ba10c-128">The `regions` property contains a list of values used to show where the text is, its position in the picture, and the word found in that part of the image.</span></span> 
- <span data-ttu-id="ba10c-129">I quattro numeri interi del valore `boundingBox` sono:</span><span class="sxs-lookup"><span data-stu-id="ba10c-129">The four integers of the `boundingBox` value are:</span></span> 
    - <span data-ttu-id="ba10c-130">la coordinata x del bordo sinistro</span><span class="sxs-lookup"><span data-stu-id="ba10c-130">the x-coordinate of the left edge</span></span> 
    - <span data-ttu-id="ba10c-131">la coordinata y del bordo superiore</span><span class="sxs-lookup"><span data-stu-id="ba10c-131">the y-coordinate of the top edge</span></span>
    - <span data-ttu-id="ba10c-132">la larghezza del rettangolo di selezione</span><span class="sxs-lookup"><span data-stu-id="ba10c-132">the width of the bounding box</span></span>
    - <span data-ttu-id="ba10c-133">l'altezza del rettangolo di selezione</span><span class="sxs-lookup"><span data-stu-id="ba10c-133">the height of the bounding box,</span></span> 
   
    <span data-ttu-id="ba10c-134">È possibile usare questi valori per disegnare riquadri attorno a ciascun blocco di testo trovato nell'immagine.</span><span class="sxs-lookup"><span data-stu-id="ba10c-134">You can use these values to draw boxes around every piece of text found in the image.</span></span>

<span data-ttu-id="ba10c-135">Come si può notare in questo esercizio, il servizio `ocr` fornisce informazioni dettagliate sul testo stampato in un'immagine.</span><span class="sxs-lookup"><span data-stu-id="ba10c-135">As you can see in this exercise, the `ocr` service gives detailed information about the printed text in an image.</span></span> 

<span data-ttu-id="ba10c-136">Per altre informazioni sull'operazione `ocr`, vedere la documentazione di riferimento relativa a [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc).</span><span class="sxs-lookup"><span data-stu-id="ba10c-136">For more information about the `ocr` operation, see the [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) reference documentation.</span></span>