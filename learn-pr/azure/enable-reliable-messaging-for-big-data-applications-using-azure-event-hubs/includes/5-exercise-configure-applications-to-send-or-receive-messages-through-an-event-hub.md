Ora è possibile configurare le applicazioni autore e consumer per l'hub eventi.

In questo esercizio verranno configurate le applicazioni per l'invio o la ricezione dei messaggi tramite l'hub eventi. Queste applicazioni sono archiviate in un repository GitHub.

Verranno configurate due applicazioni distinte: una come mittente del messaggio (**SimpleSend**), l'altra come ricevitore del messaggio (**EventProcessorSample**). Si tratta di applicazioni Java, che consentono di eseguire tutte le operazioni all'interno del browser. La stessa configurazione è comunque necessaria per qualsiasi piattaforma, ad esempio .NET.

## <a name="create-a-general-purpose-standard-storage-account"></a>Creare un account di archiviazione standard per utilizzo generico

L'applicazione ricevitore Java, che verrà configurata in questa unità, archivia i messaggi in Archiviazione BLOB di Azure. L'archiviazione BLOB richiede un account di archiviazione.

1. Creare un account di archiviazione (utilizzo generico V2) nel gruppo di risorse tramite il comando seguente:

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name>  --location <location> --sku Standard_RAGRS --encryption blob
    ```
    |Parametro      |Descrizione|
    |---------------|-----------|
    |--name (obbligatorio)  |Immettere un nome per l'account di archiviazione.|
    |--resource-group (obbligatorio)  |Immettere il gruppo di risorse creato nell'unità precedente.|
    |--location (facoltativo)    |Immettere la posizione usata per creare il gruppo di risorse nell'unità precedente.|
1. Elencare tutte le chiavi di accesso associate all'account di archiviazione tramite il comando seguente:

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```
    |Parametro      |Descrizione|
    |---------------|-----------|
    |--account-name (obbligatorio)  |Immettere il nome per l'account di archiviazione.|
    |--resource-group (obbligatorio)  |Immettere il gruppo di risorse creato nell'unità precedente.|

     Sono elencate le chiavi di accesso associate all'account di archiviazione. Copiare e salvare il valore della **chiave** per l'uso futuro. Questa chiave sarà necessaria per accedere all'account di archiviazione.
1. Visualizzare la stringa di connessione per l'account di archiviazione tramite il comando seguente:

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```
    |Parametro      |Descrizione|
    |---------------|-----------|
    |-n (obbligatorio)  |Immettere il nome per l'account di archiviazione.|
    |-g (obbligatorio)  |Immettere il nome del gruppo di risorse.|

    Questo comando restituisce i dettagli della connessione per l'account di archiviazione. Copiare e salvare il valore di **connectionString**.

1. Creare un contenitore denominato **messages** nell'account di archiviazione tramite il comando seguente. Usare il valore di **connectionString** copiato nel passaggio precedente:

    ```azurecli
    az storage container create -n messages --connection-string "<connection string>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a>Clonare il repository GitHub di Hub eventi

Per clonare il repository GitHub di Hub eventi, eseguire la procedura seguente.

1. Accedere ad Azure Cloud Shell (Bash).

