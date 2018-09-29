<span data-ttu-id="b4923-101">Nell'esercizio precedente sono state eseguite le attività seguenti tramite il portale di Azure:</span><span class="sxs-lookup"><span data-stu-id="b4923-101">In the previous exercise, we performed the following tasks using the Azure portal:</span></span>

- <span data-ttu-id="b4923-102">Visualizzare lo stato della cache del disco del sistema operativo</span><span class="sxs-lookup"><span data-stu-id="b4923-102">View OS disk cache status</span></span>
- <span data-ttu-id="b4923-103">Modificare le impostazioni della cache del disco del sistema operativo</span><span class="sxs-lookup"><span data-stu-id="b4923-103">Change the cache settings of the OS disk</span></span>
- <span data-ttu-id="b4923-104">Aggiungere un disco dati alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="b4923-104">Add a data disk to the VM</span></span>
- <span data-ttu-id="b4923-105">Modificare il tipo di memorizzazione nella cache in un nuovo disco dati</span><span class="sxs-lookup"><span data-stu-id="b4923-105">Change caching type on a new data disk</span></span>

<span data-ttu-id="b4923-106">Ora si farà pratica con queste operazioni in Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b4923-106">Let's practice these operations using Azure PowerShell.</span></span> 

> [!NOTE]
> <span data-ttu-id="b4923-107">Si userà Azure PowerShell, ma è anche possibile usare l'interfaccia della riga di comando di Azure che offre funzionalità simili con uno strumento basato su console.</span><span class="sxs-lookup"><span data-stu-id="b4923-107">We're going to use Azure PowerShell, but you could also use the Azure CLI which provides similar functionality as a console-based tool.</span></span> <span data-ttu-id="b4923-108">È supportata in macOS, Linux e Windows.</span><span class="sxs-lookup"><span data-stu-id="b4923-108">It runs on macOS, Linux, and Windows.</span></span> <span data-ttu-id="b4923-109">Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere il modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure**.</span><span class="sxs-lookup"><span data-stu-id="b4923-109">If you are interested in learning more about the Azure CLI, check out the **Manage Virtual Machines with the Azure CLI** module.</span></span>

<span data-ttu-id="b4923-110">Verrà usata la macchina virtuale creata nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="b4923-110">We're going to use the VM we created in the previous exercise.</span></span> <span data-ttu-id="b4923-111">Per le operazioni in questo lab si presuppone che:</span><span class="sxs-lookup"><span data-stu-id="b4923-111">The operations in this lab assume:</span></span>

- <span data-ttu-id="b4923-112">La macchina virtuale esista già e sia denominata **fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="b4923-112">Our VM exists and is called **fotoshareVM**</span></span>
- <span data-ttu-id="b4923-113">La macchina virtuale sia inserita in un gruppo di risorse denominato **<rgn>[Nome gruppo di risorse sandbox]</rgn>**</span><span class="sxs-lookup"><span data-stu-id="b4923-113">Our VM lives in a resource group called **<rgn>[sandbox resource group name]</rgn>**</span></span>

<span data-ttu-id="b4923-114">Se i nomi effettivi sono diversi, sostituire questi valori con i propri.</span><span class="sxs-lookup"><span data-stu-id="b4923-114">If you've gone with a different set of names, replace these values with yours.</span></span>

<span data-ttu-id="b4923-115">Di seguito è riportato lo stato corrente dei dischi della macchina virtuale dall'ultimo esercizio:</span><span class="sxs-lookup"><span data-stu-id="b4923-115">Here's the current state of our VM disks from the last exercise:</span></span>

![Screenshot dei dischi dati e del sistema operativo, entrambi impostati per la memorizzazione nella cache di sola lettura.](../media/disks-final-config-portal.PNG)

<span data-ttu-id="b4923-117">È stato usato il portale per impostare il campo **MEMORIZZAZIONE NELLA CACHE DELL'HOST** per i dischi dati e del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="b4923-117">We used the portal to set the **HOST CACHING** field for both the OS and data disks.</span></span> <span data-ttu-id="b4923-118">Tenere presente questo stato iniziale mentre si esegue la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="b4923-118">Keep this initial state in mind as we work through the following steps.</span></span>

### <a name="set-up-some-variables"></a><span data-ttu-id="b4923-119">Impostare alcune variabili</span><span class="sxs-lookup"><span data-stu-id="b4923-119">Set up some variables</span></span>

<span data-ttu-id="b4923-120">Per prima cosa, occorre archiviare alcuni nomi di risorse per poterli riusare in seguito.</span><span class="sxs-lookup"><span data-stu-id="b4923-120">First, let's store some resource names so we can use them later.</span></span>

