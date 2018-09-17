In questa unità verranno generate le anteprime da un'immagine di origine con il servizio API Visione artificiale creato in precedenza.

# <a name="generate-a-thumbnail-from-an-image-with-computer-vision-api"></a>Generare un'anteprima da un'immagine con l'API Visione artificiale

Eseguire il comando `az cognitiveservices account keys list` per recuperare una chiave usata per l'autenticazione nell'API. Archiviare l'output del comando entro la variabile `key`.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Eseguire un comando `curl` per effettuare una richiesta HTTP per l'API Visione artificiale e riutilizzare la variabile `key` dichiarata in precedenza.

È possibile fornire all'API parametri diversi per generare l'anteprima corretta in base alle esigenze. `width` e `height` sono obbligatori e indicano all'API le dimensioni necessarie per le immagini specifiche. Infine, il parametro `smartCropping` genera un ritaglio più intelligente tramite l'analisi dell'area di interesse nell'immagine affinché questa venga mantenuta nell'anteprima. Ad esempio, con il ritaglio intelligente abilitato, l'immagine ritagliata del profilo mantiene il viso della persona all'interno della cornice anche quando le proporzioni dell'immagine sono diverse rispetto a quelle richieste.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" -o clouddrive/thumbnail.jpg
```

# <a name="downloading-the-thumbnail"></a>Download dell'anteprima

L'anteprima generata sarà disponibile nell'account di archiviazione di Azure Cloud Shell, in un gruppo di risorse denominato `cloud-shell-storage-<region>`.

1. Accedere all'account di archiviazione generato automaticamente.

![immagine](../images/storage-account.png)

2. Fare clic sulla sezione File.

![immagine](../images/storage-account-click-on-files.png)

3. L'anteprima verrà visualizzata nella radice del contenitore.

![immagine](../images/storage-account-thumbnail.png)

4. Fare clic sul file e quindi scaricarlo.

Dalla cartella Download, aprire l'immagine da `100x100` pixel con qualsiasi visualizzatore di immagini.