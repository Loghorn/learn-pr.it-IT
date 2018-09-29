<span data-ttu-id="71d04-101">Le variabili di ambiente nelle istanze di contenitore consentono di offrire la configurazione dinamica dell'applicazione o dello script eseguiti dal contenitore.</span><span class="sxs-lookup"><span data-stu-id="71d04-101">Environment variables in your container instances allow you to provide dynamic configuration of the application or script run by the container.</span></span> <span data-ttu-id="71d04-102">Per impostare le variabili quando si crea il contenitore, è possibile usare l'interfaccia della riga di comando di Azure, PowerShell o il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="71d04-102">You can use the Azure CLI, PowerShell, or the Azure portal to set variables when you create the container.</span></span> <span data-ttu-id="71d04-103">Le variabili di ambiente protetto vengono usate per impedire che le informazioni riservate vengano visualizzate nell'output del contenitore.</span><span class="sxs-lookup"><span data-stu-id="71d04-103">Secured environment variables are used to prevent sensitive information from being displayed in container output.</span></span>

<span data-ttu-id="71d04-104">In questo caso, si creerà un'istanza di Azure Cosmos DB e si useranno le variabili di ambiente per passare le informazioni di connessione a un'istanza di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="71d04-104">Here, you will create an Azure Cosmos DB instance and use environment variables to pass the connection information to an Azure container instance.</span></span> <span data-ttu-id="71d04-105">Un'applicazione nel contenitore usa le variabili per scrivere e leggere i dati da Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="71d04-105">An application in the container uses the variables to write and read data from Cosmos DB.</span></span> <span data-ttu-id="71d04-106">Si creeranno sia una variabile di ambiente che una variabile di ambiente protetto.</span><span class="sxs-lookup"><span data-stu-id="71d04-106">You will create both an environment variable and a secured environment variable.</span></span>

## <a name="deploy-azure-cosmos-db"></a><span data-ttu-id="71d04-107">Distribuire Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="71d04-107">Deploy Azure Cosmos DB</span></span>

<span data-ttu-id="71d04-108">Creare l'istanza di Azure Cosmos DB con il comando `az cosmosdb create`.</span><span class="sxs-lookup"><span data-stu-id="71d04-108">Create the Azure Cosmos DB instance with the `az cosmosdb create` command.</span></span> <span data-ttu-id="71d04-109">In questo esempio, l'indirizzo dell'endpoint di Azure Cosmos DB viene inoltre inserito in una variabile denominata *COSMOS_DB_ENDPOINT*.</span><span class="sxs-lookup"><span data-stu-id="71d04-109">This example will also place the Azure Cosmos DB endpoint address in a variable named *COSMOS_DB_ENDPOINT*.</span></span> <span data-ttu-id="71d04-110">Sarà necessario fornire un nome univoco per `[cosmos-db-name]`.</span><span class="sxs-lookup"><span data-stu-id="71d04-110">You'll need to supply a unique name for `[cosmos-db-name]`.</span></span>

<span data-ttu-id="71d04-111">Il comando potrebbe richiedere alcuni minuti:</span><span class="sxs-lookup"><span data-stu-id="71d04-111">This command can take a few minutes to complete:</span></span>

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group <rgn>[sandbox resource group name]</rgn> --name [cosmos-db-name] --query documentEndpoint --output tsv)
```

<span data-ttu-id="71d04-112">Successivamente, ottenere la chiave di connessione di Azure Cosmos DB con il comando `az cosmosdb list-keys` e archiviarla in una variabile denominata *COSMOS_DB_MASTERKEY*.</span><span class="sxs-lookup"><span data-stu-id="71d04-112">Next, get the Azure Cosmos DB connection key with the `az cosmosdb list-keys` command and store it in a variable named *COSMOS_DB_MASTERKEY*.</span></span> <span data-ttu-id="71d04-113">Non dimenticare di sostituire `[cosmos-db-name]` nuovamente:</span><span class="sxs-lookup"><span data-stu-id="71d04-113">Don't forget to replace `[cosmos-db-name]` again:</span></span>

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group <rgn>[sandbox resource group name]</rgn> --name [cosmos-db-name] --query primaryMasterKey --output tsv)
```

## <a name="deploy-a-container-instance"></a><span data-ttu-id="71d04-114">Distribuire un'istanza di contenitore</span><span class="sxs-lookup"><span data-stu-id="71d04-114">Deploy a container instance</span></span>

