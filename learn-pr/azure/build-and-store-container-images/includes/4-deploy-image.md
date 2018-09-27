<span data-ttu-id="f467a-101">È possibile eseguire il pull di immagini del contenitore dal Registro contenitori di Azure usando molte piattaforme di gestione dei contenitori, ad esempio Istanze di contenitore di Azure, il registro Kubernetes di Azure e Docker per Windows o Mac.</span><span class="sxs-lookup"><span data-stu-id="f467a-101">Container images can be pulled from Azure Container Registry using many container management platforms, such as Azure Container Instances, Azure Kubernetes Registry, and Docker for Windows or Mac.</span></span> <span data-ttu-id="f467a-102">Quando si eseguono immagini del contenitore dal Registro contenitori di Azure, potrebbero essere necessarie credenziali di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f467a-102">When running container images from Azure Container Registry, authentication credentials may be needed.</span></span> 

<span data-ttu-id="f467a-103">È consigliabile usare un'entità servizio di Azure per l'autenticazione con Container Registry.</span><span class="sxs-lookup"><span data-stu-id="f467a-103">It is recommended to use an Azure service principal for authentication with Container Registry.</span></span> <span data-ttu-id="f467a-104">È anche consigliabile proteggere le credenziali dell'entità servizio di Azure in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f467a-104">It is also recommended to secure the Azure service principal credentials in Azure Key Vault.</span></span> <span data-ttu-id="f467a-105">In questa unità verrà descritto l'approccio consigliato.</span><span class="sxs-lookup"><span data-stu-id="f467a-105">In this unit we'll follow the recommended approach.</span></span>

<span data-ttu-id="f467a-106">Tuttavia, per l'esercitazione in tempo reale verrà usato l'account amministratore predefinito che può essere abilitato in tutti i registri contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="f467a-106">However, for our live practice we will use the built-in admin account that can be enabled in all Azure Container Registries.</span></span> <span data-ttu-id="f467a-107">L'account amministratore viene usato con le risorse della sandbox gratuita.</span><span class="sxs-lookup"><span data-stu-id="f467a-107">The admin account works with the free sandbox resources.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="f467a-108">Creare una variabile con il nome del registro contenitori in lettere minuscole (ad esempio, "contenitore" invece di "Contenitore").</span><span class="sxs-lookup"><span data-stu-id="f467a-108">Create a variable with the name of your container registry in lowercase (for example, instead of "MyContainer" make the value "mycontainer").</span></span> <span data-ttu-id="f467a-109">Questa variabile verrà usata in tutta l'unità.</span><span class="sxs-lookup"><span data-stu-id="f467a-109">This variable is used throughout this unit.</span></span>

```azurecli
ACR_NAME=<acrName>
```

### <a name="service-principal"></a><span data-ttu-id="f467a-110">Entità servizio</span><span class="sxs-lookup"><span data-stu-id="f467a-110">Service Principal</span></span>

<span data-ttu-id="f467a-111">Per un'applicazione di produzione sarebbe a questo punto opportuno creare un'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="f467a-111">For a production application, we would want to create a service principal here.</span></span> <span data-ttu-id="f467a-112">Ricordare che **questo non funzionerà nell'ambiente sandbox**, ma è una procedura consigliata da adottare nei sistemi reali.</span><span class="sxs-lookup"><span data-stu-id="f467a-112">Remember **this won't work in our sandbox environment**, but it's a best practice to follow in your own systems.</span></span> <span data-ttu-id="f467a-113">Per l'esercitazione pratica verranno usate le istruzioni dell'account amministratore riportate sotto.</span><span class="sxs-lookup"><span data-stu-id="f467a-113">For our live practice, you'll use the Admin Account instructions below.</span></span>

