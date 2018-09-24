L'infrastruttura di rete è stata pianificata e sono state identificate alcune macchine virtuali di cui eseguire la migrazione al cloud. Sono disponibili diverse opzioni per creare le macchine virtuali. La scelta dipende dall'ambiente con cui si ha maggiore famigliarità. Azure supporta un portale basato sul Web per la creazione e l'amministrazione delle risorse. È anche possibile usare gli strumenti da riga di comando in esecuzione in Windows, MacOS e Linux.

[!include[](../../../includes/azure-sandbox-activate.md)]

#### <a name="options-to-create-and-manage-vms"></a>Opzioni per creare e gestire le macchine virtuali

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

Esaminiamo prima di tutto il portale di Azure: è il modo più semplice per iniziare a usare Azure.

## <a name="azure-portal"></a>Portale di Azure

Il **portale di Azure** offre un'interfaccia utente basata su browser facile da usare che consente di creare e gestire tutte le risorse di Azure. È ad esempio possibile configurare un nuovo database, aumentare la potenza di calcolo delle macchine virtuali e monitorare i costi mensili. È anche uno strumento di apprendimento efficace, perché consente di esaminare tutte le risorse disponibili e usare procedure guidate per creare le risorse necessarie.

Dopo aver eseguito l'accesso, vengono visualizzate due aree principali. La prima è un menu con le opzioni che consentono di creare e monitorare le risorse e di gestire la fatturazione. La seconda è un dashboard personalizzabile che offre una visualizzazione snapshot di tutti i servizi essenziali distribuiti in Azure. Quando si inizia a usare Azure, è probabile che il portale sia l'opzione più semplice da usare.

> [!TIP]
> Le viste che vengono presentate quando si eseguono selezioni nel portale sono spesso chiamate _pannelli_. Un pannello può fungere sia da struttura di menu che da pannello di configurazione. Quando si esplora il portale di Azure, l'interfaccia utente viene distribuita da sinistra a destra e il riquadro di visualizzazione Web scorre in modo da mostrare il pannello corrente. È possibile usare il dispositivo di scorrimento nella parte inferiore per tornare rapidamente alle visualizzazioni padre.

### <a name="create-an-azure-vm-with-the-azure-portal"></a>Creare una macchina virtuale di Azure con il portale di Azure

Si supponga di che voler creare una macchina virtuale che esegue un sito Web WordPress. La configurazione di un sito non è difficile, ma ci sono un paio di aspetti da tenere presenti. È necessario installare e configurare un sistema operativo, configurare un sito Web, installare un database e occuparsi di aspetti come i firewall. Nei moduli successivi ci si occuperà in modo approfondito della creazione di macchine virtuali, ma ora ne verrà creata una per dimostrare quanto sia facile. Non verranno prese in considerazione tutte le opzioni. Consultare uno dei moduli **Creare una macchina virtuale** per ottenere informazioni dettagliate su ogni opzione.

1. Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox. 

1. Il menu per la creazione e la gestione delle risorse di Azure verrà visualizzato sulla sinistra, mentre il resto della schermata sarà occupato dal dashboard.

    ![Screenshot con il dashboard principale del portale di Azure](../media/3-dashboard-page.png)

