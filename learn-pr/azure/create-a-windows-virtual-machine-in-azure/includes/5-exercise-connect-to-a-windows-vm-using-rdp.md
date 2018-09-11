Dallo scenario precedente, l'utente è uno studente che necessita di informazioni sull'aggiunta di ruoli e funzionalità a un computer Windows Server. L'amministratore di rete non vuole tuttavia che l'operazione venga eseguita in un computer fisico nella rete e i computer dell'istituto di istruzione non sono sufficientemente specificati per l'esecuzione di Windows Hyper-V. Azure offre la soluzione perfetta: è possibile ospitare le macchine virtuali Windows nel cloud ed è possibile accedervi usando Windows Remote Desktop Protocol (RDP). Usare il client RDP per connettersi alla VM di Windows creata nell'unità precedente. È possibile connettersi scaricando ed eseguendo un file con estensione **rdp** dal portale di Azure. 

Il file con estensione rdp contiene:

* L'indirizzo IP pubblico della VM.
* Il numero della porta.

## <a name="configure-network-and-public-ip-address-settings"></a>Configurare le impostazioni di rete e dell'indirizzo IP pubblico

1. Nel portale di Azure, verificare che il pannello per la macchina virtuale creata in precedenza sia aperto. Il pannello è disponibile in **Tutte le risorse** se è necessario aprirlo.

1. Passare alla sezione **Rete**. La parte superiore di questa sezione include collegamenti alla subnet virtuale e all'indirizzo IP dinamico creati con la macchina virtuale, dal momento che sono state usate le impostazioni predefinite. Verrebbero seguiti questi collegamenti se si intendesse modificare le risorse (ad esempio, passando a un indirizzo IP statico).

1. Fare clic su **Aggiungi regola porta in ingresso**.

1. Nella parte superiore della finestra di dialogo **Aggiungi regola di sicurezza in ingresso** fare clic su **Base**.

1. In **Servizio** selezionare **RDP**, quindi fare clic su **Aggiungi**.

## <a name="connect-to-the-vm-by-using-rdp"></a>Connettersi alla VM mediante RDP

Per scaricare il file RDP e connettersi alla VM, eseguire i passaggi seguenti.

### <a name="download-the-rdp-file"></a>Scaricare il file RDP

1. Nella sezione **Panoramica** del pannello della macchina virtuale fare clic su **Connetti**.

1. Nel pannello **Connetti a macchina virtuale** notare le impostazioni **Indirizzo IP** e **Numero di porta**, quindi fare clic su **Scarica file RDP**.

1. Nel browser fare clic su **Apri** oppure **Esegui** per aprire il file RDP.

### <a name="connect-to-the-windows-vm"></a>Connettersi alla VM di Windows

1. Nella finestra di dialogo **Connessione Desktop remoto** prendere nota dell'avviso di sicurezza e dell'indirizzo IP del computer remoto, quindi fare clic su **Connetti**.

1. Nella finestra di dialogo **Sicurezza di Windows** immettere il nome utente e la password usati nei passaggi 6 e 7.

1. Nella seconda finestra di dialogo **Connessione Desktop remoto** prendere nota degli errori di certificato e fare clic su **Sì**.

   > [!Note]
   > Il desktop della macchina virtuale richiede del tempo per essere visualizzato, poiché l'immagine B1 non è sufficientemente specificata. Si potrebbe ricevere un messaggio di allocazione di memoria insufficiente.

1. Nella finestra di dialogo **Reti** fare clic su **No**.

### <a name="resize-the-vm-in-the-azure-portal"></a>Ridimensionare la VM nel portale di Azure

1. Tornare al portale di Azure. Nella pagina delle proprietà della macchina virtuale, in **Impostazioni**, fare clic su **Dimensioni**.

1. Fare clic su **D2s_v3** (2 vCPU, RAM da 8 GB), quindi fare clic su **Seleziona**. Verrà visualizzato un messaggio di ridimensionamento della macchina virtuale. Verrà chiusa anche la VM della finestra RDP.

1. Tornare al portale di Azure. Nel riquadro sinistro fare clic su **Macchine virtuali**.

1. In **Macchine virtuali** attendere finché la VM non mostrerà lo stato **In esecuzione**. Potrebbe essere necessario fare clic su **Aggiorna**.

1. Fare clic sul nome della macchina virtuale, quindi in **Impostazioni** fare clic su **Dimensioni**. Le dimensioni della VM sono ora impostate su **D2s_v3**.

### <a name="reconnect-to-the-resized-vm"></a>Riconnettersi alla VM ridimensionata

1. Fare clic su **Macchine virtuali** e sulla VM. Il valore di **Indirizzo IP pubblico** è probabilmente cambiato. Fare clic su **Connetti**, quindi su **Esegui** oppure **Apri** nel browser.

1. Nella finestra di dialogo **Connessione Desktop remoto** prendere nota dell'avviso di sicurezza e dell'indirizzo IP del computer remoto, quindi fare clic su **Connetti**.

1. Nella finestra di dialogo **Sicurezza di Windows** immettere il nome utente e la password usati nei passaggi 6 e 7.

1. Nella seconda finestra di dialogo **Connessione Desktop remoto** prendere nota degli errori di certificato e fare clic su **Sì**. La macchina virtuale è ora molto più reattiva al processo di accesso.

1. Fare clic con il pulsante destro sulla barra delle applicazioni e scegliere **Gestione attività**.

1. Nella finestra **Gestione attività** fare clic su **Altri dettagli**.

1. Fare clic sulla scheda **Prestazioni**. La memoria totale disponibile è 8 GB, di cui circa 1,2 GB in uso. Chiudere **Gestione attività**.

## <a name="summary"></a>Riepilogo

È stata eseguita la connessione a una VM di Windows mediante RDP. Con l'accesso all'interfaccia utente del desktop è possibile amministrare questa VM come qualsiasi computer Windows.
