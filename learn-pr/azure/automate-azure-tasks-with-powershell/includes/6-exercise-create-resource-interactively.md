<span data-ttu-id="eb5d3-101">Ricordare lo scenario originale: creazione di macchine virtuali per testare il software CRM.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-101">Recall our original scenario - creating VMs to test our CRM software.</span></span> <span data-ttu-id="eb5d3-102">Quando è disponibile una nuova build, si vuole creare rapidamente una nuova macchina virtuale in modo da poter testare l'esperienza di installazione completa da un'immagine pulita.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-102">When a new build is available, we want to spin up a new VM so we can test the full install experience from a clean image.</span></span> <span data-ttu-id="eb5d3-103">Una volta completati i test, la macchina virtuale dovrà essere eliminata.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-103">Then when we are finished, we want to delete the VM.</span></span>

<span data-ttu-id="eb5d3-104">Si proveranno ora i comandi da usare per creare una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-104">Let's try the commands you would use to create a VM.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm-with-azure-powershell"></a><span data-ttu-id="eb5d3-105">Creare una macchina virtuale Linux con Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="eb5d3-105">Create a Linux VM with Azure PowerShell</span></span>

<span data-ttu-id="eb5d3-106">Poiché si sta usando l'ambiente sandbox di Azure, non è necessario creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-106">Since we are using the Azure sandbox, you won't have to create a Resource Group.</span></span> <span data-ttu-id="eb5d3-107">Usare invece il gruppo di risorse **<rgn>[Nome gruppo di risorse sandbox]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-107">Instead, use the Resource Group **<rgn>[sandbox resource group name]</rgn>**.</span></span> <span data-ttu-id="eb5d3-108">Tenere anche presenti le restrizioni per la posizione.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-108">In addition, be aware of the location restrictions.</span></span>

<span data-ttu-id="eb5d3-109">È possibile creare una nuova macchina virtuale di Azure con PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-109">Let's create a new Azure VM with PowerShell.</span></span>

