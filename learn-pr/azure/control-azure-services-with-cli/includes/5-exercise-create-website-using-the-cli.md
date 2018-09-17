<span data-ttu-id="536ed-101">Ora si userà l'interfaccia della riga di comando di Azure per creare un gruppo di risorse e quindi distribuire un'app Web in questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="536ed-101">Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.</span></span>

<span data-ttu-id="536ed-102">È possibile usare l'interfaccia della riga di comando di Azure installata con la sottoscrizione di Azure, ma per questo esercizio è disponibile un ambiente sandbox gratuito con Azure Cloud Shell in cui l'interfaccia della riga di comando di Azure è già installata.</span><span class="sxs-lookup"><span data-stu-id="536ed-102">You can use the Azure CLI you installed with your own Azure subscription, but there is a free sandbox environment available for this exercise with Azure Cloud Shell where the Azure CLI is already installed.</span></span> <span data-ttu-id="536ed-103">Le istruzioni seguenti presuppongono che venga usato l'ambiente sandbox gratuito.</span><span class="sxs-lookup"><span data-stu-id="536ed-103">The following instructions assume you will be using the free sandbox.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="536ed-104">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="536ed-104">Create a resource group</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

> [!NOTE]
> <span data-ttu-id="536ed-105">In genere si crea un gruppo di risorse per tutte le risorse di Azure correlate con un comando `az group create ...`, ma per questi esercizi è già stato creato il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="536ed-105">Normally, you would create a resource group for all your related Azure resources with an `az group create ...` command, but for these exercises one has been created for you.</span></span> <span data-ttu-id="536ed-106">Il nome di questo gruppo di risorse verrà inserito nei comandi successivi in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="536ed-106">This resource group name will be injected into the later commands in this exercise.</span></span>

<!-- TODO: This is original text prior to updates to use the sandbox. These can be worked back in as instructions for people using their own subscriptions. There is one more block like this below. Note that the assignment of RESOURCE_GROUP below would need to be different as well. -->

<!-- 1. Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows. -->

<!-- 1. Start the Azure CLI and run the login command.

    ```azurecli
    az login
    ```
    If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin). -->

<!-- 1. Create a resource group.

    ```azurecli
    az group create --location westeurope --name popupResGroup
    ``` -->

1. <span data-ttu-id="536ed-107">Verificare che il gruppo di risorse sia stato creato correttamente elencando tutti i gruppi di risorse in una tabella.</span><span class="sxs-lookup"><span data-stu-id="536ed-107">Verify that the resource group was created successfully by listing all your resource groups in a table.</span></span>

    ```azurecli
    az group list --output table
    ```

    <span data-ttu-id="536ed-108">Viene visualizzato solo il gruppo di risorse `<rgn>[Sandbox resource group name]</rgn>` che è stato generato per l'uso del sandbox gratuito.</span><span class="sxs-lookup"><span data-stu-id="536ed-108">You may only see the `<rgn>[Sandbox resource group name]</rgn>` resource group that was generated for your free sandbox usage.</span></span>

<!-- > [!TIP]
> You can also confirm the resource was created in the Azure portal. Open a web browser, sign in to the portal and navigate to the **Resource Groups** section. The new resource group should be displayed in the list. -->

1. <span data-ttu-id="536ed-109">Man mano che si sviluppano altri elementi di Azure è possibile che vengano aggiunti vari gruppi di risorse.</span><span class="sxs-lookup"><span data-stu-id="536ed-109">As you do more Azure development, you can end up with several resource groups.</span></span> <span data-ttu-id="536ed-110">Se nell'elenco di gruppi sono presenti vari elementi, è possibile filtrare i valori restituiti aggiungendo un'opzione `--query`.</span><span class="sxs-lookup"><span data-stu-id="536ed-110">If you have several items in the group list, you can filter the return values by adding a `--query` option.</span></span> <span data-ttu-id="536ed-111">Provare questo comando:</span><span class="sxs-lookup"><span data-stu-id="536ed-111">Try this command:</span></span>

    ```azurecli
    az group list --query "[?name == '<rgn>[Sandbox resource group name]</rgn>']"
    ```

    <span data-ttu-id="536ed-112">La query è formattata usando **JMESPath**, che è un linguaggio di query standard per le richieste JSON.</span><span class="sxs-lookup"><span data-stu-id="536ed-112">The query is formated using **JMESPath** which is a standard query language for JSON requests.</span></span> <span data-ttu-id="536ed-113">Altre informazioni su questo linguaggio di filtro avanzato si possono trovare all'indirizzo <http://jmespath.org/>.</span><span class="sxs-lookup"><span data-stu-id="536ed-113">You can learn more about this powerful filter language at <http://jmespath.org/>.</span></span> <span data-ttu-id="536ed-114">Nel modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure** vengono analizzate le query in modo più approfondito.</span><span class="sxs-lookup"><span data-stu-id="536ed-114">We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.</span></span>

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="536ed-115">Procedura per creare un piano di servizio</span><span class="sxs-lookup"><span data-stu-id="536ed-115">Steps to create a service plan</span></span>

