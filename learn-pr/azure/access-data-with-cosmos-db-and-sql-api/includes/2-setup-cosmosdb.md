> [!NOTE]
> <span data-ttu-id="6671c-101">Se l'esercitazione **Creare un database di Azure Cosmos DB per la scalabilità** è già stata completata e il database e la raccolta di Cosmos DB creati non sono stati eliminati, è possibile ignorare questa unità e procedere direttamente all'aggiunta dei dati con Esplora dati.</span><span class="sxs-lookup"><span data-stu-id="6671c-101">If you are continuing on from **Create an Azure Cosmos DB database built to scale** and you did not delete the Cosmos DB database + collection that you created, then you can skip this unit and move on to adding data with the Data Explorer.</span></span>

<span data-ttu-id="6671c-102">La prima operazione da svolgere prevede la creazione di un database Cosmos DB vuoto e di una raccolta.</span><span class="sxs-lookup"><span data-stu-id="6671c-102">The first thing we need to do is create an empty Cosmos DB database and collection to work with.</span></span> <span data-ttu-id="6671c-103">Deve corrispondere a quello creato in precedenza nell'ultimo modulo in questo percorso di apprendimento: database denominato **"Products"** e raccolta denominata **"Clothing"**.</span><span class="sxs-lookup"><span data-stu-id="6671c-103">We want it to match the one you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**.</span></span> <span data-ttu-id="6671c-104">Usare le istruzioni seguenti e Azure Cloud Shell sulla parte destra della schermata per ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="6671c-104">Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.</span></span>

# <a name="create-a-cosmos-db-account--database-with-the-azure-cli"></a><span data-ttu-id="6671c-105">Creare un account, un database e una raccolta Cosmos DB con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="6671c-105">Create a Cosmos DB account + database with the Azure CLI</span></span>

1. <span data-ttu-id="6671c-106">Per iniziare, selezionare la sottoscrizione corretta, ovvero l'ID sottoscrizione associato alla sottoscrizione con accesso alla formazione gratuita.</span><span class="sxs-lookup"><span data-stu-id="6671c-106">Start by selecting the correct subscription - you want to select the subscription ID associated with your free education access subscription.</span></span>

    ```azurecli
    az account list --output table
    ```

1. <span data-ttu-id="6671c-107">Assicurarsi che nell'elenco delle sottoscrizioni sia visualizzata l'indicazione "sandbox" e che sia impostata come quella corrente da usare: <!-- TODO: get official name here --></span><span class="sxs-lookup"><span data-stu-id="6671c-107">Make sure you see "sandbox" in the subscription list and set it as the current one to use: <!-- TODO: get official name here --></span></span>

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. <span data-ttu-id="6671c-108">Ottenere il gruppo di risorse creato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6671c-108">Get the Resource Group that has been created for you.</span></span> <span data-ttu-id="6671c-109">Se si usa una sottoscrizione personale, ignorare questo passaggio e specificare semplicemente un nome univoco da usare nella variabile di ambiente `RESOURCE_GROUP` seguente.</span><span class="sxs-lookup"><span data-stu-id="6671c-109">If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below.</span></span> <span data-ttu-id="6671c-110">Prendere nota del nome del gruppo di risorse,</span><span class="sxs-lookup"><span data-stu-id="6671c-110">Take note of the Resource Group name.</span></span> <span data-ttu-id="6671c-111">in quanto qui verrà creato il database.</span><span class="sxs-lookup"><span data-stu-id="6671c-111">This is where we will create our database.</span></span> <!-- Do we get a token for this? -->

    ```azurecli
    az group list --out table
    ```

1. <span data-ttu-id="6671c-112">Per semplificare ulteriormente la procedura, impostare alcune variabili di ambiente per non dover digitare ogni volta i valori comuni.</span><span class="sxs-lookup"><span data-stu-id="6671c-112">To make this a bit easier, set a few environment variables so you don't have to type the common values each time.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="6671c-113">Assicurarsi di sostituire i valori con quelli appropriati per la sessione in corso.</span><span class="sxs-lookup"><span data-stu-id="6671c-113">Make sure to change these values to ones appropriate for your session.</span></span> <span data-ttu-id="6671c-114">Ad esempio, sostituire il valore `<resource group>` con il nome del gruppo di risorse indicato sopra.</span><span class="sxs-lookup"><span data-stu-id="6671c-114">For example, replace the `<resource group>` value with the Resource Group name you identified above.</span></span>

    ```azurecli
    export RESOURCE_GROUP="<resource group>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```
    
1. <span data-ttu-id="6671c-115">Successivamente, impostare una variabile per il nome del database.</span><span class="sxs-lookup"><span data-stu-id="6671c-115">Next, set a variable for the database name.</span></span> <span data-ttu-id="6671c-116">Denominarlo "Users" affinché corrisponda a quello creato nel modulo precedente.</span><span class="sxs-lookup"><span data-stu-id="6671c-116">Name it "Users" so it matches the database we created in the last module.</span></span>

    ```azurecli
    export DB_NAME="Products"
    ```
    
1. <span data-ttu-id="6671c-117">Se si esegue la procedura nella sottoscrizione personale e si usa un gruppo di risorse _Nuovo_ (scelta consigliata), per creare il gruppo di risorse usare il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="6671c-117">If you are doing this on your own subscription, and you are using a _new_ Resource Group (recommended), then use the following command to create the Resource Group.</span></span> <span data-ttu-id="6671c-118">**Importante:** se si usano le risorse di formazione gratuite fornite da Microsoft Learn, non è necessario eseguire questo passaggio,</span><span class="sxs-lookup"><span data-stu-id="6671c-118">**Important:** If you are using the free education resources provided by Microsoft Learn, then you do not need to execute this step.</span></span> <span data-ttu-id="6671c-119">ma occorre assicurarsi che la variabile `RESOURCE_GROUP` precedente sia impostata sul gruppo di risorse assegnato.</span><span class="sxs-lookup"><span data-stu-id="6671c-119">Instead, make sure the `RESOURCE_GROUP` variable above is set to your assigned resource group.</span></span>

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. <span data-ttu-id="6671c-120">Creare quindi l'account Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6671c-120">Next, create the Cosmos DB account.</span></span> <span data-ttu-id="6671c-121">Questa operazione richiederà qualche minuto.</span><span class="sxs-lookup"><span data-stu-id="6671c-121">This will take a few minutes to complete.</span></span>

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```
    
1. <span data-ttu-id="6671c-122">Creare il database `Products` nell'account.</span><span class="sxs-lookup"><span data-stu-id="6671c-122">Create the `Products` database in the account.</span></span>

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```
    
1. <span data-ttu-id="6671c-123">Infine, creare la raccolta `Clothing`.</span><span class="sxs-lookup"><span data-stu-id="6671c-123">Finally, create the `Clothing` collection.</span></span>

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

<span data-ttu-id="6671c-124">A questo punto, dopo aver creato l'account, il database e la raccolta Cosmos DB, è possibile aggiungere i dati.</span><span class="sxs-lookup"><span data-stu-id="6671c-124">Now that we have our Cosmos DB account, database, and collection, let's go add some data!</span></span>