<span data-ttu-id="13b34-101">Lo studio legale presso cui si lavora sta assistendo a un aumento dei casi gestiti e si riceve la richiesta di attivare un nuovo server Web Linux per archiviare documenti critici da svariate origini: clienti, altri studi legali e forze dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="13b34-101">Our law firm is expanding it's case load and you have been tasked with putting up a new Linux web server to store critical documents from a variety of sources - clients, other law firms, and law enforcement offices.</span></span> <span data-ttu-id="13b34-102">Il server può accettare caricamenti, che devono essere archiviati su disco.</span><span class="sxs-lookup"><span data-stu-id="13b34-102">Our server can accept uploads and needs to store them on disk.</span></span>

> [!TIP]
> <span data-ttu-id="13b34-103">Questo esercizio usa Linux come esempio, ma il processo di base per la creazione di macchine virtuali e l'aggiunta di dischi è lo stesso per Windows.</span><span class="sxs-lookup"><span data-stu-id="13b34-103">This exercise uses Linux as the example, but the basic process of creating VMs and adding disks is the same for Windows.</span></span> <span data-ttu-id="13b34-104">La differenza principale riguarda il partizionamento e la formattazione del disco, che verrebbero eseguiti in una sessione di Desktop remoto tramite gli strumenti predefiniti di Gestione disco.</span><span class="sxs-lookup"><span data-stu-id="13b34-104">The primary difference would be in partitioning and formatting the disk - that would be done in a Remote Desktop session using the built-in Disk Management tools.</span></span>

<span data-ttu-id="13b34-105">L'obiettivo dell'esercizio è creare una macchina virtuale Linux e collegare un nuovo disco rigido virtuale (VHD) denominato "uploadDataDisk1" per archiviare la directory "uploads".</span><span class="sxs-lookup"><span data-stu-id="13b34-105">The goal of the exercise is to create a Linux virtual machine (VM) and attach a new virtual hard disk (VHD) named "uploadDataDisk1" to store the "uploads" directory.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm"></a><span data-ttu-id="13b34-106">Creare una macchina virtuale Linux</span><span class="sxs-lookup"><span data-stu-id="13b34-106">Create a Linux VM</span></span>

<span data-ttu-id="13b34-107">È possibile creare una macchina virtuale Linux per ospitare il server Web tramite l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="13b34-107">Let's create a Linux VM to host our web server using the Azure CLI.</span></span>

