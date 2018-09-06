In questo esercizio si caricherà l'applicazione ASP.NET Core in Azure.

## <a name="create-a-staging-deployment-slot"></a>Creare uno slot di distribuzione di staging

1. Individuare e selezionare la voce di menu **Slot di distribuzione** dal menu di spostamento a sinistra.

1. Azure aprirà il pannello **Slot di distribuzione** come illustrato di seguito.

    ![Slot di distribuzione](../media-draft/7-deployment-slot-blade.png)

1. Individuare e selezionare il pulsante **+ Aggiungi slot** nella barra di spostamento superiore del pannello Slot di distribuzione.

1. Nel portale di Azure si aprirà il pannello **Aggiungi uno slot** come illustrato di seguito.

    ![Aggiungi uno slot](../media-draft/7-new-deployment-slot-blade.png)

    Assegnare un nome allo slot di distribuzione. In questo caso, usare **Staging**.

    Per scegliere un'**origine della configurazione**, sono disponibili 2 opzioni.

    1. È possibile scegliere di clonare gli elementi di configurazione di altri slot o un'app Web creata in Azure.

    1. È possibile scegliere di non clonare alcun elemento di configurazione. In questo caso, è necessario selezionare l'opzione **Non clonare la configurazione da uno slot esistente**.

    Selezionare la seconda opzione **Non clonare la configurazione da uno slot esistente**.

1. Individuare e fare clic sul pulsante **OK** nella parte inferiore del pannello.

1. Dopo aver creato lo slot di distribuzione, il portale di Azure passa nuovamente al pannello **Slot di distribuzione** dell'app Web.

    A questo punto, è possibile visualizzare il nuovo slot di distribuzione appena creato.

    ![Slot di distribuzione creato](../media-draft/7-deployment-slot-created.png)

1. Individuare e selezionare il nuovo slot di distribuzione.

1. Il portale di Azure passerà alla pagina **Panoramica** relativa allo slot di distribuzione creato.

    ![Slot di distribuzione di staging](../media-draft/7-deployment-slot-staging.png)

    Si noti l'**URL** dello slot di distribuzione di staging. È esattamente l'URL visto precedentemente all'inizio dell'unità.

    In Azure uno slot di distribuzione viene considerato come se fosse un'app Web. Si tratta tuttavia di un tipo speciale, un elemento figlio dell'app Web originale che può essere scambiato con l'app Web originale.

    Se si seleziona l'**URL**, sarà visualizzata la stessa pagina predefinita creata da Azure per l'app Web la prima volta in cui è stata creata nel portale di Azure.

Dopo aver creato la distribuzione di staging, è necessario configurare le **credenziali per la distribuzione**.

## <a name="create-deployment-credentials"></a>Creare le credenziali per la distribuzione

In Azure è necessario che le credenziali di distribuzione siano configurate prima di iniziare l'effettivo processo di distribuzione. Per questo motivo, si apprenderà come creare le credenziali di distribuzione.

1. Individuare e selezionare la voce di menu **Credenziali per la distribuzione** dal menu di spostamento a sinistra.

1. Il portale di Azure passerà al pannello **Credenziali per la distribuzione** come illustrato di seguito.

    ![Credenziali per la distribuzione](../media-draft/7-deployment-credentials.png)

    Immettere un **nome utente** e una **password** e riconfermare la password.

    > Ricordarsi di annotare nome utente e password in modo che non siano dimenticati. Queste credenziali saranno necessarie per avviare il caricamento e la distribuzione del codice in Azure.

    Dopo aver scelto nome utente e password, individuare e fare clic sul pulsante **Salva** nella parte superiore del pannello **Credenziali per la distribuzione**.

Le credenziali per la distribuzione sono state create. Ora è necessario configurare le **opzioni di distribuzione**.

## <a name="use-a-local-git-repository-as-your-deployment-option"></a>Usare un repository GIT locale come opzione per la distribuzione

A questo punto, perché sia possibile avviare il processo di caricamento del codice, è necessario creare un repository GIT locale in Azure.

1. Individuare e selezionare la voce di menu **Opzioni di distribuzione** dal menu di spostamento a sinistra.

1. Il portale di Azure passerà al pannello **Opzioni di distribuzione** come illustrato di seguito.

    ![Opzioni di distribuzione](../media-draft/7-deployment-options.png)

1. Fare clic su **Scegliere l'origine / Configurare le impostazioni necessarie**.

    ![Credenziali per la distribuzione](../media-draft/7-deployment-sources.png)

1. Il portale di Azure visualizza le opzioni disponibili che è possibile configurare e usare. In questo caso scegliere l'opzione **Repository GIT locale**.

    ![Repository GIT locale](../media-draft/7-local-git-repo.png)

