Tenere presente che l'obiettivo è spostare un server Linux esistente che esegue Apache in Azure. Inizieremo creando un server Ubuntu Linux.

## <a name="create-a-new-linux-virtual-machine"></a>Creare una nuova macchina virtuale Linux

È possibile creare macchine virtuali Linux con il portale di Azure, l'interfaccia della riga di comando di Azure o Azure PowerShell. L'approccio più semplice quando si inizia a usare Azure prevede l'uso del portale, perché richiede le informazioni necessarie in sequenza e fornisce suggerimenti e messaggi utili durante la creazione.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Fare clic su **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.

1. Nella casella di ricerca immettere **Ubuntu Server** per visualizzare le diverse versioni disponibili. Selezionare **Ubuntu Server 18.04** nell'elenco presentato.

1. Fare clic sul pulsante **Crea** per avviare la configurazione della macchina virtuale.

## <a name="configure-the-vm-settings"></a>Configurare le impostazioni della macchina virtuale

L'esperienza di creazione della macchina virtuale nel portale viene presentata sotto forma di "procedura guidata", che offre tutte le informazioni dettagliate necessarie per le aree di configurazione della macchina virtuale. Facendo clic sul pulsante "Avanti" si passerà alla sezione configurabile successiva. È comunque possibile spostarsi tra le sezioni nel modo preferito usando le schede nella parte superiore che identificano ogni parte.

![Creare una macchina virtuale nel portale di Azure](../media-drafts/3-azure-portal-create-vm.png)

Dopo aver compilato tutte le opzioni obbligatorie identificate con asterischi rossi, è possibile ignorare il resto della procedura guidata e iniziare a creare la macchina virtuale tramite il pulsante **Rivedi e crea** nella parte inferiore.

Si inizierà con la sezione **Informazioni di base**.

### <a name="configure-basic-vm-settings"></a>Configurare le impostazioni di base della macchina virtuale

1. Selezionare la **sottoscrizione** per cui devono essere fatturate le ore della macchina virtuale.

1. In **Gruppo di risorse** selezionare **Crea nuovo** e assegnare al gruppo di risorse il nome **ExerciseResources**.

> [!NOTE]
> Quando si cambiano le impostazioni e si esce da ogni campo, Azure convaliderà automaticamente ogni valore e visualizzerà un segno di spunta verde accanto se è corretto. È possibile passare il mouse sopra gli indicatori di errore per ottenere altre informazioni sui problemi individuati.

1. Nella sezione **Dettagli istanza** immettere un nome per la macchina virtuale del server Web, ad esempio "test-web-eus-vm1". Questo nome indica l'ambiente ("test"), il ruolo ("web"), la località ("eus" ossia Stati Uniti orientali), il servizio ("vm") e il numero di istanza ("1").
    - È consigliabile standardizzare i nomi delle risorse in modo da poterne identificare rapidamente lo scopo. I nomi di macchine virtuali Linux devono avere una lunghezza compresa tra 1 e 64 caratteri ed essere composti da numeri, lettere e trattini.

1. Selezionare un'area vicina. Per l'esempio è stata scelta l'area Stati Uniti orientali.

1. Lasciare **Availability options** (Opzioni di disponibilità) impostato su "Nessuno". Questa opzione viene usata per assicurarsi che la macchina virtuale sia a disponibilità elevata mediante il raggruppamento di più macchine virtuali tra loro in un set per gestire gli eventi di manutenzione pianificata o non pianificata oppure le interruzioni.

1. Verificare che l'immagine sia impostata su "Ubuntu Server 18.04 LTS". È possibile aprire l'elenco a discesa per vedere tutte le opzioni disponibili.

1. Il campo **Dimensioni** non è direttamente modificabile ed è impostato sulla dimensione predefinita **DS2_v3**, che è una delle selezioni di calcolo per l'utilizzo generico. Questa scelta è perfetta per un server Web pubblico, ma fare clic sul collegamento **Modifica dimensioni** per esplorare altre dimensioni di macchina virtuale. Nella finestra di dialogo risultante è possibile filtrare in base al numero di CPU, al nome e al tipo di disco. Selezionare di nuovo **DS2_v3** che include due vCPU con 8 GB di RAM.

    > [!TIP]
    > È anche possibile far scorrere la visualizzazione verso sinistra per tornare alle impostazioni della macchina virtuale, dato che è stata aperta una nuova finestra a destra, e fare scorrere la finestra per visualizzarla.

1. Nella sezione **Accesso amministratore** selezionare l'opzione per la chiave pubblica SSH in **Tipo di autenticazione**.

1. Immettere un **nomeutente** che verrà usato per accedere con SSH.