1. I file di origine per le applicazioni compilate in questo esercizio si trovano in un [repository GitHub](https://github.com/Azure/azure-event-hubs). Usare i comandi seguenti per assicurarsi di trovarsi all'interno della home directory in Cloud Shell e quindi per clonare questo repository:

    ```azurecli
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    Il repository viene clonato in `/home/<username>/azure-event-hubs`.

## <a name="use-nano-to-edit-simplesendjava"></a>Usare nano per modificare SimpleSend.java

Usare l'editor **nano** per modificare l'applicazione SimpleSend e aggiungere lo spazio dei nomi di Hub eventi, il nome dell'hub eventi, il nome dei criteri di accesso condiviso e la chiave primaria. I comandi principali sono visualizzati nella parte inferiore della finestra dell'editor. In questa unità, sarà necessario premere CTRL+O per scrivere le modifiche, INVIO per confermare il nome del file di output e CTRL+X per chiudere l'editor.

1. Passare alla cartella **SimpleSend** usando il comando seguente:

    ```azurecli
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. Aprire il file **SimpleSend.java** nell'editor **nano** usando il comando seguente:

    ```azurecli
    nano SimpleSend.java
    ```

1. Nell'editor nano individuare e sostituire le stringhe seguenti:
    - `"Your Event Hubs namespace name"` con il nome dello spazio dei nomi di Hub eventi.
    - `"Your event hub"` con il nome dell'hub eventi.
    - `"Your primary SAS key"` con il valore della chiave **primaryKey** per lo spazio dei nomi di Hub eventi che è stata salvata in precedenza.
    - `"Your policy name"` con **RootManageSharedAccessKey**.
 
        Quando si crea uno spazio dei nomi di Hub eventi, viene creata una chiave di firma di accesso condiviso a 256 bit denominata **RootManageSharedAccessKey**, a cui è associata una coppia di chiavi primaria e secondaria che concedono i diritti di invio, ascolto e gestione per lo spazio dei nomi. Nell'unità precedente è stata visualizzata la chiave usando un comando dell'interfaccia della riga di comando di Azure. È anche possibile trovare la chiave aprendo la pagina **Criteri di accesso condiviso** per lo spazio dei nomi di Hub eventi nel portale di Azure.

    ![Dettagli di configurazione per l'applicazione mittente](../media-draft/5-sender-configure.png)

1. Salvare **SimpleSend.java** con il comando seguente e uscire da nano:

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## <a name="use-maven-to-build-simplesendjava"></a>Usare Maven per compilare SimpleSend.java

A questo punto, è possibile compilare l'applicazione Java usando i comandi **mvn**.

1. Passare alla cartella **SimpleSend** principale usando il comando seguente:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    ```
1. Compilare l'applicazione Java SimpleSend usando il comando seguente. Questo garantisce che l'applicazione usi i dettagli di connessione per l'hub eventi:

    ```azurecli
    mvn clean package -DskipTests
    ```

    Il completamento del processo di compilazione può richiedere alcuni minuti. Assicurarsi che venga visualizzato il messaggio **[INFO] BUILD SUCCESS** ([INFORMAZIONI] COMPILAZIONE COMPLETATA) prima di continuare.

    ![Risultati della compilazione per l'applicazione mittente](../media-draft/5-sender-build.png)

## <a name="use-nano-to-edit-eventprocessorsamplejava"></a>Usare nano per modificare EventProcessorSample.java

Ora si configurerà un'applicazione **ricevitore** (nota anche come **sottoscrittore** o **consumer**) per inserire i dati dall'hub eventi.

Per l'applicazione ricevitore, sono disponibili due metodi: **EventHubReceiver** e **EventProcessorHost**. EventProcessorHost è basato su EventHubReceiver, ma fornisce un'interfaccia di programmazione più semplice rispetto a EventHubReceiver. EventProcessorHost può distribuire automaticamente le partizioni dei messaggi tra più istanze di EventProcessorHost usando lo stesso account di archiviazione.

In questa unità verrà usato il metodo EventProcessorHost. Verrà inoltre usato nuovamente nano per modificare l'applicazione EventProcessorSample in modo da aggiungere lo spazio dei nomi di Hub eventi, il nome dell'hub eventi, il nome dei criteri di accesso condiviso e la chiave primaria, il nome dell'account di archiviazione, la stringa di connessione e il nome del contenitore.

1. Passare alla cartella **EventProcessorSample** usando il comando seguente:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. Aprire il file **EventProcessorSample.java** nell'editor **nano** usando il comando seguente:

    ```azurecli
    nano EventProcessorSample.java
    ```
1. Nell'editor nano individuare e sostituire le stringhe seguenti:
    - `----ServiceBusNamespaceName----` con il nome dello spazio dei nomi di Hub eventi.
    - `----EventHubName----` con il nome dell'hub eventi.
    - `----SharedAccessSignatureKeyName----` con **RootManageSharedAccessKey**.
    - `----SharedAccessSignatureKey----` con il valore della chiave **primaryKey** per lo spazio dei nomi di Hub eventi che è stata salvata in precedenza.
    - `----AzureStorageConnectionString----` con la stringa di connessione dell'account di archiviazione salvata in precedenza.
    - `----StorageContainerName----` con **messages**.
    - `----HostNamePrefix----` con il nome dell'account di archiviazione.

    ![Dettagli di configurazione per l'applicazione ricevitore](../media-draft/5-receiver-configure.png)

1. Salvare **EventProcessorSample.java** con il comando seguente e uscire da nano:

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## <a name="use-maven-to-build-eventprocessorsamplejava"></a>Usare Maven per compilare EventProcessorSample.java

1. Passare alla cartella **EventProcessorSample** principale usando il comando seguente:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. Compilare l'applicazione Java SimpleSend usando il comando seguente. Questo garantisce che l'applicazione usi i dettagli di connessione per l'hub eventi:

    ```azurecli
    mvn clean package -DskipTests
    ```

    Il completamento del processo di compilazione può richiedere alcuni minuti. Assicurarsi che venga visualizzato il messaggio **[INFO] BUILD SUCCESS** ([INFORMAZIONI] COMPILAZIONE COMPLETATA) prima di continuare.

    ![Risultati della compilazione per l'applicazione ricevitore](../media-draft/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a>Avviare le app mittente e ricevitore

1. Eseguire l'applicazione Java dalla riga di comando usando il comando **java** e specificando un pacchetto con estensione jar. Usare i comandi seguenti per avviare l'applicazione SimpleSend:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. Quando viene visualizzato il messaggio **Invio completato...** premere INVIO.

    ![Risultati dell'esecuzione per l'applicazione mittente](../media-draft/5-sender-run.png)

1. Avviare l'applicazione EventProcessorSample usando il comando seguente.

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. Quando non vengono più visualizzati messaggi nella console, premere INVIO.

    ![Risultati dell'esecuzione per l'applicazione ricevitore](../media-draft/5-receiver-run.png)

## <a name="summary"></a>Riepilogo

A questo punto, è stata configurata un'applicazione mittente per l'invio dei messaggi all'hub eventi. È stata inoltre configurata un'applicazione ricevitore per la ricezione dei messaggi dall'hub eventi.
