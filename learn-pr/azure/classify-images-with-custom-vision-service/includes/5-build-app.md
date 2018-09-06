### <a name="exercise-5-create-a-nodejs-app-that-uses-the-model"></a><span data-ttu-id="b0d41-101">Esercizio 5: Creare un'app Node.js che usa il modello</span><span class="sxs-lookup"><span data-stu-id="b0d41-101">Exercise 5: Create a Node.js app that uses the model</span></span>

<span data-ttu-id="b0d41-102">Il vero punto di forza del Servizio visione artificiale personalizzato Microsoft è la semplicità con cui gli sviluppatori possono integrare le sue funzionalità nelle proprie applicazioni usando l'[API per le stime del Servizio visione artificiale personalizzato](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span><span class="sxs-lookup"><span data-stu-id="b0d41-102">The true power of the Microsoft Custom Vision Service is the ease with which developers can incorporate its intelligence into their own applications using the [Custom Vision Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span></span> <span data-ttu-id="b0d41-103">In questo esercizio si userà Visual Studio Code per modificare un'app denominata Artworks in modo da usare il modello creato e di cui è stato eseguito il training negli esercizi precedenti.</span><span class="sxs-lookup"><span data-stu-id="b0d41-103">In this exercise, you will use Visual Studio Code to modify an app named Artwork to use the model you built and trained in previous exercises.</span></span>

1. <span data-ttu-id="b0d41-104">Se Node.js non è installato nel sistema, andare all'indirizzo https://nodejs.org e installare la versione LTS più recente per il sistema operativo in uso.</span><span class="sxs-lookup"><span data-stu-id="b0d41-104">If Node.js isn't installed on your system, go to https://nodejs.org and install the latest LTS version for your operating system.</span></span>

    > <span data-ttu-id="b0d41-105">Se non si è certi che Node.js sia già installato, aprire un prompt dei comandi o una finestra del terminale e digitare **node -v**.</span><span class="sxs-lookup"><span data-stu-id="b0d41-105">If you aren't sure whether Node.js is installed, open a Command Prompt or terminal window and type **node -v**.</span></span> <span data-ttu-id="b0d41-106">Se non viene visualizzato alcun numero di versione di Node.js, Node.js non è installato.</span><span class="sxs-lookup"><span data-stu-id="b0d41-106">If you don't see a Node.js version number, then Node.js isn't installed.</span></span> <span data-ttu-id="b0d41-107">Se è installata una versione precedente a Node.js 6.0, è consigliabile scaricare e installare la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="b0d41-107">If a version of Node.js older than 6.0 is installed, it is highly recommend that you download and install the latest version.</span></span>

1. <span data-ttu-id="b0d41-108">Se Visual Studio Code non è installato nella workstation, andare all'indirizzo http://code.visualstudio.com e installarlo.</span><span class="sxs-lookup"><span data-stu-id="b0d41-108">If Visual Studio Code isn't installed on your workstation, go to http://code.visualstudio.com and install it now.</span></span>

1. <span data-ttu-id="b0d41-109">Avviare Visual Studio Code e scegliere **Apri cartella** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="b0d41-109">Start Visual Studio Code and select **Open Folder...** from the **File** menu.</span></span> <span data-ttu-id="b0d41-110">Nella finestra di dialogo successiva selezionare la cartella "Client\Artworks" inclusa nelle risorse del lab.</span><span class="sxs-lookup"><span data-stu-id="b0d41-110">In the ensuing dialog, select the "Client\Artworks" folder included in the lab resources.</span></span>

    ![Selezione della cartella Artworks](../images/fe-select-folder.png)

    <span data-ttu-id="b0d41-112">_Selezione della cartella Artworks_</span><span class="sxs-lookup"><span data-stu-id="b0d41-112">_Selecting the Artworks folder_</span></span> 

