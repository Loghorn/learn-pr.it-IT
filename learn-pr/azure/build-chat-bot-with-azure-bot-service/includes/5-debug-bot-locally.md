[!INCLUDE [0-vm-note](0-vm-note.md)]

Analogamente al codice di qualsiasi applicazione, le modifiche al codice di un bot devono essere testate e sottoposte a debug in locale prima di essere distribuite nell'ambiente di produzione. Per eseguire il debug dei bot, Microsoft mette a disposizione Bot Framework Emulator. In questa unità verrà descritto come usare Visual Studio Code e l'emulatore per eseguire il debug dei bot.

1. Eseguire il comando seguente nel terminale integrato di Visual Studio Code per installare [Restify](http://restify.com/), un pacchetto Node.js molto diffuso per la compilazione e l'utilizzo di servizi Web RESTful:

    ```bash
    npm install restify
    ```

1. Ripetere questo passaggio per i comandi seguenti per installare [Microsoft Bot Framework Bot Builder SDK per Node.js](https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-quickstart):

    ```bash
    npm install botbuilder
    npm install botbuilder-azure
    npm install botbuilder-cognitiveservices
    ```

1. Selezionare il pulsante **Esplora risorse** sulla barra delle attività di Visual Studio Code. 
1. Selezionare il file **app.js** per aprirlo nell'editor del codice. Questo file contiene il codice alla base del bot, generato dal servizio Azure Bot e scaricato dal portale di Azure.

1. Sostituire il contenuto del file **app.js** con il codice seguente e quindi salvare il file.

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

1. Impostare punti di interruzione alle righe 20, 25 e 30 (chiamate `builder.Prompts...`) selezionandoli sul margine a sinistra.

1. Selezionare il pulsante **Debug** sulla barra delle attività e quindi fare clic sul pulsante **Avvia debug** con la freccia verde per avviare una sessione di debug. Verificare che nella console di debug venga visualizzato "test bot endpoint at http://localhost:3978/api/messages".

    ![Screenshot di Visual Studio Code che mostra il sistema Debug con la voce Debug e il pulsante di riproduzione del debug usato per avviare una sessione di debug evidenziati.](../media/5-vs-launch-debugger.png)

    Il codice del bot viene ora eseguito in locale.

1. Avviare **Bot Framework Emulator** dal menu Start.

1. Selezionare il campo **Enter your endpoint URL** (Immetti URL endpoint). Immettere il nome e l'URL del bot visualizzati nella console di debug nel passaggio precedente.

1. Lasciare vuoti i campi relativi all'ID e alla password dell'app Microsoft e alle impostazioni locali e selezionare **CONNECT** (Connetti).

1. Selezionare **Save and connect** (Salva e connetti) e salvare il file di configurazione nel percorso di preferenza.

    >[!NOTE]
    > In futuro sarà possibile riconnettersi al bot semplicemente selezionando il nome del bot in "My Bots" (Bot personali).

    ![Screenshot di Bot Framework Emulator che include la schermata di configurazione del nuovo bot con il pulsante di salvataggio e connessione evidenziato.](../media/5-new-bot-configuration.png)

1. Digitare "hi" nella casella nella parte inferiore dell'emulatore e premere **INVIO**. Verificare che Visual Studio Code si interrompa alla riga 20 di **app.js**. Selezionare il pulsante **Continua** sulla barra degli strumenti di debug di Visual Studio Code e tornare all'emulatore per visualizzare la risposta del bot.

1. Il bot formulerà una serie di domande. Rispondere e selezionare **Continua** in Visual Studio Code ogni volta che viene raggiunto un punto di interruzione. Al termine, selezionare il pulsante **Arresta** sulla barra degli strumenti di debug per terminare la sessione di debug.

A questo punto è stato ottenuto un bot completamente funzionante e si è appreso a eseguirne il debug in locale in Microsoft Bot Framework Emulator. Il passaggio successivo consiste nel rendere il bot più intelligente connettendolo alla knowledge base pubblicata.