1. Individuare e fare clic sul pulsante **OK** nella parte inferiore del pannello.

1. Individuare e selezionare la voce di menu **Panoramica** dal menu di spostamento a sinistra.

    ![Slot di distribuzione con GIT configurato](../media-draft/7-staging-after-setting-git.png)

    Si notino due informazioni importanti:
    - **Nome utente GIT/distribuzione**: sono le credenziali che si useranno per la connessione al repository GIT locale in Azure.

    - **URL clone GIT**: è l'URL del repository GIT locale che si userà come **remoto** per il repository dell'applicazione Web locale.

A questo punto è possibile iniziare a caricare il codice nello slot di distribuzione di staging.

## <a name="install-git-on-your-machine"></a>Installare GIT nel computer

Si installerà GIT nel computer Ubuntu 18.04.

1. Aprire una nuova finestra **Terminale**

1. Digitare il comando seguente. Verrà richiesto di immettere la password utente Ubuntu.

    ```console
    sudo apt-get update
    ```

1. Dopo aver eseguito l'aggiornamento, digitare il comando seguente per installare GIT in locale. Verrà richiesto di accettare l'installazione di GIT nel computer.

    ```console
    sudo apt-get install git-core
    ```

1. Per verificare che GIT sia ora installato, digitare il comando seguente:

    ```console
    git --version
    ```

   Se l'installazione è stata eseguita correttamente, verrà visualizzato l'output seguente:

    ```console
    git version 2.17.1
    ```

1. È sempre consigliabile configurare le impostazioni di GIT specificando nome e indirizzo di posta elettronica. A tal scopo, eseguire i comandi seguenti, sostituendo i segnaposto per nome e indirizzo di posta elettronica, senza le parentesi graffe (`{}`):

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. Per verificare che le informazioni siano state registrate da GIT, digitare il comando seguente:

    ```console
    cat ~/.gitconfig
    ```

   Sarà visualizzato quanto riportato di seguito insieme a nome e indirizzo di posta elettronica:

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a>Inizializzare un repository GIT locale per l'app Web

Per iniziare a usare GIT, è necessario inizializzare un repository GIT locale per l'app Web.

1. Aprire una nuova finestra **Terminale**.