<span data-ttu-id="f467a-114">Per creare l'entità servizio, è possibile usare il comando `az ad sp create-for-rbac`.</span><span class="sxs-lookup"><span data-stu-id="f467a-114">The `az ad sp create-for-rbac` command can be used to create the service principal.</span></span> <span data-ttu-id="f467a-115">L'argomento `--role` configura l'entità servizio con il ruolo *lettore*, che concede l'accesso di tipo solo pull al registro.</span><span class="sxs-lookup"><span data-stu-id="f467a-115">The `--role` argument configures the service principal with the *reader* role, which grants it pull-only access to the registry.</span></span> <span data-ttu-id="f467a-116">Per concedere l'accesso sia push che pull, impostare l'argomento `--role` su *collaboratore*.</span><span class="sxs-lookup"><span data-stu-id="f467a-116">To grant both push and pull access, you would change the `--role` argument to *contributor*.</span></span>

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

<span data-ttu-id="f467a-117">L'output della creazione dell'entità servizio avrà un aspetto simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="f467a-117">This is what the output of the service principal creation would look like.</span></span> <span data-ttu-id="f467a-118">Prendere nota dei valori `appId` e `password`.</span><span class="sxs-lookup"><span data-stu-id="f467a-118">Take note of the `appId` and the `password` values.</span></span> <span data-ttu-id="f467a-119">Questi valori devono essere archiviati nell'insieme di credenziali delle chiavi di Azure.</span><span class="sxs-lookup"><span data-stu-id="f467a-119">These should be stored in the Azure key vault.</span></span>

