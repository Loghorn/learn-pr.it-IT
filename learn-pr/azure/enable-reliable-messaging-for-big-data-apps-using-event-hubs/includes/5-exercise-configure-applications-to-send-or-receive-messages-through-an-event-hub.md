<span data-ttu-id="581dd-101">È ora possibile configurare le applicazioni di pubblicazione e consumer per l'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="581dd-101">You're now ready to configure your publisher and consumer applications for your event hub.</span></span>

<span data-ttu-id="581dd-102">In questa unità si configureranno queste applicazioni per l'invio e la ricezione di messaggi tramite l'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="581dd-102">In this unit, you'll configure these applications to send or receive messages through your event hub.</span></span> <span data-ttu-id="581dd-103">Queste applicazioni sono archiviate in un repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="581dd-103">These applications are stored in a GitHub repository.</span></span>

<span data-ttu-id="581dd-104">Verranno configurate due applicazioni distinte: una come mittente del messaggio (**SimpleSend**), l'altra come ricevitore del messaggio (**EventProcessorSample**).</span><span class="sxs-lookup"><span data-stu-id="581dd-104">You'll configure two separate applications; one acts as the message sender (**SimpleSend**), the other as the message receiver (**EventProcessorSample**).</span></span> <span data-ttu-id="581dd-105">Si tratta di applicazioni Java, che consentono di eseguire tutte le operazioni all'interno del browser.</span><span class="sxs-lookup"><span data-stu-id="581dd-105">These are Java applications, which enable you to do everything within the browser.</span></span> <span data-ttu-id="581dd-106">La stessa configurazione è comunque necessaria per qualsiasi piattaforma, ad esempio .NET.</span><span class="sxs-lookup"><span data-stu-id="581dd-106">However, the same configuration is needed for any platform, such as .NET.</span></span>

## <a name="create-a-general-purpose-standard-storage-account"></a><span data-ttu-id="581dd-107">Creare un account di archiviazione standard per utilizzo generico</span><span class="sxs-lookup"><span data-stu-id="581dd-107">Create a general-purpose, standard storage account</span></span>

<span data-ttu-id="581dd-108">L'applicazione ricevitore Java, che verrà configurata in questa unità, archivia i messaggi in Archiviazione BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="581dd-108">The Java receiver application, that you'll configure in this unit, stores messages in Azure Blob Storage.</span></span> <span data-ttu-id="581dd-109">L'archiviazione BLOB richiede un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="581dd-109">Blob Storage requires a storage account.</span></span>

