È importante ricordare che l'azienda elabora contenuti video in macchine virtuali Windows. Una nuova città ha assunto l'azienda perché elabori le fotocamere del traffico, ma il modello non è mai stato usato prima dall'azienda. È necessario creare una nuova macchina virtuale Windows e installare alcuni codec proprietari in modo da poter iniziare a elaborare e ad analizzare le immagini.

## <a name="create-a-new-windows-virtual-machine"></a>Creare una macchina virtuale Windows

È possibile creare macchine virtuali Windows con il portale di Azure, l'interfaccia della riga di comando di Azure o Azure PowerShell. L'approccio più semplice consiste nell'usare il portale, perché offre tutte le informazioni dettagliate necessarie e fornisce suggerimenti e messaggi utili durante la creazione.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Fare clic su **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.

1. Nella casella di ricerca immettere **Windows Server 2016 Datacenter** e quindi fare clic sul collegamento con lo stesso titolo nell'elenco visualizzato.

1. Fare clic sul pulsante **Crea** per avviare la configurazione della macchina virtuale.

## <a name="configure-the-vm-settings"></a>Configurare le impostazioni della macchina virtuale

L'esperienza di creazione della macchina virtuale nel portale viene presentata sotto forma di "procedura guidata", che offre tutte le informazioni dettagliate necessarie per le aree di configurazione della macchina virtuale. Facendo clic sul pulsante "Avanti" si passerà alla sezione configurabile successiva. È comunque possibile spostarsi tra le sezioni nel modo preferito usando le schede nella parte superiore che identificano ogni sezione.

![Creare una macchina virtuale nel portale di Azure](../media-drafts/3-azure-portal-create-vm.png)

Dopo aver compilato tutte le opzioni obbligatorie (identificate con asterischi rossi), è possibile ignorare il resto della procedura guidata e iniziare a creare la macchina virtuale tramite il pulsante **Rivedi e crea** nella parte inferiore.

Si inizierà con la sezione **Informazioni di base**.

### <a name="configure-basic-vm-settings"></a>Configurare le impostazioni di base della macchina virtuale

1. Selezionare la **sottoscrizione** per cui devono essere fatturate le ore della macchina virtuale.

1. In **Gruppo di risorse** selezionare **Crea nuovo** e assegnare al gruppo di risorse il nome **ExerciseResources**.

> [!NOTE]
> Quando si modificano le impostazioni e si esce da ogni campo, Azure convalida automaticamente ogni valore e visualizza un segno di spunta verde accanto se è corretto. È possibile passare il puntatore del mouse sopra gli indicatori di errore per ottenere altre informazioni sui problemi individuati.

1. Nella sezione **DETTAGLI ISTANZA** immettere un nome per la macchina virtuale, ad esempio "vm2-pv-test" per "VM 2 processore video di test".
    - È consigliabile standardizzare i nomi delle risorse in modo da poterne identificare rapidamente lo scopo. I nomi delle macchine virtuali Windows devono rispettare un limite di bit ed essere composti da 2-15 caratteri costituiti da numeri, lettere e trattini.

1. Selezionare un'area nelle vicinanze.

1. Lasciare l'opzione **Availability options** (Opzioni di disponibilità) impostata su "Nessuna". Questa opzione viene usata per assicurarsi che la macchina virtuale sia a disponibilità elevata mediante il raggruppamento di più macchine virtuali tra loro in un set per gestire gli eventi di manutenzione pianificata o non pianificata o le interruzioni.

1. Assicurarsi che l'immagine sia impostata su "Windows Server 2016 Datacenter". È possibile aprire l'elenco a discesa per visualizzare tutte le opzioni disponibili.

1. Nel campo **Tipo di disco della macchina virtuale** fare clic sul menu a discesa per visualizzare le opzioni. Assicurarsi che sia selezionata l'opzione **SSD**.

