In questa unità verrà estratto testo da un'immagine con il servizio API Visione artificiale creato in precedenza.

# <a name="extracting-the-text-from-an-image"></a>Estrazione del testo da un'immagine

Eseguire il comando `az cognitiveservices account keys list` per recuperare una chiave usata per l'autenticazione nell'API. Archiviare l'output del comando nella variabile `key`.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Eseguire un comando `curl` per effettuare una richiesta HTTP all'API Visione artificiale e riutilizzare la variabile `key` dichiarata in precedenza.

L'immagine che verrà usata per il riconoscimento ottico dei caratteri (OCR) è la copertina del libro [.NET Microservices: Architecture for Containerized .NET Applications](/dotnet/standard/microservices-architecture/) (Microservizi .NET: Architettura per un'applicazione .NET in contenitori).

![Copertina dell'eBook](../images/ebook.png)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/ocr" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/ebook.png\"}" | jq '.'
```

Le prime tre proprietà sono la lingua, l'orientamento e l'angolo del testo. Le proprietà `regions` conterranno un elenco di valori usati per indicare dove si trova il testo, la posizione nell'immagine e le effettive parole incluse.

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

Provare a usare altre immagini per ottenere risultati diversi modificando l'URL usato nell'esempio precedente.