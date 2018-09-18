Si supponga di eseguire un sito per la condivisione di foto, con dati archiviati in macchine virtuali di Azure che eseguono SQL Server e applicazioni personalizzate. Si vogliono apportare le modifiche seguenti.

- È necessario modificare le impostazioni della cache del disco in una macchina virtuale
- Si vuole aggiungere un nuovo disco dati alla macchina virtuale con la memorizzazione nella cache abilitata

Si è deciso di apportare queste modifiche tramite il portale di Azure.

In questo esercizio verrà illustrato come modificare una macchina virtuale secondo le modalità descritte in precedenza. Per prima cosa, accedere al portale e creare una macchina virtuale.

## <a name="sign-in-to-the-azure-portal"></a>Accedere al portale di Azure
<!---TODO: Update for sandbox?--->

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

## <a name="create-a-virtual-machine"></a>Creare una macchina virtuale

In questo passaggio verrà creata una macchina virtuale con le proprietà seguenti.

|Proprietà  |Valore  |
|---------|---------|
|Immagine     |   **Windows Server 2016 Datacenter**      |
|Nome     |   **fotoshareVM**     |
|Gruppo di risorse     |   **fotoshare-rg**      |


1. Nel menu a sinistra del portale selezionare **Macchine virtuali**.

1. Selezionare ora **+ Aggiungi** in alto a sinistra nella schermata **Macchine virtuali**. Questa azione avvia il processo di creazione.

1. Nel pannello Calcolo che elenca le immagini delle macchine virtuali disponibili digitare *Windows Server 2016 Datacenter* nella casella di ricerca.

1. Selezionare **Windows Server 2016 Datacenter** dai risultati della ricerca e quindi scegliere **Crea** per avviare il processo di creazione della macchina virtuale.

1. Nel pannello **Nozioni di base**, nella casella **Nome** immettere **fotoshareVM**.

1. Nelle caselle **Nome utente** e **Password** immettere un nome e una password per un account amministratore su questo server.

1. Nell'elenco a discesa **Sottoscrizione** selezionare la sottoscrizione di Azure.

1. In **Gruppo di risorse** selezionare **Crea nuovo** e digitare **fotoshare-rg** nella casella.

1. Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.

Di seguito è riportato un esempio di come dovrebbero essere le opzioni di configurazione nel pannello **Nozioni di base**.

![Screenshot delle opzioni di configurazione nel pannello Nozioni di base della macchina virtuale.](../media-draft/vm-basics-settings.PNG)

1. Scegliere **OK** per procedere al passaggio successivo.

È ora necessario scegliere una dimensione per la macchina virtuale e quindi avviare la distribuzione:

> [!IMPORTANT]
> Non dimenticare che la memorizzazione nella cache del disco non può essere modificata per le macchine virtuali serie L e serie B. Verrà scelta una dimensione diversa.

1. Nella sezione **Scegli una dimensione** selezionare uno SKU **Standard**, ad esempio **F1s**, e quindi scegliere **Seleziona**.

1. Scorrere verso il basso il riquadro **Impostazioni** e osservare che non è disponibile alcuna opzione per configurare la memorizzazione nella cache del disco. Accettare le impostazioni predefinite in questo passaggio e scegliere **OK** nella parte inferiore del riquadro.

1. Nel pannello **Crea** esaminare il riepilogo e quindi scegliere **Crea**.

1. La creazione della macchina virtuale può richiedere tempo. Attendere il completamento della distribuzione della macchina virtuale prima di continuare con l'esercizio. Al termine del processo verrà visualizzato un messaggio nell'hub di notifica.

> [!IMPORTANT]
> Questa macchina virtuale verrà usata nella lezione successiva, pertanto non eliminarla.

## <a name="view-os-disk-cache-status-in-the-portal"></a>Visualizzare lo stato della cache del disco del sistema operativo nel portale

Dopo avere distribuito la macchina virtuale, è possibile verificare lo stato di memorizzazione nella cache del disco del sistema operativo usando la procedura seguente.

1. Nel menu a sinistra fare clic su **Tutte le risorse** e quindi selezionare la macchina virtuale **fotoshareVM**.

1. Nella schermata **Macchina virtuale**, in **IMPOSTAZIONI** selezionare **Dischi**.

1. Nel riquadro **Dischi** la macchina virtuale ha un disco, ovvero quello del sistema operativo. Il tipo di cache è attualmente impostato sul valore predefinito di **Lettura/scrittura**.

![Screenshot dei dischi dati e del sistema operativo, entrambi impostati per la memorizzazione nella cache di sola lettura.](../media-draft/os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a>Modificare le impostazioni della cache del disco del sistema operativo nel portale

1. Nel riquadro **Dischi** selezionare **Modifica** in alto a sinistra nella schermata.

1. Modificare il valore **Memorizzazione nella cache dell'host** per il disco del sistema operativo impostandolo su **Sola lettura** usando l'elenco a discesa e quindi selezionare **Salva** in alto a sinistra nella schermata.

1. Questo aggiornamento richiede tempo. Il motivo è che la modifica dell'impostazione della cache di un disco di Azure scollega e ricollega il disco di destinazione. Se si tratta del disco del sistema operativo, anche la macchina virtuale viene riavviata. Al termine dell'aggiornamento verrà visualizzato un messaggio simile alla notifica seguente.

![Esempio di notifica visualizzata al termine dell'aggiornamento dell'impostazione della cache.](../media-draft/vm-disk-update-complete.PNG)

4. Al termine dell'operazione, il tipo di cache del disco del sistema operativo è impostato su **Sola lettura**.

È ora possibile passare alla configurazione della cache del disco dati. Per configurare un disco, è prima necessario crearne uno.

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a>Aggiungere un disco dati alla macchina virtuale e impostare il tipo di memorizzazione nella cache

1. Nella visualizzazione **Dischi** della macchina virtuale nel portale proseguire e selezionare **Aggiungi disco dati**. Viene immediatamente visualizzato un errore nel campo **Nome** che indica che il campo non può essere vuoto. Non si dispone ancora di un disco dati, pertanto è necessario crearne uno.

1. Fare clic nell'elenco **Nome** e quindi su **Crea disco**.

1. Nel riquadro **Crea disco gestito**, nella casella **Nome** digitare **fotosharesVM-data**.

1. In **Gruppo di risorse** selezionare **Usa esistente** e quindi selezionare **fotoshare-rg** dal menu a discesa.

1. Scegliere **Crea** nella parte inferiore della schermata.

1. Attendere il completamento della creazione del disco prima di continuare.

1. Modificare il valore **Memorizzazione nella cache dell'host** per il nuovo disco dati impostandolo su **Sola lettura** usando l'elenco a discesa e quindi selezionare **Salva** in alto a sinistra nella schermata.

1. Attendere l'aggiornamento della macchina virtuale. L'aggiornamento richiede tempo, poiché Azure scollega e ricollega il disco dati per modificare questa impostazione.

1. Al termine dell'operazione, il tipo di cache del disco dati è impostato su **Sola lettura**.

In questo esercizio è stato usato il portale di Azure per configurare la memorizzazione nella cache in una nuova macchina virtuale, modificare le impostazioni della cache in un disco esistente e configurare la memorizzazione nella cache in un nuovo disco dati. Lo screenshot seguente illustra la configurazione finale. 

![Screenshot dei dischi dati e del sistema operativo, entrambi impostati per la memorizzazione nella cache di sola lettura.](../media-draft/disks-final-config-portal.PNG)