1. Il campo **Dimensioni** non è direttamente modificabile ed è impostato sulla dimensione predefinita DS1. Fare clic sul collegamento **Modifica dimensioni** per esplorare altre dimensioni di macchina virtuale. Nella finestra di dialogo risultante è possibile filtrare in base al numero di CPU, al nome e al tipo di disco. Al termine, selezionare "DS1 Standard v2" (in genere l'impostazione predefinita). Questa impostazione assegna alla macchina virtuale una CPU e 3,5 GB di memoria.

    > [!TIP]
    > È anche possibile fare scorrere la visualizzazione a sinistra per tornare alle impostazioni della macchina virtuale come se venissero aperte in una nuova finestra a destra e fare scorrere la finestra per visualizzarla.

1. Nella sezione **ACCESSO AMMINISTRATORE** impostare il campo **Nome utente** sul nome utente che verrà usato per accedere alla macchina virtuale.

1. Nel campo **Password** immettere una password di almeno 12 caratteri. La password deve contenere almeno tre di questi elementi: un carattere minuscolo, un carattere maiuscolo, un numero e un carattere speciale diverso da "\'" o "-". Usare una password facile da ricordare o annotarla, perché servirà più avanti.

1. Confermare la **password**.

1. Nella sezione **REGOLE PORTA IN INGRESSO** aprire l'elenco e _deselezionare_ "Nessuna". Poiché si tratta di una macchina virtuale Windows, è necessario avere la possibilità di accedere al desktop tramite RDP. Se necessario, scorrere l'elenco fino a individuare RDP (3389) e selezionare questa opzione. Come indicato nella nota nell'interfaccia utente, è possibile modificare anche le porte di rete dopo aver creato la macchina virtuale.

    ![Aprire la porta per l'accesso RDP nella macchina virtuale Windows](../media-drafts/3-open-ports.png)

## <a name="configure-disks-for-the-vm"></a>Configurare i dischi per la macchina virtuale

1. Fare clic su **Avanti** per passare alla sezione Dischi.

    ![Configurare i dischi per la macchina virtuale](../media-drafts/3-configure-disks.png)

1. Scegliere "SSD Premium" per **Tipo di disco del sistema operativo**.

1. Usare dischi gestiti, in modo da non dover usare account di archiviazione. Se è necessario, è possibile modificare l'opzione nella GUI per visualizzare le differenze per le informazioni richieste da Azure.

### <a name="create-a-data-disk"></a>Creare un disco dati

Tenere presente che si otterrà un disco del sistema operativo (C:) e un disco temporaneo (D:). Aggiungere anche un disco dati.

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
    - **Intervallo IP subnet**: prima subnet in cui suddividere lo spazio di indirizzi. Deve rientrare nello spazio di indirizzi definito. Dopo aver creato la rete virtuale, è possibile aggiungere altre subnet.

1. Modificare gli intervalli predefiniti in modo da usare lo spazio di indirizzi IP `172.xxx`.
    - Modificare il campo **Spazio indirizzi** specificando `172.16.0.0/16` per assegnare l'intervallo completo di indirizzi
    - Modificare il campo **Intervallo IP subnet** specificando `172.16.1.0/24` per assegnare 256 indirizzi IP dello spazio.

> [!NOTE]
> Per impostazione predefinita, Azure creerà una rete virtuale, l'interfaccia di rete e l'indirizzo IP pubblico per la macchina virtuale. Poiché non è facile modificare le opzioni di rete dopo aver creato la macchina virtuale, controllare sempre con attenzione le assegnazioni di rete per i servizi creati in Azure.

## <a name="finish-configuring-the-vm-and-create-the-image"></a>Completare la configurazione della macchina virtuale e creare l'immagine

Le opzioni rimanenti hanno impostazioni predefinite ragionevoli che non è necessario modificare. Se lo si desidera, è possibile esplorare le altre schede. Accanto alle singole opzioni è disponibile un'icona `(i)` che permette di visualizzare una finestra della Guida con una spiegazione dell'opzione. Questo è un ottimo modo per ottenere informazioni sulle varie opzioni che è possibile usare per configurare la macchina virtuale.

1. Fare clic sul pulsante **Rivedi e crea** nella parte inferiore del pannello.

1. Il sistema convaliderà le opzioni e fornirà informazioni dettagliate sulla macchina virtuale in fase di creazione.

1. Fare clic su **Crea** per creare e distribuire la macchina virtuale. Il dashboard di Azure mostrerà la macchina virtuale da distribuire. Questa operazione può richiedere alcuni minuti.

Durante la distribuzione, si esaminerà che cosa si può fare con questa macchina virtuale.