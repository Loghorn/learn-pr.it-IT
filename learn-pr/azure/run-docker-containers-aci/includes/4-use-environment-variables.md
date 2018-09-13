L'impostazione delle variabili di ambiente nelle istanze di contenitore consente di offrire la configurazione dinamica dell'applicazione o dello script eseguiti dal contenitore. Le variabili di ambiente vengono impostate tramite l'interfaccia della riga di comando di Azure, PowerShell o il portale di Azure al momento della creazione dei contenitori. Le variabili di ambiente protetto vengono usate per impedire che le informazioni riservate vengano visualizzate nell'output delle operazioni del contenitore.

In questa unità viene creata un'istanza di Azure Cosmos DB, quindi viene eseguita un'istanza di contenitore di Azure con le informazioni di connessione dell'istanza di Azure Cosmos DB archiviate come variabili di ambiente. Un'applicazione nel contenitore usa le variabili per scrivere e leggere i dati da Azure Cosmos DB. In questa unità, si creerà sia una variabile di ambiente che una variabile di ambiente protetto.

## <a name="deploy-azure-cosmos-db"></a>Distribuire Azure Cosmos DB

Creare l'istanza di Azure Cosmos DB con il comando `az Azure Cosmos DB create`. In questo esempio, l'indirizzo dell'endpoint di Azure Cosmos DB viene inoltre inserito in una variabile denominata *COSMOS_DB_ENDPOINT*.

Il comando potrebbe richiedere alcuni minuti:

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group myResourceGroup --name aci-cosmos --query documentEndpoint -o tsv)
```

Successivamente, ottenere la chiave di connessione di Azure Cosmos DB con il comando `az cosmosdb list-keys` e archiviarla in una variabile denominata *COSMOS_DB_MASTERKEY*:

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group myResourceGroup --name aci-cosmos --query primaryMasterKey -o tsv)
```

## <a name="deploy-a-container-instance"></a>Distribuire un'istanza di contenitore

Creare un'istanza di contenitore di Azure usando il comando `az container create`. Si noti che vengono create due variabili di ambiente, `COSMOS_DB_ENDPOINT` e `COSMOS_DB_ENDPOINT`. Queste variabili contengono i valori necessari per connettersi all'istanza di Azure Cosmos DB:

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

Dopo aver creato il contenitore, ottenere l'indirizzo IP usando il comando `az container show`:

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query ipAddress.ip --output tsv
```

Aprire un browser e passare all'indirizzo IP del contenitore. Dovrebbe venire visualizzata l'applicazione seguente. Quando si inserisce un voto in un contenitore, il voto viene archiviato nell'istanza di Azure Cosmos DB.

![Applicazione di votazione di Azure con due opzioni, gatti o cani.](../media-draft/azure-vote.png)

## <a name="secured-environment-variables"></a>Variabili di ambiente protetto

Nell'esercizio precedente è stato creato un contenitore con le informazioni di connessione ad Azure Cosmos DB archiviate in due variabili di ambiente. Per impostazione predefinita, le variabili di ambiente vengono visualizzate nel portale di Azure e negli strumenti da riga di comando in formato testo normale.

Ad esempio, se si ottengono informazioni sul contenitore creato nell'esercizio precedente con il comando `az container show`, le variabili di ambiente sono accessibili in formato testo normale:

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query containers[0].environmentVariables
```

Output di esempio:

```bash
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": "https://aci-cosmos.documents.azure.com:443/"
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": "Xm5BwdLlCllBvrR26V00000000S2uOusuglhzwkE7dOPMBQ3oA30n3rKd8PKA13700000000095ynys863Ghgw=="
  }
]
```

Le variabili di ambiente protetto impediscono l'output di testo non crittografato. Per usare le variabili di ambiente protetto, sostituire l'argomento `--environment-variables` con l'argomento `--secure-environment-variables`.

Eseguire l'esempio seguente per creare un contenitore denominato *aci-demo-secure* che usa le variabili di ambiente protetto:

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

A questo punto, quando il contenitore viene restituito con il comando `az container show`, non vengono visualizzate le variabili di ambiente:

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-secure --query containers[0].environmentVariables
```

Output di esempio:

```bash
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": null
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": null
  }
]
```

## <a name="summary"></a>Riepilogo

In questa unità sono state create un'istanza di Azure Cosmos DB e un'istanza di contenitore di Azure. Le variabili di ambiente sono state impostate nell'istanza di contenitore in modo che l'applicazione in esecuzione all'interno del contenitore sia in grado di connettersi all'istanza di Azure Cosmos DB.

Nell'unità successiva si monteranno i volumi di dati in un'istanza di contenitore di Azure per la persistenza dei dati.