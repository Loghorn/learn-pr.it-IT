Tenere presente che l'obiettivo è spostare un server Linux esistente che esegue Apache in Azure. Per iniziare verrà creato un server Ubuntu Linux.

## <a name="create-a-new-linux-virtual-machine"></a>Creare una nuova macchina virtuale Linux

È possibile creare macchine virtuali Linux con il portale di Azure, l'interfaccia della riga di comando di Azure o Azure PowerShell. L'approccio più semplice quando si inizia a usare Azure prevede l'uso del portale, perché richiede le informazioni necessarie in sequenza e fornisce suggerimenti e messaggi utili durante la creazione:

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Fare clic su **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.

1. Nella casella di ricerca immettere **Ubuntu Server** per visualizzare le diverse versioni disponibili. Selezionare **Ubuntu Server 18.04 LTS** nell'elenco presentato.

1. Fare clic sul pulsante **Crea** per avviare la configurazione della macchina virtuale.

## <a name="configure-the-vm-settings"></a>Configurare le impostazioni della macchina virtuale

L'esperienza di creazione della macchina virtuale nel portale viene presentata sotto forma di procedura guidata, che offre tutte le informazioni dettagliate necessarie per le aree di configurazione della macchina virtuale. Facendo clic sul pulsante **Avanti** si passerà alla sezione configurabile successiva. È comunque possibile spostarsi tra le sezioni nel modo preferito usando le schede nella parte superiore che identificano ogni parte.

![Screenshot del portale di Azure con il pannello Crea macchina virtuale iniziale per un computer server Ubuntu.](../media/3-azure-portal-create-vm.png)

Dopo aver compilato tutte le opzioni obbligatorie identificate con asterischi rossi, è possibile ignorare il resto della procedura guidata e iniziare a creare la macchina virtuale tramite il pulsante **Rivedi e crea** nella parte inferiore.

Si inizierà con la sezione **Informazioni di base**.

### <a name="configure-basic-vm-settings"></a>Configurare le impostazioni di base della macchina virtuale

1. Per **sottoscrizione** dovrebbe essere selezionata per impostazione predefinita la sottoscrizione del sandbox.

1. Per **Gruppo di risorse** dovrebbe essere selezionato per impostazione predefinita il gruppo **<rgn>[nome gruppo di risorse sandbox]</rgn>**.

1. Nella sezione **DETTAGLI ISTANZA** immettere un nome per la macchina virtuale del server Web, ad esempio **test-web-eus-vm1**. Questo nome indica l'ambiente (**test**), il ruolo (**web**), la località (**eus** ossia Stati Uniti orientali), il servizio (**vm**) e il numero di istanza (**1**).
    - È consigliabile standardizzare i nomi delle risorse per poterne identificare rapidamente lo scopo. I nomi di macchine virtuali Linux devono avere una lunghezza compresa tra 1 e 64 caratteri ed essere composti da numeri, lettere e trattini.

    > [!NOTE]
    > Quando si modificano le impostazioni e si esce da ogni campo di testo libero, Azure convalida automaticamente ogni valore e visualizza un segno di spunta verde accanto se è corretto. È possibile passare il mouse sopra gli indicatori di errore per ottenere altre informazioni sui problemi individuati.

1. Selezionare una località.

    <!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. Per **Availability options** (Opzioni di disponibilità) lasciare selezionato **No infrastructure redundancy required** (Ridondanza infrastruttura non necessaria). Questa opzione viene usata per assicurarsi che la macchina virtuale sia a disponibilità elevata mediante il raggruppamento di più macchine virtuali come set per gestire gli eventi di manutenzione pianificata o non pianificata o le interruzioni.

1. Verificare che l'immagine sia impostata su **Ubuntu Server 18.04 LTS**. È possibile aprire l'elenco a discesa per vedere tutte le opzioni disponibili.

1. Il campo **Dimensioni** non è direttamente modificabile ed è impostato sulla dimensione predefinita **DS2_v3**, che è una delle selezioni di calcolo per utilizzo generico. Questa scelta è perfetta per un server Web pubblico, ma fare clic sul collegamento **Modifica dimensioni** per esplorare altre dimensioni di macchina virtuale. Nella finestra di dialogo risultante è possibile filtrare in base al **numero di vCPU**, al **nome** e al **tipo di disco**. Selezionare di nuovo **DS2_v3** che include due vCPU con 8 GB di RAM.

    > [!TIP]
    > È anche possibile far scorrere la visualizzazione verso sinistra per tornare alle impostazioni della macchina virtuale, dato che è stata aperta una nuova finestra a destra, e fare scorrere la finestra per visualizzarla.

1. Immettere un **nomeutente** che verrà usato per accedere con SSH.

1. Nella sezione **Accesso amministratore** selezionare l'opzione per la chiave pubblica SSH in **Tipo di autenticazione**.

