<span data-ttu-id="074c3-101">Come professionista IT, è probabile che si abbia esperienza in un'area specifica,</span><span class="sxs-lookup"><span data-stu-id="074c3-101">As a technology professional, you likely have expertise in a specific area.</span></span> <span data-ttu-id="074c3-102">ad esempio come amministratore dell'archiviazione o esperto di virtualizzazione o delle procedure più recenti per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="074c3-102">Perhaps you're a storage admin or virtualization expert, or maybe you focus on the latest security practices.</span></span> <span data-ttu-id="074c3-103">Gli studenti potrebbero essere ancora alla ricerca dell'area di maggiore interesse.</span><span class="sxs-lookup"><span data-stu-id="074c3-103">If you're a student, you may still be exploring what interests you most.</span></span>

<span data-ttu-id="074c3-104">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="074c3-104">::: zone pivot="windows-cloud"</span></span>

<span data-ttu-id="074c3-105">Indipendentemente dal ruolo, la maggior parte delle persone inizia l'esperienza con il cloud creando una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-105">No matter your role, most people get started with the cloud by creating a virtual machine.</span></span> <span data-ttu-id="074c3-106">Qui verranno descritte le procedure per attivare una macchina virtuale che esegue Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="074c3-106">Here you'll bring up a virtual machine running Windows Server 2016.</span></span>

<span data-ttu-id="074c3-107">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="074c3-107">::: zone-end</span></span>

<span data-ttu-id="074c3-108">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="074c3-108">::: zone pivot="linux-cloud"</span></span>

<span data-ttu-id="074c3-109">Indipendentemente dal ruolo, la maggior parte delle persone inizia l'esperienza con il cloud creando una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-109">No matter your role, most people get started with the cloud by creating a virtual machine.</span></span> <span data-ttu-id="074c3-110">Qui verranno descritte le procedure per attivare una macchina virtuale che esegue Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="074c3-110">Here you'll bring up a virtual machine running Ubuntu 16.04.</span></span>

<span data-ttu-id="074c3-111">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="074c3-111">::: zone-end</span></span>

<span data-ttu-id="074c3-112">Esistono molti modi per creare una macchina virtuale in Azure.</span><span class="sxs-lookup"><span data-stu-id="074c3-112">There are many ways to create a virtual machine on Azure.</span></span> <span data-ttu-id="074c3-113">Qui verrà descritto come attivare una macchina virtuale Windows o Linux usando un terminale interattivo denominato Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="074c3-113">Here, you'll bring up a Windows or Linux virtual machine using an interactive terminal called Cloud Shell.</span></span> <span data-ttu-id="074c3-114">Chi lavora quotidianamente dal terminale sa che spesso rappresenta il modo più rapido per svolgere varie operazioni.</span><span class="sxs-lookup"><span data-stu-id="074c3-114">If you work from the terminal on a daily basis, you know this is often the fastest way to get the job done.</span></span>

<span data-ttu-id="074c3-115">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="074c3-115">::: zone pivot="windows-cloud"</span></span>

> [!TIP]
> <span data-ttu-id="074c3-116">Si preferisce Linux o si vuole provare qualcosa di nuovo?</span><span class="sxs-lookup"><span data-stu-id="074c3-116">Prefer Linux or want to try something new?</span></span> <span data-ttu-id="074c3-117">Selezionare **Linux** nella parte superiore di questa pagina per eseguire una macchina virtuale Linux.</span><span class="sxs-lookup"><span data-stu-id="074c3-117">Select **Linux** from the top of this page to run a Linux virtual machine.</span></span>

<span data-ttu-id="074c3-118">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="074c3-118">::: zone-end</span></span>

<span data-ttu-id="074c3-119">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="074c3-119">::: zone pivot="linux-cloud"</span></span>

> [!TIP]
> <span data-ttu-id="074c3-120">Si preferisce Windows o si vuole provare qualcosa di nuovo?</span><span class="sxs-lookup"><span data-stu-id="074c3-120">Prefer Windows or want to try something new?</span></span> <span data-ttu-id="074c3-121">Selezionare **Windows** nella parte superiore di questa pagina per eseguire una macchina virtuale Windows Server.</span><span class="sxs-lookup"><span data-stu-id="074c3-121">Select **Windows** from the top of this page to run a Windows Server virtual machine.</span></span>

<span data-ttu-id="074c3-122">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="074c3-122">::: zone-end</span></span>

