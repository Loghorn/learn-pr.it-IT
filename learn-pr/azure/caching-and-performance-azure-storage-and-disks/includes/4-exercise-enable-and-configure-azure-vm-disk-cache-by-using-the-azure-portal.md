Si supponga di che esegue una foto di condivisione del sito, i dati archiviati in macchine virtuali di Azure (VM) che eseguono SQL Server e applicazioni personalizzate. Si vuole apportare le modifiche seguenti:

- È necessario modificare le impostazioni della cache del disco in una macchina virtuale.
- Si desidera aggiungere un nuovo disco dati alla macchina virtuale con la memorizzazione nella cache abilitata.

Si è deciso di apportare queste modifiche tramite il portale di Azure.

In questo esercizio illustra apportare le modifiche in una macchina virtuale che è descritti sopra. In primo luogo, è possibile accedere al portale e creare una macchina virtuale.

## <a name="sign-in-to-the-azure-portal"></a>Accedere al portale di Azure

[!include[](../../../includes/azure-sandbox-activate.md)]

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

## <a name="create-a-virtual-machine"></a>Creare una macchina virtuale

In questo passaggio, dobbiamo creare una VM con le proprietà seguenti:

|Proprietà  |Valore  |
|---------|---------|
|Image     |   **Windows Server 2016 Datacenter**      |
|Nome     |   **fotoshareVM**     |
|Gruppo di risorse     |   **<rgn>[Nome gruppo di risorse di tipo sandbox]</rgn>**      |

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Nel menu a sinistra del portale, selezionare **macchine virtuali**.

1. A questo punto selezionare **Aggiungi** in alto a sinistra il **macchine virtuali** schermata. Questa azione avvia il processo di creazione.

1. Nel **Compute** pannello che elenca immagini VM disponibili, digitare *Windows Server 2016 Datacenter* nella casella di ricerca.

1. Selezionare **Windows Server 2016 Datacenter** dai risultati della ricerca e quindi selezionare **crea** per avviare il processo di creazione della macchina virtuale.

1. Nel **nozioni di base** panel, verificare selezionato **sottoscrizione**.

1. Per **gruppo di risorse**, fare clic su **Crea nuovo** e specificare la **Name** di `fotoshare-rg` e fare clic su **OK**.

1. nel **nome della macchina virtuale** immettere `fotoshareVM`.

1. Sotto **gruppo di risorse**, selezionare **Usa esistente** e scegliere <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>.

1. Nel **posizione** elenco a discesa, selezionare un'area nell'elenco precedente.

1. Per la macchina virtuale **dimensioni**, è possibile lasciare l'impostazione predefinita consigliata o fare clic su **modificare dimensioni** per selezionare una dimensione diversa dal **Scegli una dimensione** pannello.

    > [!NOTE]
    > Non sono disponibili opzioni per configurare la cache del disco in questo momento, anche all'interno di **dischi** scheda del Pannello di creazione.

    > [!IMPORTANT]
    > Tenere presente che la cache del disco non può essere modificata per le macchine virtuali serie g e serie B. Sceglieremo ora una dimensione diversa.

