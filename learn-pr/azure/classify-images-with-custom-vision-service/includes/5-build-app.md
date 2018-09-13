<span data-ttu-id="60058-101">Il vero punto di forza del Servizio visione artificiale personalizzato Microsoft è la semplicità con cui gli sviluppatori possono integrare le sue funzionalità nelle proprie applicazioni usando l'[API per le previsioni del Servizio visione artificiale personalizzato](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span><span class="sxs-lookup"><span data-stu-id="60058-101">The true power of the Microsoft Custom Vision Service is the ease with which developers can incorporate its intelligence into their own applications using the [Custom Vision Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span></span> <span data-ttu-id="60058-102">In questa unità si userà Visual Studio Code per modificare un'app denominata Artworks in modo da usare il modello creato e di cui è stato eseguito il training negli esercizi precedenti.</span><span class="sxs-lookup"><span data-stu-id="60058-102">In this unit, you will use Visual Studio Code to modify an app named Artwork to use the model you built and trained in previous exercises.</span></span>

1. <span data-ttu-id="60058-103">Se Node.js non è installato nel sistema, andare all'indirizzo https://nodejs.org e installare la versione LTS più recente per il sistema operativo in uso.</span><span class="sxs-lookup"><span data-stu-id="60058-103">If Node.js isn't installed on your system, go to https://nodejs.org and install the latest LTS version for your operating system.</span></span>

   > <span data-ttu-id="60058-104">Se non si è certi che Node.js sia già installato, aprire un prompt dei comandi o una finestra del terminale e digitare **node -v**.</span><span class="sxs-lookup"><span data-stu-id="60058-104">If you aren't sure whether Node.js is installed, open a command prompt or terminal window and type **node -v**.</span></span> <span data-ttu-id="60058-105">Se non viene visualizzato alcun numero di versione di Node.js, Node.js non è installato.</span><span class="sxs-lookup"><span data-stu-id="60058-105">If you don't see a Node.js version number, then Node.js isn't installed.</span></span> <span data-ttu-id="60058-106">Se è installata una versione precedente a Node.js 6.0, è consigliabile scaricare e installare la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="60058-106">If a version of Node.js older than 6.0 is installed, we highly recommend that you download and install the latest version.</span></span>

1. <span data-ttu-id="60058-107">Se Visual Studio Code non è installato nella workstation, andare all'indirizzo http://code.visualstudio.com e installarlo.</span><span class="sxs-lookup"><span data-stu-id="60058-107">If Visual Studio Code isn't installed on your workstation, go to http://code.visualstudio.com and install it now.</span></span>

1. <span data-ttu-id="60058-108">Avviare Visual Studio Code e scegliere **Apri cartella** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="60058-108">Start Visual Studio Code and select **Open Folder...** from the **File** menu.</span></span> <span data-ttu-id="60058-109">Nella finestra di dialogo successiva selezionare la cartella "Client\Artworks" inclusa nelle risorse del modulo.</span><span class="sxs-lookup"><span data-stu-id="60058-109">In the ensuing dialog, select the "Client\Artworks" folder included in the module resources.</span></span>

    ![Selezione della cartella Artworks](../media/5-fe-select-folder.png)

1. <span data-ttu-id="60058-111">Usare il comando **Visualizza** > **Terminale integrato** per aprire una finestra del terminale integrato in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="60058-111">Use the **View** > **Integrated Terminal** command to open an integrated terminal window in Visual Studio Code.</span></span> <span data-ttu-id="60058-112">Eseguire quindi il comando seguente nel terminale integrato per caricare i pacchetti necessari per l'app:</span><span class="sxs-lookup"><span data-stu-id="60058-112">Then execute the following command in the integrated terminal to load the packages required by the app:</span></span>

    ```
    npm install
    ```

