<span data-ttu-id="19c38-101">Azure archivia le immagini dei dischi rigidi virtuali come BLOB di pagine in un account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="19c38-101">Azure stores your VHD images as page blobs in an Azure Storage account.</span></span> <span data-ttu-id="19c38-102">Con i dischi gestiti, Azure si occupa di gestire la risorsa di archiviazione per conto dell'utente: uno dei motivi migliori per scegliere i dischi gestiti.</span><span class="sxs-lookup"><span data-stu-id="19c38-102">With managed disks, Azure takes care of managing the storage on your behalf - it's one of the best reasons to choose managed disks.</span></span>

<span data-ttu-id="19c38-103">Quando viene creata, la VM sceglie una dimensione per il disco del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="19c38-103">When you create the VM, it chooses a size for the OS disk.</span></span> <span data-ttu-id="19c38-104">La dimensione specifica si basa sull'immagine selezionata.</span><span class="sxs-lookup"><span data-stu-id="19c38-104">The specific size is based on the image you select.</span></span> <span data-ttu-id="19c38-105">In Linux è spesso di circa 30 GB, in Windows di circa 127 GB.</span><span class="sxs-lookup"><span data-stu-id="19c38-105">On Linux, it's often around 30 GB, and on Windows about 127 GB.</span></span>

<span data-ttu-id="19c38-106">È possibile aggiungere dischi dati per offrire spazio di archiviazione aggiuntivo, ma è anche possibile espandere un disco esistente, ad esempio se un'applicazione legacy non può suddividere i dati in unità o si esegue la migrazione dell'unità di un computer fisico ad Azure ed è necessaria un'unità del sistema operativo di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="19c38-106">You can add data disks to provide for additional storage space, but you may also wish to expand an existing disk - perhaps a legacy application cannot split its data across drives, or you are migrating a physical PC's drive to Azure and need a larger OS drive.</span></span>

> [!NOTE]
> <span data-ttu-id="19c38-107">È possibile ridimensionare un disco solo per _aumentarne_ le dimensioni.</span><span class="sxs-lookup"><span data-stu-id="19c38-107">You can only resize a disk to a _larger_ size.</span></span> <span data-ttu-id="19c38-108">La riduzione dei dischi gestiti non è attualmente supportata.</span><span class="sxs-lookup"><span data-stu-id="19c38-108">Shrinking managed disks is not supported today.</span></span>

<span data-ttu-id="19c38-109">La modifica delle dimensioni del disco può anche cambiare il livello del disco (ad esempio, da P10 a P20).</span><span class="sxs-lookup"><span data-stu-id="19c38-109">Changing the size of the disk can also change the level of the disk (for example from P10 to P20).</span></span> <span data-ttu-id="19c38-110">Questa condizione può risultare utile per gli aggiornamenti delle prestazioni, ma comporta costi più elevati passando ai livelli premium.</span><span class="sxs-lookup"><span data-stu-id="19c38-110">Keep this in mind - this can be beneficial for performance upgrades, but will also cost more as you move up the premium tiers.</span></span>

## <a name="vm-size-vs-disk-size"></a><span data-ttu-id="19c38-111">Dimensioni della VM e dimensioni del disco</span><span class="sxs-lookup"><span data-stu-id="19c38-111">VM size vs. Disk size</span></span>

<span data-ttu-id="19c38-112">La dimensione di VM scelta in fase di creazione determina il numero di risorse che può allocare.</span><span class="sxs-lookup"><span data-stu-id="19c38-112">The VM size you choose when you create your VM will determine how many resources it can allocate.</span></span> <span data-ttu-id="19c38-113">Per l'archiviazione, le dimensioni consentono di controllare il numero di dischi che è possibile aggiungere alla VM e le dimensioni massime di ogni disco.</span><span class="sxs-lookup"><span data-stu-id="19c38-113">For storage, the size will control the number of disks you can add to the VM and the max size of each disk.</span></span> 

