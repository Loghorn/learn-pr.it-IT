<span data-ttu-id="ac504-101">È possibile eseguire il pull di immagini del contenitore dal Registro contenitori di Azure da molte piattaforme di gestione contenitori, ad esempio Istanze di contenitore di Azure, servizio Kubernetes di Azure e Docker per Windows o Mac.</span><span class="sxs-lookup"><span data-stu-id="ac504-101">Container images can be pulled from Azure Container Registry from many container management platforms, such as Azure Container Instances, Azure Kubernetes Registry, and Docker for Windows or Mac.</span></span> <span data-ttu-id="ac504-102">Quando si eseguono immagini del contenitore dal Registro contenitori di Azure, potrebbero essere necessarie credenziali di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="ac504-102">When running container images from Azure Container Registry, authentication credentials may be needed.</span></span> 

<span data-ttu-id="ac504-103">È consigliabile usare un'entità servizio di Azure per l'autenticazione con Container Registry.</span><span class="sxs-lookup"><span data-stu-id="ac504-103">It is recommended to use an Azure service principal for authentication with Container Registry.</span></span> <span data-ttu-id="ac504-104">È anche consigliabile proteggere le credenziali dell'entità servizio di Azure in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ac504-104">Furthermore, it is also recommended to secure the Azure service principal credentials in Azure Key Vault.</span></span> <span data-ttu-id="ac504-105">Dal momento che si tratta dell'approccio consigliato, in questa unità ne verrà esaminato il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="ac504-105">Since that's the recommended approach we will examine how it would be done in this unit.</span></span>

<span data-ttu-id="ac504-106">Tuttavia, per l'esercitazione in tempo reale verrà usato l'account amministratore predefinito che può essere abilitato in tutti i registri contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac504-106">However, for our live practice we will use the built-in admin account that can be enabled in all Azure Container Registries.</span></span> <span data-ttu-id="ac504-107">L'account amministratore viene usato con le risorse del sandbox gratuito.</span><span class="sxs-lookup"><span data-stu-id="ac504-107">The admin account works with our free sandbox resources.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="ac504-108">Per prima cosa, creare una variabile con il nome del registro contenitori in lettere minuscole (ad esempio, "mycontainer" invece di "MyContainer").</span><span class="sxs-lookup"><span data-stu-id="ac504-108">First, create a variable with the name of your container registry in lowercase (for example, intstead of "MyContainer" make the value "mycontainer").</span></span> <span data-ttu-id="ac504-109">Questa variabile verrà usata in tutta l'unità.</span><span class="sxs-lookup"><span data-stu-id="ac504-109">This variable is used throughout this unit.</span></span>

```azurecli
ACR_NAME=<acrName>
```

### <a name="service-principal"></a><span data-ttu-id="ac504-110">Entità servizio</span><span class="sxs-lookup"><span data-stu-id="ac504-110">Service Principal</span></span>

<span data-ttu-id="ac504-111">A questo punto, si vuole creare un'entità servizio per un'applicazione di produzione.</span><span class="sxs-lookup"><span data-stu-id="ac504-111">Now, for a production application, we would want to create a service principal here.</span></span> <span data-ttu-id="ac504-112">Ricordare che **questo non funzionerà nell'ambiente sandbox**, ma è una procedura consigliata nei sistemi in uso.</span><span class="sxs-lookup"><span data-stu-id="ac504-112">Remember **this won't work in our sandbox environment**, but it's a best practice in your own systems.</span></span> <span data-ttu-id="ac504-113">Per l'esercitazione pratica verranno usate le istruzioni dell'account amministratore riportate sotto.</span><span class="sxs-lookup"><span data-stu-id="ac504-113">For our live practice you'll use the Admin Account instructions below.</span></span>

