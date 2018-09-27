Nell'esercizio precedente è stato testato il modello sottoposto a training tramite la funzionalità **Quick Test** (Test rapido) del portale di Servizio visione artificiale personalizzato. Si tratta di un ottimo modo per verificare rapidamente l'accuratezza del modello con alcune immagini di test. Procedendo, è possibile eseguire chiamate all'endpoint di stima del modello su HTTP.

[!include[](../../../includes/azure-sandbox-activate.md)]

1. Tornando al progetto *Artworks** nel portale di Servizio visione artificiale personalizzato, selezionare la scheda **Performance** (Prestazioni).

    ![Selezione della scheda Performance (Prestazioni)](../media/5-performance-tab.png)

1. Selezionare **Make default** (Imposta come predefinito) per verificare che l'ultima iterazione del modello sia l'iterazione predefinita.

1. Selezionare **Prediction URL** (URL previsione). Verrà visualizzata una finestra di dialogo delle informazioni necessarie per eseguire le chiamate. 

    ![Visualizzazione delle informazioni di Prediction URL (URL previsione)](../media/5-portal-prediction-url.png)

    Come indicato nella finestra di dialogo, è possibile chiamare l'endpoint di stima e passare un URL di immagine. È anche possibile passare un'immagine non elaborata all'endpoint nel corpo della richiesta.

    Prendere nota delle tre informazioni incluse in questa finestra di dialogo.
     - **Prediction-Key** (Chiave previsione): questa chiave deve essere impostata come intestazione in tutte le richieste. È l'elemento che fornisce l'accesso all'endpoint.
    - **Request URL** (URL richiesta): la finestra di dialogo mostra due URL diversi. Se si pubblica un URL di immagine, usare il primo URL, che termina con `/url`. Se si intende pubblicare un'immagine non elaborata nel corpo della richiesta, usare il secondo URL, che termina con `/image`.
    - **Content-Type** (Tipo di contenuto): in caso di pubblicazione di un'immagine non elaborata, impostare il corpo della richiesta sulla rappresentazione binaria dell'immagine e il tipo di contenuto su `application/octet-stream`. In caso di pubblicazione di un URL di immagine, inserirlo come JSON nel corpo e impostare il tipo di contenuto su `application/json`.
    

3. Copiare e salvare il primo URL e il valore `Prediction-Key` dalla finestra di dialogo **How to use the Prediction API** (Come usare l'API Previsioni). 

> [!TIP]
> **cURL** è uno strumento da riga di comando che può essere usato per inviare o ricevere file. È incluso in Linux, macOS e Windows 10 e può essere scaricato per la maggior parte degli altri sistemi operativi. cURL supporta vari protocolli come HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3 e così via. Per altre informazioni, consultare i collegamenti seguenti:
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/> 
> 
> cURL è già installato in Azure Cloud Shell nel sandbox. cURL verrà usato in questo esercizio per eseguire chiamate HTTP all'endpoint.

2. Eseguire il comando seguente in Cloud Shell. Sostituire **[endpoint-URL]** con l'URL salvato nell'ultimo passaggio. Sostituire **[Prediction-Key]** con il valore di `Prediction-Key` salvato nell'ultimo passaggio. 

    ```azurecli
    curl [endpoint-URL] \
    -H "Prediction-Key: [Prediction-Key]" \
    -H "Content-Type: application/json" \
    -d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/VanGoghTest_02.jpg'}" \
    | jq '.'
    ```

    Al termine del comando, si noterà una risposta JSON simile allo screenshot seguente. L'API restituisce la probabilità per ogni tag nel modello. Come si può notare, con una probabilità vicina a 1 per il valore `"painting"` **tagName**, questa immagine è certamente un disegno. Tuttavia, non è un disegno di uno qualsiasi degli artisti con cui è stato eseguito il training del modello. 

    ![Anteprima di un'immagine di Picasso di prova](../media/5-prediction-json.png) 

3. Provare altre stime sostituendo l'URL nel corpo della richiesta precedente con uno degli URL nella tabella seguente. 

    |Immagine  | URL  |
    |---------|---------|
    |![Anteprima di un'immagine di Picasso di prova](../media/picasso-test-02-thumb.jpg)     | `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PicassoTest_02.jpg`        |
    |![Anteprima di un'immagine di Rembrandt di prova](../media/rembrandt-test-01-thumb.jpg)     |  `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/RembrandtTest_01.jpg`       |
    |![Anteprima di un'immagine di Pollock di prova](../media/pollock-test-01-thumb.jpg)  |   `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PollockTest_01.jpg`     |
   

L'endpoint di stima funziona come previsto. Chiamare l'API è semplice come eseguire una richiesta HTTP all'endpoint con un valore Prediction-Key e un URL di immagine.