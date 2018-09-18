
<span data-ttu-id="fe6e9-101">Nell'esercizio precedente sono state eseguite le attività seguenti tramite il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-101">In the previous exercise, we performed the following tasks using the Azure portal.</span></span>

- <span data-ttu-id="fe6e9-102">Visualizzare lo stato della cache del disco del sistema operativo</span><span class="sxs-lookup"><span data-stu-id="fe6e9-102">View OS disk cache status</span></span>
- <span data-ttu-id="fe6e9-103">Modificare le impostazioni della cache del disco del sistema operativo</span><span class="sxs-lookup"><span data-stu-id="fe6e9-103">Change the cache settings of the OS disk</span></span>
- <span data-ttu-id="fe6e9-104">Aggiungere un disco dati alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="fe6e9-104">Add a data disk to the VM</span></span>
- <span data-ttu-id="fe6e9-105">Modificare il tipo di memorizzazione nella cache in un nuovo disco dati</span><span class="sxs-lookup"><span data-stu-id="fe6e9-105">Change caching type on a new data disk</span></span>

<span data-ttu-id="fe6e9-106">Ora si farà pratica con queste operazioni in Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-106">Let's practice these operations using Azure PowerShell.</span></span> <span data-ttu-id="fe6e9-107">Verrà usata la macchina virtuale creata nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-107">We're going to use the VM we created in the previous exercise.</span></span> <span data-ttu-id="fe6e9-108">Per le operazioni in questo lab si presuppone che:</span><span class="sxs-lookup"><span data-stu-id="fe6e9-108">The operations in this lab assume:</span></span>

- <span data-ttu-id="fe6e9-109">La macchina virtuale esista già e sia chiamata **fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="fe6e9-109">Our VM exists and is called **fotoshareVM**</span></span>
- <span data-ttu-id="fe6e9-110">La macchina virtuale si trovi in un gruppo di risorse denominato **fotoshare-rg**</span><span class="sxs-lookup"><span data-stu-id="fe6e9-110">Our VM lives in a resource group called **fotoshare-rg**</span></span>

<span data-ttu-id="fe6e9-111">Se i nomi effettivi sono diversi, sostituire questi valori con i propri.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-111">If you've gone with a different set of names, just replace these values with yours.</span></span> 

<span data-ttu-id="fe6e9-112">Di seguito è riportato lo stato corrente dei dischi della macchina virtuale dall'ultimo esercizio.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-112">Here's the current state of our VM disks from the last exercise.</span></span> 

![Screenshot dei dischi dati e del sistema operativo, entrambi impostati per la memorizzazione nella cache di sola lettura.](../media-draft/disks-final-config-portal.PNG)

<span data-ttu-id="fe6e9-114">È stato usato il portale per impostare il campo \***MEMORIZZAZIONE NELLA CACHE DELL'HOST** per i dischi dati e del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-114">We used the portal to set the \***HOST CACHING** field for both the OS and data disks.</span></span> <span data-ttu-id="fe6e9-115">Tenere presente questo stato iniziale mentre si esegue la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-115">Keep this initial state in mind as we work through the following steps.</span></span> 

### <a name="set-up-some-variables"></a><span data-ttu-id="fe6e9-116">Impostare alcune variabili</span><span class="sxs-lookup"><span data-stu-id="fe6e9-116">Set up some variables</span></span>
<span data-ttu-id="fe6e9-117">Per prima cosa, occorre archiviare alcuni nomi di risorse per poterli riusare in seguito.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-117">First, let's  store some resource names so we can use them later.</span></span>

<span data-ttu-id="fe6e9-118">Usare il terminale Azure Cloud Shell a destra per eseguire i comandi di PowerShell seguenti.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-118">Use the Azure Cloud Shell terminal on the right to run the following Powershell commands.</span></span> 

```powershell
$myRgName = "fotoshare-rg"
$myVMName = "fotoshareVM"
```

> [!TIP]
> <span data-ttu-id="fe6e9-119">Sarà necessario reimpostare queste variabili se la sessione di Cloud Shell scade. Pertanto, se possibile, eseguire interamente questo lab in una sola sessione.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-119">You'll have to set these variables again if your Cloud Shell session times out. So, if possible, work through this entire lab in a single session.</span></span> 

### <a name="get-info-about-our-vm"></a><span data-ttu-id="fe6e9-120">Ottenere informazioni sulla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="fe6e9-120">Get info about our VM</span></span>

<span data-ttu-id="fe6e9-121">Eseguire il comando seguente per ottenere le proprietà della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-121">Run the following command to get back the properties of our VM.</span></span>
 
```powershell
$myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVMName
```
<span data-ttu-id="fe6e9-122">La risposta sarà archiviata nella variabile `$myVM`.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-122">We store the response in our `$myVM` variable.</span></span> <span data-ttu-id="fe6e9-123">È possibile eseguire il comando seguente per visualizzare le proprietà specificate.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-123">We can run the following command to just show us the properties we specify here.</span></span>