1. <span data-ttu-id="581dd-110">Creare un account di archiviazione (utilizzo generico V2) nel gruppo di risorse tramite il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="581dd-110">Create a storage account (general-purpose V2) in the resource group using the following command:</span></span>

    ```azurecli
    az storage account create --name <storage account name> --resource-group <rgn>[Sandbox resource group name]</rgn>  --location <location> --sku Standard_RAGRS --encryption blob
    ```

    |<span data-ttu-id="581dd-111">Parametro</span><span class="sxs-lookup"><span data-stu-id="581dd-111">Parameter</span></span>      |<span data-ttu-id="581dd-112">Descrizione</span><span class="sxs-lookup"><span data-stu-id="581dd-112">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="581dd-113">--name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="581dd-113">--name (required)</span></span>  |<span data-ttu-id="581dd-114">Immettere un nome per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="581dd-114">Enter a name for your storage account.</span></span>|
    |<span data-ttu-id="581dd-115">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="581dd-115">--resource-group (required)</span></span>  |<span data-ttu-id="581dd-116">Immettere il gruppo di risorse creato nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="581dd-116">Enter the resource group you created in the previous unit.</span></span>|
    |<span data-ttu-id="581dd-117">--location (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="581dd-117">--location (optional)</span></span>    |<span data-ttu-id="581dd-118">Immettere la posizione usata per creare il gruppo di risorse nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="581dd-118">Enter the location you used to create your resource group in the previous unit.</span></span>|

1. <span data-ttu-id="581dd-119">Elencare tutte le chiavi di accesso associate all'account di archiviazione tramite il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="581dd-119">List all the access keys associated with your storage account using the following command:</span></span>

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <rgn>[Sandbox resource group name]</rgn>
    ```

    |<span data-ttu-id="581dd-120">Parametro</span><span class="sxs-lookup"><span data-stu-id="581dd-120">Parameter</span></span>      |<span data-ttu-id="581dd-121">Descrizione</span><span class="sxs-lookup"><span data-stu-id="581dd-121">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="581dd-122">--account-name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="581dd-122">--account-name (required)</span></span>  |<span data-ttu-id="581dd-123">Immettere il nome per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="581dd-123">Enter the name for your storage account.</span></span>|
    |<span data-ttu-id="581dd-124">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="581dd-124">--resource-group (required)</span></span>  |<span data-ttu-id="581dd-125">Immettere il gruppo di risorse creato nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="581dd-125">Enter the resource group you created in the previous unit.</span></span>|

     <span data-ttu-id="581dd-126">Sono elencate le chiavi di accesso associate all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="581dd-126">Access keys associated with your storage account are listed.</span></span> <span data-ttu-id="581dd-127">Copiare e salvare il valore della **chiave** per l'uso futuro.</span><span class="sxs-lookup"><span data-stu-id="581dd-127">Copy and save the value of **key** for future use.</span></span> <span data-ttu-id="581dd-128">Questa chiave sarà necessaria per accedere all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="581dd-128">You'll need this key to access your storage account.</span></span>

1. <span data-ttu-id="581dd-129">Visualizzare la stringa di connessione per l'account di archiviazione tramite il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="581dd-129">View the connections string for your storage account using the following command:</span></span>

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn>
    ```

    |<span data-ttu-id="581dd-130">Parametro</span><span class="sxs-lookup"><span data-stu-id="581dd-130">Parameter</span></span>      |<span data-ttu-id="581dd-131">Descrizione</span><span class="sxs-lookup"><span data-stu-id="581dd-131">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="581dd-132">-n (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="581dd-132">-n (required)</span></span>  |<span data-ttu-id="581dd-133">Immettere il nome per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="581dd-133">Enter the name for your storage account.</span></span>|
    |<span data-ttu-id="581dd-134">-g (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="581dd-134">-g (required)</span></span>  |<span data-ttu-id="581dd-135">Immettere il nome del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="581dd-135">Enter the name of your resource group.</span></span>|

    <span data-ttu-id="581dd-136">Questo comando restituisce i dettagli della connessione per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="581dd-136">This command returns the connection details for the storage account.</span></span> <span data-ttu-id="581dd-137">Copiare e salvare il valore di **connectionString**.</span><span class="sxs-lookup"><span data-stu-id="581dd-137">Copy and save the value of **connectionString**.</span></span>

1. <span data-ttu-id="581dd-138">Creare un contenitore denominato **messages** nell'account di archiviazione tramite il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="581dd-138">Create a container called **messages** in your storage account using the following command.</span></span> <span data-ttu-id="581dd-139">Usare il valore di **connectionString** copiato nel passaggio precedente:</span><span class="sxs-lookup"><span data-stu-id="581dd-139">Use the **connectionString** you copied in the previous step:</span></span>

    ```azurecli
    az storage container create -n messages --connection-string "<connection string>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a><span data-ttu-id="581dd-140">Clonare il repository GitHub di Hub eventi</span><span class="sxs-lookup"><span data-stu-id="581dd-140">Clone the Event Hubs GitHub repository</span></span>

<span data-ttu-id="581dd-141">Per clonare il repository GitHub di Hub eventi, eseguire la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="581dd-141">Use the following steps to clone the Event Hubs GitHub repository.</span></span>

1. <span data-ttu-id="581dd-142">I file di origine per le applicazioni compilate in questa unità si trovano in un [repository GitHub](https://github.com/Azure/azure-event-hubs).</span><span class="sxs-lookup"><span data-stu-id="581dd-142">The source files for the applications that you'll build In this unit are located in a [GitHub repository](https://github.com/Azure/azure-event-hubs).</span></span> <span data-ttu-id="581dd-143">Usare i comandi seguenti per assicurarsi di trovarsi all'interno della home directory in Cloud Shell e quindi per clonare questo repository:</span><span class="sxs-lookup"><span data-stu-id="581dd-143">Use the following commands to make sure that you are in your home directory in Cloud Shell, and then to clone this repository:</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    <span data-ttu-id="581dd-144">Il repository viene clonato in `/home/<username>/azure-event-hubs`.</span><span class="sxs-lookup"><span data-stu-id="581dd-144">The repository is cloned to `/home/<username>/azure-event-hubs`.</span></span>

## <a name="edit-simplesendjava"></a><span data-ttu-id="581dd-145">Modifica SimpleSend.java</span><span class="sxs-lookup"><span data-stu-id="581dd-145">Edit SimpleSend.java</span></span>

<span data-ttu-id="581dd-146">Usare l'Editor di Cloud Shell per modificare l'applicazione SimpleSend e aggiungere lo spazio dei nomi hub eventi, nome dell'hub eventi, nome dei criteri di accesso condiviso e chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="581dd-146">Use the Cloud Shell Editor to edit the SimpleSend application and add your Event Hubs namespace, event hub name, shared access policy name, and primary key.</span></span> <span data-ttu-id="581dd-147">I comandi principali sono visualizzati nella parte inferiore della finestra dell'editor. In questa unità, sarà necessario premere CTRL+O per scrivere le modifiche, INVIO per confermare il nome del file di output e CTRL+X per chiudere l'editor.</span><span class="sxs-lookup"><span data-stu-id="581dd-147">The main commands are displayed at the bottom of the editor window; in this unit, you'll need to write out your edits using CTRL +O, and then ENTER to confirm the output file name, and exit the editor using CTRL +X.</span></span>

1. <span data-ttu-id="581dd-148">Passare alla cartella **SimpleSend** usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="581dd-148">Change to the **SimpleSend** folder using the following command:</span></span>

    ```azurecli
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. <span data-ttu-id="581dd-149">Aprire il **SimpleSend.java** file nell'Editor della Shell del Cloud utilizzando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="581dd-149">Open the **SimpleSend.java** file in the Cloud Shell Editor using the following command:</span></span>

    ```azurecli
    code SimpleSend.java
    ```

1. <span data-ttu-id="581dd-150">Nell'editor, trovare e sostituire le stringhe seguenti:</span><span class="sxs-lookup"><span data-stu-id="581dd-150">In the editor, locate and replace the following strings:</span></span>

    - <span data-ttu-id="581dd-151">`"Your Event Hubs namespace name"` con il nome dello spazio dei nomi di Hub eventi.</span><span class="sxs-lookup"><span data-stu-id="581dd-151">`"Your Event Hubs namespace name"` with the name of your event hub namespace.</span></span>
    - <span data-ttu-id="581dd-152">`"Your event hub"` con il nome dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="581dd-152">`"Your event hub"` with the name of your event hub.</span></span>
    - <span data-ttu-id="581dd-153">`"Your primary SAS key"` con il valore della chiave **primaryKey** per lo spazio dei nomi di Hub eventi che è stata salvata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="581dd-153">`"Your primary SAS key"` with the value of the **primaryKey** key for your event hub namespace that you saved earlier.</span></span>
    - <span data-ttu-id="581dd-154">`"Your policy name"` con **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="581dd-154">`"Your policy name"` with **RootManageSharedAccessKey**.</span></span>
 
        <span data-ttu-id="581dd-155">Quando si crea uno spazio dei nomi di Hub eventi, viene creata una chiave di firma di accesso condiviso a 256 bit denominata **RootManageSharedAccessKey**, a cui è associata una coppia di chiavi primaria e secondaria che concedono i diritti di invio, ascolto e gestione per lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="581dd-155">When you create an Event Hubs namespace, a 256-bit SAS key called **RootManageSharedAccessKey** is created that has an associated pair of primary and secondary keys that grant send, listen, and manage rights to the namespace.</span></span> <span data-ttu-id="581dd-156">Nell'unità precedente è stata visualizzata la chiave usando un comando dell'interfaccia della riga di comando di Azure. È anche possibile trovare la chiave aprendo la pagina **Criteri di accesso condiviso** per lo spazio dei nomi di Hub eventi nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="581dd-156">In the previous unit, you displayed the key using an Azure CLI command, and you can also find this key by opening the **Shared access policies** page for your Event Hubs namespace in the Azure portal.</span></span>

1. <span data-ttu-id="581dd-157">Salvare **SimpleSend.java** tramite il menu "..." o il tasto di scelta rapida (Ctrl + S in Windows e Linux, Cmd + SS in macOS).</span><span class="sxs-lookup"><span data-stu-id="581dd-157">Save **SimpleSend.java** either through the "..." menu, or the accelerator key (Ctrl+S on Windows and Linux, Cmd+SS on macOS).</span></span>

1. <span data-ttu-id="581dd-158">È possibile chiudere l'editor di codice facendo la `{}` pulsante della barra degli strumenti di parentesi graffe nella parte superiore dell'Editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="581dd-158">You can close the code editor by clicking the `{}` curly braces toolbar button at the top of the Cloud Shell Editor.</span></span>

## <a name="use-maven-to-build-simplesendjava"></a><span data-ttu-id="581dd-159">Usare Maven per compilare SimpleSend.java</span><span class="sxs-lookup"><span data-stu-id="581dd-159">Use Maven to build SimpleSend.java</span></span>

<span data-ttu-id="581dd-160">A questo punto, è possibile compilare l'applicazione Java usando i comandi **mvn**.</span><span class="sxs-lookup"><span data-stu-id="581dd-160">You'll now build the Java application using **mvn** commands.</span></span>

1. <span data-ttu-id="581dd-161">Passare alla cartella **SimpleSend** principale usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="581dd-161">Change to the main **SimpleSend** folder using the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. <span data-ttu-id="581dd-162">Compilare l'applicazione Java SimpleSend usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="581dd-162">Build the Java SimpleSend application using the following command.</span></span> <span data-ttu-id="581dd-163">Questo garantisce che l'applicazione usi i dettagli di connessione per l'hub eventi:</span><span class="sxs-lookup"><span data-stu-id="581dd-163">This ensures that your application  uses the connection details for your event hub:</span></span>

    ```azurecli
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="581dd-164">Il completamento del processo di compilazione può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="581dd-164">The build process may take several minutes to complete.</span></span> <span data-ttu-id="581dd-165">Assicurarsi che venga visualizzato il messaggio **[INFO] BUILD SUCCESS** ([INFORMAZIONI] COMPILAZIONE COMPLETATA) prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="581dd-165">Ensure that you see the **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![Risultati della compilazione per l'applicazione mittente](../media-draft/5-sender-build.png)

## <a name="edit-eventprocessorsamplejava"></a><span data-ttu-id="581dd-167">Modifica EventProcessorSample.java</span><span class="sxs-lookup"><span data-stu-id="581dd-167">Edit EventProcessorSample.java</span></span>

<span data-ttu-id="581dd-168">Ora si configurerà un'applicazione **ricevitore** (nota anche come **sottoscrittore** o **consumer**) per inserire i dati dall'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="581dd-168">You'll now configure a **receiver** (also known as **subscribers** or **consumers**) application to ingest data from your event hub.</span></span>

<span data-ttu-id="581dd-169">Per l'applicazione ricevitore, sono disponibili due metodi: **EventHubReceiver** e **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="581dd-169">For the receiver application, two methods are available; **EventHubReceiver** and **EventProcessorHost**.</span></span> <span data-ttu-id="581dd-170">EventProcessorHost è basato su EventHubReceiver, ma fornisce un'interfaccia di programmazione più semplice rispetto a EventHubReceiver.</span><span class="sxs-lookup"><span data-stu-id="581dd-170">EventProcessorHost is built on top of EventHubReceiver, but provides simpler programmatic interface than EventHubReceiver.</span></span> <span data-ttu-id="581dd-171">EventProcessorHost può distribuire automaticamente le partizioni dei messaggi tra più istanze di EventProcessorHost usando lo stesso account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="581dd-171">EventProcessorHost can automatically distribute message partitions across multiple instances of EventProcessorHost using the same storage account.</span></span>

<span data-ttu-id="581dd-172">In questa unità verrà usato il metodo EventProcessorHost.</span><span class="sxs-lookup"><span data-stu-id="581dd-172">In this unit, you’ll use the EventProcessorHost method.</span></span> <span data-ttu-id="581dd-173">Si modificherà l'applicazione EventProcessorSample per aggiungere l'hub eventi dello spazio dei nomi, nome dell'hub eventi, nome dei criteri di accesso condiviso e chiave primaria, nome account di archiviazione, stringa di connessione e nome del contenitore.</span><span class="sxs-lookup"><span data-stu-id="581dd-173">You'll edit the EventProcessorSample application to add your Event Hubs namespace, event hub name, shared access policy name and primary key, storage account name, connection string, and container name.</span></span>

1. <span data-ttu-id="581dd-174">Passare alla cartella **EventProcessorSample** usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="581dd-174">Change to the **EventProcessorSample** folder using the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. <span data-ttu-id="581dd-175">Aprire il **EventProcessorSample.java** file nell'Editor della Shell del Cloud utilizzando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="581dd-175">Open the **EventProcessorSample.java** file in the Cloud Shell Editor using the following command:</span></span>

    ```azurecli
    code EventProcessorSample.java
    ```
1. <span data-ttu-id="581dd-176">Trovare e sostituire le stringhe seguenti nell'editor:</span><span class="sxs-lookup"><span data-stu-id="581dd-176">Locate and replace the following strings in the editor:</span></span>

    - <span data-ttu-id="581dd-177">`----ServiceBusNamespaceName----` con il nome dello spazio dei nomi di Hub eventi.</span><span class="sxs-lookup"><span data-stu-id="581dd-177">`----ServiceBusNamespaceName----` with the name of your Event Hubs namespace.</span></span>
    - <span data-ttu-id="581dd-178">`----EventHubName----` con il nome dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="581dd-178">`----EventHubName----` with the name of your event hub.</span></span>
    - <span data-ttu-id="581dd-179">`----SharedAccessSignatureKeyName----` con **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="581dd-179">`----SharedAccessSignatureKeyName----` with **RootManageSharedAccessKey**.</span></span>
    - <span data-ttu-id="581dd-180">`----SharedAccessSignatureKey----` con il valore della chiave **primaryKey** per lo spazio dei nomi di Hub eventi che è stata salvata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="581dd-180">`----SharedAccessSignatureKey----` with the value of the **primaryKey** key for your Event Hubs namespace that you saved earlier.</span></span>
    - <span data-ttu-id="581dd-181">`----AzureStorageConnectionString----` con la stringa di connessione dell'account di archiviazione salvata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="581dd-181">`----AzureStorageConnectionString----` with your storage account connection string that you saved earlier.</span></span>
    - <span data-ttu-id="581dd-182">`----StorageContainerName----` con **messages**.</span><span class="sxs-lookup"><span data-stu-id="581dd-182">`----StorageContainerName----` with **messages**.</span></span>
    - <span data-ttu-id="581dd-183">`----HostNamePrefix----` con il nome dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="581dd-183">`----HostNamePrefix----` with the name of your storage account.</span></span>

1. <span data-ttu-id="581dd-184">Salvare **EventProcessorSample.java** tramite il menu "..." o il tasto di scelta rapida (Ctrl + S in Windows e Linux, Cmd + SS in macOS).</span><span class="sxs-lookup"><span data-stu-id="581dd-184">Save **EventProcessorSample.java** either through the "..." menu, or the accelerator key (Ctrl+S on Windows and Linux, Cmd+SS on macOS).</span></span>

1. <span data-ttu-id="581dd-185">È possibile chiudere l'editor di codice facendo la `{}` pulsante della barra degli strumenti di parentesi graffe nella parte superiore dell'Editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="581dd-185">You can close the code editor by clicking the `{}` curly braces toolbar button at the top of the Cloud Shell Editor.</span></span>

## <a name="use-maven-to-build-eventprocessorsamplejava"></a><span data-ttu-id="581dd-186">Usare Maven per compilare EventProcessorSample.java</span><span class="sxs-lookup"><span data-stu-id="581dd-186">Use Maven to build EventProcessorSample.java</span></span>

1. <span data-ttu-id="581dd-187">Passare alla cartella **EventProcessorSample** principale usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="581dd-187">Change to the main **EventProcessorSample** folder using the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. <span data-ttu-id="581dd-188">Compilare l'applicazione Java SimpleSend usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="581dd-188">Build the Java SimpleSend application using the following command.</span></span> <span data-ttu-id="581dd-189">Questo garantisce che l'applicazione usi i dettagli di connessione per l'hub eventi:</span><span class="sxs-lookup"><span data-stu-id="581dd-189">This ensures that your application uses the connection details for your event hub:</span></span>

    ```azurecli
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="581dd-190">Il completamento del processo di compilazione può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="581dd-190">The build process may take several minutes to complete.</span></span> <span data-ttu-id="581dd-191">Assicurarsi che venga visualizzato il messaggio **[INFO] BUILD SUCCESS** ([INFORMAZIONI] COMPILAZIONE COMPLETATA) prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="581dd-191">Ensure that you see a **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![Risultati della compilazione per l'applicazione ricevitore](../media-draft/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a><span data-ttu-id="581dd-193">Avviare le app mittente e ricevitore</span><span class="sxs-lookup"><span data-stu-id="581dd-193">Start the sender and receiver apps</span></span>

1. <span data-ttu-id="581dd-194">Eseguire l'applicazione Java dalla riga di comando usando il comando **java** e specificando un pacchetto con estensione jar.</span><span class="sxs-lookup"><span data-stu-id="581dd-194">Run Java application from the command line by using the **java** command, and specifying a .jar package.</span></span> <span data-ttu-id="581dd-195">Usare i comandi seguenti per avviare l'applicazione SimpleSend:</span><span class="sxs-lookup"><span data-stu-id="581dd-195">Use the following commands to start the SimpleSend application:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. <span data-ttu-id="581dd-196">Quando viene visualizzato il messaggio **Invio completato...** premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="581dd-196">When you see **Send Complete...**, press ENTER.</span></span>

    ![Risultati dell'esecuzione per l'applicazione mittente](../media-draft/5-sender-run.png)

1. <span data-ttu-id="581dd-198">Avviare l'applicazione EventProcessorSample usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="581dd-198">Start the EventProcessorSample application using the following command.</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. <span data-ttu-id="581dd-199">Quando non vengono più visualizzati messaggi nella console, premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="581dd-199">When messages stop being displayed to the console, press ENTER.</span></span>

    ![Risultati dell'esecuzione per l'applicazione ricevitore](../media-draft/5-receiver-run.png)

## <a name="summary"></a><span data-ttu-id="581dd-201">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="581dd-201">Summary</span></span>

<span data-ttu-id="581dd-202">A questo punto, è stata configurata un'applicazione mittente per l'invio dei messaggi all'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="581dd-202">You've now configured a sender application ready to send messages to your event hub.</span></span> <span data-ttu-id="581dd-203">È stata inoltre configurata un'applicazione ricevitore per la ricezione dei messaggi dall'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="581dd-203">You've also configured a receiver application ready to receive messages from your event hub.</span></span>
