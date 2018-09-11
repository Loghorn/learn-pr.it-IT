In questo esercizio si creerà un nuovo account di archiviazione nella sottoscrizione di Azure. Si userà quindi Azure Cloud Shell per creare una nuova coda, aggiungere un messaggio e quindi leggere il messaggio e rimuoverlo dalla coda.

Queste sono le stesse azioni eseguite dai componenti in un'applicazione distribuita. Ad esempio, un'app per dispositivi mobili può aggiungere un messaggio a una coda, in cui attende che un servizio Web lo recuperi e lo elabori.

## <a name="create-a-storage-account"></a>Creare un account di archiviazione

Poiché le code di archiviazione di Azure fanno parte degli account di archiviazione per utilizzo generico di Azure, è necessario iniziare creando un account di archiviazione:

1. In un browser passare al [portale di Azure](https://portal.azure.com?azure-portal=true) e accedere con le credenziali normali.

1. In alto a sinistra fare clic su **Tutti i servizi**.

1. Scorrere verso il basso fino alla sezione **Archiviazione** e quindi fare clic su **Account di archiviazione**.

1. In alto a sinistra nel pannello **Account di archiviazione** fare clic su **Aggiungi**.

  ![Screenshot del pannello Account di archiviazione con Aggiungi evidenziato](../media-draft/4-create-a-storage-account-1.png)

1. Nella finestra di dialogo risultante immettere le informazioni seguenti. Ognuna di queste opzioni ha un'icona `(i)` nel portale, che è possibile usare per ottenere altre informazioni sull'opzione.

    - Nella casella di testo **Nome** digitare un nome univoco per l'account di archiviazione.
    - In **Modello di distribuzione** assicurarsi che l'opzione **Resource Manager** sia selezionata.
    - Nell'elenco a discesa **Tipologia account** selezionare **Archiviazione (utilizzo generico v2)**.
    - Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.
    - Nell'elenco a discesa **Replica** selezionare **Archiviazione con ridondanza locale**.
    - In **Prestazioni** selezionare **Standard**.
    - In **Livello di accesso** selezionare **Accesso sporadico**.
    - In **Trasferimento sicuro obbligatorio** selezionare **Disabilitato**.
    - In **Sottoscrizione** selezionare la propria sottoscrizione.
    - In **Gruppo di risorse** selezionare **Crea nuovo**. Nella casella di testo digitare **MusicSharingResourceGroup**.
    - In **Reti virtuali** selezionare **Disabilitato**. 

    ![Screenshot della finestra di dialogo Crea account di archiviazione](../media-draft/4-create-a-storage-account-2.png)

1. Fare clic su **Crea**: Azure creerà un nuovo gruppo di risorse con un nuovo account di archiviazione associato.

    ![Screenshot della finestra di dialogo Crea account di archiviazione, con Crea evidenziato](../media-draft/4-create-a-storage-account-3.png)

## <a name="create-a-queue"></a>Creare una coda

Ora che è stato creato l'account di archiviazione, è possibile aggiungere una nuova coda. È necessario creare la coda usando i comandi di PowerShell:

1. In alto a destra nel portale fare clic sul collegamento **Cloud Shell** `(>_)`.

    ![Screenshot del portale di Azure con l'icona Cloud Shell evidenziata](../media-draft/4-create-a-storage-queue-1.png)

1. Nella schermata **Benvenuto in Azure Cloud Shell** fare clic su **PowerShell (Linux)**.

1. Se viene visualizzata la schermata **Non sono state montate risorse di archiviazione** fare clic su **Crea risorsa di archiviazione**.

1. Quando viene visualizzato il prompt `PS Azure`, per ottenere l'account di archiviazione, digitare il comando seguente. Sostituire `<storageaccountname>` con il nome univoco dell'account di archiviazione creato prima e quindi premere **INVIO**. Si vuole assegnare l'oggetto risultante a una variabile denominata `$storageaccount`.

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. È quindi necessario ottenere il _contesto_ dell'account di archiviazione, che è una proprietà per l'oggetto restituito. Verrà ora assegnato a un'altra variabile denominata `$context`.

    ```powershell
    $context = $storageaccount.Context
    ```

1. A questo punto è possibile creare la coda. Usare il comando `New-AzureStorageQueue` e assegnarla a una variabile `$messageQueue`.
    - Passare un parametro `-Name` con il valore `musicsharingmessages`
    - Passare un parametro `-Context` con il valore recuperato nel passaggio precedente.

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a>Aggiungere un messaggio alla coda

Dopo avere creato una coda nell'account di archiviazione, è possibile aggiungervi un messaggio.

1. Per creare un nuovo messaggio, usare il metodo `New-Object` per creare un elemento `CloudQueueMessage` .NET con un argomento basato su stringa:

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. Per aggiungere il nuovo messaggio alla nuova coda, passare l'elemento `CloudQueueMessage` creato al metodo `AddMessageAsync` nella coda `$messageQueue`.

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

## <a name="verify-the-message-was-queued"></a>Verificare che messaggio sia stato accodato

Quando si usano le code, è possibile usare **Storage Explorer**. Sono disponibili due varianti:

- Un'app desktop multipiattaforma per Linux, macOS e Windows che è possibile scaricare.
- Una versione Web di anteprima nel portale di Azure. Quest'ultima sarà quella usata qui, ma è possibile installare la versione desktop se si preferisce. Le istruzioni sono molto simili.

1. Fare clic su **Tutte le risorse** nel menu sul lato sinistro del portale di Azure.

1. Nell'elenco delle risorse fare clic sull'account di archiviazione creato in precedenza.

1. Nel pannello dell'account di archiviazione fare clic su **Storage Explorer (anteprima)**.

1. In Storage Explorer, sotto **Code**, fare clic su **musicsharingmessages**. Storage Explorer visualizzerà il messaggio appena aggiunto.

## <a name="retrieve-and-remove-the-message"></a>Recuperare e rimuovere il messaggio

Un componente di destinazione per un messaggio in una coda di archiviazione deve recuperare il messaggio all'inizio della coda. Il componente di destinazione deve quindi elaborare il messaggio ed eliminarlo dalla coda in modo che gli altri componenti non lo recuperino.

1. È possibile recuperare il primo messaggio disponibile in PowerShell usando il metodo `GetMessageAsync` sulla coda. Si tratta di un metodo .NET asincrono. Poiché lo si vuole attendere, è sufficiente usare la proprietà `Result` per ottenere il valore restituito. Viene restituito un oggetto che rappresenta il messaggio che è possibile assegnare a un parametro.

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. È possibile ottenere una versione testuale del messaggio chiamando `AsString`. In questo modo il valore verrà visualizzato nella console.

    ```powershell
    $retrievedMessage.AsString
    ```

1. In alternativa, è possibile visualizzare tutte le proprietà del messaggio digitando il nome della variabile e premendo **INVIO**.

    ```powershell
    $retrievedMessage
    ```

1. `GetMessageAsync` *non* rimuove il messaggio, semplicemente lo restituisce ed è quindi possibile elaborarlo nuovamente. Per rimuovere il messaggio dalla coda, è possibile usare il metodo `DeleteMessageAsync` sulla coda. A questo scopo, è necessario passare il messaggio che si vuole rimuovere.

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. Per verificare che il messaggio sia stato rimosso, aggiornare la visualizzazione della coda nel portale di Azure passando al pannello Account di archiviazione e selezionando **Panoramica> Storage Explorer**. In **Code** fare clic su **musicsharingmessages**. Storage Explorer mostrerà ora che la coda è vuota perché è stato rimosso l'unico messaggio.


## <a name="summary"></a>Riepilogo
Le code dell'account di archiviazione sono un'ottima soluzione quando si vogliono passare _messaggi_ tra i componenti di un'applicazione distribuita. Si tratta di un'ottima scelta se si vuole garantire il recapito o assicurarsi che i messaggi vengano recapitati nello stesso ordine in cui sono stato inviati. Le code implicano tuttavia che il mittente e il destinatario conoscano il formato dei dati passati. Esiste infatti tra di essi un contratto di dati implicito che aggiunge una sorta di "accoppiamento" tra i due servizi comunicanti.

Non tutte le architetture devono passare blocchi formattati di dati. Per alcune sono sufficienti messaggi semplici da gestire in base alla modalità fire-and-forget senza dover necessariamente conoscere come verrà gestito il messaggio. In questi scenari una coda non è la scelta ideale. Verrà ora esaminata un'altra strategia di messaggistica più adatta a questo stile di comunicazione.