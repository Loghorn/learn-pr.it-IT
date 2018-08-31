<span data-ttu-id="f5f0b-101">È ora possibile creare un nuovo hub eventi.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-101">You're now ready to create a new event hub.</span></span> <span data-ttu-id="f5f0b-102">Dopo aver creato l'hub eventi, si userà il portale di Azure per visualizzare il nuovo hub.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-102">After creating the event hub, you'll use the Azure portal to view your new hub.</span></span>

<span data-ttu-id="f5f0b-103">L'hub eventi verrà creato usando l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-103">You'll create an event hub using the Azure CLI.</span></span> <span data-ttu-id="f5f0b-104">Per questo esercizio, usare l'interfaccia della riga di comando di Azure 2.0.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-104">For this exercise, use the Azure CLI 2.0.</span></span> 

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="f5f0b-105">Creare uno spazio dei nomi di Hub eventi</span><span class="sxs-lookup"><span data-stu-id="f5f0b-105">Create an Event Hubs namespace</span></span>

<span data-ttu-id="f5f0b-106">Usare la procedura seguente per creare uno spazio dei nomi di Hub eventi usando la shell Bash supportata da Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="f5f0b-106">Use the following steps to create an Event Hubs namespace using Bash shell supported by Azure Cloud shell:</span></span>

1. <span data-ttu-id="f5f0b-107">Accedere a Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="f5f0b-107">Sign in to the Cloud Shell (Bash).</span></span>  