<span data-ttu-id="074c3-123">Verranno presentati alcuni termini di base e poi si procederà all'attivazione ed esecuzione della prima macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-123">Let's review some basic terms and get your first virtual machine up and running.</span></span>

## <a name="what-is-a-virtual-machine"></a><span data-ttu-id="074c3-124">Che cos'è una macchina virtuale?</span><span class="sxs-lookup"><span data-stu-id="074c3-124">What is a virtual machine?</span></span>

<span data-ttu-id="074c3-125">Una macchina virtuale è un'emulazione software di un computer fisico.</span><span class="sxs-lookup"><span data-stu-id="074c3-125">A virtual machine, or VM, is a software emulation of a physical computer.</span></span> <span data-ttu-id="074c3-126">Poiché le macchine virtuali esistono come software, bastano pochi minuti per generare decine, centinaia o persino migliaia di macchine virtuali di Azure e per eliminarle quando non servono più.</span><span class="sxs-lookup"><span data-stu-id="074c3-126">Because VMs exist as software, dozens, hundreds, or even thousands of Azure VMs can be generated in minutes, then deleted when you don't need them anymore.</span></span> <span data-ttu-id="074c3-127">Il costo è conveniente e la fatturazione viene calcolata al minuto, quindi si paga solo per le risorse di calcolo usate e fino a quando le si usa.</span><span class="sxs-lookup"><span data-stu-id="074c3-127">With low-cost, per-minute billing, you pay only for the compute resources you use, for as long as you are using them.</span></span> <span data-ttu-id="074c3-128">Esistono inoltre molti modi per configurare le macchine virtuali in modo da soddisfare esigenze specifiche.</span><span class="sxs-lookup"><span data-stu-id="074c3-128">Plus, there are many ways to configure the VMs to fit your needs.</span></span>

<span data-ttu-id="074c3-129">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="074c3-129">::: zone pivot="windows-cloud"</span></span>

<span data-ttu-id="074c3-130">Uno snapshot di una macchina virtuale in esecuzione viene chiamato _immagine_.</span><span class="sxs-lookup"><span data-stu-id="074c3-130">A snapshot of a running VM is called an _image_.</span></span> <span data-ttu-id="074c3-131">Azure offre immagini per Windows e per varie versioni di Linux.</span><span class="sxs-lookup"><span data-stu-id="074c3-131">Azure provides images for Windows and several flavors of Linux.</span></span> <span data-ttu-id="074c3-132">È anche possibile creare immagini preconfigurate personalizzate per velocizzare le distribuzioni.</span><span class="sxs-lookup"><span data-stu-id="074c3-132">You can also create your own preconfigured images to make deployments go faster.</span></span> <span data-ttu-id="074c3-133">Qui verranno descritte le procedure per attivare una macchina virtuale con Windows Server 2016 fornita da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="074c3-133">Here you'll bring up a Windows Server 2016 VM, provided by Microsoft.</span></span>

<span data-ttu-id="074c3-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="074c3-134">::: zone-end</span></span>

<span data-ttu-id="074c3-135">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="074c3-135">::: zone pivot="linux-cloud"</span></span>

<span data-ttu-id="074c3-136">Uno snapshot di una macchina virtuale in esecuzione viene chiamato _immagine_.</span><span class="sxs-lookup"><span data-stu-id="074c3-136">A snapshot of a running VM is called an _image_.</span></span> <span data-ttu-id="074c3-137">Azure offre immagini per Windows e per varie versioni di Linux.</span><span class="sxs-lookup"><span data-stu-id="074c3-137">Azure provides images for Windows and several flavors of Linux.</span></span> <span data-ttu-id="074c3-138">È anche possibile creare immagini preconfigurate personalizzate per velocizzare le distribuzioni.</span><span class="sxs-lookup"><span data-stu-id="074c3-138">You can also create your own preconfigured images to make deployments go faster.</span></span> <span data-ttu-id="074c3-139">Qui verranno descritte le procedure per attivare una macchina virtuale con Ubuntu 16.04 fornita da Canonical.</span><span class="sxs-lookup"><span data-stu-id="074c3-139">Here you'll bring up an Ubuntu 16.04 VM, provided by Canonical.</span></span>

<span data-ttu-id="074c3-140">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="074c3-140">::: zone-end</span></span>