1. Passare alla cartella radice del contenuto dell'app Web. È possibile digitare quanto segue:

    ```console
    cd ~/Documents/BestBikeApp/
    ```

    ![Cartella radice del contenuto dell'app Web](../media-draft/7-web-app-content-root-folder.png)

1. Eseguire il comando seguente per inizializzare un nuovo repository GIT:

    ```console
    git init
    ```

    Se il comando ha esito positivo, viene visualizzato un messaggio simile al seguente:

    ```console
    Initialized empty Git repository in /home/your-user/Documents/BestBikeApp/.git/
    ```

1. Eseguire lo staging di tutti i file dell'app Web in GIT.

   Nel passaggio successivo GIT riconosce i file dell'app Web. Aggiungere quindi tutti i file della directory di lavoro in modo che siano **gestiti temporaneamente** da GIT. Digitare il comando seguente:

    ```console
    git add .
    ```

    Il comando precedente aggiunge tutti i file, rappresentati da ".", allo stato di staging di GIT.

1. A questo punto, eseguire il commit delle modifiche in GIT.

   Dopo aver completato lo staging dei file in GIT, è necessario eseguire il commit dei file nella **cronologia del commit GIT** nel computer locale. A tal scopo, digitare il comando seguente:

   `git commit -m "Initial create"`

   Il comando `commit` accetta l'argomento `-m` per includere un messaggio con il commit che si sta creando. Quando si eseguirà il push del codice in Azure, sarà possibile visualizzare lo stesso messaggio archiviato con questo commit specifico.

## <a name="add-a-remote-for-the-local-git-repository"></a>Aggiungere un repository remoto per il repository GIT locale

A questo punto è stato inizializzato un nuovo repository GIT locale. È stato anche eseguito il commit di tutti i file dell'app Web in GIT. Rimane da aggiungere un repository **remoto** al quale connettere il repository GIT locale che è stato ospitato in Azure.

A tale scopo, eseguire le operazioni seguenti:

1. Copiare l'**URL clone GIT** visto in precedenza.

1. Dopo averlo copiato, tornare alla finestra **Terminale** ed eseguire il comando GIT seguente:

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    Il comando GIT precedente consente al repository GIT locale di collegarsi a quello ospitato in Azure. È ora possibile eseguire il push e il pull tra il repository GIT locale e quello remoto.

1. Per verificare il comando precedente, digitare il comando GIT seguente:

    ```
    git remote -v
    ```

    Il comando precedente genera l'output seguente:

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a>Eseguire il push del codice in Azure

Ora che in Azure il repository GIT locale è collegato al repository GIT remoto, sarà possibile sviluppare e compilare l'app ed eseguire il push dell'app Web in Azure.

1. Digitare il comando GIT seguente per eseguire il push del ramo **principale** nel repository GIT remoto in Azure:

    ```console
    git push origin master
    ```

1. Verrà richiesto di immettere la password configurata nella sezione precedente **Credenziali per la distribuzione**. Immettere la password e premere INVIO. GIT inizia a caricare i file di cui è stato eseguito il commit nel repository GIT remoto di Azure configurato, all'interno dello slot di distribuzione di staging.

## <a name="verify-the-web-app-is-uploaded-to-azure"></a>Verificare che l'app Web sia caricata in Azure

1. Accedere al portale di Azure.

1. Individuare e selezionare la voce di menu **Tutte le risorse** dal menu di spostamento a sinistra.

1. Il portale di Azure passerà all'elenco di tutte le risorse create in Azure fino a questo momento.

1. Individuare e fare clic sullo slot di staging creato in precedenza. Tenere presente che uno slot di distribuzione viene considerato come se fosse un'app Web e pertanto sarà visualizzato come risorsa dell'app Web in **Tutte le risorse**.

1. Dopo essere passati al pannello dello slot di distribuzione di staging, scegliere **Opzioni di distribuzione**.

    ![Slot di distribuzione di staging dopo aver caricato l'app Web](../media-draft/7-staging-deployment-slot-after-uploading-files.png)

    Si noti che il primo commit disponibile in locale nel computer è ora caricato nel portale di Azure.

    Eseguendo il push del codice in locale nel repository GIT remoto ospitato in Azure, Azure ha registrato l'operazione.

    Ogni volta che si esegue il push del codice in Azure, verrà visualizzato un nuovo record insieme al messaggio digitato durante il commit delle modifiche in locale nel computer.

1. Osserviamo ora l'URL dello **slot di staging**. L'URL è stato specificato in precedenza. Se però è stato dimenticato, è sempre possibile accedere alla pagina **Panoramica** dello slot di distribuzione di staging per recuperarlo.

1. Digitare l'URL seguente nella barra degli indirizzi del browser: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).

    ![Slot di staging ospitato online](../media-draft/7-staging-slot-hosted-online.png)

I file dell'app Web locale sono stati caricati nello slot di distribuzione di staging in Azure.

## <a name="swapping-the-staging-and-production-deployment-slots"></a>Scambio di slot di distribuzione di staging e di produzione

Ora che l'applicazione è attiva e in esecuzione nello slot di distribuzione di staging ospitato in Azure, è possibile scambiare questo slot con quello di produzione. A questo scopo, attenersi alla procedura seguente:

1. Individuare e passare al pannello dell'app Web originale creata nell'unità 2.

1. Individuare e selezionare la voce di menu **Slot di distribuzione** dal menu di spostamento a sinistra.

    ![Pannello Slot di distribuzione dell'app Web](../media-draft/7-web-app-slots.png)

1. Individuare e fare clic sul pulsante **Scambia** nella parte superiore del pannello.

1. Nel portale di Azure viene visualizzato il pannello **Scambia**.

    ![Pannello Scambia](../media-draft/7-swap-blade.png)

1. Per il campo **Scambia**, selezionare **Scambia**.

1. Per il campo **Origine**, selezionare **Staging**.

1. Per il campo **Destinazione**, selezionare **Produzione**.

1. Individuare e fare clic sul pulsante **OK** nella parte inferiore del pannello.

1. Azure avvierà il processo di scambio. Questa operazione richiede generalmente pochi secondi, a seconda delle dimensioni dell'app Web sottoposta a scambio.

1. Al termine dell'operazione, visitare l'URL dell'app Web: [ https://bestbike.azurewebsites.net/ ](https://bestbike.azurewebsites.net/).

    ![Pagina App Web](../media-draft/7-web-app-page.png)

    L'operazione di scambio è stata completata. È ora possibile osservare che il codice che è stato caricato nello slot di distribuzione di staging è anche ospitato nello slot di produzione.

1. A questo punto, visitare l'URL dello slot di staging [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).

    ![Staging dell'app Web](../media-draft/7-staging-after-swapping.png)

    Lo slot di distribuzione di staging ora contiene i file dell'applicazione Web predefiniti e originali che erano in precedenza ospitati nello slot di produzione.

Congratulazioni. L'app Web è stata caricata in Azure.