1. Nella **ACCOUNT di amministratore** , quindi immettere un **Username** e **Password**/**Conferma password** per un account amministratore nella nuova VM.

1. L'immagine seguente è riportato un esempio di ciò che il **nozioni di base** simile di configurazione durante la compilazione. Lasciare i valori predefiniti per le schede e i campi rimanenti e fare clic su **revisione + Crea**.

    ![Screenshot del portale di Azure con la creazione di un pannello della macchina virtuale con alcune configurazioni di nozioni di base riportato di seguito viene compilato come descritto.](../media/4-basics-vm.png)

1. Dopo aver esaminato le nuove impostazioni della macchina virtuale, fare clic su **Create** per iniziare a distribuire la nuova macchina virtuale.

Creazione della macchina virtuale può richiedere un po' di tempo. Attendere il completamento della distribuzione della macchina virtuale prima di continuare con l'esercizio. Si riceverà un messaggio nell'hub di notifica quando il processo è completato.

> [!IMPORTANT]
> Si verrà usare questa macchina virtuale nella lezione successiva, quindi, rimanere disponibile per un periodo di tempo!

## <a name="view-os-disk-cache-status-in-the-portal"></a>Stato di visualizzazione del sistema operativo disco della cache nel portale di

Dopo aver distribuita la VM, è possibile verificare lo stato di memorizzazione nella cache del disco del sistema operativo usando la procedura seguente:

1. Nel menu a sinistra, fare clic su **tutte le risorse**, quindi selezionare la VM **fotoshareVM**.

1. Nel **macchina virtuale** pannello, in **impostazioni**, selezionare **dischi**.

1. Nel **dischi** riquadro, la macchina virtuale è un disco, il disco del sistema operativo. Il tipo di cache è attualmente impostato sul valore predefinito di **lettura/scrittura**.

![Screenshot del portale di Azure che mostra la sezione dischi del blade della macchina virtuale, con il disco del sistema operativo riportati e impostare per la memorizzazione nella cache di sola lettura.](../media/4-os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a>Modificare le impostazioni della cache del disco del sistema operativo nel portale

1. Nel **Disks** riquadro, selezionare **modifica** in alto a sinistra della schermata.

1. Modifica il **memorizzazione nella cache HOST** valore per il disco del sistema operativo **Read-only** usando l'elenco a discesa elenco e quindi selezionare **Salva** in alto a sinistra della schermata.

1. Questo aggiornamento può richiedere alcuni minuti. Il motivo è che la modifica l'impostazione della cache di un disco di Azure Scollega e ricollega il disco di destinazione. Se il disco del sistema operativo, la VM viene riavviata anche. Al termine dell'operazione, si riceverà una notifica che informa che i dischi delle macchine Virtuali sono stati aggiornati.

1. Al termine dell'operazione, il tipo di cache del disco del sistema operativo è impostato su **Read-only**.

È possibile passare alla configurazione della cache del disco dati. Per configurare un disco, è necessario prima crearne uno.

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a>Aggiungere un disco dati per la macchina virtuale e impostare la memorizzazione nella cache di tipo

1. Nella **Disks** vista della macchina virtuale nel portale andare avanti e selezionare **Aggiungi disco dati**. Viene immediatamente visualizzato un errore nel **nome** campo, indicando che il campo non può essere vuoto. Si dispone di un disco dati ancora non, creato appositamente.

1. Fare clic nell'elenco **Nome** e quindi su **Crea disco**.

1. Nel **creare un disco gestito** riquadro, nella **nome** , digitare **fotosharesVM-data**.

1. Sotto **gruppo di risorse**, selezionare **Usa esistente**e selezionare <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>.

1. Lasciare i campi rimanenti come predefiniti, quindi scegliere **Create** nella parte inferiore della schermata.

    Attendere il completamento della creazione del disco prima di continuare.

1. Modifica il **memorizzazione nella cache HOST** valore per il nuovo disco dati a **Read-only** usando l'elenco a discesa elenco e quindi selezionare **Salva** in alto a sinistra della schermata.

    Al termine dell'aggiornamento il nuovo disco dati della macchina virtuale di attesa. Al termine dell'operazione, il tipo di cache del disco dati verrà automaticamente impostato **Read-only**.

In questo esercizio viene usato il portale di Azure per configurare la memorizzazione nella cache in una nuova VM, modificare le impostazioni della cache in un disco esistente e configurare la memorizzazione nella cache in un nuovo disco dati. Lo screenshot seguente mostra la configurazione finale:

![Screenshot del portale di Azure che mostra il disco del sistema operativo e il nuovo disco dati nella sezione dei dischi del nostro pannello macchina virtuale, con entrambi i dischi impostati per la memorizzazione nella cache di sola lettura.](../media/disks-final-config-portal.PNG)