1. <span data-ttu-id="b0d41-113">Usare il comando **Visualizza** > **Terminale integrato** per aprire una finestra del terminale integrato in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b0d41-113">Use the **View** > **Integrated Terminal** command to open an integrated terminal window in Visual Studio Code.</span></span> <span data-ttu-id="b0d41-114">Eseguire quindi il comando seguente nel terminale integrato per caricare i pacchetti necessari per l'app:</span><span class="sxs-lookup"><span data-stu-id="b0d41-114">Then execute the following command in the integrated terminal to load the packages required by the app:</span></span>

    ```
    npm install
    ```

1. <span data-ttu-id="b0d41-115">Tornare al progetto Artworks nel portale del Servizio visione artificiale personalizzato, fare clic su **Prestazioni** e quindi su **Predefinito** per assicurarsi che l'iterazione più recente del modello sia quella predefinita.</span><span class="sxs-lookup"><span data-stu-id="b0d41-115">Return to the Artwork project in the Custom Vision Service portal, click **Performance**, and then click **Make default** to make sure the latest iteration of the model is the default iteration.</span></span> 

    ![Impostazione dell'iterazione predefinita](../images/portal-make-default.png)

    <span data-ttu-id="b0d41-117">_Impostazione dell'iterazione predefinita_</span><span class="sxs-lookup"><span data-stu-id="b0d41-117">_Specifying the default iteration_</span></span> 

1. <span data-ttu-id="b0d41-118">Prima di poter eseguire l'app e usarla per chiamare il Servizio visione artificiale personalizzato, l'app deve essere modificata in modo da includere informazioni su endpoint e autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="b0d41-118">Before you can run the app and use it to call the Custom Vision Service, it must be modified to include endpoint and authorization information.</span></span> <span data-ttu-id="b0d41-119">A questo scopo, fare clic su **Prediction URL** (URL stime).</span><span class="sxs-lookup"><span data-stu-id="b0d41-119">To that end, click **Prediction URL**.</span></span>

    ![Visualizzazione delle informazioni sull'URL dell'API per le stime](../images/portal-prediction-url.png)

    <span data-ttu-id="b0d41-121">_Visualizzazione delle informazioni sull'URL dell'API per le stime_</span><span class="sxs-lookup"><span data-stu-id="b0d41-121">_Viewing Prediction URL information_</span></span> 

1. <span data-ttu-id="b0d41-122">Nella finestra di dialogo successiva sono indicati due URL, uno per il caricamento di immagini tramite URL e un altro per il caricamento di immagini locali.</span><span class="sxs-lookup"><span data-stu-id="b0d41-122">The ensuing dialog lists two URLs: one for uploading images via URL, and another for uploading local images.</span></span> <span data-ttu-id="b0d41-123">Copiare l'URL dell'API per le stime per i file di immagine negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="b0d41-123">Copy the Prediction API URL for image files to the clipboard.</span></span> 

    ![Copia dell'URL dell'API per le stime](../images/copy-prediction-url.png)

    <span data-ttu-id="b0d41-125">_Copia dell'URL dell'API per le stime_</span><span class="sxs-lookup"><span data-stu-id="b0d41-125">_Copying the Prediction API URL_</span></span> 

1. <span data-ttu-id="b0d41-126">Tornare a Visual Studio Code e fare clic su **predict.js** per aprirlo nell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="b0d41-126">Return to Visual Studio Code and click **predict.js** to open it in the code editor.</span></span>

    ![Apertura di predict.js](../images/vs-predict-file.png)

    <span data-ttu-id="b0d41-128">_Apertura di predict.js_</span><span class="sxs-lookup"><span data-stu-id="b0d41-128">_Opening predict.js_</span></span> 

1. <span data-ttu-id="b0d41-129">Sostituire "PREDICTION_ENDPOINT" nella riga 3 con l'URL copiato negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="b0d41-129">Replace "PREDICTION_ENDPOINT" in line 3 with the URL on the clipboard.</span></span>

    ![Aggiunta dell'URL dell'API per le stime](../images/vs-prediction-endpoint.png)

    <span data-ttu-id="b0d41-131">_Aggiunta dell'URL dell'API per le stime_</span><span class="sxs-lookup"><span data-stu-id="b0d41-131">_Adding the Prediction API URL_</span></span> 

1. <span data-ttu-id="b0d41-132">Tornare al portale del Servizio visione artificiale personalizzato e copiare la chiave API per le stime negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="b0d41-132">Return to the Custom Vision Service portal and copy the Prediction API key to the clipboard.</span></span> 

    ![Copia della chiave API per le stime](../images/copy-prediction-key.png)

    <span data-ttu-id="b0d41-134">_Copia della chiave API per le stime_</span><span class="sxs-lookup"><span data-stu-id="b0d41-134">_Copying the Prediction API key_</span></span> 

1. <span data-ttu-id="b0d41-135">Tornare a Visual Studio Code e sostituire "PREDICTION_KEY" nella riga 4 di **predict.js** con la chiave API copiata negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="b0d41-135">Return to Visual Studio Code and replace "PREDICTION_KEY" in line 4 of **predict.js** with the API key on the clipboard.</span></span>

    ![Aggiunta della chiave API per le stime](../images/vs-prediction-key.png)

    <span data-ttu-id="b0d41-137">_Aggiunta della chiave API per le stime_</span><span class="sxs-lookup"><span data-stu-id="b0d41-137">_Adding the Prediction API key_</span></span> 

1. <span data-ttu-id="b0d41-138">Scorrere verso il basso in **predict.js** ed esaminare il blocco di codice che inizia alla riga 34.</span><span class="sxs-lookup"><span data-stu-id="b0d41-138">Scroll down in **predict.js** and examine the block of code that begins on line 34.</span></span> <span data-ttu-id="b0d41-139">Si tratta del codice che effettua una chiamata in uscita al Servizio visione artificiale personalizzato tramite AJAX.</span><span class="sxs-lookup"><span data-stu-id="b0d41-139">This is the code that calls out to the Custom Vision Service using AJAX.</span></span> <span data-ttu-id="b0d41-140">L'uso dell'API per le stime del Servizio visione artificiale personalizzato è tanto facile quanto l'esecuzione di una semplice richiesta POST autenticata a un endpoint REST.</span><span class="sxs-lookup"><span data-stu-id="b0d41-140">Using the Custom Vision Prediction API is as easy as making a simple, authenticated POST to a REST endpoint.</span></span>

    ![Esecuzione di una chiamata all'API per le stime](../images/vs-code-block.png)

    <span data-ttu-id="b0d41-142">_Esecuzione di una chiamata all'API per le stime_</span><span class="sxs-lookup"><span data-stu-id="b0d41-142">_Making a call to the Prediction API_</span></span> 

1. <span data-ttu-id="b0d41-143">Tornare al terminale integrato in Visual Studio Code ed eseguire il comando seguente per avviare l'app:</span><span class="sxs-lookup"><span data-stu-id="b0d41-143">Return to the integrated terminal in Visual Studio Code and execute the following command to start the app:</span></span>

    ```
    npm start
    ```

1. <span data-ttu-id="b0d41-144">Verificare che l'app Artworks venga avviata e visualizzi una finestra come questa:</span><span class="sxs-lookup"><span data-stu-id="b0d41-144">Confirm that the Artworks app starts and displays a window like this one:</span></span>

    ![App Artworks](../images/app-startup.png)

    <span data-ttu-id="b0d41-146">_App Artworks_</span><span class="sxs-lookup"><span data-stu-id="b0d41-146">_The Artworks app_</span></span> 

<span data-ttu-id="b0d41-147">Artworks è un'app multipiattaforma scritta con Node.js ed [Electron](https://electron.atom.io/).</span><span class="sxs-lookup"><span data-stu-id="b0d41-147">Artworks is a cross-platform app written with Node.js and [Electron](https://electron.atom.io/).</span></span> <span data-ttu-id="b0d41-148">In quanto tale, può essere eseguita ugualmente in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="b0d41-148">As such, it is equally capable of running on Windows, macOS, and Linux.</span></span> <span data-ttu-id="b0d41-149">Nel prossimo esercizio si userà questa app per classificare immagini in base agli artisti che le hanno dipinte.</span><span class="sxs-lookup"><span data-stu-id="b0d41-149">In the next exercise, you will use it to classify images by the artists who painted them.</span></span>