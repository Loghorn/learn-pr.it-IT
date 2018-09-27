
Si supponga di eseguire un sito per la condivisione di foto, con dati archiviati in macchine virtuali di Azure che eseguono SQL Server e applicazioni personalizzate. Si vogliono apportare le modifiche seguenti:

- È necessario modificare le impostazioni della cache del disco in una macchina virtuale.
- Si vuole aggiungere un nuovo disco dati alla macchina virtuale con la memorizzazione nella cache abilitata.

Si è deciso di apportare queste modifiche tramite il portale di Azure.

In questo esercizio verrà illustrato come modificare una macchina virtuale secondo le modalità descritte in precedenza. Per prima cosa, accedere al portale e creare una macchina virtuale.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-virtual-machine"></a>Creare una macchina virtuale

In questo passaggio si creerà una macchina virtuale con le proprietà seguenti:

| Proprietà        | Valore   |
|-----------------|---------|
| Immagine           | **Windows Server 2016 Datacenter** |
| Nome            | **fotoshareVM** |
| Gruppo di risorse  |   **<rgn>[nome gruppo di risorse sandbox]</rgn>** |
| Località        | Vedere qui di seguito. |

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.

1. Selezionare **Creare una risorsa** nel menu sulla barra laterale a sinistra.

1. _VM Windows Server 2016_ deve essere nell'elenco degli elementi del Marketplace **Più comuni**. In caso contrario, provare a cercare "Windows Server 2016 DataCenter" usando la casella di ricerca in alto.

1. Selezionare la macchina virtuale Windows e fare clic su **Crea** per avviare il processo di creazione della macchina virtuale.

1. Nel pannello **Informazioni di base** verificare che la sottoscrizione selezionata in **Sottoscrizione** sia _Concierge Subscription_ (Sottoscrizione Concierge).

1. In **Gruppo di risorse** selezionare **Usa esistente** e scegliere _<rgn>[nome gruppo di risorse sandbox]</rgn>_.

1. Nella casella **Nome macchina virtuale** immettere _fotoshareVM_.

1. Nell'elenco a discesa **Località** selezionare l'area più vicina a quella in cui ci si trova nell'elenco seguente.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. Per **Dimensioni** per la macchina virtuale, il valore predefinito è **DS1 Standard v2**, che assegna una singola CPU e 3,5 GB di memoria. Si tratta di dimensioni appropriate per questo esempio.

1. Nella sezione **ACCOUNT AMMINISTRATORE** completare i campi **Nome utente** e **Password**/**Conferma password** per un account amministratore nella nuova macchina virtuale.

1. Di seguito è riportato un esempio di come appariranno le opzioni di configurazione nel pannello **Informazioni di base**. Mantenere le impostazioni predefinite per le altre schede e campi rimanenti e fare clic su **Rivedi e crea**.

    ![Screenshot del portale di Azure che mostra il pannello Crea macchina virtuale con alcune opzioni di configurazione di esempio in Informazioni di base, descritte sopra.](../media/4-basics-vm.png)

1. Dopo aver esaminato le nuove impostazioni della macchina virtuale, fare clic su **Crea** per iniziare a distribuire la nuova macchina virtuale.

La creazione della macchina virtuale può richiedere alcuni minuti per la creazione di tutte le varie risorse (archiviazione, interfaccia di rete e così via) per supportare la macchina virtuale. Attendere il completamento della distribuzione della macchina virtuale prima di continuare con l'esercizio.

## <a name="view-os-disk-cache-status-in-the-portal"></a>Visualizzare lo stato della cache del disco del sistema operativo nel portale

Dopo avere distribuito la macchina virtuale, è possibile verificare lo stato di memorizzazione nella cache del disco del sistema operativo usando la procedura seguente:

1. Selezionare la risorsa **fotoshareVM** per aprire i dettagli della macchina virtuale nel portale. In alternativa, è possibile fare clic su **Tutte le risorse** sulla barra laterale a sinistra e quindi selezionare la macchina virtuale **fotoshareVM**.

1. In **Impostazioni** selezionare **Dischi**.

1. Nel riquadro **Dischi** la macchina virtuale ha un disco, ovvero quello del sistema operativo. Il tipo di cache è attualmente impostato sul valore predefinito **Lettura/scrittura**.

![Screenshot del portale di Azure che mostra la sezione Dischi del pannello Macchina virtuale, con il disco del sistema operativo visualizzato e impostato per la memorizzazione nella cache in sola lettura.](../media/4-os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a>Modificare le impostazioni della cache del disco del sistema operativo nel portale

1. Nel riquadro **Dischi** selezionare **Modifica** in alto a sinistra nella schermata.

1. Modificare il valore **Memorizzazione nella cache dell'host** per il disco del sistema operativo impostandolo su **Sola lettura** usando l'elenco a discesa e quindi selezionare **Salva** in alto a sinistra nella schermata.

1. Questo aggiornamento può richiedere tempo. Il motivo è che la modifica dell'impostazione della cache di un disco di Azure scollega e ricollega il disco di destinazione. Se si tratta del disco del sistema operativo, anche la macchina virtuale viene riavviata. Al termine dell'operazione, si riceverà una notifica che indica che i dischi della macchina virtuale sono stati aggiornati.

1. Al termine dell'operazione, il tipo di cache del disco del sistema operativo è impostato su **Sola lettura**.

È ora possibile passare alla configurazione della cache del disco dati. Per configurare un disco, è prima necessario crearne uno.

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a>Aggiungere un disco dati alla macchina virtuale e impostare il tipo di memorizzazione nella cache

1. Di nuovo nella visualizzazione **Dischi** della macchina virtuale nel portale proseguire e fare clic su **Aggiungi disco dati**. Viene immediatamente visualizzato un errore nel campo **Nome** che indica che il campo non può essere vuoto. Poiché non è ancora presente un disco dati, è necessario crearne uno.

1. Fare clic nell'elenco **Nome** e quindi su **Crea disco**.

1. Nella casella **Nome** nel riquadro **Crea disco gestito** digitare **fotosharesVM-data**.

1. In **Gruppo di risorse** selezionare **Utilizza esistente** e scegliere _<rgn>[nome gruppo di risorse sandbox]</rgn>_.

1. Osservare le impostazioni predefinite per gli altri campi:
    - SSD Premium
    - Dimensioni di 1023 GB
    - Stessa località della macchina virtuale (non modificabile).
    - Limite per operazioni di I/O al secondo: 5000
    - Limite per velocità effettiva (MB/s): 200

1. Fare clic su **Crea** nella parte inferiore della schermata.

    Attendere il completamento della creazione del disco prima di continuare.

1. Modificare il valore di **MEMORIZZAZIONE NELLA CACHE DELL'HOST** per il nuovo disco dati impostandolo su **Sola lettura** usando l'elenco a discesa (potrebbe essere già impostato) e quindi fare clic su **Salva** in alto a sinistra nella schermata.

    Al termine che la macchina virtuale completi l'aggiornamento del nuovo disco dati. Al termine, sarà presente un nuovo disco dati nella macchina virtuale.

In questo esercizio è stato usato il portale di Azure per configurare la memorizzazione nella cache in una nuova macchina virtuale, modificare le impostazioni della cache in un disco esistente e configurare la memorizzazione nella cache in un nuovo disco dati. Lo screenshot seguente mostra la configurazione finale:

![Screenshot del portale di Azure che mostra il disco del sistema operativo e il nuovo disco dati nella sezione Disco del pannello Macchina virtuale, con entrambi i dischi impostati sulla memorizzazione nella cache in sola lettura.](../media/disks-final-config-portal.PNG)
