<span data-ttu-id="33979-101">A questo punto è possibile eseguire l'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="33979-101">Now it's time to run our app in Azure.</span></span> <span data-ttu-id="33979-102">È necessario creare un'app del Servizio app di Azure, configurarla con l'identità del servizio gestita e la configurazione dell'insieme di credenziali e quindi distribuire il codice.</span><span class="sxs-lookup"><span data-stu-id="33979-102">We need to create an Azure App Service app, set it up with MSI and our vault configuration, and deploy our code.</span></span>

## <a name="create-the-app-service-plan-and-app"></a><span data-ttu-id="33979-103">Creare l'app e il piano di servizio app</span><span class="sxs-lookup"><span data-stu-id="33979-103">Create the App Service plan and app</span></span>

<span data-ttu-id="33979-104">La creazione di un'app di Servizio app è un processo che si svolge in due passaggi: per prima cosa, creare il *piano* e poi l'*app*.</span><span class="sxs-lookup"><span data-stu-id="33979-104">Creating an App Service app is a two-step process: First create the *plan*, then the *app*.</span></span>

<span data-ttu-id="33979-105">Il nome del *piano* deve solamente essere univoco all'interno della sottoscrizione, quindi è possibile usare lo stesso nome usato da noi: **insieme di credenziali delle chiavi-esercizio-piano**.</span><span class="sxs-lookup"><span data-stu-id="33979-105">The *plan* name only needs to be unique within your subscription, so you can use the same name we've used: **keyvault-exercise-plan**.</span></span> <span data-ttu-id="33979-106">Il nome dell'app deve essere globalmente univoco, ma è necessario sceglierne uno proprio.</span><span class="sxs-lookup"><span data-stu-id="33979-106">The app name needs to be globally unique, though, so you'll need to pick your own.</span></span>

<span data-ttu-id="33979-107">In Azure Cloud Shell, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="33979-107">In Azure Cloud Shell, run the following:</span></span>

```azurecli
az appservice plan create --name keyvault-exercise-plan --resource-group keyvault-exercise-group
az webapp create --name <your-unique-app-name> --plan keyvault-exercise-plan --resource-group keyvault-exercise-group
```

## <a name="add-configuration-to-the-app"></a><span data-ttu-id="33979-108">Aggiungere una configurazione per l'app</span><span class="sxs-lookup"><span data-stu-id="33979-108">Add configuration to the app</span></span>

<span data-ttu-id="33979-109">Per la distribuzione in Azure, si seguirà la procedura consigliata del servizio app per inserire la configurazione VaultName nelle impostazioni dell'applicazione, anziché in un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="33979-109">For deploying to Azure, we'll follow the App Service best practice of putting the VaultName configuration in an application setting instead of a configuration file.</span></span>

```azurecli
az webapp config appsettings set --name <your-unique-app-name> --resource-group keyvault-exercise-group --settings VaultName=<your-unique-vault-name>
```

## <a name="enable-msi"></a><span data-ttu-id="33979-110">Abilitare l'identità del servizio gestita</span><span class="sxs-lookup"><span data-stu-id="33979-110">Enable MSI</span></span>

<span data-ttu-id="33979-111">Abilitazione dell'identità del servizio gestita in un'app è una singola riga di codice:</span><span class="sxs-lookup"><span data-stu-id="33979-111">Enabling MSI on an app is a one-liner:</span></span>

```azurecli
az webapp identity assign --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="33979-112">Nell'output JSON risultante, copiare il valore **principalId**.</span><span class="sxs-lookup"><span data-stu-id="33979-112">From the JSON output that results, copy the **principalId** value.</span></span> <span data-ttu-id="33979-113">Proprietà PrincipalId è l'ID univoco della nuova identità dell'app in Azure Active Directory e dobbiamo usarlo nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="33979-113">PrincipalId is the unique ID of the app's new identity in Azure Active Directory, and we're going to use it in the next step.</span></span>

## <a name="grant-access-to-the-vault"></a><span data-ttu-id="33979-114">Concedere l'accesso all'insieme di credenziali</span><span class="sxs-lookup"><span data-stu-id="33979-114">Grant access to the vault</span></span>

<span data-ttu-id="33979-115">A questo punto è necessario concedere all'app le autorizzazioni di identità per ottenere ed elencare i segreti dall'insieme di credenziali di ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="33979-115">Now you need to give your app identity permissions to get and list secrets from your production-environment vault.</span></span> <span data-ttu-id="33979-116">Usare la **principalId** copiata nel passaggio precedente come valore per **id dell'oggetto** nel comando seguente.</span><span class="sxs-lookup"><span data-stu-id="33979-116">Use the **principalId** value you copied from the previous step as the value for **object-id** in the command below.</span></span>

```azurecli
az keyvault set-policy --name <your-unique-vault-name> --object-id <your-msi-principleid> --secret-permissions get list
```

## <a name="deploy-the-app-and-try-it-out"></a><span data-ttu-id="33979-117">Distribuire l'app e provarla</span><span class="sxs-lookup"><span data-stu-id="33979-117">Deploy the app and try it out</span></span>

<span data-ttu-id="33979-118">Tutte le configurazioni sono impostate e i può avviare la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="33979-118">All your configuration is set and you're ready to deploy!</span></span> <span data-ttu-id="33979-119">I comandi seguenti pubblicheranno il sito nella cartella `pub`, la comprimeranno in `site.zip` e quindi distribuiranno il file zip nel Servizio app.</span><span class="sxs-lookup"><span data-stu-id="33979-119">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="33979-120">Sarà necessario `cd` indietro alla directory di KeyVaultDemoApp, se non è già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="33979-120">You'll need to `cd` back to the KeyVaultDemoApp directory if you haven't already.</span></span>

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*
az webapp deployment source config-zip --src site.zip --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="33979-121">Dopo aver ottenuto un risultato che indica che il sito è stato distribuito, aprire `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in un browser.</span><span class="sxs-lookup"><span data-stu-id="33979-121">Once you get a result that indicates the site has deployed, open `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in a browser.</span></span> <span data-ttu-id="33979-122">Il valore del segreto, **reindeer_flotilla** dovrebbe essere visualizzato.</span><span class="sxs-lookup"><span data-stu-id="33979-122">You should see the secret value, **reindeer_flotilla**.</span></span>

<span data-ttu-id="33979-123">L'app è stata completata e distribuita.</span><span class="sxs-lookup"><span data-stu-id="33979-123">Your app is finished and deployed!</span></span>