## <a name="what-defines-a-virtual-machine-on-azure"></a><span data-ttu-id="074c3-141">Cosa definisce una macchina virtuale in Azure?</span><span class="sxs-lookup"><span data-stu-id="074c3-141">What defines a virtual machine on Azure?</span></span>

<span data-ttu-id="074c3-142">Una macchina virtuale è definita da numerosi fattori, tra cui dimensioni e posizione.</span><span class="sxs-lookup"><span data-stu-id="074c3-142">A virtual machine is defined by a number of factors, including its size and location.</span></span> <span data-ttu-id="074c3-143">Prima di attivare la macchina virtuale, verranno illustrati brevemente gli elementi coinvolti.</span><span class="sxs-lookup"><span data-stu-id="074c3-143">Before you bring up your VM, let's briefly cover what's involved.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="074c3-144">**Dimensioni**</span><span class="sxs-lookup"><span data-stu-id="074c3-144">**Size**</span></span>
    :::column-end:::
    <span data-ttu-id="074c3-145">:::column span="3"::: Le _dimensioni_ di una macchina virtuale ne definiscono la velocità del processore, la quantità di memoria, la quantità iniziale di spazio di archiviazione e la larghezza di banda di rete prevista.</span><span class="sxs-lookup"><span data-stu-id="074c3-145">:::column span="3"::: A VM's _size_ defines its processor speed, amount of memory, initial amount of storage, and expected network bandwidth.</span></span> <span data-ttu-id="074c3-146">Alcune dimensioni includono anche hardware specialistico, come GPU per carichi intensivi di rendering della grafica e modifica di video.</span><span class="sxs-lookup"><span data-stu-id="074c3-146">Some sizes even include specialized hardware such as GPUs for heavy graphics rendering and video editing.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="074c3-147">**Area**</span><span class="sxs-lookup"><span data-stu-id="074c3-147">**Region**</span></span>
    :::column-end:::
    <span data-ttu-id="074c3-148">:::column span="3"::: Azure è costituito da data center distribuiti in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="074c3-148">:::column span="3"::: Azure is made up of data centers distributed throughout the world.</span></span> <span data-ttu-id="074c3-149">Un'_area_ è un set di data center di Azure in un'area geografica denominata.</span><span class="sxs-lookup"><span data-stu-id="074c3-149">A _region_ is a set of Azure data centers in a named geographic location.</span></span> <span data-ttu-id="074c3-150">A ogni risorsa di Azure, incluse le macchine virtuali, viene assegnata un'area.</span><span class="sxs-lookup"><span data-stu-id="074c3-150">Every Azure resource, including virtual machines, is assigned a region.</span></span> <span data-ttu-id="074c3-151">Stati Uniti orientali ed Europa settentrionale sono esempi di aree.</span><span class="sxs-lookup"><span data-stu-id="074c3-151">East US and North Europe are examples of regions.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="074c3-152">**Rete**</span><span class="sxs-lookup"><span data-stu-id="074c3-152">**Network**</span></span>
    :::column-end:::
    <span data-ttu-id="074c3-153">:::column span="3"::: Una _rete virtuale_ è una rete isolata logicamente in Azure.</span><span class="sxs-lookup"><span data-stu-id="074c3-153">:::column span="3"::: A _virtual network_ is a logically isolated network on Azure.</span></span> <span data-ttu-id="074c3-154">Ogni macchina virtuale in Azure è associata a una rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-154">Each virtual machine on Azure is associated with a virtual network.</span></span> <span data-ttu-id="074c3-155">Azure offre firewall a livello di cloud per le reti virtuali denominati _gruppi di sicurezza di rete_.</span><span class="sxs-lookup"><span data-stu-id="074c3-155">Azure provides cloud-level firewalls for your virtual networks called _network security groups_.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="074c3-156">**Gruppi di risorse**</span><span class="sxs-lookup"><span data-stu-id="074c3-156">**Resource groups**</span></span>
    :::column-end:::
    <span data-ttu-id="074c3-157">:::column span="3"::: Le macchine virtuali e altre risorse cloud sono raggruppate in contenitori logici denominati _gruppi di risorse_.</span><span class="sxs-lookup"><span data-stu-id="074c3-157">:::column span="3"::: Virtual machines and other cloud resources are grouped into logical containers called _resource groups_.</span></span> <span data-ttu-id="074c3-158">I gruppi vengono in genere usati per organizzare set di risorse distribuite insieme come parte di un'applicazione o un servizio.</span><span class="sxs-lookup"><span data-stu-id="074c3-158">Groups are typically used to organize sets of resources that are deployed together as part of an application or service.</span></span> <span data-ttu-id="074c3-159">Si fa riferimento a un gruppo di risorse con il nome.</span><span class="sxs-lookup"><span data-stu-id="074c3-159">You refer to a resource group by its name.</span></span>
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="074c3-160">Che cos'è Azure Cloud Shell?</span><span class="sxs-lookup"><span data-stu-id="074c3-160">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="074c3-161">Azure Cloud Shell è un'esperienza di riga di comando basata su browser per la gestione e lo sviluppo di risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="074c3-161">Azure Cloud Shell is a browser-based command-line experience for managing and developing Azure resources.</span></span> <span data-ttu-id="074c3-162">Cloud Shell può essere paragonato a una console interattiva eseguita nel cloud.</span><span class="sxs-lookup"><span data-stu-id="074c3-162">Think of Cloud Shell as an interactive console that you run in the cloud.</span></span>

