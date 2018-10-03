In questa unità si creerà una funzione di Azure per visualizzare il nome e le dimensioni di un BLOB quando viene creato o aggiornato.

## <a name="create-a-blob-trigger"></a>Creare un trigger BLOB

Anche in questo caso, continuare a usare l'applicazione Funzioni di Azure esistente e aggiungere un trigger BLOB.

1. Assicurarsi di aver eseguito l'accesso al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato il sandbox.

1. Passare alla schermata **Tutte le risorse** e selezionare l'app per le funzioni.

1. Selezionare **Funzioni** e quindi l'icona del segno più (+).

1. Selezionare **Trigger BLOB**.

1. Lasciare **Nome** impostato sul valore predefinito.

1. Lasciare **Percorso** impostato sul valore predefinito.

1. Selezionare il collegamento _nuovo_ accanto all'elenco a discesa **Connessione dell'account di archiviazione**. Nel pannello che viene visualizzato fare clic su **Crea nuovo**. Immettere un nome univoco per il nuovo account di archiviazione e selezionare **OK** per creare l'account e chiudere il riquadro.

1. Nella schermata Nuova funzione che viene visualizzata di nuovo fare clic su **Crea** per creare la funzione.

## <a name="create-a-blob-container"></a>Creare un contenitore BLOB

Ora che è stato creato un trigger di BLOB, si userà Storage Explorer per creare un BLOB e attivare la funzione.

1. Aprire l'account di archiviazione usato (o creato) in una nuova scheda.

    > [!TIP]
    > È possibile duplicare una scheda nella maggior parte dei browser facendo clic con il pulsante destro del mouse sulla scheda in questione e selezionando **Duplica** dal menu visualizzato. Si sceglie di aprire una nuova scheda per poter passare da uno all'altro dei due servizi usati.

1. Nella barra laterale selezionare **Account di archiviazione** o **Tutte le risorse** e quindi applicare il filtro in base al nome.

1. Fare clic sulla sezione **Storage Explorer (anteprima)** per aprire un nuovo pannello in cui è possibile usare i BLOB e i file.

Tenere presente che il trigger del BLOB monitora solo la posizione descritta nel campo **Percorso**. Per impostazione predefinita, il percorso deve essere:

> samples-workitems/{name}

È necessario creare un contenitore denominato **samples-workitems**.

1. Fare clic con il pulsante destro del mouse su **CONTENITORI BLOB** e scegliere **Crea contenitore BLOB**.

1. Immettere **samples-workitems** come nome, lasciare l'impostazione predefinita **Privato** per il livello di accesso e selezionare **OK**.

## <a name="turn-on-your-blob-trigger"></a>Attivare il trigger del BLOB

Dopo che è stato creato il contenitore per il monitoraggio, è possibile eseguire la funzione per visualizzare l'output quando il BLOB viene creato.

1. Tornare alla scheda del browser con Funzione di Azure (o aprirla di nuovo).

1. Selezionare il trigger del BLOB per aprire la schermata di codice.

1. Aprire la scheda **Log** nella parte inferiore della schermata.

## <a name="create-a-blob"></a>Creare un BLOB

Il trigger del blob è ora attivo e in ascolto per l'attività. È possibile creare un BLOB per vedere se viene visualizzato un messaggio di registro.

1. Tornare alla scheda del browser con Storage Explorer.

1. In Storage Explorer selezionare il contenitore **samples-workitems** nell'elenco **CONTENITORI BLOB**.

1. Nella barra degli strumenti selezionare **Carica**.

1. Selezionare un file qualsiasi nel computer in uso.

1. In **Tipo di autenticazione** selezionare **SAS**.

1. Selezionare **Carica**.

1. Tornare alla scheda Funzioni di Azure e cercare nei log di output un messaggio che visualizza il file caricato.