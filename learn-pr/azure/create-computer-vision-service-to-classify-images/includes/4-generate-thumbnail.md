I distributori principali dei prodotti dell'azienda scansionano ed eseguono l'upload degli scaffali del magazzino di cui eseguono il restocking. L'utente è un responsabile dello sviluppo presso l'azienda e deve creare anteprime delle immagini. Le anteprime vengono usate nei report online creati dall'utente per il team di vendite. Di recente, il responsabile vendite ha comunicato che le immagini nei report sono sfocate e spesso non inquadrano la parte anteriore e centrale del prodotto. Questo complica la scansione e la creazione del report di grandi dimensioni. Spetta all'utente migliorare la situazione.

L'utente decide di provare la funzionalità di generazione di anteprime dell'API Visione artificiale. Si aspetta di ottenere risultati migliori di quelli restituiti dalla funzione di ridimensionamento scritta dall'utente stesso.

Visione artificiale genera prima di tutto un'anteprima di qualità elevata e quindi analizza gli oggetti inclusi nell'immagine per determinare l'area di interesse. Quindi Visione artificiale ritaglia l'immagine per soddisfare i requisiti dell'area di interesse. L'anteprima generata può essere visualizzata con proporzioni diverse da quelle dell'immagine originale, in base alle esigenze specifiche. Qui si vede l'esempio in azione.

## <a name="calling-the-computer-vision-api-to-generate-a-thumbnail"></a>Chiamata dell'API Visione artificiale per generare un'anteprima

L'operazione `generateThumbnail` crea un'immagine di anteprima con la larghezza e l'altezza specificate dall'utente. Per impostazione predefinita il servizio analizza l'immagine, identifica l'area di interesse e genera le coordinate di ritaglio intelligente sulla base dell'area di interesse. La funzionalità di ritaglio intelligente è utile quando si specificano proporzioni diverse da quelle dell'immagine di input. L'URL della richiesta ha il formato seguente:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=<...>&height=<...>&smartCropping=<...>`

È possibile fornire all'API parametri diversi per generare l'anteprima corretta in base alle esigenze. I parametri `width` e `height` sono obbligatori. Tali parametri indicano all'API le dimensioni necessarie per un'immagine specifica. Il parametro `smartCropping` genera un ritaglio più intelligente analizzando l'area di interesse nell'immagine per includerla nell'anteprima. Ad esempio, con il ritaglio intelligente abilitato, l'immagine ritagliata di un profilo mantiene il viso della persona all'interno della cornice anche quando le proporzioni dell'immagine sono diverse.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="generate-a-thumbnail"></a>Generare un'anteprima

Per questo esempio si userà l'immagine seguente, ma è possibile provare lo stesso comando con URL corrispondenti ad altre immagini. 

![Immagine di un cane bianco su un prato verde.](../media/4-dog.png)

1. Eseguire i comandi seguenti in Azure Cloud Shell. Sostituire `<region>` nel comando con l'area dell'account di Servizi cognitivi

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/dog.png'}" \
-o  thumbnail.jpg
```

In questo esempio si richiede al servizio di creare un'anteprima con dimensioni 100x100. Il ritaglio intelligente è abilitato. Una risposta corretta contiene il file binario dell'immagine di anteprima, che viene scritto in un file con nome thumbnail.jpg.

> [!CAUTION]
> Il parametro `-o` reindirizza l'output a un file. Il file viene sempre sovrascritto, pertanto per mantenere più anteprime mentre si prova questo comando, modificare ogni volta il nome del file.

## <a name="view-the-generated-thumbnail"></a>Visualizzare l'anteprima generata

L'anteprima generata sarà disponibile nell'account di archiviazione di Azure Cloud Shell. Il nome del file è **thumbnail.jpg**. 

Cloud Shell in Microsoft Learn non può scaricare file, ma è possibile seguire queste istruzioni per scaricare l'anteprima tramite il portale di Azure.

1. Eseguire i comandi seguenti in Azure Cloud Shell per verificare che il file **thumbnail.jpg** esista nella cartella principale.

    ```azurecli
    cd ~
    ls -l
    ```

    

1. Eseguire il comando seguente per spostare `thumbnail.jpg` nella cartella clouddrive.

    ```azurecli
    mv ~/thumbnail.jpg ~/clouddrive
    ```
1. Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.
1. Nel pannello **Tutte le risorse** del dashboard del portale selezionare l'account di archiviazione il cui nome inizia con `cloudshell`. 
1. Nel pannello Account di archiviazione selezionare **Storage Explorer**, quindi **CONDIVISIONI FILE** e infine la condivisione file nella raccolta il cui nome inizia con **cloudshellfiles***.
1. Selezionare il file *thunbnail.jpg* e quindi **Scarica** nel menu in alto per visualizzare l'immagine.

L'operazione `generateThumbnail` è un generatore di anteprime avanzato, in grado includere l'area di interesse di un'immagine nell'anteprima.

Per altre informazioni sull'operazione `generateThumbnails`, vedere la documentazione di riferimento relativa a [Richiesta di generazione di un'anteprima](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb).