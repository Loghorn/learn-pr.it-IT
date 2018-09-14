> [!NOTE]
> Se l'esercitazione **Creare un database di Azure Cosmos DB per la scalabilità** è già stata completata e il database e la raccolta di Cosmos DB creati non sono stati eliminati, è possibile ignorare questa unità e procedere direttamente all'aggiunta dei dati con Esplora dati.

La prima operazione da svolgere prevede la creazione di un database Cosmos DB vuoto e di una raccolta. Deve corrispondere a quello creato in precedenza nell'ultimo modulo in questo percorso di apprendimento: database denominato **"Products"** e raccolta denominata **"Clothing"**. Usare le istruzioni seguenti e Azure Cloud Shell sulla parte destra della schermata per ricreare il database.

# <a name="create-a-cosmos-db-account--database-with-the-azure-cli"></a>Creare un account, un database e una raccolta Cosmos DB con l'interfaccia della riga di comando di Azure

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

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

1. Impostare alcune variabili di ambiente in modo da non dover digitare ogni volta che i valori comuni.

    > [!IMPORTANT]
    > Assicurarsi di sostituire i valori con quelli appropriati per la sessione in corso. Ad esempio, sostituire il `<comsos db name>` valore con il nome desiderato di Cosmos DB per avere.

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```

2. Successivamente, impostare una variabile per il nome del database. Denominarlo "Users" affinché corrisponda a quello creato nel modulo precedente.

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

3. Creare quindi l'account Cosmos DB. Questa operazione richiederà qualche minuto.

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

A questo punto, dopo aver creato l'account, il database e la raccolta Cosmos DB, è possibile aggiungere i dati.