<span data-ttu-id="074c3-163">Cloud Shell offre due esperienze tra cui scegliere: Bash e PowerShell.</span><span class="sxs-lookup"><span data-stu-id="074c3-163">Cloud Shell provides two experiences to choose from: Bash and PowerShell.</span></span> <span data-ttu-id="074c3-164">Entrambe includono l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="074c3-164">Both include access to the Azure CLI, the command-line interface for Azure.</span></span>

<span data-ttu-id="074c3-165">È possibile usare qualsiasi interfaccia di gestione di Azure, tra cui il portale di Azure, l'interfaccia della riga di comando di Azure e Azure PowerShell, per gestire qualsiasi tipo di macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-165">You can use any Azure management interface, including the Azure portal, Azure CLI, and Azure PowerShell, to manage any kind of VM.</span></span> <span data-ttu-id="074c3-166">Ai fini dell'apprendimento, si userà l'interfaccia della riga di comando di Azure per creare e gestire una macchina virtuale Windows o Linux.</span><span class="sxs-lookup"><span data-stu-id="074c3-166">For learning purposes, here you'll use the Azure CLI to create and manage either a Windows or Linux VM.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="creating-resources-in-azure"></a><span data-ttu-id="074c3-167">Creazione di risorse in Azure</span><span class="sxs-lookup"><span data-stu-id="074c3-167">Creating resources in Azure</span></span>

<span data-ttu-id="074c3-168">In genere, per prima cosa si crea un _gruppo di risorse_ per contenere tutto ciò che è necessario creare.</span><span class="sxs-lookup"><span data-stu-id="074c3-168">Normally, the first thing we'd do is to create a _resource group_ to hold all the things that we need to create.</span></span> <span data-ttu-id="074c3-169">In questo modo è possibile gestire le macchine virtuali, i dischi, le interfacce di rete e gli altri elementi che costituiscono la soluzione come un'unità.</span><span class="sxs-lookup"><span data-stu-id="074c3-169">This allows us to administer all the VMs, disks, network interfaces, and other elements that make up our solution as a unit.</span></span> <span data-ttu-id="074c3-170">È possibile usare l'interfaccia della riga di comando di Azure per creare un gruppo di risorse con il comando `az group create`.</span><span class="sxs-lookup"><span data-stu-id="074c3-170">We can use the Azure CLI to create a resource group with the `az group create` command.</span></span> <span data-ttu-id="074c3-171">Sono necessari un parametro `--name` per assegnare un nome univoco al gruppo nella sottoscrizione e un parametro `--location` per indicare ad Azure l'area del mondo in cui si vuole collocare le risorse per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="074c3-171">It takes a `--name` to give it a unique name in our subscription, and a `--location` to tell Azure what area of the world we want the resources to be located by default.</span></span>

<span data-ttu-id="074c3-172">Poiché si sta usando l'ambiente sandbox di Azure gratuito, non è necessario eseguire questo passaggio, ma si userà il gruppo di risorse creato in precedenza, **<rgn>[Nome gruppo di risorse]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="074c3-172">Since we are in the free Azure Sandbox environment, you don't need to do this step, instead, you will use the pre-created resource group **<rgn>[Resource Group Name]</rgn>**.</span></span>

