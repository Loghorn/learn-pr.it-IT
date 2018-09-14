Abbiamo visto come l'API visione artificiale può estrarre il testo stampato dalle immagini. In questo esercizio si userà il servizio per rilevare il testo scritto a mano.

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a>Chiamare l'API visione artificiale per estrarre testo scritto a mano

Il `recognizeText` operazione rileva e consente di estrarre testo scritto a mano da note, lettere, saggi, lavagne, moduli e altre origini. L'URL della richiesta ha il formato seguente:

`https://[location].api.cognitive.microsoft.com/vision/v2.0/recognizeText[?mode] `

Come di consueto, è necessario effettuare tutte le chiamate nel percorso in cui è stato creato l'account. Se il `mode` è possibile impostare parametri ust `Handwritten` o `Printed` e tra maiuscole e minuscole. Se il parametro è impostato su `Handwritten` o viene omesso, viene eseguito il riconoscimento della grafia. In caso contrario, si imposta il parametro su `Printed` riconoscimento del testo stampato viene eseguito chiamando l'operazione di riconoscimento.

Il tempo che necessario per ottenere un risultato di questa chiamata dipende dalla quantità di riconoscimento della grafia nell'immagine. Quindi, potremmo occorre attendere alcuni secondi. Pertanto, in questo esempio, si sarà:

- Scarica le intestazioni della risposta nella console usando cUrl `-D` opzione
- Copia il `Operation-Location` dalle intestazioni è visualizzato nella risposta del valore dell'intestazione
- Dopo alcuni secondi, verificare che l'URL specificato da di `Operation-Location` per i risultati

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a>Rilevare ed estrarre testo scritto a mano da un'immagine

In questo esempio useremo l'immagine seguente, ma è possibile ripetere lo stesso comando con gli URL per le altre immagini.

![Immagine di un esempio di riconoscimento della grafia una nota](../media/6-handwriting.jpg)

1. Eseguire i comandi seguenti in Azure Cloud Shell:

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

Il codice precedente esegue il dump le intestazioni di questa operazione nella console. Ad esempio:

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

Il `Operation-Location` intestazione viene in cui verranno registrati i risultati al termine dell'operazione.

2. Copia il `Operation-Location` valore dell'intestazione.
1. Eseguire il comando seguente in Azure Cloud Shell sostituendo `"<Operation-Location>"` con il valore per il **Operation-Location** intestazione è stato copiato nel passaggio precedente.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

Se l'operazione è stata completata, si riceverà un file JSON che contiene il risultato della richiesta di riconoscimento della grafia.

Per altre informazioni sul `recognizeText` operazione, vedere la [riconoscere testo scritto a mano](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) documentazione di riferimento.