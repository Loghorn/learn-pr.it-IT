<span data-ttu-id="d1d97-101">Ora si userà l'interfaccia della riga di comando di Azure per creare un gruppo di risorse e quindi distribuire un'app Web in questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="d1d97-101">Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="using-a-resource-group"></a><span data-ttu-id="d1d97-102">Uso di un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="d1d97-102">Using a resource group</span></span>

<span data-ttu-id="d1d97-103">Quando si usano il proprio computer e la propria sottoscrizione di Azure, è necessario accedere prima ad Azure tramite il comando `az login`.</span><span class="sxs-lookup"><span data-stu-id="d1d97-103">When you are working with your own machine and Azure subscription you will need to first login to Azure using the `az login` command.</span></span> <span data-ttu-id="d1d97-104">Questa operazione non è necessaria se si usa l'ambiente Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="d1d97-104">This is unnecessary with the Cloud Shell environment.</span></span>

<span data-ttu-id="d1d97-105">In seguito, si crea in genere un gruppo di risorse per tutte le risorse di Azure correlate con un comando `az group create`, ma per questi esercizi il gruppo di risorse è già stato creato.</span><span class="sxs-lookup"><span data-stu-id="d1d97-105">Next, you would normally create a resource group for all your related Azure resources with an `az group create` command, but for these exercises one has been created for you.</span></span> <span data-ttu-id="d1d97-106">Usare **<rgn>[Nome gruppo di risorse sandbox]</rgn>** per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="d1d97-106">Use **<rgn>[sandbox resource group name]</rgn>** for your resource group.</span></span>

1. <span data-ttu-id="d1d97-107">È possibile elencare tutti i gruppi di risorse presenti in una tabella usando l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="d1d97-107">You can ask the Azure CLI to list all your resource groups in a table.</span></span> <span data-ttu-id="d1d97-108">Poiché si usa un ambiente sandbox di Azure gratuito, deve essere presente un solo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="d1d97-108">There should just be one while you are in the free Azure sandbox.</span></span>

    ```azurecli
    az group list --output table
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. <span data-ttu-id="d1d97-109">Con lo sviluppo di altri elementi di Azure è possibile che vengano aggiunti vari gruppi di risorse.</span><span class="sxs-lookup"><span data-stu-id="d1d97-109">As you do more Azure development, you can end up with several resource groups.</span></span> <span data-ttu-id="d1d97-110">Se nell'elenco di gruppi sono presenti vari elementi, è possibile filtrare i valori restituiti aggiungendo un'opzione `--query`.</span><span class="sxs-lookup"><span data-stu-id="d1d97-110">If you have several items in the group list, you can filter the return values by adding a `--query` option.</span></span> <span data-ttu-id="d1d97-111">Provare questo comando:</span><span class="sxs-lookup"><span data-stu-id="d1d97-111">Try this command:</span></span>

    ```azurecli
    az group list --query "[?name == '<rgn>[sandbox resource group name]</rgn>']"
    ```

    <span data-ttu-id="d1d97-112">La query è formattata usando **JMESPath**, che è un linguaggio di query standard per le richieste JSON.</span><span class="sxs-lookup"><span data-stu-id="d1d97-112">The query is formated using **JMESPath** which is a standard query language for JSON requests.</span></span> <span data-ttu-id="d1d97-113">Altre informazioni su questo linguaggio di filtro avanzato si possono trovare all'indirizzo <http://jmespath.org/>.</span><span class="sxs-lookup"><span data-stu-id="d1d97-113">You can learn more about this powerful filter language at <http://jmespath.org/>.</span></span> <span data-ttu-id="d1d97-114">Nel modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure** vengono analizzate le query in modo più approfondito.</span><span class="sxs-lookup"><span data-stu-id="d1d97-114">We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.</span></span>

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="d1d97-115">Procedura per creare un piano di servizio</span><span class="sxs-lookup"><span data-stu-id="d1d97-115">Steps to create a service plan</span></span>

<span data-ttu-id="d1d97-116">Quando si eseguono app Web, usando il servizio app di Azure, si paga per le risorse di calcolo di Azure usate dall'app e ciò dipende dal piano di servizio app associato alle app Web.</span><span class="sxs-lookup"><span data-stu-id="d1d97-116">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="d1d97-117">I piani di servizio determinano l'area usata per il data center dell'app, il numero di macchine virtuali usate e il piano tariffario.</span><span class="sxs-lookup"><span data-stu-id="d1d97-117">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

1. <span data-ttu-id="d1d97-118">Creare un piano di servizio app per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d1d97-118">Create an App Service plan to run your app.</span></span> <span data-ttu-id="d1d97-119">Il comando seguente non specifica un piano tariffario o dettagli sull'istanza della macchina virtuale, quindi per impostazione predefinita si otterrà un piano **Basic** con una istanza di macchina virtuale **Piccola**.</span><span class="sxs-lookup"><span data-stu-id="d1d97-119">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    > [!WARNING]
    > <span data-ttu-id="d1d97-120">I nomi dell'app e del piano devono essere _univoci_. Aggiungere pertanto un suffisso al nome e sostituire il testo `<unique>` nel comando seguente con un set di numeri, con le proprie iniziali o con qualsiasi altro testo per verificare che sia univoco in Azure.</span><span class="sxs-lookup"><span data-stu-id="d1d97-120">The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure.</span></span>

    <span data-ttu-id="d1d97-121">Per il parametro `--location`, usare uno dei valori di località seguenti.</span><span class="sxs-lookup"><span data-stu-id="d1d97-121">For the `--location` parameter, use one of the below location values.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --location <location>
    ```

    <span data-ttu-id="d1d97-122">Il completamento di questo comando può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="d1d97-122">This command can take several minutes to complete.</span></span>

