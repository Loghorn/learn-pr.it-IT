<span data-ttu-id="36d4a-101">La prima cosa che dobbiamo fare è creare un database vuoto di Azure Cosmos DB e una raccolta per lavorare con.</span><span class="sxs-lookup"><span data-stu-id="36d4a-101">The first thing we need to do is create an empty Azure Cosmos DB database and collection to work with.</span></span> <span data-ttu-id="36d4a-102">Si desidera che corrispondano ai nomi creata nell'ultimo modulo in questo percorso di apprendimento: un database denominato **"Prodotti"** e una raccolta denominata **"Clothing"**.</span><span class="sxs-lookup"><span data-stu-id="36d4a-102">We want them to match the ones you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**.</span></span> <span data-ttu-id="36d4a-103">Usare le istruzioni seguenti e Azure Cloud Shell sulla parte destra della schermata per ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="36d4a-103">Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.</span></span>

# <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a><span data-ttu-id="36d4a-104">Creare un account Azure Cosmos DB e i database con la CLI di Azure</span><span class="sxs-lookup"><span data-stu-id="36d4a-104">Create an Azure Cosmos DB account + database with the Azure CLI</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<!--
TODO: This is original text prior to updates to use the sandbox. These can be worked back in as instructions for people using their own subscriptions. There is one more block like this below. Note that the assignment of RESOURCE_GROUP below would need to be different as well.

1. Start by selecting the correct subscription - you want to select the subscription ID associated with your free education access subscription.

    ```azurecli
    az account list --output table
    ```

1. Make sure you see "sandbox" in the subscription list and set it as the current one to use:

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. Get the Resource Group that has been created for you. If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below. Take note of the Resource Group name. This is where we will create our database.

    ```azurecli
    az group list --out table
    ```
-->

1. <span data-ttu-id="36d4a-105">Impostare alcune variabili di ambiente in modo da non dover digitare ogni volta che i valori comuni.</span><span class="sxs-lookup"><span data-stu-id="36d4a-105">Set a few environment variables so you don't have to type the common values each time.</span></span> <span data-ttu-id="36d4a-106">Iniziare impostando un nome per l'account Azure Cosmos DB, ad esempio `export NAME="mycosmosdbaccount"`.</span><span class="sxs-lookup"><span data-stu-id="36d4a-106">Start by setting a name for the Azure Cosmos DB account, for example `export NAME="mycosmosdbaccount"`.</span></span> <span data-ttu-id="36d4a-107">Il campo può contenere solo lettere minuscole lettere, numeri e '-' di caratteri e deve essere compresa tra 3 e 31 caratteri.</span><span class="sxs-lookup"><span data-stu-id="36d4a-107">The field can contain only lowercase letters, numbers and the '-' character, and must be between 3 and 31 characters.</span></span>

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

2. <span data-ttu-id="36d4a-108">Impostare il gruppo di risorse per usare il gruppo di risorse esistente Sandbox.</span><span class="sxs-lookup"><span data-stu-id="36d4a-108">Set the resource group to use the existing Sandbox resource group.</span></span>

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    ```

2. <span data-ttu-id="36d4a-109">Selezionare l'area più vicina all'utente e impostare la variabile di ambiente, ad esempio `export LOCATION="EastUS"`.</span><span class="sxs-lookup"><span data-stu-id="36d4a-109">Select the region closest to you, and set the environment variable, such as `export LOCATION="EastUS"`.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

2. <span data-ttu-id="36d4a-110">Impostare una variabile per il nome del database.</span><span class="sxs-lookup"><span data-stu-id="36d4a-110">Set a variable for the database name.</span></span> <span data-ttu-id="36d4a-111">Il nome "Prodotti" in modo che corrisponda il database creati nell'ultimo modulo.</span><span class="sxs-lookup"><span data-stu-id="36d4a-111">Name it "Products" so it matches the database we created in the last module.</span></span>

    ```azurecli
    export DB_NAME="Products"
    ```

<!-- 

TODO: Pre-sandbox text to be worked back in.

1. If you are doing this on your own subscription, and you are using a _new_ Resource Group (recommended), then use the following command to create the Resource Group. **Important:** If you are using the free education resources provided by Microsoft Learn, then you do not need to execute this step. Instead, make sure the `RESOURCE_GROUP` variable above is set to your assigned resource group.

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
-->

3. <span data-ttu-id="36d4a-112">Successivamente, creare l'account Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="36d4a-112">Next, create the Azure Cosmos DB account.</span></span> <span data-ttu-id="36d4a-113">Questa operazione richiederà qualche minuto.</span><span class="sxs-lookup"><span data-stu-id="36d4a-113">This will take a few minutes to complete.</span></span>

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

4. <span data-ttu-id="36d4a-114">Creare il database `Products` nell'account.</span><span class="sxs-lookup"><span data-stu-id="36d4a-114">Create the `Products` database in the account.</span></span>

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

5. <span data-ttu-id="36d4a-115">Infine, creare la raccolta `Clothing`.</span><span class="sxs-lookup"><span data-stu-id="36d4a-115">Finally, create the `Clothing` collection.</span></span>

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

<span data-ttu-id="36d4a-116">Dopo aver creato l'account Azure Cosmos DB, database e una raccolta, aggiungiamo alcuni dati.</span><span class="sxs-lookup"><span data-stu-id="36d4a-116">Now that you have your Azure Cosmos DB account, database, and collection, let's go add some data!</span></span>