1. Copiare la chiave SSH dal file di chiave pubblica e incollarla nel campo **Chiave pubblica SSH**.

    > [!IMPORTANT]
    > Quando si copia la chiave pubblica nel portale di Azure, assicurarsi di non aggiungere spazi o caratteri di avanzamento riga.

1. Nella sezione **REGOLE PORTA IN INGRESSO** aprire l'elenco e deselezionare **Non sono state trovate porte in ingresso pubbliche**. Trattandosi di una macchina virtuale Linux, è necessario avere la possibilità di accedere alla macchina virtuale con SSH da remoto. Scorrere l'elenco, se necessario, fino a trovare **SSH (22)** e selezionare questa opzione. Come indicato nella nota nell'interfaccia utente, è anche possibile modificare le porte di rete dopo aver creato la macchina virtuale.

    ![Screenshot del portale di Azure con l'account amministratore e le impostazioni delle porte in ingresso descritte per aprire una macchina virtuale Linux per l'accesso SSH.](../media/3-open-ports.png)

## <a name="configure-disks-for-the-vm"></a>Configurare i dischi per la macchina virtuale

1. Fare clic su **Avanti: Dischi >** per passare alla sezione **Dischi**.

    ![Screenshot del portale di Azure con la sezione Dischi del pannello Crea macchina virtuale.](../media/3-configure-disks.png)

1. Scegliere **SSD Premium** per **Tipo di disco del sistema operativo**.

1. Usare dischi gestiti, per non dover usare account di archiviazione. Se lo si desidera, è possibile modificare l'opzione nella GUI per visualizzare le differenze per le informazioni richieste da Azure.

### <a name="create-a-data-disk"></a>Creare un disco dati

È importante ricordare che si otterranno un disco del sistema operativo (dev/sda) e un disco temporaneo (dev/sdb). Aggiungere anche un disco dati:

1. Fare clic sul collegamento **Create and attach a new disk** (Crea e collega un nuovo disco) nella sezione **DISCHI DATI**.

    ![Screenshot del portale di Azure che mostra il pannello Create a new disk (Crea un nuovo disco).](../media/3-add-data-disk.png)

1. È possibile accettare tutte le impostazioni predefinite: SSD Premium, 1023 GB e **Nessuno** (disco vuoto). Si noti, tuttavia, che questa è la posizione in cui si potrebbe usare uno snapshot o un archivio BLOB di Azure per creare un disco rigido virtuale.

1. Fare clic su **OK** per creare il disco e tornare alla sezione **DISCHI DATI**.

1. A questo punto, nella prima riga sarà presente un nuovo disco.

    ![Screenshot del portale di Azure con la riga del disco dati appena creato per il processo di creazione della macchina virtuale.](../media/3-new-disk.png)

## <a name="configure-the-network"></a>Configurare la rete

1. Fare clic su **Avanti: Rete >** per passare alla sezione **Rete**.

1. In un sistema di produzione in cui sono già presenti altri componenti è consigliabile usare una rete virtuale _esistente_. In questo modo, la macchina virtuale può comunicare con altri servizi cloud nella soluzione. Se non è già stata definita una rete virtuale per questa località, è possibile crearla e configurare gli elementi seguenti:
    - **Spazio di indirizzi**: spazio IPV4 complessivo disponibile per questa rete.
    - **Intervallo IP subnet**: prima subnet in cui suddividere lo spazio di indirizzi. Deve rientrare nello spazio di indirizzi definito. Dopo aver creato la rete virtuale, è possibile aggiungere altre subnet.

> [!NOTE]
> Per impostazione predefinita, Azure creerà una rete virtuale, l'interfaccia di rete e l'indirizzo IP pubblico per la macchina virtuale. Poiché non è facile modificare le opzioni di rete dopo aver creato la macchina virtuale, controllare sempre con attenzione le assegnazioni di rete per i servizi creati in Azure.

## <a name="finish-configuring-the-vm-and-create-the-image"></a>Completare la configurazione della macchina virtuale e creare l'immagine

Le opzioni rimanenti hanno impostazioni predefinite ragionevoli che non è necessario modificare. Se lo si desidera, è possibile esplorare le altre schede. Accanto alle singole opzioni è disponibile un'icona `(i)` che permette di visualizzare un suggerimento con una spiegazione dell'opzione. Questo è un ottimo modo per ottenere informazioni sulle varie opzioni che è possibile usare per configurare la macchina virtuale:

1. Fare clic sul pulsante **Rivedi e crea** nella parte inferiore del pannello.

1. Il sistema convaliderà le opzioni e fornirà informazioni dettagliate sulla macchina virtuale in fase di creazione.

1. Fare clic su **Crea** per creare e distribuire la macchina virtuale. Il dashboard di Azure mostrerà la macchina virtuale da distribuire. Questa operazione può richiedere alcuni minuti.

Durante la distribuzione, si esaminerà che cosa si può fare con questa macchina virtuale.