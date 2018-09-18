In questo esercizio si presuppone che l'azienda esegua un server di posta elettronica SMTP (Simple Mail Transfer Protocol). Si vuole eseguire la migrazione di questo server in Azure. Si vuole che il server SMTP archivi i messaggi in ingresso per il proprio dominio in una cartella denominata "Drop" in un disco rigido virtuale dedicato.

L'obiettivo dell'esercizio è creare una macchina virtuale (VM) Windows e collegare un nuovo disco rigido virtuale (VHD) denominato "Incoming" per archiviare la directory "Drop".

## <a name="sign-in-to-azure"></a>Accedere ad Azure
<!---TODO: Need update for sanbox?--->

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

## <a name="create-a-windows-vm-in-the-azure-portal"></a>Creare una macchina virtuale Windows nel portale di Azure

Per creare una macchina virtuale per ospitare il server SMTP con le unità dati, seguire questa procedura:

1. Scegliere **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.

1. Nella casella di ricerca sopra l'elenco di risorse di Azure Marketplace cercare e selezionare **Windows Server 2016 Datacenter** e quindi scegliere **Crea**.

1. Nel riquadro **Generale** che viene visualizzato a destra immettere i valori delle proprietà seguenti. 


|Proprietà  |Valore  |Note  |
|---------|---------|---------|
|Nome     |   **MailSenderVM**      |         |
|Tipo di disco della macchina virtuale     |  **Unità disco rigido Standard**       |   Selezionare questo valore nell'elenco a discesa.      |
|Nome utente     |  **mailmaster**       |         |
|Password     |  La password deve contenere almeno 12 caratteri e soddisfare i [requisiti di complessità definiti](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).       | Assicurarsi di ricordare nome utente e password, che verranno usati in tutto il modulo.         |
|Sottoscrizione     |  Scegliere la propria sottoscrizione.       |  Selezionare questo valore nell'elenco a discesa.       |
|Gruppo di risorse     |  Selezionare **Crea nuovo** e quindi digitare **MailInfrastructure**.       |  Le risorse usate in questo modulo verranno raccolte in un unico gruppo di risorse.       |
|Località     |   Una località nelle vicinanze.      | Selezionare questo valore nell'elenco a discesa.        |

4. Selezionare **OK** nella parte inferiore della pagina **Generale** per passare al riquadro di configurazione **Dimensione**.

1. Nel riquadro di configurazione **Dimensione** cercare e selezionare **B1ms** e quindi fare clic su **Seleziona**.

1. Nel riquadro **Impostazioni**, in **Usa dischi gestiti** fare clic su **No**. I dischi gestiti verranno illustrati più avanti in questo modulo.

1. Nell'elenco a discesa **Selezionare le porte in ingresso pubbliche** selezionare **RDP (3389)**. Si userà questa porta per la connessione remota alla macchina virtuale dopo la creazione.

1. Lasciare i valori predefiniti per tutte le altre impostazioni e fare clic su **OK**.

1. Nel riquadro **Crea** esaminare la configurazione.

1. Una volta esaminata la configurazione, fare clic su **Crea**. Azure crea e avvia la nuova macchina virtuale.

> [!TIP]
> Per la creazione della macchina virtuale e la sua distribuzione in Azure possono essere necessari alcuni minuti. È possibile monitorare lo stato di avanzamento nell'hub **Notifiche**. Al termine, Azure visualizzerà una finestra di dialogo di notifica.

## <a name="add-an-empty-data-disk-to-our-vm"></a>Aggiungere un disco dati vuoto alla macchina virtuale

Al disco che archivia la directory "Drop" per il server SMTP verrà assegnato il nome "Incoming". Aggiungere un nuovo disco vuoto al server usando la procedura seguente:

1. Nel riquadro di navigazione a sinistra, in **PREFERITI** selezionare **Macchine virtuali**.

1. Nell'elenco di macchine virtuali selezionare **MailSenderVM**.

1. In **IMPOSTAZIONI** nel menu di configurazione di **MailSenderVM** a sinistra selezionare **Dischi**.

1. In **Dischi dati** selezionare **Aggiungi disco dati**.

1. Nel riquadro **Collega disco non gestito** impostare le proprietà seguenti.


|Proprietà  |Valore  |Note  |
|---------|---------|---------|
|Nome     |   **MailSenderVMIncoming**      |         |
|Tipo origine     |  **Nuovo (disco vuoto)**       |   Selezionare questo valore nell'elenco a discesa.       |
|Tipo di account     |  **Unità disco rigido Standard**       |  Selezionare questo valore nell'elenco a discesa.        |


6. A sinistra del campo **Contenitore di archiviazione** selezionare **Sfoglia**.

1. Nell'elenco di account di archiviazione cercare l'account di archiviazione il cui nome inizia con **mailinfrastructure** e selezionarlo.

1. Nell'elenco di contenitori fare clic su **vhd** e quindi scegliere **Seleziona**.

1. Tornare alla schermata **Collega disco non gestito** e selezionare **OK**.

1. Tornare alla schermata **MailSenderVM - Dischi** e selezionare **Salva**.