1. <span data-ttu-id="13b34-108">Iniziare impostando alcune impostazioni predefinite per questa sessione.</span><span class="sxs-lookup"><span data-stu-id="13b34-108">Start by setting some defaults for this session.</span></span> <span data-ttu-id="13b34-109">Per prima cosa è necessario decidere la _località_ in cui posizionare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="13b34-109">The first thing you need to decide is the _location_ to place the VM.</span></span> <span data-ttu-id="13b34-110">L'ideale sarebbe una località nelle vicinanze dei clienti.</span><span class="sxs-lookup"><span data-stu-id="13b34-110">Ideally this would be close to your clients.</span></span> <span data-ttu-id="13b34-111">In questo caso, selezionare l'area più vicina tra le località disponibili per l'ambiente sandbox di Azure.</span><span class="sxs-lookup"><span data-stu-id="13b34-111">In this case, select the closest region from the locations available to the Azure sandbox.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="13b34-112">Usare il comando `az configure` per impostare la località predefinita da usare.</span><span class="sxs-lookup"><span data-stu-id="13b34-112">Use the `az configure` command to set the default location you want to use.</span></span> <span data-ttu-id="13b34-113">Sostituire "eastus" con tale località.</span><span class="sxs-lookup"><span data-stu-id="13b34-113">Replace "eastus" with the location.</span></span>

    ```azurecli
    az configure --defaults location=eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. <span data-ttu-id="13b34-114">Impostare il valore del gruppo di risorse predefinito sul gruppo di risorse preconfigurato creato per l'ambiente sandbox di Azure: **<rgn>[gruppo di risorse sandbox]</rgn>**</span><span class="sxs-lookup"><span data-stu-id="13b34-114">Set the default resource group value to the pre-configured resource group created for the Azure sandbox: **<rgn>[sandbox resource group]</rgn>**</span></span>

    ```azurecli
    az configure --defaults group="<rgn>[sandbox Resource Group]</rgn>"
    ```

1. <span data-ttu-id="13b34-115">Usare poi il comando `vm create` per creare una nuova macchina virtuale Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="13b34-115">Next, use the `vm create` command to create a new Ubuntu Linux VM.</span></span>
    - <span data-ttu-id="13b34-116">Assegnare il nome "support-web-vm01".</span><span class="sxs-lookup"><span data-stu-id="13b34-116">Name it "support-web-vm01".</span></span>
    - <span data-ttu-id="13b34-117">Impostare le **dimensioni** su _Standard_DS2_v2_.</span><span class="sxs-lookup"><span data-stu-id="13b34-117">Set the **size** to _Standard_DS2_v2_.</span></span>
    - <span data-ttu-id="13b34-118">Impostare il **nome utente amministratore** su "azureuser" (o il nome preferito).</span><span class="sxs-lookup"><span data-stu-id="13b34-118">Set the **admin-username** to "azureuser" (or whatever you prefer).</span></span>
    - <span data-ttu-id="13b34-119">Generare le chiavi SSH con il parametro `--generate-ssh-keys`.</span><span class="sxs-lookup"><span data-stu-id="13b34-119">Generate SSH keys with the `--generate-ssh-keys` parameter.</span></span>

    ```azurecli
    az vm create --name support-web-vm01 \
        --image UbuntuLTS \
        --size Standard_DS2_v2 \
        --admin-username azureuser \
        --generate-ssh-keys
    ```
    
    > [!TIP]
    > <span data-ttu-id="13b34-120">Per la creazione della macchina virtuale e la sua distribuzione in Azure possono essere necessari alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="13b34-120">Creating your VM and deploying it in Azure can take a few minutes.</span></span> <span data-ttu-id="13b34-121">È possibile monitorare lo stato di avanzamento in Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="13b34-121">You can watch the progress in the Cloud Shell.</span></span>
    
1. <span data-ttu-id="13b34-122">Verrà generato un blocco JSON con i dettagli sulla macchina virtuale creata.</span><span class="sxs-lookup"><span data-stu-id="13b34-122">This will result in a JSON block with the details about the created VM.</span></span>

    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/xxx/resourceGroups/<rgn>[sandbox resource group]</rgn>/providers/Microsoft.Compute/virtualMachines/support-web-vm01",
        "location": "eastus",
        "macAddress": "00-0D-3A-18-DE-B4",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "40.76.193.249",
        "resourceGroup": "<rgn>[sandbox resource group]</rgn>",
        "zones": ""
    }
    ```
    <span data-ttu-id="13b34-123">Prendere nota di **publicIpAddress** nell'output.</span><span class="sxs-lookup"><span data-stu-id="13b34-123">Note of the **publicIpAddress** in your output.</span></span> <span data-ttu-id="13b34-124">Questo è l'indirizzo che potrà essere usato per accedere alla macchina virtuale in modalità remota.</span><span class="sxs-lookup"><span data-stu-id="13b34-124">This is how we can access the VM remotely.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13b34-125">Questa macchina virtuale viene usata per imparare a gestire i dischi.</span><span class="sxs-lookup"><span data-stu-id="13b34-125">We are using this VM to learn how to manage disks.</span></span> <span data-ttu-id="13b34-126">Se davvero venisse usata come server Web, sarebbe necessario aprire porte aggiuntive con il comando `az vm open-port` installare software per server Web.</span><span class="sxs-lookup"><span data-stu-id="13b34-126">If we were truly going to use it as a web server, we'd want to open up additional ports with the `az vm open-port` command, and install web server software.</span></span> <span data-ttu-id="13b34-127">Queste attività esulano dagli scopi di questo modulo, ma è possibile eseguire il modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure** per imparare a eseguire questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="13b34-127">These tasks are outside the scope of this module, but you can go through the **Manage virtual machines with the Azure CLI** module to learn how to do these steps.</span></span> 

## <a name="add-an-empty-data-disk-to-our-vm"></a><span data-ttu-id="13b34-128">Aggiungere un disco dati vuoto alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="13b34-128">Add an empty data disk to our VM</span></span>

<span data-ttu-id="13b34-129">Il disco che archivierà la directory "upload" per il server Web verrà denominato "uploadDataDisk1".</span><span class="sxs-lookup"><span data-stu-id="13b34-129">We're going to name the disk that stores the "uploads" directory for your web server "uploadDataDisk1".</span></span> 

> [!TIP]
> <span data-ttu-id="13b34-130">È possibile aggiungere dischi dati quando si crea la macchina virtuale usando il parametro `--data-disk-sizes-gb` per il comando `vm create`.</span><span class="sxs-lookup"><span data-stu-id="13b34-130">You can add data disks when the VM is created using the `--data-disk-sizes-gb` parameter on the `vm create` command.</span></span>

