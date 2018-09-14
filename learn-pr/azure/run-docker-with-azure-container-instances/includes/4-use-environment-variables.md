<span data-ttu-id="508a8-101">Le variabili di ambiente nelle istanze di contenitore consentono di specificare la configurazione dinamica dell'applicazione o script eseguiti dal contenitore.</span><span class="sxs-lookup"><span data-stu-id="508a8-101">Environment variables in your container instances allow you to provide dynamic configuration of the application or script run by the container.</span></span> <span data-ttu-id="508a8-102">Per impostare le variabili quando si crea il contenitore è usare il comando di Azure, PowerShell o il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="508a8-102">You use the Azure CLI, PowerShell, or the Azure portal to set variables when you create the container.</span></span> <span data-ttu-id="508a8-103">Le variabili di ambiente protetto vengono usate per impedire che le informazioni riservate vengano visualizzate nell'output delle operazioni del contenitore.</span><span class="sxs-lookup"><span data-stu-id="508a8-103">Secured environment variables are used to prevent sensitive information from being displayed in container operations output.</span></span>

<span data-ttu-id="508a8-104">In questo caso, si creerà un'istanza di Azure Cosmos DB e usare le variabili di ambiente per passare le informazioni di connessione a un'istanza di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="508a8-104">Here, you will create an Azure Cosmos DB instance and use environment variables to pass the connection information to an Azure container instance.</span></span> <span data-ttu-id="508a8-105">Un'applicazione nel contenitore utilizza le variabili per scrivere e leggere i dati da Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="508a8-105">An application in the container uses the variables to write and read data from Cosmos DB.</span></span> <span data-ttu-id="508a8-106">Si creerà una variabile di ambiente sia una variabile di ambiente protetto.</span><span class="sxs-lookup"><span data-stu-id="508a8-106">You will create both an environment variable and a secured environment variable.</span></span>

## <a name="deploy-azure-cosmos-db"></a><span data-ttu-id="508a8-107">Distribuire Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="508a8-107">Deploy Azure Cosmos DB</span></span>

<span data-ttu-id="508a8-108">Creare l'istanza di Azure Cosmos DB con il comando `az Azure Cosmos DB create`.</span><span class="sxs-lookup"><span data-stu-id="508a8-108">Create the Azure Cosmos DB instance with the `az Azure Cosmos DB create` command.</span></span> <span data-ttu-id="508a8-109">In questo esempio, l'indirizzo dell'endpoint di Azure Cosmos DB viene inoltre inserito in una variabile denominata *COSMOS_DB_ENDPOINT*.</span><span class="sxs-lookup"><span data-stu-id="508a8-109">This example will also place the Azure Cosmos DB endpoint address in a variable named *COSMOS_DB_ENDPOINT*.</span></span>

<span data-ttu-id="508a8-110">Il comando potrebbe richiedere alcuni minuti:</span><span class="sxs-lookup"><span data-stu-id="508a8-110">This command can take a few minutes to complete:</span></span>

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query documentEndpoint -o tsv)
```

<span data-ttu-id="508a8-111">Successivamente, ottenere la chiave di connessione di Azure Cosmos DB con il comando `az cosmosdb list-keys` e archiviarla in una variabile denominata *COSMOS_DB_MASTERKEY*:</span><span class="sxs-lookup"><span data-stu-id="508a8-111">Next, get the Azure Cosmos DB connection key with the `az cosmosdb list-keys` command and store it in a variable named *COSMOS_DB_MASTERKEY*:</span></span>

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query primaryMasterKey -o tsv)
```

## <a name="deploy-a-container-instance"></a><span data-ttu-id="508a8-112">Distribuire un'istanza di contenitore</span><span class="sxs-lookup"><span data-stu-id="508a8-112">Deploy a container instance</span></span>

