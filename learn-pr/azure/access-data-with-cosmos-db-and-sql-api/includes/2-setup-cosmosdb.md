<span data-ttu-id="5b682-101">La prima operazione da svolgere prevede la creazione di un database Azure Cosmos DB vuoto e di una raccolta.</span><span class="sxs-lookup"><span data-stu-id="5b682-101">The first thing we need to do is create an empty Azure Cosmos DB database and collection to work with.</span></span> <span data-ttu-id="5b682-102">Devono corrispondere a quelli creati nell'ultimo modulo in questo percorso di apprendimento, ovvero un database denominato **"Products"** e una raccolta denominata **"Clothing"**.</span><span class="sxs-lookup"><span data-stu-id="5b682-102">We want them to match the ones you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**.</span></span> <span data-ttu-id="5b682-103">Usare le istruzioni seguenti e Azure Cloud Shell sulla parte destra della schermata per ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="5b682-103">Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.</span></span>

## <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a><span data-ttu-id="5b682-104">Creare un account e un database di Azure Cosmos DB con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="5b682-104">Create an Azure Cosmos DB account + database with the Azure CLI</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="select-a-subscription"></a><span data-ttu-id="5b682-105">Selezionare una sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="5b682-105">Select a subscription</span></span>

<span data-ttu-id="5b682-106">Se si usa Azure già da tempo, è possibile che siano disponibili più sottoscrizioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="5b682-106">If you've been using Azure for a while, you might have multiple subscriptions available to you.</span></span> <span data-ttu-id="5b682-107">Questo accade spesso per gli sviluppatori che potrebbero avere una sottoscrizione per Visual Studio e un'altra per le risorse aziendali.</span><span class="sxs-lookup"><span data-stu-id="5b682-107">This is often the case for developers who might have a subscription for Visual Studio, and another for corporate resources.</span></span>

<span data-ttu-id="5b682-108">La sandbox di Azure ha già selezionato automaticamente la sottoscrizione Concierge in Cloud Shell ed è possibile convalidare l'impostazione della sottoscrizione seguendo questa procedura.</span><span class="sxs-lookup"><span data-stu-id="5b682-108">The Azure Sandbox has already selected the Concierge Subscription for you in the Cloud Shell, and you can validate the subscription setting using these steps.</span></span> <span data-ttu-id="5b682-109">In alternativa, quando si lavora con la propria sottoscrizione, è possibile usare la procedura seguente per cambiare la sottoscrizione con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="5b682-109">Or, when you are working with your own subscription, you can use the following steps to switch subscriptions with the Azure CLI.</span></span>

1. <span data-ttu-id="5b682-110">Per iniziare, ottenere un elenco delle sottoscrizioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="5b682-110">Start by listing the available subscriptions.</span></span>

    ```azurecli
    az account list --output table
    ```

   <span data-ttu-id="5b682-111">Se si utilizza una sottoscrizione Concierge, dovrebbe essere l'unica elencata.</span><span class="sxs-lookup"><span data-stu-id="5b682-111">If you're working with a Concierge Subscription, it should be the only one listed.</span></span>

1. <span data-ttu-id="5b682-112">Successivamente, se la sottoscrizione predefinita non è quella che si vuole usare, è possibile cambiarla con il comando `account set`:</span><span class="sxs-lookup"><span data-stu-id="5b682-112">Next, if the default subscription isn't the one you want to use, you can change it with the `account set` command:</span></span>

    ```azurecli
    az account set --subscription "<subscription name>"
    ```
    
1. <span data-ttu-id="5b682-113">Ottenere il gruppo di risorse creato automaticamente dalla sandbox.</span><span class="sxs-lookup"><span data-stu-id="5b682-113">Get the Resource Group that has been created for you by the sandbox.</span></span> <span data-ttu-id="5b682-114">Se si usa una sottoscrizione personale, ignorare questo passaggio e specificare semplicemente un nome univoco da usare nella variabile di ambiente `RESOURCE_GROUP` seguente.</span><span class="sxs-lookup"><span data-stu-id="5b682-114">If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below.</span></span> <span data-ttu-id="5b682-115">Prendere nota del nome del gruppo di risorse,</span><span class="sxs-lookup"><span data-stu-id="5b682-115">Take note of the Resource Group name.</span></span> <span data-ttu-id="5b682-116">in quanto qui verrà creato il database.</span><span class="sxs-lookup"><span data-stu-id="5b682-116">This is where we will create our database.</span></span>

    ```azurecli
    az group list --out table
    ```
### <a name="setup-environment-variables"></a><span data-ttu-id="5b682-117">Impostare le variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="5b682-117">Setup environment variables</span></span>

1. <span data-ttu-id="5b682-118">Impostare alcune variabili di ambiente per non dover digitare ogni volta i valori comuni.</span><span class="sxs-lookup"><span data-stu-id="5b682-118">Set a few environment variables so you don't have to type the common values each time.</span></span> <span data-ttu-id="5b682-119">Iniziare impostando un nome per l'account di Azure Cosmos DB, ad esempio `export NAME="mycosmosdbaccount"`.</span><span class="sxs-lookup"><span data-stu-id="5b682-119">Start by setting a name for the Azure Cosmos DB account, for example `export NAME="mycosmosdbaccount"`.</span></span> <span data-ttu-id="5b682-120">Il campo può contenere solo lettere minuscole, numeri e il carattere '-' e deve avere una lunghezza compresa tra 3 e 31 caratteri.</span><span class="sxs-lookup"><span data-stu-id="5b682-120">The field can contain only lowercase letters, numbers and the '-' character, and must be between 3 and 31 characters.</span></span>

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