<span data-ttu-id="19c38-114">Come indicato nell'unità precedente, alcune dimensioni di VM supportano solo unità di archiviazione Standard, con limiti per le prestazioni I/O.</span><span class="sxs-lookup"><span data-stu-id="19c38-114">As mentioned in the previous unit, some VM sizes only support Standard storage drives - limiting the I/O performance.</span></span>

<span data-ttu-id="19c38-115">Se si ritiene di aver bisogno di più spazio di archiviazione rispetto a ciò che consentono le dimensioni della VM, è possibile modificare queste ultime.</span><span class="sxs-lookup"><span data-stu-id="19c38-115">If you find that you need more storage than what your VM size allows for, you can change the VM size.</span></span> <span data-ttu-id="19c38-116">Questo argomento è illustrato nel modulo **Introduzione alle macchine virtuali di Azure**.</span><span class="sxs-lookup"><span data-stu-id="19c38-116">We cover that topic in the **Introduction to Azure Virtual Machines** module.</span></span>

## <a name="expanding-a-disk-using-the-azure-cli"></a><span data-ttu-id="19c38-117">Espansione di un disco con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="19c38-117">Expanding a disk using the Azure CLI</span></span>

> [!WARNING]
> <span data-ttu-id="19c38-118">Assicurarsi sempre di eseguire il backup dei dati prima di eseguire operazioni di ridimensionamento dei dischi.</span><span class="sxs-lookup"><span data-stu-id="19c38-118">Always make sure that you back up your data before performing disk resize operations!</span></span>

<span data-ttu-id="19c38-119">Non è possibile eseguire operazioni sui dischi rigidi virtuali quando la macchina virtuale è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="19c38-119">Operations on VHDs cannot be performed with the VM running.</span></span> <span data-ttu-id="19c38-120">Il primo passaggio è quello di arrestare e deallocare la VM con `az vm deallocate` fornendo il nome della VM e del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="19c38-120">The first step is to stop and deallocate the VM with `az vm deallocate` supplying the VM name and resource group name.</span></span>

<span data-ttu-id="19c38-121">La deallocazione di una VM, a differenza del suo semplice _arresto_, rilascia le risorse di calcolo associate e consente ad Azure di apportare modifiche di configurazione all'hardware virtualizzato.</span><span class="sxs-lookup"><span data-stu-id="19c38-121">Deallocating a VM, unlike just _stopping_ a VM releases the associated computing resources and allows Azure to make configuration changes to the virtualized hardware.</span></span>

```azurecli
az vm deallocate --resource-group <resource-group-name> --name <vm-name>
```

<span data-ttu-id="19c38-122">Successivamente, per ridimensionare un disco, usare `az disk update`, passando il nome del disco, il nome del gruppo di risorse e le nuove dimensioni richieste.</span><span class="sxs-lookup"><span data-stu-id="19c38-122">Next, to resize a disk, you use `az disk update`, passing the disk name, resource group name, and newly requested size.</span></span> <span data-ttu-id="19c38-123">Quando si espande un disco gestito, si esegue il mapping delle dimensioni specificate alle dimensioni del disco gestito più vicino.</span><span class="sxs-lookup"><span data-stu-id="19c38-123">When you expand a managed disk, the specified size is mapped to the nearest managed disk size.</span></span>

```azurecli
az disk update \
    --resource-group <resource-group-name> \
    --name <disk-name> \
    --size-gb 200
```

<span data-ttu-id="19c38-124">Infine, si avvia nuovamente la VM con `az vm start`:</span><span class="sxs-lookup"><span data-stu-id="19c38-124">Finally, you start the VM again with `az vm start`:</span></span>

```azurecli
az vm start --resource-group <resource-group-name> --name <vm-name>
```

## <a name="expanding-a-disk-using-the-azure-portal"></a><span data-ttu-id="19c38-125">Espansione di un disco con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="19c38-125">Expanding a disk using the Azure portal</span></span>

