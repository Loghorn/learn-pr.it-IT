### <a name="exercise-5-connect-the-bot-to-the-knowledge-base"></a><span data-ttu-id="9d980-101">Esercizio 5: Connettere il bot alla knowledge base</span><span class="sxs-lookup"><span data-stu-id="9d980-101">Exercise 5: Connect the bot to the knowledge base</span></span>

<span data-ttu-id="9d980-102">In questo esercizio si connetterà il bot alla knowledge base di QnA Maker creata in precedenza, in modo che possa svolgere una conversazione intelligente.</span><span class="sxs-lookup"><span data-stu-id="9d980-102">In this exercise, you will connect your bot to the QnA Maker knowledge base you built earlier so the bot can carry on an intelligent conversation.</span></span> <span data-ttu-id="9d980-103">Per la connessione alla knowledge base occorre recuperare alcune informazioni dal portale di QnA Maker, copiarle nel portale di Azure, aggiornare il codice del bot e quindi distribuire nuovamente il bot in Azure.</span><span class="sxs-lookup"><span data-stu-id="9d980-103">Connecting to the knowledge base involves retrieving some information from the QnA Maker portal, copying it into the Azure portal, updating the bot code, and then redeploying the bot to Azure.</span></span>

1. <span data-ttu-id="9d980-104">Tornare al [portale di QnA Maker](https://www.qnamaker.ai/) e fare clic sul proprio nome nell'angolo superiore destro.</span><span class="sxs-lookup"><span data-stu-id="9d980-104">Return to the [QnA Maker portal](https://www.qnamaker.ai/) and click your name in the upper-right corner.</span></span> <span data-ttu-id="9d980-105">Selezionare **Manage endpoint keys** (Gestisci chiavi endpoint) dal menu a discesa.</span><span class="sxs-lookup"><span data-stu-id="9d980-105">Select **Manage endpoint keys** from the menu that drops down.</span></span> <span data-ttu-id="9d980-106">Fare clic su **Show** (Mostra) per visualizzare la chiave endpoint primaria e su **Copy** (Copia) per copiarla negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="9d980-106">Click **Show** to show the primary endpoint key, and **Copy** to copy it to the clipboard.</span></span> <span data-ttu-id="9d980-107">Incollarla quindi in un file di testo in modo da recuperarla facilmente più avanti.</span><span class="sxs-lookup"><span data-stu-id="9d980-107">Then paste it into a text file so you can easily retrieve it in a moment.</span></span>

    ![Copia della chiave endpoint](../images/copy-primary-key.png)
    
    <span data-ttu-id="9d980-109">_Copia della chiave endpoint_</span><span class="sxs-lookup"><span data-stu-id="9d980-109">_Copying the endpoint key_</span></span> 

1. <span data-ttu-id="9d980-110">Fare clic su **My knowledge bases** (Knowledge base personali) nel menu nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="9d980-110">Click **My knowledge bases** in the menu at the top of the page.</span></span> <span data-ttu-id="9d980-111">Quindi fare clic su **View Code** (Visualizza codice) per la knowledge base creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9d980-111">Then click **View Code** for the knowledge base that you created earlier.</span></span>

    ![Apertura della knowledge base](../images/open-knowledge-base.png)

    <span data-ttu-id="9d980-113">_Apertura della knowledge base_</span><span class="sxs-lookup"><span data-stu-id="9d980-113">_Opening the knowledge base_</span></span>

1. <span data-ttu-id="9d980-114">Copiare l'ID della knowledge base dalla prima riga e il nome host dalla seconda riga e incollarli in un file di testo.</span><span class="sxs-lookup"><span data-stu-id="9d980-114">Copy the knowledge base ID from the first line and the host name from the second line and paste them into a text file as well.</span></span> <span data-ttu-id="9d980-115">Chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="9d980-115">Then close the dialog.</span></span> <span data-ttu-id="9d980-116">**Non** includere il prefisso "https://" nel nome host copiato.</span><span class="sxs-lookup"><span data-stu-id="9d980-116">**Do not** include the "https://" prefix in the host name that you copy.</span></span>

    ![Copia di nome host e ID della knowledge base](../images/copy-endpoint-info.png)
    
    <span data-ttu-id="9d980-118">_Copia di nome host e ID della knowledge base_</span><span class="sxs-lookup"><span data-stu-id="9d980-118">_Copying the knowledge base ID and host name_</span></span>  

1. <span data-ttu-id="9d980-119">Tornare al bot app Web nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="9d980-119">Return to the Web App Bot in the Azure portal.</span></span> <span data-ttu-id="9d980-120">Fare clic su **Impostazioni applicazione** nel menu a sinistra e scorrere verso il basso fino a trovare le impostazioni denominate "QnAKnowledgebaseId", "QnAAuthKey" e "QnAEndpointHostName."</span><span class="sxs-lookup"><span data-stu-id="9d980-120">Click **Application settings** in the menu on the left and scroll down until you find application settings named "QnAKnowledgebaseId," "QnAAuthKey,", and "QnAEndpointHostName."</span></span> <span data-ttu-id="9d980-121">Incollare l'ID della knowledge base e il nome host ottenuti nel passaggio 3 e la chiave endpoint ottenuta nel passaggio 1 in questi campi.</span><span class="sxs-lookup"><span data-stu-id="9d980-121">Paste the knowledge base ID and host name obtained in Step 3 and the endpoint key obtained in Step 1 into these fields.</span></span> <span data-ttu-id="9d980-122">Fare quindi clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="9d980-122">Then click **Save**.</span></span>

    ![Modifica delle impostazioni applicazione](../images/enter-app-settings.png)

    <span data-ttu-id="9d980-124">_Modifica delle impostazioni applicazione_</span><span class="sxs-lookup"><span data-stu-id="9d980-124">_Editing application settings_</span></span> 
 
1. <span data-ttu-id="9d980-125">Tornare a Visual Studio Code e sostituire il contenuto di **app.js** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="9d980-125">Return to Visual Studio Code and replace the contents of **app.js** with the code below.</span></span> <span data-ttu-id="9d980-126">Salvare quindi il file.</span><span class="sxs-lookup"><span data-stu-id="9d980-126">Then save the file.</span></span>

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

    <span data-ttu-id="9d980-127">Notare la chiamata per creare un'istanza di ```QnAMakerDialog``` nella riga 30.</span><span class="sxs-lookup"><span data-stu-id="9d980-127">Note the call to create a ```QnAMakerDialog``` instance on line 30.</span></span> <span data-ttu-id="9d980-128">Questa crea una finestra di dialogo che integra un bot compilato con il servizio Azure Bot con una knowledge base creata con Microsoft QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="9d980-128">This creates a dialog that integrates a bot built with the Azure Bot Service with a knowledge base built Microsoft QnA Maker.</span></span>
 
1. <span data-ttu-id="9d980-129">Fare clic sul pulsante **Controllo del codice sorgente** sulla barra delle attività di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9d980-129">Click the **Source Control** button in the activity bar in Visual Studio Code.</span></span> <span data-ttu-id="9d980-130">Digitare "Connected to knowledge base" nella finestra di messaggio e fare clic sul segno di spunta per eseguire il commit delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="9d980-130">Type "Connected to knowledge base" into the message box, and click the check mark to commit your changes.</span></span> <span data-ttu-id="9d980-131">Quindi fare clic sui puntini di sospensione e usare il comando **Pubblica ramo** per effettuare il push di queste modifiche nel repository remoto (e dunque nell'app Web di Azure).</span><span class="sxs-lookup"><span data-stu-id="9d980-131">Then click the ellipsis and use the **Publish Branch** command to push these changes to the remote repository (and therefore to the Azure Web App).</span></span>

1. <span data-ttu-id="9d980-132">Tornare al bot app Web nel portale di Azure e fare clic su **Test in Web Chat** (Testa nella chat Web) a sinistra per aprire la console di test.</span><span class="sxs-lookup"><span data-stu-id="9d980-132">Return to the Web App Bot in the Azure portal and click **Test in Web Chat** on the left to open the test console.</span></span> <span data-ttu-id="9d980-133">Digitare "What's the most popular software programming language in the world?"</span><span class="sxs-lookup"><span data-stu-id="9d980-133">Type "What's the most popular software programming language in the world?"</span></span> <span data-ttu-id="9d980-134">nella casella nella parte inferiore della finestra della chat e premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="9d980-134">into the box at the bottom of the chat window and press **Enter**.</span></span> <span data-ttu-id="9d980-135">Verificare che il bot risponda come segue:</span><span class="sxs-lookup"><span data-stu-id="9d980-135">Confirm that the bot responds as follows:</span></span>

    ![Test del bot](../images/portal-testing-chat.png)

    <span data-ttu-id="9d980-137">_Test del bot_</span><span class="sxs-lookup"><span data-stu-id="9d980-137">_Testing the bot_</span></span>

<span data-ttu-id="9d980-138">Ora che il bot è connesso alla knowledge base, il passaggio finale consiste nel testarlo in circostanze normali.</span><span class="sxs-lookup"><span data-stu-id="9d980-138">Now that the bot is connected to the knowledge base, the final step is to test it in the wild.</span></span> <span data-ttu-id="9d980-139">Ad esempio, è possibile testarlo con Skype.</span><span class="sxs-lookup"><span data-stu-id="9d980-139">And what could be wilder than testing it with Skype?</span></span>