1. <span data-ttu-id="13b34-131">Aggiungere un nuovo disco vuoto al server con il comando `vm disk attach`.</span><span class="sxs-lookup"><span data-stu-id="13b34-131">Add a new empty disk to the server with the `vm disk attach` command.</span></span>
    - <span data-ttu-id="13b34-132">Denominarlo "uploadDataDisk1"</span><span class="sxs-lookup"><span data-stu-id="13b34-132">Name it "uploadDataDisk1"</span></span>
    - <span data-ttu-id="13b34-133">Impostarlo per 64 GB.</span><span class="sxs-lookup"><span data-stu-id="13b34-133">Set it to be 64 GB.</span></span>
    - <span data-ttu-id="13b34-134">Impostare lo **SKU** su "_Premium_LRS_" in modo da poter usare l'archiviazione Premium con ridondanza locale.</span><span class="sxs-lookup"><span data-stu-id="13b34-134">Set the **sku** to "_Premium_LRS_" so we use premium storage with local redundancy.</span></span>

    ```azurecli
    az vm disk attach \
        --vm-name support-web-vm01 \
        --disk uploadDataDisk1 \
        --size-gb 64 \
        --sku Premium_LRS \
        --new
    ```
    
<span data-ttu-id="13b34-135">È stato definito un disco denominato "uploadDataDisk1".</span><span class="sxs-lookup"><span data-stu-id="13b34-135">We've now defined a disk named "uploadDataDisk1" .</span></span> <span data-ttu-id="13b34-136">Per usare il disco, è necessario partizionarlo e formattarlo.</span><span class="sxs-lookup"><span data-stu-id="13b34-136">To use the disk, we'll need to partition and format it.</span></span>

## <a name="initialize-data-disks"></a><span data-ttu-id="13b34-137">Inizializzare i dischi dati</span><span class="sxs-lookup"><span data-stu-id="13b34-137">Initialize data disks</span></span>

<span data-ttu-id="13b34-138">Tutte le unità aggiuntive create da zero dovranno essere inizializzate e formattate.</span><span class="sxs-lookup"><span data-stu-id="13b34-138">Any additional drives you create from scratch will need to be initialized and formatted.</span></span> <span data-ttu-id="13b34-139">Il processo per eseguire questa operazione è identico a quello valido per un disco fisico:</span><span class="sxs-lookup"><span data-stu-id="13b34-139">The process for doing this is identical to a physical disk:</span></span>

1. <span data-ttu-id="13b34-140">Prima di tutto, connettersi tramite SSH al server Linux usando l'indirizzo IP pubblico ottenuto dalla risposta di creazione della macchina virtuale originale.</span><span class="sxs-lookup"><span data-stu-id="13b34-140">First, SSH into the Linux server using the public IP address you got back from the original VM creation response.</span></span> <span data-ttu-id="13b34-141">Se non è disponibile, è possibile usare il comando seguente per ottenere l'indirizzo IP:</span><span class="sxs-lookup"><span data-stu-id="13b34-141">If you don't have that, you can use the following command to get the IP address:</span></span>

    ```azurecli
    az vm list-ip-addresses -n support-web-vm01
    ```

1. <span data-ttu-id="13b34-142">Usare SSH con l'indirizzo IP pubblico e il nome utente creato.</span><span class="sxs-lookup"><span data-stu-id="13b34-142">Use SSH with the public IP and the username you created.</span></span>

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. <span data-ttu-id="13b34-143">È quindi necessario identificare il disco con il comando `lsblk` per elencare tutti i dispositivi a blocchi: qui verranno visualizzate le unità.</span><span class="sxs-lookup"><span data-stu-id="13b34-143">Next, identify the disk with the `lsblk` command to list all the block devices - here you will see your drives.</span></span>

    ```bash
    lsblk
    ```

    ```output
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0   14G  0 disk
    └─sdb1   8:17   0   14G  0 part /mnt
    sr0     11:0    1  628K  0 rom
    sdc      8:32   0   64G  0 disk
    sda      8:0    0   30G  0 disk
    └─sda1   8:1    0   30G  0 part /
    ```

<span data-ttu-id="13b34-144">Cercare l'unità da 64 GB creata. Si può vedere che è **sdc** e non è montata, perché non è stata inizializzata.</span><span class="sxs-lookup"><span data-stu-id="13b34-144">Look for the 64 GB drive we created - here you can see it's **sdc** and it's not mounted - this is because it's not been initialized.</span></span>

