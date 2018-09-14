Tenere presente che l'obiettivo è spostare un server Linux esistente che esegue Apache in Azure. Per iniziare verrà creato un server Ubuntu Linux.

## <a name="create-a-new-linux-virtual-machine"></a>Creare una nuova macchina virtuale Linux

È possibile creare macchine virtuali Linux con il portale di Azure, il comando di Azure o Azure PowerShell. L'approccio più semplice quando si avvia con Azure è usare il portale, perché illustra le informazioni necessarie e fornisce suggerimenti e i messaggi utili durante la creazione:

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Fare clic su **crea una risorsa** nell'angolo superiore sinistro del portale di Azure.

1. Nella casella di ricerca immettere **Ubuntu Server** per visualizzare le diverse versioni disponibili. Selezionare **Ubuntu Server 18.04 LTS** dall'elenco presentato.

1. Fare clic sul pulsante **Crea** per avviare la configurazione della macchina virtuale.

## <a name="configure-the-vm-settings"></a>Configurare le impostazioni della macchina virtuale

L'esperienza di creazione della macchina virtuale nel portale viene visualizzato in un formato di procedura guidata illustra in dettaglio tutte le aree di configurazione per la macchina virtuale. Facendo clic sui **successivo** pulsante passerà alla prossima sezione configurabile. È comunque possibile spostarsi tra le sezioni nel modo preferito usando le schede nella parte superiore che identificano ogni parte.

![Screenshot del portale di Azure che illustra la creazione iniziale di un pannello della macchina virtuale per un computer Ubuntu Server.](../media/3-azure-portal-create-vm.png)

Dopo aver compilato tutte le opzioni necessarie (identificate da un asterisco rosso), è possibile ignorare il resto dell'esperienza della procedura guidata e iniziare a creare la macchina virtuale tramite il **revisione + Crea** nella parte inferiore.

Si inizierà con la sezione **Informazioni di base**.

### <a name="configure-basic-vm-settings"></a>Configurare le impostazioni di base della macchina virtuale

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Selezionare la **sottoscrizione** per cui devono essere fatturate le ore della macchina virtuale.

1. Per la **gruppo di risorse**, selezionare **Usa esistente** e scegliere il gruppo di risorse con il nome  **<rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>**.

    > [!NOTE]
    > Quando si modificano le impostazioni e uscirne ogni campo tramite tab, Azure convalidare ogni valore automaticamente e inserire un segno di spunta verde accanto a esso quando è consigliabile. È possibile passare il mouse sopra gli indicatori di errore per ottenere altre informazioni sui problemi individuati.

#### <a name="select-a-location"></a>Selezionare una località

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Nel **dettagli di istanze** sezione, immettere un nome per il server web, macchine Virtuali, ad esempio **test-web-eus-vm1**. Indica l'ambiente (**testare**), il ruolo (**web**), località (**Stati Uniti orientali**), service (**vm**) e l'istanza numero (**1**).
    - È consigliabile standardizzare i nomi delle risorse, pertanto è possibile identificare rapidamente il loro scopo. I nomi di macchine virtuali Linux devono avere una lunghezza compresa tra 1 e 64 caratteri ed essere composti da numeri, lettere e trattini.

1. Selezionare un'area nelle vicinanze. Assicurarsi di selezionare uno dei percorsi disponibili elencati in precedenza.

1. Lasciare **opzioni di disponibilità** come **None**. Questa opzione viene utilizzata per verificare che la macchina virtuale è a disponibilità elevata mediante il raggruppamento di più macchine virtuali come un set di affrontare pianificata o gli eventi di manutenzione non pianificati o interruzioni.

1. Assicurarsi che l'immagine è impostata su **Ubuntu Server 18.04 LTS**. È possibile aprire l'elenco a discesa per vedere tutte le opzioni disponibili.

1. Il **dimensioni** campo non è direttamente modificabile e ha una **DS2_v3** dimensioni predefinite, vale a dire una delle selezioni di calcolo per utilizzo generico. Questa scelta è perfetta per un server Web pubblico, ma fare clic sul collegamento **Modifica dimensioni** per esplorare altre dimensioni di macchina virtuale. Finestra di dialogo risultante consente di filtrare in base **& di CPU**, **Name**, e **tipo di disco**. Selezionare lo stesso **DS2_v3** scelta, che fornisce due Vcpu con 8 GB di RAM.

    > [!TIP]
    > È anche possibile far scorrere la visualizzazione verso sinistra per tornare alle impostazioni della macchina virtuale, dato che è stata aperta una nuova finestra a destra, e fare scorrere la finestra per visualizzarla.

1. Nella sezione **Accesso amministratore** selezionare l'opzione per la chiave pubblica SSH in **Tipo di autenticazione**.

1. Immettere un **nomeutente** che verrà usato per accedere con SSH.