A questo punto è stato definito un disco denominato **MainSenderVMIncoming**. Per usare il disco, è necessario prima di tutto partizionarlo e formattarlo quando si accede alla macchina virtuale. 

## <a name="partition-and-format-a-data-disk"></a>Partizionare e formattare un disco dati

Come nel caso dei dischi fisici, è necessario inizializzare e formattare una partizione in un disco rigido virtuale prima di poterlo usare.

### <a name="log-into-our-windows-vm-using-rdp"></a>Accedere alla macchina virtuale Windows usando RDP

1. Nella schermata principale della macchina virtuale **MailSenderVM** selezionare **Panoramica**.

1. Selezionare **Connetti** in alto a sinistra nella schermata Panoramica.

1. Nella finestra di dialogo **Connect to virtual machine** (Connetti a macchina virtuale) visualizzata a destra selezionare **Scarica file RDP**.

   ![Screenshot della finestra di dialogo "Connect to virtual machine" (Connetti a macchina virtuale) con il pulsante "Scarica file RDP" evidenziato.](../media-draft/download-rdp.png)

4. Un file denominato **MailSenderVM.rdp** viene scaricato nella cartella `Downloads` locale. Si tratta del file di configurazione desktop remoto per la macchina virtuale MailSenderVM. Aprire il file per avviare il processo di connessione.

1. Nella finestra di dialogo **Connessione desktop remoto** fare clic su **Connetti**.

1. Nella finestra di dialogo **Sicurezza di Windows** fare clic su **Usa un altro account**.

1. Nella casella di testo **Nome utente** digitare **mailmaster**.

1. Nella casella di testo **Password** digitare la password immessa per il nome utente in questo esercizio. 

1. Nella finestra di dialogo **Connessione desktop remoto** fare clic su **Sì**.

Verrà avviata una sessione Desktop remoto alla macchina virtuale. Per il primo accesso, potrebbero essere necessari alcuni minuti. Una volta completato l'accesso, verrà visualizzato lo strumento **Server Manager**.

### <a name="partition-and-format-our-data-disk-using-server-manager"></a>Partizionare e formattare il disco dati tramite Server Manager

1. Quando viene visualizzato **Server Manager**, selezionare **Servizi file e archiviazione** nel riquadro di spostamento a sinistra.

1. In **Volumi** selezionare **Dischi**.

1. Nell'elenco di dischi il disco **0** è il disco del sistema operativo e il disco **1** è il disco temporaneo. Selezionare il disco **2**, che corrisponde al nuovo disco rigido virtuale appena aggiunto.

1. Nella parte superiore del riquadro **VOLUMI** selezionare **ATTIVITÀ** e quindi **Nuovo volume**. Il menu nella parte superiore destra della schermata è illustrato di seguito.

   ![Screenshot del menu "ATTIVITÀ" espanso per visualizzare tre voci di menu. Le voci di menu sono "Nuovo volume", "Nuova analisi archiviazione" e "Aggiorna".](../media-draft/tasks-menu.png)


1. Nella procedura guidata **Nuovo volume** fare clic su **Avanti**.

1. Nella pagina **Select server and disk** (Seleziona server e disco) selezionare **MailSenderVM** e **Disco 2** e quindi fare clic su **Avanti**.

1. Nella finestra di dialogo **Offline or Uninitiated Disk** (Disco offline o non inizializzato) fare clic su **OK**.

1. Nella pagina **Specifica dimensioni del volume** fare clic su **Avanti**.

1. Nella pagina **Assegnazione lettera di unità** selezionare **F:** e quindi **Avanti**.

1. Nella pagina **Selezionare le impostazioni del file system**, nella casella di testo **Etichetta volume** digitare **Incoming** e quindi selezionare **Avanti**.

1. Nella pagina **Conferma selezioni** selezionare **Crea**. Windows inizializza il disco e formatta la nuova partizione.

1. Nella pagina **Completamento** selezionare **Chiudi**.

Esaminare il nostro nuovo volume del disco in Esplora File. 
1. Aprire **Esplora file**.

1. Nel riquadro di spostamento a sinistra fare clic su **Questo PC** e quindi fare doppio clic su **Incoming (F:)**.

1. Selezionare **Home** e quindi **Nuova cartella**.

1. Digitare **Drop** e quindi premere INVIO.

È ora disponibile un nuovo volume creato nel disco rigido virtuale denominato **Incoming** ed è stata creata una cartella denominata **Drop** in tale volume.  

1. Chiudere Esplora risorse.

1. Sulla **barra della applicazioni** fare clic sul pulsante **Start**, quindi su **Arresta** e infine su **Disconnetti**.

La procedura è stata completata. È stata creata una macchina virtuale Windows, è stato collegato un nuovo disco rigido virtuale, è stato creato un volume nel disco rigido virtuale ed è stata aggiunta una cartella al volume. Per il nuovo disco rigido virtuale è stato usato il tipo **Unità disco rigido Standard**. Nell'unità successiva verranno illustrate le differenze tra Archiviazione Standard e Premium. 
