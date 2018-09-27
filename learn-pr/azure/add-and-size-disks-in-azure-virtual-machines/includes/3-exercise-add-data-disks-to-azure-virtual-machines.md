Lo studio legale presso cui si lavora sta assistendo a un aumento dei casi gestiti e si riceve la richiesta di attivare un nuovo server Web Linux per archiviare documenti critici da svariate origini: clienti, altri studi legali e forze dell'ordine. Il server può accettare caricamenti, che devono essere archiviati su disco.

> [!TIP]
> Questo esercizio usa Linux come esempio, ma il processo di base per la creazione di macchine virtuali e l'aggiunta di dischi è lo stesso per Windows. La differenza principale riguarda il partizionamento e la formattazione del disco, che verrebbero eseguiti in una sessione di Desktop remoto tramite gli strumenti predefiniti di Gestione disco.

L'obiettivo dell'esercizio è creare una macchina virtuale Linux e collegare un nuovo disco rigido virtuale (VHD) denominato "uploadDataDisk1" per archiviare la directory "uploads".

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm"></a>Creare una macchina virtuale Linux

È possibile creare una macchina virtuale Linux per ospitare il server Web tramite l'interfaccia della riga di comando di Azure.

1. Iniziare impostando alcune impostazioni predefinite per questa sessione. Per prima cosa è necessario decidere la _località_ in cui posizionare la macchina virtuale. L'ideale sarebbe una località nelle vicinanze dei clienti. In questo caso, selezionare l'area più vicina tra le località disponibili per l'ambiente sandbox di Azure.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Usare il comando `az configure` per impostare la località predefinita da usare. Sostituire "eastus" con tale località.

    ```azurecli
    az configure --defaults location=eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. Impostare il valore del gruppo di risorse predefinito sul gruppo di risorse preconfigurato creato per l'ambiente sandbox di Azure: **<rgn>[gruppo di risorse sandbox]</rgn>**

    ```azurecli
    az configure --defaults group="<rgn>[sandbox Resource Group]</rgn>"
    ```

1. Usare poi il comando `vm create` per creare una nuova macchina virtuale Ubuntu Linux.
    - Assegnare il nome "support-web-vm01".
    - Impostare le **dimensioni** su _Standard_DS2_v2_.
    - Impostare il **nome utente amministratore** su "azureuser" (o il nome preferito).
    - Generare le chiavi SSH con il parametro `--generate-ssh-keys`.

    ```azurecli
    az vm create --name support-web-vm01 \
        --image UbuntuLTS \
        --size Standard_DS2_v2 \
        --admin-username azureuser \
        --generate-ssh-keys
    ```
    
    > [!TIP]
    > Per la creazione della macchina virtuale e la sua distribuzione in Azure possono essere necessari alcuni minuti. È possibile monitorare lo stato di avanzamento in Cloud Shell.
    
1. Verrà generato un blocco JSON con i dettagli sulla macchina virtuale creata.

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
    Prendere nota di **publicIpAddress** nell'output. Questo è l'indirizzo che potrà essere usato per accedere alla macchina virtuale in modalità remota.

    > [!NOTE]
    > Questa macchina virtuale viene usata per imparare a gestire i dischi. Se davvero venisse usata come server Web, sarebbe necessario aprire porte aggiuntive con il comando `az vm open-port` installare software per server Web. Queste attività esulano dagli scopi di questo modulo, ma è possibile eseguire il modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure** per imparare a eseguire questi passaggi. 

## <a name="add-an-empty-data-disk-to-our-vm"></a>Aggiungere un disco dati vuoto alla macchina virtuale

Il disco che archivierà la directory "upload" per il server Web verrà denominato "uploadDataDisk1". 

> [!TIP]
> È possibile aggiungere dischi dati quando si crea la macchina virtuale usando il parametro `--data-disk-sizes-gb` per il comando `vm create`.

1. Aggiungere un nuovo disco vuoto al server con il comando `vm disk attach`.
    - Denominarlo "uploadDataDisk1"
    - Impostarlo per 64 GB.
    - Impostare lo **SKU** su "_Premium_LRS_" in modo da poter usare l'archiviazione Premium con ridondanza locale.

    ```azurecli
    az vm disk attach \
        --vm-name support-web-vm01 \
        --disk uploadDataDisk1 \
        --size-gb 64 \
        --sku Premium_LRS \
        --new
    ```
    
È stato definito un disco denominato "uploadDataDisk1". Per usare il disco, è necessario partizionarlo e formattarlo.

## <a name="initialize-data-disks"></a>Inizializzare i dischi dati

Tutte le unità aggiuntive create da zero dovranno essere inizializzate e formattate. Il processo per eseguire questa operazione è identico a quello valido per un disco fisico:

1. Prima di tutto, connettersi tramite SSH al server Linux usando l'indirizzo IP pubblico ottenuto dalla risposta di creazione della macchina virtuale originale. Se non è disponibile, è possibile usare il comando seguente per ottenere l'indirizzo IP:

    ```azurecli
    az vm list-ip-addresses -n support-web-vm01
    ```

1. Usare SSH con l'indirizzo IP pubblico e il nome utente creato.

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. È quindi necessario identificare il disco con il comando `lsblk` per elencare tutti i dispositivi a blocchi: qui verranno visualizzate le unità.

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

Cercare l'unità da 64 GB creata. Si può vedere che è **sdc** e non è montata, perché non è stata inizializzata.

1. Dopo aver identificato l'unità (**sdc**) è necessario inizializzarla ed è possibile usare `fdisk` a tale scopo. Sarà necessario eseguire il comando con `sudo` e specificare il disco da partizionare:

    ```bash
    sudo fdisk /dev/sdc
    ```

1. Usare il comando <kbd>n</kbd> per aggiungere una nuova partizione. In questo esempio si sceglie <kbd>p</kbd> per una partizione primaria e si accettano gli altri valori predefiniti. L'output sarà simile all'esempio seguente:   

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

1. Stampare la tabella delle partizioni con il comando <kbd>p</kbd>. L'output dovrebbe essere simile al seguente:

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

1. Scrivere le modifiche con il comando <kbd>w<kbd>. Si uscirà dallo strumento.

1. A questo punto è necessario scrivere un file system nella partizione con il comando `mkfs`. Sarà necessario specificare il tipo di file system e il nome del dispositivo ottenuti dall'output di `fdisk`:
    - Passare `-t ext4` per creare un file system _ext4_.
    - Il nome del dispositivo è `/dev/sdc`.

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    Il completamento del comando richiederà alcuni minuti.

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
    
1. Creare quindi una directory che verrà usata come punto di montaggio. Si supponga di avere una cartella `uploads`:

    ```bash
    sudo mkdir /uploads
    ```
1. Usare infine `mount` per collegare il disco al punto di montaggio:

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```
    Dovrebbe essere ora possibile usare `lsblk` per vedere l'unità montata:
    
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
    