```powershell
$myVM | select-object -property ResourceGroupName, Name, Type, Location
```

<span data-ttu-id="fe6e9-124">Come illustrato nello screenshot seguente, questa macchina virtuale è effettivamente quella richiesta.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-124">As the following screenshot shows, this VM is indeed the VM we're after.</span></span> <span data-ttu-id="fe6e9-125">Pertanto è possibile cominciare.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-125">So, let's move on.</span></span> 

![Console di PowerShell che mostra i risultati degli ultimi quattro comandi eseguiti.](../media-draft/ps-commands-1.PNG)

### <a name="view-os-disk-cache-status"></a><span data-ttu-id="fe6e9-127">Visualizzare lo stato della cache del disco del sistema operativo</span><span class="sxs-lookup"><span data-stu-id="fe6e9-127">View OS disk cache status</span></span>

<span data-ttu-id="fe6e9-128">È possibile controllare l'impostazione di memorizzazione nella cache tramite l'oggetto `StorageProfile` come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-128">We can check the caching  setting through  the `StorageProfile` object as follows.</span></span>

```powershell
$myVM.StorageProfile.OsDisk.Caching
```
<span data-ttu-id="fe6e9-129">In questo esempio il valore corrente è **Nessuna**.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-129">In this example, the current value is **None**.</span></span> <span data-ttu-id="fe6e9-130">Verrà ripristinato il valore predefinito per un disco del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-130">Let's change it back to the default for an OS disk.</span></span>

![Console di PowerShell che mostra il disco del sistema operativo con un valore di memorizzazione nella cache uguale a "Nessuna".](../media-draft/ps-oscaching-none.PNG)

### <a name="change-the-cache-settings-of-the-os-disk"></a><span data-ttu-id="fe6e9-132">Modificare le impostazioni della cache del disco del sistema operativo</span><span class="sxs-lookup"><span data-stu-id="fe6e9-132">Change the cache settings of the OS disk</span></span>