<span data-ttu-id="19c38-126">L'espansione di un disco con il portale di Azure è ancora più semplice.</span><span class="sxs-lookup"><span data-stu-id="19c38-126">Expanding a disk using the Azure portal is even easier.</span></span>

1. <span data-ttu-id="19c38-127">Arrestare la VM usando il pulsante **Arresta** sulla barra degli strumenti nella vista **Panoramica** della VM.</span><span class="sxs-lookup"><span data-stu-id="19c38-127">Stop the VM using the **Stop** button in the toolbar on the **Overview** view of the VM.</span></span>

1. <span data-ttu-id="19c38-128">Fare clic su **Dischi** nella sezione **Impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="19c38-128">Click **Disks** in the **Settings** section.</span></span>

1. <span data-ttu-id="19c38-129">Selezionare il disco dati che si intende ridimensionare.</span><span class="sxs-lookup"><span data-stu-id="19c38-129">Select the data disk you want to resize.</span></span>

    ![Screenshot che mostra la sezione dei dischi di una VM con evidenziato il disco rigido virtuale da modificare](../media/5-portal-disks.png)

1. <span data-ttu-id="19c38-131">Nei dettagli del disco digitare una dimensione _maggiore_ rispetto alla dimensione corrente.</span><span class="sxs-lookup"><span data-stu-id="19c38-131">In the disk details, type a size _larger_ than the current size.</span></span> <span data-ttu-id="19c38-132">Qui è anche possibile passare dalla versione Premium alla versione Standard (o viceversa).</span><span class="sxs-lookup"><span data-stu-id="19c38-132">You can also change from Premium to Standard (or vice-versa) here.</span></span> <span data-ttu-id="19c38-133">Queste impostazioni regolano le prestazioni in base a quanto riportato nella sezione delle operazioni di I/O al secondo stimate.</span><span class="sxs-lookup"><span data-stu-id="19c38-133">These settings will adjust your performance as shown in the predicted IOPS section.</span></span>

    ![Screenshot che mostra la schermata di modifica del disco rigido virtuale con evidenziato il campo della nuova dimensione](../media/5-resize-disk.png)

1. <span data-ttu-id="19c38-135">Fare clic su **Salva** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="19c38-135">Click **Save** to save the changes.</span></span>

1. <span data-ttu-id="19c38-136">Riavviare la VM.</span><span class="sxs-lookup"><span data-stu-id="19c38-136">Restart the VM.</span></span>


### <a name="expanding-the-partition"></a><span data-ttu-id="19c38-137">Espansione della partizione</span><span class="sxs-lookup"><span data-stu-id="19c38-137">Expanding the partition</span></span>

<span data-ttu-id="19c38-138">Esattamente come per l'aggiunta di un nuovo disco dati, un disco espanso non aggiunge spazio utilizzabile fino a quando non si espandono la partizione e il file system.</span><span class="sxs-lookup"><span data-stu-id="19c38-138">Just like adding a new data disk, an expanded disk won't add any usable space until you expand the partition and filesystem.</span></span> <span data-ttu-id="19c38-139">Questa operazione deve essere eseguita mediante gli strumenti del sistema operativo disponibili per la VM.</span><span class="sxs-lookup"><span data-stu-id="19c38-139">This must be done using the OS tools available to the VM.</span></span> 

<span data-ttu-id="19c38-140">In Windows usiamo lo strumento di gestione dischi o lo strumento della riga di comando `diskpart`.</span><span class="sxs-lookup"><span data-stu-id="19c38-140">On Windows, we would use the Disk Manager tool or the `diskpart` command line tool.</span></span>

<span data-ttu-id="19c38-141">In Linux usiamo `parted` e `resize2fs`.</span><span class="sxs-lookup"><span data-stu-id="19c38-141">On Linux, you will use `parted` and `resize2fs`.</span></span>

<span data-ttu-id="19c38-142">Proviamo alcuni di questi comandi.</span><span class="sxs-lookup"><span data-stu-id="19c38-142">Let's try out some of these commands.</span></span>