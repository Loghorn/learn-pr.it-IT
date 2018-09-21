Abbiamo visto come l'API Visione artificiale può estrarre il testo stampato dalle immagini. In questo esercizio si userà il servizio per rilevare il testo scritto a mano.

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a>Chiamata all'API Visione artificiale per estrarre testo scritto a mano

L'operazione `recognizeText` consente di rilevare ed estrarre testo scritto a mano da note, lettere, saggi, lavagne, moduli e così via. L'URL della richiesta ha il formato seguente:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=<...>`

Tutte le chiamate devono essere effettuate all'area in cui è stato creato l'account.

Se presente, il parametro `mode` deve essere impostato su `Handwritten` o `Printed` e fa distinzione tra maiuscole e minuscole. Se il parametro è impostato su `Handwritten` o viene omesso, viene eseguito il riconoscimento della grafia. Se il parametro è impostato su `Printed`, viene eseguito il riconoscimento del testo. Il tempo necessario per ottenere un risultato da questa chiamata dipende dalla quantità di testo nell'immagine.

Nell'esempio si procederà a:

- Stampare le intestazioni della risposta nella console usando l'opzione `-D` cUrl
- Copiare il valore dell'intestazione `Operation-Location` dalle intestazioni ricevute nella risposta
- Dopo alcuni secondi, verificare i risultati nell'URL specificato da `Operation-Location`

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a>Rilevare ed estrarre testo scritto a mano da un'immagine

Per questo esempio si userà l'immagine seguente, ma è possibile provare lo stesso comando con gli URL alle altre immagini.

![Immagine di un esempio di grafia su una nota](../media/6-handwriting.jpg)

1. Eseguire i comandi seguenti in Azure Cloud Shell. Sostituire `<region>` nel comando con l'area dell'account di Servizi cognitivi.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

I comandi precedenti eseguono il dump delle intestazioni di questa operazione nella console. Ad esempio:

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

L'intestazione `Operation-Location` è dove verranno registrati i risultati al termine dell'operazione.

2. Copiare il valore dell'intestazione `Operation-Location`.
1. Eseguire il comando seguente in Azure Cloud Shell sostituendo `"<Operation-Location>"` con il valore dell'intestazione **Operation-Location** copiato nel passaggio precedente.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

Se l'operazione è terminata, verrà restituito un file JSON contenente il risultato della richiesta di riconoscimento della grafia.

Per altre informazioni sull'operazione `recognizeText`, vedere la documentazione di riferimento relativa al [riconoscimento del testo scritto a mano](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200).