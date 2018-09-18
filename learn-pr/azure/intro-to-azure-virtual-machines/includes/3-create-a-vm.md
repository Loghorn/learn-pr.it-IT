L'infrastruttura di rete è stata pianificata e sono state identificate alcune macchine virtuali di cui eseguire la migrazione al cloud. Sono disponibili diverse opzioni per creare le macchine virtuali. La scelta dipende dall'ambiente con cui si ha maggiore famigliarità. Azure supporta un portale basato sul Web per la creazione e l'amministrazione delle risorse. È anche possibile usare gli strumenti da riga di comando in esecuzione su Windows, MacOS e Linux.

> [!TIP]
> Tutti gli esercizi di Microsoft Learn sono gratuiti, ma quando si inizia a esplorare per proprio conto, è necessario procurarsi una sottoscrizione di Azure. Se non si dispone di una sottoscrizione, creare un account [gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) in pochi minuti.

Esaminiamo innanzitutto il portale di Azure: è il modo più semplice per iniziare a usare Azure.

## <a name="azure-portal"></a>Portale di Azure

Il **portale di Azure** offre un'interfaccia utente basata su browser facile da usare che consente di creare e gestire tutte le risorse di Azure. È ad esempio possibile configurare un nuovo database, aumentare la potenza di calcolo delle macchine virtuali e monitorare i costi mensili. È anche uno strumento di apprendimento efficace, perché consente di esaminare tutte le risorse disponibili e usare procedure guidate per creare le risorse necessarie.

Dopo aver eseguito l'accesso, vengono visualizzate due aree principali. La prima è un menu con le opzioni che consentono di creare e monitorare le risorse e di gestire la fatturazione. La seconda è un dashboard personalizzabile che offre una visualizzazione snapshot di tutti i servizi essenziali distribuiti in Azure. Quando si inizia a usare Azure, è probabile che il portale sia l'opzione più semplice da usare.

> [!NOTE]
> Le viste che vengono presentate quando si eseguono selezioni nel portale sono spesso chiamate _pannelli_. Un pannello può fungere sia da struttura di menu che da pannello di configurazione. Quando si esplora il portale di Azure, l'interfaccia utente viene distribuita da sinistra a destra e il riquadro di visualizzazione Web scorre in modo da mostrare il pannello corrente. È possibile usare il dispositivo di scorrimento nella parte inferiore per tornare rapidamente alle visualizzazioni padre.

### <a name="create-an-azure-vm-with-the-azure-portal"></a>Creare una macchina virtuale di Azure con il portale di Azure

Si supponga di che voler creare una macchina virtuale che esegue un sito Web WordPress. La configurazione di un sito non è difficile, ma ci sono un paio di aspetti da tenere presenti. È necessario installare e configurare un sistema operativo, configurare un sito Web, installare un database e occuparsi di aspetti come i firewall. Nei moduli successivi ci si occuperà in modo approfondito della creazione di macchine virtuali, ma ora ne verrà creata una per dimostrare quanto sia facile. Non verranno prese in considerazione tutte le opzioni. Consultare uno dei moduli **Creare una macchina virtuale** per ottenere informazioni dettagliate su ogni opzione.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true). Il menu per la creazione e la gestione delle risorse di Azure verrà visualizzato sulla sinistra, mentre il resto della schermata sarà occupato dal dashboard.

    ![Dashboard principale del portale di Azure](../media-draft/3-dashboard-page.png)

1. Selezionare l'opzione **Crea una risorsa** nell'angolo superiore sinistro del portale. Verrà aperto il pannello di Azure Marketplace. Se la barra laterale sinistra è compressa, viene visualizzato un "più" verde. È possibile espandere la barra laterale facendo clic sul cursore di espansione per vedere il testo completo, come illustrato nell'immagine precedente.

    ![Azure Marketplace](../media-draft/3-create-new-resource.png)

    Come si può notare, le opzioni selezionabili sono numerose. Tenere presente che si intende creare una VM che esegue un sito Web WordPress. Dato che le VM sono risorse di calcolo di Azure, selezionare l'opzione **Calcolo** nell'elenco disponibile e quindi cercare le immagini di VM WordPress.