## <a name="choosing-a-location"></a><span data-ttu-id="074c3-173">Scelta di una località</span><span class="sxs-lookup"><span data-stu-id="074c3-173">Choosing a location</span></span>

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="074c3-174">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="074c3-174">::: zone pivot="windows-cloud"</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="074c3-175">Creare una macchina virtuale Windows</span><span class="sxs-lookup"><span data-stu-id="074c3-175">Create a Windows VM</span></span>

<span data-ttu-id="074c3-176">Si vedrà ora come preparare ed eseguire una macchina virtuale Windows.</span><span class="sxs-lookup"><span data-stu-id="074c3-176">Let's get your Windows VM up and running.</span></span>

1. <span data-ttu-id="074c3-177">In Cloud Shell, a destra della pagina, eseguire il comando `az vm create` seguente per creare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-177">In the Cloud Shell on the right side of this page, run the following `az vm create` command to create your VM.</span></span> <span data-ttu-id="074c3-178">Il comando crea la macchina virtuale nella località "Stati Uniti orientali", ma è possibile scegliere una qualsiasi delle località elencate in precedenza ed è consigliabile sceglierne una vicina alla propria posizione.</span><span class="sxs-lookup"><span data-stu-id="074c3-178">The command creates the VM in the "East US" location, you can change that to any of the locations listed above &mdash; we recommend you select one close to you.</span></span>

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --location eastus \
      --admin-username azureuser
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    <span data-ttu-id="074c3-179">Anche se è possibile specificare la password di amministratore di Windows tramite il comando, qui verrà chiesto di immetterne una.</span><span class="sxs-lookup"><span data-stu-id="074c3-179">Although you can specify the Windows admin password through the command, here you'll be prompted to enter one.</span></span> <span data-ttu-id="074c3-180">Scegliere una password contenente almeno 12 caratteri con una combinazione di lettere maiuscole e minuscole, numeri e simboli.</span><span class="sxs-lookup"><span data-stu-id="074c3-180">Choose a password that contains at least 12 characters with a combination of upper and lowercase letters, numbers, and symbols.</span></span> <span data-ttu-id="074c3-181">Non usare una password usata in altre posizioni.</span><span class="sxs-lookup"><span data-stu-id="074c3-181">Don't use a password you use elsewhere.</span></span>

    <span data-ttu-id="074c3-182">Per l'attivazione della macchina virtuale saranno necessari da quattro a cinque minuti.</span><span class="sxs-lookup"><span data-stu-id="074c3-182">Your VM will take four to five minutes to come up.</span></span> <span data-ttu-id="074c3-183">Confrontare questo tempo con quello che sarebbe necessario per acquistare, installare in rack e configurare un sistema nel proprio data center.</span><span class="sxs-lookup"><span data-stu-id="074c3-183">Compare that to the time it takes to purchase, rack, and configure a system in your data center.</span></span> <span data-ttu-id="074c3-184">La differenza è notevole.</span><span class="sxs-lookup"><span data-stu-id="074c3-184">Quite a difference!</span></span>

<span data-ttu-id="074c3-185">Durante l'attesa è possibile esaminare il comando appena eseguito.</span><span class="sxs-lookup"><span data-stu-id="074c3-185">While you're waiting, let's review the command you just ran.</span></span>

* <span data-ttu-id="074c3-186">Il nome della macchina virtuale è **myVM**.</span><span class="sxs-lookup"><span data-stu-id="074c3-186">The VM is named **myVM**.</span></span> <span data-ttu-id="074c3-187">Questo nome identifica la macchina virtuale in Azure.</span><span class="sxs-lookup"><span data-stu-id="074c3-187">This name identifies the VM in Azure.</span></span> <span data-ttu-id="074c3-188">Diventa anche il nome host interno, o nome computer, della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-188">It also becomes the VM's internal hostname, or computer name.</span></span>
* <span data-ttu-id="074c3-189">Il gruppo di risorse, ovvero il contenitore logico della macchina virtuale, è denominato **<rgn>[Nome gruppo di risorse sandbox]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="074c3-189">The resource group, or the VM's logical container, is named **<rgn>[Sandbox resource group name]</rgn>**.</span></span>
* <span data-ttu-id="074c3-190">**Win2016Datacenter** specifica l'immagine di macchina virtuale Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="074c3-190">**Win2016Datacenter** specifies the Windows Server 2016 VM image.</span></span>
* <span data-ttu-id="074c3-191">**Standard_DS2_v2** indica le dimensioni della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-191">**Standard_DS2_v2** refers to the size of the VM.</span></span> <span data-ttu-id="074c3-192">Queste dimensioni includono due CPU virtuali e 7 GB di memoria.</span><span class="sxs-lookup"><span data-stu-id="074c3-192">This size has two virtual CPUs and 7 GB of memory.</span></span>
* <span data-ttu-id="074c3-193">Il nome utente e la password consentono di connettersi alla macchina virtuale in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="074c3-193">The username and password enable you to connect to your VM later.</span></span> <span data-ttu-id="074c3-194">Ad esempio, è possibile connettersi tramite Desktop remoto o Gestione remota Windows per usare e configurare il sistema.</span><span class="sxs-lookup"><span data-stu-id="074c3-194">For example, you can connect over Remote Desktop or WinRM to work with and configure the system.</span></span>

