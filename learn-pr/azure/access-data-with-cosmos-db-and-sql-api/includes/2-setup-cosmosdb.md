La prima cosa che dobbiamo fare è creare un database vuoto di Azure Cosmos DB e una raccolta per lavorare con. Si desidera che corrispondano ai nomi creata nell'ultimo modulo in questo percorso di apprendimento: un database denominato **"Prodotti"** e una raccolta denominata **"Clothing"**. Usare le istruzioni seguenti e Azure Cloud Shell sulla parte destra della schermata per ricreare il database.

# <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a>Creare un account Azure Cosmos DB e i database con la CLI di Azure

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

1. Impostare alcune variabili di ambiente in modo da non dover digitare ogni volta che i valori comuni. Iniziare impostando un nome per l'account Azure Cosmos DB, ad esempio `export NAME="mycosmosdbaccount"`. Il campo può contenere solo lettere minuscole lettere, numeri e '-' di caratteri e deve essere compresa tra 3 e 31 caratteri.

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

2. Impostare il gruppo di risorse per usare il gruppo di risorse esistente Sandbox.

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    ```

2. Selezionare l'area più vicina all'utente e impostare la variabile di ambiente, ad esempio `export LOCATION="EastUS"`.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

2. Impostare una variabile per il nome del database. Il nome "Prodotti" in modo che corrisponda il database creati nell'ultimo modulo.

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

3. Successivamente, creare l'account Azure Cosmos DB. Questa operazione richiederà qualche minuto.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

4. Creare il database `Products` nell'account.

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

5. Infine, creare la raccolta `Clothing`.

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

Dopo aver creato l'account Azure Cosmos DB, database e una raccolta, aggiungiamo alcuni dati.