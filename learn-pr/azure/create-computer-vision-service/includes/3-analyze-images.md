In questa unità verranno analizzate le immagini con il servizio API Visione artificiale creato nel passaggio precedente.

# <a name="analyzing-an-image-with-computer-vision-api"></a>Analisi di un'immagine con l'API Visione artificiale

Eseguire il comando `az cognitiveservices account keys list` per recuperare una chiave usata per l'autenticazione nell'API. Archiviare l'output del comando nella variabile `key`.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Eseguire un comando `curl` per effettuare una richiesta HTTP per l'API Visione artificiale e riutilizzare la variabile `key` dichiarata in precedenza.

I parametri inviati al servizio sono `visualFeatures`, `details` e `languages`. Anche se il servizio funziona ugualmente senza di essi, forniscono al servizio informazioni sul contenuto delle immagini. Ad esempio, è possibile impostare `details` su `Landmarks` o `Celebrities` per identificare luoghi di interesse o celebrità. `visualFeatures` identifica la tipologia di informazioni che deve essere restituita dal servizio. L'opzione `Categories` classifica il contenuto delle immagini, ad esempio alberi, edifici e così via. `Faces` identifica i visi delle persone e fornisce indicazioni su sesso ed età.

Per informazioni dettagliate, vedere la [documentazione](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa).

Per adesso, verranno identificati i luoghi di interesse e verrà richiesto al servizio di restituire `Categories` e `Description`.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}"
```

Il risultato della richiesta è costituito dal file JSON non elaborato che descrive l'immagine in `url`. È anche possibile aggiungere ` | jq '.'` alla fine per migliorare l'output del file JSON.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" | jq '.'
```

Copiare il collegamento ad altre immagini trovate online e provare il servizio sostituendo l'URL nei comandi illustrati in precedenza.