<span data-ttu-id="074c3-195">Per impostazione predefinita, Azure assegna un indirizzo IP pubblico alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-195">By default, Azure assigns a public IP address to your VM.</span></span> <span data-ttu-id="074c3-196">È possibile configurare una macchina virtuale in modo che sia accessibile da Internet o solo dalla rete interna.</span><span class="sxs-lookup"><span data-stu-id="074c3-196">You can configure a VM to be accessible from the Internet or only from the internal network.</span></span>

<span data-ttu-id="074c3-197">È anche possibile guardare questo breve video su alcune delle opzioni disponibili per creare e gestire le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="074c3-197">You can also check out this short video about some of the options you have to create and manage VMs.</span></span>

#### <a name="options-to-create-and-manage-vms"></a><span data-ttu-id="074c3-198">Opzioni per creare e gestire le macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="074c3-198">Options to create and manage VMs</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

<span data-ttu-id="074c3-199">Quando la macchina virtuale è pronta vengono visualizzate informazioni su di essa.</span><span class="sxs-lookup"><span data-stu-id="074c3-199">When the VM is ready, you see information about it.</span></span> <span data-ttu-id="074c3-200">Ecco un esempio.</span><span class="sxs-lookup"><span data-stu-id="074c3-200">Here's an example.</span></span>