1. <span data-ttu-id="5b682-121">Impostare il gruppo di risorse per usare il gruppo di risorse Sandbox esistente.</span><span class="sxs-lookup"><span data-stu-id="5b682-121">Set the resource group to use the existing Sandbox resource group.</span></span>

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    ```

1. <span data-ttu-id="5b682-122">Selezionare l'area più vicina e impostare la variabile di ambiente, ad esempio `export LOCATION="EastUS"`.</span><span class="sxs-lookup"><span data-stu-id="5b682-122">Select the region closest to you, and set the environment variable, such as `export LOCATION="EastUS"`.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

1. <span data-ttu-id="5b682-123">Impostare una variabile per il nome del database.</span><span class="sxs-lookup"><span data-stu-id="5b682-123">Set a variable for the database name.</span></span> <span data-ttu-id="5b682-124">Denominarla "Products" in modo che corrisponda a quella creata nel modulo precedente.</span><span class="sxs-lookup"><span data-stu-id="5b682-124">Name it "Products" so it matches the database we created in the last module.</span></span>

    ```azurecli
    export DB_NAME="Products"
    ```

### <a name="create-a-resource-group-in-your-subscription"></a><span data-ttu-id="5b682-125">Creare un gruppo di risorse nella sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="5b682-125">Create a resource group in your subscription</span></span>

<span data-ttu-id="5b682-126">Quando si crea un database Cosmos DB nella propria sottoscrizione, sarà utile creare un nuovo gruppo di risorse per contenere tutte le risorse correlate.</span><span class="sxs-lookup"><span data-stu-id="5b682-126">When you are creating a Cosmos DB on your own subscription you will want to create a new resource group to hold all the related resources.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5b682-127">Se si usa la sandbox di Azure fornita da Microsoft Learn, non è necessario eseguire questo passaggio,</span><span class="sxs-lookup"><span data-stu-id="5b682-127">If you are using the Azure Sandbox provided by Microsoft Learn, then you do not need to execute this step.</span></span> <span data-ttu-id="5b682-128">ma occorre assicurarsi che la variabile `RESOURCE_GROUP` precedente sia impostata su **<rgn>[nome gruppo di risorse sandbox</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="5b682-128">Instead, make sure the `RESOURCE_GROUP` variable above is set to **<rgn>[Sandbox Resource Group Name</rgn>**.</span></span>

<span data-ttu-id="5b682-129">Nella propria sottoscrizione si userebbe il comando seguente per creare il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="5b682-129">In your own subscription you would use the following command to create the Resource Group.</span></span> 

```azurecli
az group create --name <name> --location <location>
```

### <a name="create-the-azure-cosmos-db-account"></a><span data-ttu-id="5b682-130">Creare l'account di Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5b682-130">Create the Azure Cosmos DB account</span></span>

1. <span data-ttu-id="5b682-131">Creare un account di Azure Cosmos DB con il comando `cosmosdb create`.</span><span class="sxs-lookup"><span data-stu-id="5b682-131">Create the Azure Cosmos DB account with the `cosmosdb create` command.</span></span> <span data-ttu-id="5b682-132">Il comando usa i parametri seguenti e può essere eseguito senza modifiche, se si impostano le variabili di ambiente come consigliato.</span><span class="sxs-lookup"><span data-stu-id="5b682-132">The command uses the following parameters and can be run with no modifications if you set the environment variables as recommended.</span></span>
    - <span data-ttu-id="5b682-133">`--name`: nome univoco per la risorsa.</span><span class="sxs-lookup"><span data-stu-id="5b682-133">`--name`: Unique name for the resource.</span></span>
    - <span data-ttu-id="5b682-134">`--kind`: tipo di database, usare _GlobalDocumentDB_.</span><span class="sxs-lookup"><span data-stu-id="5b682-134">`--kind`: Kind of database, use _GlobalDocumentDB_.</span></span>
    - <span data-ttu-id="5b682-135">`--resource-group`: gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="5b682-135">`--resource-group`: The resource group.</span></span> <span data-ttu-id="5b682-136">Usare **<rgn>[gruppo di risorse sandbox]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="5b682-136">Use **<rgn>[Sandbox Resource Group]</rgn>**.</span></span> <span data-ttu-id="5b682-137">Deve essere assegnato alla variabile `RESOURCE_GROUP`.</span><span class="sxs-lookup"><span data-stu-id="5b682-137">It should be assigned to the `RESOURCE_GROUP` variable.</span></span>

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

    <span data-ttu-id="5b682-138">Per il completamento del comando sono richiesti alcuni minuti e Cloud Shell visualizza le impostazioni per il nuovo account dopo la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="5b682-138">The command takes a few minutes to complete and the cloud shell displays the settings for the new account once it's deployed.</span></span>

1. <span data-ttu-id="5b682-139">Creare il database `Products` nell'account.</span><span class="sxs-lookup"><span data-stu-id="5b682-139">Create the `Products` database in the account.</span></span>

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

1. <span data-ttu-id="5b682-140">Infine, creare la raccolta `Clothing`.</span><span class="sxs-lookup"><span data-stu-id="5b682-140">Finally, create the `Clothing` collection.</span></span>

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

<span data-ttu-id="5b682-141">A questo punto, dopo aver creato l'account, il database e la raccolta di Azure Cosmos DB, è possibile procedere con l'aggiunta di dati.</span><span class="sxs-lookup"><span data-stu-id="5b682-141">Now that you have your Azure Cosmos DB account, database, and collection, let's go add some data!</span></span>