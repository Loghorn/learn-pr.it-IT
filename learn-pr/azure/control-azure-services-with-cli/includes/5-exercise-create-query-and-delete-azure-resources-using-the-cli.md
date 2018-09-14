
<span data-ttu-id="82df4-101">In questo esercizio si userà l'interfaccia della riga di comando di Azure nel computer locale per creare un gruppo di risorse e quindi per distribuire un'app Web in questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="82df4-101">In this exercise, you will use the Azure CLI on your local machine to create a resource group, and then to deploy a web app into this resource group.</span></span> 

### <a name="steps-to-create-a-resource-group"></a><span data-ttu-id="82df4-102">Procedura per creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="82df4-102">Steps to create a resource group</span></span>
1. <span data-ttu-id="82df4-103">Aprire una shell di bash in Linux o macOS oppure aprire la finestra del prompt dei comandi o PowerShell in ambiente Windows.</span><span class="sxs-lookup"><span data-stu-id="82df4-103">Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows.</span></span>

1. <span data-ttu-id="82df4-104">Avviare l'interfaccia della riga di comando di Azure ed eseguire il comando di accesso.</span><span class="sxs-lookup"><span data-stu-id="82df4-104">Start the Azure CLI and run the login command.</span></span>

    ```bash
    az login
    ```
    <span data-ttu-id="82df4-105">Se non viene visualizzata la pagina di accesso ad Azure nel Web browser, seguire le istruzioni della riga di comando e immettere un codice di autorizzazione all'indirizzo [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span><span class="sxs-lookup"><span data-stu-id="82df4-105">If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

1. <span data-ttu-id="82df4-106">Creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="82df4-106">Create a resource group.</span></span>

    ```bash
    az group create --location westeurope --name popupResGroup
    ```

1. <span data-ttu-id="82df4-107">Verificare che il gruppo di risorse sia stato creato correttamente elencando tutti i gruppi di risorse in una tabella.</span><span class="sxs-lookup"><span data-stu-id="82df4-107">Verify that the resource group was created successfully by listing all your resource groups in a table.</span></span>

    ```bash
    az group list --output table
    ```
