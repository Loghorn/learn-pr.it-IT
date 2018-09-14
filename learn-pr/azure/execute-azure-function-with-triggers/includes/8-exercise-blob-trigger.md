In questo esercizio, creeremo una funzione di Azure che consente di visualizzare il nome e le dimensioni di un blob quando viene creato o aggiornato. 

> [!NOTE]
> Per completare questo esercizio, assicurarsi di aver eseguito l'accesso al [portale di Azure](https://portal.azure.com/) con un account valido.

## <a name="create-a-blob-trigger"></a>Creare un trigger del BLOB

Anche in questo caso, continuiamo a usare l'applicazione di Funzioni di Azure esistente e aggiungiamo un trigger del blob.

1. Selezionare **Funzioni**, quindi l'icona del segno più (+).

    ![Selezionare Funzioni, quindi il segno più (+)](../media-drafts/4-hover-function.png)

1. Selezionare **Trigger del blob**.

1. Selezionare il linguaggio di programmazione **C#**. 

1. Lasciare **Nome** impostato sul valore predefinito.

1. Lasciare **Percorso** impostato sul valore predefinito.

1. Selezionare un account di archiviazione di Azure esistente oppure selezionare **Crea** se si desidera che Azure crei un nuovo account per l'utente.

## <a name="download-storage-explorer"></a>Scaricare Storage Explorer

Ora che abbiamo creato un trigger del blob, è possibile scaricare Storage Explorer grazie a cui creare facilmente un blob.

- Scaricare [Storage Explorer](http://storageexplorer.com).

## <a name="connect-to-your-azure-storage-account"></a>Collegare l'account di archiviazione di Azure

Ora abbiamo scaricato Storage Explorer. È possibile accedere usando le credenziali che sono state specificate.

1. In Storage Explorer selezionare l'icona del più (+) a sinistra.

1. Selezionare **Use a storage account name and key** (Usare un nome e una chiave dell'account di archiviazione).

1. Selezionare **Avanti**.

1. In Azure nel trigger del BLOB selezionare **Integrazione**.

1. Selezionare **Documentazione** per espandere la visualizzazione.

1. Digitare il **Nome dell'account** e la **Chiave dell'account**.

1. Tornare in Storage Explorer e incollarli in **Nome dell'account** e in **Chiave dell'account**.

1. Inserire il **Nome visualizzato**. Questo valore è il nome della connessione in Storage Explorer.

1. Selezionare **Avanti**.

1. Selezionare **Connessione**. 

## <a name="create-a-blob-container"></a>Creare un contenitore BLOB

Non siamo connessi all'account di archiviazione di Azure. Teniamo presente che il trigger del blob esegue solo il monitoraggio del percorso descritto nel campo **Percorso**. Per impostazione predefinita, il percorso deve essere:

> samples-workitems/{name}

È necessario creare un contenitore denominato **samples-workitems**.

1. In Storage Explorer, espandere l'account di archiviazione. Il nome deve essere il **Nome visualizzato** fornito durante il processo di connessione.

1. Fare clic con il pulsante destro del mouse su **Contenitori BLOB** e scegliere **Crea contenitore BLOB**.

1. Inserire **samples-workitems**.

## <a name="turn-on-your-blob-trigger"></a>Attivare il trigger del blob

Ora che abbiamo creato il contenitore per il monitoraggio, è possibile eseguire la funzione per vedere l'output quando il blob viene creato.

1. Selezionare il trigger del blob per aprire la schermata di codice.

1. Selezionare **Run** (Esegui).

## <a name="create-a-blob"></a>Creare un blob

Il trigger del blob è ora attivo e in ascolto per l'attività. È possibile creare un blob per vedere se viene visualizzato un messaggio di registro.

1. In Storage Explorer, selezionare il contenitore **samples-workitems**.

1. Selezionare **Carica**. 

1. Selezionare **Carica file**.

1. Selezionare i file dal computer.

1. Selezionare **Carica**.

1. Tornare ad Azure. Controllare i registri per trovare il messaggio che consente di visualizzare i file caricati.

## <a name="clean-up"></a>Eseguire la pulizia

Per assicurarsi che non sono previsti costi per questa funzione, selezionare **Sospendi** sopra la finestra del log.

![Sospendi](../media-drafts/4-pause-timer.png)