1. <span data-ttu-id="60058-113">Tornare al progetto Artworks nel portale del Servizio visione artificiale personalizzato, fare clic su **Prestazioni** e quindi su **Predefinito** per assicurarsi che l'iterazione più recente del modello sia quella predefinita.</span><span class="sxs-lookup"><span data-stu-id="60058-113">Return to the Artwork project in the Custom Vision Service portal, click **Performance**, and then click **Make default** to make sure the latest iteration of the model is the default iteration.</span></span>

    ![Impostazione dell'iterazione predefinita](../media/5-portal-make-default.png)

1. <span data-ttu-id="60058-115">Prima di poter eseguire l'app e usarla per chiamare il Servizio visione artificiale personalizzato, l'app deve essere modificata in modo da includere informazioni su endpoint e autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="60058-115">Before you can run the app and use it to call the Custom Vision Service, it must be modified to include endpoint and authorization information.</span></span> <span data-ttu-id="60058-116">A questo scopo, fare clic su **Prediction URL** (URL stime).</span><span class="sxs-lookup"><span data-stu-id="60058-116">To that end, click **Prediction URL**.</span></span>

    ![Visualizzazione delle informazioni sull'URL dell'API per le previsioni](../media/5-portal-prediction-url.png)

1. <span data-ttu-id="60058-118">Nella finestra di dialogo successiva sono indicati due URL, uno per il caricamento di immagini tramite URL e un altro per il caricamento di immagini locali.</span><span class="sxs-lookup"><span data-stu-id="60058-118">The ensuing dialog lists two URLs: one for uploading images via URL, and another for uploading local images.</span></span> <span data-ttu-id="60058-119">Copiare l'URL dell'API per le stime per i file di immagine negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="60058-119">Copy the Prediction API URL for image files to the clipboard.</span></span>

    ![Copia dell'URL dell'API per le previsioni](../media/5-copy-prediction-url.png)

1. <span data-ttu-id="60058-121">Tornare a Visual Studio Code e fare clic su **predict.js** per aprirlo nell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="60058-121">Return to Visual Studio Code and click **predict.js** to open it in the code editor.</span></span>

    ![Apertura di predict.js](../media/5-vs-predict-file.png)

1. <span data-ttu-id="60058-123">Sostituire "PREDICTION_ENDPOINT" nella riga 3 con l'URL copiato negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="60058-123">Replace "PREDICTION_ENDPOINT" in line 3 with the URL on the clipboard.</span></span>

    ![Aggiunta dell'URL dell'API per le previsioni](../media/5-vs-prediction-endpoint.png)

1. <span data-ttu-id="60058-125">Tornare al portale del Servizio visione artificiale personalizzato e copiare la chiave API per le previsioni negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="60058-125">Return to the Custom Vision Service portal and copy the Prediction API key to the clipboard.</span></span>

    ![Copia della chiave API per le previsioni](../media/5-copy-prediction-key.png)

1. <span data-ttu-id="60058-127">Tornare a Visual Studio Code e sostituire "PREDICTION_KEY" nella riga 4 di **predict.js** con la chiave API copiata negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="60058-127">Return to Visual Studio Code and replace "PREDICTION_KEY" in line 4 of **predict.js** with the API key on the clipboard.</span></span>

    ![Aggiunta della chiave API per le previsioni](../media/5-vs-prediction-key.png)

1. <span data-ttu-id="60058-129">Scorrere verso il basso in **predict.js** ed esaminare il blocco di codice che inizia alla riga 34.</span><span class="sxs-lookup"><span data-stu-id="60058-129">Scroll down in **predict.js** and examine the block of code that begins on line 34.</span></span> <span data-ttu-id="60058-130">Si tratta del codice che effettua una chiamata in uscita al Servizio visione artificiale personalizzato tramite AJAX.</span><span class="sxs-lookup"><span data-stu-id="60058-130">This is the code that calls out to the Custom Vision Service using AJAX.</span></span> <span data-ttu-id="60058-131">L'uso dell'API per le stime del Servizio visione artificiale personalizzato è tanto facile quanto l'esecuzione di una semplice richiesta POST autenticata a un endpoint REST.</span><span class="sxs-lookup"><span data-stu-id="60058-131">Using the Custom Vision Prediction API is as easy as making a simple, authenticated POST to a REST endpoint.</span></span>

    ![Esecuzione di una chiamata all'API per le previsioni](../media/5-vs-code-block.png)

1. <span data-ttu-id="60058-133">Tornare al terminale integrato in Visual Studio Code ed eseguire il comando seguente per avviare l'app:</span><span class="sxs-lookup"><span data-stu-id="60058-133">Return to the integrated terminal in Visual Studio Code and execute the following command to start the app:</span></span>

    ```
    npm start
    ```

1. <span data-ttu-id="60058-134">Verificare che l'app Artworks venga avviata e visualizzi una finestra come questa:</span><span class="sxs-lookup"><span data-stu-id="60058-134">Confirm that the Artworks app starts and displays a window like this one:</span></span>

    ![App Artworks](../media/5-app-startup.png)

<span data-ttu-id="60058-136">Artworks è un'app multipiattaforma scritta con Node.js ed [Electron](https://electron.atom.io/).</span><span class="sxs-lookup"><span data-stu-id="60058-136">Artworks is a cross-platform app written with Node.js and [Electron](https://electron.atom.io/).</span></span> <span data-ttu-id="60058-137">In quanto tale, può essere eseguita ugualmente in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="60058-137">As such, it is equally capable of running on Windows, macOS, and Linux.</span></span> <span data-ttu-id="60058-138">Nel prossimo esercizio si userà questa app per classificare immagini in base agli artisti che le hanno dipinte.</span><span class="sxs-lookup"><span data-stu-id="60058-138">In the next exercise, you will use it to classify images by the artists who painted them.</span></span>