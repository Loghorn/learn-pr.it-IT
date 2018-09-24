È ora possibile configurare le applicazioni di pubblicazione e consumer per l'hub eventi.

In questa unità si configureranno queste applicazioni per l'invio e la ricezione di messaggi tramite l'hub eventi. Queste applicazioni sono archiviate in un repository GitHub.

Verranno configurate due applicazioni distinte: una come mittente del messaggio (**SimpleSend**), l'altra come ricevitore del messaggio (**EventProcessorSample**). Si tratta di applicazioni Java, che consentono di eseguire tutte le operazioni all'interno del browser. La stessa configurazione è comunque necessaria per qualsiasi piattaforma, ad esempio .NET.

## <a name="create-a-general-purpose-standard-storage-account"></a>Creare un account di archiviazione standard per utilizzo generico

L'applicazione ricevitore Java, che verrà configurata in questa unità, archivia i messaggi in Archiviazione BLOB di Azure. Archiviazione BLOB richiede un account di archiviazione.

1. Creare un account di archiviazione (utilizzo generico V2) tramite il comando `storage account create`. Tenere presente che sono stati impostati un gruppo di risorse e una posizione predefiniti, quindi anche se normalmente questi parametri sono _obbligatori_ è possibile ometterli.

    |Parametro      |Descrizione|
    |---------------|-----------|
    |--name (obbligatorio)  | Nome per l'account di archiviazione. |
    |--resource-group (obbligatorio)  |Proprietario del gruppo di risorse. Verrà usato il gruppo di risorse sandbox creato in precedenza.|
    |--location (facoltativo)    |Posizione facoltativa se si vuole che l'account di archiviazione si trovi in una località specifica rispetto a quella del gruppo di risorse.|

    Impostare il nome dell'account di archiviazione in una variabile. Deve essere costituito interamente da lettere minuscole e numeri; i trattini come separatori sono consentiti. Deve inoltre essere univoco in Azure.

    ```azurecli
    STORAGE_NAME=[name]
    ```

    Usare quindi questo comando per creare l'account di archiviazione.

    ```azurecli
    az storage account create --name $STORAGE_NAME --sku Standard_RAGRS --encryption blob
    ```

    > [!TIP]
    > Se si verifica un errore durante la creazione dell'account di archiviazione, modificare la variabile di ambiente e riprovare.

1. Elencare tutte le chiavi di accesso associate all'account di archiviazione tramite il comando `account keys list`. Sono richiesti il nome dell'account e il gruppo di risorse (impostato come predefinito).

    ```azurecli
    az storage account keys list --account-name $STORAGE_NAME
    ```

     Sono elencate le chiavi di accesso associate all'account di archiviazione. Copiare e salvare il valore della **chiave** per l'uso futuro. Questa chiave sarà necessaria per accedere all'account di archiviazione.

1. Visualizzare la stringa di connessione per l'account di archiviazione tramite il comando seguente:

    ```azurecli
    az storage account show-connection-string -n $STORAGE_NAME
    ```

    Questo comando restituisce i dettagli della connessione per l'account di archiviazione. Copiare e salvare il _valore_ di **connectionString**. Dovrebbe essere simile a:

    ```output
    "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=storage_account_name;AccountKey=VZjXuMeuDqjCkT60xX6L5fmtXixYuY2wiPmsrXwYHIhwo736kSAUAj08XBockRZh7CZwYxuYBPe31hi8XfHlWw=="
    ```

1. Creare un contenitore denominato **messages** nell'account di archiviazione tramite il comando seguente. Usare il valore di **connectionString** copiato nel passaggio precedente:

    ```azurecli
    az storage container create -n messages --connection-string "<connection string here>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a>Clonare il repository GitHub di Hub eventi

Seguire questa procedura per clonare il repository GitHub di Hub eventi con `git`. Questa operazione può essere eseguita direttamente in Cloud Shell.

1. I file di origine per le applicazioni compilate in questa unità si trovano in un [repository GitHub](https://github.com/Azure/azure-event-hubs). Usare i comandi seguenti per assicurarsi di trovarsi all'interno della home directory in Cloud Shell e quindi per clonare questo repository:

    ```bash
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    Il repository viene clonato nella cartella principale.

## <a name="edit-simplesendjava"></a>Modificare SimpleSend.java

Verrà usato l'editor di codice di Cloud Shell predefinito. È basato sull'editor Monaco ed è simile a Visual Studio Code, ma completamente online.

