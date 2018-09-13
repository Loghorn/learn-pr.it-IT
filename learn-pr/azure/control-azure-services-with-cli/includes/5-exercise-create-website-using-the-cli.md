<span data-ttu-id="c7eb8-101">Usare quindi l'interfaccia della riga di comando di Azure per creare un gruppo di risorse e quindi per distribuire un'app Web in questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-101">Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.</span></span> 

### <a name="create-a-resource-group"></a><span data-ttu-id="c7eb8-102">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="c7eb8-102">Create a resource group</span></span>

1. <span data-ttu-id="c7eb8-103">Aprire una shell di bash in Linux o macOS oppure aprire la finestra del prompt dei comandi o PowerShell in ambiente Windows.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-103">Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows.</span></span>

1. <span data-ttu-id="c7eb8-104">Avviare l'interfaccia della riga di comando di Azure ed eseguire il comando di accesso.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-104">Start the Azure CLI and run the login command.</span></span>

    ```azurecli
    az login
    ```
    <span data-ttu-id="c7eb8-105">Se non viene visualizzata la pagina di accesso ad Azure nel Web browser, seguire le istruzioni della riga di comando e immettere un codice di autorizzazione all'indirizzo [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span><span class="sxs-lookup"><span data-stu-id="c7eb8-105">If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

1. <span data-ttu-id="c7eb8-106">Creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-106">Create a resource group.</span></span>

    ```azurecli
    az group create --location westeurope --name popupResGroup
    ```

1. <span data-ttu-id="c7eb8-107">Verificare che il gruppo di risorse sia stato creato correttamente elencando tutti i gruppi di risorse in una tabella.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-107">Verify that the resource group was created successfully by listing all your resource groups in a table.</span></span>

    ```azurecli
    az group list --output table
    ```

> [!TIP]
> <span data-ttu-id="c7eb8-108">Verificare anche che la risorsa sia stata creata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-108">You can also confirm the resource was created in the Azure portal.</span></span> <span data-ttu-id="c7eb8-109">Aprire un Web browser, accedere al portale e passare alla sezione **Gruppi di risorse**.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-109">Open a web browser, sign in to the portal and navigate to the **Resource Groups** section.</span></span> <span data-ttu-id="c7eb8-110">Il nuovo gruppo di risorse dovrebbe essere visualizzato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-110">The new resource group should be displayed in the list.</span></span>

1. <span data-ttu-id="c7eb8-111">Se nel gruppo sono presenti molti elementi, è possibile filtrare i valori restituiti aggiungendo un'opzione `--query`. Provare questo comando:</span><span class="sxs-lookup"><span data-stu-id="c7eb8-111">If you have a lot of items in the group, you can filter the return values by adding a `--query` option, try this command:</span></span>

    ```azurecli
    az group list --query '[?name == popupResGroup]'
    ```

    <span data-ttu-id="c7eb8-112">La query è formattata usando **JMESPath**, che è un linguaggio di query standard per le richieste JSON.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-112">The query is formmated using **JMESPath** which is a standard query language for JSON requests.</span></span> <span data-ttu-id="c7eb8-113">Altre informazioni su questo linguaggio di filtro avanzato si possono trovare all'indirizzo <http://jmespath.org/>.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-113">You can learn more about this powerful filter language at <http://jmespath.org/>.</span></span> <span data-ttu-id="c7eb8-114">Nel modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure** vengono analizzate le query in modo più approfondito.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-114">We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.</span></span>

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="c7eb8-115">Procedura per creare un piano di servizio</span><span class="sxs-lookup"><span data-stu-id="c7eb8-115">Steps to create a service plan</span></span>

<span data-ttu-id="c7eb8-116">Quando si eseguono app Web, usando il servizio app di Azure, si paga per le risorse di calcolo di Azure usate dall'app e ciò dipende dal piano di servizio app associato alle app Web.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-116">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="c7eb8-117">I piani di servizio determinano l'area usata per il data center dell'app, il numero di macchine virtuali usate e il piano tariffario.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-117">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

1. <span data-ttu-id="c7eb8-118">Creare un piano di servizio app per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-118">Create an App Service plan to run your app.</span></span> <span data-ttu-id="c7eb8-119">Il comando seguente non specifica un piano tariffario o dettagli sull'istanza della macchina virtuale, quindi per impostazione predefinita si otterrà un piano **Basic** con una istanza di macchina virtuale **Piccola**.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-119">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    > [!WARNING]
    > <span data-ttu-id="c7eb8-120">I nomi dell'app e del piano devono essere _univoci_. Aggiungere quindi un suffisso al nome e sostituire il testo `<unique>` nel comando seguente con un set di numeri, con le proprie iniziali o con qualsiasi altro testo per assicurarsi che sia univoco in Azure.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-120">The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure.</span></span> 

    ```azurecli
    az appservice plan create --name popupapp-<unique> --resource-group popupResGroup --location westeurope
    ```

