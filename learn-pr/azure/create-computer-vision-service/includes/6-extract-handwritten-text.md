In questa unità verrà estratta la grafia da un'immagine con il servizio API Visione artificiale creato in precedenza.

# <a name="extracting-the-handwriting-from-an-image"></a>Estrazione della grafia da un'immagine

Eseguire il comando `az cognitiveservices account keys list` per recuperare una chiave usata per l'autenticazione nell'API. Archiviare l'output del comando entro la variabile `key`.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Eseguire un comando `curl` per effettuare una richiesta HTTP per l'API Visione artificiale e riutilizzare la variabile `key` dichiarata in precedenza.

L'immagine che verrà usata per il riconoscimento della grafia è un'immagine di questa area note.

![Grafia](../images/handwriting.jpg)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/handwriting.jpg\"}" -D -
```

Il comando precedente genererà l'output delle intestazioni. Copiare il valore dell'intestazione `Operation-Location` nel comando seguente.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>"
```

Verrà restituito un file JSON contenente il risultato della richiesta di riconoscimento della grafia.

Provare a usare altre immagini per ottenere risultati diversi modificando l'URL usato nell'esempio precedente.