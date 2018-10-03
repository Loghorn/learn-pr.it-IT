[!INCLUDE [0-vm-note](0-vm-note.md)]

<span data-ttu-id="bd7aa-101">In questa unità, il bot verrà connesso alla knowledge base di QnA Maker creata in precedenza, in modo che possa condurre una conversazione intelligente.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-101">In this unit, you will connect your bot to the QnA Maker knowledge base you built earlier so that the bot can carry on an intelligent conversation.</span></span> <span data-ttu-id="bd7aa-102">Per la connessione alla knowledge base, è necessario recuperare alcune informazioni dal portale di QnA Maker, copiarle nel portale di Azure, aggiornare il codice del bot e quindi distribuire nuovamente il bot in Azure.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-102">Connecting to the knowledge base involves retrieving some information from the QnA Maker portal, copying it into the Azure portal, updating the bot code, and then redeploying the bot to Azure.</span></span>

1. <span data-ttu-id="bd7aa-103">Tornare al portale di QnA Maker all'indirizzo https://www.qnamaker.ai nel browser della macchina virtuale e selezionare il pulsante dell'account nell'angolo superiore destro.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-103">Return to the QnA Maker portal at https://www.qnamaker.ai in the VM browser and select the account button in the upper-right corner.</span></span>
1. <span data-ttu-id="bd7aa-104">Selezionare **Manage endpoint keys** (Gestisci chiavi endpoint) dal menu a discesa.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-104">Select **Manage endpoint keys** from the menu that drops down.</span></span>
1. <span data-ttu-id="bd7aa-105">Selezionare **Show** (Mostra) per visualizzare la chiave endpoint **Primary** (Primaria) e **Copy** (Copia) per copiarla negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-105">Select **Show** to show the **Primary** endpoint key and **Copy** to copy it to the clipboard.</span></span> <span data-ttu-id="bd7aa-106">Incollarla in un file di testo in modo da poterla recuperare facilmente fra poco.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-106">Paste it into a text file, so you can easily retrieve it in a moment.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd7aa-107">A seconda delle impostazioni del browser, per completare questa unità può essere necessario consentire i cookie di terze parti.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-107">Depending on your browser settings, you may need to allow third-party cookies to complete this unit.</span></span>

1. <span data-ttu-id="bd7aa-108">Selezionare **My knowledge bases** (Knowledge base personali) nel menu nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-108">Select **My knowledge bases** in the menu at the top of the page.</span></span>
1. <span data-ttu-id="bd7aa-109">Selezionare quindi **View Code** (Visualizza codice) per la knowledge base creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-109">Then, select **View Code** for the knowledge base that you created earlier.</span></span>

1. <span data-ttu-id="bd7aa-110">Copiare l'ID knowledge base dalla prima riga e il nome host dalla seconda riga.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-110">Copy the knowledge base ID from the first line and the host name from the second line.</span></span> <span data-ttu-id="bd7aa-111">Incollarli anche in un file di testo.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-111">Paste them into a text file, as well.</span></span> <span data-ttu-id="bd7aa-112">Chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-112">Then, close the dialog.</span></span> <span data-ttu-id="bd7aa-113">**Non** includere il prefisso "https://" nel nome host copiato.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-113">**Do not** include the "https://" prefix in the host name that you copy.</span></span>

    ![Screenshot del portale di QnA Maker che illustra la richiesta HTTP di esempio con l'ID e il nome host dell'endpoint della knowledge base evidenziati.](../media/6-copy-endpoint-info.png)