1. <span data-ttu-id="13b34-145">Dopo aver identificato l'unità (**sdc**) è necessario inizializzarla ed è possibile usare `fdisk` a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="13b34-145">Once you know the drive (**sdc**) you need to initialize, you can use `fdisk` to do that.</span></span> <span data-ttu-id="13b34-146">Sarà necessario eseguire il comando con `sudo` e specificare il disco da partizionare:</span><span class="sxs-lookup"><span data-stu-id="13b34-146">You will need to run the command with `sudo` and supply the disk you want to partition:</span></span>

    ```bash
    sudo fdisk /dev/sdc
    ```

1. <span data-ttu-id="13b34-147">Usare il comando <kbd>n</kbd> per aggiungere una nuova partizione.</span><span class="sxs-lookup"><span data-stu-id="13b34-147">Use the <kbd>n</kbd> command to add a new partition.</span></span> <span data-ttu-id="13b34-148">In questo esempio si sceglie <kbd>p</kbd> per una partizione primaria e si accettano gli altri valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="13b34-148">In this example, we also choose <kbd>p</kbd> for a primary partition and accept the rest of the default values.</span></span> <span data-ttu-id="13b34-149">L'output sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="13b34-149">The output will be similar to the following example:</span></span>   

    ```output
    Device does not contain a recognized partition table.
    Created a new DOS disklabel with disk identifier 0x3b44089f.
    
    Command (m for help): n
    Partition type
       p   primary (0 primary, 0 extended, 4 free)
       e   extended (container for logical partitions)
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-268435455, default 2048):
    Last sector, +sectors or +size{K,M,G,T,P} ...
    Created a new partition 1 of type 'Linux' and of size 64 GiB.    
    ```

1. <span data-ttu-id="13b34-150">Stampare la tabella delle partizioni con il comando <kbd>p</kbd>.</span><span class="sxs-lookup"><span data-stu-id="13b34-150">Print the partition table with the <kbd>p</kbd> command.</span></span> <span data-ttu-id="13b34-151">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="13b34-151">It should look something like this:</span></span>

    ```output
    Disk /dev/sdc: 64 GiB ...
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: dos
    Disk identifier: 0x3b44089f
    
    Device     Boot Start ... Size Id Type
    /dev/sdc1        2048 ... 64G 83 Linux
    ```

1. <span data-ttu-id="13b34-152">Scrivere le modifiche con il comando <kbd>w<kbd>.</span><span class="sxs-lookup"><span data-stu-id="13b34-152">Write the changes with the <kbd>w<kbd> command.</span></span> <span data-ttu-id="13b34-153">Si uscirà dallo strumento.</span><span class="sxs-lookup"><span data-stu-id="13b34-153">This will exit the tool.</span></span>