2. <span data-ttu-id="f5f0b-108">Creare un gruppo di risorse di Azure usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f5f0b-108">Create an Azure resource group using the following command:</span></span>
    ```azurecli
        az group create --name <resource group name> --location <location>
    ```
    |<span data-ttu-id="f5f0b-109">Parametro</span><span class="sxs-lookup"><span data-stu-id="f5f0b-109">Parameter</span></span>      |<span data-ttu-id="f5f0b-110">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f5f0b-110">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f5f0b-111">--name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-111">--name (required)</span></span>      |<span data-ttu-id="f5f0b-112">Immettere un nuovo nome di gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-112">Enter a new resource group name.</span></span>|
    |<span data-ttu-id="f5f0b-113">--location (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-113">--location (required)</span></span>     |<span data-ttu-id="f5f0b-114">Immettere la posizione del data center di Azure più vicino, ad esempio, westus.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-114">Enter the location of your nearest Azure datacenter, for example, westus.</span></span>|
3. <span data-ttu-id="f5f0b-115">Creare lo spazio dei nomi di Hub eventi con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f5f0b-115">Create the Event Hubs namespace using the following command:</span></span>
    ```azurecli
        az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```
    |<span data-ttu-id="f5f0b-116">Parametro</span><span class="sxs-lookup"><span data-stu-id="f5f0b-116">Parameter</span></span>      |<span data-ttu-id="f5f0b-117">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f5f0b-117">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f5f0b-118">--name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-118">--name (required)</span></span>      |<span data-ttu-id="f5f0b-119">Immettere un nome univoco con una lunghezza compresa tra 6 e 50 caratteri per lo spazio dei nomi di Hub eventi.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-119">Enter a 6-50 characters-long unique name for your Event Hubs namespace.</span></span> <span data-ttu-id="f5f0b-120">Il nome deve contenere solo lettere, numeri e trattini.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-120">The name should contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="f5f0b-121">Deve iniziare con una lettera e terminare con una lettera o un numero.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-121">It should start with a letter and end with a letter or number.</span></span>|
    |<span data-ttu-id="f5f0b-122">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-122">--resource-group (required)</span></span>  |<span data-ttu-id="f5f0b-123">Immettere il gruppo di risorse creato nel passaggio 1.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-123">Enter the resource group you created in step 1.</span></span>
    |<span data-ttu-id="f5f0b-124">--l (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-124">--l (optional)</span></span>     |<span data-ttu-id="f5f0b-125">Immettere la posizione del data center di Azure più vicino, ad esempio, westus.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-125">Enter the location of your nearest Azure datacenter, for example, westus.</span></span>|
4. <span data-ttu-id="f5f0b-126">Recuperare la stringa di connessione per lo spazio dei nomi di Hub eventi usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-126">Fetch the connection string for your Event Hubs namespace using the following command.</span></span> <span data-ttu-id="f5f0b-127">Sarà necessaria per configurare le applicazioni per inviare e ricevere messaggi tramite l'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-127">You'll need this to configure applications to send and receive messages using your event hub.</span></span>
    ```azurecli
        az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```
    |<span data-ttu-id="f5f0b-128">Parametro</span><span class="sxs-lookup"><span data-stu-id="f5f0b-128">Parameter</span></span>      |<span data-ttu-id="f5f0b-129">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f5f0b-129">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f5f0b-130">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-130">--resource-group (required)</span></span>  |<span data-ttu-id="f5f0b-131">Immettere il gruppo di risorse creato nel passaggio 1.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-131">Enter the resource group you created in step 1.</span></span>|
    |<span data-ttu-id="f5f0b-132">--namespace-name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-132">--namespace-name (required)</span></span>      |<span data-ttu-id="f5f0b-133">Immettere lo spazio dei nomi creato nel passaggio 2.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-133">Enter the namespace you created in step 2.</span></span>|

    <span data-ttu-id="f5f0b-134">Questo comando restituisce la stringa di connessione per lo spazio dei nomi di Hub eventi che si userà in un secondo momento per configurare le applicazioni di pubblicazione e consumer.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-134">This command returns the connection string for your Event Hubs namespace that you'll use later to configure your publisher and consumer applications.</span></span> <span data-ttu-id="f5f0b-135">Salvare il valore delle chiavi seguenti per un uso successivo.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-135">Save the value of the following keys for later use.</span></span>
    - <span data-ttu-id="f5f0b-136">**primaryConnectionString**</span><span class="sxs-lookup"><span data-stu-id="f5f0b-136">**primaryConnectionString**</span></span>
    - <span data-ttu-id="f5f0b-137">**primaryKey**</span><span class="sxs-lookup"><span data-stu-id="f5f0b-137">**primaryKey**</span></span>

## <a name="create-an-event-hub"></a><span data-ttu-id="f5f0b-138">Creare un hub eventi</span><span class="sxs-lookup"><span data-stu-id="f5f0b-138">Create an event hub</span></span>

<span data-ttu-id="f5f0b-139">Per creare il nuovo hub eventi, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="f5f0b-139">Use the following steps to create your new event hub:</span></span>

1. <span data-ttu-id="f5f0b-140">Creare un nuovo hub eventi con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f5f0b-140">Create a new event hub using the following command:</span></span>
    ```azurecli
        az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```
    |<span data-ttu-id="f5f0b-141">Parametro</span><span class="sxs-lookup"><span data-stu-id="f5f0b-141">Parameter</span></span>      |<span data-ttu-id="f5f0b-142">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f5f0b-142">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f5f0b-143">--name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-143">--name (required)</span></span>  |<span data-ttu-id="f5f0b-144">Immette un nome per l'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-144">Enter a name for your event hub.</span></span>|
    |<span data-ttu-id="f5f0b-145">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-145">--resource-group (required)</span></span>  |<span data-ttu-id="f5f0b-146">Immettere il gruppo di risorse creato nella procedura precedente.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-146">Enter the resource group you created in the previous procedure.</span></span>|
    |<span data-ttu-id="f5f0b-147">--namespace-name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-147">--namespace-name (required)</span></span>      |<span data-ttu-id="f5f0b-148">Immettere lo spazio dei nomi creato nella procedura precedente.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-148">Enter the namespace you created in the previous procedure.</span></span>|
2. <span data-ttu-id="f5f0b-149">Visualizzare i dettagli dell'hub eventi con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f5f0b-149">View the details of your event hub using the following command:</span></span> 
    ```azurecli
        az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>
    ```
    |<span data-ttu-id="f5f0b-150">Parametro</span><span class="sxs-lookup"><span data-stu-id="f5f0b-150">Parameter</span></span>      |<span data-ttu-id="f5f0b-151">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f5f0b-151">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f5f0b-152">--resource-group (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-152">--resource-group (required)</span></span>  |<span data-ttu-id="f5f0b-153">Immettere il gruppo di risorse creato nella procedura precedente.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-153">Enter the resource group that you created in the previous procedure.</span></span>|
    |<span data-ttu-id="f5f0b-154">--namespace-name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-154">--namespace-name (required)</span></span>      |<span data-ttu-id="f5f0b-155">Immettere lo spazio dei nomi creato nella procedura precedente.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-155">Enter the namespace you created in the previous procedure.</span></span>|
    |<span data-ttu-id="f5f0b-156">--name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="f5f0b-156">--name  (required)</span></span>|<span data-ttu-id="f5f0b-157">Immettere il nome dell'hub eventi creato nel passaggio 1.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-157">Enter the name of the event hub you created in step 1.</span></span>|

## <a name="view-the-event-hub-in-the-azure-portal"></a><span data-ttu-id="f5f0b-158">Visualizzare l'hub eventi nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="f5f0b-158">View the event hub in the Azure portal</span></span>

<span data-ttu-id="f5f0b-159">Usare questa procedura per visualizzare l'hub eventi nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-159">Use the following steps view your event hub in the Azure portal.</span></span>

1. <span data-ttu-id="f5f0b-160">Trovare lo spazio dei nomi di Hub eventi usando la barra di ricerca nella parte superiore del [portale di Azure](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="f5f0b-160">Find your Event Hubs namespace using the Search bar at the top of the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>
1. <span data-ttu-id="f5f0b-161">Fare clic sullo spazio dei nomi per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-161">Click your namespace to open it.</span></span>
1. <span data-ttu-id="f5f0b-162">In **Spazio dei nomi Hub eventi** > **Entità** fare clic su **Hub eventi**.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-162">From **Event Hubs Namespace** > **ENTITIES**, click **Event Hubs**.</span></span>
    <span data-ttu-id="f5f0b-163">L'hub eventi viene visualizzato con stato **Attivo** e i valori predefiniti per **Conservazione messaggi** (*7*) e **Conteggio partizioni** (*4*).</span><span class="sxs-lookup"><span data-stu-id="f5f0b-163">Your event hub displays with a status of **Active**, and default values for **Message Retention** (*7*) and **Partition Count** of (*4*).</span></span>

    ![Hub eventi visualizzato nel portale di Azure](../media-draft/3-event-hub.png)

## <a name="summary"></a><span data-ttu-id="f5f0b-165">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f5f0b-165">Summary</span></span>

<span data-ttu-id="f5f0b-166">È stato creato un nuovo hub eventi e sono ora disponibili tutte le informazioni necessarie per configurare le applicazioni di pubblicazione e consumer.</span><span class="sxs-lookup"><span data-stu-id="f5f0b-166">You've now created a new event hub, and you've all the necessary information ready to configure your publisher and consumer applications.</span></span>