<span data-ttu-id="4056f-101">Abbiamo visto come l'API Visione artificiale può estrarre il testo stampato dalle immagini.</span><span class="sxs-lookup"><span data-stu-id="4056f-101">We saw how the Computer Vision API can extract printed text from images.</span></span> <span data-ttu-id="4056f-102">In questo esercizio si userà il servizio per rilevare il testo scritto a mano.</span><span class="sxs-lookup"><span data-stu-id="4056f-102">In this exercise, we'll use the service to detect handwritten text.</span></span>

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a><span data-ttu-id="4056f-103">Chiamata all'API Visione artificiale per estrarre testo scritto a mano</span><span class="sxs-lookup"><span data-stu-id="4056f-103">Calling the Computer Vision API to extract handwritten text</span></span>

<span data-ttu-id="4056f-104">L'operazione `recognizeText` consente di rilevare ed estrarre testo scritto a mano da note, lettere, saggi, lavagne, moduli e così via.</span><span class="sxs-lookup"><span data-stu-id="4056f-104">The `recognizeText` operation detects and extracts handwritten text from notes, letters, essays, whiteboards, forms, and other sources.</span></span> <span data-ttu-id="4056f-105">L'URL della richiesta ha il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="4056f-105">The request URL has the following format:</span></span>

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=<...>`

<span data-ttu-id="4056f-106">Tutte le chiamate devono essere effettuate all'area in cui è stato creato l'account.</span><span class="sxs-lookup"><span data-stu-id="4056f-106">All calls must be made to the region where the account was created.</span></span>

<span data-ttu-id="4056f-107">Se presente, il parametro `mode` deve essere impostato su `Handwritten` o `Printed` e fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4056f-107">If present, the `mode` parameter must be set to `Handwritten` or `Printed` and is case-sensitive.</span></span> <span data-ttu-id="4056f-108">Se il parametro è impostato su `Handwritten` o viene omesso, viene eseguito il riconoscimento della grafia.</span><span class="sxs-lookup"><span data-stu-id="4056f-108">If the parameter is set to `Handwritten` or is not specified, handwriting recognition is performed.</span></span> <span data-ttu-id="4056f-109">Se il parametro è impostato su `Printed`, viene eseguito il riconoscimento del testo.</span><span class="sxs-lookup"><span data-stu-id="4056f-109">If the parameter is set to `Printed`, then printed text recognition is performed.</span></span> <span data-ttu-id="4056f-110">Il tempo necessario per ottenere un risultato da questa chiamata dipende dalla quantità di testo nell'immagine.</span><span class="sxs-lookup"><span data-stu-id="4056f-110">The time it takes to get a result from this call depends on the amount of writing in the image.</span></span>

<span data-ttu-id="4056f-111">Nell'esempio si procederà a:</span><span class="sxs-lookup"><span data-stu-id="4056f-111">In this example, we'll:</span></span>

- <span data-ttu-id="4056f-112">Stampare le intestazioni della risposta nella console usando l'opzione `-D` cUrl</span><span class="sxs-lookup"><span data-stu-id="4056f-112">Print the response headers to the console using cUrl's `-D` option</span></span>
- <span data-ttu-id="4056f-113">Copiare il valore dell'intestazione `Operation-Location` dalle intestazioni ricevute nella risposta</span><span class="sxs-lookup"><span data-stu-id="4056f-113">Copy the `Operation-Location` header value from the headers we receive in the response</span></span>
- <span data-ttu-id="4056f-114">Dopo alcuni secondi, verificare i risultati nell'URL specificato da `Operation-Location`</span><span class="sxs-lookup"><span data-stu-id="4056f-114">After a few seconds, check the URL specified by the `Operation-Location` for the results</span></span>

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a><span data-ttu-id="4056f-115">Rilevare ed estrarre testo scritto a mano da un'immagine</span><span class="sxs-lookup"><span data-stu-id="4056f-115">Detect and extract handwritten text from an image</span></span>

<span data-ttu-id="4056f-116">Per questo esempio si userà l'immagine seguente, ma è possibile provare lo stesso comando con gli URL alle altre immagini.</span><span class="sxs-lookup"><span data-stu-id="4056f-116">We'll use the following image in this example, but you're free to try the same command with URLs to other images.</span></span>

![Immagine di un esempio di grafia su una nota](../media/6-handwriting.jpg)

1. <span data-ttu-id="4056f-118">Eseguire i comandi seguenti in Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="4056f-118">Execute the following commands in Azure Cloud Shell.</span></span> <span data-ttu-id="4056f-119">Sostituire `<region>` nel comando con l'area dell'account di Servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="4056f-119">Replace `<region>` in the command with the region of your cognitive services account.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

<span data-ttu-id="4056f-120">I comandi precedenti eseguono il dump delle intestazioni di questa operazione nella console.</span><span class="sxs-lookup"><span data-stu-id="4056f-120">The above dumps the headers of this operation to the console.</span></span> <span data-ttu-id="4056f-121">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4056f-121">Here's an example:</span></span>

```azurecli
HTTP/1.1 202 Accepted
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Expires: -1
Operation-Location: https://westus2.api.cognitive.microsoft.com/vision/v2.0/textOperations/d0e9b397-4072-471c-ae61-7490bec8f077
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
apim-request-id: f5663487-03c6-4760-9be7-c9157fac10a1
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Date: Wed, 12 Sep 2018 19:22:00 GMT
```

<span data-ttu-id="4056f-122">L'intestazione `Operation-Location` è dove verranno registrati i risultati al termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="4056f-122">The `Operation-Location` header is where the results will be posted once complete.</span></span>

2. <span data-ttu-id="4056f-123">Copiare il valore dell'intestazione `Operation-Location`.</span><span class="sxs-lookup"><span data-stu-id="4056f-123">Copy the `Operation-Location` header value.</span></span>
1. <span data-ttu-id="4056f-124">Eseguire il comando seguente in Azure Cloud Shell sostituendo `"<Operation-Location>"` con il valore dell'intestazione **Operation-Location** copiato nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="4056f-124">Execute the following command in Azure Cloud Shell replacing `"<Operation-Location>"` with the value for the **Operation-Location** header you copied from the preceding step.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

<span data-ttu-id="4056f-125">Se l'operazione è terminata, verrà restituito un file JSON contenente il risultato della richiesta di riconoscimento della grafia.</span><span class="sxs-lookup"><span data-stu-id="4056f-125">If the operation has completed, you'll receive a JSON file containing the result of the handwriting recognition request.</span></span>

<span data-ttu-id="4056f-126">Per altre informazioni sull'operazione `recognizeText`, vedere la documentazione di riferimento relativa al [riconoscimento del testo scritto a mano](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200).</span><span class="sxs-lookup"><span data-stu-id="4056f-126">For more information about the `recognizeText` operation, see the [Recognize Handwritten Text](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) reference documentation.</span></span>