```output
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="admin-account"></a><span data-ttu-id="f467a-120">Account amministratore</span><span class="sxs-lookup"><span data-stu-id="f467a-120">Admin Account</span></span>

<span data-ttu-id="f467a-121">I registri contenitori di Azure sono dotati di un account amministratore predefinito.</span><span class="sxs-lookup"><span data-stu-id="f467a-121">Azure Container Registries come with a built-in Admin account.</span></span> <span data-ttu-id="f467a-122">Questo account non è collegato ad Azure AD o ai controlli degli accessi in base al ruolo e di conseguenza **deve essere usato solo a scopo di test**.</span><span class="sxs-lookup"><span data-stu-id="f467a-122">This isn't tied to Azure AD or role based access controls and therefore **should only be used for testing**.</span></span> 

1. <span data-ttu-id="f467a-123">Abilitare l'account amministratore.</span><span class="sxs-lookup"><span data-stu-id="f467a-123">Enable the Admin Account.</span></span>
    ```azurecli
      az acr update -n $ACR_NAME --admin-enabled true
    ```

2. <span data-ttu-id="f467a-124">Eseguire una query per ottenere il nome utente e la password generati automaticamente</span><span class="sxs-lookup"><span data-stu-id="f467a-124">Query to get the autogenerated username and password</span></span>

    ```azurecli
      az acr credential show --name $ACR_NAME
    ```

<span data-ttu-id="f467a-125">L'output è simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="f467a-125">The output is similar to below.</span></span> <span data-ttu-id="f467a-126">Prendere nota di `username` e `value` abbinati alla "password" `name`.</span><span class="sxs-lookup"><span data-stu-id="f467a-126">Take note of the `username` and the `value` paired with the `name` "password".</span></span> <span data-ttu-id="f467a-127">Salvare tali valori nell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="f467a-127">You'll save these into key vault.</span></span>

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

### <a name="save-the-username-and-password-to-the-key-vault"></a><span data-ttu-id="f467a-128">Salvare il nome utente e la password nell'insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="f467a-128">Save the username and password to the key vault</span></span>

1. <span data-ttu-id="f467a-129">Creare un insieme di credenziali delle chiavi di Azure con il comando `az keyvault create`.</span><span class="sxs-lookup"><span data-stu-id="f467a-129">Create an Azure Key Vault with the `az keyvault create` command.</span></span>

    ```azurecli
    az keyvault create --resource-group <rgn>[sandbox resource group name]</rgn> --name $ACR_NAME-keyvault
    ```

1. <span data-ttu-id="f467a-130">Usare il comando `az keyvault secret set` per archiviare il nome utente per il registro contenitori di Azure nell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="f467a-130">Use the `az keyvault secret set` command to store the username for the ACR in the vault.</span></span> <span data-ttu-id="f467a-131">Se si usano entità servizio, per questo valore è necessario usare l'ID dell'app.</span><span class="sxs-lookup"><span data-stu-id="f467a-131">If you were using service principals you'd use the appId for this value.</span></span> <span data-ttu-id="f467a-132">Poiché è stato usato l'account amministratore questa volta verrà salvato il nome utente dalla query eseguita in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f467a-132">Since we're using the admin account this time we'll save the username from our query above.</span></span> <span data-ttu-id="f467a-133">Immettere il comando seguente senza dimenticare di sostituire `<username>`.</span><span class="sxs-lookup"><span data-stu-id="f467a-133">Enter the command below, don't forget to replace `<username>`.</span></span>

    ```azurecli
    az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <username>
    ```

1. <span data-ttu-id="f467a-134">Usare il comando `az keyvault secret set` per archiviare la *password* nell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="f467a-134">Use the `az keyvault secret set` command to store the *password* in the vault.</span></span> <span data-ttu-id="f467a-135">Sostituire `<password>` con `password` dalla query precedente.</span><span class="sxs-lookup"><span data-stu-id="f467a-135">Replace `<password>` with the `password` from the query above.</span></span>

    ```azurecli
    az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
    ```

<span data-ttu-id="f467a-136">A questo punto è stato creato un insieme di credenziali delle chiavi di Azure e vi sono stati archiviati due segreti:</span><span class="sxs-lookup"><span data-stu-id="f467a-136">You've now created an Azure key vault and stored two secrets in it:</span></span>

* <span data-ttu-id="f467a-137">`$ACR_NAME-pull-usr`: il **nome utente** del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="f467a-137">`$ACR_NAME-pull-usr`: the container registry **username**.</span></span>
* <span data-ttu-id="f467a-138">`$ACR_NAME-pull-pwd`: la **password** del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="f467a-138">`$ACR_NAME-pull-pwd`: the container registry **password**.</span></span>

<span data-ttu-id="f467a-139">Ora è possibile fare riferimento a questi segreti per nome quando gli utenti o le applicazioni e i servizi eseguono il pull di immagini dal registro.</span><span class="sxs-lookup"><span data-stu-id="f467a-139">You can now reference these secrets by name when you or your applications and services pull images from the registry.</span></span>

### <a name="deploy-a-container-with-azure-cli"></a><span data-ttu-id="f467a-140">Distribuire un contenitore con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="f467a-140">Deploy a container with Azure CLI</span></span>

<span data-ttu-id="f467a-141">Ora che le credenziali dell'entità servizio sono archiviate in Azure Key Vault, le applicazioni e i servizi possono usarle per accedere al registro privato.</span><span class="sxs-lookup"><span data-stu-id="f467a-141">Now that the service principal credentials are stored in Azure Key Vault, your applications and services can use them to access your private registry.</span></span>

<span data-ttu-id="f467a-142">Eseguire il comando `az container create` seguente per distribuire un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="f467a-142">Execute the following `az container create` command to deploy a container instance.</span></span> <span data-ttu-id="f467a-143">Il comando usa le credenziali dell'entità servizio archiviate in Azure Key Vault per eseguire l'autenticazione al registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="f467a-143">The command uses the service principal's credentials stored in Azure Key Vault to authenticate to your container registry.</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name acr-tasks \
    --image $ACR_NAME.azurecr.io/helloacrtasks:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --location eastus \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

<span data-ttu-id="f467a-144">Ottenere l'indirizzo IP dell'istanza di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="f467a-144">Get the IP address of the Azure container instance.</span></span>

```azurecli
az container show --resource-group  <rgn>[sandbox resource group name]</rgn> --name acr-tasks --query ipAddress.ip --output table
```

<span data-ttu-id="f467a-145">Aprire un browser e passare all'indirizzo IP del contenitore.</span><span class="sxs-lookup"><span data-stu-id="f467a-145">Open a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="f467a-146">Se tutto è configurato correttamente, si dovrebbero visualizzare i risultati seguenti:</span><span class="sxs-lookup"><span data-stu-id="f467a-146">If everything has been configured correctly, you should see the following results:</span></span>

![Applicazione Web di esempio con il testo Hello World](../media/hello.png)