```json
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1E-1B-3B",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "104.211.9.245",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

<span data-ttu-id="074c3-201">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="074c3-201">::: zone-end</span></span>

<span data-ttu-id="074c3-202">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="074c3-202">::: zone pivot="linux-cloud"</span></span>

## <a name="create-a-linux-vm"></a><span data-ttu-id="074c3-203">Creare una macchina virtuale Linux</span><span class="sxs-lookup"><span data-stu-id="074c3-203">Create a Linux VM</span></span>

<span data-ttu-id="074c3-204">Si vedrà ora come preparare ed eseguire una macchina virtuale Linux.</span><span class="sxs-lookup"><span data-stu-id="074c3-204">Let's get your Linux VM up and running.</span></span>

1. <span data-ttu-id="074c3-205">In Cloud Shell, a destra della pagina, eseguire il comando `az vm create` per creare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-205">From Cloud Shell on the right side of this page, run the `az vm create` command to create your VM.</span></span> <span data-ttu-id="074c3-206">Il comando seguente crea la macchina virtuale nella località "Stati Uniti orientali", ma è possibile scegliere una qualsiasi delle località elencate in precedenza ed è consigliabile sceglierne una vicina alla propria posizione.</span><span class="sxs-lookup"><span data-stu-id="074c3-206">The following command creates the VM in the "East US" location, you can change that to any of the locations listed above - we recommend you select one close to you.</span></span>

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image UbuntuLTS \
      --location eastus \
      --size Standard_DS2_v2 \
      --generate-ssh-keys
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    <span data-ttu-id="074c3-207">Per l'attivazione della macchina virtuale saranno necessari un paio di minuti circa.</span><span class="sxs-lookup"><span data-stu-id="074c3-207">Your VM will take about two minutes to come up.</span></span> <span data-ttu-id="074c3-208">Confrontare questo tempo con quello che sarebbe necessario per acquistare, installare in rack e configurare un sistema nel proprio data center.</span><span class="sxs-lookup"><span data-stu-id="074c3-208">Compare that to the time it takes to purchase, rack, and configure a system in your data center.</span></span> <span data-ttu-id="074c3-209">La differenza è notevole.</span><span class="sxs-lookup"><span data-stu-id="074c3-209">Quite a difference!</span></span>

<span data-ttu-id="074c3-210">Durante l'attesa è possibile esaminare il comando appena eseguito.</span><span class="sxs-lookup"><span data-stu-id="074c3-210">While you're waiting, let's review the command you just ran.</span></span>

* <span data-ttu-id="074c3-211">Il nome della macchina virtuale è **myVM**.</span><span class="sxs-lookup"><span data-stu-id="074c3-211">The VM is named **myVM**.</span></span> <span data-ttu-id="074c3-212">Questo nome identifica la macchina virtuale in Azure.</span><span class="sxs-lookup"><span data-stu-id="074c3-212">This name identifies the VM in Azure.</span></span> <span data-ttu-id="074c3-213">Diventa anche il nome host interno, o nome computer, della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-213">It also becomes the VM's internal hostname, or computer name.</span></span>
* <span data-ttu-id="074c3-214">Il gruppo di risorse, ovvero il contenitore logico della macchina virtuale, è denominato **<rgn>[Nome gruppo di risorse sandbox]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="074c3-214">The resource group, or the VM's logical container, is named **<rgn>[Sandbox resource group name]</rgn>**.</span></span>
* <span data-ttu-id="074c3-215">**UbuntuLTS** specifica l'immagine di macchina virtuale Ubuntu 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="074c3-215">**UbuntuLTS** specifies the Ubuntu 16.04 LTS VM image.</span></span>
* <span data-ttu-id="074c3-216">**Standard_DS2_v2** indica le dimensioni della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-216">**Standard_DS2_v2** refers to the size of the VM.</span></span> <span data-ttu-id="074c3-217">Queste dimensioni includono due CPU virtuali e 7 GB di memoria.</span><span class="sxs-lookup"><span data-stu-id="074c3-217">This size has two virtual CPUs and 7 GB of memory.</span></span>
* <span data-ttu-id="074c3-218">L'opzione `--generate-ssh-keys` crea una coppia di chiavi SSH per consentire l'accesso alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-218">The `--generate-ssh-keys` option creates an SSH key pair to enable you to log in to the VM.</span></span>

<span data-ttu-id="074c3-219">Per impostazione predefinita, Azure assegna un indirizzo IP pubblico alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="074c3-219">By default, Azure assigns a public IP address to your VM.</span></span> <span data-ttu-id="074c3-220">È possibile configurare una macchina virtuale in modo che sia accessibile da Internet o solo dalla rete interna.</span><span class="sxs-lookup"><span data-stu-id="074c3-220">You can configure a VM to be accessible from the Internet or only from the internal network.</span></span>

<span data-ttu-id="074c3-221">È anche possibile guardare questo breve video su alcune delle opzioni disponibili per creare e gestire le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="074c3-221">You can also check out this short video about some of the options you have to create and manage VMs.</span></span>

#### <a name="options-to-create-and-manage-vms"></a><span data-ttu-id="074c3-222">Opzioni per creare e gestire le macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="074c3-222">Options to create and manage VMs</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

<span data-ttu-id="074c3-223">Quando la macchina virtuale è pronta vengono visualizzate informazioni su di essa.</span><span class="sxs-lookup"><span data-stu-id="074c3-223">When the VM is ready, you see information about it.</span></span> <span data-ttu-id="074c3-224">Ecco un esempio.</span><span class="sxs-lookup"><span data-stu-id="074c3-224">Here's an example.</span></span>

```json
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

<span data-ttu-id="074c3-225">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="074c3-225">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="074c3-226">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="074c3-226">Summary</span></span>

<span data-ttu-id="074c3-227">È sufficiente conoscere alcuni concetti per riuscire ad attivare una macchina virtuale in Azure in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="074c3-227">With just a few concepts under your belt, you're able to spin up a VM on Azure in just a few minutes.</span></span> <span data-ttu-id="074c3-228">Molti di questi concetti, ad esempio le dimensioni della macchina virtuale e le regole del firewall, sono probabilmente già noti.</span><span class="sxs-lookup"><span data-stu-id="074c3-228">Many of these concepts, such as a VM's size and firewall rules, are likely familiar to you already.</span></span>

<span data-ttu-id="074c3-229">Successivamente si vedrà come installare un server Web nella macchina virtuale e configurare il server Web per gestire un sito Web di base.</span><span class="sxs-lookup"><span data-stu-id="074c3-229">Next, you'll install a web server on your VM and configure your web server to serve up a basic web site.</span></span>