1. Copiare la chiave SSH dal file di chiave pubblica e incollarla nel campo **Chiave pubblica SSH**.

> [!IMPORTANT]
> Quando si copia la chiave pubblica nel portale di Azure, assicurarsi di non aggiungere spazi o caratteri di avanzamento riga.

1. Nella sezione **Regole porta in ingresso** aprire l'elenco e _deselezionare_ "Nessuna". Trattandosi di una macchina virtuale Linux, è necessario avere la possibilità di accedere alla macchina virtuale con SSH da remoto. Scorrere l'elenco, se necessario, fino a individuare ssh (22) e selezionare questa opzione. Come indicato nella nota nell'interfaccia utente, è possibile modificare anche le porte di rete dopo aver creato la macchina virtuale.

    ![Aprire la porta per l'accesso SSH nella macchina virtuale Linux](../media-drafts/3-open-ports.png)

## <a name="configure-disks-for-the-vm"></a>Configurare i dischi per la macchina virtuale

1. Fare clic su **Avanti** per passare alla sezione Dischi.

    ![Configurare i dischi per la macchina virtuale](../media-drafts/3-configure-disks.png)

1. Scegliere "SSD Premium" per **Tipo di disco del sistema operativo**.

1. Si useranno dischi gestiti per evitare di dover utilizzare account di archiviazione. Se lo si desidera, è possibile modificare l'opzione nella GUI per visualizzare le differenze per le informazioni richieste da Azure.

### <a name="create-a-data-disk"></a>Creare un disco dati

È importante ricordare che si otterrà un disco del sistema operativo (dev/sda) e un disco temporaneo (dev/sdb). Verrà aggiunto anche un disco dati.

1. Fare clic sul collegamento **Create and attach a new disk** (Crea e collega un nuovo disco) nella sezione **Dischi dati**.

    ![Creare un disco dati per la macchina virtuale nel portale](../media-drafts/3-add-data-disk.png)

1. È possibile accettare tutte le impostazioni predefinite: SSD Premium, 1023 GB e Nessuno (disco vuoto). Si noti, tuttavia, che questa è la posizione in cui si potrebbe usare uno snapshot o un BLOB di archiviazione per creare un disco rigido virtuale.

1. Fare clic su **OK** per creare il disco e tornare alla sezione **DISCHI DATI**.

1. A questo punto, nella prima riga sarà presente un nuovo disco.

    ![Nuovo disco nella macchina virtuale](../media-drafts/3-new-disk.png)

## <a name="configure-the-network"></a>Configurare la rete

1. Fare clic su **Avanti** per passare alla sezione Rete.

1. In un sistema di produzione in cui sono già presenti altri componenti è consigliabile usare una rete virtuale _esistente_. In questo modo, la macchina virtuale può comunicare con altri servizi cloud nella soluzione. Se non è già stata definita una rete virtuale per questa località, è possibile crearla e configurare gli elementi seguenti:
    - **Spazio di indirizzi**: spazio IPV4 complessivo disponibile per questa rete.
    - **Intervallo di subnet**: la prima subnet per suddividere lo spazio di indirizzi. Deve rientrare nello spazio di indirizzi definito. Dopo aver creato la rete virtuale, è possibile aggiungere altre subnet.

> [!NOTE]
> Per impostazione predefinita, Azure creerà una rete virtuale, l'interfaccia di rete e l'indirizzo IP pubblico per la macchina virtuale. Poiché non è facile modificare le opzioni di rete dopo aver creato la macchina virtuale, controllare sempre con attenzione le assegnazioni di rete per i servizi creati in Azure.

## <a name="finish-configuring-the-vm-and-create-the-image"></a>Completare la configurazione della macchina virtuale e creare l'immagine

Il resto delle opzioni hanno impostazioni predefinite ragionevoli e non è necessario modificarle. Se lo si desidera, è possibile esplorare le altre schede. Accanto alle singole opzioni è disponibile un'icona `(i)` che permette di visualizzare una finestra della Guida con una spiegazione dell'opzione. Questo è un ottimo modo per ottenere informazioni sulle varie opzioni che è possibile usare per configurare la macchina virtuale.

1. Fare clic sul pulsante **Rivedi e crea** nella parte inferiore del pannello.

1. Il sistema convaliderà le opzioni e fornirà informazioni dettagliate sulla macchina virtuale in fase di creazione.

1. Fare clic su **Crea** per creare e distribuire la macchina virtuale. Il dashboard di Azure mostrerà la macchina virtuale da distribuire. Questa operazione può richiedere alcuni minuti.

Durante la distribuzione, si esaminerà che cosa si può fare con questa macchina virtuale.