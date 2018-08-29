In questo esercizio si creerà un nuovo account di archiviazione nella sottoscrizione di Azure. Si userà quindi Azure Cloud Shell per creare una nuova coda, aggiungere un messaggio e quindi leggere il messaggio e rimuoverlo dalla coda.

Queste sono le stesse azioni eseguite dai componenti in un'applicazione distribuita. Ad esempio, un'app per dispositivi mobili può aggiungere un messaggio a una coda, in cui attende che un servizio Web lo recuperi e lo elabori.

## <a name="create-a-storage-account"></a>Creare un account di archiviazione

Le code di archiviazione fanno parte degli account di archiviazione per utilizzo generico di Azure. È necessario iniziare creando un account di archiviazione:

1. In un browser passare al [portale di Azure](http://portal.azure.com) e accedere con le credenziali normali.
1. In alto a sinistra fare clic su **Tutti i servizi**.
1. Scorrere verso il basso fino alla sezione **Archiviazione** e quindi fare clic su **Account di archiviazione**.
1. In alto a sinistra nel pannello **Account di archiviazione** fare clic su **Aggiungi**.

    ![Creare un account di archiviazione](../images/5-create-a-storage-account-1.png)

1. Nella casella di testo **Nome** digitare un nome univoco per l'account di archiviazione.
1. In **Modello di distribuzione** assicurarsi che l'opzione **Resource Manager** sia selezionata.
1. Nell'elenco a discesa **Tipologia account** selezionare **Archiviazione (utilizzo generico v2)**.
1. Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.
1. Nell'elenco a discesa **Replica** selezionare **Archiviazione con ridondanza locale**.
1. In **Prestazioni** selezionare **Standard**.
1. In **Livello di accesso** selezionare **Accesso sporadico**.
1. In **Trasferimento sicuro obbligatorio** selezionare **Disabilitato**.
1. In **Sottoscrizione** selezionare la propria sottoscrizione.

    ![Creare un account di archiviazione](../images/5-create-a-storage-account-2.png)

1. In **Gruppo di risorse** selezionare **Crea nuovo** e quindi nella casella di testo digitare **MusicSharingResourceGroup**.
1. In **Reti virtuali** selezionare **Disabilitato** e quindi fare clic su **Crea**.

    ![Creare un account di archiviazione](../images/5-create-a-storage-account-3.png)

Azure crea il nuovo account di archiviazione e il nuovo gruppo di risorse.

## <a name="create-a-queue"></a>Creare una coda

Ora che è stato creato l'account di archiviazione, è possibile aggiungere una nuova coda. È necessario creare la coda usando i comandi di PowerShell:

1. In alto a destra nel portale fare clic sul collegamento **Cloud Shell**.

    ![Avviare Cloud Shell](../images/5-create-a-storage-queue-1.png)

1. Nella schermata **Benvenuto in Azure Cloud Shell** fare clic su **PowerShell (Linux)**.
1. Se viene visualizzata la schermata **You have no storage mounted** (Nessuna risorsa di archiviazione montata) fare clic su **Create storage** (Crea archiviazione).
1. Quando viene visualizzato il prompt `PS Azure`, per ottenere l'account di archiviazione digitare il comando seguente, sostituendo `<storageaccountname>` con il nome univoco dell'account di archiviazione e quindi premere INVIO:

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. Per ottenere il contesto dell'account di archiviazione, digitare il comando seguente e quindi premere INVIO:

    ```powershell
    $context = $storageaccount.Context
    ```

1. Per creare una nuova coda, digitare il comando seguente e quindi premere INVIO:

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a>Aggiungere un messaggio alla coda

Dopo avere creato una coda nell'account di archiviazione, è possibile aggiungervi un messaggio.

1. Per creare un nuovo messaggio, digitare il comando seguente e quindi premere INVIO:

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. Per aggiungere il nuovo messaggio alla nuova coda, digitare il comando seguente e quindi premere INVIO:

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

1. Fare clic su **Tutte le risorse** nel menu sul lato sinistro del portale di Azure.
1. Nell'elenco delle risorse fare clic sull'account di archiviazione creato in precedenza.
1. Nel pannello dell'account di archiviazione fare clic su **Storage Explorer (anteprima)**.
1. In Storage Explorer, sotto **Code**, fare clic su **musicsharingmessages**. Storage Explorer visualizza il messaggio appena aggiunto.

## <a name="retrieve-and-remove-the-message"></a>Recuperare e rimuovere il messaggio

Un componente di destinazione per un messaggio in una coda di archiviazione deve recuperare il messaggio all'inizio della coda, elaborarlo e quindi eliminarlo dalla coda in modo che non venga recuperato da altri componenti:

1. In Azure Cloud Shell, per ottenere il messaggio all'inizio della coda, digitare il comando seguente e quindi premere INVIO:

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. Per visualizzare il messaggio, digitare il comando seguente e quindi premere INVIO:

    ```powershell
    $retrievedMessage.AsString
    ```

1. Per visualizzare tutte le proprietà del messaggio, digitare il comando seguente e quindi premere INVIO:

    ```powershell
    $retrievedMessage
    ```

1. Per rimuovere il messaggio dalla coda, digitare il comando seguente e quindi premere INVIO:

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. Nel portale di Azure, per aggiornare la visualizzazione della coda, nel pannello Account di archiviazione fare clic su **Panoramica** e quindi fare clic su **Storage Explorer**.
1. In **Code** fare clic su **musicsharingmessages**. Storage Explorer mostra che la coda è vuota perché è stato rimosso l'unico messaggio.

## <a name="cleanup"></a>Pulizia

Per rimuovere tutte le risorse create nel corso dell'esercizio, immettere il comando seguente in Azure Cloud Shell 
```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```


## <a name="summary"></a>Summary

In questo esercizio è stato creato un account di archiviazione nella sottoscrizione di Azure ed è stata creata una nuova coda in esso. Si è anche usato PowerShell per simulare le azioni dei componenti dell'applicazione distribuita aggiungendo un messaggio alla coda per poi recuperarlo e rimuoverlo.

Le code dell'account di archiviazione di Azure sono un'ottima soluzione quando si vogliono passare messaggi tra i componenti di un'applicazione distribuita. Non scegliere le code di archiviazione quando si vogliono pubblicare eventi.