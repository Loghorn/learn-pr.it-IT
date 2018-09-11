> [!NOTE]
> Se l'esercitazione **Creare un database di Azure Cosmos DB per la scalabilità** è già stata completata e il database e la raccolta di Cosmos DB creati non sono stati eliminati, è possibile ignorare questa unità e procedere direttamente all'aggiunta dei dati con Esplora dati.

La prima operazione da svolgere prevede la creazione di un database Cosmos DB vuoto e di una raccolta. Deve corrispondere a quello creato in precedenza nell'ultimo modulo in questo percorso di apprendimento: database denominato **"Products"** e raccolta denominata **"Clothing"**. Usare le istruzioni seguenti e Azure Cloud Shell sulla parte destra della schermata per ricreare il database.

# <a name="create-a-cosmos-db-account--database-with-the-azure-cli"></a>Creare un account, un database e una raccolta Cosmos DB con l'interfaccia della riga di comando di Azure

1. Per iniziare, selezionare la sottoscrizione corretta, ovvero l'ID sottoscrizione associato alla sottoscrizione con accesso alla formazione gratuita.

    ```azurecli
    az account list --output table
    ```

1. Assicurarsi che nell'elenco delle sottoscrizioni sia visualizzata l'indicazione "sandbox" e che sia impostata come quella corrente da usare: <!-- TODO: get official name here -->

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. Ottenere il gruppo di risorse creato automaticamente. Se si usa una sottoscrizione personale, ignorare questo passaggio e specificare semplicemente un nome univoco da usare nella variabile di ambiente `RESOURCE_GROUP` seguente. Prendere nota del nome del gruppo di risorse, in quanto qui verrà creato il database. <!-- Do we get a token for this? -->

    ```azurecli
    az group list --out table
    ```

1. Per semplificare ulteriormente la procedura, impostare alcune variabili di ambiente per non dover digitare ogni volta i valori comuni. 

    > [!IMPORTANT]
    > Assicurarsi di sostituire i valori con quelli appropriati per la sessione in corso. Ad esempio, sostituire il valore `<resource group>` con il nome del gruppo di risorse indicato sopra.

    ```azurecli
    export RESOURCE_GROUP="<resource group>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```
    
1. Successivamente, impostare una variabile per il nome del database. Denominarlo "Users" affinché corrisponda a quello creato nel modulo precedente.

    ```azurecli
    export DB_NAME="Products"
    ```
    
1. Se si esegue la procedura nella sottoscrizione personale e si usa un gruppo di risorse _Nuovo_ (scelta consigliata), per creare il gruppo di risorse usare il comando seguente. **Importante:** se si usano le risorse di formazione gratuite fornite da Microsoft Learn, non è necessario eseguire questo passaggio, ma occorre assicurarsi che la variabile `RESOURCE_GROUP` precedente sia impostata sul gruppo di risorse assegnato.

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. Creare quindi l'account Cosmos DB. Questa operazione richiederà qualche minuto.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```
    
1. Creare il database `Products` nell'account.

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```
    
1. Infine, creare la raccolta `Clothing`.

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

A questo punto, dopo aver creato l'account, il database e la raccolta Cosmos DB, è possibile aggiungere i dati.