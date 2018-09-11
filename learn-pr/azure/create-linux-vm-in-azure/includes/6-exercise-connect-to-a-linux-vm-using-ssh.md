A questo punto la macchina virtuale Linux è stata distribuita ed è in esecuzione, ma non è configurata per eseguire alcuna operazione. Di seguito verrà descritto come connettersi alla macchina virtuale con SSH e configurare Apache, in modo da avere un server Web in esecuzione.

## <a name="connect-to-the-vm-with-ssh"></a>Connettersi alla macchina virtuale con SSH

Per connettersi a una macchina virtuale di Azure con un client SSH, è necessario:

- Il software client SSH (presente nella maggior parte dei sistemi operativi più recenti)
- L'indirizzo IP pubblico della macchina virtuale (o privato se la macchina virtuale è configurata per connettersi alla rete).

### <a name="get-the-public-ip-address"></a>Ottenere l'indirizzo IP pubblico

1. Nel [portale di Azure](https://portal.azure.com?azure-portal=true) verificare che il pannello **Panoramica** per la macchina virtuale creata in precedenza sia aperto. La macchina virtuale è disponibile in **Tutte le risorse** se è necessario aprirla. Il pannello di panoramica presenta numerose informazioni sulla macchina virtuale.

    - È possibile vedere se la macchina virtuale è in esecuzione.
    - Arrestarla o riavviarla.
    - Ottenere l'indirizzo IP pubblico per connettersi alla macchina virtuale.
    - Visualizzare l'attività di CPU, disco e rete.

1. Fare clic sul pulsante **Connetti** nella parte superiore del riquadro.

1. Nel pannello **Connect to virtual machine** (Connetti a macchina virtuale) notare le impostazioni **Indirizzo IP** e **Numero di porta**. Nella scheda **SSH** è anche disponibile il comando che è necessario eseguire in locale per connettersi alla macchina virtuale. Copiarlo negli Appunti.

<!-- TODO: this will be necessary if we ever have inline portal integration 

### Open the Azure Cloud Shell

Let's use the Cloud Shell in the Azure Portal. If you generated the SSH key locally, you need to use your local session since the private key won't be in your storage account.

1. Switch back to the **Dashboard** by clicking the Dashboard button in the Azure sidebar.

1. Open the Cloud Shell by clicking the shell button in the top toolbar.

    ![Open the Azure Cloud Shell](../media-drafts/6-cloud-shell.png)

1. Select **Bash** as the shell type. PowerShell is also available if you are a Windows administrator.

    ![Select bash shell in the portal](../media-drafts/6-use-bash-shell.png)

-->

## <a name="connect-with-ssh"></a>Connettersi tramite SSH

1. Incollare la riga di comando copiata dalla scheda SSH in Cloud Shell. Il risultato dovrebbe essere simile al seguente, anche se l'indirizzo IP sarà diverso e probabilmente anche il nome utente se non è stato usato il nome **jim**.

    ```bash
    ssh jim@137.117.101.249
    ```

1. Questo comando aprirà una connessione sicura alla shell e un prompt dei comandi della shell tradizionale per Linux.

1. Provare a eseguire alcuni comandi di Linux
    - `ls -la /` per visualizzare la radice del disco
    - `ps -l` per visualizzare tutti i processi in esecuzione
    - `dmesg` per elencare tutti i messaggi del kernel
    - `lsblk` per elencare tutti i dispositivi a blocchi: qui verranno visualizzate le unità

L'aspetto più interessante da osservare nell'elenco delle unità è cosa _manca_. Si noti che l'unità **Dati** (`sdc`) è presente ma non montata nel file system. Azure ha aggiunto un disco rigido virtuale, ma non l'ha inizializzato.

## <a name="initialize-data-disks"></a>Inizializzare i dischi dati

Tutte le unità aggiuntive create da zero dovranno essere inizializzate e formattate. Il processo per eseguire questa operazione è identico a quello valido per un disco fisico.

1. Prima di tutto identificare il disco. Questo passaggio è già stato eseguito. Si potrebbe anche usare `dmesg | grep SCSI` che elencherà tutti i messaggi dal kernel per i dispositivi SCSI.

1. Dopo aver identificato l'unità (`sdc`) è necessario inizializzarla ed è possibile usare `fdisk` a tale scopo. Sarà necessario eseguire il comando con `sudo` e specificare il disco da partizionare.

    ```bash
    sudo fdisk /dev/sdc
    ```
1. Usare il comando `n` per aggiungere una nuova partizione.  In questo esempio si sceglie p per una partizione primaria e si accettano gli altri valori predefiniti. L'output sarà simile all'esempio seguente:   

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

1. Stampare la tabella delle partizioni con il comando `p`. Dovrebbe essere visualizzata una schermata analoga alla seguente:

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
    
1. Scrivere le modifiche con il comando `w`. Si uscirà dallo strumento.

1. A questo punto è necessario scrivere un file system nella partizione con il comando `mkfs`. Sarà necessario specificare il tipo di file system e il nome del dispositivo ottenuti dall'output di `fdisk`.
    - Passare `-t ext4` per creare un file system _ext4_.
    - Il nome del dispositivo è `/dev/sdc`.

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    Il completamento del comando richiederà alcuni minuti.

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

1. Creare quindi una directory che verrà usata come punto di montaggio. Si supponga di avere una cartella `data`.

    ```bash
    sudo mkdir /data
    ```
1. Usare infine `mount` per collegare il disco al punto di montaggio.

    ```bash
    sudo mount /dev/sdc1 /data
    ```
    Dovrebbe essere ora possibile usare `lsblk` per vedere l'unità montata.
    
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

### <a name="mounting-the-drive-automatically"></a>Montare l'unità automaticamente

Per assicurarsi che l'unità venga montata automaticamente dopo un riavvio, è necessario aggiungerla al file `/etc/fstab`. È anche consigliabile usare l'UUID (Universally Unique Identifier, identificatore univoco universale) in `/etc/fstab` per fare riferimento all'unità invece di usare solo il nome del dispositivo, ad esempio `/dev/sdc1`. Se il sistema operativo rileva un errore del disco durante l'avvio, l'uso di UUID evita che venga montato il disco non corretto in una posizione specifica. Ai dischi dati rimanenti verrebbero quindi assegnati gli stessi ID di dispositivo. Per individuare l'UUID della nuova unità, usare l'utilità `blkid`:

```bash
sudo -i blkid
```

Il risultato sarà simile al seguente:

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. Copiare l'UUID per l'unità `/dev/sdc1` e aprire il file `/etc/fstab` in un editor di testo.

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> Se il file `/etc/fstab` non viene modificato in modo corretto, il sistema potrebbe non essere disponibile per l'avvio. In caso di dubbi, fare riferimento alla documentazione della distribuzione per informazioni su come modificare correttamente questo file. È anche consigliabile creare una copia di backup del file prima della modifica nei sistemi di produzione.

1. Premere **G** per spostarsi sull'ultima riga del file.

1. Premere **I** per attivare la modalità di inserimento. La modalità dovrebbe essere indicata nella parte inferiore della schermata.

1. Premere **FINE** per spostarsi alla fine della riga. In alternativa, è possibile usare i tasti di direzione. Premere **INVIO** per spostarsi su una nuova riga.

1. Digitare la riga seguente nell'editor. I valori possono essere separati da spazi o tabulazioni. Per altre informazioni su ognuna delle colonne, vedere la documentazione.

    ```output
    UUID=<uuid-goes-here>    /data    ext4    defaults,nofail    1    2
    ```
1. Premere **ESC** e quindi digitare **:w!** per scrivere il file e **:q** per uscire dall'editor.

1. Infine, controllare per verificare che la voce sia corretta, chiedendo al sistema operativo di aggiornare i punti di montaggio.

    ```bash
    sudo mount -a
    ```
    
    Se viene restituito un errore, modificare il file per individuare il problema.

> [!TIP]
> Alcuni kernel di Linux supportano la funzionalità TRIM per rimuovere i blocchi inutilizzati sui dischi. Questa funzionalità è disponibile per i dischi di Azure e consente di risparmiare denaro se si creano file di grandi dimensioni che poi vengono eliminati. Vedere la documentazione per informazioni su come [attivare questa funzionalità](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure).

## <a name="install-software-onto-the-vm"></a>Installare software nella macchina virtuale

Sono disponibili diverse opzioni per installare software nella macchina virtuale. Prima di tutto, come già accennato, è possibile usare `scp` per copiare i file locali dal computer alla macchina virtuale. Ciò consente di copiare i dati o le applicazioni personalizzate da eseguire.

È anche possibile installare software tramite Secure Shell. Le macchine di Azure sono connesse a Internet per impostazione predefinita. È possibile usare i comandi standard per installare i pacchetti software più diffusi direttamente dai repository standard. Questo è l'approccio che verrà usato per installare Apache.

### <a name="install-apache-web-server"></a>Installare il server Web Apache

Apache è disponibile all'interno dei repository software predefiniti di Ubuntu, quindi verrà installato usando gli strumenti di gestione dei pacchetti convenzionali.

1. Per iniziare, aggiornare l'indice dei pacchetti locali in modo che rispecchi le modifiche upstream più recenti.

    ```bash
    sudo apt-get update
    ```
    
1. Successivamente, installare Apache.

    ```bash
    sudo apt-get install apache2
    ```
    
1. L'operazione dovrebbe essere avviata automaticamente ed è possibile controllare lo stato usando `systemctl`:

    ```bash
    sudo systemctl status apache2
    ```

    Questo comando dovrebbe restituire informazioni simili alle seguenti:

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

1. Infine, è possibile provare a recuperare la pagina predefinita tramite l'indirizzo IP pubblico. Dovrebbe essere restituita una pagina predefinita.

    ![Pagina Web predefinita di Apache](../media-drafts/6-apache-works.png)

Come si può notare, SSH consente di usare una macchina virtuale Linux come un computer locale. È possibile amministrare questa macchina virtuale come qualsiasi altro computer Linux: installare software, configurare ruoli, mettere a punto le funzionalità ed eseguire altre attività quotidiane. Si tratta tuttavia di processi manuali. Se è necessario installare sempre alcuni software, valutare la possibilità di automatizzare il processo tramite scripting.
