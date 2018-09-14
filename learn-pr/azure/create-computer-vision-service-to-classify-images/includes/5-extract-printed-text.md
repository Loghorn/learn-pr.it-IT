Si supponga di che voler ora leggere il testo tra le immagini predefinite che il server di distribuzione nel sistema dalla post al server. In particolare, si vuole analizzare prodotto cercando adesivi promozionali che contiene i prezzi di vendita. È possibile provare la funzionalità di riconoscimento ottico dei caratteri (OCR) di API visione artificiale. 

## <a name="calling-the-computer-vision-api-to-extract-printed-text"></a>Chiamare l'API visione artificiale per estrarre il testo stampato

Il `ocr` operazione rileva testo in un'immagine ed estrae i caratteri riconosciuti in un flusso utilizzabile da computer. L'URL della richiesta ha il formato seguente:

`https://[location].api.cognitive.microsoft.com/vision/v2.0/ocr[?language][&detectOrientation ] `

Come di consueto, è necessario effettuare tutte le chiamate nel percorso in cui è stato creato l'account. La chiamata accetta due parametri facoltativi:

- **linguaggio**: il codice di lingua del testo rilevato nell'immagine. Il valore predefinito è `unk`, o sconosciuti. Questo verrà automaticamente il servizio rileva la lingua del testo nell'immagine.
- **detectOrientation**: se è true, il servizio tenta di rilevare l'orientamento dell'immagine e correggere l'errore prima dell'ulteriore elaborazione, ad esempio, se l'immagine viene capovolta. 

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="extract-printed-text-from-an-image-using-ocr"></a>Estrai testo stampato da un'immagine con OCR

L'immagine che verrà usata per il riconoscimento ottico dei caratteri (OCR) è la copertina del libro *.NET Microservices: Architecture for Containerized .NET Applications* (Microservizi .NET: Architettura per un'applicazione .NET in contenitori).

![Immagine della copertura di ebook Microservizi .NET: architettura per applicazioni .NET in contenitori](../media/5-ebook.png)

1. Eseguire i comandi seguenti in Azure Cloud Shell:

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/ocr" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json"  \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/ebook.png'}" \
 | jq '.'
```

Il codice JSON seguente è riportato un esempio di risposta che riceviamo da questa chiamata. Alcune righe di JSON sono stati rimossi per adattare il frammento di codice meglio nella pagina.

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

È possibile esaminare questa risposta in modo più dettagliato. 

- Il servizio identificato il testo in lingua inglese. Il valore della `language` campo contiene il codice di lingua BCP-47 del testo rilevato nell'immagine. In questo esempio viene utilizzato **en**, o in lingua inglese. 
- Il `orientation` è stata rilevata come **backup**. Questa proprietà è la direzione è rivolta verso la parte superiore del testo riconosciuto, dopo che l'immagine è stato ruotato intorno al centro in base all'angolo del testo rilevato. 
- Il `textAngle` è l'angolo mediante il quale deve essere ruotato il testo per diventare orizzontale o verticale. In questo esempio, il testo è stato orizzontale, in modo che il valore restituito è **0**.  
- Il `regions` proprietà contiene un elenco di valori utilizzati per mostrare dove è il testo, la posizione dell'immagine e la parola trovata in quella parte dell'immagine. 
- I quattro interi del `boundingBox` valore sono: 
    - la coordinata x del bordo sinistro 
    - la coordinata y del bordo superiore
    - la larghezza del rettangolo di selezione
    - l'altezza del rettangolo di selezione, 
   
    È possibile usare questi valori a disegnare caselle ciascun blocco di testo trovato nell'immagine.

Come si può notare in questo esercizio il `ocr` servizio fornisce informazioni dettagliate sul stampato il testo in un'immagine. 

Per altre informazioni sul `ocr` operazione, vedere la [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) documentazione di riferimento.