<span data-ttu-id="fe6e9-133">È possibile impostare il valore per il tipo di cache usando lo stesso oggetto StorageProfile\` come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-133">We can set the value for the cache type using the same StorageProfile\` object as follows.</span></span>
 
```powershell
$myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
```

<span data-ttu-id="fe6e9-134">Questo comando viene eseguito rapidamente, il che significa che esegue un'operazione in locale.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-134">This command runs fast, which should tell you it's doing something locally.</span></span> <span data-ttu-id="fe6e9-135">Il comando modifica semplicemente la proprietà dell'oggetto myVM.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-135">The command just changes the property on the myVM object.</span></span> <span data-ttu-id="fe6e9-136">Come indicato nello screenshot seguente, se si aggiorna la variabile `$myVM`, il valore di memorizzazione nella cache non risulterà modificato nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-136">As the following screenshot shows, if you refresh the `$myVM` variable,  the caching value won't have changed on the VM.</span></span>

![Console di PowerShell che mostra che l'aggiornamento dell'oggetto "myVM" reimposta la memorizzazione nella cache su "nessuna" dal momento che la macchina virtuale non è stata effettivamente aggiornata.](../media-draft/ps-commands-2.PNG)

<span data-ttu-id="fe6e9-138">Per apportare la modifica nella macchina virtuale stessa, chiamare `Update-AzureRmVM` come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-138">To  make the change on the VM itself, call `Update-AzureRmVM` as follows.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

<span data-ttu-id="fe6e9-139">Si noti che questa chiamata richiede alcuni minuti per il completamento.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-139">Notice that this call takes a while to complete.</span></span> <span data-ttu-id="fe6e9-140">Ciò avviene perché è in corso l'aggiornamento della macchina virtuale effettiva e Azure riavvia la macchina virtuale per apportare la modifica.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-140">That's because we're updating the actual VM and Azure restarts the VM  to make the change.</span></span>

![Console di PowerShell che mostra il disco del sistema operativo con un valore di memorizzazione nella cache uguale a "Nessuna".](../media-draft/ps-oscaching-rw.PNG)

<span data-ttu-id="fe6e9-142">Se si aggiorna nuovamente la variabile `$myVM`, si noterà la modifica nell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-142">If you refresh the `$myVM` variable again, you'll see the change on the object.</span></span> <span data-ttu-id="fe6e9-143">Anche esaminando il disco nel portale, la modifica sarà visibile.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-143">Looking at the disk in the portal, you'd also see the change there.</span></span> <span data-ttu-id="fe6e9-144">Si passerà ora alla creazione di un nuovo disco dati.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-144">Ok, let's move on to creating a new data disk.</span></span>  

### <a name="list-data-disk-info"></a><span data-ttu-id="fe6e9-145">Elencare le informazioni del disco dati</span><span class="sxs-lookup"><span data-stu-id="fe6e9-145">List data disk info</span></span>

<span data-ttu-id="fe6e9-146">Per visualizzare i dischi dati disponibili nella macchina virtuale, eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-146">To see what data disks we have on our VM, run the following command.</span></span> 

```powershell
$myVM.StorageProfile.DataDisks
```

<span data-ttu-id="fe6e9-147">Al momento è presente un solo disco dati.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-147">We've only one data disk at the moment.</span></span> <span data-ttu-id="fe6e9-148">Il campo `Lun` è importante.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-148">The `Lun` field is important.</span></span> <span data-ttu-id="fe6e9-149">È il numero di unità logica (**L\*\*\*\*U\*\*\*\*N**) univoco.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-149">It's the unique **L**ogical **U**nit **N**umber.</span></span> <span data-ttu-id="fe6e9-150">Quando si aggiunge un altro disco dati, viene assegnato un valore `Lun` univoco.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-150">When we add another data disk, we'll give it a unique `Lun` value.</span></span> 

### <a name="add-a-new-data-disk-to-our-vm"></a><span data-ttu-id="fe6e9-151">Aggiungere un nuovo disco dati alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="fe6e9-151">Add a new data disk to our VM</span></span> 

<span data-ttu-id="fe6e9-152">Per praticità, verrà archiviato il nuovo nome del disco.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-152">For convenience, we'll store our new disk name.</span></span>

```powershell
$newDiskName = "fotoshareVM-data2"
```

<span data-ttu-id="fe6e9-153">Eseguire il comando `Add-AzureRmVMDataDisk` seguente per definire un nuovo disco.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-153">Run the following `Add-AzureRmVMDataDisk` command to define a new disk.</span></span> 

```powershell
Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
```

<span data-ttu-id="fe6e9-154">A questo disco è stato assegnato un valore LUN uguale a 1, non essendo ancora preso.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-154">We've given this disk a Lun value of 1, since that's not taken.</span></span> <span data-ttu-id="fe6e9-155">È stato definito il disco che si vuole creare, pertanto è il momento di eseguire `Update-AzureRmVM` per apportare la modifica effettiva.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-155">We defined the disk we want to create, so it's time to run `Update-AzureRmVM` to make the actual change.</span></span> 

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

<span data-ttu-id="fe6e9-156">Esaminare nuovamente le informazioni sul disco dati.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-156">Let's look at our data disk info again.</span></span>

```powershell
$myVM.StorageProfile.DataDisks
```

![Console di PowerShell che mostra i due dischi dati.](../media-draft/2-data-disks-part1.png)

<span data-ttu-id="fe6e9-158">Sono ora disponibili due dischi.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-158">We now have two disks.</span></span> <span data-ttu-id="fe6e9-159">Il nuovo disco ha un valore **LUN** pari a 1 e il valore predefinito per **Memorizzazione nella cache** è **Nessuna**.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-159">Our new disk has a **Lun** of 1 and the default value for **Caching** is **None**.</span></span> <span data-ttu-id="fe6e9-160">Questo valore verrà ora modificato.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-160">Let's change that value.</span></span>

### <a name="change-cache-settings-of-new-data-disk"></a><span data-ttu-id="fe6e9-161">Modificare le impostazioni della cache del nuovo disco dati</span><span class="sxs-lookup"><span data-stu-id="fe6e9-161">Change cache settings of new data disk</span></span>

<span data-ttu-id="fe6e9-162">Le proprietà di un disco dati della macchina virtuale possono essere modificate con il cmdlet `Set-AzureRmVMDataDisk`, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-162">We modify properties of a virtual machine data disk with the `Set-AzureRmVMDataDisk` cmdlet as follows.</span></span>

```powershell
Set-AzureRmVMDataDisk -Lun "1" -Caching ReadWrite
```

<span data-ttu-id="fe6e9-163">Come sempre, eseguire il commit delle modifiche con `Update-AzureRmVM`.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-163">As always, commit the changes with `Update-AzureRmVM`.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

<span data-ttu-id="fe6e9-164">Ecco una visualizzazione dal portale delle operazioni che sono state eseguite in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-164">Here's a view from the portal of what we've accomplished in this exercise.</span></span> <span data-ttu-id="fe6e9-165">La macchina virtuale ha ora due dischi dati e sono state modificate tutte le impostazioni di **Memorizzazione nella cache dell'host**.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-165">Our VM now has two data disks and we've adjust all **HOST Caching** settings.</span></span> <span data-ttu-id="fe6e9-166">Tutte queste operazioni sono state eseguite con pochi semplici comandi,</span><span class="sxs-lookup"><span data-stu-id="fe6e9-166">We did all of that with just a few commands.</span></span> <span data-ttu-id="fe6e9-167">direttamente da Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe6e9-167">That's the power of Azure PowerShell.</span></span>

![Console di PowerShell che mostra i due dischi dati.](../media-draft/disks-final-config-portal2.png)
