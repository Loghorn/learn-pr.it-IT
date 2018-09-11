<span data-ttu-id="98311-101">In questa unità verranno generate le anteprime da un'immagine di origine con il servizio API Visione artificiale creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="98311-101">In this unit, you will generate thumbnails from a source image with the Computer Vision API service that we created previously.</span></span>

# <a name="generate-a-thumbnail-from-an-image-with-computer-vision-api"></a><span data-ttu-id="98311-102">Generare un'anteprima da un'immagine con l'API Visione artificiale</span><span class="sxs-lookup"><span data-stu-id="98311-102">Generate a thumbnail from an image with Computer Vision API</span></span>

<span data-ttu-id="98311-103">Eseguire il comando `az cognitiveservices account keys list` per recuperare una chiave usata per l'autenticazione nell'API.</span><span class="sxs-lookup"><span data-stu-id="98311-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="98311-104">Archiviare l'output del comando nella variabile `key`.</span><span class="sxs-lookup"><span data-stu-id="98311-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="98311-105">Eseguire un comando `curl` per effettuare una richiesta HTTP per l'API Visione artificiale e riutilizzare la variabile `key` dichiarata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="98311-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reusing the previously declared variable `key`.</span></span>

<span data-ttu-id="98311-106">È possibile fornire all'API parametri diversi per generare l'anteprima corretta in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="98311-106">Different parameters can be provided to the API to generate the proper thumbnail for your needs.</span></span> <span data-ttu-id="98311-107">`width` e `height` sono obbligatori e indicano all'API le dimensioni necessarie per le immagini specifiche.</span><span class="sxs-lookup"><span data-stu-id="98311-107">`width` and `height` are required and will tell the API which size you need for a specific images.</span></span> <span data-ttu-id="98311-108">Infine, il parametro `smartCropping` genera un ritaglio più intelligente tramite l'analisi dell'area di interesse nell'immagine affinché questa venga mantenuta nell'anteprima.</span><span class="sxs-lookup"><span data-stu-id="98311-108">Finally, the `smartCropping` parameter generate a smarter cropping by analyzing the region of interest in your image as to keep them within the thumbnails.</span></span> <span data-ttu-id="98311-109">Ad esempio, con il ritaglio intelligente abilitato, l'immagine ritagliata del profilo mantiene il viso della persona all'interno della cornice anche quando le proporzioni dell'immagine sono diverse rispetto a quelle richieste.</span><span class="sxs-lookup"><span data-stu-id="98311-109">As an example, with smart cropping enabled, cropped profile picture would keep someone's face within the picture frame even when the picture isn't in the same ratio as the one that we asked.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" -o clouddrive/thumbnail.jpg
```

# <a name="downloading-the-thumbnail"></a><span data-ttu-id="98311-110">Download dell'anteprima</span><span class="sxs-lookup"><span data-stu-id="98311-110">Downloading the thumbnail</span></span>

<span data-ttu-id="98311-111">L'anteprima generata sarà disponibile nell'account di archiviazione di Cloud Shell, in un gruppo di risorse denominato `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="98311-111">The generated thumbnail will be found in your Cloud Shell storage account within a resource group named `cloud-shell-storage-<region>`.</span></span>

1. <span data-ttu-id="98311-112">Accedere all'account di archiviazione generato automaticamente</span><span class="sxs-lookup"><span data-stu-id="98311-112">Get into the automatically generated storage account</span></span>

![immagine](../images/storage-account.png)

2. <span data-ttu-id="98311-114">Fare clic sulla sezione File</span><span class="sxs-lookup"><span data-stu-id="98311-114">Click on the files section</span></span>

![immagine](../images/storage-account-click-on-files.png)

3. <span data-ttu-id="98311-116">L'anteprima verrà visualizzata nella radice del contenitore.</span><span class="sxs-lookup"><span data-stu-id="98311-116">You will find the thumbnail at the root of the container.</span></span>

![immagine](../images/storage-account-thumbnail.png)

4. <span data-ttu-id="98311-118">Fare clic sul file e quindi scaricarlo</span><span class="sxs-lookup"><span data-stu-id="98311-118">Click on the file then download it</span></span>

<span data-ttu-id="98311-119">Dalla cartella Download, aprire l'immagine da `100x100` pixel con qualsiasi visualizzatore di immagini.</span><span class="sxs-lookup"><span data-stu-id="98311-119">From within your download folder, you can open the `100x100` pixels image with any image viewer.</span></span>