1. Usare la barra di ricerca **Cerca nel Marketplace** per cercare "WordPress". Verrà visualizzato un elenco di opzioni. Selezionare l'opzione **WordPress Certified by Bitnami** (WordPress certificata da Bitnami).

    ![Ricerche nell'Azure Marketplace](../media-draft/3-search-vm-image.png)

    Il pannello visualizzato successivamente presenta informazioni sulle licenze per l'immagine che si intende utilizzare. Fare clic su **Crea**.

    ![Selezionare e creare il sito WordPress](../media-draft/3-create-vm-image.png)

1. Verrà visualizzato il pannello **Crea macchina virtuale**. Si noti l'approccio basato su procedura guidata che è possibile usare per configurare la VM.

    ![Passaggio di configurazione 1](../media-draft/3-create-vm-1.png)

    È necessario configurare i parametri di base della macchina virtuale WordPress. È normale che, a questo punto, alcune opzioni non siano ancora chiare. Tenere presente che tutte queste opzioni verranno illustrate in un modulo futuro. È consigliabile copiare i valori usati qui.

<!-- TODO: fix subscription + resource group -->
1. Usare i valori seguenti nella scheda **Base**.
    - Selezionare la sottoscrizione gratuita e un gruppo di risorse.
    - Immettere un **Nome** per la macchina virtuale: qui abbiamo usato `test-wp1-eus-vm`.
    - Selezionare un'**Area** nelle vicinanze. È possibile scegliere la località dall'elenco a discesa.
    - Scegliere **Nessuna** per le opzioni di disponibilità. In un altro modulo verrà illustrata la disponibilità elevata.
    - L'**immagine** dovrà essere l'opzione **WordPress di Bitnami** selezionata nel Marketplace.
    - Lasciare l'impostazione predefinita in **Dimensione**. Si otterranno così un singolo core e 3,5 GB di memoria, che dovrebbero essere sufficienti per un semplice sito Web.
    - Passare a **Password** come tipo di autenticazione e immettere un nome utente e una password.
    - Aprire l'elenco a discesa **Selezionare le porte in ingresso pubbliche** e selezionare **HTTP** come illustrato di seguito.

    ![Aprire la porta HTTP](../media-draft/3-open-http-port.png)

1. Esistono diverse altre schede che è possibile esplorare per visualizzare le impostazioni da modificare durante la creazione della macchina virtuale. Al termine dell'esplorazione, fare clic su **Rivedi e crea** per esaminare e convalidare le impostazioni.

    ![Passaggio di configurazione 2](../media-draft/3-review-create-vm.png)

1. Nella schermata di revisione Azure convaliderà le impostazioni. Verificare che tutte le impostazioni siano impostate nel modo desiderato e quindi fare clic su **Crea** per distribuire e creare la VM.

1. È possibile monitorare la distribuzione tramite il pannello **Notifiche**. Fare clic sull'icona nella barra degli strumenti superiore per visualizzare il pannello.

    ![Monitorare lo stato della distribuzione](../media-draft/3-deploying.png)

1. Il processo di distribuzione della macchina virtuale richiede alcuni minuti. Si riceverà una notifica che informa che la distribuzione ha avuto esito positivo. Fare clic sul messaggio per passare al gruppo di risorse con tutti gli elementi che sono stati creati per la macchina virtuale.

    ![VM distribuita](../media-draft/3-deployment-succeeded.png)

1. Selezionare la voce della VM, che dovrebbe essere la prima e avrà il nome che si è specificato.

    ![Selezionare la VM nel gruppo di risorse](../media-draft/3-open-vm-properties.png)

1. Questa operazione farà passare alla **Panoramica** della macchina virtuale appena creata. Qui è possibile vedere tutte le informazioni e le opzioni di configurazione per la macchina virtuale WordPress appena creata. Una delle informazioni presenti è l'**indirizzo IP pubblico**.

    ![Ottenere l'indirizzo IP pubblico della macchina virtuale](../media-draft/3-public-ip-address.png)

11. Copiare l'indirizzo IP, aprire una nuova scheda nel browser e incollarlo. Si passerà così a un nuovo sito WordPress.

    ![Nuovo sito WordPress live](../media-draft/3-my-new-blog.png)

La procedura è stata completata. Con pochi passaggi è stata distribuita una VM che esegue Linux, con un database installato e un sito Web funzionale. Verranno ora esaminati alcuni altri modi in cui avrebbe potuto essere creare una VM.