1. <span data-ttu-id="bd7aa-115">Tornare al bot dell'app Web nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-115">Return to the Web App Bot in the Azure portal.</span></span> <span data-ttu-id="bd7aa-116">Selezionare **Impostazioni applicazione** nel menu a sinistra e scorrere verso il basso fino a trovare le impostazioni denominate "QnAKnowledgebaseId", "QnAAuthKey" e "QnAEndpointHostName."</span><span class="sxs-lookup"><span data-stu-id="bd7aa-116">Select **Application settings** in the menu on the left, and scroll down until you find application settings named "QnAKnowledgebaseId," "QnAAuthKey," and "QnAEndpointHostName."</span></span> <span data-ttu-id="bd7aa-117">Incollare in questi campi l'ID della knowledge base e il nome host appena ottenuti e la chiave endpoint ottenuta in precedenza.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-117">Paste the knowledge base ID and host name you just obtained and the endpoint key obtained previously into these fields.</span></span> <span data-ttu-id="bd7aa-118">Selezionare quindi **Salva** nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-118">Then, select **Save** at the top.</span></span>

    ![Screenshot del portale di Azure che illustra il pannello del bot e i dettagli delle impostazioni dell'applicazione con la voce di menu Impostazioni applicazione e le chiavi di impostazione appropriate evidenziate.](../media/6-enter-app-settings.png)

1. <span data-ttu-id="bd7aa-120">Tornare a **Visual Studio Code** e sostituire il contenuto di **app.js** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-120">Return to **Visual Studio Code** and replace the contents of **app.js** with the code below.</span></span> <span data-ttu-id="bd7aa-121">Salvare quindi il file.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-121">Then, save the file.</span></span>

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
        appPassword: process.env.MicrosoftAppPassword,
        openIdMetadata: process.env.BotOpenIdMetadata
    });

    // Listen for messages from users
    server.post('/api/messages', connector.listen());

    var tableName = 'botdata';
    var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
    var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

    // Create your bot with a function to receive messages from the user
    var bot = new builder.UniversalBot(connector);
    bot.set('storage', tableStorage);

    // Recognizer and and Dialog for preview QnAMaker service
    var previewRecognizer = new builder_cognitiveservices.QnAMakerRecognizer({
        knowledgeBaseId: process.env.QnAKnowledgebaseId,
        authKey: process.env.QnAAuthKey || process.env.QnASubscriptionKey
    });

    var basicQnAMakerPreviewDialog = new builder_cognitiveservices.QnAMakerDialog({
        recognizers: [previewRecognizer],
        defaultMessage: 'No match! Try changing the query terms!',
        qnaThreshold: 0.3
    }
    );

    bot.dialog('basicQnAMakerPreviewDialog', basicQnAMakerPreviewDialog);

    // Recognizer and and Dialog for GA QnAMaker service
    var recognizer = new builder_cognitiveservices.QnAMakerRecognizer({
        knowledgeBaseId: process.env.QnAKnowledgebaseId,
        authKey: process.env.QnAAuthKey || process.env.QnASubscriptionKey, // Backward compatibility with QnAMaker (Preview)
        endpointHostName: process.env.QnAEndpointHostName
    });

    var basicQnAMakerDialog = new builder_cognitiveservices.QnAMakerDialog({
        recognizers: [recognizer],
        defaultMessage: "I'm not quite sure what you're asking. Please ask your question again.",
        qnaThreshold: 0.3
    });

    bot.dialog('basicQnAMakerDialog', basicQnAMakerDialog);

    bot.dialog('/', //basicQnAMakerDialog);
        [
            function (session) {
                var qnaKnowledgebaseId = process.env.QnAKnowledgebaseId;
                var qnaAuthKey = process.env.QnAAuthKey || process.env.QnASubscriptionKey;
                var endpointHostName = process.env.QnAEndpointHostName;

                // QnA Subscription Key and KnowledgeBase Id null verification
                if ((qnaAuthKey == null || qnaAuthKey == '') || (qnaKnowledgebaseId == null || qnaKnowledgebaseId == ''))
                    session.send('Please set QnAKnowledgebaseId, QnAAuthKey and QnAEndpointHostName (if applicable) in App Settings. Learn how to get them at https://aka.ms/qnaabssetup.');
                else {
                    if (endpointHostName == null || endpointHostName == '')
                        // Replace with Preview QnAMakerDialog service
                        session.replaceDialog('basicQnAMakerPreviewDialog');
                    else
                        // Replace with GA QnAMakerDialog service
                        session.replaceDialog('basicQnAMakerDialog');
                }
            }
        ]);
    ```

    > [!NOTE]
    > <span data-ttu-id="bd7aa-122">Si noti la chiamata per creare un'istanza di `QnAMakerDialog` nella riga 30.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-122">The call to create a `QnAMakerDialog` instance on line 30.</span></span> <span data-ttu-id="bd7aa-123">Questa crea una finestra di dialogo che integra un bot compilato tramite il servizio Azure Bot con una knowledge base creata tramite Microsoft QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-123">This creates a dialog that integrates a bot built with the Azure Bot Service with a knowledge base built via Microsoft QnA Maker.</span></span>

1. <span data-ttu-id="bd7aa-124">Selezionare il pulsante **Controllo codice sorgente** sulla barra delle attività di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-124">Select the **Source Control** button in the activity bar in Visual Studio Code.</span></span>
1. <span data-ttu-id="bd7aa-125">Passare il mouse sul file **app.js** e selezionare il pulsante __+__ per inserire le modifiche del file per il commit successivo.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-125">Hover over the **app.js** file and select the __+__ button to stage that file's changes for the next commit.</span></span>
1. <span data-ttu-id="bd7aa-126">Digitare "Connected to knowledge base" nella finestra di messaggio e selezionare il segno di spunta per eseguire il commit delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-126">Type "Connected to knowledge base" into the message box, and select the check mark to commit your changes.</span></span>

    > [!Warning]
    > <span data-ttu-id="bd7aa-127">Se si notano modifiche a un file **package.json**, assicurarsi di NON includerle nel commit.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-127">If you see changes to a **package.json** file ensure you do NOT include them in your commit.</span></span> <span data-ttu-id="bd7aa-128">Il commit deve includere solo le modifiche apportate al file **app.js**.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-128">Your commit should only include your changes to **app.js**.</span></span>

1. <span data-ttu-id="bd7aa-129">Selezionare quindi il pulsante con i puntini di sospensione (__...__) e usare il comando **Pubblica ramo** per eseguire il push di queste modifiche nel repository remoto e nell'app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-129">Then, select the ellipsis (__...__) button and use the **Publish Branch** command to push these changes to the remote repository and the Azure Web App.</span></span>

1. <span data-ttu-id="bd7aa-130">Tornare al bot app Web nel portale di Azure e selezionare **Test in Web Chat** (Testa nella chat Web) a sinistra per aprire la console di test.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-130">Return to the web app bot in the Azure portal, and select **Test in Web Chat** on the left to open the test console.</span></span> <span data-ttu-id="bd7aa-131">Digitare "What's the most popular software programming language in the world?"</span><span class="sxs-lookup"><span data-stu-id="bd7aa-131">Type "What's the most popular software programming language in the world?"</span></span> <span data-ttu-id="bd7aa-132">nella casella nella parte inferiore della finestra della chat e premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-132">into the box at the bottom of the chat window and press **Enter**.</span></span> <span data-ttu-id="bd7aa-133">Verificare che il bot risponda.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-133">Confirm that the bot responds.</span></span>

<span data-ttu-id="bd7aa-134">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-134">Congratulations!</span></span> <span data-ttu-id="bd7aa-135">Il bot è connesso alla knowledge base e può rispondere alle domande.</span><span class="sxs-lookup"><span data-stu-id="bd7aa-135">Your bot is connected to the knowledge base and can respond to questions.</span></span>
