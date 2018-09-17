<span data-ttu-id="63235-101">In questa unità verrà estratto testo da un'immagine con il servizio API Visione artificiale creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="63235-101">In this unit, you will extract text from an image with the Computer Vision API service that we created previously.</span></span>

# <a name="extracting-the-text-from-an-image"></a><span data-ttu-id="63235-102">Estrazione del testo da un'immagine</span><span class="sxs-lookup"><span data-stu-id="63235-102">Extracting the text from an image</span></span>

<span data-ttu-id="63235-103">Eseguire il comando `az cognitiveservices account keys list` per recuperare una chiave usata per l'autenticazione nell'API.</span><span class="sxs-lookup"><span data-stu-id="63235-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="63235-104">Archiviare l'output del comando entro la variabile `key`.</span><span class="sxs-lookup"><span data-stu-id="63235-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="63235-105">Eseguire un comando `curl` per effettuare una richiesta HTTP per l'API Visione artificiale e riutilizzare la variabile `key` dichiarata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="63235-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reuse the previously declared variable `key`.</span></span>

<span data-ttu-id="63235-106">L'immagine che verrà usata per il riconoscimento ottico dei caratteri (OCR) è la copertina del libro [.NET Microservices: Architecture for Containerized .NET Applications](/dotnet/standard/microservices-architecture/) (Microservizi .NET: Architettura per un'applicazione .NET in contenitori).</span><span class="sxs-lookup"><span data-stu-id="63235-106">The image we're going to be using for Optical Character Recognition (OCR) is the cover from the book [.NET Microservices: Architecture for Containerized .NET Applications](/dotnet/standard/microservices-architecture/).</span></span>

![Copertina dell'eBook](../images/ebook.png)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/ocr" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/ebook.png\"}" | jq '.'
```

<span data-ttu-id="63235-108">Le prime tre proprietà sono la lingua, l'orientamento e l'angolo del testo.</span><span class="sxs-lookup"><span data-stu-id="63235-108">The first three properties are the language, the orientation, and the text angle.</span></span> <span data-ttu-id="63235-109">Le proprietà `regions` conterranno un elenco di valori usati per indicare dove si trova il testo, la posizione nell'immagine e le parole effettive incluse.</span><span class="sxs-lookup"><span data-stu-id="63235-109">Then, the `regions` properties will contain a list of values used to show where the text is, its position in the picture, and the actual words it contains.</span></span>

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

<span data-ttu-id="63235-110">Provare a usare altre immagini per ottenere risultati diversi modificando l'URL usato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="63235-110">Experiment with other images to get different results by changing the URL used in the sample above.</span></span>