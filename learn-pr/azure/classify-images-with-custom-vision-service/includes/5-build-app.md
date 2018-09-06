### <a name="exercise-5-create-a-nodejs-app-that-uses-the-model"></a>Esercizio 5: Creare un'app Node.js che usa il modello

Il vero punto di forza del Servizio visione artificiale personalizzato Microsoft è la semplicità con cui gli sviluppatori possono integrare le sue funzionalità nelle proprie applicazioni usando l'[API per le stime del Servizio visione artificiale personalizzato](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814). In questo esercizio si userà Visual Studio Code per modificare un'app denominata Artworks in modo da usare il modello creato e di cui è stato eseguito il training negli esercizi precedenti.

1. Se Node.js non è installato nel sistema, andare all'indirizzo https://nodejs.org e installare la versione LTS più recente per il sistema operativo in uso.

    > Se non si è certi che Node.js sia già installato, aprire un prompt dei comandi o una finestra del terminale e digitare **node -v**. Se non viene visualizzato alcun numero di versione di Node.js, Node.js non è installato. Se è installata una versione precedente a Node.js 6.0, è consigliabile scaricare e installare la versione più recente.

1. Se Visual Studio Code non è installato nella workstation, andare all'indirizzo http://code.visualstudio.com e installarlo.

1. Avviare Visual Studio Code e scegliere **Apri cartella** dal menu **File**. Nella finestra di dialogo successiva selezionare la cartella "Client\Artworks" inclusa nelle risorse del lab.

    ![Selezione della cartella Artworks](../images/fe-select-folder.png)

    _Selezione della cartella Artworks_ 

1. Usare il comando **Visualizza** > **Terminale integrato** per aprire una finestra del terminale integrato in Visual Studio Code. Eseguire quindi il comando seguente nel terminale integrato per caricare i pacchetti necessari per l'app:

    ```
    npm install
    ```

1. Tornare al progetto Artworks nel portale del Servizio visione artificiale personalizzato, fare clic su **Prestazioni** e quindi su **Predefinito** per assicurarsi che l'iterazione più recente del modello sia quella predefinita. 

    ![Impostazione dell'iterazione predefinita](../images/portal-make-default.png)

    _Impostazione dell'iterazione predefinita_ 

1. Prima di poter eseguire l'app e usarla per chiamare il Servizio visione artificiale personalizzato, l'app deve essere modificata in modo da includere informazioni su endpoint e autorizzazioni. A questo scopo, fare clic su **Prediction URL** (URL stime).

    ![Visualizzazione delle informazioni sull'URL dell'API per le stime](../images/portal-prediction-url.png)

    _Visualizzazione delle informazioni sull'URL dell'API per le stime_ 

1. Nella finestra di dialogo successiva sono indicati due URL, uno per il caricamento di immagini tramite URL e un altro per il caricamento di immagini locali. Copiare l'URL dell'API per le stime per i file di immagine negli Appunti. 

    ![Copia dell'URL dell'API per le stime](../images/copy-prediction-url.png)

    _Copia dell'URL dell'API per le stime_ 

1. Tornare a Visual Studio Code e fare clic su **predict.js** per aprirlo nell'editor di codice.

    ![Apertura di predict.js](../images/vs-predict-file.png)

    _Apertura di predict.js_ 

1. Sostituire "PREDICTION_ENDPOINT" nella riga 3 con l'URL copiato negli Appunti.

    ![Aggiunta dell'URL dell'API per le stime](../images/vs-prediction-endpoint.png)

    _Aggiunta dell'URL dell'API per le stime_ 

1. Tornare al portale del Servizio visione artificiale personalizzato e copiare la chiave API per le stime negli Appunti. 

    ![Copia della chiave API per le stime](../images/copy-prediction-key.png)

    _Copia della chiave API per le stime_ 

1. Tornare a Visual Studio Code e sostituire "PREDICTION_KEY" nella riga 4 di **predict.js** con la chiave API copiata negli Appunti.

    ![Aggiunta della chiave API per le stime](../images/vs-prediction-key.png)

    _Aggiunta della chiave API per le stime_ 

1. Scorrere verso il basso in **predict.js** ed esaminare il blocco di codice che inizia alla riga 34. Si tratta del codice che effettua una chiamata in uscita al Servizio visione artificiale personalizzato tramite AJAX. L'uso dell'API per le stime del Servizio visione artificiale personalizzato è tanto facile quanto l'esecuzione di una semplice richiesta POST autenticata a un endpoint REST.

    ![Esecuzione di una chiamata all'API per le stime](../images/vs-code-block.png)

    _Esecuzione di una chiamata all'API per le stime_ 

1. Tornare al terminale integrato in Visual Studio Code ed eseguire il comando seguente per avviare l'app:

    ```
    npm start
    ```

1. Verificare che l'app Artworks venga avviata e visualizzi una finestra come questa:

    ![App Artworks](../images/app-startup.png)

    _App Artworks_ 

Artworks è un'app multipiattaforma scritta con Node.js ed [Electron](https://electron.atom.io/). In quanto tale, può essere eseguita ugualmente in Windows, macOS e Linux. Nel prossimo esercizio si userà questa app per classificare immagini in base agli artisti che le hanno dipinte.