1. <span data-ttu-id="d1d97-123">Verificare che il piano di servizio sia stato creato correttamente elencando tutti i piani in una tabella.</span><span class="sxs-lookup"><span data-stu-id="d1d97-123">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="d1d97-124">Procedura per creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="d1d97-124">Steps to create a web app</span></span>

<span data-ttu-id="d1d97-125">A questo punto verrà creata l'app Web nel piano di servizio.</span><span class="sxs-lookup"><span data-stu-id="d1d97-125">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="d1d97-126">È possibile distribuire il codice nello stesso momento, ma per questo esempio l'operazione verrà eseguita in passaggi separati.</span><span class="sxs-lookup"><span data-stu-id="d1d97-126">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="d1d97-127">Creare l'app Web e specificare il nome del piano creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="d1d97-127">Create the web app, supply the name of the plan you created above.</span></span> <span data-ttu-id="d1d97-128">**Analogamente al nome del piano, anche il nome dell'app deve essere univoco**. Sostituire l'indicatore `<unique>` con un testo per rendere il nome univoco a livello globale.</span><span class="sxs-lookup"><span data-stu-id="d1d97-128">**Just like the plan, the app name must be unique**, replace the `<unique>` marker with some text to make the name globally unique.</span></span>

    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --plan popupappplan-<unique>
    ```

1. <span data-ttu-id="d1d97-129">Verificare che l'app sia stata creata correttamente elencando tutte le app in una tabella.</span><span class="sxs-lookup"><span data-stu-id="d1d97-129">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```azurecli
    az webapp list --output table
    ```

1. <span data-ttu-id="d1d97-130">Prendere nota dell'elemento **DefaultHostName** elencato nella tabella perché si tratta dell'indirizzo Web raggiungibile per il nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="d1d97-130">Make a note of the **DefaultHostName** listed in the table; this is the reachable web address for the new website.</span></span> <span data-ttu-id="d1d97-131">Azure renderà disponibile il sito Web tramite il nome app univoco nel dominio `azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d1d97-131">Azure will make your website available through the unique app name in the `azurewebsites.net` domain.</span></span> <span data-ttu-id="d1d97-132">Se ad esempio il nome del sito era "popupwebapp-mslearn123", l'URL del sito Web è: `http://popupwebapp-mslearn123.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d1d97-132">For example, if my app name was "popupwebapp-mslearn123", then my website URL would be: `http://popupwebapp-mslearn123.azurewebsites.net`.</span></span>

1. <span data-ttu-id="d1d97-133">Nel sito è presente una pagina di avvio rapido creata da Azure che è possibile visualizzare in un browser o tramite CURL, usando solo l'elemento **DefaultHostName**:</span><span class="sxs-lookup"><span data-stu-id="d1d97-133">Your site has a "QuickStart" page created by Azure that you can see either in a browser, or with CURL, just use the **DefaultHostName**:</span></span>

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="d1d97-134">Procedura per distribuire il codice da GitHub</span><span class="sxs-lookup"><span data-stu-id="d1d97-134">Steps to deploy code from GitHub</span></span>

1. <span data-ttu-id="d1d97-135">Il passaggio finale consiste nel distribuire il codice da un repository di GitHub all'app Web.</span><span class="sxs-lookup"><span data-stu-id="d1d97-135">The final step is to deploy code from a GitHub repository to the web app.</span></span> <span data-ttu-id="d1d97-136">Verrà usata una semplice pagina PHP disponibile nel repository GitHub di esempi di Azure che visualizza "HelloWorld!"</span><span class="sxs-lookup"><span data-stu-id="d1d97-136">Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!"</span></span> <span data-ttu-id="d1d97-137">quando viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="d1d97-137">when it executes.</span></span> <span data-ttu-id="d1d97-138">Assicurarsi di usare il nome dell'app Web creata.</span><span class="sxs-lookup"><span data-stu-id="d1d97-138">Make sure to use the web app name you created.</span></span>

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group <rgn>[sandbox resource group name]</rgn> --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="d1d97-139">Dopo la distribuzione, accedere nuovamente al sito con un browser o tramite CURL.</span><span class="sxs-lookup"><span data-stu-id="d1d97-139">Once it's deployed, hit your site again with a browser or CURL.</span></span>

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
    <span data-ttu-id="d1d97-140">Nella pagina viene visualizzato "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d1d97-140">The page displays "Hello World!"</span></span>

    ```output
    Hello World!
    ```

<span data-ttu-id="d1d97-141">In questo esercizio è stato dimostrato un criterio tipico per una sessione interattiva dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="d1d97-141">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="d1d97-142">È stato prima di tutto usato un comando standard per creare un nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="d1d97-142">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="d1d97-143">È stato quindi usato un set di comandi per distribuire una risorsa (in questo esempio, un'app Web) in questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="d1d97-143">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="d1d97-144">Questo set di comandi può essere facilmente combinato in uno script della shell da eseguire ogni volta che è necessario creare la stessa risorsa.</span><span class="sxs-lookup"><span data-stu-id="d1d97-144">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>