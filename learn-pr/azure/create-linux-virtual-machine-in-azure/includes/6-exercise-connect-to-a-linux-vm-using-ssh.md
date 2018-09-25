A questo punto la macchina virtuale Linux è stata distribuita ed è in esecuzione, ma non è configurata per eseguire alcuna operazione. Di seguito verrà descritto come connettersi alla macchina virtuale con SSH e configurare Apache, in modo da avere un server Web in esecuzione.

## <a name="connect-to-the-vm-with-ssh"></a>Connettersi alla macchina virtuale con SSH

Per connettersi a una macchina virtuale di Azure con un client SSH, è necessario:

- Software client SSH (presente nella maggior parte dei sistemi operativi più recenti)
- L'indirizzo IP pubblico della macchina virtuale (o privato se la macchina virtuale è configurata per connettersi alla rete)

### <a name="get-the-public-ip-address"></a>Ottenere l'indirizzo IP pubblico

1. Nel [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) verificare che il pannello **Panoramica** per la macchina virtuale creata in precedenza sia aperto. La macchina virtuale è disponibile in **Tutte le risorse** se è necessario aprirla. Il pannello di panoramica presenta numerose informazioni sulla macchina virtuale.

    - È possibile vedere se la macchina virtuale è in esecuzione
    - Arrestarla o riavviarla
    - Ottenere l'indirizzo IP pubblico per connettersi alla macchina virtuale
    - Visualizzare l'attività di CPU, disco e rete

1. Fare clic sul pulsante **Connetti** nella parte superiore del riquadro.

1. Nel pannello **Connect to virtual machine** (Connetti a macchina virtuale) notare le impostazioni **Indirizzo IP** e **Numero di porta**. Nella scheda **SSH** è anche disponibile il comando che è necessario eseguire in locale per connettersi alla macchina virtuale. Copiarlo negli Appunti.

## <a name="connect-with-ssh"></a>Connettersi tramite SSH

1. Incollare la riga di comando copiata dalla scheda SSH in Azure Cloud Shell. Il risultato dovrebbe essere simile al seguente, anche se l'indirizzo IP sarà diverso e probabilmente anche il nome utente se non è stato usato il nome **jim**:

    ```bash
    ssh jim@137.117.101.249
    ```

1. Questo comando aprirà una connessione Secure Shell e un prompt dei comandi della shell tradizionale per Linux.

1. Provare a eseguire alcuni comandi di Linux
    - `ls -la /` per visualizzare la radice del disco
    - `ps -l` per visualizzare tutti i processi in esecuzione
    - `dmesg` per elencare tutti i messaggi del kernel
    - `lsblk` per elencare tutti i dispositivi a blocchi: qui verranno visualizzate le unità

L'aspetto più interessante da osservare nell'elenco delle unità è cosa _manca_. Si noti che l'unità **Dati** (`sdc`) è presente ma non montata nel file system. Azure ha aggiunto un disco rigido virtuale, ma non l'ha inizializzato.

## <a name="initialize-data-disks"></a>Inizializzare i dischi dati

Tutte le unità aggiuntive create da zero dovranno essere inizializzate e formattate. Il processo per eseguire questa operazione è identico a quello valido per un disco fisico:

1. Prima di tutto identificare il disco. Questo passaggio è già stato eseguito. Si potrebbe anche usare `dmesg | grep SCSI`, che elencherà tutti i messaggi dal kernel per i dispositivi SCSI.

1. Dopo aver identificato l'unità (`sdc`) è necessario inizializzarla ed è possibile usare `fdisk` a tale scopo. Sarà necessario eseguire il comando con `sudo` e specificare il disco da partizionare. Useremo il comando seguente per creare una nuova partizione primaria:

    ```bash
    (echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
    ```

1. A questo punto, è necessario scrivere un file system nella partizione con il comando `mkfs`.

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```

1. Infine, è necessario montare l'unità nel file system. Si supponga di avere una cartella `data`. Creiamo la cartella del punto di montaggio e montiamo l'unità.

    ```bash
    sudo mkdir /data & sudo mount /dev/sdc1 /data
    ```

    > [!TIP]
    > Il disco è stato inizializzato e montato. Per ulteriori dettagli su questo processo, consultare il modulo **Aggiungere e ridimensionare i dischi di macchine virtuali di Azure**. Questa attività viene trattata in maggior dettaglio in questo modulo.

## <a name="install-software-onto-the-vm"></a>Installare software nella macchina virtuale

Come si può notare, SSH consente di usare una macchina virtuale Linux come un computer locale. È possibile amministrare questa macchina virtuale come qualsiasi altro computer Linux: installare software, configurare ruoli, mettere a punto le funzionalità ed eseguire altre attività quotidiane. Concentriamoci sull'installazione di software.

Sono disponibili diverse opzioni per installare software nella macchina virtuale. Prima di tutto, come già accennato, è possibile usare `scp` per copiare i file locali dal computer alla macchina virtuale. Ciò consente di copiare i dati o le applicazioni personalizzate da eseguire.

È anche possibile installare software tramite Secure Shell. Le macchine di Azure sono connesse a Internet per impostazione predefinita. È possibile usare i comandi standard per installare i pacchetti software più diffusi direttamente dai repository standard. Questo è l'approccio che verrà usato per installare Apache.

### <a name="install-the-apache-web-server"></a>Installare il server Web Apache

Apache è disponibile all'interno dei repository software predefiniti di Ubuntu, quindi verrà installato usando gli strumenti di gestione dei pacchetti convenzionali:

1. Per iniziare, aggiornare l'indice dei pacchetti locali in modo che rispecchi le modifiche upstream più recenti:

    ```bash
    sudo apt-get update
    ```

1. Successivamente, installare Apache:

    ```bash
    sudo apt-get install apache2 -y
    ```

1. L'operazione dovrebbe essere avviata automaticamente ed è possibile controllare lo stato usando `systemctl`:

    ```bash
    sudo systemctl status apache2 --no-pager
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
    > [!NOTE]
    > Questi comandi sono di banale esecuzione, ma si tratta, tuttavia, di processi manuali. Se è necessario installare sempre alcuni software, valutare la possibilità di automatizzare il processo tramite scripting.

1. Infine, è possibile provare a recuperare la pagina predefinita tramite l'indirizzo IP pubblico. Tuttavia, anche se il server Web è in esecuzione nella macchina virtuale, non si otterrà una connessione o una risposta valida. Perché?

È necessario eseguire un altro passaggio per poter interagire con il server Web. La rete virtuale sta bloccando la richiesta in ingresso: si tratta del comportamento predefinito. È possibile cambiare questo comportamento tramite la configurazione. Questo aspetto verrà esaminato più avanti.