1. <span data-ttu-id="eb5d3-110">Usare il cmdlet `New-AzureRmVm` per creare una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-110">Use the `New-AzureRmVm` cmdlet to create a VM.</span></span>
    - <span data-ttu-id="eb5d3-111">Usare il gruppo di risorse **<rgn>[Nome gruppo di risorse sandbox]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-111">Use the Resource Group **<rgn>[sandbox resource group name]</rgn>**.</span></span>
    - <span data-ttu-id="eb5d3-112">Assegnare un nome alla macchina virtuale: in genere si usa un nome significativo che identifichi gli scopi della macchina virtuale, la posizione e il numero di istanza (se ne esiste più di una).</span><span class="sxs-lookup"><span data-stu-id="eb5d3-112">Give the VM a name - typically you want to use something meaningful that identifies the purposes of the VM, location, and (if there is more than one) instance number.</span></span> <span data-ttu-id="eb5d3-113">Si userà "testvm-eus-01" per "Test VM in East US, instance 1".</span><span class="sxs-lookup"><span data-stu-id="eb5d3-113">We'll use "testvm-eus-01" for "Test VM in East US, instance 1".</span></span> <span data-ttu-id="eb5d3-114">Scegliere un nome appropriato in base a dove è stata posizionata la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-114">Come up with your own name based on where you place the VM.</span></span>
    - <span data-ttu-id="eb5d3-115">Selezionare una località vicina dall'elenco seguente, disponibile nella sandbox di Azure.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-115">Select a location close to you from the following list available in the Azure sandbox.</span></span> <span data-ttu-id="eb5d3-116">Assicurarsi di modificare il valore nel comando di esempio seguente, se si usa Copia e incolla.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-116">Make sure to change the value in the below example command if you are using copy and paste.</span></span>

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - <span data-ttu-id="eb5d3-117">Usare "UbuntuLTS" per l'immagine (Ubuntu Linux).</span><span class="sxs-lookup"><span data-stu-id="eb5d3-117">Use "UbuntuLTS" for the image - this is Ubuntu Linux.</span></span>
    - <span data-ttu-id="eb5d3-118">Usare il cmdlet `Get-Credential` e inviare i risultati nel parametro `Credential`.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-118">Use the `Get-Credential` cmdlet and feed the results into the `Credential` parameter.</span></span>
    - <span data-ttu-id="eb5d3-119">Aggiungere il parametro `-OpenPorts` e passare "22" come porta. In questo modo sarà possibile connettersi con SSH alla macchina.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-119">Add the `-OpenPorts` parameter and pass "22" as the port - this will let us SSH into the machine.</span></span>
 
    ```powershell
    New-AzureRmVm -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -Name "testvm-eus-01" -Credential (Get-Credential) -Location "East US" -Image UbuntuLTS -OpenPorts 22
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
    
1. <span data-ttu-id="eb5d3-120">Questa operazione richiederà qualche minuto.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-120">This will take a few minutes to complete.</span></span> <span data-ttu-id="eb5d3-121">Al termine, è possibile eseguire una query e assegnare l'oggetto macchina virtuale a una variabile (`$vm`).</span><span class="sxs-lookup"><span data-stu-id="eb5d3-121">Once it does, you can query it and assign the VM object to a variable (`$vm`).</span></span>

    ```powershell
    $vm = Get-AzureRmVM -Name "testvm-eus-01" -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```
    
1. <span data-ttu-id="eb5d3-122">Eseguire quindi una query per ottenere il valore per eseguire il dump delle informazioni relative alla macchina virtuale:</span><span class="sxs-lookup"><span data-stu-id="eb5d3-122">Then query the value to dump out the information about the VM:</span></span>

    ```powershell
    $vm
    ```

    <span data-ttu-id="eb5d3-123">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="eb5d3-123">You should see something like:</span></span>

    ```output
    ResourceGroupName : <rgn>[sandbox resource group name]</rgn>
    Id                : /subscriptions/xxxxxxxx-xxxx-aaaa-bbbb-cccccccccccc/resourceGroups/<rgn>[sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/testvm-eus-01
    VmId              : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    Name              : testvm-eus-01
    Type              : Microsoft.Compute/virtualMachines
    Location          : eastus
    Tags              : {}
    HardwareProfile   : {VmSize}
    NetworkProfile    : {NetworkInterfaces}
    OSProfile         : {ComputerName, AdminUsername, LinuxConfiguration, Secrets}
    ProvisioningState : Succeeded
    StorageProfile    : {ImageReference, OsDisk, DataDisks}
    ```
    
1. <span data-ttu-id="eb5d3-124">È possibile accedere a oggetti complessi tramite la sintassi con punto (".").</span><span class="sxs-lookup"><span data-stu-id="eb5d3-124">You can reach into complex objects through a dot (".") syntax.</span></span> <span data-ttu-id="eb5d3-125">Ad esempio, per visualizzare le proprietà nell'oggetto `VMSize` associato alla sezione HardwareProfile è possibile digitare:</span><span class="sxs-lookup"><span data-stu-id="eb5d3-125">For example, to see the properties in the `VMSize` object associated with the HardwareProfile section you can type:</span></span>

    ```powershell
    $vm.HardwareProfile
    ```

1. <span data-ttu-id="eb5d3-126">Oppure, per ottenere informazioni su uno dei dischi:</span><span class="sxs-lookup"><span data-stu-id="eb5d3-126">Or to get information on one of the disks:</span></span>

    ```powershell
    $vm.StorageProfile.OsDisk
    ```

1. <span data-ttu-id="eb5d3-127">È anche possibile passare l'oggetto macchina virtuale in altri cmdlet.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-127">You can even pass the VM object into other cmdlets.</span></span> <span data-ttu-id="eb5d3-128">Ad esempio, questo comando consentirà di recupererà l'indirizzo IP pubblico della macchina virtuale:</span><span class="sxs-lookup"><span data-stu-id="eb5d3-128">For example, this will retrieve the public IP address of your VM:</span></span>

    ```powershell
    $vm | Get-AzureRmPublicIpAddress
    ```

1. <span data-ttu-id="eb5d3-129">Con l'indirizzo IP è possibile connettersi alla macchina virtuale con SSH.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-129">With the IP address, you can connect to the VM with SSH.</span></span> <span data-ttu-id="eb5d3-130">Ad esempio, se è stato usato il nome utente "bob" e l'indirizzo IP è "205.22.16.5", questo comando consente alla macchina Linux di connettersi:</span><span class="sxs-lookup"><span data-stu-id="eb5d3-130">For example, if you used the username "bob", and the IP address is "205.22.16.5", then this command would connect to the Linux machine:</span></span>

    ```powershell
    ssh bob@205.22.16.5
    ```

    <span data-ttu-id="eb5d3-131">Disconnettersi quindi digitando `exit`.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-131">Go ahead and log out by typing `exit`.</span></span>


## <a name="delete-a-vm"></a><span data-ttu-id="eb5d3-132">Eliminare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="eb5d3-132">Delete a VM</span></span>

<span data-ttu-id="eb5d3-133">Per provare alcuni altri comandi, si vedrà come eliminare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-133">Just to try out some more commands, let's delete the VM.</span></span> <span data-ttu-id="eb5d3-134">È prima di tutto necessario arrestarla.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-134">We'll shut it down first.</span></span>

```powershell
Stop-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

<span data-ttu-id="eb5d3-135">Eliminare quindi la macchina virtuale con il cmdlet `Remove-AzureRmVM`:</span><span class="sxs-lookup"><span data-stu-id="eb5d3-135">Now, let's delete the VM with the `Remove-AzureRmVM` cmdlet:</span></span>

```powershell
Remove-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

<span data-ttu-id="eb5d3-136">Provare questo comando per elencare tutte le risorse nel gruppo di risorse:</span><span class="sxs-lookup"><span data-stu-id="eb5d3-136">Try this command to list all the resources in your resource group:</span></span>

```powershell
Get-AzureRmResource -ResourceGroupName $vm.ResourceGroupName | ft
```

<span data-ttu-id="eb5d3-137">Verrà visualizzata una serie di risorse (dischi, reti virtuali e così via) ancora esistenti.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-137">You should see a bunch of resources (disks, virtual networks, etc.) that all still exist.</span></span> 

```output
Microsoft.Compute/disks
Microsoft.Network/networkInterfaces
Microsoft.Network/networkSecurityGroups
Microsoft.Network/publicIPAddresses
Microsoft.Network/virtualNetworks
```

<span data-ttu-id="eb5d3-138">Questo perché il comando `Remove-AzureRmVM` _elimina solo la macchina virtuale_</span><span class="sxs-lookup"><span data-stu-id="eb5d3-138">This is because the `Remove-AzureRmVM` command _just deletes the VM_.</span></span> <span data-ttu-id="eb5d3-139">e non pulisce tutte le altre risorse.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-139">It doesn't cleanup any of the other resources!</span></span> <span data-ttu-id="eb5d3-140">A questo punto, sarebbe probabilmente sufficiente eliminare il gruppo di risorse stesso per fare pulizia.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-140">At this point, we'd likely just delete the Resource Group itself and be done with it.</span></span> <span data-ttu-id="eb5d3-141">Si vedrà invece come eseguire manualmente l'operazione eseguendo l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-141">However, let's just run through the exercise to clean it up manually.</span></span> <span data-ttu-id="eb5d3-142">Si noterà uno schema nei comandi.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-142">You should see a pattern in the commands.</span></span>

1. <span data-ttu-id="eb5d3-143">Eliminare l'interfaccia di rete.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-143">Delete the Network Interface.</span></span>

    ```powershell
    $vm | Remove-AzureRmNetworkInterface –Force
    ```
    
1. <span data-ttu-id="eb5d3-144">Eliminare i dischi del sistema operativo gestiti e l'account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="eb5d3-144">Delete the managed OS disks and storage account</span></span>

    ```powershell
    Get-AzureRmDisk -ResourceGroupName $vm.ResourceGroupName -DiskName $vm.StorageProfile.OSDisk.Name | Remove-AzureRmDisk -Force
    ```

1. <span data-ttu-id="eb5d3-145">Eliminare poi la rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-145">Next, delete the virtual network.</span></span>

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmVirtualNetwork -Force
    ```

1. <span data-ttu-id="eb5d3-146">Eliminare il gruppo di sicurezza di rete.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-146">Delete the network security group.</span></span>

    ```powershell
    Get-AzureRmNetworkSecurityGroup -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmNetworkSecurityGroup -Force
    ```

1. <span data-ttu-id="eb5d3-147">E infine l'indirizzo IP pubblico.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-147">And finally, the public IP address.</span></span>

    ```powershell
    Get-AzureRmPublicIpAddress -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmPublicIpAddress -Force
    ```

<span data-ttu-id="eb5d3-148">In questo modo si dovrebbe essere riusciti a intercettare tutte le risorse create. Per essere sicuri, controllare il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-148">We should have caught all the created resources; check the resource group just to be sure.</span></span> <span data-ttu-id="eb5d3-149">Sono stati eseguiti numerosi comandi manuali, ma un approccio migliore sarebbe scrivere uno _script_ in modo da poter riutilizzare questa logica in un secondo momento per creare o eliminare una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-149">We did a lot of manual commands here but a better approach would have been to write a _script_ so we could reuse this logic later to create or delete a VM.</span></span> <span data-ttu-id="eb5d3-150">Si vedranno ora le tecniche di scripting con PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eb5d3-150">Let's look at scripting with PowerShell.</span></span>