### <a name="mounting-the-drive-automatically"></a>Montare l'unità automaticamente

Per assicurarsi che l'unità venga montata automaticamente dopo un riavvio, è necessario aggiungerla al file `/etc/fstab`. È anche consigliabile usare l'identificatore univoco universale (UUID, Universally Unique Identifier) in `/etc/fstab` per fare riferimento all'unità invece di usare solo il nome del dispositivo, ad esempio `/dev/sdc1`. Se il sistema operativo rileva un errore del disco durante l'avvio, l'uso di UUID evita che venga montato il disco non corretto in una posizione specifica. Ai dischi dati rimanenti verrebbero quindi assegnati gli stessi ID di dispositivo. Per individuare l'UUID della nuova unità, usare l'utilità `blkid`:

```bash
sudo -i blkid
```

Il risultato sarà simile al seguente:

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. Copiare l'UUID per l'unità `/dev/sdc1` e aprire il file `/etc/fstab` in un editor di testo:

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> Se il file `/etc/fstab` non viene modificato in modo corretto, il sistema potrebbe non essere disponibile per l'avvio. In caso di dubbi, fare riferimento alla documentazione della distribuzione per informazioni su come modificare correttamente questo file. È anche consigliabile creare una copia di backup del file prima della modifica nei sistemi di produzione.

1. Premere <kbd>G</kbd> per spostarsi sull'ultima riga del file.

1. Premere <kbd>I</kbd> per attivare la modalità di inserimento. La modalità dovrebbe essere indicata nella parte inferiore della schermata.

1. Premere <kbd>FINE</kbd> per spostarsi alla fine della riga. In alternativa, è possibile usare i tasti di direzione. Premere <kbd>INVIO</kbd> per spostarsi su una nuova riga.

1. Digitare la riga seguente nell'editor. I valori possono essere separati da spazi o tabulazioni. Per altre informazioni su ognuna delle colonne, vedere la documentazione:

    ```output
    UUID=<uuid-goes-here>    /uploads    ext4    defaults,nofail    1    2
    ```
1. Premere **ESC** e quindi digitare **:w!** per scrivere il file e **:q** per uscire dall'editor.

1. Infine, controllare per verificare che la voce sia corretta, chiedendo al sistema operativo di aggiornare i punti di montaggio:

    ```bash
    sudo mount -a
    ```

    Se viene restituito un errore, modificare il file per trovare il problema.

> [!TIP]
> Alcuni kernel di Linux supportano la funzionalità TRIM per rimuovere i blocchi inutilizzati sui dischi. Questa funzionalità è disponibile per i dischi di Azure e consente di risparmiare denaro se si creano file di grandi dimensioni che poi vengono eliminati. Vedere la documentazione per informazioni su come [attivare questa funzionalità](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure).

Ora che si è visto come è facile aggiungere dischi alle macchine virtuali, verranno esaminati più in dettaglio i tipi di dischi che è possibile creare. In particolare la scelta tra archiviazione Standard e Premium.