1. Selezionare l'opzione **Crea una risorsa** nell'angolo superiore sinistro del portale. Verrà aperto il pannello di Azure Marketplace. Se la barra laterale sinistra è compressa, viene visualizzato un "più" verde. È possibile espandere la barra laterale facendo clic sul cursore di espansione per vedere il testo completo, come illustrato nell'immagine precedente.

    ![Screenshot di Azure Marketplace con l'opzione Crea una risorsa evidenziata.](../media/3-create-new-resource.png)

    Come si può notare, le opzioni selezionabili sono numerose. Si intende creare una macchina virtuale che esegue un sito Web WordPress. Dato che le macchine virtuali sono risorse di calcolo di Azure, selezionare l'opzione **Calcolo** nell'elenco disponibile e quindi cercare le immagini di macchine virtuali WordPress. È possibile fare clic su **Visualizza tutto** per ottenere l'elenco completo.

1. Usare la barra di ricerca **Cerca nel Marketplace** per cercare "WordPress". Verrà visualizzato un elenco di opzioni. Selezionare l'opzione **WordPress 4.9.7** come illustrato di seguito.

    ![Screenshot che mostra Cerca nel Marketplace con WordPress 4.9.7 evidenziato.](../media/3-search-vm-image.png)

    Il pannello visualizzato successivamente presenta informazioni sulle licenze per l'immagine che si intende usare. Fare clic su **Crea**.

    ![Screenshot del pannello per la selezione e la creazione di WordPress con WordPress 4.9.7 evidenziato.](../media/3-create-vm-image.png)

1. Verrà visualizzato il pannello **Crea macchina virtuale**. Si noti l'approccio basato su procedura guidata che è possibile usare per configurare la macchina virtuale.

### <a name="configure-the-vm"></a>Configurare la macchina virtuale

È necessario configurare i parametri di base della macchina virtuale WordPress. È normale che, a questo punto, alcune opzioni non siano ancora chiare. Tutte queste opzioni verranno illustrate in un modulo futuro. È consigliabile copiare i valori usati qui.

1. Usare i valori seguenti nella scheda **Base**.
    - La sottoscrizione deve essere impostata su _Concierge Subscription_.
    - Selezionare **Usa esistente** per l'area e quindi selezionare <rgn>[Nome gruppo di risorse sandbox]</rgn> nell'elenco a discesa.
    - Immettere un **nome** per la macchina virtuale. Usare _test-wp1-eus-vm_.
    - Selezionare un'**area** nelle vicinanze dall'elenco seguente.
        [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]
    - Scegliere _Nessuna_ per **Opzioni di disponibilità**. Questa impostazione è relativa alla disponibilità elevata.
    - L'**immagine** dovrà essere l'opzione _WordPress 4.9.7_ selezionata nel Marketplace.
    - Lasciare l'impostazione predefinita **A1 Standard** in _Dimensione_. Si otterranno così un singolo core e 1,75 GB di memoria, che dovrebbero essere sufficienti per un semplice sito Web.
    - Passare a **Password** come tipo di autenticazione e immettere un nome utente e una password.

    ![Screenshot della schermata per la creazione di una macchina virtuale con i dettagli inseriti](../media/3-create-vm-1.png)

1. Esistono diverse altre schede che è possibile esplorare per visualizzare le impostazioni da modificare durante la creazione della macchina virtuale. Al termine dell'esplorazione, fare clic su **Rivedi e crea** per esaminare e convalidare le impostazioni.

1. Nella schermata di revisione Azure convaliderà le impostazioni. Potrebbe essere necessario fornire alcune informazioni aggiuntive in base ai requisiti dell'autore dell'immagine. Verificare che tutte le impostazioni siano impostate nel modo desiderato e quindi fare clic su **Crea** per distribuire e creare la macchina virtuale.

1. È possibile monitorare la distribuzione tramite il pannello **Notifiche**. Fare clic sull'icona nella barra degli strumenti superiore per visualizzare o nascondere il pannello.

    ![Screenshot che mostra il monitoraggio dello stato della distribuzione](../media/3-deploying.png)

1. Il processo di distribuzione della macchina virtuale richiede alcuni minuti. Si riceverà una notifica che informa che la distribuzione ha avuto esito positivo. Fare clic sul pulsante **Vai alla risorsa** per passare alla pagina della panoramica della macchina virtuale.

    ![Screenshot che mostra la distribuzione completata dell'immagine WordPress](../media/3-deployment-succeeded.png)

1. Qui è possibile vedere tutte le informazioni e le opzioni di configurazione per la macchina virtuale WordPress appena creata. Una delle informazioni presenti è l'**indirizzo IP pubblico**.

    ![Screenshot della pagina della panoramica della macchina virtuale con l'indirizzo IP pubblico della macchina virtuale evidenziato.](../media/3-public-ip-address.png)

11. Copiare l'indirizzo IP, aprire una nuova scheda nel browser e incollarlo. Si passerà così a un nuovo sito WordPress.

    ![Screenshot del nuovo sito WordPress attivo](../media/3-my-new-blog.png)

La procedura è stata completata. Con pochi passaggi è stata distribuita una VM che esegue Linux, con un database installato e un sito Web funzionale. Verranno ora esaminati alcuni altri modi in cui avrebbe potuto essere creare una VM.