1. <span data-ttu-id="82df4-108">Facoltativamente, verificare che la risorsa sia stata creata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="82df4-108">Optionally, confirm the resource was created in the Azure portal.</span></span> <span data-ttu-id="82df4-109">Aprire un Web browser, accedere al portale e passare alla sezione **Gruppi di risorse** (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="82df4-109">Open a web browser, sign in to the portal and navigate to the **Resource Groups** section (see below).</span></span> <span data-ttu-id="82df4-110">Il nuovo gruppo di risorse dovrebbe essere visualizzato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="82df4-110">The new resource group should be displayed in the list.</span></span>

![Uso del portale per elencare i gruppi di risorse](../media-drafts/5-listing-resource-groups.png)

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="82df4-112">Procedura per creare un piano di servizio</span><span class="sxs-lookup"><span data-stu-id="82df4-112">Steps to create a service plan</span></span>
<span data-ttu-id="82df4-113">Quando si eseguono app Web, usando il servizio app di Azure, si paga per le risorse di calcolo di Azure usate dall'app e ciò dipende dal piano di servizio app associato alle app Web.</span><span class="sxs-lookup"><span data-stu-id="82df4-113">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="82df4-114">I piani di servizio determinano l'area usata per il data center dell'app, il numero di macchine virtuali usate e il piano tariffario.</span><span class="sxs-lookup"><span data-stu-id="82df4-114">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

1. <span data-ttu-id="82df4-115">Creare un piano di servizio app per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="82df4-115">Create an App Service plan to run your app.</span></span> <span data-ttu-id="82df4-116">Il nome dell'app e del piano deve essere univoco, quindi modificare la stringa "12345" in un numero casuale.</span><span class="sxs-lookup"><span data-stu-id="82df4-116">The name of the app and plan must be unique, so change the string "12345" to a random number.</span></span> <span data-ttu-id="82df4-117">Il comando seguente non specifica un piano tariffario o dettagli sull'istanza della macchina virtuale, quindi per impostazione predefinita si otterrà un piano **Basic** con una istanza di macchina virtuale **Piccola**.</span><span class="sxs-lookup"><span data-stu-id="82df4-117">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    ```bash
    az appservice plan create --name popupapp12345 --resource-group popupResGroup --location westeurope
    ```

1. <span data-ttu-id="82df4-118">Verificare che il piano di servizio sia stato creato correttamente elencando tutti i piani in una tabella.</span><span class="sxs-lookup"><span data-stu-id="82df4-118">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```bash
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="82df4-119">Procedura per creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="82df4-119">Steps to create a web app</span></span>
<span data-ttu-id="82df4-120">A questo punto verrà creata l'app Web nel piano di servizio.</span><span class="sxs-lookup"><span data-stu-id="82df4-120">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="82df4-121">È possibile distribuire il codice nello stesso momento, ma per questo esempio l'operazione verrà eseguita in passaggi separati.</span><span class="sxs-lookup"><span data-stu-id="82df4-121">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="82df4-122">Creare l'app Web, ricordando di sostituire la stringa "12345" con lo stesso numero casuale usato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="82df4-122">Create the web app, remembering to change the string "12345" to the same random number you used earlier.</span></span>
    ```bash
    az webapp create --name popupapp12345 --resource-group popupResGroup --plan popupapp12345
    ```

1. <span data-ttu-id="82df4-123">Verificare che l'app sia stata creata correttamente elencando tutte le app in una tabella.</span><span class="sxs-lookup"><span data-stu-id="82df4-123">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```bash
    az webapp list --output table
    ```

1. <span data-ttu-id="82df4-124">Prendere nota di **DefaultHostName** perché sarà necessario in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="82df4-124">Make a note of the **DefaultHostName**; you will need this later.</span></span>

### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="82df4-125">Procedura per distribuire il codice da GitHub</span><span class="sxs-lookup"><span data-stu-id="82df4-125">Steps to deploy code from GitHub</span></span>
1. <span data-ttu-id="82df4-126">Il passaggio finale consiste nel distribuire il codice da un repository di GitHub all'app Web, ricordando ancora una volta di sostituire la stringa "12345" con lo stesso numero casuale usato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="82df4-126">The final step is to deploy code from a GitHub repository to the web app, again remembering to change the string "12345" to the same random number you used earlier.</span></span>
    ```bash
    az webapp deployment source config --name popupapp12345 --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="82df4-127">Copiare l'URL seguente in un browser per visualizzare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="82df4-127">Copy the following url into a browser to see the web app.</span></span>
<span data-ttu-id="82df4-128">http://popupapp12345.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="82df4-128">http://popupapp12345.azurewebsites.net</span></span>

1. <span data-ttu-id="82df4-129">Nella pagina viene visualizzato "HelloWorld!"</span><span class="sxs-lookup"><span data-stu-id="82df4-129">The page displays "HelloWorld!"</span></span>

1. <span data-ttu-id="82df4-130">Chiudere la finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="82df4-130">Close the browser window.</span></span>

## <a name="summary"></a><span data-ttu-id="82df4-131">Summary</span><span class="sxs-lookup"><span data-stu-id="82df4-131">Summary</span></span>
<span data-ttu-id="82df4-132">In questo esercizio è stato dimostrato un modello tipico per una sessione interattiva dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="82df4-132">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="82df4-133">È stato prima di tutto usato un comando standard per creare un nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="82df4-133">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="82df4-134">È stato quindi usato un set di comandi per distribuire una risorsa (in questo esempio, un'app Web) in questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="82df4-134">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="82df4-135">Questo set di comandi può essere facilmente combinato in uno script della shell da eseguire ogni volta che è necessario creare la stessa risorsa.</span><span class="sxs-lookup"><span data-stu-id="82df4-135">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>
