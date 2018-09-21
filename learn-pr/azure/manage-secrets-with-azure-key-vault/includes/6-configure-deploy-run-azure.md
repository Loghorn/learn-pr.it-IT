<span data-ttu-id="d1bae-101">A questo punto è possibile eseguire l'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="d1bae-101">Now it's time to run our app in Azure.</span></span> <span data-ttu-id="d1bae-102">È necessario creare un'app del Servizio app di Azure, configurarla con un'identità gestita e la configurazione dell'insieme di credenziali e quindi distribuire il codice.</span><span class="sxs-lookup"><span data-stu-id="d1bae-102">We need to create an Azure App Service app, set it up with a managed identity and our vault configuration, and deploy our code.</span></span>

## <a name="create-the-app-service-plan-and-app"></a><span data-ttu-id="d1bae-103">Creare l'app e il piano di servizio app</span><span class="sxs-lookup"><span data-stu-id="d1bae-103">Create the App Service plan and app</span></span>

<span data-ttu-id="d1bae-104">La creazione di un'app di Servizio app è un processo che si svolge in due passaggi: per prima cosa, creare il *piano* e poi l'*app*.</span><span class="sxs-lookup"><span data-stu-id="d1bae-104">Creating an App Service app is a two-step process: First create the *plan*, then the *app*.</span></span>

<span data-ttu-id="d1bae-105">Il nome del *piano* deve solamente essere univoco all'interno della sottoscrizione, quindi è possibile usare lo stesso nome usato da noi: **insieme di credenziali delle chiavi-esercizio-piano**.</span><span class="sxs-lookup"><span data-stu-id="d1bae-105">The *plan* name only needs to be unique within your subscription, so you can use the same name we've used: **keyvault-exercise-plan**.</span></span> <span data-ttu-id="d1bae-106">Il nome dell'app deve essere globalmente univoco, pertanto è necessario sceglierne uno proprio.</span><span class="sxs-lookup"><span data-stu-id="d1bae-106">The app name needs to be globally unique, though, so you'll need to pick your own.</span></span> <span data-ttu-id="d1bae-107">Usare lo stesso percorso usato durante la creazione dell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="d1bae-107">Use the same location you used when creating the vault.</span></span>

<span data-ttu-id="d1bae-108">In Azure Cloud Shell eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d1bae-108">In Azure Cloud Shell, run the following:</span></span>

```azurecli
az appservice plan create \
    --name keyvault-exercise-plan \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
    --location eastus

az webapp create \
    --name <your-unique-app-name> \
    --plan keyvault-exercise-plan \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

## <a name="add-configuration-to-the-app"></a><span data-ttu-id="d1bae-109">Aggiungere una configurazione per l'app</span><span class="sxs-lookup"><span data-stu-id="d1bae-109">Add configuration to the app</span></span>

<span data-ttu-id="d1bae-110">Per la distribuzione in Azure si seguirà la procedura consigliata del servizio app per inserire la configurazione VaultName nelle impostazioni dell'applicazione, anziché in un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d1bae-110">For deploying to Azure, we'll follow the App Service best practice of putting the VaultName configuration in an application setting instead of a configuration file.</span></span> <span data-ttu-id="d1bae-111">Eseguire questo comando per creare l'impostazione dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="d1bae-111">Run this command to create the application setting:</span></span>

```azurecli
az webapp config appsettings set \
    --name <your-unique-app-name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --settings VaultName=<your-unique-vault-name>
```

## <a name="enable-managed-identity"></a><span data-ttu-id="d1bae-112">Abilitare l'identità gestita</span><span class="sxs-lookup"><span data-stu-id="d1bae-112">Enable managed identity</span></span>

<span data-ttu-id="d1bae-113">L'abilitazione dell'identità gestita in un'app è una singola riga di codice &mdash;. Eseguire questa riga per abilitarla nell'app:</span><span class="sxs-lookup"><span data-stu-id="d1bae-113">Enabling managed identity on an app is a one-liner &mdash; run this to enable it on your app:</span></span>

```azurecli
az webapp identity assign \
    --name <your-unique-app-name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="d1bae-114">Nell'output JSON risultante copiare il valore di **principalId**.</span><span class="sxs-lookup"><span data-stu-id="d1bae-114">From the JSON output that results, copy the **principalId** value.</span></span> <span data-ttu-id="d1bae-115">Proprietà PrincipalId è l'ID univoco della nuova identità dell'app in Azure Active Directory e dobbiamo usarlo nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="d1bae-115">PrincipalId is the unique ID of the app's new identity in Azure Active Directory, and we're going to use it in the next step.</span></span>

## <a name="grant-access-to-the-vault"></a><span data-ttu-id="d1bae-116">Concedere l'accesso all'insieme di credenziali</span><span class="sxs-lookup"><span data-stu-id="d1bae-116">Grant access to the vault</span></span>

<span data-ttu-id="d1bae-117">L'ultimo passaggio prima della distribuzione consiste nell'assegnare le autorizzazioni di Key Vault all'identità gestita dell'app.</span><span class="sxs-lookup"><span data-stu-id="d1bae-117">The last step before deploying is to assign Key Vault permissions to your app's managed identity.</span></span> <span data-ttu-id="d1bae-118">Usare il valore di **principalId** copiato dal passaggio precedente come valore per **object-id** nel comando seguente.</span><span class="sxs-lookup"><span data-stu-id="d1bae-118">Use the **principalId** value you copied from the previous step as the value for **object-id** in the command below.</span></span> <span data-ttu-id="d1bae-119">L'esecuzione di questo comando concederà l'accesso **Get** e **List**:</span><span class="sxs-lookup"><span data-stu-id="d1bae-119">Running this command will grant **Get** and **List** access:</span></span>

```azurecli
az keyvault set-policy \
    --name <your-unique-vault-name> \
    --object-id <your-managed-identity-principleid> \
    --secret-permissions get list
```

## <a name="deploy-the-app-and-try-it-out"></a><span data-ttu-id="d1bae-120">Distribuire l'app e provarla</span><span class="sxs-lookup"><span data-stu-id="d1bae-120">Deploy the app and try it out</span></span>

<span data-ttu-id="d1bae-121">Tutte le configurazioni sono impostate e i può avviare la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="d1bae-121">All your configuration is set and you're ready to deploy!</span></span> <span data-ttu-id="d1bae-122">I comandi seguenti pubblicheranno il sito nella cartella `pub`, la comprimeranno in `site.zip` e quindi distribuiranno il file zip nel Servizio app.</span><span class="sxs-lookup"><span data-stu-id="d1bae-122">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="d1bae-123">Sarà necessario `cd` indietro alla directory di KeyVaultDemoApp, se non è già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="d1bae-123">You'll need to `cd` back to the KeyVaultDemoApp directory if you haven't already.</span></span>

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*

az webapp deployment source config-zip \
    --src site.zip \
    --name <your-unique-app-name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="d1bae-124">Dopo aver ottenuto un risultato che indica che il sito è stato distribuito, aprire `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in un browser.</span><span class="sxs-lookup"><span data-stu-id="d1bae-124">Once you get a result that indicates the site has deployed, open `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in a browser.</span></span> <span data-ttu-id="d1bae-125">Il valore del segreto, **reindeer_flotilla** dovrebbe essere visualizzato.</span><span class="sxs-lookup"><span data-stu-id="d1bae-125">You should see the secret value, **reindeer_flotilla**.</span></span>

<span data-ttu-id="d1bae-126">L'app è stata completata e distribuita.</span><span class="sxs-lookup"><span data-stu-id="d1bae-126">Your app is finished and deployed!</span></span>