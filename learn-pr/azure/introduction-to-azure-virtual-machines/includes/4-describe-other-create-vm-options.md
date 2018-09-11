<span data-ttu-id="919d6-101">Il portale di Azure è il modo più semplice per creare per la prima volta risorse come le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="919d6-101">The Azure portal is the easiest way to create resources such as VMs when you are getting started.</span></span> <span data-ttu-id="919d6-102">Tuttavia, non si tratta necessariamente del modo più rapido o efficiente per lavorare con Azure, specialmente se si devono creare più risorse contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="919d6-102">However, it's not necessarily the most efficient or quickest way to work with Azure, particularly if you need to create several resources together.</span></span> <span data-ttu-id="919d6-103">In questo caso, si creeranno decine di macchine virtuali per gestire attività diverse.</span><span class="sxs-lookup"><span data-stu-id="919d6-103">In our case, we will eventually be creating dozens of VMs to handle different tasks.</span></span> <span data-ttu-id="919d6-104">Senza dubbio, crearle manualmente nel portale di Azure non è la soluzione ideale;</span><span class="sxs-lookup"><span data-stu-id="919d6-104">Creating them manually in the Azure portal wouldn't be a fun task!</span></span>

<span data-ttu-id="919d6-105">verranno pertanto illustrati altri modi per creare e amministrare risorse in Azure:</span><span class="sxs-lookup"><span data-stu-id="919d6-105">Let's look at some other ways to create and administer resources in Azure:</span></span>

