<span data-ttu-id="b8fd7-101">È ora possibile configurare le applicazioni di pubblicazione e consumer per l'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-101">You're now ready to configure your publisher and consumer applications for your Event Hub.</span></span>

<span data-ttu-id="b8fd7-102">In questa unità si configureranno queste applicazioni per l'invio e la ricezione di messaggi tramite l'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-102">In this unit, you'll configure these applications to send or receive messages through your Event Hub.</span></span> <span data-ttu-id="b8fd7-103">Queste applicazioni sono archiviate in un repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-103">These applications are stored in a GitHub repository.</span></span>

<span data-ttu-id="b8fd7-104">Verranno configurate due applicazioni distinte: una come mittente del messaggio (**SimpleSend**), l'altra come ricevitore del messaggio (**EventProcessorSample**).</span><span class="sxs-lookup"><span data-stu-id="b8fd7-104">You'll configure two separate applications; one acts as the message sender (**SimpleSend**), the other as the message receiver (**EventProcessorSample**).</span></span> <span data-ttu-id="b8fd7-105">Si tratta di applicazioni Java, che consentono di eseguire tutte le operazioni all'interno del browser.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-105">These are Java applications, which enable you to do everything within the browser.</span></span> <span data-ttu-id="b8fd7-106">La stessa configurazione è comunque necessaria per qualsiasi piattaforma, ad esempio .NET.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-106">However, the same configuration is needed for any platform, such as .NET.</span></span>

## <a name="create-a-general-purpose-standard-storage-account"></a><span data-ttu-id="b8fd7-107">Creare un account di archiviazione standard per utilizzo generico</span><span class="sxs-lookup"><span data-stu-id="b8fd7-107">Create a general-purpose, standard storage account</span></span>

<span data-ttu-id="b8fd7-108">L'applicazione ricevitore Java, che verrà configurata in questa unità, archivia i messaggi in Archiviazione BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-108">The Java receiver application, that you'll configure in this unit, stores messages in Azure Blob Storage.</span></span> <span data-ttu-id="b8fd7-109">Archiviazione BLOB richiede un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-109">Blob Storage requires a storage account.</span></span>