1. <span data-ttu-id="b4923-121">Usare il terminale Azure Cloud Shell a destra per eseguire i comandi di PowerShell seguenti:</span><span class="sxs-lookup"><span data-stu-id="b4923-121">Use the Azure Cloud Shell terminal on the right to run the following PowerShell commands:</span></span>

    > [!NOTE]
    > <span data-ttu-id="b4923-122">Passare la sessione di Cloud Shell su **PowerShell** prima di provare questi comandi, se necessario.</span><span class="sxs-lookup"><span data-stu-id="b4923-122">Switch your Cloud Shell session to **PowerShell** before trying these commands, if it isn't already.</span></span>
    
    ```powershell
    $myRgName = "<rgn>[sandbox resource group name]</rgn>"
    $myVMName = "fotoshareVM"
    ```
    
    > [!TIP]
    > <span data-ttu-id="b4923-123">Sarà necessario reimpostare queste variabili se la sessione di Cloud Shell scade. Pertanto, se possibile, eseguire interamente questo lab in una sola sessione.</span><span class="sxs-lookup"><span data-stu-id="b4923-123">You'll have to set these variables again if your Cloud Shell session times out. So, if possible, work through this entire lab in a single session.</span></span>
    
### <a name="get-info-about-our-vm"></a><span data-ttu-id="b4923-124">Ottenere informazioni sulla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="b4923-124">Get info about our VM</span></span>

1. <span data-ttu-id="b4923-125">Eseguire il comando seguente per ottenere le proprietà della macchina virtuale:</span><span class="sxs-lookup"><span data-stu-id="b4923-125">Run the following command to get back the properties of our VM:</span></span>

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
    ```
    
1. <span data-ttu-id="b4923-126">La risposta verrà archiviata nella variabile `$myVM`.</span><span class="sxs-lookup"><span data-stu-id="b4923-126">We store the response in our `$myVM` variable.</span></span> <span data-ttu-id="b4923-127">È possibile inviare tramite pipe l'output nel cmdlet `select-object` per visualizzare proprietà specifiche:</span><span class="sxs-lookup"><span data-stu-id="b4923-127">We can pipe the output into the `select-object` cmdlet to filter the display to specific properties:</span></span>

    ```powershell
    $myVM | select-object -property ResourceGroupName, Name, Type, Location
    ```
    
1. <span data-ttu-id="b4923-128">Il risultato dovrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="b4923-128">You should get something like the following.</span></span>

    ```output
    ResourceGroupName Name        Type                              Location
    ----------------- ----        ----                              --------
    <rgn>[sandbox resource group name]</rgn> fotoshareVM Microsoft.Compute/virtualMachines eastus
    ```
    
### <a name="view-os-disk-cache-status"></a><span data-ttu-id="b4923-129">Visualizzare lo stato della cache del disco del sistema operativo</span><span class="sxs-lookup"><span data-stu-id="b4923-129">View OS disk cache status</span></span>

1. <span data-ttu-id="b4923-130">È possibile controllare l'impostazione di memorizzazione nella cache tramite l'oggetto `StorageProfile` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b4923-130">We can check the caching  setting through  the `StorageProfile` object, as follows:</span></span>

    ```powershell
    $myVM.StorageProfile.OsDisk.Caching
    ```

    ```output
    ReadOnly
    ```
   
1. <span data-ttu-id="b4923-131">Ripristinare il valore predefinito per un disco del sistema operativo, ovvero _ReadWrite_.</span><span class="sxs-lookup"><span data-stu-id="b4923-131">Let's change it back to the default for an OS disk which is _ReadWrite_.</span></span>

### <a name="change-the-cache-settings-of-the-os-disk"></a><span data-ttu-id="b4923-132">Modificare le impostazioni della cache del disco del sistema operativo</span><span class="sxs-lookup"><span data-stu-id="b4923-132">Change the cache settings of the OS disk</span></span>

1. <span data-ttu-id="b4923-133">È possibile impostare il valore per il tipo di cache usando lo stesso oggetto `StorageProfile` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b4923-133">We can set the value for the cache type using the same `StorageProfile` object, as follows:</span></span>

    ```powershell
    $myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
    ```
    
    <span data-ttu-id="b4923-134">Questo comando viene eseguito rapidamente, il che significa che esegue un'operazione in locale.</span><span class="sxs-lookup"><span data-stu-id="b4923-134">This command runs fast, which should tell you it's doing something locally.</span></span> <span data-ttu-id="b4923-135">Il comando modifica solo la proprietà dell'oggetto `myVM`.</span><span class="sxs-lookup"><span data-stu-id="b4923-135">The command only changes the property on the `myVM` object.</span></span> <span data-ttu-id="b4923-136">Come indicato nello screenshot seguente, se si aggiorna la variabile `$myVM` riassegnandola con il cmdlet `Get-AzureRmVM`, il valore di memorizzazione nella cache non risulterà modificato nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="b4923-136">As the following screenshot shows, if you refresh the `$myVM` variable by reassigning it using the `Get-AzureRmVM` cmdlet, the caching value won't have changed on the VM.</span></span>

1. <span data-ttu-id="b4923-137">Per apportare la modifica nella macchina virtuale stessa, chiamare `Update-AzureRmVM` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b4923-137">To make the change on the VM itself, call `Update-AzureRmVM`, as follows:</span></span>

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
    <span data-ttu-id="b4923-138">Si noti che questa chiamata richiede alcuni minuti per il completamento.</span><span class="sxs-lookup"><span data-stu-id="b4923-138">Notice that this call takes a while to complete.</span></span> <span data-ttu-id="b4923-139">Ciò avviene perché è in corso l'aggiornamento della macchina virtuale effettiva e Azure riavvia la macchina virtuale per apportare la modifica.</span><span class="sxs-lookup"><span data-stu-id="b4923-139">That's because we're updating the actual VM, and Azure restarts the VM  to make the change.</span></span>

    ```output
    RequestId IsSuccessStatusCode StatusCode ReasonPhrase
    --------- ------------------- ---------- ------------
                             True         OK OK
    ```
    
1. <span data-ttu-id="b4923-140">Se si aggiorna nuovamente la variabile `$myVM`, si noterà la modifica nell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="b4923-140">If you refresh the `$myVM` variable again, you'll see the change on the object.</span></span> <span data-ttu-id="b4923-141">Anche esaminando il disco nel portale, la modifica sarà visibile.</span><span class="sxs-lookup"><span data-stu-id="b4923-141">Looking at the disk in the portal, you'd also see the change there.</span></span> 

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
    $myVM.StorageProfile.OsDisk.Caching
    ```
    
    ```output
    ReadWrite
    ```
    