- [<span data-ttu-id="919d6-106">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="919d6-106">Azure Resource Manager</span></span>](#Azure_RM)
- [<span data-ttu-id="919d6-107">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="919d6-107">Azure PowerShell</span></span>](#Azure_PowerShell)
- [<span data-ttu-id="919d6-108">Interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-108">Azure CLI</span></span>](#Azure_CLI)
- [<span data-ttu-id="919d6-109">API REST di Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-109">Azure REST API</span></span>](#Azure_REST_API)
- [<span data-ttu-id="919d6-110">SDK per client Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-110">Azure Client SDK</span></span>](#Azure_Client_SDK)
- [<span data-ttu-id="919d6-111">Estensioni di macchina virtuale di Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-111">Azure VM Extensions</span></span>](#Azure_VMExtensions)
- [<span data-ttu-id="919d6-112">Servizi di Automazione di Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-112">Azure Automation Services</span></span>](#Azure_Automation)

<a name="Azure_RM" />

## <a name="azure-resource-manager"></a><span data-ttu-id="919d6-113">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="919d6-113">Azure Resource Manager</span></span>

<span data-ttu-id="919d6-114">Si supponga di voler creare una copia di una macchina virtuale con le stesse impostazioni.</span><span class="sxs-lookup"><span data-stu-id="919d6-114">Let's assume you want to create a copy of a VM with the same settings.</span></span> <span data-ttu-id="919d6-115">È possibile creare un'immagine di macchina virtuale, caricarla in Azure e basarsi su di essa per la nuova macchina,</span><span class="sxs-lookup"><span data-stu-id="919d6-115">You could create a VM image, upload it to Azure and reference it as the basis for your new VM.</span></span> <span data-ttu-id="919d6-116">ma si tratta di un processo inefficiente e che richiede molto tempo.</span><span class="sxs-lookup"><span data-stu-id="919d6-116">This process is inefficient and time-consuming.</span></span> <span data-ttu-id="919d6-117">Azure offre la possibilità di creare un modello da cui creare una copia esatta di una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="919d6-117">Azure provides you with the option to create a template from which to create an exact copy of a VM.</span></span>

<span data-ttu-id="919d6-118">In genere, l'infrastruttura di Azure contiene molte risorse, molte delle quali correlate in qualche modo.</span><span class="sxs-lookup"><span data-stu-id="919d6-118">Typically, your Azure infrastructure will contain many resources, many of them related to one another in some way.</span></span> <span data-ttu-id="919d6-119">Ad esempio, la macchina virtuale creata dispone di archiviazione, interfaccia di rete, server Web, database e della macchina stessa, tutte risorse create contemporaneamente per eseguire il sito WordPress.</span><span class="sxs-lookup"><span data-stu-id="919d6-119">For example, the VM we created has the virtual machine itself, storage, network interface, web server, and a database - all created together to run the WordPress site.</span></span> <span data-ttu-id="919d6-120">**Azure Resource Manager** permette di lavorare in modo più efficiente con queste risorse correlate,</span><span class="sxs-lookup"><span data-stu-id="919d6-120">**Azure Resource Manager** makes working with these related resources more efficient.</span></span> <span data-ttu-id="919d6-121">organizzandole in **gruppi di risorse** denominati che consentono di distribuirle, aggiornarle o eliminarle tutte contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="919d6-121">It organizes resources into named **Resource Groups** that let you deploy, update, or delete all of the resources together.</span></span> <span data-ttu-id="919d6-122">Quando è stato creato il sito WordPress, il gruppo di risorse è stato identificato come parte della creazione della macchina virtuale e Resource Manager ha posizionato le risorse associate nel medesimo gruppo.</span><span class="sxs-lookup"><span data-stu-id="919d6-122">When we created the WordPress site, we identified the Resource Group as part of the VM creation, and Resource Manager placed the associated resources into the same group.</span></span>

<span data-ttu-id="919d6-123">Resource Manager consente anche di creare _modelli_ utilizzabili per creare e distribuire configurazioni specifiche.</span><span class="sxs-lookup"><span data-stu-id="919d6-123">Resource Manager also allows you to create _templates_ which can be used to create and deploy specific configurations.</span></span>

### <a name="what-are-resource-manager-templates"></a><span data-ttu-id="919d6-124">Cosa sono i modelli di Azure Resource Manager?</span><span class="sxs-lookup"><span data-stu-id="919d6-124">What are Resource Manager templates?</span></span>

<span data-ttu-id="919d6-125">I **modelli di Resource Manager** sono file JSON che definiscono le risorse che è necessario distribuire per la soluzione.</span><span class="sxs-lookup"><span data-stu-id="919d6-125">**Resource Manager templates** are JSON files that define the resources you need to deploy for your solution.</span></span>

<span data-ttu-id="919d6-126">È possibile creare modelli di risorse dalla sezione **Impostazioni** per una determinata macchina virtuale selezionando l'opzione Script di automazione.</span><span class="sxs-lookup"><span data-stu-id="919d6-126">You can create resource templates from the **Settings** section for a specific VM by selecting the Automation script option.</span></span>

![Script di automazione per la macchina virtuale](../media-draft/4-automation-script.png)

<span data-ttu-id="919d6-128">È possibile salvare il modello di risorse per usarlo in un secondo momento o distribuire subito una nuova macchina virtuale basata su di esso.</span><span class="sxs-lookup"><span data-stu-id="919d6-128">You have the option to save the resource template for later use or immediately deploy a new VM based on this template.</span></span> <span data-ttu-id="919d6-129">Ad esempio, si potrebbe creare una macchina virtuale da un modello in un ambiente di test e riscontrare che non è adatta a sostituire quella locale.</span><span class="sxs-lookup"><span data-stu-id="919d6-129">For example, you might create a VM from a template in a test environment and find it doesn’t quite work to replace your on-premise machine.</span></span> <span data-ttu-id="919d6-130">È possibile eliminare il gruppo di risorse, azione che eliminerà tutte le risorse, modificare il modello e riprovare.</span><span class="sxs-lookup"><span data-stu-id="919d6-130">You can delete the resource group, which deletes all of the resources, tweak the template and try again.</span></span> <span data-ttu-id="919d6-131">Se si desidera solo apportare modifiche alle risorse distribuite esistenti, è possibile modificare il modello usato per crearla e distribuirla nuovamente.</span><span class="sxs-lookup"><span data-stu-id="919d6-131">If you only want to make changes to the existing deployed resources, you can change the template used to create it and deploy it again.</span></span> <span data-ttu-id="919d6-132">Resource Manager modificherà le risorse in modo da correlarle al nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="919d6-132">Resource Manager will change the resources to match the new template.</span></span>

<span data-ttu-id="919d6-133">Una volta ottenuto il modello desiderato, è possibile usarlo per ricreare più versioni dell'infrastruttura, ad esempio per la gestione temporanea e la produzione.</span><span class="sxs-lookup"><span data-stu-id="919d6-133">Once you have it working the way you want it, you can take that template and easily re-create multiple versions of your infrastructure, such as staging and production.</span></span> <span data-ttu-id="919d6-134">È possibile impostare i parametri per i campi, ad esempio il nome della macchina virtuale, il nome di rete, il nome dell'account di archiviazione e così via, con la possibilità di caricare il modello più volte usando parametri diversi per personalizzare ciascun ambiente.</span><span class="sxs-lookup"><span data-stu-id="919d6-134">You can parameterize fields such as the VM name, network name, storage account name, etc., and load the template repeatedly, using different parameters to customize each environment.</span></span>

<span data-ttu-id="919d6-135">È possibile usare strumenti per gli script di automazione come l'interfaccia della riga di comando di Azure, Azure PowerShell o anche le API REST di Azure con il linguaggio di programmazione preferito per elaborare i modelli di risorse, cosa che rende questo strumento utile per rendere subito operativa l'infrastruttura.</span><span class="sxs-lookup"><span data-stu-id="919d6-135">You can use automation scripting tools such as the Azure CLI, Azure PowerShell, or even the Azure REST APIs with your favorite programming language to process resource templates making this a powerful tool for quickly spinning up your infrastructure.</span></span>

<a name="Azure_PowerShell" />

## <a name="azure-powershell"></a><span data-ttu-id="919d6-136">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="919d6-136">Azure PowerShell</span></span>

<span data-ttu-id="919d6-137">La creazione di script di amministrazione è un modo efficace per ottimizzare il flusso di lavoro, poiché è possibile automatizzare attività quotidiane e ripetitive e, una volta verificato uno script, questo verrà eseguito costantemente, riducendo gli errori potenziali.</span><span class="sxs-lookup"><span data-stu-id="919d6-137">Creating administration scripts is a powerful way to optimize your workflow, you can automate everyday, repetitive tasks, and once a script has been verified, it will run consistently, likely reducing errors.</span></span> <span data-ttu-id="919d6-138">**Azure PowerShell** è ideale per attività occasionali interattive e per l'automazione di attività ripetute.</span><span class="sxs-lookup"><span data-stu-id="919d6-138">**Azure PowerShell** is ideal for one-off interactive tasks and or the automate of repeated tasks.</span></span>

> [!NOTE]
> <span data-ttu-id="919d6-139">PowerShell è una shell multipiattaforma che fornisce servizi come la finestra della shell e l'analisi dei comandi.</span><span class="sxs-lookup"><span data-stu-id="919d6-139">PowerShell is a cross-platform shell that provides services like the shell window and command parsing.</span></span> <span data-ttu-id="919d6-140">Azure PowerShell è un pacchetto aggiuntivo facoltativo che aggiunge i comandi specifici di Azure (detti **cmdlet**).</span><span class="sxs-lookup"><span data-stu-id="919d6-140">Azure PowerShell is an optional add-on package which adds the Azure-specific commands (referred to as **cmdlets**).</span></span> <span data-ttu-id="919d6-141">È possibile ottenere altre informazioni sull'installazione e l'utilizzo di Azure PowerShell in un modulo di formazione separato.</span><span class="sxs-lookup"><span data-stu-id="919d6-141">You can learn more about installing and using Azure PowerShell in a separate training module.</span></span>

<span data-ttu-id="919d6-142">Ad esempio, è possibile usare il cmdlet `New-AzureRmVM` per creare una nuova macchina virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="919d6-142">For example, you can use the `New-AzureRmVM` cmdlet to create a new Azure Virtual Machine.</span></span> 

```powershell
New-AzureRmVm `
    -ResourceGroupName "TestResourceGroup" `
    -Name "test-wp1-eus-vm" `
    -Location "East US" `
    -VirtualNetworkName "test-wp1-eus-network" `
    -SubnetName "default" `
    -SecurityGroupName "test-wp1-eus-nsg" `
    -PublicIpAddressName "test-wp1-eus-pubip" `
    -OpenPorts 80,3389
```

<span data-ttu-id="919d6-143">Come illustrato di seguito, vengono forniti vari parametri per gestire tutte le varie impostazioni di configurazione della macchina virtuale disponibili.</span><span class="sxs-lookup"><span data-stu-id="919d6-143">As shown here, you supply various parameters to handle the large number of VM configuration settings available.</span></span> <span data-ttu-id="919d6-144">La maggior parte dei parametri ha valori accettabili, pertanto è necessario specificare solo quelli richiesti.</span><span class="sxs-lookup"><span data-stu-id="919d6-144">Most of the parameters have reasonable values; you only need to specify the required parameters.</span></span> <span data-ttu-id="919d6-145">Altre informazioni sulla creazione e la gestione di macchine virtuali con Azure PowerShell sono disponibili nel modulo **Automatizzare le attività di Azure usando gli script con PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="919d6-145">Learn more about creating and managing VMs with Azure PowerShell in the **Automate Azure tasks using scripts with PowerShell** module.</span></span>
<a name="Azure_CLI" />

## <a name="azure-cli"></a><span data-ttu-id="919d6-146">Interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-146">Azure CLI</span></span>

<span data-ttu-id="919d6-147">Un'altra opzione per l'interazione di scripting e da riga di comando in Azure è l'**interfaccia della riga di comando di Azure**.</span><span class="sxs-lookup"><span data-stu-id="919d6-147">Another option for scripting and command-line Azure interaction is the **Azure CLI**.</span></span>

<span data-ttu-id="919d6-148">L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure, ad esempio macchine virtuali e dischi, dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="919d6-148">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources such as virtual machines and disks from the command line.</span></span> <span data-ttu-id="919d6-149">È disponibile per macOS, Linux e Windows oppure nel browser, tramite Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="919d6-149">It's available for macOS, Linux, and Windows, or in the browser using the Cloud Shell.</span></span> <span data-ttu-id="919d6-150">Come Azure PowerShell, l'interfaccia della riga di comando di Azure è un potente strumento da usare per semplificare il flusso di lavoro di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="919d6-150">Like Azure PowerShell, the Azure CLI is a powerful way to streamline your administrative workflow.</span></span> <span data-ttu-id="919d6-151">A differenza di Azure PowerShell, l'interfaccia della riga di comando di Azure non necessita di PowerShell per il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="919d6-151">Unlike Azure PowerShell, the Azure CLI does not need PowerShell to function.</span></span>

<span data-ttu-id="919d6-152">Ad esempio, è possibile creare una macchina virtuale di Azure con il comando `az vm create`.</span><span class="sxs-lookup"><span data-stu-id="919d6-152">For example, you can create an Azure VM with the `az vm create` command.</span></span>

```bash
az vm create \
    --resource-group TestResourceGroup \
    --name test-wp1-eus-vm \
    --image win2016datacenter \
    --admin-username jonc \
    --admin-password aReallyGoodPasswordHere
```

<span data-ttu-id="919d6-153">L'interfaccia della riga di comando di Azure è compatibile con altri linguaggi di scripting, ad esempio Ruby e Python.</span><span class="sxs-lookup"><span data-stu-id="919d6-153">The Azure CLI can be used with other scripting languages, for example, Ruby and Python.</span></span> <span data-ttu-id="919d6-154">Entrambi i linguaggi vengono comunemente usati nei computer non basati su Windows in cui lo sviluppatore potrebbe non avere familiarità con PowerShell.</span><span class="sxs-lookup"><span data-stu-id="919d6-154">Both languages are commonly used on non-windows based machines where the developer may not be familiar with PowerShell.</span></span>

<span data-ttu-id="919d6-155">Altre informazioni sulla creazione e la gestione di macchine virtuali sono disponibili nel modulo **Gestire le macchine virtuali con l'interfaccia della riga di comando di Azure**.</span><span class="sxs-lookup"><span data-stu-id="919d6-155">Learn more about creating and managing VMs in the **Manage virtual machines with the Azure CLI tool** module.</span></span>

## <a name="programmatic-apis"></a><span data-ttu-id="919d6-156">Approccio a livello di codice (API)</span><span class="sxs-lookup"><span data-stu-id="919d6-156">Programmatic (APIs)</span></span>

<span data-ttu-id="919d6-157">In generale, Azure PowerShell e l'interfaccia della riga di comando di Azure sono modi validi e si dispone di script semplici da eseguire e si preferisce usare strumenti a riga di comando.</span><span class="sxs-lookup"><span data-stu-id="919d6-157">Generally speaking, both Azure PowerShell and Azure CLI are good options if you have simple scripts to run and want to stick to command-line tools.</span></span> <span data-ttu-id="919d6-158">Quando entrano in gioco scenari più complessi, dove la creazione e la gestione delle macchine virtuali fanno parte di un'applicazione più ampia con logica più strutturata, è necessario un approccio diverso.</span><span class="sxs-lookup"><span data-stu-id="919d6-158">When it comes to more complex scenarios where the creation and management of VM form part of a larger application with complex logic, another approach is needed.</span></span>

<span data-ttu-id="919d6-159">È possibile interagire con tutti i tipi di risorse in Azure a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="919d6-159">You can interact with every type of resource in Azure programmatically.</span></span>

<a name="Azure_REST_API" />

### <a name="azure-rest-api"></a><span data-ttu-id="919d6-160">API REST di Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-160">Azure REST API</span></span>

<span data-ttu-id="919d6-161">L'API REST di Azure offre agli sviluppatori operazioni categorizzate per risorsa, oltre alla possibilità di creare e gestire le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="919d6-161">The Azure REST API provides developers operations categorized by resource as well as the ability to create and manage VMs.</span></span> <span data-ttu-id="919d6-162">Le operazioni sono esposte come URI con i corrispondenti metodi HTTP (`GET`, `PUT`, `POST`, `DELETE` e `PATCH`) e una risposta corrispondente.</span><span class="sxs-lookup"><span data-stu-id="919d6-162">Operations are exposed as URIs with corresponding HTTP methods (`GET`, `PUT`, `POST`, `DELETE`, and `PATCH`) and a corresponding response.</span></span>

<span data-ttu-id="919d6-163">Le API di calcolo di Azure forniscono accesso a livello di codice alle macchine virtuali e alle relative risorse di supporto.</span><span class="sxs-lookup"><span data-stu-id="919d6-163">The Azure Compute APIs give you programmatic access to virtual machines and their supporting resources.</span></span> <span data-ttu-id="919d6-164">Con questa API, sono disponibili operazioni per:</span><span class="sxs-lookup"><span data-stu-id="919d6-164">With this API, you have operations to:</span></span>

- <span data-ttu-id="919d6-165">Creare e gestire set di disponibilità</span><span class="sxs-lookup"><span data-stu-id="919d6-165">Create and manage availability sets</span></span>
- <span data-ttu-id="919d6-166">Aggiungere e gestire estensioni macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="919d6-166">Add and manage virtual machine extensions</span></span>
- <span data-ttu-id="919d6-167">Creare e gestire dischi gestiti, snapshot e immagini</span><span class="sxs-lookup"><span data-stu-id="919d6-167">Create and manage managed disks, snapshots, and images</span></span>
- <span data-ttu-id="919d6-168">Accedere alle immagini della piattaforma disponibili in Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-168">Access the platform images available in Azure</span></span>
- <span data-ttu-id="919d6-169">Recuperare le informazioni sull'utilizzo delle risorse</span><span class="sxs-lookup"><span data-stu-id="919d6-169">Retrieve usage information of your resources</span></span>
- <span data-ttu-id="919d6-170">Creare e gestire macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="919d6-170">Create and manage virtual machines</span></span>
- <span data-ttu-id="919d6-171">Creare e gestire i set di scalabilità delle macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="919d6-171">Create and manage virtual machine scale sets</span></span>

<a name="Azure_Client_SDK" />

### <a name="azure-client-sdk"></a><span data-ttu-id="919d6-172">SDK per client Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-172">Azure Client SDK</span></span>

<span data-ttu-id="919d6-173">Anche se l'API REST non dipende da una piattaforma o da un linguaggio, spesso gli sviluppatori cercheranno un livello superiore di astrazione.</span><span class="sxs-lookup"><span data-stu-id="919d6-173">Even though the REST API is platform and language agnostic, most often developers will look towards a higher level of abstraction.</span></span> <span data-ttu-id="919d6-174">L'SDK per client Azure incapsula l'API REST, facilitando l'interazione degli sviluppatori con Azure.</span><span class="sxs-lookup"><span data-stu-id="919d6-174">The Azure Client SDK encapsulates the Azure REST API making it much easier for developers to interact with Azure.</span></span>

<span data-ttu-id="919d6-175">Gli SDK per client Azure sono disponibili per vari linguaggi e framework, inclusi quelli basati su .NET come C#, Java, Node.js, PHP, Python, Ruby e Go.</span><span class="sxs-lookup"><span data-stu-id="919d6-175">The Azure Client SDKs are available for a variety of languages and frameworks including .NET based languages such as C#, Java, Node.js, PHP, Python, Ruby, and Go.</span></span>

<span data-ttu-id="919d6-176">Ecco un frammento di esempio di codice C# per creare una macchina virtuale di Azure usando il pacchetto NuGet `Microsoft.Azure.Management.Fluent`:</span><span class="sxs-lookup"><span data-stu-id="919d6-176">Here's an example snippet of C# code to create an Azure VM using the `Microsoft.Azure.Management.Fluent` NuGet package:</span></span>

```csharp
var azure = Azure
    .Configure()
    .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
    .Authenticate(credentials)
    .WithDefaultSubscription();
// ...
var vmName = "test-wp1-eus-vm";

azure.VirtualMachines.Define(vmName)
    .WithRegion(Region.USEast)
    .WithExistingResourceGroup("TestResourceGroup")
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("jonc")
    .WithAdminPassword("aReallyGoodPasswordHere")
    .WithComputerName(vmName)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

<span data-ttu-id="919d6-177">Di seguito è riportato lo stesso frammento in Java con **Azure Java SDK**:</span><span class="sxs-lookup"><span data-stu-id="919d6-177">Here's the same snippet in Java using the **Azure Java SDK**:</span></span>

```java
String vmName = "test-wp1-eus-vm";
// ...
VirtualMachine virtualMachine = azure.virtualMachines()
    .define(vmName)
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("TestResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("jonc")
    .withAdminPassword("aReallyGoodPasswordHere")
    .withComputerName(vmName)
    .withSize("Standard_DS1")
    .create();
```

<a name="Azure_VMExtensions" />

## <a name="azure-vm-extensions"></a><span data-ttu-id="919d6-178">Estensioni di macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="919d6-178">Azure VM Extensions</span></span>

<span data-ttu-id="919d6-179">Si supponga di voler configurare e installare software aggiuntivo sulla macchina virtuale dopo la distribuzione iniziale.</span><span class="sxs-lookup"><span data-stu-id="919d6-179">Let's assume you want to configure and install additional software on your virtual machine after the initial deployment.</span></span> <span data-ttu-id="919d6-180">Si desidera che l'attività usi una configurazione specifica, monitorata ed eseguita automaticamente.</span><span class="sxs-lookup"><span data-stu-id="919d6-180">You want this task to use a specific configuration, monitored and executed automatically.</span></span>

<span data-ttu-id="919d6-181">**Le estensioni macchina virtuale di Azure** sono piccole applicazioni che consentono di configurare e automatizzare le attività nelle macchine virtuali di Azure dopo la distribuzione iniziale.</span><span class="sxs-lookup"><span data-stu-id="919d6-181">**Azure VM extensions** are small applications that allow you to configure and automate tasks on Azure VMs after initial deployment.</span></span> <span data-ttu-id="919d6-182">**Le estensioni macchina virtuale di Azure** possono essere eseguite tramite l'interfaccia della riga di comando di Azure, PowerShell, i modelli di Azure Resource Manager e il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="919d6-182">**Azure VM extensions** can be run with the Azure CLI, PowerShell, Azure Resource Manager templates, and the Azure portal.</span></span>

<span data-ttu-id="919d6-183">Possono essere aggregate con una nuova distribuzione di macchina virtuale o eseguite su un sistema esistente.</span><span class="sxs-lookup"><span data-stu-id="919d6-183">You bundle extensions with a new VM deployment or run them against an existing system.</span></span>

<a name="Azure_Automation" />

## <a name="azure-automation-services"></a><span data-ttu-id="919d6-184">Servizi di Automazione di Azure</span><span class="sxs-lookup"><span data-stu-id="919d6-184">Azure Automation Services</span></span>

<span data-ttu-id="919d6-185">Alcune delle sfide operative più rilevanti nella gestione dell'infrastruttura remota consistono nel risparmiare tempo, ridurre gli errori e aumentare l'efficienza.</span><span class="sxs-lookup"><span data-stu-id="919d6-185">Saving time, reducing errors, and increasing efficiency are some of the most significant operational management challenges faced when managing remote infrastructure.</span></span> <span data-ttu-id="919d6-186">Se si dispone di molti servizi di infrastruttura, è consigliabile usare servizi di livello superiore in Azure per lavorare con più facilità.</span><span class="sxs-lookup"><span data-stu-id="919d6-186">If you have a lot of infrastructure services, you might want to consider using higher-level services in Azure to help you operate from a higher level.</span></span>

<span data-ttu-id="919d6-187">**Automazione di Azure** consente di integrare i servizi che consentono di automatizzare con facilità le attività di gestione più frequenti, complesse e soggette a errori.</span><span class="sxs-lookup"><span data-stu-id="919d6-187">**Azure Automation** allows you to integrate services that allow you to automate frequent, time-consuming and error-prone management tasks with ease.</span></span> <span data-ttu-id="919d6-188">Questi servizi includono **automazione dei processi**, \*\*gestione della configurazione e **gestione degli aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="919d6-188">These services include **process automation**, \*\*configuration management, and **update management**.</span></span>

- <span data-ttu-id="919d6-189">**Gestione dei processi**.</span><span class="sxs-lookup"><span data-stu-id="919d6-189">**Process Management**.</span></span> <span data-ttu-id="919d6-190">Si supponga di avere una macchina virtuale monitorata per un evento di errore specifico.</span><span class="sxs-lookup"><span data-stu-id="919d6-190">Let's assume you have a VM that is monitored for a specific error event.</span></span> <span data-ttu-id="919d6-191">Si desidera intervenire e risolvere il problema non appena viene segnalato.</span><span class="sxs-lookup"><span data-stu-id="919d6-191">You want to take action and fix the problem as soon as it's reported.</span></span> <span data-ttu-id="919d6-192">L'automazione dei processi consente di configurare attività watcher in grado di rispondere agli eventi che possono verificarsi nel data center.</span><span class="sxs-lookup"><span data-stu-id="919d6-192">Process automation allows you to set up watcher tasks that can respond to events that may occur in your datacenter.</span></span>

- <span data-ttu-id="919d6-193">**Gestione della configurazione**.</span><span class="sxs-lookup"><span data-stu-id="919d6-193">**Configuration Management**.</span></span>  <span data-ttu-id="919d6-194">Potrebbe essere necessario tener traccia degli aggiornamenti software disponibili per il sistema operativo eseguito nella macchina virtuale,</span><span class="sxs-lookup"><span data-stu-id="919d6-194">Perhaps you want to track software updates that become available for the operating system that runs on your VM.</span></span> <span data-ttu-id="919d6-195">con aggiornamenti specifici da includere o escludere.</span><span class="sxs-lookup"><span data-stu-id="919d6-195">There are specific updates you may want to include or exclude.</span></span> <span data-ttu-id="919d6-196">La gestione della configurazione consente di tenere traccia di questi aggiornamenti e di intervenire in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="919d6-196">Configuration management allows you to track these updates and take action as required.</span></span> <span data-ttu-id="919d6-197">È possibile usare **System Center Configuration Manager** per gestire PC, server e dispositivi mobili aziendali.</span><span class="sxs-lookup"><span data-stu-id="919d6-197">You use **System Center Configuration Manager** to manage your company's PC, servers, and mobile devices.</span></span> <span data-ttu-id="919d6-198">Il supporto può essere esteso alle macchine virtuali di Azure grazie a Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="919d6-198">You can extend this support to your Azure VMs with Configuration Manager.</span></span>

- <span data-ttu-id="919d6-199">**Gestione aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="919d6-199">**Update Management**.</span></span> <span data-ttu-id="919d6-200">È possibile gestire gli aggiornamenti e le patch per le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="919d6-200">This is used to manage updates and patches for your VMs.</span></span> <span data-ttu-id="919d6-201">Con questo servizio, si può valutare in modo rapido lo stato degli aggiornamenti disponibili, pianificare l'installazione ed esaminare i risultati della distribuzione per verificare che gli aggiornamenti siano stati applicati correttamente.</span><span class="sxs-lookup"><span data-stu-id="919d6-201">With this service, you're able to assess the status of available updates, schedule installation, and review deployment results to verify updates applied successfully.</span></span> <span data-ttu-id="919d6-202">La gestione degli aggiornamenti include servizi che permettono di gestire i processi e la configurazione.</span><span class="sxs-lookup"><span data-stu-id="919d6-202">Update management incorporates services that provide process and configuration management.</span></span> <span data-ttu-id="919d6-203">È possibile abilitare Gestione aggiornamenti per una macchina virtuale direttamente nell'account di **Automazione di Azure**.</span><span class="sxs-lookup"><span data-stu-id="919d6-203">You enable update management for a VM directly from your **Azure Automation** account.</span></span> <span data-ttu-id="919d6-204">È anche possibile abilitare Gestione aggiornamenti per una singola macchina virtuale dal riquadro della macchina virtuale nel portale.</span><span class="sxs-lookup"><span data-stu-id="919d6-204">You can also allow update management for a single virtual machine from the virtual machine blade in the portal.</span></span>

<span data-ttu-id="919d6-205">Come si può notare, Azure offre un'ampia gamma di strumenti per creare e amministrare le risorse, pertanto è possibile integrare le operazioni di gestione nel processo _più appropriato_.</span><span class="sxs-lookup"><span data-stu-id="919d6-205">As you can see, Azure provides a variety of tools to create and administer resources so you can integrate management operations into a process _that works for you_.</span></span> <span data-ttu-id="919d6-206">Ora verranno esaminati altri servizi di Azure per assicurare il funzionamento ottimale delle risorse di infrastruttura.</span><span class="sxs-lookup"><span data-stu-id="919d6-206">Let's examine some of the other services Azure to make sure your infrastructure resources are running smoothly.</span></span>
