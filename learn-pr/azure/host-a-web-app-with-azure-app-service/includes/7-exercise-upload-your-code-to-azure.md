In questa unità l'applicazione ASP.NET Core verrà caricata in Servizio app di Azure.

## <a name="create-a-staging-deployment-slot"></a>Creare uno slot di distribuzione di staging

1. Tornare al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. Aprire la risorsa Servizio app (l'app Web) creata in precedenza. È possibile ritrovarla cercando l'app in **Tutte le risorse** oppure cercando il gruppo di risorse che la contiene in **Gruppi di risorse**.

1. Fare clic sulla voce di menu **Slot di distribuzione** nel riquadro di spostamento a sinistra.

1. Nella pagina **Slot di distribuzione** fare clic sul pulsante **Aggiungi slot** sulla barra di spostamento superiore della pagina Slot di distribuzione.

1. Nel portale di Azure si aprirà la pagina **Aggiungi uno slot** come illustrato di seguito.

    1. Assegnare un nome allo slot di distribuzione. In questo caso, usare `staging`.

    2. Per scegliere un'**Origine configurazione**, sono disponibili due opzioni.

        * È possibile scegliere di clonare gli elementi di configurazione da qualsiasi slot di distribuzione esistente o app del servizio app.
        * In alternativa, è possibile scegliere di non clonare alcun elemento di configurazione. Selezionare l'opzione **Non clonare la configurazione da uno slot esistente**.

        Per questo slot di distribuzione scegliere la seconda opzione, **Non clonare la configurazione da uno slot esistente**. Verrà eseguita la configurazione diretta.

    ![Screenshot del portale di Azure che mostra la configurazione per un nuovo slot di distribuzione di staging.](../media/7-new-deployment-slot-blade.png)

1. Fare clic sul pulsante **OK** nella parte inferiore della pagina per creare un nuovo slot di distribuzione.

1. Dopo aver creato lo slot di distribuzione, il portale di Azure torna alla pagina **Slot di distribuzione** dell'app Web.

    A questo punto, è possibile visualizzare il nuovo slot di distribuzione appena creato.

    ![Screenshot del portale di Azure che mostra la pagina Slot di distribuzione con il nuovo slot creato.](../media/7-deployment-slot-created.png)

1. Selezionare il nuovo slot di distribuzione.

1. Il portale di Azure passerà alla pagina **Panoramica** relativa allo slot di distribuzione creato.

    ![Slot di distribuzione di staging](../media/7-deployment-slot-staging.png)

    Si noti l'**URL** dello slot di distribuzione di staging. Si tratta di un URL diverso rispetto a quello mostrato in precedenza, con il nome dello slot aggiunto come suffisso.

    Uno slot di distribuzione viene considerato come un'app del servizio app completa in Azure. Si tratta tuttavia di un tipo speciale, un elemento figlio dell'app originale che può essere scambiato con l'app originale.

    Se si fa clic sull'**URL**, verrà visualizzata la stessa pagina predefinita creata da Azure per l'app dello slot di distribuzione la prima volta in cui è stata creata nel portale di Azure.

Dopo aver creato lo slot di distribuzione di staging, è necessario configurare le **credenziali per la distribuzione**.

## <a name="create-deployment-credentials"></a>Creare le credenziali per la distribuzione

In Azure è necessario che le credenziali di distribuzione siano configurate prima di iniziare l'effettivo processo di distribuzione. Per questo motivo, si apprenderà come creare le credenziali di distribuzione.

1. Selezionare la voce di menu **Credenziali per la distribuzione** nel riquadro di spostamento a sinistra.

1. Il portale di Azure passerà alla pagina **Credenziali per la distribuzione** come illustrato di seguito.

    Immettere un **nome utente** e una **password** e riconfermare la password.

    > [!NOTE]
    > Assicurarsi di non dimenticare nome utente e password. Queste credenziali saranno necessarie in seguito per avviare il caricamento e la distribuzione del codice in Azure.

    ![Screenshot del portale di Azure che mostra la pagina Credenziali per la distribuzione dello slot di staging con credenziali di esempio nei campi obbligatori.](../media/7-deployment-credentials.png)

1. Fare clic sul pulsante **Salva** nella parte superiore della pagina **Credenziali per la distribuzione**.

Le credenziali per la distribuzione sono state create. Ora è necessario configurare altre opzioni di distribuzione.

## <a name="use-a-local-git-repository-as-your-deployment-option"></a>Usare un repository GIT locale come opzione per la distribuzione

Verrà creato un repository Git locale in Azure per consentire di avviare il caricamento del codice.

1. Nell'app dello slot di distribuzione di **staging** selezionare la voce di menu **Opzioni di distribuzione** dal riquadro di spostamento a sinistra.

1. Il portale di Azure passerà alla pagina **Opzioni di distribuzione**.

1. Fare clic su **Scegliere l'origine** per configurare le impostazioni necessarie.

1. Il portale di Azure visualizza le opzioni disponibili che è possibile configurare e usare. In questo caso scegliere l'opzione **Repository Git locale**.

1. Si tornerà alla pagina **Opzioni di distribuzione**. Fare clic sul pulsante **OK** nella parte inferiore della pagina per configurare l'origine della distribuzione.

1. A questo punto passare alla sezione **Panoramica** nel riquadro di spostamento a sinistra.

    L'informazione rilevante in questo contesto è l'**URI di git clone**, ovvero l'URL del repository Git locale che verrà usato come **remoto** per il repository del codice dell'applicazione locale.

A questo punto è possibile iniziare a caricare il codice nello slot di distribuzione di staging.

## <a name="set-up-git-on-cloud-shell"></a>Configurare Git in Cloud Shell

Git è già installato in Azure Cloud Shell, ma è opportuno configurare il nome utente e l'indirizzo di posta elettronica per l'account di Cloud Shell.

1. In Cloud Shell a destra digitare i comandi seguenti, sostituendo i segnaposto `[your name]` e `[your email]` con il proprio nome e il proprio indirizzo di posta elettronica, senza le parentesi graffe:

    ```bash
    git config --global user.name "[your name]"
    git config --global user.email "[your email]"
    ```

1. Per verificare che le informazioni siano state registrate da Git, digitare il comando seguente:

    ```bash
    cat ~/.gitconfig
    ```

   Sarà visualizzato quanto riportato di seguito insieme a nome e indirizzo di posta elettronica:

    ```output
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-code"></a>Inizializzare un repository Git locale per il codice

Per iniziare a usare Git, è necessario inizializzare un repository Git locale per il codice dell'applicazione .NET Core.

1. Assicurarsi di trovarsi nella cartella del progetto creata in precedenza.

    ```bash
    cd ~/BestBikeApp/
    ```

1. Eseguire il comando seguente per inizializzare un nuovo repository Git:

    ```bash
    git init
    ```

    Se il comando ha esito positivo, viene visualizzato un messaggio simile al seguente:

    ```output
    Initialized empty Git repository in /home/{your-user}/BestBikeApp/.git/
    ```

1. Eseguire lo staging di tutti i file dell'applicazione in Git.

   Il passaggio successivo consiste nel segnalare i file dell'applicazione a Git. Aggiungere quindi tutti i file della directory di lavoro in modo che siano **inclusi nello staging** da Git. Digitare il comando seguente:

    ```bash
    git add .
    ```

    Il comando precedente aggiunge tutti i file, rappresentati da ".", allo stato di staging di GIT.

1. A questo punto, eseguire il commit delle modifiche in GIT.

   Dopo aver completato lo staging dei file in GIT, è necessario eseguire il commit dei file nella **cronologia del commit GIT** nel computer locale. A tal scopo, digitare il comando seguente:

    ```bash
   git commit -m "Initial create"
    ```

   Il comando `commit` accetta l'argomento `-m` per includere un messaggio con il commit che si sta creando. Quando si effettuerà il push del codice in Azure, sarà possibile visualizzare lo stesso messaggio archiviato con questo commit specifico.

## <a name="add-a-remote-for-the-local-git-repository"></a>Aggiungere un repository remoto per il repository GIT locale

A questo punto è stato inizializzato un nuovo repository Git locale. È stato anche eseguito il commit di tutti i file dell'applicazione in Git. Rimane da aggiungere un repository **remoto** per connettere il repository Git locale a quello ospitato in Azure.

A tale scopo, eseguire le operazioni seguenti:

1. Copiare l'**URL di git clone** visto in precedenza.

1. Dopo averlo copiato, tornare alla finestra **Terminale** ed eseguire il comando Git seguente con l'URL in uso:

    ```bash
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    Il comando Git precedente consente al repository Git locale di collegarsi a quello ospitato in Azure. È ora possibile effettuare il push e il pull tra il repository GIT locale e quello remoto.

1. Per verificare il comando precedente, digitare il comando GIT seguente:

    ```bash
    git remote -v
    ```

    Il comando precedente genera l'output seguente:

    ```output
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a>Effettuare il push del codice in Azure

Ora che in Azure il repository Git locale è collegato al repository Git remoto, sarà possibile sviluppare e compilare l'app ed eseguire il push del codice dell'applicazione in Azure.

1. Digitare il comando Git seguente per effettuare il push del ramo **master** nel repository Git remoto in Azure:

    ```bash
    git push origin master
    ```

1. Verrà richiesto di immettere la password configurata nella sezione precedente **Credenziali per la distribuzione**. Immetter la password e premere INVIO. Git inizia a caricare i file di cui è stato eseguito il commit nel repository Git remoto di Azure configurato, all'interno dello slot di distribuzione di staging.

## <a name="verify-the-code-is-uploaded-to-azure"></a>Verificare che il codice venga caricato in Azure

1. Tornare al portale di Azure.

1. Selezionare la voce di menu **Tutte le risorse** nel riquadro di spostamento a sinistra.

1. Il portale di Azure passerà all'elenco di tutte le risorse create in Azure fino a questo momento.

1. Fare clic sullo slot di staging creato in precedenza. Tenere presente che uno slot di distribuzione viene considerato un'app, pertanto verrà visualizzato come risorsa Servizio app in **Tutte le risorse**.

1. Quando viene visualizzata la pagina dello slot di distribuzione di staging, passare a **Opzioni di distribuzione**.

    Si noti che il primo commit disponibile in locale nel computer è ora caricato nel portale di Azure.

    Quando si effettua il push del codice in locale nel repository Git remoto in Servizio app, Azure registra l'operazione.

    Ogni volta che si effettua il push del codice in Azure, verrà visualizzato un nuovo record insieme al messaggio digitato durante il commit delle modifiche in locale nel computer.

    ![Screenshot del portale di Azure che mostra una distribuzione di repository Git recente nella pagina Opzioni di distribuzione.](../media/7-staging-deployment-slot-after-uploading-files.png)

1. Visitare ora l'URL dello **slot di staging**. L'URL è stato specificato in precedenza. Se però è stato dimenticato, è sempre possibile accedere alla pagina **Panoramica** dello slot di distribuzione di staging per recuperarlo.

1. Digitare l'URL seguente nella barra degli indirizzi del browser: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).

    ![Screenshot che mostra una visualizzazione del Web browser del sito Web dello slot di distribuzione di staging.](../media/7-staging-slot-hosted-online.png)

I file dell'applicazione locale sono stati caricati nello slot di distribuzione di staging in Azure.

## <a name="swapping-the-staging-and-production-deployment-slots"></a>Scambio di slot di distribuzione di staging e di produzione

Ora che l'applicazione è attiva e in esecuzione nello slot di distribuzione di staging ospitato in Azure, è possibile scambiare questo slot con quello di produzione. A questo scopo, seguire questa procedura:

1. Passare alla pagina dell'app originale creata in precedenza. L'app Web originale è disponibile nella pagina **Tutte le risorse**.

1. Fare clic sulla voce di menu **Slot di distribuzione** nel riquadro di spostamento a sinistra.

1. Fare clic sul pulsante **Scambia** nella parte superiore della pagina.

1. Nel portale di Azure viene visualizzata la pagina **Scambia**.

1. Per il campo **Scambia** selezionare **Scambia**.

1. Per il campo **Origine**, selezionare **Staging**.

1. Per il campo **Destinazione** selezionare **Produzione**.

    ![Screenshot del portale di Azure che mostra la pagina Scambia dello slot di distribuzione.](../media/7-swap-blade.png)

1. Fare clic sul pulsante **OK** nella parte inferiore della pagina.

1. Azure avvierà il processo di scambio. Questa operazione richiede generalmente pochi secondi, a seconda delle dimensioni dell'app Web sottoposta a scambio.

1. Al termine dell'operazione, visitare l'URL dell'app Web, disponibile nella pagina Panoramica per il servizio app nel portale: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).

    ![Screenshot che mostra una visualizzazione del Web browser dello slot di distribuzione di staging precedente ospitato ora come app Web primaria.](../media/7-web-app-page.png)

    L'operazione di scambio è stata completata. È ora possibile osservare che il codice che è stato caricato nello slot di distribuzione di staging è anche ospitato nello slot di produzione.

1. A questo punto, visitare l'URL dello slot di staging [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).

    ![Screenshot che mostra una visualizzazione del Web browser dello slot di distribuzione primario precedente ospitato ora come app Web dello slot di distribuzione di staging.](../media/7-staging-after-swapping.png)

    Lo slot di distribuzione di staging fornisce ora i file HTML predefiniti e originali che erano in precedenza forniti dallo slot di produzione.

Congratulazioni. Il codice dell'applicazione è stato caricato in Azure e gli slot di distribuzione sono stati scambiati.
