<span data-ttu-id="c4ea7-101">È stato sottovaluto il fatto che alcuni caricamenti possono avere dimensioni eccessive per i dischi di caricamento in possesso.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-101">We underestimated how big some of the uploads would be and our upload disk is running out of space.</span></span> <span data-ttu-id="c4ea7-102">Si decide quindi di raddoppiare lo spazio da 64 a 128 GB.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-102">You decide to double the space and upgrade it from 64 GB to 128 GB.</span></span>

## <a name="resize-the-data-disk"></a><span data-ttu-id="c4ea7-103">Ridimensionare il disco dati</span><span class="sxs-lookup"><span data-stu-id="c4ea7-103">Resize the data disk</span></span>

<span data-ttu-id="c4ea7-104">Per ridimensionare un disco, è necessario l'ID o il nome.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-104">To resize a disk, we need the ID or name of the disk.</span></span> <span data-ttu-id="c4ea7-105">In questo caso si conosce già il nome (uploadDataDisk1).</span><span class="sxs-lookup"><span data-stu-id="c4ea7-105">In this case we already know the name (uploadDataDisk1).</span></span> <span data-ttu-id="c4ea7-106">Ma nel caso in cui non lo si ricordasse o se è stato creato da qualcun altro, è possibile ottenere un elenco di dischi tramite il comando `az disk list`.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-106">But in case we didn't remember that, or it was created by someone else we can get a list of disks using the `az disk list` command.</span></span>

1. <span data-ttu-id="c4ea7-107">Per iniziare, ottenere un elenco dei dischi gestiti nel gruppo di risorse. Se si hanno più VM in un unico gruppo di risorse, l'elenco può includere altri dischi.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-107">Start by getting a list of the managed disks in the resource group; this might include other disks if you have multiple VMs in a single resource group.</span></span> <span data-ttu-id="c4ea7-108">Per questo esempio, dovrà essere presente solo il server Web.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-108">For our example, there should just be our web server.</span></span>

    ```azurecli
    az disk list \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

1. <span data-ttu-id="c4ea7-109">Successivamente, arrestare e deallocare la VM usando `az vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-109">Next, stop and deallocate the VM using `az vm deallocate`.</span></span> 

    ```azurecli
    az vm deallocate --name support-web-vm01
    ```
1. <span data-ttu-id="c4ea7-110">A questo punto è possibile ridimensionare il disco con il comando `az disk update`.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-110">Now we can resize the disk with the `az disk update` command.</span></span>

    ```azurecli
    az disk update --name uploadDataDisk1 --size-gb 128
    ```
    
1. <span data-ttu-id="c4ea7-111">Dopo aver completato l'operazione di ridimensionamento, riavviare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-111">Once the resize operation has completed, restart the VM.</span></span>

    ```azurecli
    az vm start --name support-web-vm01
    ```

## <a name="expand-the-disk-partition"></a><span data-ttu-id="c4ea7-112">Espandere la partizione del disco</span><span class="sxs-lookup"><span data-stu-id="c4ea7-112">Expand the disk partition</span></span>

<span data-ttu-id="c4ea7-113">Il passaggio finale consiste nell'indicare al sistema operativo lo spazio disponibile.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-113">The final step is to tell the OS about the available space.</span></span> <span data-ttu-id="c4ea7-114">Come per i passaggi di partizionamento e formattazione svolti in precedenza, questo processo è identico per le espansioni dei dischi locali.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-114">Just like the partitioning and format steps we did earlier, this process is identical for on-premise disk expansions.</span></span> 

