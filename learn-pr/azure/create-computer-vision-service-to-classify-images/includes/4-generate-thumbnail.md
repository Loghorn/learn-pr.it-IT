Frontline distributori dei prodotti analizzano e caricano le immagini di archivio o sugli scaffali che sta re-immagazzinamento. Quando lead developer presso l'azienda, si creano le anteprime delle immagini. Le anteprime vengono usate nei report online che è creare per il team di vendita. Di recente, il responsabile vendite ha dichiarato che le immagini nel report sono sfocate e spesso non sono il prodotto con protagonista, rendendo difficile analizzare il report di grandi dimensioni. Spetta all'utente per migliorare la situazione.

Si decide di provare la funzionalità di generazione di anteprime di API visione artificiale. Può probabilmente è preferibile rispetto alla funzione di ridimensionamento che è stato scritto.

Visione artificiale prima genera un'anteprima di qualità elevata e quindi analizza gli oggetti all'interno dell'immagine per determinare l'area di interesse (ROI). Visione artificiale ritaglia quindi l'immagine per soddisfare i requisiti dell'area di interesse. L'anteprima generata può essere visualizzata con proporzioni diverse da quelle dell'immagine originale, in base alle esigenze specifiche. Provalo.

## <a name="calling-the-computer-vision-api-to-generate-a-thumbnail"></a>Chiamare l'API visione artificiale per generare un'immagine di anteprima

Il `generateThumbnail` operazione consente di creare un'immagine di anteprima con la larghezza specificata dall'utente e l'altezza. Per impostazione predefinita, il servizio analizza l'immagine, identifica l'area di interesse (ROI) e genera le coordinate di ritaglio intelligente basate il rendimento del capitale investito. Ritaglio intelligente è utile quando si specifica un rapporto di aspetto diverso da proporzioni dell'immagine di input. L'URL della richiesta ha il formato seguente:

`https://[location].api.cognitive.microsoft.com/vision/v2.0/generateThumbnail[?width][&height][&smartCropping]`

È possibile fornire all'API parametri diversi per generare l'anteprima corretta in base alle esigenze. Il `width` e `height` parametri sono obbligatori. L'API indicano le dimensioni che è necessario per un'immagine specifica. Il `smartCropping` parametro genera il ritaglio intelligente analizzando l'area di interesse nell'immagine per contenerlo entro l'anteprima. Ad esempio, con il ritaglio intelligente abilitata, un'immagine del profilo ritagliato consigliabile mantenere il volto all'interno della cornice anche quando l'immagine include una proporzione di aspetto diverso.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="generate-a-thumbnail"></a>Generare un'anteprima

In questo esempio useremo l'immagine seguente, ma è possibile ripetere lo stesso comando con gli URL per le altre immagini. 

![Immagine di un suggestivo cane bianco fungendo da annunci di grass verde.](../media/4-dog.png)

1. Eseguire i comandi seguenti in Azure Cloud Shell:

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/dog.png'}" \
-o  thumbnail.jpg
```

In questo esempio, chiediamo al servizio di creare un'anteprima che corrisponde a 100 x 100. Ritaglio intelligente è abilitato. Una risposta corretta contiene l'immagine di anteprima binario, che viene scritto in un file denominato thumbnail.jpg.  

> [!CAUTION]
> Il `-o` parametro reindirizza l'output in un file. Il file viene sempre sovrascritto, pertanto, se si desidera mantenere attorno al più anteprima durante il tentativo di questo comando, modificare il nome del file.

## <a name="downloading-the-thumbnail"></a>Download dell'anteprima

Anteprima generata verrà trovato nell'account di archiviazione di Azure Cloud Shell. Il file è denominato **thumbnail.jpg**. 

1. Eseguire i comandi seguenti in Azure Cloud Shell per verificare che il file **thumbnail.jg** esista nella cartella principale.

```azurecli
cd ~
ls -l
```
2. Nel menu di Cloud Shell, selezionare **caricare e scaricare file**, quindi selezionare **scaricare** nel menu a discesa.

3. Nel **scaricare un file** finestra di dialogo visualizzata, immettere **thumbnail.jpg** nel campo richiesto e selezionare **scaricare**. Avvia il download dell'immagine nel computer locale.

4. Trovare l'immagine scaricata **thumbnail.jpg** nella cartella download. Aprire l'immagine nel Visualizzatore immagini preferito per visualizzare l'anteprima che è stato creato.

Il `generateThumbnail` operazione è un generatore di anteprima potente che è in grado di mantenere l'area di interesse (ROI) di un'immagine in anteprima. 

Per altre informazioni sul `generateThumbnails` operazione, vedere la [ottenere anteprime](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) documentazione di riferimento.