<span data-ttu-id="ef2fc-101">A questo punto la macchina virtuale Linux è stata distribuita ed è in esecuzione, ma non è configurata per eseguire alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-101">We have our Linux VM deployed and running, but it's not configured to do any work.</span></span> <span data-ttu-id="ef2fc-102">Di seguito verrà descritto come connettersi alla macchina virtuale con SSH e configurare Apache, in modo da avere un server Web in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-102">Let's connect to it with SSH and configure Apache, so we have a running web server.</span></span>

## <a name="connect-to-the-vm-with-ssh"></a><span data-ttu-id="ef2fc-103">Connettersi alla macchina virtuale con SSH</span><span class="sxs-lookup"><span data-stu-id="ef2fc-103">Connect to the VM with SSH</span></span>

<span data-ttu-id="ef2fc-104">Per connettersi a una macchina virtuale di Azure con un client SSH, è necessario:</span><span class="sxs-lookup"><span data-stu-id="ef2fc-104">To connect to an Azure VM with an SSH client, you will need:</span></span>

- <span data-ttu-id="ef2fc-105">Software client SSH (presente nella maggior parte dei sistemi operativi più recenti)</span><span class="sxs-lookup"><span data-stu-id="ef2fc-105">SSH client software (present on most modern operating systems)</span></span>
- <span data-ttu-id="ef2fc-106">L'indirizzo IP pubblico della macchina virtuale (o privato se la macchina virtuale è configurata per connettersi alla rete)</span><span class="sxs-lookup"><span data-stu-id="ef2fc-106">The public IP address of the VM (or private if the VM is configured to connect to your network)</span></span>

### <a name="get-the-public-ip-address"></a><span data-ttu-id="ef2fc-107">Ottenere l'indirizzo IP pubblico</span><span class="sxs-lookup"><span data-stu-id="ef2fc-107">Get the public IP address</span></span>

