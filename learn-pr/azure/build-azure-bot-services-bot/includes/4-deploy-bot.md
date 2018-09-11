Quando è stato creato un bot app Web di Azure nell'[Esercizio 1](#Exercise1), è stata distribuita un'app Web di Azure per ospitarlo. Il bot tuttavia richiede del codice e deve ancora essere distribuito all'app Web di Azure. Fortunatamente, il codice è stato generato automaticamente dal servizio Azure Bot. In questa unità si userà Visual Studio Code per inserire il codice in un repository Git locale e pubblicare il bot in Azure eseguendo il push delle modifiche dal repository locale in un repository remoto connesso all'app Web di Azure che ospita il bot, un processo noto come [integrazione continua](https://en.wikipedia.org/wiki/Continuous_integration).

1. Se [Git](https://git-scm.com/) non è installato nel PC, passare a https://git-scm.com/downloads e installare il client Git per il sistema operativo in uso. Git è un sistema di controllo della versione open source e distribuito che si integra facilmente in Visual Studio Code. Se non si è certi che Git sia installato, aprire un prompt dei comandi o una finestra del terminale ed eseguire il comando seguente:

    ``` 
    git --version
    ```

    Se viene visualizzato un numero di versione, il client Git è installato.

1. Se Node.js non è installato nel PC, passare a https://nodejs.org/ e installare l'ultima versione LTS. È possibile determinare se Node.js è installato aprendo un prompt dei comandi o una finestra del terminale e digitando il comando seguente:

    ```
    node --version
    ```

    Se Node è installato, verrà visualizzato il numero di versione.

1. Se Visual Studio Code non è installato nel PC, passare a https://code.visualstudio.com/ e installarlo ora.

1. Creare una cartella denominata "Factbot" per il codice sorgente del bot nella posizione scelta del disco rigido.

1. Tornare al portale di Azure e aprire il gruppo di risorse "factbot-rg". Quindi fare clic sul bot app Web creato nell'[Esercizio 1](#Exercise1).

    ![Apertura del bot app Web](../media-draft/4-open-web-app-bot.png)

1. Fare clic su **Build** nel menu a sinistra, quindi fare clic su **Scarica il file zip** per preparare un file ZIP contenente il codice sorgente del bot. Dopo aver preparato il file ZIP, fare clic sul pulsante **Scarica il file zip** per scaricarlo. Una volta completato il download, copiare il contenuto del file ZIP nella cartella "Factbot" creata al Passaggio 4.

    ![Download del codice sorgente](../media-draft/4-download-source.png)

1. Sempre nel pannello "Build" fare clic su **Configura distribuzione continua**. Fare clic su **Configura** nella parte superiore del pannello che segue, seguito da **Scegliere l'origine**. Selezionare **Repository Git locale** come origine della distribuzione e fare clic su **OK**. 

    ![Specifica di un repository Git locale come origine della distribuzione](../media-draft/4-portal-set-local-git.png)

1. Chiudere il pannello "Distribuzioni" e fare clic su **Tutte le impostazione del servizio app** nel menu a sinistra.

1. Fare clic su **Credenziali distribuzione** e immettere un nome utente e una password. Probabilmente si dovrà immettere un nome utente diverso da "FactbotAdministrator", perché il nome deve essere univoco all'interno di Azure. Fare clic su **Salva** e chiudere il pannello.

    ![Immissione delle credenziali di distribuzione](../media-draft/4-portal-enter-ci-creds.png)

1. Avviare Visual Studio Code e usare il comando **File** > **Apri cartella...** per aprire la cartella "Factbot" in cui è stato copiato il codice sorgente del bot nel Passaggio 6.

1. Fare clic sul pulsante **Controllo del codice sorgente** sulla barra delle attività sul lato sinistro di Visual Studio Code e scegliere l'icona **Inizializza repository** nella parte superiore. Quindi, fare clic sul pulsante **Inizializza repository** nella finestra di dialogo che segue.

    ![Inizializzazione di un repository Git locale](../media-draft/4-vs-init-git-repo.png)

1. Digitare "First commit." nella finestra di messaggio e quindi fare clic sul segno di spunta per eseguire il commit delle modifiche.

    ![Commit delle modifiche nel repository locale](../media-draft/4-vs-first-git-commit.png)

1. Selezionare **Terminale integrato** dal menu **Visualizza** di Visual Studio Code per aprire un terminale integrato. Eseguire quindi il comando seguente nel terminale integrato, sostituendo BOT_NAME in due punti con il nome del bot immesso al passaggio 3 dell'Esercizio 1.

    ```
    git remote add qna-factbot https://BOT_NAME.scm.azurewebsites.net:443/BOT_NAME.git
    ```

1. Fare clic sui puntini di sospensione nella parte superiore del pannello CONTROLLO DEL CODICE SORGENTE e selezionare **Pubblica ramo** dal menu per eseguire il push del codice del bot dal repository locale ad Azure. Se vengono richieste le credenziali, immettere il nome utente e la password specificati al Passaggio 9 di questo esercizio.

Il bot è stato pubblicato in Azure. Prima di testarlo in Azure, tuttavia, verrà eseguito in locale e si apprenderà come eseguirne il debug in Visual Studio Code.