1. <span data-ttu-id="c7eb8-121">Verificare che il piano di servizio sia stato creato correttamente elencando tutti i piani in una tabella.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-121">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="c7eb8-122">Procedura per creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="c7eb8-122">Steps to create a web app</span></span>

<span data-ttu-id="c7eb8-123">A questo punto verrà creata l'app Web nel piano di servizio.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-123">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="c7eb8-124">È possibile distribuire il codice nello stesso momento, ma per questo esempio l'operazione verrà eseguita in passaggi separati.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-124">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="c7eb8-125">Creare l'app Web e specificare il nome del piano creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-125">Create the web app, supply the name of the plan you created above.</span></span> <span data-ttu-id="c7eb8-126">**Analogamente al nome del piano, anche il nome dell'app deve essere univoco. Sostituire l'indicatore `<unique>` con un testo per rendere il nome univoco a livello globale.**</span><span class="sxs-lookup"><span data-stu-id="c7eb8-126">**Just like the plan, the app name must be unique, replace the `<unique>` marker with some text to make the name globally unique.**</span></span>
    ```azurecli
    az webapp create --name popupapp-<unique> --resource-group popupResGroup --plan popupapp-<unique>
    ```

1. <span data-ttu-id="c7eb8-127">Verificare che l'app sia stata creata correttamente elencando tutte le app in una tabella.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-127">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```azurecli
    az webapp list --output table
    ```

1. <span data-ttu-id="c7eb8-128">Prendere nota di **DefaultHostName** perché sarà necessario in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-128">Make a note of the **DefaultHostName**; you will need this later.</span></span>

### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="c7eb8-129">Procedura per distribuire il codice da GitHub</span><span class="sxs-lookup"><span data-stu-id="c7eb8-129">Steps to deploy code from GitHub</span></span>

1. <span data-ttu-id="c7eb8-130">Il passaggio finale consiste nel distribuire il codice da un repository di GitHub all'app Web.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-130">The final step is to deploy code from a GitHub repository to the web app.</span></span> <span data-ttu-id="c7eb8-131">Verrà usata una semplice pagina PHP disponibile nel repository GitHub di esempi di Azure che visualizza "HelloWorld!"</span><span class="sxs-lookup"><span data-stu-id="c7eb8-131">Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!"</span></span> <span data-ttu-id="c7eb8-132">quando viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-132">when it executes.</span></span> <span data-ttu-id="c7eb8-133">Assicurarsi di usare il nome dell'app Web creato.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-133">Make sure to use the web app name you created.</span></span>

    ```azurecli
    az webapp deployment source config --name popupapp-<unique> --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="c7eb8-134">Al termine della distribuzione, Azure renderà disponibile il sito Web tramite il nome app univoco nel dominio `azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-134">Once it's deployed, Azure will make your website available through the unique app name in the `azurewebsites.net` domain.</span></span> <span data-ttu-id="c7eb8-135">Se, ad esempio, il nome dell'app fosse "popupapp-mslearn123", l'indirizzo del sito Web sarebbe: <http://popupapp-mslearn123.azurewebsites.net>.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-135">For example, if my app name was "popupapp-mslearn123", then my website address would be: <http://popupapp-mslearn123.azurewebsites.net>.</span></span> <span data-ttu-id="c7eb8-136">Sarà necessario digitare l'URL corretto per accedere all'istanza specifica.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-136">You will need to type in the correct URL to hit your specific instance.</span></span>

1. <span data-ttu-id="c7eb8-137">Nella pagina viene visualizzato "HelloWorld!"</span><span class="sxs-lookup"><span data-stu-id="c7eb8-137">The page displays "HelloWorld!"</span></span>

1. <span data-ttu-id="c7eb8-138">Chiudere la finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-138">Close the browser window.</span></span>

## <a name="summary"></a><span data-ttu-id="c7eb8-139">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c7eb8-139">Summary</span></span>

<span data-ttu-id="c7eb8-140">In questo esercizio è stato dimostrato un modello tipico per una sessione interattiva dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-140">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="c7eb8-141">È stato prima di tutto usato un comando standard per creare un nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-141">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="c7eb8-142">È stato quindi usato un set di comandi per distribuire una risorsa (in questo esempio, un'app Web) in questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-142">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="c7eb8-143">Questo set di comandi può essere facilmente combinato in uno script della shell da eseguire ogni volta che è necessario creare la stessa risorsa.</span><span class="sxs-lookup"><span data-stu-id="c7eb8-143">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>