1. Copiare la chiave SSH dal file di chiave pubblica e incollarla nel campo **Chiave pubblica SSH**.

> [!IMPORTANT]
> Quando si copia la chiave pubblica nel portale di Azure, assicurarsi di non aggiungere spazi o caratteri di avanzamento di riga.

1. Nel **regole porta in ingresso** sezione, aprire l'elenco e deselezionare **Nessuna porta in ingresso pubblica**. Trattandosi di una macchina virtuale Linux, è necessario avere la possibilità di accedere alla macchina virtuale con SSH da remoto. Scorrere l'elenco se necessario, finché non trovano **SSH (22)** e selezionarlo. Come indicato nella nota nell'interfaccia utente, è anche possibile modificare le porte di rete dopo aver creato la macchina virtuale.

    ![Screenshot del portale di Azure che mostra l'account amministratore e le impostazioni delle porte in ingresso, come descritto per aprirsi a una VM Linux per l'accesso SSH.](../media/3-open-ports.png)

## <a name="configure-disks-for-the-vm"></a>Configurare i dischi per la macchina virtuale

1. Fare clic su **successiva: dischi >** per spostare le **dischi** sezione.

    ![Screenshot del portale di Azure che mostra la sezione dischi della creazione di un pannello della macchina virtuale.](../media/3-configure-disks.png)

1. Scegli **Premium SSD** per il **tipo di disco del sistema operativo**.

1. Si useranno dischi gestiti per evitare di dover utilizzare account di archiviazione. Se lo si desidera, è possibile modificare l'opzione nella GUI per visualizzare le differenze per le informazioni richieste da Azure.

### <a name="create-a-data-disk"></a>Creare un disco dati

È importante ricordare che si otterrà un disco temporaneo (dev/sdb) e un disco del sistema operativo (dev/sda). È possibile aggiungere anche un disco dati:

1. Fare clic sul collegamento **Create and attach a new disk** (Crea e collega un nuovo disco) nella sezione **Dischi dati**.

    ![Screenshot del portale di Azure che illustra la creazione di un nuovo pannello del disco.](../media/3-add-data-disk.png)

1. È possibile eseguire tutte le impostazioni predefinite: unità SSD Premium, 1.023 GB, e **None** (disco vuoto), anche se si noti che qui è dove, utili uno snapshot o l'archiviazione Blob di Azure per creare un disco rigido virtuale.

1. Fare clic su **OK** per creare il disco e tornare alla sezione **DISCHI DATI**.

1. A questo punto, nella prima riga sarà presente un nuovo disco.

    ![Screenshot del portale di Azure che illustra la riga di disco dati appena creato per il processo di creazione della macchina virtuale.](../media/3-new-disk.png)

## <a name="configure-the-network"></a>Configurare la rete

1. Fare clic su **successivo: rete >** per spostare le **Networking** sezione.

1. In un sistema di produzione in cui sono già presenti altri componenti è consigliabile usare una rete virtuale _esistente_. In questo modo, la macchina virtuale può comunicare con altri servizi cloud nella nostra soluzione. Se non è già stata definita una rete virtuale per questa località, è possibile crearla e configurare gli elementi seguenti:
    - **Addressspace**: lo spazio IPV4 complessivo disponibile a questa rete.
    - **Intervallo di subnet**: la prima subnet per suddividere lo spazio degli indirizzi, è necessario che lo spazio di indirizzi definiti. Dopo aver creato la rete virtuale, è possibile aggiungere altre subnet.

> [!NOTE]
> Per impostazione predefinita, Azure creerà una rete virtuale, l'interfaccia di rete e l'indirizzo IP pubblico per la macchina virtuale. Non è facile per modificare le opzioni di rete dopo aver creata la macchina virtuale, quindi è sempre consigliabile verificare le assegnazioni di rete nei servizi creati in Azure.

## <a name="finish-configuring-the-vm-and-create-the-image"></a>Completare la configurazione della macchina virtuale e creare l'immagine

Le opzioni rimanenti hanno impostazioni predefinite ragionevoli che non è necessario modificare. Se lo si desidera, è possibile esplorare le altre schede. Le singole opzioni hanno un' `(i)` icona accanto a ciascun che mostra un suggerimento della Guida per spiegare l'opzione. Questo è un ottimo modo per apprendere le varie opzioni che è possibile usare per configurare la macchina virtuale:

1. Fare clic sul pulsante **Rivedi e crea** nella parte inferiore del pannello.

1. Il sistema convaliderà le opzioni e fornirà informazioni dettagliate sulla macchina virtuale in fase di creazione.

1. Fare clic su **Crea** per creare e distribuire la macchina virtuale. Il dashboard di Azure mostrerà la macchina virtuale da distribuire. Questa operazione può richiedere alcuni minuti.

Durante la distribuzione, si esaminerà che cosa si può fare con questa macchina virtuale.