1. <span data-ttu-id="ef2fc-108">Nel [portale di Azure](https://portal.azure.com?azure-portal=true) verificare che il pannello **Panoramica** per la macchina virtuale creata in precedenza sia aperto.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-108">In the [Azure portal](https://portal.azure.com?azure-portal=true), ensure the **Overview** panel for the virtual machine that you created earlier is open.</span></span> <span data-ttu-id="ef2fc-109">La macchina virtuale è disponibile in **Tutte le risorse** se è necessario aprirla.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-109">You can find the VM under **All Resources** if you need to open it.</span></span> <span data-ttu-id="ef2fc-110">Il pannello di panoramica presenta numerose informazioni sulla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-110">The overview panel has a lot of information about the VM.</span></span>

    - <span data-ttu-id="ef2fc-111">È possibile vedere se la macchina virtuale è in esecuzione</span><span class="sxs-lookup"><span data-stu-id="ef2fc-111">You can see whether the VM is running</span></span>
    - <span data-ttu-id="ef2fc-112">Arrestarla o riavviarla</span><span class="sxs-lookup"><span data-stu-id="ef2fc-112">Stop or restart it</span></span>
    - <span data-ttu-id="ef2fc-113">Ottenere l'indirizzo IP pubblico per connettersi alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="ef2fc-113">Get the public IP address to connect to the VM</span></span>
    - <span data-ttu-id="ef2fc-114">Visualizzare l'attività di CPU, disco e rete</span><span class="sxs-lookup"><span data-stu-id="ef2fc-114">See the activity of the CPU, disk, and network</span></span>

1. <span data-ttu-id="ef2fc-115">Fare clic sul pulsante **Connetti** nella parte superiore del riquadro.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-115">Click the **Connect** button at the top of the pane.</span></span>

1. <span data-ttu-id="ef2fc-116">Nel pannello **Connetti a macchina virtuale** notare le impostazioni **Indirizzo IP** e **Numero di porta**.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-116">In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings.</span></span> <span data-ttu-id="ef2fc-117">Nella scheda **SSH** è inoltre disponibile il comando che è necessario eseguire in locale per connettersi alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-117">On the **SSH** tab, you will also find the command you need to execute locally to connect to the VM.</span></span> <span data-ttu-id="ef2fc-118">Copiarlo negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-118">Copy this to the clipboard.</span></span>

<!-- TODO: this will be necessary if we ever have inline portal integration 

### Open the Azure Cloud Shell

Let's use the Cloud Shell in the Azure Portal. If you generated the SSH key locally, you need to use your local session since the private key won't be in your storage account.

1. Switch back to the **Dashboard** by clicking the Dashboard button in the Azure sidebar.

1. Open the Cloud Shell by clicking the shell button in the top toolbar.

    ![Open the Azure Cloud Shell](../media-drafts/6-cloud-shell.png)

1. Select **Bash** as the shell type. PowerShell is also available if you are a Windows administrator.

    ![Select bash shell in the portal](../media-drafts/6-use-bash-shell.png)

-->

## <a name="connect-with-ssh"></a><span data-ttu-id="ef2fc-119">Connettersi tramite SSH</span><span class="sxs-lookup"><span data-stu-id="ef2fc-119">Connect with SSH</span></span>

1. <span data-ttu-id="ef2fc-120">Incollare la riga di comando copiata dalla scheda SSH in Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-120">Paste the command line you got from the SSH tab into the Cloud Shell.</span></span> <span data-ttu-id="ef2fc-121">Il risultato dovrebbe essere simile al seguente, anche se l'indirizzo IP sarà diverso e probabilmente anche il nome utente se non è stato usato il nome **jim**.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-121">It should look something like this; however it will have a different IP address (and perhaps a different username if you didn't use **jim**!)</span></span>

    ```bash
    ssh jim@137.117.101.249
    ```

1. <span data-ttu-id="ef2fc-122">Questo comando aprirà una connessione sicura alla shell e un prompt dei comandi della shell tradizionale per Linux.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-122">This command will open a secure shell connection and place you at a traditional shell command prompt for Linux.</span></span>

1. <span data-ttu-id="ef2fc-123">Provare a eseguire alcuni comandi di Linux</span><span class="sxs-lookup"><span data-stu-id="ef2fc-123">Try executing a few Linux commands</span></span>
    - <span data-ttu-id="ef2fc-124">`ls -la /` per mostrare la radice del disco</span><span class="sxs-lookup"><span data-stu-id="ef2fc-124">`ls -la /` to show the root of the disk</span></span>
    - <span data-ttu-id="ef2fc-125">`ps -l` per mostrare tutti i processi in esecuzione</span><span class="sxs-lookup"><span data-stu-id="ef2fc-125">`ps -l` to show all the running processes</span></span>
    - <span data-ttu-id="ef2fc-126">`dmesg` per elencare tutti i messaggi del kernel</span><span class="sxs-lookup"><span data-stu-id="ef2fc-126">`dmesg` to list all the kernel messages</span></span>
    - <span data-ttu-id="ef2fc-127">`lsblk` per elencare tutti i dispositivi a blocchi - qui verranno visualizzate le unità</span><span class="sxs-lookup"><span data-stu-id="ef2fc-127">`lsblk` to list all the block devices - here you will see your drives</span></span>

<span data-ttu-id="ef2fc-128">L'aspetto più interessante da osservare nell'elenco delle unità è cosa _manca_.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-128">The more interesting thing to observe in the list of drives is what is _missing_.</span></span> <span data-ttu-id="ef2fc-129">Si noti che l'unità **dati** (`sdc`) è presente ma non montata nel file system.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-129">Notice that our **Data** drive (`sdc`) is present but not mounted into the file system.</span></span> <span data-ttu-id="ef2fc-130">Azure ha aggiunto un disco rigido virtuale, ma non l'ha inizializzato.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-130">Azure added a VHD but didn't initialize it.</span></span>

## <a name="initialize-data-disks"></a><span data-ttu-id="ef2fc-131">Inizializzare i dischi dati</span><span class="sxs-lookup"><span data-stu-id="ef2fc-131">Initialize data disks</span></span>

<span data-ttu-id="ef2fc-132">Tutte le unità aggiuntive create da zero dovranno essere inizializzate e formattate.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-132">Any additional drives you create from scratch will need to be initialized and formatted.</span></span> <span data-ttu-id="ef2fc-133">Il processo per eseguire questa operazione è identico a quello valido per un disco fisico.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-133">The process for doing this is identical to a physical disk.</span></span>

1. <span data-ttu-id="ef2fc-134">Prima di tutto identificare il disco.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-134">First, identify the disk.</span></span> <span data-ttu-id="ef2fc-135">Questo passaggio è già stato eseguito.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-135">We did that above.</span></span> <span data-ttu-id="ef2fc-136">Si potrebbe anche usare `dmesg | grep SCSI` che elencherà tutti i messaggi dal kernel per i dispositivi SCSI.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-136">You could also use `dmesg | grep SCSI` which will list all the messages from the kernel for SCSI devices.</span></span>

1. <span data-ttu-id="ef2fc-137">Dopo aver identificato l'unità (`sdc`) è necessario inizializzarla ed è possibile usare `fdisk` a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-137">Once you know the drive (`sdc`) you need to initialize, you can use `fdisk` to do that.</span></span> <span data-ttu-id="ef2fc-138">Sarà necessario eseguire il comando con `sudo` e specificare il disco da partizionare.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-138">You will need to run the command with `sudo` and supply the disk you want to partition.</span></span>

    ```bash
    sudo fdisk /dev/sdc
    ```
1. <span data-ttu-id="ef2fc-139">Usare il comando `n` per aggiungere una nuova partizione.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-139">Use the `n` command to add a new partition.</span></span>  <span data-ttu-id="ef2fc-140">In questo esempio si sceglie p per una partizione primaria e si accettano gli altri valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-140">In this example, we also choose p for a primary partition and accept the rest of the default values.</span></span> <span data-ttu-id="ef2fc-141">L'output sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="ef2fc-141">The output will be similar to the following example:</span></span>   

    ```output
    Device does not contain a recognized partition table.
    Created a new DOS disklabel with disk identifier 0x1f2d0c46.
    
    Command (m for help): n
    Partition type
       p   primary (0 primary, 0 extended, 4 free)
       e   extended (container for logical partitions)
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-2145386495, default 2048):
    Last sector, +sectors or +size{K,M,G,T,P} (2048-2145386495, default 2145386495):
    
    Created a new partition 1 of type 'Linux' and of size 1023 GiB.
    ```    

1. <span data-ttu-id="ef2fc-142">Stampare la tabella delle partizioni con il comando `p`.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-142">Print the partition table with the `p` command.</span></span> <span data-ttu-id="ef2fc-143">Dovrebbe essere visualizzata una schermata analoga alla seguente:</span><span class="sxs-lookup"><span data-stu-id="ef2fc-143">It should look something like this:</span></span>

    ```output
    Disk /dev/sdc: 1023 GiB, 1098437885952 bytes, 2145386496 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: dos
    Disk identifier: 0x1f2d0c46
    
    Device     Boot Start        End    Sectors  Size Id Type
    /dev/sdc1        2048 2145386495 2145384448 1023G 83 Linux
    ```
    
1. <span data-ttu-id="ef2fc-144">Scrivere le modifiche con il comando `w`.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-144">Write the changes with the `w` command.</span></span> <span data-ttu-id="ef2fc-145">Si uscirà dallo strumento.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-145">This will exit the tool.</span></span>

1. <span data-ttu-id="ef2fc-146">A questo punto è necessario scrivere un file system nella partizione con il comando `mkfs`.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-146">Next, we need to write a file system to the partition with the `mkfs` command.</span></span> <span data-ttu-id="ef2fc-147">Sarà necessario specificare il tipo di file system e il nome del dispositivo ottenuti dall'output di `fdisk`.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-147">We will need to specify the file system type and device name which we got from the `fdisk` output.</span></span>
    - <span data-ttu-id="ef2fc-148">Passare `-t ext4` per creare un file system _ext4_.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-148">Pass `-t ext4` to create an _ext4_ filesystem.</span></span>
    - <span data-ttu-id="ef2fc-149">Il nome del dispositivo è `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-149">The device name is `/dev/sdc`.</span></span>

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    <span data-ttu-id="ef2fc-150">Il completamento del comando richiederà alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-150">This command will take a few minutes to complete.</span></span>

    ```output
    mke2fs 1.44.1 (24-Mar-2018)
    Discarding device blocks: done
    Creating filesystem with 268173056 4k blocks and 67043328 inodes
    Filesystem UUID: e311c905-e0d9-43ab-af63-7f4ee4ef108e
    Superblock backups stored on blocks:
            32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
            4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
            102400000, 214990848
    
    Allocating group tables: done
    Writing inode tables: done
    Creating journal (262144 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```

1. <span data-ttu-id="ef2fc-151">Creare quindi una directory che verrà usata come punto di montaggio.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-151">Next, create a directory we will use as out mount point.</span></span> <span data-ttu-id="ef2fc-152">Si supponga di avere una cartella `data`.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-152">Let's assume we will have a `data` folder.</span></span>

    ```bash
    sudo mkdir /data
    ```
1. <span data-ttu-id="ef2fc-153">Usare infine `mount` per collegare il disco al punto di montaggio.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-153">Finally, use `mount` to attach the disk to the mount point.</span></span>

    ```bash
    sudo mount /dev/sdc1 /data
    ```
    <span data-ttu-id="ef2fc-154">Dovrebbe essere ora possibile usare `lsblk` per vedere l'unità montata.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-154">You should be able to use `lsblk` to see the mounted drive now.</span></span>
    
    ```output
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda       8:0    0   30G  0 disk
    ├─sda1    8:1    0 29.9G  0 part /
    ├─sda14   8:14   0    4M  0 part
    └─sda15   8:15   0  106M  0 part /boot/efi
    sdb       8:16   0   16G  0 disk
    └─sdb1    8:17   0   16G  0 part /mnt
    sdc       8:32   0 1023G  0 disk
    └─sdc1    8:33   0 1023G  0 part /data
    sr0      11:0    1  628K  0 rom
    ```

### <a name="mounting-the-drive-automatically"></a><span data-ttu-id="ef2fc-155">Montare l'unità automaticamente</span><span class="sxs-lookup"><span data-stu-id="ef2fc-155">Mounting the drive automatically</span></span>

<span data-ttu-id="ef2fc-156">Per assicurarsi che l'unità venga montata automaticamente dopo un riavvio, è necessario aggiungerla al file `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-156">To ensure that the drive is mounted automatically after a reboot, it must be added to the `/etc/fstab` file.</span></span> <span data-ttu-id="ef2fc-157">È anche consigliabile usare l'UUID (Universally Unique Identifier) in `/etc/fstab` per fare riferimento all'unità invece di usare solo il nome del dispositivo, ad esempio `/dev/sdc1`.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-157">It is also highly recommended that the UUID (Universally Unique Identifier) is used in `/etc/fstab` to refer to the drive rather than just the device name (such as, `/dev/sdc1`).</span></span> <span data-ttu-id="ef2fc-158">Se il sistema operativo rileva un errore del disco durante l'avvio, l'uso dell'UUID evita che venga montato il disco non corretto in una posizione specificata.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-158">If the OS detects a disk error during boot, using the UUID avoids the incorrect disk being mounted to a given location.</span></span> <span data-ttu-id="ef2fc-159">Ai dischi dati rimanenti verrebbero quindi assegnati gli stessi ID di dispositivo.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-159">Remaining data disks would then be assigned those same device IDs.</span></span> <span data-ttu-id="ef2fc-160">Per individuare l'UUID della nuova unità, usare l'utilità `blkid`:</span><span class="sxs-lookup"><span data-stu-id="ef2fc-160">To find the UUID of the new drive, use the `blkid` utility:</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="ef2fc-161">Il risultato sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ef2fc-161">It will return something like:</span></span>

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. <span data-ttu-id="ef2fc-162">Copiare l'UUID per l'unità `/dev/sdc1` e aprire il file `/etc/fstab` in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-162">Copy the UUID for the `/dev/sdc1` drive and open the `/etc/fstab` file in a text editor.</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> <span data-ttu-id="ef2fc-163">Se il file `/etc/fstab` non viene modificato in modo corretto, il sistema potrebbe diventare non avviabile.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-163">Improperly editing the `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="ef2fc-164">In caso di dubbi, fare riferimento alla documentazione della distribuzione per informazioni su come modificare correttamente questo file.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-164">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="ef2fc-165">È anche consigliabile creare una copia di backup del file prima della modifica nei sistemi di produzione.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-165">It is also recommended that a backup of the file is created before editing when you are working with production systems.</span></span>

1. <span data-ttu-id="ef2fc-166">Premere **G** per spostarsi sull'ultima riga nel file.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-166">Press **G** to move to the last line in the file.</span></span>

1. <span data-ttu-id="ef2fc-167">Premere **I** per attivare la modalità di inserimento.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-167">Press **I** to enter INSERT mode.</span></span> <span data-ttu-id="ef2fc-168">La modalità dovrebbe essere indicata nella parte inferiore della schermata.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-168">It should indicate the mode at the bottom of the screen.</span></span>

1. <span data-ttu-id="ef2fc-169">Premere **FINE** per spostarsi alla fine della riga.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-169">Press the **END** key to move to the end of the line.</span></span> <span data-ttu-id="ef2fc-170">In alternativa, è possibile usare i tasti di direzione.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-170">Alternatively, you can use the arrow keys.</span></span> <span data-ttu-id="ef2fc-171">Premere **INVIO** per spostarsi su una nuova riga.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-171">Press **ENTER** to move to a new line.</span></span>

1. <span data-ttu-id="ef2fc-172">Digitare il codice seguente nell'editor.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-172">Type the following line into the editor.</span></span> <span data-ttu-id="ef2fc-173">I valori possono essere separati da spazio o tabulazione.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-173">The values can be space or tab separated.</span></span> <span data-ttu-id="ef2fc-174">Per altre informazioni su ognuna delle colonne, vedere la documentazione.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-174">Check the documentation for more information on each of the columns.</span></span>

    ```output
    UUID=<uuid-goes-here>    /data    ext4    defaults,nofail    1    2
    ```
1. <span data-ttu-id="ef2fc-175">Premere **ESC** e quindi digitare **:w!**</span><span class="sxs-lookup"><span data-stu-id="ef2fc-175">Press **ESC**, then type **:w!**</span></span> <span data-ttu-id="ef2fc-176">per scrivere il file e **:q** per uscire dall'editor.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-176">to write the file, and **:q** to quit the editor.</span></span>

1. <span data-ttu-id="ef2fc-177">Infine, è possibile controllare per verificare che la voce sia corretta, chiedendo al sistema operativo di aggiornare i punti di montaggio.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-177">Finally, let's check to make sure the entry is correct by asking the OS to refresh the mount points.</span></span>

    ```bash
    sudo mount -a
    ```
    
    <span data-ttu-id="ef2fc-178">Se viene restituito un errore, modificare il file per individuare il problema.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-178">If it returns an error, edit the file to find the problem.</span></span>

> [!TIP]
> <span data-ttu-id="ef2fc-179">Alcuni kernel di Linux supportano TRIM per rimuovere i blocchi inutilizzati sui dischi.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-179">Some Linux kernels support TRIM to discard unused blocks on disks.</span></span> <span data-ttu-id="ef2fc-180">Questa funzionalità è disponibile per i dischi di Azure e consente di risparmiare denaro se si creano file di grandi dimensioni e quindi vengono eliminati.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-180">This feature is available on Azure disks and can save you money if you create large files and then delete them.</span></span> <span data-ttu-id="ef2fc-181">Vedere la documentazione per informazioni su come [attivare questa funzionalità](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure).</span><span class="sxs-lookup"><span data-stu-id="ef2fc-181">Learn how to [turn this feature on](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure) in our documentation.</span></span>

## <a name="install-software-onto-the-vm"></a><span data-ttu-id="ef2fc-182">Installare software nella macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="ef2fc-182">Install software onto the VM</span></span>

<span data-ttu-id="ef2fc-183">Sono disponibili diverse opzioni per installare il software nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-183">You have several options to install software onto the VM.</span></span> <span data-ttu-id="ef2fc-184">Prima di tutto, come già accennato, è possibile usare `scp` per copiare i file locali dal computer alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-184">First, as mentioned, you can use `scp` to copy local files from your machine to the VM.</span></span> <span data-ttu-id="ef2fc-185">Ciò consente di copiare i dati o le applicazioni personalizzate da eseguire.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-185">This lets you copy over data, or custom applications you want to run.</span></span>

<span data-ttu-id="ef2fc-186">È anche possibile installare software tramite Secure Shell.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-186">You can also install software through the secure shell.</span></span> <span data-ttu-id="ef2fc-187">Le macchine di Azure sono connesse a Internet per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-187">Azure machines are by default, Internet-connected.</span></span> <span data-ttu-id="ef2fc-188">È possibile usare i comandi standard per installare i pacchetti software più diffusi direttamente dai repository standard.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-188">You can use standard commands to install popular software packages directly from standard repositories.</span></span> <span data-ttu-id="ef2fc-189">Questo è l'approccio che verrà usato per installare Apache.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-189">Let's use this approach to install Apache.</span></span>

### <a name="install-apache-web-server"></a><span data-ttu-id="ef2fc-190">Installare il server Web Apache</span><span class="sxs-lookup"><span data-stu-id="ef2fc-190">Install Apache web server</span></span>

<span data-ttu-id="ef2fc-191">Apache è disponibile all'interno dei repository software predefiniti di Ubuntu, quindi verrà installato usando gli strumenti di gestione dei pacchetti convenzionali.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-191">Apache is available within Ubuntu's default software repositories, so we will install it using conventional package management tools.</span></span>

1. <span data-ttu-id="ef2fc-192">Per iniziare, aggiornare l'indice dei pacchetti locali in modo che rispecchi le modifiche upstream più recenti.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-192">Start by updating the local package index to reflect the latest upstream changes.</span></span>

    ```bash
    sudo apt-get update
    ```
    
1. <span data-ttu-id="ef2fc-193">Successivamente, installare Apache.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-193">Next, install Apache.</span></span>

    ```bash
    sudo apt-get install apache2
    ```
    
1. <span data-ttu-id="ef2fc-194">L'operazione dovrebbe essere avviata automaticamente ed è possibile controllare lo stato usando `systemctl`:</span><span class="sxs-lookup"><span data-stu-id="ef2fc-194">It should start automatically - we can check the status using `systemctl`:</span></span>

    ```bash
    sudo systemctl status apache2
    ```

    <span data-ttu-id="ef2fc-195">Questo comando dovrebbe restituire informazioni simili alle seguenti:</span><span class="sxs-lookup"><span data-stu-id="ef2fc-195">This should return something like:</span></span>

    ```output
    apache2.service - The Apache HTTP Server
       Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
      Drop-In: /lib/systemd/system/apache2.service.d
               └─apache2-systemd.conf
       Active: active (running) since Mon 2018-09-03 21:00:03 UTC; 1min 34s ago
     Main PID: 11156 (apache2)
        Tasks: 55 (limit: 4915)
       CGroup: /system.slice/apache2.service
               ├─11156 /usr/sbin/apache2 -k start
               ├─11158 /usr/sbin/apache2 -k start
               └─11159 /usr/sbin/apache2 -k start
    
    test-web-eus-vm1 systemd[1]: Starting The Apache HTTP Server...
    test-web-eus-vm1 apachectl[11129]: AH00558: apache2: Could not reliably determine the server's fully qua
    test-web-eus-vm1 systemd[1]: Started The Apache HTTP Server.
    ```

1. <span data-ttu-id="ef2fc-196">Infine, è possibile provare a recuperare la pagina predefinita tramite l'indirizzo IP pubblico.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-196">Finally, we can try retrieving the default page through the public IP address.</span></span> <span data-ttu-id="ef2fc-197">Dovrebbe essere restituita una pagina predefinita.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-197">It should return a default page.</span></span>

    ![Pagina Web predefinita di Apache](../media-drafts/6-apache-works.png)

<span data-ttu-id="ef2fc-199">Come si può notare, SSH consente di lavorare con la macchina virtuale Linux come con un computer locale.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-199">As you can see, SSH allows you to work with the Linux VM just like a local computer.</span></span> <span data-ttu-id="ef2fc-200">È possibile amministrare questa macchina virtuale come qualsiasi altro computer Linux: installare software, configurare ruoli, mettere a punto le funzionalità ed eseguire altre attività quotidiane.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-200">You can administer this VM as you would any other Linux computer: installing software, configuring roles, adjusting features and other everyday tasks.</span></span> <span data-ttu-id="ef2fc-201">Si tratta tuttavia di processi manuali. Se è necessario installare sempre alcuni software, è possibile valutare la possibilità di automatizzare il processo tramite scripting.</span><span class="sxs-lookup"><span data-stu-id="ef2fc-201">However, it's a manual process - if we always need to install some software, you might consider automating the process using scripting.</span></span>