1. <span data-ttu-id="b8fd7-110">Creare un account di archiviazione (utilizzo generico V2) tramite il comando `storage account create`.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-110">Create a storage account (general-purpose V2) using the `storage account create` command.</span></span> <span data-ttu-id="b8fd7-111">Tenere presente che sono stati impostati un gruppo di risorse e una posizione predefiniti, quindi anche se normalmente questi parametri sono _obbligatori_ è possibile ometterli.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-111">Remember we set a default resource group and location, so even though those parameters are normally _required_, we can leave them off.</span></span>

    |<span data-ttu-id="b8fd7-112">Parametro</span><span class="sxs-lookup"><span data-stu-id="b8fd7-112">Parameter</span></span>      |<span data-ttu-id="b8fd7-113">Descrizione</span><span class="sxs-lookup"><span data-stu-id="b8fd7-113">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="b8fd7-114">--name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="b8fd7-114">--name (required)</span></span>  | <span data-ttu-id="b8fd7-115">Nome per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-115">A name for your storage account.</span></span> |
    |<span data-ttu-id="b8fd7-116">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="b8fd7-116">--resource-group (required)</span></span>  |<span data-ttu-id="b8fd7-117">Proprietario del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-117">The resource group owner.</span></span> <span data-ttu-id="b8fd7-118">Verrà usato il gruppo di risorse sandbox creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-118">We'll use the pre-created Sandbox resource group.</span></span>|
    |<span data-ttu-id="b8fd7-119">--location (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="b8fd7-119">--location (optional)</span></span>    |<span data-ttu-id="b8fd7-120">Posizione facoltativa se si vuole che l'account di archiviazione si trovi in una località specifica rispetto a quella del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-120">An optional location if you want the storage account in a specific place vs. the resource group location.</span></span>|

    <span data-ttu-id="b8fd7-121">Impostare il nome dell'account di archiviazione in una variabile.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-121">Set the storage account name into a variable.</span></span> <span data-ttu-id="b8fd7-122">Deve essere costituito interamente da lettere minuscole e numeri; i trattini come separatori sono consentiti.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-122">It must be composed of all lower-case letters, numbers, with hyphen separators allowed.</span></span> <span data-ttu-id="b8fd7-123">Deve inoltre essere univoco in Azure.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-123">It also must be unique within Azure.</span></span>

    ```azurecli
    STORAGE_NAME=[name]
    ```

    <span data-ttu-id="b8fd7-124">Usare quindi questo comando per creare l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-124">Then use this command to create the storage account.</span></span>

    ```azurecli
    az storage account create --name $STORAGE_NAME --sku Standard_RAGRS --encryption blob
    ```

    > [!TIP]
    > <span data-ttu-id="b8fd7-125">Se si verifica un errore durante la creazione dell'account di archiviazione, modificare la variabile di ambiente e riprovare.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-125">If the storage account creation fails, change your environment variable and try again.</span></span>

1. <span data-ttu-id="b8fd7-126">Elencare tutte le chiavi di accesso associate all'account di archiviazione tramite il comando `account keys list`.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-126">List all the access keys associated with your storage account using the `account keys list` command.</span></span> <span data-ttu-id="b8fd7-127">Sono richiesti il nome dell'account e il gruppo di risorse (impostato come predefinito).</span><span class="sxs-lookup"><span data-stu-id="b8fd7-127">It takes your account name and the resource group (which is defaulted).</span></span>

    ```azurecli
    az storage account keys list --account-name $STORAGE_NAME
    ```

     <span data-ttu-id="b8fd7-128">Sono elencate le chiavi di accesso associate all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-128">Access keys associated with your storage account are listed.</span></span> <span data-ttu-id="b8fd7-129">Copiare e salvare il valore della **chiave** per l'uso futuro.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-129">Copy and save the value of **key** for future use.</span></span> <span data-ttu-id="b8fd7-130">Questa chiave sarà necessaria per accedere all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-130">You'll need this key to access your storage account.</span></span>

1. <span data-ttu-id="b8fd7-131">Visualizzare la stringa di connessione per l'account di archiviazione tramite il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-131">View the connections string for your storage account using the following command:</span></span>

    ```azurecli
    az storage account show-connection-string -n $STORAGE_NAME
    ```

    <span data-ttu-id="b8fd7-132">Questo comando restituisce i dettagli della connessione per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-132">This command returns the connection details for the storage account.</span></span> <span data-ttu-id="b8fd7-133">Copiare e salvare il _valore_ di **connectionString**.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-133">Copy and save the _value_ of **connectionString**.</span></span> <span data-ttu-id="b8fd7-134">Dovrebbe essere simile a:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-134">It should look something like:</span></span>

    ```output
    "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=storage_account_name;AccountKey=VZjXuMeuDqjCkT60xX6L5fmtXixYuY2wiPmsrXwYHIhwo736kSAUAj08XBockRZh7CZwYxuYBPe31hi8XfHlWw=="
    ```

1. Creare un contenitore denominato **messages** nell'account di archiviazione tramite il comando seguente. <span data-ttu-id="b8fd7-136">Usare il valore di **connectionString** copiato nel passaggio precedente:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-136">Use the **connectionString** you copied in the previous step:</span></span>

    ```azurecli
    az storage container create -n messages --connection-string "<connection string here>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a><span data-ttu-id="b8fd7-137">Clonare il repository GitHub di Hub eventi</span><span class="sxs-lookup"><span data-stu-id="b8fd7-137">Clone the Event Hubs GitHub repository</span></span>

<span data-ttu-id="b8fd7-138">Seguire questa procedura per clonare il repository GitHub di Hub eventi con `git`.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-138">Use the following steps to clone the Event Hubs GitHub repository with `git`.</span></span> <span data-ttu-id="b8fd7-139">Questa operazione può essere eseguita direttamente in Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-139">You can execute this right in the Cloud Shell.</span></span>

1. <span data-ttu-id="b8fd7-140">I file di origine per le applicazioni compilate in questa unità si trovano in un [repository GitHub](https://github.com/Azure/azure-event-hubs).</span><span class="sxs-lookup"><span data-stu-id="b8fd7-140">The source files for the applications that you'll build in this unit are located in a [GitHub repository](https://github.com/Azure/azure-event-hubs).</span></span> <span data-ttu-id="b8fd7-141">Usare i comandi seguenti per assicurarsi di trovarsi all'interno della home directory in Cloud Shell e quindi per clonare questo repository:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-141">Use the following commands to make sure that you are in your home directory in Cloud Shell, and then to clone this repository:</span></span>

    ```bash
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    <span data-ttu-id="b8fd7-142">Il repository viene clonato nella cartella principale.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-142">The repository is cloned to your home folder.</span></span>

## <a name="edit-simplesendjava"></a><span data-ttu-id="b8fd7-143">Modificare SimpleSend.java</span><span class="sxs-lookup"><span data-stu-id="b8fd7-143">Edit SimpleSend.java</span></span>

<span data-ttu-id="b8fd7-144">Verrà usato l'editor di codice di Cloud Shell predefinito.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-144">We're going to use the built-in Cloud Shell Code editor.</span></span> <span data-ttu-id="b8fd7-145">È basato sull'editor Monaco ed è simile a Visual Studio Code, ma completamente online.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-145">This is based on the Monaco editor and is similar to Visual Studio Code, but completely online.</span></span>

<span data-ttu-id="b8fd7-146">L'editor verrà usato per modificare l'applicazione SimpleSend e aggiungere lo spazio dei nomi di Hub eventi, il nome dell'hub eventi, il nome dei criteri di accesso condiviso e la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-146">We'll use the editor to modify the SimpleSend application and add your Event Hubs namespace, Event Hub name, shared access policy name, and primary key.</span></span> <span data-ttu-id="b8fd7-147">I comandi principali sono visualizzati nella parte inferiore della finestra dell'editor.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-147">The main commands are displayed at the bottom of the editor window.</span></span> 

<span data-ttu-id="b8fd7-148">Sarà necessario premere <kbd>CTRL+O</kbd> per scrivere le modifiche, <kbd>INVIO</kbd> per confermare il nome del file di output e <kbd>CTRL+X</kbd> per chiudere l'editor.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-148">You'll need to write out your edits using <kbd>Ctrl+O</kbd>, and then <kbd>ENTER</kbd> to confirm the output file name, and exit the editor using <kbd>Ctrl+X</kbd>.</span></span> <span data-ttu-id="b8fd7-149">In alternativa, nell'angolo in alto a destra dell'editor è disponibile un menu "..." per tutti i comandi di modifica.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-149">Alternatively, the editor has a "..." menu in the top/right corner for all the editing commands.</span></span>

1. <span data-ttu-id="b8fd7-150">Passare alla cartella **SimpleSend**.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-150">Change to the **SimpleSend** folder.</span></span>

    ```bash
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. <span data-ttu-id="b8fd7-151">Aprire l'editor di codice nella cartella corrente.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-151">Open the code editor in the current folder.</span></span> <span data-ttu-id="b8fd7-152">Verrà visualizzato un elenco di file a sinistra e uno spazio per l'editor a destra.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-152">This will show a list of files on the left and an editor space on the right.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="b8fd7-153">Aprire il file **SimpleSend.java** selezionandolo dall'elenco file.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-153">Open the **SimpleSend.java** file by selecting it from the file list.</span></span>

1. <span data-ttu-id="b8fd7-154">Nell'editor individuare e sostituire le stringhe seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-154">In the editor, locate and replace the following strings:</span></span>

    - <span data-ttu-id="b8fd7-155">`"Your Event Hubs namespace name"` con il nome dello spazio dei nomi dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-155">`"Your Event Hubs namespace name"` with the name of your Event Hub namespace.</span></span>
    - <span data-ttu-id="b8fd7-156">`"Your Event Hub"` con il nome dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-156">`"Your Event Hub"` with the name of your Event Hub.</span></span>
    - <span data-ttu-id="b8fd7-157">`"Your policy name"` con **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-157">`"Your policy name"` with **RootManageSharedAccessKey**.</span></span>
    - <span data-ttu-id="b8fd7-158">`"Your primary SAS key"` con il valore della chiave **primaryKey** per lo spazio dei nomi dell'hub eventi che è stata salvata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-158">`"Your primary SAS key"` with the value of the **primaryKey** key for your Event Hub namespace that you saved earlier.</span></span>
 
    > [!TIP]
    > <span data-ttu-id="b8fd7-159">A differenza della finestra del terminale, l'editor può usare i normali tasti di scelta rapida per le operazioni di copia/incolla disponibili per il sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-159">Unlike the terminal window, the editor can use typical copy/paste keyboard accelerator keys for your OS.</span></span>

    <span data-ttu-id="b8fd7-160">Se non si ricordano, è possibile passare alla finestra del terminale sotto l'editor e usare il comando `echo` per visualizzare l'elenco delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-160">If you've forgotten some of them, you can switch down to the terminal window below the editor and use the `echo` command to list out one of the environment variables.</span></span> <span data-ttu-id="b8fd7-161">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-161">For example:</span></span>

    ```bash
    echo $NS_NAME
    ```
    <span data-ttu-id="b8fd7-162">Quando si crea uno spazio dei nomi di Hub eventi, viene creata una chiave di firma di accesso condiviso a 256 bit denominata **RootManageSharedAccessKey**, a cui è associata una coppia di chiavi primaria e secondaria che concedono i diritti di invio, ascolto e gestione per lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-162">When you create an Event Hubs namespace, a 256-bit SAS key called **RootManageSharedAccessKey** is created that has an associated pair of primary and secondary keys that grant send, listen, and manage rights to the namespace.</span></span> <span data-ttu-id="b8fd7-163">Nell'unità precedente è stata visualizzata la chiave usando un comando dell'interfaccia della riga di comando di Azure. È anche possibile trovare la chiave aprendo la pagina **Criteri di accesso condiviso** per lo spazio dei nomi di Hub eventi nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-163">In the previous unit, you displayed the key using an Azure CLI command, and you can also find this key by opening the **Shared access policies** page for your Event Hubs namespace in the Azure portal.</span></span>

1. <span data-ttu-id="b8fd7-164">Salvare **SimpleSend.java** tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux, <kbd>Comando+S</kbd> in macOS).</span><span class="sxs-lookup"><span data-stu-id="b8fd7-164">Save **SimpleSend.java** either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="b8fd7-165">Chiudere l'editor tramite il menu "..." o il tasto di scelta rapida <kbd>CTRL+Q</kbd>.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-165">Close the editor through the "..." menu, or the accelerator key <kbd>CTRL+Q</kbd>.</span></span>

## <a name="use-maven-to-build-simplesendjava"></a><span data-ttu-id="b8fd7-166">Usare Maven per compilare SimpleSend.java</span><span class="sxs-lookup"><span data-stu-id="b8fd7-166">Use Maven to build SimpleSend.java</span></span>

<span data-ttu-id="b8fd7-167">A questo punto, è possibile compilare l'applicazione Java usando i comandi **mvn**.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-167">You'll now build the Java application using **mvn** commands.</span></span>

1. <span data-ttu-id="b8fd7-168">Tornare alla cartella principale **SimpleSend**.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-168">Change back to the main **SimpleSend** folder.</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. <span data-ttu-id="b8fd7-169">Compilare l'applicazione Java SimpleSend.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-169">Build the Java SimpleSend application.</span></span> <span data-ttu-id="b8fd7-170">Questo garantisce che l'applicazione usi i dettagli di connessione per l'hub eventi:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-170">This ensures that your application  uses the connection details for your Event Hub:</span></span>

    ```bash
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="b8fd7-171">Il completamento del processo di compilazione può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-171">The build process may take several minutes to complete.</span></span> <span data-ttu-id="b8fd7-172">Assicurarsi che venga visualizzato il messaggio **[INFO] BUILD SUCCESS** ([INFORMAZIONI] COMPILAZIONE COMPLETATA) prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-172">Ensure that you see the **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![Risultati della compilazione per l'applicazione mittente](../media/5-sender-build.png)

## <a name="edit-eventprocessorsamplejava"></a><span data-ttu-id="b8fd7-174">Modificare EventProcessorSample.java</span><span class="sxs-lookup"><span data-stu-id="b8fd7-174">Edit EventProcessorSample.java</span></span>

<span data-ttu-id="b8fd7-175">Ora si configurerà un'applicazione **ricevitore** (nota anche come **sottoscrittore** o **consumer**) per inserire i dati dall'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-175">You'll now configure a **receiver** (also known as **subscribers** or **consumers**) application to ingest data from your Event Hub.</span></span>

<span data-ttu-id="b8fd7-176">Per l'applicazione ricevitore, sono disponibili due metodi: **EventHubReceiver** e **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-176">For the receiver application, two methods are available; **EventHubReceiver** and **EventProcessorHost**.</span></span> <span data-ttu-id="b8fd7-177">EventProcessorHost è basato su EventHubReceiver, ma fornisce un'interfaccia di programmazione più semplice rispetto a EventHubReceiver.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-177">EventProcessorHost is built on top of EventHubReceiver, but provides simpler programmatic interface than EventHubReceiver.</span></span> <span data-ttu-id="b8fd7-178">EventProcessorHost può distribuire automaticamente le partizioni dei messaggi tra più istanze di EventProcessorHost usando lo stesso account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-178">EventProcessorHost can automatically distribute message partitions across multiple instances of EventProcessorHost using the same storage account.</span></span>

<span data-ttu-id="b8fd7-179">In questa unità verrà usato il metodo EventProcessorHost.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-179">In this unit, you’ll use the EventProcessorHost method.</span></span> <span data-ttu-id="b8fd7-180">Verrà modificata l'applicazione EventProcessorSample in modo da aggiungere lo spazio dei nomi di Hub eventi, il nome dell'hub eventi, il nome dei criteri di accesso condiviso e la chiave primaria, il nome dell'account di archiviazione, la stringa di connessione e il nome del contenitore.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-180">You'll edit the EventProcessorSample application to add your Event Hubs namespace, Event Hub name, shared access policy name and primary key, storage account name, connection string, and container name.</span></span>

1. <span data-ttu-id="b8fd7-181">Passare alla cartella **EventProcessorSample** usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-181">Change to the **EventProcessorSample** folder using the following command:</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. <span data-ttu-id="b8fd7-182">Aprire l'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-182">Open the code editor.</span></span>

    ```bash
    code .
    ```
    
1. <span data-ttu-id="b8fd7-183">Selezionare il file **EventProcessorSample.java**.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-183">Select the **EventProcessorSample.java** file.</span></span>

1. <span data-ttu-id="b8fd7-184">Individuare e sostituire le stringhe seguenti nell'editor:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-184">Locate and replace the following strings in the editor:</span></span>

    - <span data-ttu-id="b8fd7-185">`----ServiceBusNamespaceName----` con il nome dello spazio dei nomi di Hub eventi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-185">`----ServiceBusNamespaceName----` with the name of your Event Hubs namespace.</span></span>
    - <span data-ttu-id="b8fd7-186">`----EventHubName----` con il nome dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-186">`----EventHubName----` with the name of your Event Hub.</span></span>
    - <span data-ttu-id="b8fd7-187">`----SharedAccessSignatureKeyName----` con **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-187">`----SharedAccessSignatureKeyName----` with **RootManageSharedAccessKey**.</span></span>
    - <span data-ttu-id="b8fd7-188">`----SharedAccessSignatureKey----` con il valore della chiave **primaryKey** per lo spazio dei nomi di Hub eventi che è stata salvata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-188">`----SharedAccessSignatureKey----` with the value of the **primaryKey** key for your Event Hubs namespace that you saved earlier.</span></span>
    - <span data-ttu-id="b8fd7-189">`----AzureStorageConnectionString----` con la stringa di connessione dell'account di archiviazione salvata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-189">`----AzureStorageConnectionString----` with your storage account connection string that you saved earlier.</span></span>
    - <span data-ttu-id="b8fd7-190">`----StorageContainerName----` con **messages**.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-190">`----StorageContainerName----` with **messages**.</span></span>
    - <span data-ttu-id="b8fd7-191">`----HostNamePrefix----` con il nome dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-191">`----HostNamePrefix----` with the name of your storage account.</span></span>

1. <span data-ttu-id="b8fd7-192">Salvare **EventProcessorSample.java** tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux, <kbd>Comando+S</kbd> in macOS).</span><span class="sxs-lookup"><span data-stu-id="b8fd7-192">Save **EventProcessorSample.java** either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="b8fd7-193">Chiudere l'editor.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-193">Close the editor.</span></span>

## <a name="use-maven-to-build-eventprocessorsamplejava"></a><span data-ttu-id="b8fd7-194">Usare Maven per compilare EventProcessorSample.java</span><span class="sxs-lookup"><span data-stu-id="b8fd7-194">Use Maven to build EventProcessorSample.java</span></span>

1. <span data-ttu-id="b8fd7-195">Passare alla cartella **EventProcessorSample** principale usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-195">Change to the main **EventProcessorSample** folder using the following command:</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. <span data-ttu-id="b8fd7-196">Compilare l'applicazione Java SimpleSend usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-196">Build the Java SimpleSend application using the following command.</span></span> <span data-ttu-id="b8fd7-197">Questo garantisce che l'applicazione usi i dettagli di connessione per l'hub eventi:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-197">This ensures that your application uses the connection details for your Event Hub:</span></span>

    ```bash
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="b8fd7-198">Il completamento del processo di compilazione può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-198">The build process may take several minutes to complete.</span></span> <span data-ttu-id="b8fd7-199">Assicurarsi che venga visualizzato il messaggio **[INFO] BUILD SUCCESS** ([INFORMAZIONI] COMPILAZIONE COMPLETATA) prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-199">Ensure that you see a **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![Risultati della compilazione per l'applicazione ricevitore](../media/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a><span data-ttu-id="b8fd7-201">Avviare le app mittente e ricevitore</span><span class="sxs-lookup"><span data-stu-id="b8fd7-201">Start the sender and receiver apps</span></span>

1. <span data-ttu-id="b8fd7-202">Eseguire l'applicazione Java dalla riga di comando usando il comando **java** e specificando un pacchetto con estensione jar.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-202">Run Java application from the command line by using the **java** command, and specifying a .jar package.</span></span> <span data-ttu-id="b8fd7-203">Usare i comandi seguenti per avviare l'applicazione SimpleSend:</span><span class="sxs-lookup"><span data-stu-id="b8fd7-203">Use the following commands to start the SimpleSend application:</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="b8fd7-204">Quando viene visualizzato il messaggio **Invio completato...**, premere <kbd>INVIO</kbd>.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-204">When you see **Send Complete...**, press <kbd>ENTER</kbd>.</span></span>

    ```output
    jar-with-dependencies.jar
    SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
    SLF4J: Defaulting to no-operation (NOP) logger implementation
    SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
    2018-09-18T19:42:15.146Z: Send Complete...
    ```

1. <span data-ttu-id="b8fd7-205">Avviare l'applicazione EventProcessorSample usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-205">Start the EventProcessorSample application using the following command.</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="b8fd7-206">Quando non vengono più visualizzati messaggi nella console, premere <kbd>INVIO</kbd> o <kbd>CTRL+C</kbd> per terminare il programma.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-206">When messages stop being displayed to the console, press <kbd>ENTER</kbd> or <kbd>CTRL+C</kbd> to end the program.</span></span>

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

## <a name="summary"></a><span data-ttu-id="b8fd7-207">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="b8fd7-207">Summary</span></span>

<span data-ttu-id="b8fd7-208">A questo punto, è stata configurata un'applicazione mittente per l'invio dei messaggi all'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-208">You've now configured a sender application ready to send messages to your Event Hub.</span></span> <span data-ttu-id="b8fd7-209">È stata inoltre configurata un'applicazione ricevitore per la ricezione dei messaggi dall'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="b8fd7-209">You've also configured a receiver application ready to receive messages from your Event Hub.</span></span>