1. <span data-ttu-id="c4ea7-115">In primo luogo, ottenere l'indirizzo IP pubblico della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-115">First, get the public IP address of your VM.</span></span> <span data-ttu-id="c4ea7-116">Dal momento che è stata riavviata, è probabile che sia cambiato.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-116">Since it was rebooted, it has likely changed.</span></span> <span data-ttu-id="c4ea7-117">È possibile provare ora un approccio diverso e usare `az vm show` con un filtro per restituire l'indirizzo IP pubblico.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-117">Let's try a different approach this time and use `az vm show` with a filter to return the public IP address.</span></span>

    > [!TIP]
    > <span data-ttu-id="c4ea7-118">Gli indirizzi IP sono dinamici per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-118">IP addresses are dynamic by default.</span></span> <span data-ttu-id="c4ea7-119">DNS di Azure compenserà automaticamente il cambio di indirizzo IP oppure è possibile modificare il comportamento usando indirizzi IP statici.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-119">Azure DNS will automatically compensate for the IP change, or you can alter the behavior by using static IP addresses.</span></span>

    ```azurecli
    az vm show --name support-web-vm01 -d --query [publicIps] --o tsv
    ```
    
1. <span data-ttu-id="c4ea7-120">Connettersi tramite SSH nel computer Linux.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-120">SSH into the Linux machine.</span></span> <span data-ttu-id="c4ea7-121">È necessario fornire l'indirizzo IP corretto.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-121">You will need to supply your correct IP address.</span></span>

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. <span data-ttu-id="c4ea7-122">Smontare il disco.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-122">Unmount the disk.</span></span> <span data-ttu-id="c4ea7-123">Ricordarsi che si tratta di `/dev/sdc1`.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-123">Recall that it was `/dev/sdc1`.</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

1. <span data-ttu-id="c4ea7-124">Avviare `parted` in una shell con privilegi elevati</span><span class="sxs-lookup"><span data-stu-id="c4ea7-124">Launch `parted` in an elevated shell</span></span>

    ```bash
    sudo parted /dev/sdc
    ```
    
1. <span data-ttu-id="c4ea7-125">Espandere la partizione con il comando `resizepart`.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-125">Expand the partition with the `resizepart` command.</span></span> <span data-ttu-id="c4ea7-126">Immettere la partizione 1 e le nuove dimensioni (128 GB).</span><span class="sxs-lookup"><span data-stu-id="c4ea7-126">Enter partition 1 and the new size (128GB).</span></span>

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [64GB]? 128GB
    ```

    > [!WARNING]
    > <span data-ttu-id="c4ea7-127">Prestare attenzione alle dimensioni.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-127">Be careful about the size.</span></span> <span data-ttu-id="c4ea7-128">Il ridimensionamento della partizione consente anche di ridurla, con la possibilità di una perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-128">Resizing the partition allows you to shrink a partition too, and that will likely result in data loss.</span></span>
    
1. <span data-ttu-id="c4ea7-129">Chiudere lo strumento digitando `quit`.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-129">Exit the tool by typing `quit`.</span></span>

1. <span data-ttu-id="c4ea7-130">Lo strumento di partizionamento _rimonterà_ automaticamente l'unità.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-130">The partition tool will automatically _remount_ the drive.</span></span> <span data-ttu-id="c4ea7-131">Smontarla nuovamente in modo da poterla formattare.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-131">So unmount it again so we can format it.</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```
    
1. <span data-ttu-id="c4ea7-132">Verificare la coerenza tra le partizioni con `e2fsck`.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-132">Verify the partition consistency with `e2fsck`.</span></span> <span data-ttu-id="c4ea7-133">Questo passaggio è assolutamente necessario, ma è anche consigliabile ogni volta che si intende modificare le dimensioni in un volume del disco.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-133">This step is absolutely necessary but is a good idea any time you are changing sizes on a disk volume.</span></span>

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. <span data-ttu-id="c4ea7-134">Ridimensionare il file system con `resize2fs`.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-134">Resize the filesystem with `resize2fs`.</span></span>

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. <span data-ttu-id="c4ea7-135">Infine, rimontare l'unità sul punto di montaggio.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-135">Finally, mount the drive back to the mount point.</span></span>

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```

<span data-ttu-id="c4ea7-136">Per verificare che il disco del sistema operativo sia stato ridimensionato, usare `df -h`.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-136">To verify the OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="c4ea7-137">Dovrebbe ora indicare che l'unità è di 128 GB.</span><span class="sxs-lookup"><span data-stu-id="c4ea7-137">It should now show that the drive is 128 GB.</span></span>
