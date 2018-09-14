In questo esercizio, si supponga che l'azienda esegue un server di posta elettronica Simple Mail Transfer Protocol (SMTP). Si vuole eseguire la migrazione di questo server in Azure. Si vuole che il server SMTP per archiviare messaggi in ingresso per il proprio dominio in una cartella denominata "Drop" in un disco rigido virtuale dedicato.

L'obiettivo dell'esercizio consiste nel creare una macchina virtuale di Windows (VM) e collegare un nuovo disco rigido virtuale (VHD) denominato "In ingresso" per archiviare la directory di "Ricezione".

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

## <a name="create-a-windows-vm-in-the-azure-portal"></a>Creare una macchina virtuale Windows nel portale di Azure

Per creare una macchina virtuale per ospitare il server SMTP con le unità dati, seguire questa procedura:

1. Scegli **crea una risorsa** nell'angolo superiore sinistro del portale di Azure.

1. Nella casella di ricerca sopra l'elenco delle risorse di Azure Marketplace, cercare e selezionare **Windows Server 2016 Datacenter**, quindi scegliere **crea**.

1. Nel **nozioni di base** riquadro che viene visualizzato a destra, immettere i valori delle proprietà seguenti. 


|Proprietà  |Valore  |Note  |
|---------|---------|---------|
|Nome     |   **MailSenderVM**      |         |
|Tipo di disco della macchina virtuale     |  **Unità disco rigido standard**       |   Selezionare questo valore dall'elenco a discesa.      |
|Nome utente     |  **mailmaster**       |         |
|Password     |  La password deve contenere almeno 12 caratteri e soddisfare i [requisiti di complessità definiti](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).       | Assicurarsi di ricordare questo nome utente e la password perché verranno usati in tutto il modulo.         |
|Sottoscrizione     |  Scegliere la propria sottoscrizione.       |  Selezionare questo valore dall'elenco a discesa.       |
|Gruppo di risorse     |  Selezionare **Usa esistente** e scegliere <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>.       |  È possibile ottenere tutte le risorse utilizzate in questo modulo in un unico gruppo di risorse.       |
|Posizione     |   Una località vicino a te.      | Selezionare questo valore dall'elenco a discesa.        |

4. Selezionare **OK** in fondo il **nozioni di base** pagina per continuare al **dimensioni** riquadro di configurazione.

1. Nel **dimensioni** riquadro di configurazione, cercare e selezionare **B1ms**, quindi fare clic su **selezionare**.

1. Nel **impostazioni** riquadro lascia la maggior parte dei valori impostate sui valori predefiniti. Sotto **Usa dischi gestiti**, fare clic su **No**. Illustreremo i dischi gestiti più avanti in questo modulo.

1. Nel **selezionare le porte in ingresso pubbliche** elenco a discesa, selezionare **RDP (3389)**. Si userà questa porta in remoto la macchina virtuale dopo averla creata.

1. Lasciare tutte le altre impostazioni in base all'impostazione predefinita e quindi fare clic su **OK**.

1. Nel **Create** riquadro, esaminare la configurazione.

1. Dopo avere esaminato la configurazione, selezionare **Create**. Azure crea e avvia la nuova macchina virtuale.

> [!TIP]
> Creazione di una macchina virtuale e la distribuzione in Azure può richiedere alcuni minuti. È possibile controllare lo stato di avanzamento nel **notifiche** hub. Al termine, Azure visualizzerà una finestra di dialogo di notifica.

## <a name="add-an-empty-data-disk-to-our-vm"></a>Aggiungere un disco dati vuoto per la macchina virtuale

Siamo in corso per assegnare un nome disco archivia la directory "Drop" per il server SMTP "In ingresso". È possibile aggiungere un nuovo disco vuoto al server usando la procedura seguente:

1. Nel riquadro di spostamento a sinistra, sotto **Favorits**, selezionare **macchine virtuali**.

1. Nell'elenco delle macchine virtuali, selezionare **MailSenderVM**.

1. Sotto **le impostazioni** delle **MailSenderVM** dal menu di configurazione a sinistra, seleziona **dischi**.

1. Sotto **dischi dati**, selezionare **Aggiungi disco dati**.

1. Nel **collegare i dischi non gestiti** riquadro, impostare le proprietà seguenti.

|Proprietà  |Valore  |Note  |
|---------|---------|---------|
|Nome     |   **MailSenderVMIncoming**      |         |
|Tipo di origine     |  **Nuovo (disco vuoto)**       |   Selezionare questo valore dall'elenco a discesa.       |
|Tipo di account     |  **Unità disco rigido standard**       |  Selezionare questo valore dall'elenco a discesa.        |

6. A sinistra del **contenitore di archiviazione** campi, selezionare **Sfoglia**.

1. Nell'elenco degli account di archiviazione, cercare l'account di archiviazione il cui nome inizia con **mailinfrastructure** e selezionarlo.

1. Nell'elenco di contenitori, fare clic su **VHD** e quindi scegliere **seleziona**.

1. Nella **collega disco non gestito** schermata, seleziona **OK**.

