<span data-ttu-id="43773-101">In questo unità si connetterà il bot alla knowledge base di QnA Maker creata in precedenza, in modo che possa svolgere una conversazione intelligente.</span><span class="sxs-lookup"><span data-stu-id="43773-101">In this unit, you will connect your bot to the QnA Maker knowledge base you built earlier so the bot can carry on an intelligent conversation.</span></span> <span data-ttu-id="43773-102">Per la connessione alla knowledge base occorre recuperare alcune informazioni dal portale di QnA Maker, copiarle nel portale di Azure, aggiornare il codice del bot e quindi distribuire nuovamente il bot in Azure.</span><span class="sxs-lookup"><span data-stu-id="43773-102">Connecting to the knowledge base involves retrieving some information from the QnA Maker portal, copying it into the Azure portal, updating the bot code, and then redeploying the bot to Azure.</span></span>

1. <span data-ttu-id="43773-103">Tornare al [portale di QnA Maker](https://www.qnamaker.ai/) e fare clic sul proprio nome nell'angolo superiore destro.</span><span class="sxs-lookup"><span data-stu-id="43773-103">Return to the [QnA Maker portal](https://www.qnamaker.ai/) and click your name in the upper-right corner.</span></span> <span data-ttu-id="43773-104">Selezionare **Manage endpoint keys** (Gestisci chiavi endpoint) dal menu a discesa.</span><span class="sxs-lookup"><span data-stu-id="43773-104">Select **Manage endpoint keys** from the menu that drops down.</span></span> <span data-ttu-id="43773-105">Fare clic su **Show** (Mostra) per visualizzare la chiave endpoint primaria e su **Copy** (Copia) per copiarla negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="43773-105">Click **Show** to show the primary endpoint key, and **Copy** to copy it to the clipboard.</span></span> <span data-ttu-id="43773-106">Incollarla quindi in un file di testo in modo da recuperarla facilmente più avanti.</span><span class="sxs-lookup"><span data-stu-id="43773-106">Then, paste it into a text file so you can easily retrieve it in a moment.</span></span>

    ![Copia della chiave endpoint](../media-draft/6-copy-primary-key.png)

1. <span data-ttu-id="43773-108">Fare clic su **My knowledge bases** (Knowledge base personali) nel menu nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="43773-108">Click **My knowledge bases** in the menu at the top of the page.</span></span> <span data-ttu-id="43773-109">Fare quindi clic su **View Code** (Visualizza codice) per la knowledge base creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="43773-109">Then, click **View Code** for the knowledge base that you created earlier.</span></span>

    ![Apertura della knowledge base](../media-draft/6-open-knowledge-base.png)

1. <span data-ttu-id="43773-111">Copiare l'ID della knowledge base dalla prima riga e il nome host dalla seconda riga.</span><span class="sxs-lookup"><span data-stu-id="43773-111">Copy the knowledge base ID from the first line and the host name from the second line.</span></span> <span data-ttu-id="43773-112">Incollarli anche in un file di testo.</span><span class="sxs-lookup"><span data-stu-id="43773-112">Paste them into a text file, as well.</span></span> <span data-ttu-id="43773-113">Chiudere quindi la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="43773-113">Then, close the dialog.</span></span> <span data-ttu-id="43773-114">**Non** includere il prefisso "https://" nel nome host copiato.</span><span class="sxs-lookup"><span data-stu-id="43773-114">**Do not** include the "https://" prefix in the host name that you copy.</span></span>

    ![Copia di nome host e ID della knowledge base](../media-draft/6-copy-endpoint-info.png)  

1. <span data-ttu-id="43773-116">Tornare al bot app Web nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="43773-116">Return to the web app bot in the Azure portal.</span></span> <span data-ttu-id="43773-117">Fare clic su **Impostazioni applicazione** nel menu a sinistra e scorrere verso il basso fino a individuare le impostazioni denominate "QnAKnowledgebaseId", "QnAAuthKey" e "QnAEndpointHostName."</span><span class="sxs-lookup"><span data-stu-id="43773-117">Click **Application settings** in the menu on the left and scroll down until you find application settings named "QnAKnowledgebaseId," "QnAAuthKey," and "QnAEndpointHostName."</span></span> <span data-ttu-id="43773-118">Incollare in questi campi l'ID della knowledge base e il nome host ottenuti nel passaggio 3 e la chiave endpoint ottenuta nel passaggio 1.</span><span class="sxs-lookup"><span data-stu-id="43773-118">Paste the knowledge base ID and host name obtained in Step 3 and the endpoint key obtained in Step 1 into these fields.</span></span> <span data-ttu-id="43773-119">Fare quindi clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="43773-119">Then, click **Save**.</span></span>

    ![Modifica delle impostazioni applicazione](../media-draft/6-enter-app-settings.png)

1. <span data-ttu-id="43773-121">Tornare a Visual Studio Code e sostituire il contenuto di **app.js** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="43773-121">Return to Visual Studio Code and replace the contents of **app.js** with the code below.</span></span> <span data-ttu-id="43773-122">Salvare quindi il file.</span><span class="sxs-lookup"><span data-stu-id="43773-122">Then, save the file.</span></span>

    ```JavaScript
    var restify = require('restify');
    var builder = require('botbuilder');
    var botbuilder_azure = require("botbuilder-azure");
    var builder_cognitiveservices = require("botbuilder-cognitiveservices");
    
    // Setup Restify Server
    var server = restify.createServer();
    server.listen(process.env.port || process.env.PORT || 3978, function () {
       console.log('%s listening to %s', server.name, server.url); 
    });
      
    // Create chat connector for communicating with the Bot Framework Service
    var connector = new builder.ChatConnector({
        appId: process.env.MicrosoftAppId,
        appPassword: process.env.MicrosoftAppPassword     
    });
    
    // Listen for messages from users 
    server.post('/api/messages', connector.listen());
     
    // Create your bot with a function to receive messages from the user
    var bot = new builder.UniversalBot(connector);
    
    var recognizer = new builder_cognitiveservices.QnAMakerRecognizer({
        knowledgeBaseId: process.env.QnAKnowledgebaseId, 
        authKey: process.env.QnAAuthKey,
        endpointHostName: process.env.QnAEndpointHostName
    });
    
    var basicQnAMakerDialog = new builder_cognitiveservices.QnAMakerDialog({
        recognizers: [recognizer],
        defaultMessage: "I'm not quite sure what you're asking. Please ask your question again.",
        qnaThreshold: 0.3
    });
    
    bot.dialog('basicQnAMakerDialog', basicQnAMakerDialog);
    
    bot.dialog('/',
    [
        function (session) {
            session.replaceDialog('basicQnAMakerDialog');
        }
    ]);
    ```

    <span data-ttu-id="43773-123">Notare la chiamata per creare un'istanza di `QnAMakerDialog` nella riga 30.</span><span class="sxs-lookup"><span data-stu-id="43773-123">Note the call to create a `QnAMakerDialog` instance on line 30.</span></span> <span data-ttu-id="43773-124">Questa crea una finestra di dialogo che integra un bot compilato con il servizio Azure Bot con una knowledge base creata con Microsoft QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="43773-124">This creates a dialog that integrates a bot built with the Azure Bot Service with a knowledge base built Microsoft QnA Maker.</span></span>
 
1. <span data-ttu-id="43773-125">Fare clic sul pulsante **Controllo del codice sorgente** sulla barra delle attività di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="43773-125">Click the **Source Control** button in the activity bar in Visual Studio Code.</span></span> <span data-ttu-id="43773-126">Digitare "Connected to knowledge base" nella finestra di messaggio e fare clic sul segno di spunta per eseguire il commit delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="43773-126">Type "Connected to knowledge base" into the message box, and click the check mark to commit your changes.</span></span> <span data-ttu-id="43773-127">Fare quindi clic sui puntini di sospensione e usare il comando **Pubblica ramo** per eseguire il push di queste modifiche nel repository remoto (e dunque</span><span class="sxs-lookup"><span data-stu-id="43773-127">Then, click the ellipsis and use the **Publish Branch** command to push these changes to the remote repository (and therefore.</span></span> <span data-ttu-id="43773-128">nell'app Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="43773-128">to the Azure Web App).</span></span>

1. <span data-ttu-id="43773-129">Tornare al bot app Web nel portale di Azure e fare clic su **Test in Web Chat** (Testa nella chat Web) a sinistra per aprire la console di test.</span><span class="sxs-lookup"><span data-stu-id="43773-129">Return to the web app bot in the Azure portal and click **Test in Web Chat** on the left to open the test console.</span></span> <span data-ttu-id="43773-130">Digitare "What's the most popular software programming language in the world?"</span><span class="sxs-lookup"><span data-stu-id="43773-130">Type "What's the most popular software programming language in the world?"</span></span> <span data-ttu-id="43773-131">nella casella nella parte inferiore della finestra della chat e premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="43773-131">into the box at the bottom of the chat window and press **Enter**.</span></span> <span data-ttu-id="43773-132">Verificare che il bot risponda come segue:</span><span class="sxs-lookup"><span data-stu-id="43773-132">Confirm that the bot responds as follows:</span></span>

    ![Test del bot](../media-draft/6-portal-testing-chat.png)

<span data-ttu-id="43773-134">Ora che il bot è connesso alla knowledge base, il passaggio finale consiste nel testarlo in condizioni normali.</span><span class="sxs-lookup"><span data-stu-id="43773-134">Now that the bot is connected to the knowledge base, the final step is to test it in the wild.</span></span> <span data-ttu-id="43773-135">Ad esempio, è possibile testarlo con Skype.</span><span class="sxs-lookup"><span data-stu-id="43773-135">And what could be wilder than testing it with Skype?</span></span>