### <a name="list-data-disk-info"></a><span data-ttu-id="b4923-142">Elencare le informazioni del disco dati</span><span class="sxs-lookup"><span data-stu-id="b4923-142">List data disk info</span></span>

1. <span data-ttu-id="b4923-143">Per visualizzare i dischi dati disponibili nella macchina virtuale, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b4923-143">To see what data disks we have on our VM, run the following command:</span></span>

    ```powershell
    $myVM.StorageProfile.DataDisks
    ```
    
    ```output
    Name            : fotosharesVM-data
    DiskSizeGB      : 1023
    Lun             : 0
    Caching         : ReadOnly
    CreateOption    : Attach
    SourceImage     :
    VirtualHardDisk :
    ```
    
<span data-ttu-id="b4923-144">Al momento è presente un solo disco dati.</span><span class="sxs-lookup"><span data-stu-id="b4923-144">We have only one data disk at the moment.</span></span> <span data-ttu-id="b4923-145">Il campo `Lun` è importante.</span><span class="sxs-lookup"><span data-stu-id="b4923-145">The `Lun` field is important.</span></span> <span data-ttu-id="b4923-146">È il numero di unità logica (**L\*\*\*\*U\*\*\*\*N**) univoco.</span><span class="sxs-lookup"><span data-stu-id="b4923-146">It's the unique **L**ogical **U**nit **N**umber.</span></span> <span data-ttu-id="b4923-147">Quando si aggiunge un altro disco dati, viene assegnato un valore `Lun` univoco.</span><span class="sxs-lookup"><span data-stu-id="b4923-147">When we add another data disk, we'll give it a unique `Lun` value.</span></span>

### <a name="add-a-new-data-disk-to-our-vm"></a><span data-ttu-id="b4923-148">Aggiungere un nuovo disco dati alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="b4923-148">Add a new data disk to our VM</span></span>

1. <span data-ttu-id="b4923-149">Per praticità, verrà archiviato il nuovo nome del disco:</span><span class="sxs-lookup"><span data-stu-id="b4923-149">For convenience, we'll store our new disk name:</span></span>

    ```powershell
    $newDiskName = "fotoshareVM-data2"
    ```
    