<span data-ttu-id="71d04-115">Creare un'istanza di contenitore di Azure usando il comando `az container create`.</span><span class="sxs-lookup"><span data-stu-id="71d04-115">Create an Azure container instance using the `az container create` command.</span></span> <span data-ttu-id="71d04-116">Si noti che vengono create due variabili di ambiente, `COSMOS_DB_ENDPOINT` e `COSMOS_DB_MASTERKEY`.</span><span class="sxs-lookup"><span data-stu-id="71d04-116">Take note that two environment variables are created, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_MASTERKEY`.</span></span> <span data-ttu-id="71d04-117">Queste variabili contengono i valori necessari per connettersi all'istanza di Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="71d04-117">These variables hold the values needed to connect to the Azure Cosmos DB instance:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --location eastus \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

<span data-ttu-id="71d04-118">Dopo aver creato il contenitore, ottenere l'indirizzo IP usando il comando `az container show`:</span><span class="sxs-lookup"><span data-stu-id="71d04-118">Once the container has been created, get the IP address with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo \
    --query ipAddress.ip \
    --output tsv
```

<span data-ttu-id="71d04-119">Aprire un browser e passare all'indirizzo IP del contenitore.</span><span class="sxs-lookup"><span data-stu-id="71d04-119">Open a browser and navigate to the IP address of the container.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="71d04-120">A volte i contenitori impiegano un minuto o due per avviarsi completamente ed essere in grado di ricevere i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="71d04-120">Sometimes containers take a minute or two to fully start up and be able to receive connections.</span></span> <span data-ttu-id="71d04-121">Se non si riceve alcuna risposta quando si naviga verso l'indirizzo IP nel browser, attendere qualche istante e aggiornare la pagina.</span><span class="sxs-lookup"><span data-stu-id="71d04-121">If there's no response when you navigate to the IP address in your browser,  wait a few moments and refresh the page.</span></span>

 <span data-ttu-id="71d04-122">Una volta che l'applicazione è disponibile, dovrebbe venire visualizzata l'applicazione seguente.</span><span class="sxs-lookup"><span data-stu-id="71d04-122">Once the app is available, you should see the following application.</span></span> <span data-ttu-id="71d04-123">Quando si esegue il cast di un voto, il voto viene archiviato nell'istanza di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="71d04-123">When casting a vote, the vote is stored in the Azure Cosmos DB instance.</span></span>

![Applicazione di votazione di Azure con due opzioni, gatti o cani.](../media/4-azure-vote.png)

## <a name="secured-environment-variables"></a><span data-ttu-id="71d04-125">Variabili di ambiente protetto</span><span class="sxs-lookup"><span data-stu-id="71d04-125">Secured environment variables</span></span>

<span data-ttu-id="71d04-126">Nell'esercizio precedente è stato creato un contenitore con le informazioni di connessione ad Azure Cosmos DB archiviate in due variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="71d04-126">In the previous exercise, a container was created with connection information for Azure Cosmos DB stored in two environment variables.</span></span> <span data-ttu-id="71d04-127">Per impostazione predefinita, le variabili di ambiente vengono visualizzate nel portale di Azure e negli strumenti da riga di comando in formato testo normale.</span><span class="sxs-lookup"><span data-stu-id="71d04-127">By default, environment variables are displayed in the Azure portal and command-line tools in plain text.</span></span>

<span data-ttu-id="71d04-128">Ad esempio, se si ottengono informazioni sul contenitore creato nell'esercizio precedente con il comando `az container show`, le variabili di ambiente sono accessibili in formato testo normale:</span><span class="sxs-lookup"><span data-stu-id="71d04-128">For example, if you get information about the container created in the previous exercise with the `az container show` command, the environment variables are accessible in plain text:</span></span>

```azurecli
az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name aci-demo --query containers[0].environmentVariables
```

<span data-ttu-id="71d04-129">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="71d04-129">Example output:</span></span>

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

Le variabili di ambiente protetto impediscono l'output di testo non crittografato. <span data-ttu-id="71d04-131">Per usare le variabili di ambiente protetto, sostituire l'argomento `--environment-variables` con l'argomento `--secure-environment-variables`.</span><span class="sxs-lookup"><span data-stu-id="71d04-131">To use secure environment variables, replace the `--environment-variables` argument with the `--secure-environment-variables` argument.</span></span>

<span data-ttu-id="71d04-132">Eseguire l'esempio seguente per creare un contenitore denominato *aci-demo-secure* che usa le variabili di ambiente protetto:</span><span class="sxs-lookup"><span data-stu-id="71d04-132">Run the following example to create a container named *aci-demo-secure* that utilizes secured environment variables:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --location eastus \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

<span data-ttu-id="71d04-133">A questo punto, quando il contenitore viene restituito con il comando `az container show`, non vengono visualizzate le variabili di ambiente:</span><span class="sxs-lookup"><span data-stu-id="71d04-133">Now, when the container is returned with the `az container show` command, the environment variables are not displayed:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --query containers[0].environmentVariables
```

<span data-ttu-id="71d04-134">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="71d04-134">Example output:</span></span>

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