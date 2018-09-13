In questa unità si creerà una funzione di Azure per visualizzare il nome e le dimensioni di un BLOB quando viene creato o aggiornato. 

## <a name="create-a-blob-trigger"></a>Creare un trigger BLOB

Anche in questo caso, continuiamo a usare l'applicazione Funzioni di Azure esistente e aggiungiamo un trigger BLOB.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Selezionare **Funzioni**, quindi l'icona del segno più (+).

    ![Selezionare Funzioni, quindi il segno più](../media-drafts/4-hover-function.png)

1. Selezionare **Funzione personalizzata** e quindi **Trigger BLOB**.

1. Selezionare il linguaggio di programmazione **C#**. 

1. Lasciare **Nome** impostato sul valore predefinito.

1. Lasciare **Percorso** impostato sul valore predefinito.

1. Selezionare un account di archiviazione di Azure esistente oppure selezionare **Crea** se si vuole che Azure crei un nuovo account per l'utente.

## <a name="create-a-blob-container"></a>Creare un contenitore BLOB

Ora che è stato creato un trigger di BLOB, si userà Storage Explorer per creare un BLOB e attivare la funzione.

1. Aprire l'account di archiviazione usato (o creato) in una nuova scheda. Un modo semplice per farlo consiste nell'aprire una nuova scheda del portale di Azure e fare clic su **Account di archiviazione** nella barra laterale oppure usare **Tutti i servizi** nella barra laterale e quindi filtrare per nome. Si sceglie di aprire una nuova scheda per poter passare da uno all'altro dei due servizi usati.

1. Fare clic sulla sezione **Storage Explorer (anteprima)** per aprire un nuovo pannello in cui è possibile usare i BLOB e i file.

Tenere presente che il trigger del BLOB monitora solo la posizione descritta nel campo **Percorso**. Per impostazione predefinita, il percorso deve essere:

> samples-workitems/{name}

È necessario creare un contenitore denominato **samples-workitems**.

1. Fare clic con il pulsante destro del mouse su **CONTENITORI BLOB** e scegliere **Crea contenitore BLOB**.

1. Immettere **samples-workitems** come nome e lasciare l'impostazione predefinita **Privato** per il livello di accesso.

## <a name="turn-on-your-blob-trigger"></a>Attivare il trigger del BLOB

Ora che è stato creato il contenitore per il monitoraggio, è possibile eseguire la funzione per visualizzare l'output quando il BLOB viene creato.

1. Tornare alla scheda Funzione di Azure (o riaprirla).

1. Selezionare il trigger del BLOB per aprire la schermata di codice.

1. Selezionare **Esegui** per aprire la finestra di output.

## <a name="create-a-blob"></a>Creare un BLOB

Il trigger del blob è ora attivo e in ascolto per l'attività. È possibile creare un blob per vedere se viene visualizzato un messaggio di registro.

1. In Storage Explorer selezionare il contenitore **samples-workitems**.

1. Selezionare **Carica** dalla barra degli strumenti.

1. Selezionare i file dal computer.

1. Selezionare **Carica**.

1. Tornare alla scheda Funzione di Azure e cercare nei log di output un messaggio che visualizza il file caricato.

## <a name="pause-the-function"></a>Sospendere la funzione

Per assicurarsi che non verranno applicati costi per richieste aggiuntive, è possibile fare clic su **Sospendi** sopra la finestra del log.

![Sospendere la funzione](../media-drafts/4-pause-timer.png)