L'editor verrà usato per modificare l'applicazione SimpleSend e aggiungere lo spazio dei nomi di Hub eventi, il nome dell'hub eventi, il nome dei criteri di accesso condiviso e la chiave primaria. I comandi principali sono visualizzati nella parte inferiore della finestra dell'editor. 

Sarà necessario premere <kbd>CTRL+O</kbd> per scrivere le modifiche, <kbd>INVIO</kbd> per confermare il nome del file di output e <kbd>CTRL+X</kbd> per chiudere l'editor. In alternativa, nell'angolo in alto a destra dell'editor è disponibile un menu "..." per tutti i comandi di modifica.

1. Passare alla cartella **SimpleSend**.

    ```bash
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. Aprire l'editor di codice nella cartella corrente. Verrà visualizzato un elenco di file a sinistra e uno spazio per l'editor a destra.

    ```bash
    code .
    ```

1. Aprire il file **SimpleSend.java** selezionandolo dall'elenco file.

1. Nell'editor individuare e sostituire le stringhe seguenti:

    - `"Your Event Hubs namespace name"` con il nome dello spazio dei nomi dell'hub eventi.
    - `"Your Event Hub"` con il nome dell'hub eventi.
    - `"Your policy name"` con **RootManageSharedAccessKey**.
    - `"Your primary SAS key"` con il valore della chiave **primaryKey** per lo spazio dei nomi dell'hub eventi che è stata salvata in precedenza.
 
    > [!TIP]
    > A differenza della finestra del terminale, l'editor può usare i normali tasti di scelta rapida per le operazioni di copia/incolla disponibili per il sistema operativo.

    Se non si ricordano, è possibile passare alla finestra del terminale sotto l'editor e usare il comando `echo` per visualizzare l'elenco delle variabili di ambiente. Ad esempio:

    ```bash
    echo $NS_NAME
    ```
    Quando si crea uno spazio dei nomi di Hub eventi, viene creata una chiave di firma di accesso condiviso a 256 bit denominata **RootManageSharedAccessKey**, a cui è associata una coppia di chiavi primaria e secondaria che concedono i diritti di invio, ascolto e gestione per lo spazio dei nomi. Nell'unità precedente è stata visualizzata la chiave usando un comando dell'interfaccia della riga di comando di Azure. È anche possibile trovare la chiave aprendo la pagina **Criteri di accesso condiviso** per lo spazio dei nomi di Hub eventi nel portale di Azure.

1. Salvare **SimpleSend.java** tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux, <kbd>Comando+S</kbd> in macOS).

1. Chiudere l'editor tramite il menu "..." o il tasto di scelta rapida <kbd>CTRL+Q</kbd>.

## <a name="use-maven-to-build-simplesendjava"></a>Usare Maven per compilare SimpleSend.java

A questo punto, è possibile compilare l'applicazione Java usando i comandi **mvn**.

1. Tornare alla cartella principale **SimpleSend**.

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. Compilare l'applicazione Java SimpleSend. Questo garantisce che l'applicazione usi i dettagli di connessione per l'hub eventi:

    ```bash
    mvn clean package -DskipTests
    ```

    Il completamento del processo di compilazione può richiedere alcuni minuti. Assicurarsi che venga visualizzato il messaggio **[INFO] BUILD SUCCESS** ([INFORMAZIONI] COMPILAZIONE COMPLETATA) prima di continuare.

    ![Risultati della compilazione per l'applicazione mittente](../media/5-sender-build.png)

## <a name="edit-eventprocessorsamplejava"></a>Modificare EventProcessorSample.java

Ora si configurerà un'applicazione **ricevitore** (nota anche come **sottoscrittore** o **consumer**) per inserire i dati dall'hub eventi.

Per l'applicazione ricevitore, sono disponibili due metodi: **EventHubReceiver** e **EventProcessorHost**. EventProcessorHost è basato su EventHubReceiver, ma fornisce un'interfaccia di programmazione più semplice rispetto a EventHubReceiver. EventProcessorHost può distribuire automaticamente le partizioni dei messaggi tra più istanze di EventProcessorHost usando lo stesso account di archiviazione.

In questa unità verrà usato il metodo EventProcessorHost. Verrà modificata l'applicazione EventProcessorSample in modo da aggiungere lo spazio dei nomi di Hub eventi, il nome dell'hub eventi, il nome dei criteri di accesso condiviso e la chiave primaria, il nome dell'account di archiviazione, la stringa di connessione e il nome del contenitore.

1. Passare alla cartella **EventProcessorSample** usando il comando seguente:

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. Aprire l'editor di codice.

    ```bash
    code .
    ```
    
1. Selezionare il file **EventProcessorSample.java**.

1. Individuare e sostituire le stringhe seguenti nell'editor:

    - `----ServiceBusNamespaceName----` con il nome dello spazio dei nomi di Hub eventi.
    - `----EventHubName----` con il nome dell'hub eventi.
    - `----SharedAccessSignatureKeyName----` con **RootManageSharedAccessKey**.
    - `----SharedAccessSignatureKey----` con il valore della chiave **primaryKey** per lo spazio dei nomi di Hub eventi che è stata salvata in precedenza.
    - `----AzureStorageConnectionString----` con la stringa di connessione dell'account di archiviazione salvata in precedenza.
    - `----StorageContainerName----` con **messages**.
    - `----HostNamePrefix----` con il nome dell'account di archiviazione.

1. Salvare **EventProcessorSample.java** tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux, <kbd>Comando+S</kbd> in macOS).

1. Chiudere l'editor.

## <a name="use-maven-to-build-eventprocessorsamplejava"></a>Usare Maven per compilare EventProcessorSample.java

1. Passare alla cartella **EventProcessorSample** principale usando il comando seguente:

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. Compilare l'applicazione Java SimpleSend usando il comando seguente. Questo garantisce che l'applicazione usi i dettagli di connessione per l'hub eventi:

    ```bash
    mvn clean package -DskipTests
    ```

    Il completamento del processo di compilazione può richiedere alcuni minuti. Assicurarsi che venga visualizzato il messaggio **[INFO] BUILD SUCCESS** ([INFORMAZIONI] COMPILAZIONE COMPLETATA) prima di continuare.

    ![Risultati della compilazione per l'applicazione ricevitore](../media/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a>Avviare le app mittente e ricevitore

1. Eseguire l'applicazione Java dalla riga di comando usando il comando **java** e specificando un pacchetto con estensione jar. Usare i comandi seguenti per avviare l'applicazione SimpleSend:

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. Quando viene visualizzato il messaggio **Invio completato...**, premere <kbd>INVIO</kbd>.

    ```output
    jar-with-dependencies.jar
    SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
    SLF4J: Defaulting to no-operation (NOP) logger implementation
    SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
    2018-09-18T19:42:15.146Z: Send Complete...
    ```

1. Avviare l'applicazione EventProcessorSample usando il comando seguente.

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. Quando non vengono più visualizzati messaggi nella console, premere <kbd>INVIO</kbd> o <kbd>CTRL+C</kbd> per terminare il programma.

    ```output
    ...
    SAMPLE: Partition 0 checkpointing at 1064,19
    SAMPLE (3,1120,20): "Message 80"
    SAMPLE (3,1176,21): "Message 84"
    SAMPLE (3,1232,22): "Message 88"
    SAMPLE (3,1288,23): "Message 92"
    SAMPLE (3,1344,24): "Message 96"
    SAMPLE: Partition 3 checkpointing at 1344,24
    SAMPLE (2,1120,20): "Message 83"
    SAMPLE (2,1176,21): "Message 87"
    SAMPLE (2,1232,22): "Message 91"
    SAMPLE (2,1288,23): "Message 95"
    SAMPLE (2,1344,24): "Message 99"
    SAMPLE: Partition 2 checkpointing at 1344,24
    SAMPLE: Partition 1 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE (0,1120,20): "Message 81"
    SAMPLE (0,1176,21): "Message 85"
    SAMPLE: Partition 0 batch size was 10 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 got event batch
    SAMPLE (0,1232,22): "Message 89"
    SAMPLE (0,1288,23): "Message 93"
    SAMPLE (0,1344,24): "Message 97"
    SAMPLE: Partition 0 checkpointing at 1344,24
    SAMPLE: Partition 3 batch size was 8 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 2 batch size was 9 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    ```

## <a name="summary"></a>Riepilogo

A questo punto, è stata configurata un'applicazione mittente per l'invio dei messaggi all'hub eventi. È stata inoltre configurata un'applicazione ricevitore per la ricezione dei messaggi dall'hub eventi.