<span data-ttu-id="ac504-114">Per creare l'entità servizio, è possibile usare il comando `az ad sp create-for-rbac`.</span><span class="sxs-lookup"><span data-stu-id="ac504-114">The `az ad sp create-for-rbac` command can be used to create the service principal.</span></span> <span data-ttu-id="ac504-115">L'argomento `--role` configura l'entità servizio con il ruolo *lettore*, che concede l'accesso di tipo solo pull al registro.</span><span class="sxs-lookup"><span data-stu-id="ac504-115">The `--role` argument configures the service principal with the *reader* role, which grants it pull-only access to the registry.</span></span> <span data-ttu-id="ac504-116">Per concedere l'accesso sia push che pull, impostare l'argomento `--role` su *collaboratore*.</span><span class="sxs-lookup"><span data-stu-id="ac504-116">To grant both push and pull access, you would change the `--role` argument to *contributor*.</span></span>

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

<span data-ttu-id="ac504-117">L'output della creazione dell'entità servizio avrà un aspetto simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="ac504-117">This is what the output of the service principal creation would look like.</span></span> <span data-ttu-id="ac504-118">Prendere nota dei valori `appId` e `password`.</span><span class="sxs-lookup"><span data-stu-id="ac504-118">Take note of the `appId` and the `password` values.</span></span> <span data-ttu-id="ac504-119">Questi valori devono essere archiviati nell'insieme di credenziali delle chiavi di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac504-119">These should be stored in the Azure key vault.</span></span>

```output
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="admin-account"></a><span data-ttu-id="ac504-120">Account amministratore</span><span class="sxs-lookup"><span data-stu-id="ac504-120">Admin Account</span></span>

<span data-ttu-id="ac504-121">I registri contenitori di Azure sono dotati di un account amministratore predefinito.</span><span class="sxs-lookup"><span data-stu-id="ac504-121">Azure Container Registries come with a built-in Admin account.</span></span> <span data-ttu-id="ac504-122">Tale account non è collegato ad Azure AD o ai controlli degli accessi in base al ruolo e di conseguenza **deve essere usato solo a scopo di test**.</span><span class="sxs-lookup"><span data-stu-id="ac504-122">This isn't tied to Azure AD or role based access controls and therefore **should only be used for testing**.</span></span> 

<span data-ttu-id="ac504-123">Prima di tutto è necessario abilitare l'account amministratore</span><span class="sxs-lookup"><span data-stu-id="ac504-123">First we must enable the Admin Account</span></span>
```azurecli
  az acr update -n $ACR_NAME --admin-enabled true
```

<span data-ttu-id="ac504-124">A questo punto eseguire una query per ottenere il nome utente e la password generati automaticamente</span><span class="sxs-lookup"><span data-stu-id="ac504-124">Now query to get the autogenerated username and password</span></span>

```azurecli
  az acr credential show --name $ACR_NAME
```

<span data-ttu-id="ac504-125">L'output è simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="ac504-125">Output is similar to below.</span></span> <span data-ttu-id="ac504-126">Prendere nota di `username` e `value` abbinati alla "password" `name`.</span><span class="sxs-lookup"><span data-stu-id="ac504-126">Take note of the `username`, and the `value` paired with the `name` "password".</span></span> <span data-ttu-id="ac504-127">Salvare tali valori nell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="ac504-127">You'll save these into key vault.</span></span>

```output
{  "passwords": [
    {
      "name": "password",
      "value": "aaaaa"
    },
    {
      "name": "password2",
      "value": "bbbbb"
    }
  ],
  "username": "ccccc"
}
```

### <a name="save-the-username-and-password-to-keyvault"></a><span data-ttu-id="ac504-128">Immettere il nome utente e la password nell'insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="ac504-128">Save the username and password to keyvault</span></span>

<span data-ttu-id="ac504-129">Creare un insieme di credenziali delle chiavi di Azure con il comando `az keyvault create`.</span><span class="sxs-lookup"><span data-stu-id="ac504-129">Create an Azure key vault with the `az keyvault create` command.</span></span>

```azurecli
az keyvault create --resource-group <rgn>[Sandbox resource group name]</rgn> --name $ACR_NAME-keyvault
```

<span data-ttu-id="ac504-130">Successivamente, usare il comando `az keyvault secret set` per archiviare il nome utente per il registro contenitori di Azure nell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="ac504-130">Next, use the `az keyvault secret set` command to store the username for the ACR in the vault.</span></span> <span data-ttu-id="ac504-131">Se si usano entità servizio, per questo valore è necessario usare l'id dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac504-131">If you were using service principals you'd use the appId for this value.</span></span> <span data-ttu-id="ac504-132">Poiché è stato usato l'account amministratore questa volta verrà salvato il nome utente dalla query eseguita in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ac504-132">Since we're using the admin account this time we'll save the username from our query above.</span></span> <span data-ttu-id="ac504-133">Una volta eseguito il comando qui sotto, non dimenticare di sostituire `<username>`.</span><span class="sxs-lookup"><span data-stu-id="ac504-133">Issue the command below, don't forget to replace `<username>`.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <username>
```

