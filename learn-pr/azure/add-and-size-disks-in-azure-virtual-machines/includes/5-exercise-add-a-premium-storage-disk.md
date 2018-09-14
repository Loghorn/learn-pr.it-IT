Se la macchina virtuale (VM) ospita un'applicazione a elevato utilizzo di dischi, è consigliabile usare archiviazione premium per dischi rigidi virtuali (VHD).

Ad esempio, si decide di aggiungere un disco rigido virtuale per l'archiviazione della posta in uscita sul server SMTP della macchina virtuale. Per ottimizzare le prestazioni del disco, si decide in archiviazione premium per la posta in uscita.

Aggiungere un'unità SSD premium alla macchina virtuale.

## <a name="sign-in-to-azure"></a>Accedere ad Azure

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

## <a name="create-a-premium-storage-account"></a>Creare un account di Archiviazione Premium

Archiviazione premium di tutti i dischi rigidi virtuali deve essere archiviati in un account di archiviazione premium. Seguire questi passaggi per creare un account di archiviazione premium.

1. Selezionare **gli account di archiviazione** sotto **Preferiti** dal menu a sinistra del portale.

1. Selezionare **+ Aggiungi** in alto a sinistra il **gli account di archiviazione** dello schermo.

1. Nel **creare account di archiviazione** riquadro che viene aperta, impostare le proprietà seguenti.

|Proprietà  |Valore  |Note  |
|---------|---------|---------|
|Nome     |    *Un nome univoco (vedere la nota)*     |   Questo nome deve essere univoco tra tutti i nomi account di archiviazione esistenti in Azure. Deve essere compreso tra 3 e 24 caratteri e può contenere solo lettere minuscole e numeri.      |
|Tipo di account     |  **Archiviazione (per utilizzo generico v1)**       |         |
|Posizione     |  *Selezionare la stessa località della macchina virtuale creata in precedenza*       |         |
|Replica     |   **Archiviazione con ridondanza locale (LRS)**      |  Selezionare questo valore dall'elenco a discesa. Si ricorderà, stiamo creando un account di archiviazione premium e archiviazione premium supporta solo la replica con ridondanza locale.       |
|Prestazioni     |  **Premium**       | Account di archiviazione Premium sono supportati da unità SSD e offrono prestazioni coerenti e bassa latenza.        |
|Gruppo di risorse     |  *Selezionare **Usa esistente** e quindi <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>*      |  Si vuole mantenere tutte le risorse sotto il gruppo di risorse stesso.       |

Quando è stato compilato questa finestra di dialogo, dovrebbe essere simile allo screenshot seguente. 

!["Creare account di archiviazione" finestra di dialogo che mostra tutte le proprietà impostate come indicato.](../media-draft/create-premium-sa.png)

1. Selezionare **Create** per avviare il processo di creazione di account di archiviazione. Questo processo può richiedere alcuni minuti per il completamento. 

1. Quando si riceve una notifica che la distribuzione del nuovo account di archiviazione è stata completata correttamente, selezionare **Aggiorna** nella risorsa di archiviazione elenco per visualizzare l'account di archiviazione premium è stato creato l'account. Prendere nota del nome di questo account, che verrà usato nel passaggio successivo.

## <a name="create-vhd-in-the-premium-storage-account"></a>Creare un disco rigido virtuale nell'account di archiviazione premium

A questo punto è possibile aggiungere un nuovo disco rigido virtuale alla macchina virtuale e specificare l'account di archiviazione premium come posizione. Seguire questa procedura:

1. Nel riquadro di spostamento a sinistra, sotto **Preferiti**, selezionare **macchine virtuali**.

1. Nell'elenco delle macchine virtuali, selezionare **MailSenderVM**.

1. Sotto **le impostazioni** delle **MailSenderVM** dal menu di configurazione a sinistra, seleziona **dischi**.

1. Sotto **dischi dati**, selezionare **Aggiungi disco dati**.

1. Nel **collegare i dischi non gestiti** riquadro, impostare le proprietà seguenti.


|Proprietà  |Valore  |Note  |
|---------|---------|---------|
|Nome     |   **MailSenderVMOutgoing**      |         |
|Tipo di origine     |  **Nuovo (disco vuoto)**       |   Selezionare questo valore dall'elenco a discesa.       |
|Tipo di account     |  **Unità SSD Premium**       |  Selezionare questo valore dall'elenco a discesa.        |

1. A sinistra del **contenitore di archiviazione** campi, selezionare **Sfoglia**.

1. Nell'elenco degli account di archiviazione, trovare e selezionare l'account di archiviazione premium creato in precedenza in questa unità. Il tipo della voce verrà elencato come **archiviazione con ridondanza locale Premium**.

1. Nell'elenco di contenitori, selezionare __+ contenitore__.

1. Nel **nuovo contenitore** riquadro, nella **nome** nella casella di testo, digitare **dischi rigidi virtuali** e quindi selezionare **OK**.

1. Nell'elenco di contenitori, selezionare **VHD** e quindi scegliere **seleziona**.

1. Nella **collega disco non gestito** riquadro, selezionare **OK**.

1. Nella **MailSenderVM - Disks** riquadro, selezionare **salvare**. Azure aggiunge il nuovo disco di archiviazione premium alla macchina virtuale.

Della macchina virtuale è un disco del sistema operativo, un disco standard e un disco basato su unità SSD premium.

> [!NOTE]
> Il nuovo disco deve essere inizializzato, partizionamento e formattazione prima che può archiviare i dati. Per evitare la ripetizione, questi passaggi sono stati omessi in questo esercizio. Se si desidera completare queste attività, completare i passaggi descritti nel [partizionare e formattare un disco dati](../3-exercise-add-data-disks-to-azure-virtual-machines.yml##partition-and-format-a-data-disk) sezione dell'esercizio precedente.
