<span data-ttu-id="d87ab-101">È possibile eseguire il pull di immagini del contenitore dal Registro contenitori di Azure da molte piattaforme di gestione contenitori, ad esempio Istanze di contenitore di Azure, servizio Kubernetes di Azure e Docker per Windows o Mac.</span><span class="sxs-lookup"><span data-stu-id="d87ab-101">Container images can be pulled from Azure Container Registry from many container management platforms, such as Azure Container Instances, Azure Kubernetes Registry, and Docker for Windows or Mac.</span></span> <span data-ttu-id="d87ab-102">Quando si eseguono immagini del contenitore dal Registro contenitori di Azure, potrebbero essere necessarie credenziali di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d87ab-102">When running container images from Azure Container Registry, authentication credentials may be needed.</span></span> <span data-ttu-id="d87ab-103">È consigliabile usare un'entità servizio di Azure per l'autenticazione con Container Registry.</span><span class="sxs-lookup"><span data-stu-id="d87ab-103">It is recommended to use an Azure service principal for authentication with Container Registry.</span></span> <span data-ttu-id="d87ab-104">Inoltre, è consigliabile anche proteggere le credenziali dell'entità servizio di Azure in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d87ab-104">Furthermore, it is also recommended to secure the Azure service principal credentials in Azure Key Vault.</span></span>

<span data-ttu-id="d87ab-105">In questa unità si crea un'entità servizio per il registro contenitori di Azure, la si archivia in Azure Key Vault e quindi si distribuisce il contenitore in Istanze di contenitore di Azure usando le credenziali dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="d87ab-105">In this unit, you will create a service principal for your Azure container registry, store it in Azure Key Vault, and then deploy the container to Azure Container Instances using the service principal's credentials.</span></span>

## <a name="configure-registry-authentication"></a><span data-ttu-id="d87ab-106">Configurare l'autenticazione del registro</span><span class="sxs-lookup"><span data-stu-id="d87ab-106">Configure registry authentication</span></span>

<span data-ttu-id="d87ab-107">In tutti gli scenari di produzione dovrebbero essere usate entità servizio per accedere a un registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="d87ab-107">All production scenarios should use service principals to access an Azure container registry.</span></span> <span data-ttu-id="d87ab-108">Le entità servizio consentono di fornire il controllo degli accessi in base al ruolo alle immagini del contenitore.</span><span class="sxs-lookup"><span data-stu-id="d87ab-108">Service principals allow you to provide role-based access control (RBAC) to your container images.</span></span> <span data-ttu-id="d87ab-109">Ad esempio, è possibile configurare un'entità servizio con accesso solo pull a un registro.</span><span class="sxs-lookup"><span data-stu-id="d87ab-109">For example, you can configure a service principal with pull-only access to a registry.</span></span>

<span data-ttu-id="d87ab-110">Se non si ha già un insieme di credenziali in Azure Key Vault, crearne uno usando i comandi seguenti nell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="d87ab-110">If you don't already have a vault in Azure Key Vault, create one with the Azure CLI using the following commands.</span></span>

<span data-ttu-id="d87ab-111">Prima di tutto, creare una variabile con il nome del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="d87ab-111">First, create a variable with the name of your container registry.</span></span> <span data-ttu-id="d87ab-112">Questa variabile verrà usata in tutta l'unità.</span><span class="sxs-lookup"><span data-stu-id="d87ab-112">This variable is used throughout this unit.</span></span>

```azurecli
ACR_NAME=<acrName>
```

<span data-ttu-id="d87ab-113">Creare un insieme di credenziali delle chiavi di Azure con il comando `az keyvault create`.</span><span class="sxs-lookup"><span data-stu-id="d87ab-113">Create an Azure key vault with the `az keyvault create` command.</span></span>

```azurecli
az keyvault create --resource-group myResourceGroup --name $ACR_NAME-keyvault
```

<span data-ttu-id="d87ab-114">A questo punto occorre creare un'entità servizio e archiviarne le credenziali nell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="d87ab-114">Now, you need to create a service principal and store its credentials in your key vault.</span></span>

<span data-ttu-id="d87ab-115">Per creare l'entità servizio, usare il comando `az ad sp create-for-rbac`.</span><span class="sxs-lookup"><span data-stu-id="d87ab-115">Use the `az ad sp create-for-rbac` command to create the service principal.</span></span> <span data-ttu-id="d87ab-116">L'argomento `--role` configura l'entità servizio con il ruolo *lettore*, che concede l'accesso di tipo solo pull al registro.</span><span class="sxs-lookup"><span data-stu-id="d87ab-116">The `--role` argument configures the service principal with the *reader* role, which grants it pull-only access to the registry.</span></span> <span data-ttu-id="d87ab-117">Per concedere l'accesso sia push che pull, impostare l'argomento `--role` su *collaboratore*.</span><span class="sxs-lookup"><span data-stu-id="d87ab-117">To grant both push and pull access, change the `--role` argument to *contributor*.</span></span>

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

