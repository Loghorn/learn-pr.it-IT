Analogamente al codice di qualsiasi applicazione, le modifiche al codice di un bot devono essere testate e sottoposte a debug in locale prima di essere distribuite nell'ambiente di produzione. Per eseguire il debug dei bot, Microsoft offre [Bot Framework Emulator](https://emulator.botframework.com/). In questa unità verrà descritto come usare Visual Studio Code e l'emulatore per eseguire il debug dei bot.

1. Se Microsoft Bot Framework Emulator non è installato, installarlo adesso. È possibile scaricarlo all'indirizzo https://emulator.botframework.com/. Sono disponibili versioni per Windows, macOS e Linux.

1. Eseguire il comando seguente nel terminale integrato di Visual Studio Code per installare [Restify](http://restify.com/), un diffuso pacchetto Node.js per la creazione e l'utilizzo di servizi Web RESTful:

    ```
    npm install restify
    ```

1. Ripetere questo passaggio per i comandi seguenti per installare [Microsoft Bot Framework Bot Builder SDK per Node.js](https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-quickstart):

    ```
    npm install botbuilder
    npm install botbuilder-azure
    npm install botbuilder-cognitiveservices
    ```

1. Fare clic sul pulsante **Explorer** sulla barra delle attività di Visual Studio Code. Selezionare quindi **app.js** per aprirlo nell'editor del codice. Questo file contiene il codice del bot, generato dal servizio Azure Bot e scaricato dal portale di Azure.

    ![Apertura di app.js](../media-draft/5-vs-select-index-js.png)

1. Sostituire il contenuto del file **app.js** con il codice seguente, quindi salvare il file.

    ```JavaScript
    "use strict";
    var builder = require("botbuilder");
    var botbuilder_azure = require("botbuilder-azure");
    
    var useEmulator = true; 
    var userName = ""; 
    var yearsCoding = ""; 
    var selectedLanguage = "";
    
    var connector = useEmulator ? new builder.ChatConnector() : new botbuilder_azure.BotServiceConnector({
        appId: process.env.MicrosoftAppId,
        appPassword: process.env.MicrosoftAppPassword      
    });
    
    var bot = new builder.UniversalBot(connector);
    
    bot.dialog('/', [
    
    function (session) {
        builder.Prompts.text(session, "Hello, and welcome to QnA Factbot! What's your name?");
    },
    
    function (session, results) {
        userName = results.response;
        builder.Prompts.number(session, "Hi " + userName + ", how many years have you been writing code?"); 
    },
    
    function (session, results) {
        yearsCoding = results.response;
        builder.Prompts.choice(session, "What language do you love the most?", ["C#", "Python", "Node.js", "Visual FoxPro"]);
    },
    
    function (session, results) {
        selectedLanguage = results.response.entity;   
    
        session.send("Okay, " + userName + ", I think I've got it:" +
                    " You've been writing code for " + yearsCoding + " years," +
                    " and prefer to use " + selectedLanguage + ".");
    }]);
     
    var restify = require('restify');
    var server = restify.createServer();

    server.listen(3978, function() {
        console.log('test bot endpoint at http://localhost:3978/api/messages');
    });

    server.post('/api/messages', connector.listen());    
    ```

1. Impostare punti di interruzione alle righe 20, 25 e 30 facendo clic sul margine a sinistra.
 
    ![Aggiunta di punti di interruzione al file app.js](../media-draft/5-vs-add-breakpoints.png)

1. Fare clic sul pulsante **Debug** sulla barra delle attività, quindi fare clic sulla freccia verde per avviare una sessione di debug. Verificare che nella console di debug compaia "test bot endpoint at http://localhost:3978/api/messages".
 
    ![Avvio del debugger](../media-draft/5-vs-launch-debugger.png)

1. Il codice del bot è ora eseguito in locale. Avviare Bot Framework Emulator e fare clic su **Create a new bot configuration** (Crea nuova configurazione bot). Immettere il nome del bot e l'URL del bot visualizzato nella console di debug nel passaggio precedente. Fare clic su **Save and connect** (Salva e connetti) e salvare il file di configurazione nella posizione scelta.

    > In futuro sarà possibile riconnettersi al bot semplicemente facendo clic sul nome del bot in "My Bots" (Bot personali).

    ![Connessione al bot](../media-draft/5-new-bot-configuration.png)

1. Digitare "hi" nella casella nella parte inferiore dell'emulatore e premere **INVIO**. Verificare che Visual Studio Code si interrompa alla riga 20 di **app.js**. Fare quindi clic sul pulsante **Continua** sulla barra degli strumenti di debug di Visual Studio Code e tornare all'emulatore per visualizzare la risposta del bot.
 
    ![Prosecuzione nel debugger](../media-draft/5-continue-debugging.png)

1. Il bot formulerà una serie di domande. Rispondere e fare clic su **Continua** in Visual Studio Code ogni volta che viene raggiunto un punto di interruzione. Al termine, fare clic sul pulsante **Arresta** sulla barra degli strumenti di debug per terminare la sessione di debug.

A questo punto è stato ottenuto un bot completamente funzionante e si è appreso a eseguirne il debug in locale in Microsoft Bot Framework Emulator. Il passaggio successivo consiste nel rendere il bot più intelligente connettendolo alla knowledge base pubblicata.