<span data-ttu-id="1c478-101">È ora possibile creare un nuovo hub eventi.</span><span class="sxs-lookup"><span data-stu-id="1c478-101">You're now ready to create a new Event Hub.</span></span> <span data-ttu-id="1c478-102">Dopo aver creato l'hub eventi, si userà il portale di Azure per visualizzare il nuovo hub.</span><span class="sxs-lookup"><span data-stu-id="1c478-102">After creating the Event Hub, you'll use the Azure portal to view your new hub.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="set-some-defaults-in-the-azure-cli"></a><span data-ttu-id="1c478-103">Impostare alcuni valori predefiniti nell'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="1c478-103">Set some defaults in the Azure CLI</span></span>

<span data-ttu-id="1c478-104">Si inizierà fornendo alcuni valori predefiniti per l'interfaccia della riga di comando di Azure in Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="1c478-104">Let's start by providing some default values for the Azure CLI in the Cloud Shell.</span></span> <span data-ttu-id="1c478-105">Così facendo si eviterà di doverli digitare tutte le volte.</span><span class="sxs-lookup"><span data-stu-id="1c478-105">This will keep you from having to type these in every time.</span></span> <span data-ttu-id="1c478-106">In particolare, verranno impostati il _gruppo di risorse_ e la _località_.</span><span class="sxs-lookup"><span data-stu-id="1c478-106">In particular, let's set the _resource group_ and _location_.</span></span> <span data-ttu-id="1c478-107">Selezionare una località dall'elenco seguente.</span><span class="sxs-lookup"><span data-stu-id="1c478-107">Select a location from the following list.</span></span>

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="1c478-108">Digitare quindi il comando seguente nell'interfaccia della riga di comando di Azure, assicurandosi di sostituire la località con una nelle vicinanze.</span><span class="sxs-lookup"><span data-stu-id="1c478-108">Then type the following command into the Azure CLI, make sure to replace the location with one close to you.</span></span>

```azurecli
az configure --defaults group=<rgn>[sandbox Resource Group]</rgn> location=westus2
```

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="1c478-109">Creare uno spazio dei nomi di Hub eventi</span><span class="sxs-lookup"><span data-stu-id="1c478-109">Create an Event Hubs namespace</span></span>

<span data-ttu-id="1c478-110">Usare la procedura seguente per creare uno spazio dei nomi di Hub eventi usando la shell Bash supportata da Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="1c478-110">Use the following steps to create an Event Hubs namespace using bash shell supported by Azure Cloud shell:</span></span>