1. <span data-ttu-id="b4923-150">Eseguire il comando `Add-AzureRmVMDataDisk` seguente per definire un nuovo disco dati da 1 GB:</span><span class="sxs-lookup"><span data-stu-id="b4923-150">Run the following `Add-AzureRmVMDataDisk` command to define a new empty 1 GB data disk:</span></span>

    ```powershell
    Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
    ```
    <span data-ttu-id="b4923-151">Si otterrà una risposta simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="b4923-151">You'll get a response like:</span></span>

    ```output
    ResourceGroupName  : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx
    Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxx-xxxxxxx/resourceGroups/<rgn>[sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/fotoshareVM
    VmId               : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
    Name               : fotoshareVM
    Type               : Microsoft.Compute/virtualMachines
    Location           : eastus
    Tags               : {}
    DiagnosticsProfile : {BootDiagnostics}
    HardwareProfile    : {VmSize}
    NetworkProfile     : {NetworkInterfaces}
    OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
    ProvisioningState  : Succeeded
    StorageProfile     : {ImageReference, OsDisk, DataDisks}
        ```
    
1. We've given this disk a `Lun` value of `1` because it's not taken. We defined the disk we want to create, so it's time to run `Update-AzureRmVM` to make the actual change:

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
1. <span data-ttu-id="b4923-152">Esaminare nuovamente le informazioni sul disco dati:</span><span class="sxs-lookup"><span data-stu-id="b4923-152">Let's look at our data disk info again:</span></span>

    ```powershell
    $myVM.StorageProfile.DataDisks
    ```
    
    ```output
    Name            : fotosharesVM-data
    DiskSizeGB      : 1023
    Lun             : 0
    Caching         : ReadOnly
    CreateOption    : Attach
    SourceImage     :
    VirtualHardDisk :
    
    Name            : fotoshareVM-data2
    DiskSizeGB      : 1
    Lun             : 1
    Caching         : None
    CreateOption    : Empty
    SourceImage     :
    VirtualHardDisk :
    ```

<span data-ttu-id="b4923-153">Sono ora disponibili due dischi.</span><span class="sxs-lookup"><span data-stu-id="b4923-153">We now have two disks.</span></span> <span data-ttu-id="b4923-154">Il valore `Lun` del nuovo disco è `1` e il valore predefinito per `Caching` è `None`.</span><span class="sxs-lookup"><span data-stu-id="b4923-154">Our new disk has a `Lun` of `1` and the default value for `Caching` is `None`.</span></span> <span data-ttu-id="b4923-155">Questo valore verrà ora modificato.</span><span class="sxs-lookup"><span data-stu-id="b4923-155">Let's change that value.</span></span>

### <a name="change-cache-settings-of-new-data-disk"></a><span data-ttu-id="b4923-156">Modificare le impostazioni della cache del nuovo disco dati</span><span class="sxs-lookup"><span data-stu-id="b4923-156">Change cache settings of new data disk</span></span>

1. <span data-ttu-id="b4923-157">Le proprietà di un disco dati della macchina virtuale possono essere modificate con il cmdlet `Set-AzureRmVMDataDisk`, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b4923-157">We modify properties of a virtual machine data disk with the `Set-AzureRmVMDataDisk` cmdlet, as follows:</span></span>

    ```powershell
    Set-AzureRmVMDataDisk -VM $myVM -Lun "1" -Caching ReadWrite
    ```
    
1. <span data-ttu-id="b4923-158">Come sempre, eseguire il commit delle modifiche con `Update-AzureRmVM`:</span><span class="sxs-lookup"><span data-stu-id="b4923-158">As always, commit the changes with `Update-AzureRmVM`:</span></span>

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
<span data-ttu-id="b4923-159">Ecco una visualizzazione dal portale delle operazioni che sono state eseguite in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="b4923-159">Here's a view from the portal of what we've accomplished in this exercise.</span></span> <span data-ttu-id="b4923-160">La macchina virtuale ha ora due dischi dati e sono state modificate tutte le impostazioni di **Memorizzazione nella cache dell'host**.</span><span class="sxs-lookup"><span data-stu-id="b4923-160">Our VM now has two data disks, and we've adjusted all **HOST CACHING** settings.</span></span> <span data-ttu-id="b4923-161">Tutte queste operazioni sono state eseguite con pochi semplici comandi,</span><span class="sxs-lookup"><span data-stu-id="b4923-161">We did all of that with just a few commands.</span></span> <span data-ttu-id="b4923-162">direttamente da Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b4923-162">That's the power of Azure PowerShell.</span></span>

![Screenshot del portale di Azure con la sezione Dischi del pannello della macchina virtuale con due dischi dati.](../media/disks-final-config-portal2.png)
