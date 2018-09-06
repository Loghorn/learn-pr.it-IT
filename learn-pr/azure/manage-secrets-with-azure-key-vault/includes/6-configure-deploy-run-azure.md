<span data-ttu-id="cc777-101">A questo punto è possibile eseguire l'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="cc777-101">Now it's time to run our app in Azure.</span></span> <span data-ttu-id="cc777-102">È necessario creare un'app di Servizio app di Azure, configurarla con identità del servizio gestita e la configurazione d'insieme e quindi distribuire il codice.</span><span class="sxs-lookup"><span data-stu-id="cc777-102">We need to create an Azure App Service app, set it up with MSI and our vault configuration, and deploy our code.</span></span>

## <a name="exercise"></a><span data-ttu-id="cc777-103">Esercizio</span><span class="sxs-lookup"><span data-stu-id="cc777-103">Exercise</span></span>

### <a name="create-the-app-service-plan-and-app"></a><span data-ttu-id="cc777-104">Creare il piano e l'app di Servizio app</span><span class="sxs-lookup"><span data-stu-id="cc777-104">Create the App Service plan and app</span></span>

<span data-ttu-id="cc777-105">La creazione di un'app di Servizio app è un processo che si svolge in due passaggi: per prima cosa, creare il *piano* e poi l'*app*.</span><span class="sxs-lookup"><span data-stu-id="cc777-105">Creating an App Service app is a two-step process: First create the *plan*, then the *app*.</span></span>

<span data-ttu-id="cc777-106">Il nome del *piano* deve solamente essere univoco all'interno della sottoscrizione, quindi è possibile usare lo stesso nome usato da noi: **insieme di credenziali delle chiavi-esercizio-piano**.</span><span class="sxs-lookup"><span data-stu-id="cc777-106">The *plan* name only needs to be unique within your subscription, so you can use the same name we've used: **keyvault-exercise-plan**.</span></span> <span data-ttu-id="cc777-107">Il nome dell'app deve essere globalmente univoco, ma è necessario sceglierne uno proprio.</span><span class="sxs-lookup"><span data-stu-id="cc777-107">The app name needs to be globally unique, though, so you'll need to pick your own.</span></span>

<span data-ttu-id="cc777-108">In Azure Cloud Shell, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="cc777-108">In Azure Cloud Shell, run the following:</span></span>

```azurecli
az appservice plan create --name keyvault-exercise-plan --resource-group keyvault-exercise-group
az webapp create --name <your-unique-app-name> --plan keyvault-exercise-plan --resource-group keyvault-exercise-group
```

### <a name="add-configuration-to-the-app"></a><span data-ttu-id="cc777-109">Aggiungere una configurazione per l'app</span><span class="sxs-lookup"><span data-stu-id="cc777-109">Add configuration to the app</span></span>

<span data-ttu-id="cc777-110">Per la distribuzione in Azure, si seguirà la procedura consigliata del servizio app per inserire la configurazione VaultName nelle impostazioni dell'applicazione, anziché in un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="cc777-110">For deploying to Azure, we'll follow the App Service best practice of putting the VaultName configuration in an application setting instead of a configuration file.</span></span>

```azurecli
az webapp config appsettings set --name <your-unique-app-name> --resource-group keyvault-exercise-group --settings VaultName=<your-unique-vault-name>
```

### <a name="enable-msi"></a><span data-ttu-id="cc777-111">Abilitare l'identità del servizio gestita</span><span class="sxs-lookup"><span data-stu-id="cc777-111">Enable MSI</span></span>

<span data-ttu-id="cc777-112">Abilitazione dell'identità del servizio gestita in un'app è una singola riga di codice:</span><span class="sxs-lookup"><span data-stu-id="cc777-112">Enabling MSI on an app is a one-liner:</span></span>

```azurecli
az webapp identity assign --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="cc777-113">Nell'output JSON risultante, copiare il valore **principalId**.</span><span class="sxs-lookup"><span data-stu-id="cc777-113">From the JSON output that results, copy the **principalId** value.</span></span> <span data-ttu-id="cc777-114">Proprietà PrincipalId è l'ID univoco della nuova identità dell'app in Azure Active Directory e dobbiamo usarlo nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="cc777-114">PrincipalId is the unique ID of the app's new identity in Azure Active Directory, and we're going to use it in the next step.</span></span>

### <a name="grant-access-to-the-vault"></a><span data-ttu-id="cc777-115">Concedere l'accesso all'insieme di credenziali</span><span class="sxs-lookup"><span data-stu-id="cc777-115">Grant access to the vault</span></span>

<span data-ttu-id="cc777-116">A questo punto è necessario concedere all'app le autorizzazioni di identità per ottenere ed elencare i segreti dall'insieme di credenziali di ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="cc777-116">Now you need to give your app identity permissions to get and list secrets from your production-environment vault.</span></span> <span data-ttu-id="cc777-117">Usare la **principalId** copiata nel passaggio precedente come valore per **id dell'oggetto** nel comando seguente.</span><span class="sxs-lookup"><span data-stu-id="cc777-117">Use the **principalId** value you copied from the previous step as the value for **object-id** in the command below.</span></span>

```azurecli
az keyvault set-policy --name <your-unique-vault-name> --object-id <your-msi-principleid> --secret-permissions get list
```

### <a name="deploy-the-app-and-try-it-out"></a><span data-ttu-id="cc777-118">Distribuire l'app e provarla</span><span class="sxs-lookup"><span data-stu-id="cc777-118">Deploy the app and try it out</span></span>

<span data-ttu-id="cc777-119">Tutte le configurazioni sono impostate e i può avviare la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="cc777-119">All your configuration is set and you're ready to deploy!</span></span> <span data-ttu-id="cc777-120">I comandi seguenti pubblicheranno il sito nella cartella `pub`, la comprimeranno in `site.zip` e quindi distribuiranno il file zip nel Servizio app.</span><span class="sxs-lookup"><span data-stu-id="cc777-120">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="cc777-121">Sarà necessario `cd` indietro alla directory di KeyVaultDemoApp, se non è già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="cc777-121">You'll need to `cd` back to the KeyVaultDemoApp directory if you haven't already.</span></span>

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*
az webapp deployment source config-zip --src site.zip --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="cc777-122">Dopo aver ottenuto un risultato che indica che il sito è stato distribuito, aprire `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in un browser.</span><span class="sxs-lookup"><span data-stu-id="cc777-122">Once you get a result that indicates the site has deployed, open `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in a browser.</span></span> <span data-ttu-id="cc777-123">Il valore del segreto, **reindeer_flotilla** dovrebbe essere visualizzato.</span><span class="sxs-lookup"><span data-stu-id="cc777-123">You should see the secret value, **reindeer_flotilla**.</span></span>

<span data-ttu-id="cc777-124">L'app è stata completata e distribuita.</span><span class="sxs-lookup"><span data-stu-id="cc777-124">Your app is finished and deployed!</span></span>