1. <span data-ttu-id="1c478-111">Creare lo spazio dei nomi di Hub eventi con il comando `az eventhubs namespace create`.</span><span class="sxs-lookup"><span data-stu-id="1c478-111">Create the Event Hubs namespace using the `az eventhubs namespace create` command.</span></span> <span data-ttu-id="1c478-112">Usare i parametri seguenti.</span><span class="sxs-lookup"><span data-stu-id="1c478-112">Use the following parameters.</span></span>

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="1c478-113">Parametro</span><span class="sxs-lookup"><span data-stu-id="1c478-113">Parameter</span></span>      |<span data-ttu-id="1c478-114">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1c478-114">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="1c478-115">--name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-115">--name (required)</span></span>      |<span data-ttu-id="1c478-116">Immettere un nome univoco con una lunghezza compresa tra 6 e 50 caratteri per lo spazio dei nomi di Hub eventi.</span><span class="sxs-lookup"><span data-stu-id="1c478-116">Enter a 6-50 characters-long unique name for your Event Hubs namespace.</span></span> <span data-ttu-id="1c478-117">Il nome deve contenere solo lettere, numeri e trattini.</span><span class="sxs-lookup"><span data-stu-id="1c478-117">The name should contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="1c478-118">Deve iniziare con una lettera e terminare con una lettera o un numero.</span><span class="sxs-lookup"><span data-stu-id="1c478-118">It should start with a letter and end with a letter or number.</span></span>|
    > |<span data-ttu-id="1c478-119">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-119">--resource-group (required)</span></span> | <span data-ttu-id="1c478-120">Si tratta del gruppo di risorse sandbox di Azure creato in precedenza, tratto dai valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="1c478-120">This will be the pre-created Azure sandbox resource group supplied from the defaults.</span></span> |
    > |<span data-ttu-id="1c478-121">--l (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="1c478-121">--l (optional)</span></span>     |<span data-ttu-id="1c478-122">Immettere la posizione del data center di Azure più vicino. Verrà usata l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1c478-122">Enter the location of your nearest Azure datacenter, this will use your default.</span></span>|
    > |<span data-ttu-id="1c478-123">--sku (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="1c478-123">--sku (optional)</span></span> | <span data-ttu-id="1c478-124">Piano tariffario per lo spazio dei nomi [Basic</span><span class="sxs-lookup"><span data-stu-id="1c478-124">The pricing tier for the namespace [Basic</span></span> | <span data-ttu-id="1c478-125">Standard]. L'impostazione predefinita è _Standard_.</span><span class="sxs-lookup"><span data-stu-id="1c478-125">Standard], defaults to _Standard_.</span></span> <span data-ttu-id="1c478-126">Determina le connessioni e le soglie di consumer.</span><span class="sxs-lookup"><span data-stu-id="1c478-126">This determines the connections and consumer thresholds.</span></span> |

    <span data-ttu-id="1c478-127">Impostare il nome in una variabile di ambiente in modo da riutilizzarlo.</span><span class="sxs-lookup"><span data-stu-id="1c478-127">Set the name into an environment variable so we can reuse it.</span></span>

    ```azurecli
    NS_NAME=myEvt-HubNs1
    ````

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```azurecli
    az eventhubs namespace create --name $NS_NAME
    ```

    > [!NOTE]
    > <span data-ttu-id="1c478-128">In Azure i requisiti relativi al nome sono molto rigidi e l'interfaccia della riga di comando restituisce **Richiesta non valida** se il nome esiste già o non è valido.</span><span class="sxs-lookup"><span data-stu-id="1c478-128">Azure is very picky about the name and the CLI returns **Bad Request** if the name exists or is invalid.</span></span> <span data-ttu-id="1c478-129">Provare a usare un altro nome modificando la variabile di ambiente e rieseguendo il comando.</span><span class="sxs-lookup"><span data-stu-id="1c478-129">Try a different name by changing your environment variable and reissuing the command.</span></span>


1. <span data-ttu-id="1c478-130">Recuperare la stringa di connessione per lo spazio dei nomi di Hub eventi usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="1c478-130">Fetch the connection string for your Event Hubs namespace using the following command.</span></span> <span data-ttu-id="1c478-131">Sarà necessaria per configurare le applicazioni per inviare e ricevere messaggi tramite l'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="1c478-131">You'll need this to configure applications to send and receive messages using your Event Hub.</span></span>

    ```azurecli
    az eventhubs namespace authorization-rule keys list --name RootManageSharedAccessKey --namespace-name $NS_NAME
    ```

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="1c478-132">Parametro</span><span class="sxs-lookup"><span data-stu-id="1c478-132">Parameter</span></span>      |<span data-ttu-id="1c478-133">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1c478-133">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="1c478-134">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-134">--resource-group (required)</span></span>  | <span data-ttu-id="1c478-135">Si tratta del gruppo di risorse sandbox di Azure creato in precedenza, tratto dai valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="1c478-135">This will be the pre-created Azure sandbox resource group supplied from the defaults.</span></span> |
    > |<span data-ttu-id="1c478-136">--namespace-name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-136">--namespace-name (required)</span></span>  | <span data-ttu-id="1c478-137">Immettere il nome dello spazio dei nomi creato.</span><span class="sxs-lookup"><span data-stu-id="1c478-137">Enter the name of the namespace you created.</span></span> |

    <span data-ttu-id="1c478-138">Questo comando restituisce un blocco JSON con la stringa di connessione per lo spazio dei nomi di Hub eventi che si userà in un secondo momento per configurare le applicazioni di pubblicazione e consumer.</span><span class="sxs-lookup"><span data-stu-id="1c478-138">This command returns a JSON block with the connection string for your Event Hubs namespace that you'll use later to configure your publisher and consumer applications.</span></span> <span data-ttu-id="1c478-139">Salvare il valore delle chiavi seguenti per un uso successivo.</span><span class="sxs-lookup"><span data-stu-id="1c478-139">Save the value of the following keys for later use.</span></span>

    - <span data-ttu-id="1c478-140">**primaryConnectionString**</span><span class="sxs-lookup"><span data-stu-id="1c478-140">**primaryConnectionString**</span></span>
    - <span data-ttu-id="1c478-141">**primaryKey**</span><span class="sxs-lookup"><span data-stu-id="1c478-141">**primaryKey**</span></span>

## <a name="create-an-event-hub"></a><span data-ttu-id="1c478-142">Creare un hub eventi</span><span class="sxs-lookup"><span data-stu-id="1c478-142">Create an Event Hub</span></span>

<span data-ttu-id="1c478-143">Per creare il nuovo hub eventi, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="1c478-143">Use the following steps to create your new Event Hub:</span></span>

1. <span data-ttu-id="1c478-144">Creare un nuovo hub eventi con il comando `eventhub create`.</span><span class="sxs-lookup"><span data-stu-id="1c478-144">Create a new Event Hub using the `eventhub create` command.</span></span> <span data-ttu-id="1c478-145">Sono necessari i parametri seguenti:</span><span class="sxs-lookup"><span data-stu-id="1c478-145">It needs the following parameters:</span></span>

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="1c478-146">Parametro</span><span class="sxs-lookup"><span data-stu-id="1c478-146">Parameter</span></span>      |<span data-ttu-id="1c478-147">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1c478-147">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="1c478-148">--name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-148">--name (required)</span></span>  |<span data-ttu-id="1c478-149">Immettere un nome per l'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="1c478-149">Enter a name for your Event Hub.</span></span>|
    > |<span data-ttu-id="1c478-150">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-150">--resource-group (required)</span></span>  |<span data-ttu-id="1c478-151">Proprietario del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="1c478-151">Resource group owner.</span></span>|
    > |<span data-ttu-id="1c478-152">--namespace-name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-152">--namespace-name (required)</span></span>      |<span data-ttu-id="1c478-153">Immettere lo spazio dei nomi creato.</span><span class="sxs-lookup"><span data-stu-id="1c478-153">Enter the namespace you created.</span></span>|

    <span data-ttu-id="1c478-154">Per prima cosa definire il nome dell'hub eventi in una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1c478-154">Let's define the Event Hub name in an environment variable first.</span></span>

    ```azurecli
    HUB_NAME=[name]
    ```

    ```azurecli
    az eventhubs eventhub create --name $HUB_NAME --namespace-name $NS_NAME
    ```

1. <span data-ttu-id="1c478-155">Visualizzare i dettagli dell'hub eventi con il comando `eventhub show`.</span><span class="sxs-lookup"><span data-stu-id="1c478-155">View the details of your Event Hub using the `eventhub show` command.</span></span> <span data-ttu-id="1c478-156">È necessario quanto segue:</span><span class="sxs-lookup"><span data-stu-id="1c478-156">It takes:</span></span>

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="1c478-157">Parametro</span><span class="sxs-lookup"><span data-stu-id="1c478-157">Parameter</span></span>      |<span data-ttu-id="1c478-158">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1c478-158">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="1c478-159">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-159">--resource-group (required)</span></span>  |<span data-ttu-id="1c478-160">Proprietario del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="1c478-160">Resource group owner.</span></span>|
    > |<span data-ttu-id="1c478-161">--namespace-name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-161">--namespace-name (required)</span></span>      |<span data-ttu-id="1c478-162">Immettere lo spazio dei nomi creato.</span><span class="sxs-lookup"><span data-stu-id="1c478-162">Enter the namespace you created.</span></span>|
    > |<span data-ttu-id="1c478-163">--name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="1c478-163">--name  (required)</span></span>|<span data-ttu-id="1c478-164">Nome dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="1c478-164">Name of the Event Hub.</span></span>|

    ```azurecli
    az eventhubs eventhub show --namespace-name $NS_NAME --name $HUB_NAME
    ```

## <a name="view-the-event-hub-in-the-azure-portal"></a><span data-ttu-id="1c478-165">Visualizzare l'hub eventi nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="1c478-165">View the Event Hub in the Azure portal</span></span>

<span data-ttu-id="1c478-166">Si osserverà ora il risultato nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="1c478-166">Next, let's see what this looks like in the Azure portal.</span></span>

1. <span data-ttu-id="1c478-167">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="1c478-167">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="1c478-168">Trovare lo spazio dei nomi di Hub eventi usando la barra di ricerca nella parte superiore del portale.</span><span class="sxs-lookup"><span data-stu-id="1c478-168">Find your Event Hubs namespace using the Search bar at the top of portal.</span></span>

1. <span data-ttu-id="1c478-169">Selezionare lo spazio dei nomi per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="1c478-169">Select your namespace to open it.</span></span>

1. <span data-ttu-id="1c478-170">Selezionare **Spazio dei nomi di Hub eventi** nella sezione **ENTITÀ**.</span><span class="sxs-lookup"><span data-stu-id="1c478-170">Select **Event Hubs namespace** under the **ENTITIES** section.</span></span>

1. <span data-ttu-id="1c478-171">Fare clic su **Hub eventi**.</span><span class="sxs-lookup"><span data-stu-id="1c478-171">Click **Event Hubs**.</span></span>

    <span data-ttu-id="1c478-172">L'hub eventi viene visualizzato con stato **Attivo** e i valori predefiniti per **Conservazione messaggi** (*7*) e **Conteggio partizioni** (*4*).</span><span class="sxs-lookup"><span data-stu-id="1c478-172">Your Event Hub displays with a status of **Active**, and default values for **Message Retention** (*7*) and **Partition Count** of (*4*).</span></span>

    ![Hub eventi visualizzato nel portale di Azure](../media/3-event-hub.png)

## <a name="summary"></a><span data-ttu-id="1c478-174">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1c478-174">Summary</span></span>

<span data-ttu-id="1c478-175">È stato creato un nuovo hub eventi e sono ora disponibili tutte le informazioni necessarie per configurare le applicazioni di pubblicazione e consumer.</span><span class="sxs-lookup"><span data-stu-id="1c478-175">You've now created a new Event Hub, and you've all the necessary information ready to configure your publisher and consumer applications.</span></span>
