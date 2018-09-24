Si supponga di voler leggere il testo nelle immagini dello stock che i principali distributori pubblicano nel server. In particolare, si desidera individuare gli adesivi promozionali che contengono prezzi scontati. La funzionalità di riconoscimento ottico dei caratteri (OCR) di Visione artificiale fa al caso nostro. 

## <a name="calling-the-computer-vision-api-to-extract-printed-text"></a>Chiamata all'API Visione artificiale per estrarre testo stampato

Con l'operazione `ocr` è possibile rilevare il testo stampato in un'immagine ed estrarre i caratteri riconosciuti in un flusso utilizzabile da computer. L'URL della richiesta ha il formato seguente:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr?language=<...>&detectOrientation=<...>`

Come di consueto, tutte le chiamate devono essere effettuate all'area in cui è stato creato l'account. La chiamata accetta due parametri facoltativi:

- **language**: il codice della lingua del testo rilevato nell'immagine. Il valore predefinito è `unk` o sconosciuto. Ciò lascia che il servizio rilevi automaticamente la lingua del testo nell'immagine.
- **detectOrientation**: se è true, il servizio tenta di rilevare l'orientamento dell'immagine e correggerla prima di ulteriori elaborazioni, ad esempio, nel caso in cui l'immagine fosse capovolta. 

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="extract-printed-text-from-an-image-using-ocr"></a>Estrarre testo stampato da un'immagine mediante OCR

L'immagine che verrà usata per il riconoscimento ottico dei caratteri (OCR) è la copertina del libro *.NET Microservices: Architecture for Containerized .NET Applications* (Microservizi .NET: Architettura per un'applicazione .NET in contenitori).

![Immagine della copertina del libro .NET Microservices: Architecture for Containerized .NET Applications](../media/5-ebook.png)

1. Eseguire il comando seguente in Cloud Shell. Sostituire `<region>` nel comando con l'area dell'account di Servizi cognitivi.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json"  \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/ebook.png'}" \
 | jq '.'
```

Il documento JSON seguente è un esempio della risposta ricevuta da questa chiamata. Alcune righe di JSON sono stati rimosse per adattare il frammento di codice alla pagina.

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

Esaminiamo questa risposta in modo più dettagliato. 

- Il servizio ha identificato la lingua del testo come inglese. Il valore del campo `language` contiene il codice lingua BCP-47 del testo rilevato nell'immagine. In questo esempio si tratta di **en** o Inglese. 
- Il valore di `orientation` rilevato è **up**. Questa proprietà indica la direzione verso cui è rivolta la parte superiore del testo riconosciuto, dopo che l'immagine è stata ruotata intorno al suo centro in funzione dell'inclinazione rilevata nel testo. 
- `textAngle` indica l'angolo di rotazione da imprimere al testo per farlo diventare orizzontale o verticale. In questo esempio, il testo era perfettamente orizzontale, quindi il valore restituito è **0**.  
- La proprietà `regions` contiene un elenco di valori usati per indicare dove si trova il testo, la posizione nell'immagine e la parole trovata in quella parte dell'immagine. 
- I quattro numeri interi del valore `boundingBox` sono: 
    - la coordinata x del bordo sinistro 
    - la coordinata y del bordo superiore
    - la larghezza del rettangolo di selezione
    - l'altezza del rettangolo di selezione 
   
    È possibile usare questi valori per disegnare riquadri attorno a ciascun blocco di testo trovato nell'immagine.

Come si può notare in questo esercizio, il servizio `ocr` fornisce informazioni dettagliate sul testo stampato in un'immagine. 

Per altre informazioni sull'operazione `ocr`, vedere la documentazione di riferimento relativa a [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc).