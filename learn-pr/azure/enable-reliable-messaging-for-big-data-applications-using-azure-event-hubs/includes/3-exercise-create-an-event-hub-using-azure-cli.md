È ora possibile creare un nuovo hub eventi. Dopo aver creato l'hub eventi, si userà il portale di Azure per visualizzare il nuovo hub.

L'hub eventi verrà creato usando l'interfaccia della riga di comando di Azure. Per questo esercizio, usare l'interfaccia della riga di comando di Azure 2.0. 

## <a name="create-an-event-hubs-namespace"></a>Creare uno spazio dei nomi di Hub eventi

Usare la procedura seguente per creare uno spazio dei nomi di Hub eventi usando la shell Bash supportata da Azure Cloud Shell:

1. Accedere a Cloud Shell (Bash).  

1. Creare un gruppo di risorse di Azure usando il comando seguente:

    ```azurecli
        az group create --name <resource group name> --location <location>
    ```

    |Parametro      |Descrizione|
    |---------------|-----------|
    |--name (obbligatorio)      |Immettere un nuovo nome di gruppo di risorse.|
    |--location (obbligatorio)     |Immettere la posizione del data center di Azure più vicino, ad esempio, westus.|

1. Creare lo spazio dei nomi di Hub eventi con il comando seguente:

    ```azurecli
        az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

    |Parametro      |Descrizione|
    |---------------|-----------|
    |--name (obbligatorio)      |Immettere un nome univoco con una lunghezza compresa tra 6 e 50 caratteri per lo spazio dei nomi di Hub eventi. Il nome deve contenere solo lettere, numeri e trattini. Deve iniziare con una lettera e terminare con una lettera o un numero.|
    |--resource-group (obbligatorio)  |Immettere il gruppo di risorse creato nel passaggio 1.
    |--l (facoltativo)     |Immettere la posizione del data center di Azure più vicino, ad esempio, westus.|

1. Recuperare la stringa di connessione per lo spazio dei nomi di Hub eventi usando il comando seguente. Sarà necessaria per configurare le applicazioni per inviare e ricevere messaggi tramite l'hub eventi.

    ```azurecli
        az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

    |Parametro      |Descrizione|
    |---------------|-----------|
    |--resource-group (obbligatorio)  |Immettere il gruppo di risorse creato nel passaggio 1.|
    |--namespace-name (obbligatorio)      |Immettere lo spazio dei nomi creato nel passaggio 2.|

    Questo comando restituisce la stringa di connessione per lo spazio dei nomi di Hub eventi che si userà in un secondo momento per configurare le applicazioni di pubblicazione e consumer. Salvare il valore delle chiavi seguenti per un uso successivo.

    - **primaryConnectionString**
    - **primaryKey**

## <a name="create-an-event-hub"></a>Creare un hub eventi

Per creare il nuovo hub eventi, seguire questa procedura:

1. Creare un nuovo hub eventi con il comando seguente:

    ```azurecli
        az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

    |Parametro      |Descrizione|
    |---------------|-----------|
    |--name (obbligatorio)  |Immette un nome per l'hub eventi.|
    |--resource-group (obbligatorio)  |Immettere il gruppo di risorse creato nella procedura precedente.|
    |--namespace-name (obbligatorio)      |Immettere lo spazio dei nomi creato nella procedura precedente.|

1. Visualizzare i dettagli dell'hub eventi con il comando seguente: 

    ```azurecli
        az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>
    ```

    |Parametro      |Descrizione|
    |---------------|-----------|
    |--resource-group (obbligatorio)  |Immettere il gruppo di risorse creato nella procedura precedente.|
    |--namespace-name (obbligatorio)      |Immettere lo spazio dei nomi creato nella procedura precedente.|
    |--name (obbligatorio)|Immettere il nome dell'hub eventi creato nel passaggio 1.|

## <a name="view-the-event-hub-in-the-azure-portal"></a>Visualizzare l'hub eventi nel portale di Azure

Usare questa procedura per visualizzare l'hub eventi nel portale di Azure.

1. Trovare lo spazio dei nomi di Hub eventi usando la barra di ricerca nella parte superiore del [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Fare clic sullo spazio dei nomi per aprirlo.

1. In **Spazio dei nomi Hub eventi** > **Entità** fare clic su **Hub eventi**.
    L'hub eventi viene visualizzato con stato **Attivo** e i valori predefiniti per **Conservazione messaggi** (*7*) e **Conteggio partizioni** (*4*).

    ![Hub eventi visualizzato nel portale di Azure](../media-draft/3-event-hub.png)

## <a name="summary"></a>Riepilogo

È stato creato un nuovo hub eventi e sono ora disponibili tutte le informazioni necessarie per configurare le applicazioni di pubblicazione e consumer.