1. Nella **MailSenderVM - Disks** schermata, seleziona **salvare**.

Abbiamo definito un disco denominato **MainSenderVMIncoming**. Per usare il disco, è necessario innanzitutto suddividere in partizioni e formattarlo quando si accede alla macchina virtuale.

## <a name="partition-and-format-a-data-disk"></a>Partizioni e formattare un disco dati

Come con i dischi fisici, avviare e formattare una partizione su un disco rigido virtuale prima che possa essere utilizzato.

### <a name="log-into-our-windows-vm-using-rdp"></a>Accedere a Windows VM tramite RDP

1. Nel **MailSenderVM** macchina virtuale schermata principale di select **Panoramica**.

1. Selezionare **Connect** dalla parte superiore sinistra della schermata Panoramica.

1. Nel **Connetti a macchina virtuale** finestra di dialogo che viene visualizzata a destra, selezionare **Scarica file RDP**.

   ![Screenshot della finestra di dialogo "Connetti a macchina virtuale" con il pulsante "Scarica file RDP" evidenziato.](../media-draft/download-rdp.png)

4. Un file denominato **MailSenderVM.rdp** viene scaricato nel computer locale `Downloads` cartella. Questo file è il file di configurazione di desktop remoto per la macchina virtuale MailSenderVM. Aprire il file per avviare il processo di connessione.

1. Nel **connessione Desktop remoto** finestra di dialogo, fare clic su **Connect**.

1. Nella finestra di dialogo **Protezione di Windows** fare clic su **Usa un altro account**.

1. Nel **nomeutente** nella casella di testo, digitare **mailmaster**.

1. Nel **Password** nella casella di testo, digitare la password immessa per il nome utente in questo esercizio. 

1. Nel **connessione Desktop remoto** finestra di dialogo, fare clic su **Yes**.

A questo punto viene avviata una sessione desktop remoto alla macchina virtuale. Potrebbe richiedere alcuni istanti per la prima volta. Al termine Accedi, il **Server Manager** strumento verrà visualizzato sullo schermo.

### <a name="partition-and-format-our-data-disk-using-server-manager"></a>Creare partizioni e formattare il disco dati tramite Server Manager

1. Quando **Server Manager** è visualizzato, selezionare **servizi File e archiviazione** nel riquadro di spostamento a sinistra.

1. Sotto **volumi**, selezionare **dischi**.

1. Nell'elenco dei dischi, del disco **0** è il sistema operativo disco e del disco **1** è il disco temporaneo. Disco selezionare **2**, ovvero il nuovo disco rigido virtuale appena aggiunto.

1. In cima il **volumi** riquadro, selezionare **attività** seguita da **nuovo Volume...** . Questo sarà disponibile nella parte superiore destra della schermata come indicato di seguito.

   ![Screenshot del menu "Attività" espansa per visualizzare tre voci di menu. Sono "Nuovo Volume in corso", "Ripeti analisi archiviazione" e "Aggiorna".](../media-draft/tasks-menu.png)


1. Nel **nuovo Volume** procedura guidata, fare clic su **successivo**.

1. Nel **selezionare server e il disco** pagina, selezionare **MailSenderVM** e **disco 2**, quindi fare clic su **Avanti**.

1. Nel **Offline o senza inizializzazione disco** finestra di dialogo, fare clic su **OK**.

1. Nel **specificare le dimensioni del volume** pagina, fare clic su **successivo**.

1. Nel **assegnare una lettera di unità** pagina, selezionare **f:** aggiungendo **Avanti**.

1. Nel **selezionare le impostazioni file system** nella pagina il **etichetta di Volume** digitare **in ingresso** e quindi selezionare **Avanti**.

1. Nel **confermare le selezioni** pagina, selezionare **crea**. Windows avvia il disco e formatta la nuova partizione.

1. Nel **completamento** pagina, selezionare **Chiudi**.

È possibile avere un'occhiata al nostro nuovo volume del disco in Esplora File.

1. Aprire **Esplora File**.

1. Nel riquadro di spostamento a sinistra, fare clic su **PC This** e quindi fare doppio clic su **in ingresso (f)**.

1. Selezionare **casa**e quindi **nuova cartella**.

1. Tipo di **Drop** e quindi premere INVIO.

È ora disponibile un nuovo volume creato il disco rigido virtuale denominato **in ingresso**, e abbiamo creato una cartella denominata **Drop** in tale volume.  

1. Chiudere Windows Explorer.

1. Nel **sulla barra delle applicazioni**, fare clic sul **avviare** e fare clic il **Power** pulsante e quindi fare clic su **Disconnect**.

La procedura è stata completata. Aver correttamente creato una macchina virtuale Windows, un nuovo disco rigido virtuale collegato, creato un volume nel disco rigido virtuale e aggiunto una cartella al volume specifico. Si ricorderà, il tipo di disco è stato usato per il nuovo disco rigido virtuale è stata una **unità disco rigido Standard**. L'unità successiva, si illustra le differenze tra archiviazione standard e premium. 
