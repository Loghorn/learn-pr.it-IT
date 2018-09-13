In questo esercizio si connetterà il bot alla knowledge base di QnA Maker creata in precedenza, in modo che possa svolgere una conversazione intelligente. Per la connessione alla knowledge base occorre recuperare alcune informazioni dal portale di QnA Maker, copiarle nel portale di Azure, aggiornare il codice del bot e quindi distribuire nuovamente il bot in Azure.

1. Tornare al [portale di QnA Maker](https://www.qnamaker.ai/) e fare clic sul proprio nome nell'angolo superiore destro. Selezionare **Manage endpoint keys** (Gestisci chiavi endpoint) dal menu a discesa. Fare clic su **Show** (Mostra) per visualizzare la chiave endpoint primaria e su **Copy** (Copia) per copiarla negli Appunti. Incollarla quindi in un file di testo in modo da recuperarla facilmente più avanti.

    ![Copia della chiave endpoint](../images/copy-primary-key.png)
    
    _Copia della chiave endpoint_ 

1. Fare clic su **My knowledge bases** (Knowledge base personali) nel menu nella parte superiore della pagina. Quindi fare clic su **View Code** (Visualizza codice) per la knowledge base creata in precedenza.

    ![Apertura della knowledge base](../images/open-knowledge-base.png)

    _Apertura della knowledge base_

1. Copiare l'ID della knowledge base dalla prima riga e il nome host dalla seconda riga. Incollarli anche in un file di testo. Chiudere la finestra di dialogo. **Non** includere il prefisso "https://" nel nome host copiato.

    ![Copia di nome host e ID della knowledge base](../images/copy-endpoint-info.png)
    
    _Copia di nome host e ID della knowledge base_  

1. Tornare al bot app Web nel portale di Azure. Fare clic su **Impostazioni applicazione** nel menu a sinistra e scorrere verso il basso fino a trovare le impostazioni denominate "QnAKnowledgebaseId", "QnAAuthKey" e "QnAEndpointHostName." Incollare l'ID della knowledge base e il nome host ottenuti nel passaggio 3 e la chiave endpoint ottenuta nel passaggio 1 in questi campi. Fare quindi clic su **Salva**.

    ![Modifica delle impostazioni applicazione](../images/enter-app-settings.png)

    _Modifica delle impostazioni applicazione_ 
 
1. Tornare a Visual Studio Code e sostituire il contenuto di **app.js** con il codice seguente. Salvare quindi il file.

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

    Notare la chiamata per creare un'istanza di `QnAMakerDialog` nella riga 30. Questa crea una finestra di dialogo che integra un bot compilato con il servizio Azure Bot con una knowledge base creata con Microsoft QnA Maker.
 
1. Fare clic sul pulsante **Controllo del codice sorgente** sulla barra delle attività di Visual Studio Code. Digitare "Connected to knowledge base" nella finestra di messaggio e fare clic sul segno di spunta per eseguire il commit delle modifiche. Fare quindi clic sui puntini di sospensione e usare il comando **Pubblica ramo** per effettuare il push di queste modifiche nel repository remoto (e dunque nell'app Web di Azure).

1. Tornare al bot app Web nel portale di Azure e fare clic su **Test in Web Chat** (Testa nella chat Web) a sinistra per aprire la console di test. Digitare "What's the most popular software programming language in the world?" nella casella nella parte inferiore della finestra della chat e premere **INVIO**. Verificare che il bot risponda come segue:

    ![Test del bot](../images/portal-testing-chat.png)

    _Test del bot_

Ora che il bot è connesso alla knowledge base, il passaggio finale consiste nel testarlo in circostanze normali. Ad esempio, è possibile testarlo con Skype.