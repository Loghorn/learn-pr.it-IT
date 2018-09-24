È ora possibile creare un nuovo hub eventi. Dopo aver creato l'hub eventi, si userà il portale di Azure per visualizzare il nuovo hub.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="set-some-defaults-in-the-azure-cli"></a>Impostare alcuni valori predefiniti nell'interfaccia della riga di comando di Azure

Si inizierà fornendo alcuni valori predefiniti per l'interfaccia della riga di comando di Azure in Cloud Shell. Così facendo si eviterà di doverli digitare tutte le volte. In particolare, verranno impostati il _gruppo di risorse_ e la _località_. Selezionare una località dall'elenco seguente.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Digitare quindi il comando seguente nell'interfaccia della riga di comando di Azure, assicurandosi di sostituire la località con una nelle vicinanze.

```azurecli
az configure --defaults group=<rgn>[Sandbox Resource Group]</rgn> location=westus2
```

## <a name="create-an-event-hubs-namespace"></a>Creare uno spazio dei nomi di Hub eventi

Usare la procedura seguente per creare uno spazio dei nomi di Hub eventi usando la shell Bash supportata da Azure Cloud Shell:

1. Creare lo spazio dei nomi di Hub eventi con il comando `az eventhubs namespace create`. Usare i parametri seguenti.

    > [!div class="mx-tableFixed"]
    > |Parametro      |Descrizione|
    > |---------------|-----------|
    > |--name (obbligatorio)      |Immettere un nome univoco con una lunghezza compresa tra 6 e 50 caratteri per lo spazio dei nomi di Hub eventi. Il nome deve contenere solo lettere, numeri e trattini. Deve iniziare con una lettera e terminare con una lettera o un numero.|
    > |--resource-group (obbligatorio) | Si tratta del gruppo di risorse sandbox di Azure creato in precedenza specificato tra i valori predefiniti. |
    > |--l (facoltativo)     |Immettere la posizione del data center di Azure più vicino. Verrà usata l'impostazione predefinita.|
    > |--sku (facoltativo) | Piano tariffario per lo spazio dei nomi [Basic | Standard]. L'impostazione predefinita è _Standard_. Determina le connessioni e le soglie di consumer. |

    Impostare il nome in una variabile di ambiente in modo da riutilizzarlo.

    ```azurecli
    NS_NAME=myEvt-HubNs1
    ````

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```azurecli
    az eventhubs namespace create --name $NS_NAME
    ```

    > [!NOTE] 
    > In Azure i requisiti relativi al nome sono molto rigidi e l'interfaccia della riga di comando restituisce **Richiesta non valida** se il nome esiste già o non è valido. Provare a usare un altro nome modificando la variabile di ambiente e rieseguendo il comando.


1. Recuperare la stringa di connessione per lo spazio dei nomi di Hub eventi usando il comando seguente. Sarà necessaria per configurare le applicazioni per inviare e ricevere messaggi tramite l'hub eventi.

    ```azurecli
    az eventhubs namespace authorization-rule keys list --name RootManageSharedAccessKey --namespace-name $NS_NAME 
    ```

    > [!div class="mx-tableFixed"]
    > |Parametro      |Descrizione|
    > |---------------|-----------|
    > |--resource-group (obbligatorio)  | Si tratta del gruppo di risorse sandbox di Azure creato in precedenza specificato tra i valori predefiniti. |
    > |--namespace-name (obbligatorio)  | Immettere il nome dello spazio dei nomi creato. |

    Questo comando restituisce un blocco JSON con la stringa di connessione per lo spazio dei nomi di Hub eventi che si userà in un secondo momento per configurare le applicazioni di pubblicazione e consumer. Salvare il valore delle chiavi seguenti per un uso successivo.

    - **primaryConnectionString**
    - **primaryKey**

## <a name="create-an-event-hub"></a>Creare un hub eventi

Per creare il nuovo hub eventi, seguire questa procedura:

1. Creare un nuovo hub eventi con il comando `eventhub create`. Sono necessari i parametri seguenti:

    > [!div class="mx-tableFixed"]
    > |Parametro      |Descrizione|
    > |---------------|-----------|
    > |--name (obbligatorio)  |Immettere un nome per l'hub eventi.|
    > |--resource-group (obbligatorio)  |Proprietario del gruppo di risorse.|
    > |--namespace-name (obbligatorio)      |Immettere lo spazio dei nomi creato.|

    Per prima cosa definire il nome dell'hub eventi in una variabile di ambiente.

    ```azurecli
    HUB_NAME=[name]
    ```

    ```azurecli
    az eventhubs eventhub create --name $HUB_NAME --namespace-name $NS_NAME
    ```

1. Visualizzare i dettagli dell'hub eventi con il comando `eventhub show`. È necessario quanto segue:

    > [!div class="mx-tableFixed"]
    > |Parametro      |Descrizione|
    > |---------------|-----------|
    > |--resource-group (obbligatorio)  |Proprietario del gruppo di risorse.|
    > |--namespace-name (obbligatorio)      |Immettere lo spazio dei nomi creato.|
    > |--name (obbligatorio)|Nome dell'hub eventi.|

    ```azurecli
    az eventhubs eventhub show --namespace-name $NS_NAME --name $HUB_NAME
    ```

## <a name="view-the-event-hub-in-the-azure-portal"></a>Visualizzare l'hub eventi nel portale di Azure

Si osserverà ora il risultato nel portale di Azure. 

1. Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Trovare lo spazio dei nomi di Hub eventi usando la barra di ricerca nella parte superiore del portale.

1. Selezionare lo spazio dei nomi per aprirlo.

1. Selezionare **Spazio dei nomi di Hub eventi** nella sezione **ENTITÀ**.

1. Fare clic su **Hub eventi**.

    L'hub eventi viene visualizzato con stato **Attivo** e i valori predefiniti per **Conservazione messaggi** (*7*) e **Conteggio partizioni** (*4*).

    ![Hub eventi visualizzato nel portale di Azure](../media/3-event-hub.png)

## <a name="summary"></a>Riepilogo

È stato creato un nuovo hub eventi e sono ora disponibili tutte le informazioni necessarie per configurare le applicazioni di pubblicazione e consumer.