<span data-ttu-id="d87ab-118">L'output della creazione dell'entità servizio avrà un aspetto simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="d87ab-118">This is what the output of the service principal creation will look like.</span></span> <span data-ttu-id="d87ab-119">Prendere nota dei valori `appId` e `password`.</span><span class="sxs-lookup"><span data-stu-id="d87ab-119">Take note of the `appId` and the `password` values.</span></span> <span data-ttu-id="d87ab-120">Questi valori verranno archiviati dell'insieme di credenziali delle chiavi di Azure.</span><span class="sxs-lookup"><span data-stu-id="d87ab-120">These will be stored in the Azure key vault.</span></span>

```bash
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

<span data-ttu-id="d87ab-121">Successivamente, usare il comando `az keyvault secret set` per archiviare l'*appId* dell'entità servizio nell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="d87ab-121">Next, use the `az keyvault secret set` command to store the service principal's *appId* in the vault.</span></span> <span data-ttu-id="d87ab-122">Sostituire `<appId>` con il valore `appId` dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="d87ab-122">Replace `<appId>` with the `appId` of the service principal.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <appId>
```

<span data-ttu-id="d87ab-123">A questo punto, usare il comando `az keyvault secret set` per archiviare la *password* dell'entità servizio nell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="d87ab-123">Now, use the `az keyvault secret set` command to store the service principal's *password* in the vault.</span></span> <span data-ttu-id="d87ab-124">Sostituire `<password>` con il valore `password` dell'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="d87ab-124">Replace `<password>` with the `password` of the service principal.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
```

<span data-ttu-id="d87ab-125">È stato creato un insieme di credenziali delle chiavi di Azure e vi sono stati archiviati due segreti:</span><span class="sxs-lookup"><span data-stu-id="d87ab-125">You've created an Azure key vault and stored two secrets in it:</span></span>

* <span data-ttu-id="d87ab-126">`$ACR_NAME-pull-usr`: ID dell'entità servizio, da usare come **nome utente** del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="d87ab-126">`$ACR_NAME-pull-usr`: The service principal ID, for use as the container registry **username**.</span></span>
* <span data-ttu-id="d87ab-127">`$ACR_NAME-pull-pwd`: password dell'entità servizio, da usare come **password** del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="d87ab-127">`$ACR_NAME-pull-pwd`: The service principal password, for use as the container registry **password**.</span></span>

<span data-ttu-id="d87ab-128">Ora è possibile fare riferimento a questi segreti per nome quando gli utenti o le applicazioni e i servizi eseguono il pull di immagini dal registro.</span><span class="sxs-lookup"><span data-stu-id="d87ab-128">You can now reference these secrets by name when you or your applications and services pull images from the registry.</span></span>

### <a name="deploy-a-container-with-azure-cli"></a><span data-ttu-id="d87ab-129">Distribuire un contenitore con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="d87ab-129">Deploy a container with Azure CLI</span></span>

<span data-ttu-id="d87ab-130">Ora che le credenziali dell'entità servizio sono archiviate in Azure Key Vault, le applicazioni e i servizi possono usarle per accedere al registro privato.</span><span class="sxs-lookup"><span data-stu-id="d87ab-130">Now that the service principal credentials are stored in Azure Key Vault, your applications and services can use them to access your private registry.</span></span>

<span data-ttu-id="d87ab-131">Eseguire il comando `az container create` seguente per distribuire un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="d87ab-131">Execute the following `az container create` command to deploy a container instance.</span></span> <span data-ttu-id="d87ab-132">Il comando usa le credenziali dell'entità servizio archiviate in Azure Key Vault per eseguire l'autenticazione al registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="d87ab-132">The command uses the service principal's credentials stored in Azure Key Vault to authenticate to your container registry.</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

<span data-ttu-id="d87ab-133">Ottenere l'indirizzo IP dell'istanza di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="d87ab-133">Get the IP address of the Azure container instance.</span></span>

```azurecli
az container show --resource-group myResourceGroup --name acr-build --query ipAddress.ip --output table
```

<span data-ttu-id="d87ab-134">Aprire un browser e passare all'indirizzo IP del contenitore.</span><span class="sxs-lookup"><span data-stu-id="d87ab-134">Open up a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="d87ab-135">Se tutto è configurato correttamente, si dovrebbero visualizzare i risultati seguenti:</span><span class="sxs-lookup"><span data-stu-id="d87ab-135">If everything has been configured correctly, you should see the following results:</span></span>

![Applicazione Web di esempio con il testo Hello World](../media/hello.png)

## <a name="summary"></a><span data-ttu-id="d87ab-137">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="d87ab-137">Summary</span></span>

<span data-ttu-id="d87ab-138">In questa unità, le credenziali del Registro contenitori di Azure sono state archiviate in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d87ab-138">In this unit, the Azure Container Registry credentials were stored in Azure Key Vault.</span></span> <span data-ttu-id="d87ab-139">Un'immagine del contenitore è stata quindi distribuita in Istanze di contenitore di Azure tramite l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="d87ab-139">A container image was then deployed to Azure Container Instances using the Azure CLI.</span></span> <span data-ttu-id="d87ab-140">Nella prossima unità si userà la replica geografica del Registro contenitori di Azure per copiare l'immagine del contenitore in più data center di Azure.</span><span class="sxs-lookup"><span data-stu-id="d87ab-140">In the next unit, you will use Azure Container Registry geo-replication to copy the container image to multiple Azure datacenters.</span></span>