<span data-ttu-id="508a8-113">Creare un'istanza di contenitore di Azure usando il comando `az container create`.</span><span class="sxs-lookup"><span data-stu-id="508a8-113">Create an Azure container instance using the `az container create` command.</span></span> <span data-ttu-id="508a8-114">Si noti che vengono create due variabili di ambiente, `COSMOS_DB_ENDPOINT` e `COSMOS_DB_ENDPOINT`.</span><span class="sxs-lookup"><span data-stu-id="508a8-114">Take note that two environment variables are created, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_ENDPOINT`.</span></span> <span data-ttu-id="508a8-115">Queste variabili contengono i valori necessari per connettersi all'istanza di Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="508a8-115">These variables hold the values needed to connect to the Azure Cosmos DB instance:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

<span data-ttu-id="508a8-116">Dopo aver creato il contenitore, ottenere l'indirizzo IP usando il comando `az container show`:</span><span class="sxs-lookup"><span data-stu-id="508a8-116">Once the container has been created, get the IP address with the `az container show` command:</span></span>

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo --query ipAddress.ip --output tsv
```

<span data-ttu-id="508a8-117">Aprire un browser e passare all'indirizzo IP del contenitore.</span><span class="sxs-lookup"><span data-stu-id="508a8-117">Open a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="508a8-118">Dovrebbe venire visualizzata l'applicazione seguente.</span><span class="sxs-lookup"><span data-stu-id="508a8-118">You should see the following application.</span></span> <span data-ttu-id="508a8-119">Quando si esegue il cast di un voto, i voti vengono archiviati nell'istanza di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="508a8-119">When casting a vote, the vote is stored in the Azure Cosmos DB instance.</span></span>

![Applicazione di votazione di Azure con due opzioni, gatti o cani.](../media-draft/azure-vote.png)

## <a name="secured-environment-variables"></a><span data-ttu-id="508a8-121">Variabili di ambiente protetto</span><span class="sxs-lookup"><span data-stu-id="508a8-121">Secured environment variables</span></span>

<span data-ttu-id="508a8-122">Nell'esercizio precedente è stato creato un contenitore con le informazioni di connessione ad Azure Cosmos DB archiviate in due variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="508a8-122">In the previous exercise, a container was created with connection information for Azure Cosmos DB stored in two environment variables.</span></span> <span data-ttu-id="508a8-123">Per impostazione predefinita, le variabili di ambiente vengono visualizzate nel portale di Azure e negli strumenti da riga di comando in formato testo normale.</span><span class="sxs-lookup"><span data-stu-id="508a8-123">By default, environment variables are displayed in the Azure portal and command-line tools in plain text.</span></span>

<span data-ttu-id="508a8-124">Ad esempio, se si ottengono informazioni sul contenitore creato nell'esercizio precedente con il comando `az container show`, le variabili di ambiente sono accessibili in formato testo normale:</span><span class="sxs-lookup"><span data-stu-id="508a8-124">For example, if you get information about the container created in the previous exercise with the `az container show` command, the environment variables are accessible in plain text:</span></span>

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo --query containers[0].environmentVariables
```

<span data-ttu-id="508a8-125">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="508a8-125">Example output:</span></span>

```json
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

Le variabili di ambiente protetto impediscono l'output di testo non crittografato. <span data-ttu-id="508a8-127">Per usare le variabili di ambiente protetto, sostituire l'argomento `--environment-variables` con l'argomento `--secure-environment-variables`.</span><span class="sxs-lookup"><span data-stu-id="508a8-127">To use secure environment variables, replace the `--environment-variables` argument with the `--secure-environment-variables` argument.</span></span>

<span data-ttu-id="508a8-128">Eseguire l'esempio seguente per creare un contenitore denominato *aci-demo-secure* che usa le variabili di ambiente protetto:</span><span class="sxs-lookup"><span data-stu-id="508a8-128">Run the following example to create a container named *aci-demo-secure* that utilizes secured environment variables:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

<span data-ttu-id="508a8-129">A questo punto, quando il contenitore viene restituito con il comando `az container show`, non vengono visualizzate le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="508a8-129">Now, when the container is returned with the `az container show` command, the environment variables are not displayed:</span></span>

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo-secure --query containers[0].environmentVariables
```

<span data-ttu-id="508a8-130">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="508a8-130">Example output:</span></span>

```json
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