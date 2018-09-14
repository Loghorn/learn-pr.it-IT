> [!NOTE]
> Se si sta continuando dal **creare un database di Azure Cosmos DB incorporato di scalabilità** oppure **query e inserimento dati nel database di Azure Cosmos DB** e ha già un account Azure Cosmos DB, quindi è possibile saltare questo passaggio l'unità ed e passare all'impostazione di Visual Studio Code.

La prima cosa che dobbiamo fare è creare un account Azure Cosmos DB.

# <a name="create-an-azure-cosmos-db-account"></a>Creare un account Azure Cosmos DB

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
      
1. Se si esegue la procedura nella sottoscrizione personale e si usa un gruppo di risorse _Nuovo_ (scelta consigliata), per creare il gruppo di risorse usare il comando seguente. **Importante:** se si usano le risorse di formazione gratuite fornite da Microsoft Learn, non è necessario eseguire questo passaggio, ma occorre assicurarsi che la variabile `RESOURCE_GROUP` precedente sia impostata sul gruppo di risorse assegnato.

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. Creare quindi l'account Cosmos DB. Questa operazione richiederà qualche minuto.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

Ora che abbiamo l'account Cosmos DB, consente di iniziare a lavorare in Visual Studio Code.