<span data-ttu-id="536ed-116">Quando si eseguono app Web, usando il servizio app di Azure, si paga per le risorse di calcolo di Azure usate dall'app e ciò dipende dal piano di servizio app associato alle app Web.</span><span class="sxs-lookup"><span data-stu-id="536ed-116">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="536ed-117">I piani di servizio determinano l'area usata per il data center dell'app, il numero di macchine virtuali usate e il piano tariffario.</span><span class="sxs-lookup"><span data-stu-id="536ed-117">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="536ed-118">Creare un piano di servizio app per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="536ed-118">Create an App Service plan to run your app.</span></span> <span data-ttu-id="536ed-119">Il comando seguente non specifica un piano tariffario o dettagli sull'istanza della macchina virtuale, quindi per impostazione predefinita si otterrà un piano **Basic** con una istanza di macchina virtuale **Piccola**.</span><span class="sxs-lookup"><span data-stu-id="536ed-119">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    > [!WARNING]
    > <span data-ttu-id="536ed-120">I nomi dell'app e del piano devono essere _univoci_. Aggiungere pertanto un suffisso al nome e sostituire il testo `<unique>` nel comando seguente con un set di numeri, con le proprie iniziali o con qualsiasi altro testo per assicurarsi che sia univoco in Azure.</span><span class="sxs-lookup"><span data-stu-id="536ed-120">The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure.</span></span>

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --location eastus
    ```

    <span data-ttu-id="536ed-121">Il completamento di questo comando può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="536ed-121">This command can take several minutes to complete.</span></span>

1. <span data-ttu-id="536ed-122">Verificare che il piano di servizio sia stato creato correttamente elencando tutti i piani in una tabella.</span><span class="sxs-lookup"><span data-stu-id="536ed-122">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="536ed-123">Procedura per creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="536ed-123">Steps to create a web app</span></span>

<span data-ttu-id="536ed-124">A questo punto verrà creata l'app Web nel piano di servizio.</span><span class="sxs-lookup"><span data-stu-id="536ed-124">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="536ed-125">È possibile distribuire il codice nello stesso momento, ma per questo esempio l'operazione verrà eseguita in passaggi separati.</span><span class="sxs-lookup"><span data-stu-id="536ed-125">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="536ed-126">Creare l'app Web e specificare il nome del piano creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="536ed-126">Create the web app, supply the name of the plan you created above.</span></span> <span data-ttu-id="536ed-127">**Analogamente al nome del piano, anche il nome dell'app deve essere univoco. Sostituire l'indicatore `<unique>` con un testo per rendere il nome univoco a livello globale.**</span><span class="sxs-lookup"><span data-stu-id="536ed-127">**Just like the plan, the app name must be unique, replace the `<unique>` marker with some text to make the name globally unique.**</span></span>
    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group popupResGroup --plan popupappplan-<unique>
    ```

1. <span data-ttu-id="536ed-128">Verificare che l'app sia stata creata correttamente elencando tutte le app in una tabella.</span><span class="sxs-lookup"><span data-stu-id="536ed-128">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```azurecli
    az webapp list --output table
    ```

1. <span data-ttu-id="536ed-129">Prendere nota di **DefaultHostName** perché sarà necessario in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="536ed-129">Make a note of the **DefaultHostName**; you will need this later.</span></span>

### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="536ed-130">Procedura per distribuire il codice da GitHub</span><span class="sxs-lookup"><span data-stu-id="536ed-130">Steps to deploy code from GitHub</span></span>

1. <span data-ttu-id="536ed-131">Il passaggio finale consiste nel distribuire il codice da un repository di GitHub all'app Web.</span><span class="sxs-lookup"><span data-stu-id="536ed-131">The final step is to deploy code from a GitHub repository to the web app.</span></span> <span data-ttu-id="536ed-132">Verrà usata una semplice pagina PHP disponibile nel repository GitHub di esempi di Azure che visualizza "HelloWorld!"</span><span class="sxs-lookup"><span data-stu-id="536ed-132">Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!"</span></span> <span data-ttu-id="536ed-133">quando viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="536ed-133">when it executes.</span></span> <span data-ttu-id="536ed-134">Assicurarsi di usare il nome dell'app Web creato.</span><span class="sxs-lookup"><span data-stu-id="536ed-134">Make sure to use the web app name you created.</span></span>

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="536ed-135">Al termine della distribuzione, Azure renderà disponibile il sito Web tramite il nome app univoco nel dominio `azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="536ed-135">Once it's deployed, Azure will make your website available through the unique app name in the `azurewebsites.net` domain.</span></span> <span data-ttu-id="536ed-136">Se, ad esempio, il nome dell'app fosse "popupapp-mslearn123", l'indirizzo del sito Web sarebbe: <http://popupapp-mslearn123.azurewebsites.net>.</span><span class="sxs-lookup"><span data-stu-id="536ed-136">For example, if my app name was "popupapp-mslearn123", then my website address would be: <http://popupapp-mslearn123.azurewebsites.net>.</span></span> <span data-ttu-id="536ed-137">Sarà necessario digitare l'URL corretto per accedere all'istanza specifica.</span><span class="sxs-lookup"><span data-stu-id="536ed-137">You will need to type in the correct URL to hit your specific instance.</span></span>

1. <span data-ttu-id="536ed-138">Nella pagina viene visualizzato "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="536ed-138">The page displays "Hello World!"</span></span>

1. <span data-ttu-id="536ed-139">Chiudere la finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="536ed-139">Close the browser window.</span></span>

<span data-ttu-id="536ed-140">In questo esercizio è stato dimostrato un criterio tipico per una sessione interattiva dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="536ed-140">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="536ed-141">È stato prima di tutto usato un comando standard per creare un nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="536ed-141">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="536ed-142">È stato quindi usato un set di comandi per distribuire una risorsa (in questo esempio, un'app Web) in questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="536ed-142">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="536ed-143">Questo set di comandi può essere facilmente combinato in uno script della shell da eseguire ogni volta che è necessario creare la stessa risorsa.</span><span class="sxs-lookup"><span data-stu-id="536ed-143">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>