1. <span data-ttu-id="13b34-154">A questo punto è necessario scrivere un file system nella partizione con il comando `mkfs`.</span><span class="sxs-lookup"><span data-stu-id="13b34-154">Next, we need to write a file system to the partition with the `mkfs` command.</span></span> <span data-ttu-id="13b34-155">Sarà necessario specificare il tipo di file system e il nome del dispositivo ottenuti dall'output di `fdisk`:</span><span class="sxs-lookup"><span data-stu-id="13b34-155">We will need to specify the file system type and device name that we got from the `fdisk` output:</span></span>
    - <span data-ttu-id="13b34-156">Passare `-t ext4` per creare un file system _ext4_.</span><span class="sxs-lookup"><span data-stu-id="13b34-156">Pass `-t ext4` to create an _ext4_ filesystem.</span></span>
    - <span data-ttu-id="13b34-157">Il nome del dispositivo è `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="13b34-157">The device name is `/dev/sdc`.</span></span>

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    <span data-ttu-id="13b34-158">Il completamento del comando richiederà alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="13b34-158">This command will take a minute or so to complete.</span></span>

    ```output
    mke2fs 1.42.13 (17-May-2015)
    Discarding device blocks: done
    Creating filesystem with 16777088 4k blocks and 4194304 inodes
    ...
    Allocating group tables: done
    Writing inode tables: done
    Creating journal (32768 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```
    
1. <span data-ttu-id="13b34-159">Creare quindi una directory che verrà usata come punto di montaggio.</span><span class="sxs-lookup"><span data-stu-id="13b34-159">Next, create a directory we will use as our mount point.</span></span> <span data-ttu-id="13b34-160">Si supponga di avere una cartella `uploads`:</span><span class="sxs-lookup"><span data-stu-id="13b34-160">Let's assume we will have a `uploads` folder:</span></span>

    ```bash
    sudo mkdir /uploads
    ```
1. <span data-ttu-id="13b34-161">Usare infine `mount` per collegare il disco al punto di montaggio:</span><span class="sxs-lookup"><span data-stu-id="13b34-161">Finally, use `mount` to attach the disk to the mount point:</span></span>

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```
    <span data-ttu-id="13b34-162">Dovrebbe essere ora possibile usare `lsblk` per vedere l'unità montata:</span><span class="sxs-lookup"><span data-stu-id="13b34-162">You should be able to use `lsblk` to see the mounted drive now:</span></span>
    
    ```output
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0   14G  0 disk
    └─sdb1   8:17   0   14G  0 part /mnt
    sr0     11:0    1  628K  0 rom
    sdc      8:32   0   64G  0 disk
    └─sdc1   8:33   0   64G  0 part /uploads
    sda      8:0    0   30G  0 disk
    └─sda1   8:1    0   30G  0 part /
    ```
    
### <a name="mounting-the-drive-automatically"></a><span data-ttu-id="13b34-163">Montare l'unità automaticamente</span><span class="sxs-lookup"><span data-stu-id="13b34-163">Mounting the drive automatically</span></span>

<span data-ttu-id="13b34-164">Per assicurarsi che l'unità venga montata automaticamente dopo un riavvio, è necessario aggiungerla al file `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="13b34-164">To ensure that the drive is mounted automatically after a reboot, it must be added to the `/etc/fstab` file.</span></span> <span data-ttu-id="13b34-165">È anche consigliabile usare l'identificatore univoco universale (UUID, Universally Unique Identifier) in `/etc/fstab` per fare riferimento all'unità invece di usare solo il nome del dispositivo, ad esempio `/dev/sdc1`.</span><span class="sxs-lookup"><span data-stu-id="13b34-165">It is also highly recommended that the UUID (universally unique identifier) is used in `/etc/fstab` to refer to the drive rather than just the device name (such as `/dev/sdc1`).</span></span> <span data-ttu-id="13b34-166">Se il sistema operativo rileva un errore del disco durante l'avvio, l'uso di UUID evita che venga montato il disco non corretto in una posizione specifica.</span><span class="sxs-lookup"><span data-stu-id="13b34-166">If the OS detects a disk error during boot, using the UUID avoids the incorrect disk being mounted to a given location.</span></span> <span data-ttu-id="13b34-167">Ai dischi dati rimanenti verrebbero quindi assegnati gli stessi ID di dispositivo.</span><span class="sxs-lookup"><span data-stu-id="13b34-167">Remaining data disks would then be assigned those same device IDs.</span></span> <span data-ttu-id="13b34-168">Per individuare l'UUID della nuova unità, usare l'utilità `blkid`:</span><span class="sxs-lookup"><span data-stu-id="13b34-168">To find the UUID of the new drive, use the `blkid` utility:</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="13b34-169">Il risultato sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="13b34-169">It will return something like:</span></span>

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. <span data-ttu-id="13b34-170">Copiare l'UUID per l'unità `/dev/sdc1` e aprire il file `/etc/fstab` in un editor di testo:</span><span class="sxs-lookup"><span data-stu-id="13b34-170">Copy the UUID for the `/dev/sdc1` drive and open the `/etc/fstab` file in a text editor:</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> <span data-ttu-id="13b34-171">Se il file `/etc/fstab` non viene modificato in modo corretto, il sistema potrebbe non essere disponibile per l'avvio.</span><span class="sxs-lookup"><span data-stu-id="13b34-171">Improperly editing the `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="13b34-172">In caso di dubbi, fare riferimento alla documentazione della distribuzione per informazioni su come modificare correttamente questo file.</span><span class="sxs-lookup"><span data-stu-id="13b34-172">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="13b34-173">È anche consigliabile creare una copia di backup del file prima della modifica nei sistemi di produzione.</span><span class="sxs-lookup"><span data-stu-id="13b34-173">It is also recommended that a backup of the file is created before editing when you are working with production systems.</span></span>

1. <span data-ttu-id="13b34-174">Premere <kbd>G</kbd> per spostarsi sull'ultima riga del file.</span><span class="sxs-lookup"><span data-stu-id="13b34-174">Press <kbd>G</kbd> to move to the last line in the file.</span></span>

1. <span data-ttu-id="13b34-175">Premere <kbd>I</kbd> per attivare la modalità di inserimento.</span><span class="sxs-lookup"><span data-stu-id="13b34-175">Press <kbd>I</kbd> to enter INSERT mode.</span></span> <span data-ttu-id="13b34-176">La modalità dovrebbe essere indicata nella parte inferiore della schermata.</span><span class="sxs-lookup"><span data-stu-id="13b34-176">It should indicate the mode at the bottom of the screen.</span></span>

1. <span data-ttu-id="13b34-177">Premere <kbd>FINE</kbd> per spostarsi alla fine della riga.</span><span class="sxs-lookup"><span data-stu-id="13b34-177">Press the <kbd>END</kbd> key to move to the end of the line.</span></span> <span data-ttu-id="13b34-178">In alternativa, è possibile usare i tasti di direzione.</span><span class="sxs-lookup"><span data-stu-id="13b34-178">Alternatively, you can use the arrow keys.</span></span> <span data-ttu-id="13b34-179">Premere <kbd>INVIO</kbd> per spostarsi su una nuova riga.</span><span class="sxs-lookup"><span data-stu-id="13b34-179">Press <kbd>ENTER</kbd> to move to a new line.</span></span>

1. <span data-ttu-id="13b34-180">Digitare la riga seguente nell'editor.</span><span class="sxs-lookup"><span data-stu-id="13b34-180">Type the following line into the editor.</span></span> <span data-ttu-id="13b34-181">I valori possono essere separati da spazi o tabulazioni.</span><span class="sxs-lookup"><span data-stu-id="13b34-181">The values can be space or tab separated.</span></span> <span data-ttu-id="13b34-182">Per altre informazioni su ognuna delle colonne, vedere la documentazione:</span><span class="sxs-lookup"><span data-stu-id="13b34-182">Check the documentation for more information on each of the columns:</span></span>

    ```output
    UUID=<uuid-goes-here>    /uploads    ext4    defaults,nofail    1    2
    ```
1. <span data-ttu-id="13b34-183">Premere **ESC** e quindi digitare **:w!**</span><span class="sxs-lookup"><span data-stu-id="13b34-183">Press **ESC**, then type **:w!**</span></span> <span data-ttu-id="13b34-184">per scrivere il file e **:q** per uscire dall'editor.</span><span class="sxs-lookup"><span data-stu-id="13b34-184">to write the file and **:q** to quit the editor.</span></span>

1. <span data-ttu-id="13b34-185">Infine, controllare per verificare che la voce sia corretta, chiedendo al sistema operativo di aggiornare i punti di montaggio:</span><span class="sxs-lookup"><span data-stu-id="13b34-185">Finally, let's check to make sure the entry is correct by asking the OS to refresh the mount points:</span></span>

    ```bash
    sudo mount -a
    ```

    <span data-ttu-id="13b34-186">Se viene restituito un errore, modificare il file per trovare il problema.</span><span class="sxs-lookup"><span data-stu-id="13b34-186">If it returns an error, edit the file to find the problem.</span></span>

> [!TIP]
> <span data-ttu-id="13b34-187">Alcuni kernel di Linux supportano la funzionalità TRIM per rimuovere i blocchi inutilizzati sui dischi.</span><span class="sxs-lookup"><span data-stu-id="13b34-187">Some Linux kernels support TRIM to discard unused blocks on disks.</span></span> <span data-ttu-id="13b34-188">Questa funzionalità è disponibile per i dischi di Azure e consente di risparmiare denaro se si creano file di grandi dimensioni che poi vengono eliminati.</span><span class="sxs-lookup"><span data-stu-id="13b34-188">This feature is available on Azure disks and can save you money if you create large files and then delete them.</span></span> <span data-ttu-id="13b34-189">Vedere la documentazione per informazioni su come [attivare questa funzionalità](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure).</span><span class="sxs-lookup"><span data-stu-id="13b34-189">Learn how to [turn this feature on](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure) in our documentation.</span></span>

<span data-ttu-id="13b34-190">Ora che si è visto come è facile aggiungere dischi alle macchine virtuali, verranno esaminati più in dettaglio i tipi di dischi che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="13b34-190">Now that you've seen how easy it is to add disks to your VMs, let's explore a bit more about the types of disks you might create.</span></span> <span data-ttu-id="13b34-191">In particolare la scelta tra archiviazione Standard e Premium.</span><span class="sxs-lookup"><span data-stu-id="13b34-191">In particular the choice between Standard and Premium storage.</span></span>