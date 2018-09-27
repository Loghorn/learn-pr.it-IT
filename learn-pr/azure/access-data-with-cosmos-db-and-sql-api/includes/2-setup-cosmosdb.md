La prima operazione da svolgere prevede la creazione di un database Azure Cosmos DB vuoto e di una raccolta. Devono corrispondere a quelli creati nell'ultimo modulo in questo percorso di apprendimento, ovvero un database denominato **"Products"** e una raccolta denominata **"Clothing"**. Usare le istruzioni seguenti e Azure Cloud Shell sulla parte destra della schermata per ricreare il database.

## <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a>Creare un account e un database di Azure Cosmos DB con l'interfaccia della riga di comando di Azure

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="select-a-subscription"></a>Selezionare una sottoscrizione

Se si usa Azure già da tempo, è possibile che siano disponibili più sottoscrizioni di Azure. Questo accade spesso per gli sviluppatori che potrebbero avere una sottoscrizione per Visual Studio e un'altra per le risorse aziendali.

La sandbox di Azure ha già selezionato automaticamente la sottoscrizione Concierge in Cloud Shell ed è possibile convalidare l'impostazione della sottoscrizione seguendo questa procedura. In alternativa, quando si lavora con la propria sottoscrizione, è possibile usare la procedura seguente per cambiare la sottoscrizione con l'interfaccia della riga di comando di Azure.

1. Per iniziare, ottenere un elenco delle sottoscrizioni disponibili.

    ```azurecli
    az account list --output table
    ```

   Se si utilizza una sottoscrizione Concierge, dovrebbe essere l'unica elencata.

1. Successivamente, se la sottoscrizione predefinita non è quella che si vuole usare, è possibile cambiarla con il comando `account set`:

    ```azurecli
    az account set --subscription "<subscription name>"
    ```
    
1. Ottenere il gruppo di risorse creato automaticamente dalla sandbox. Se si usa una sottoscrizione personale, ignorare questo passaggio e specificare semplicemente un nome univoco da usare nella variabile di ambiente `RESOURCE_GROUP` seguente. Prendere nota del nome del gruppo di risorse, in quanto qui verrà creato il database.

    ```azurecli
    az group list --out table
    ```
### <a name="setup-environment-variables"></a>Impostare le variabili di ambiente

1. Impostare alcune variabili di ambiente per non dover digitare ogni volta i valori comuni. Iniziare impostando un nome per l'account di Azure Cosmos DB, ad esempio `export NAME="mycosmosdbaccount"`. Il campo può contenere solo lettere minuscole, numeri e il carattere '-' e deve avere una lunghezza compresa tra 3 e 31 caratteri.

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

1. Impostare il gruppo di risorse per usare il gruppo di risorse Sandbox esistente.

    ```azurecli
    export RESOURCE_GROUP="<rgn>[sandbox resource group name]</rgn>"
    ```

1. Selezionare l'area più vicina e impostare la variabile di ambiente, ad esempio `export LOCATION="EastUS"`.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

1. Impostare una variabile per il nome del database. Denominarla "Products" in modo che corrisponda a quella creata nel modulo precedente.

    ```azurecli
    export DB_NAME="Products"
    ```

### <a name="create-a-resource-group-in-your-subscription"></a>Creare un gruppo di risorse nella sottoscrizione

Quando si crea un database Cosmos DB nella propria sottoscrizione, sarà utile creare un nuovo gruppo di risorse per contenere tutte le risorse correlate.

> [!IMPORTANT]
> Se si usa la sandbox di Azure fornita da Microsoft Learn, non è necessario eseguire questo passaggio, ma occorre assicurarsi che la variabile `RESOURCE_GROUP` precedente sia impostata su **<rgn>[nome gruppo di risorse sandbox</rgn>**.

Nella propria sottoscrizione si userebbe il comando seguente per creare il gruppo di risorse. 

```azurecli
az group create --name <name> --location <location>
```

### <a name="create-the-azure-cosmos-db-account"></a>Creare l'account di Azure Cosmos DB

1. Creare un account di Azure Cosmos DB con il comando `cosmosdb create`. Il comando usa i parametri seguenti e può essere eseguito senza modifiche, se si impostano le variabili di ambiente come consigliato.
    - `--name`: nome univoco per la risorsa.
    - `--kind`: tipo di database, usare _GlobalDocumentDB_.
    - `--resource-group`: gruppo di risorse. Usare **<rgn>[gruppo di risorse sandbox]</rgn>**. Deve essere assegnato alla variabile `RESOURCE_GROUP`.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

    Per il completamento del comando sono richiesti alcuni minuti e Cloud Shell visualizza le impostazioni per il nuovo account dopo la distribuzione.

1. Creare il database `Products` nell'account.

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

1. Infine, creare la raccolta `Clothing`.

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

A questo punto, dopo aver creato l'account, il database e la raccolta di Azure Cosmos DB, è possibile procedere con l'aggiunta di dati.