<span data-ttu-id="ac504-134">A questo punto, usare il comando `az keyvault secret set` per archiviare la *password* nell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="ac504-134">Now, use the `az keyvault secret set` command to store the *password* in the vault.</span></span> <span data-ttu-id="ac504-135">Sostituire `<password>` con `password` dalla query precedente.</span><span class="sxs-lookup"><span data-stu-id="ac504-135">Replace `<password>` with the `password` from the query above.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
```

<span data-ttu-id="ac504-136">È stato creato un insieme di credenziali delle chiavi di Azure e vi sono stati archiviati due segreti:</span><span class="sxs-lookup"><span data-stu-id="ac504-136">You've created an Azure key vault and stored two secrets in it:</span></span>

* <span data-ttu-id="ac504-137">`$ACR_NAME-pull-usr`: il **nome utente** del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="ac504-137">`$ACR_NAME-pull-usr`: the container registry **username**.</span></span>
* <span data-ttu-id="ac504-138">`$ACR_NAME-pull-pwd`: la **password** del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="ac504-138">`$ACR_NAME-pull-pwd`: the container registry **password**.</span></span>

<span data-ttu-id="ac504-139">Ora è possibile fare riferimento a questi segreti per nome quando gli utenti o le applicazioni e i servizi eseguono il pull di immagini dal registro.</span><span class="sxs-lookup"><span data-stu-id="ac504-139">You can now reference these secrets by name when you or your applications and services pull images from the registry.</span></span>

### <a name="deploy-a-container-with-azure-cli"></a><span data-ttu-id="ac504-140">Distribuire un contenitore con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="ac504-140">Deploy a container with Azure CLI</span></span>

<span data-ttu-id="ac504-141">Ora che le credenziali dell'entità servizio sono archiviate in Azure Key Vault, le applicazioni e i servizi possono usarle per accedere al registro privato.</span><span class="sxs-lookup"><span data-stu-id="ac504-141">Now that the service principal credentials are stored in Azure Key Vault, your applications and services can use them to access your private registry.</span></span>

<span data-ttu-id="ac504-142">Eseguire il comando `az container create` seguente per distribuire un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="ac504-142">Execute the following `az container create` command to deploy a container instance.</span></span> <span data-ttu-id="ac504-143">Il comando usa le credenziali dell'entità servizio archiviate in Azure Key Vault per eseguire l'autenticazione al registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="ac504-143">The command uses the service principal's credentials stored in Azure Key Vault to authenticate to your container registry.</span></span>

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --location eastus \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

<span data-ttu-id="ac504-144">Ottenere l'indirizzo IP dell'istanza di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac504-144">Get the IP address of the Azure container instance.</span></span>

```azurecli
az container show --resource-group  <rgn>[Sandbox resource group name]</rgn> --name acr-build --query ipAddress.ip --output table
```

<span data-ttu-id="ac504-145">Aprire un browser e passare all'indirizzo IP del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ac504-145">Open up a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="ac504-146">Se tutto è configurato correttamente, si dovrebbero visualizzare i risultati seguenti:</span><span class="sxs-lookup"><span data-stu-id="ac504-146">If everything has been configured correctly, you should see the following results:</span></span>

![Applicazione Web di esempio con il testo Hello World](../media/hello.png)

