<span data-ttu-id="aad63-101">In questa unità si estrarrà la grafia da un'immagine con il servizio API Visione artificiale creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="aad63-101">In this unit, you will extract handwriting from an image with the Computer Vision API service that we created previously.</span></span>

# <a name="extracting-the-handwriting-from-an-image"></a><span data-ttu-id="aad63-102">Estrazione della grafia da un'immagine</span><span class="sxs-lookup"><span data-stu-id="aad63-102">Extracting the handwriting from an image</span></span>

<span data-ttu-id="aad63-103">Eseguire il comando `az cognitiveservices account keys list` per recuperare una chiave usata per l'autenticazione nell'API.</span><span class="sxs-lookup"><span data-stu-id="aad63-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="aad63-104">Archiviare l'output del comando nella variabile `key`.</span><span class="sxs-lookup"><span data-stu-id="aad63-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="aad63-105">Eseguire un comando `curl` per effettuare una richiesta HTTP all'API Visione artificiale e riutilizzare la variabile `key` dichiarata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="aad63-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reuse the previously declared variable `key`.</span></span>

<span data-ttu-id="aad63-106">L'immagine che verrà usata per il riconoscimento della grafia è un'immagine di questa area note.</span><span class="sxs-lookup"><span data-stu-id="aad63-106">The image we're going to be using for handwriting recognition is a picture of this note board.</span></span>

![Grafia](../images/handwriting.jpg)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/handwriting.jpg\"}" -D -
```

<span data-ttu-id="aad63-108">Il comando precedente restituisce come output le intestazioni.</span><span class="sxs-lookup"><span data-stu-id="aad63-108">The command above will output the headers.</span></span> <span data-ttu-id="aad63-109">Copiare il valore dell'intestazione `Operation-Location` nel comando seguente.</span><span class="sxs-lookup"><span data-stu-id="aad63-109">Copy the `Operation-Location` header value into the command below.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>"
```

<span data-ttu-id="aad63-110">Verrà restituito un file JSON contenente il risultato della richiesta di riconoscimento della grafia.</span><span class="sxs-lookup"><span data-stu-id="aad63-110">You will now receive a JSON file containing the result of the handwriting recognition request.</span></span>

<span data-ttu-id="aad63-111">Provare a usare altre immagini per ottenere risultati diversi modificando l'URL usato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="aad63-111">Experiment with other images to get different results by changing the URL used in the sample above.</span></span>