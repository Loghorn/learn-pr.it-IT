Prima di descrivere alcuni modi comuni per eseguire, elencare ed eliminare i contenitori, si analizzerà una configurazione Nginx di base in esecuzione su un contenitore Docker ospitato in una macchina virtuale (VM) Ubuntu.

In questa parte si apprenderà come:

1. Creare una macchina virtuale di Linux configurata per l'installazione di Docker al primo avvio della macchina virtuale.
1. Connettersi alla macchina virtuale e avviare un contenitore Docker che esegue Nginx.
1. Usare il binding delle porte per rendere il server Web accessibile dall'esterno della macchina virtuale.

## <a name="what-are-containers"></a>Che cosa sono i contenitori?

Un contenitore è un ambiente di virtualizzazione, ma a differenza di una macchina virtuale, non include sempre un sistema operativo. Il contenitore fa invece riferimento al sistema operativo dell'ambiente host che esegue il contenitore stesso. Se ad esempio si eseguono cinque contenitori su un server con un kernel Linux specifico, tutti e cinque i contenitori sono in esecuzione sullo stesso kernel.

I contenitori consentono di creare un pacchetto con l'applicazione e tutti gli elementi necessari per la sua esecuzione in un elemento definito _immagine del contenitore_. I contenitori sono isolati, ovvero non interferiscono con altri contenitori in esecuzione nello stesso sistema. Le immagini del contenitore sono anche portabili. È possibile caricare le immagini in un registro contenitori, ad esempio nell'hub Docker o nel registro contenitori di Azure. È quindi possibile eseguire i contenitori nel cloud e aspettarsi che funzionino esattamente come nell'ambiente di sviluppo.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

I contenitori supportano l'iterazione rapida. Dato che non devono includere un sistema operativo, i contenitori sono in genere molto più piccoli delle macchine virtuali complete. Di conseguenza è possibile avviare e arrestare i contenitori rapidamente, spesso in pochi secondi.

## <a name="understand-the-setup"></a>Informazioni sulla configurazione

Ai fini dell'apprendimento, viene reso disponibile un ambiente sandbox in cui lavorare. Questo ambiente include Azure Cloud Shell, un'esperienza della riga di comando basata su browser per la gestione e lo sviluppo di risorse di Azure.

Da Cloud Shell si creerà una VM Linux che include Docker. Quindi ci si connetterà alla VM Linux tramite SSH per creare e usare i contenitori Docker in modo interattivo.

Si consideri questa VM Linux come un ambiente di sviluppo e uno spazio per apprendere il funzionamento dei contenitori. In pratica Docker viene installato nel computer che si usa per lo sviluppo delle app. Docker viene eseguito in Windows, macOS e Linux.

Alla fine di questo modulo vengono rese disponibili risorse per ottenere altre informazioni sull'esecuzione di Docker nell'ambiente di sviluppo locale.

## <a name="what-is-nginx"></a>Che cos'è Nginx?

Nginx (pronunciato "engine-x") è un server Web diffuso, gratuito e open source che può essere eseguito in Unix, Linux, macOS e Windows. La configurazione di un server Web è un buon metodo per visualizzare i contenitori in azione. In questo caso si userà Nginx per presentare una semplice pagina Web.

## <a name="what-is-cloud-init"></a>Che cos'è cloud-init?

Cloud-init consente di personalizzare una macchina virtuale Linux al primo avvio. Cloud-init consente di installare pacchetti e scrivere file o configurare utenti e impostazioni di sicurezza. Nel contesto corrente si userà uno script cloud-init per installare Docker nella macchina virtuale.

## <a name="what-is-port-binding"></a>Che cos'è il binding della porta?

Il binding della porta consente di inoltrare il traffico di rete a un contenitore dall'host corrispondente.

Un contenitore Docker viene eseguito in una rete locale nel sistema host del contenitore. In questo caso la rete del contenitore si trova nella VM Linux.

Le richieste Web vengono in genere eseguite sulla porta 80 (HTTP). Poiché il contenitore è in esecuzione in una rete locale all'interno della macchina virtuale, non è accessibile dall'esterno.

Docker consente di pubblicare (o esporre) una porta per rendere accessibile il contenitore dall'esterno della macchina virtuale. In questo caso si configura il contenitore in modo che il traffico di rete sulla porta 8080 della macchina virtuale venga inoltrato alla porta 80 del contenitore.

## <a name="create-a-virtual-machine-to-host-docker"></a>Creare una macchina virtuale per ospitare Docker

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="create-the-cloud-init-script"></a>Creare lo script cloud-init

1. Passare alla cartella **clouddrive**.

    ```azurecli
    cd clouddrive
    ```

1. Creare una nuova cartella denominata **vm-config**.

    ```azurecli
    mkdir vm-config
    ```

1. Passare alla nuova cartella.

    ```azurecli
    cd vm-config
    ```

1. Creare un nuovo file usando l'editor integrato di Cloud Shell.

    ```azurecli
    code .
    ```

1. Incollare questo script cloud-init nell'editor.

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```yaml
    #cloud-config
    package_upgrade: true
    write_files:
      - path: /etc/systemd/system/docker.service.d/docker.conf
        content: |
          [Service]
          ExecStart=
          ExecStart=/usr/bin/dockerd
      - path: /etc/docker/daemon.json
        content: |
          {
            "hosts": ["fd://","tcp://127.0.0.1:2375"]
          }
    runcmd:
      - curl -sSL https://get.docker.com/ | sh
      - usermod -aG docker azureuser
    ```

1. Salvare il file con il nome **docker-vm-config.txt** (<kbd>CTRL+S</kbd> o fare clic con il pulsante destro del mouse e selezionare **Salva**).

1. Chiudere l'editor (<kbd>CTRL+Q</kbd> o fare clic con il pulsante destro del mouse e selezionare **Esci**).

### <a name="create-the-vm"></a>Creare la VM

> [!IMPORTANT]
> In genere si crea un gruppo di risorse per tutte le risorse di Azure correlate con il comando `az group create`, ma per questi esercizi il gruppo di risorse è già stato creato. Usare "**<rgn>[nome gruppo di risorse sandbox]</rgn>**" come gruppo di risorse in tutti i passaggi.

1. Eseguire il comando `az vm create` per creare la macchina virtuale.

    ```azurecli
    az vm create \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys \
      --custom-data docker-vm-config.txt
    ```

L'argomento `--custom-data` specifica lo script cloud-init che installa Docker al primo avvio della macchina virtuale. Questa operazione richiede qualche minuto.

## <a name="connect-to-the-vm"></a>Connettersi alla macchina virtuale

Ora si creerà una connessione SSH alla macchina virtuale per poterla configurare.

1. Eseguire questo comando `az vm show` per ottenere l'indirizzo IP della VM.

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. Accedere tramite SSH alla macchina virtuale. Sostituire **ip-address** con il proprio indirizzo.

    ```bash
    ssh azureuser@ip-address
    ```

1. Verificare la versione di Docker.

    ```bash
    docker version
    ```

    > [!NOTE]
    > Se il comando non riesce o viene visualizzato un messaggio di errore relativo alla connessione al socket del daemon Docker, l'esecuzione dello script cloud-init non è stata ancora completata. È possibile attendere il completamento dello script, ma per il momento eseguire `exit` per uscire dalla sessione SSH, quindi riprovare la connessione dopo uno o due minuti.

## <a name="start-nginx"></a>Avviare Nginx

Ora si avvierà il contenitore Docker che esegue Nginx.

1. Eseguire questo commando `docker run` per avviare un contenitore Docker che esegue Nginx.

    ```bash
    docker run -d -p 8080:80 --name nginx nginx
    ```

    Il comando scarica un'immagine Docker preconfigurata con Nginx dall'[hub Docker](https://hub.docker.com/_/nginx?azure-portal=true).

    L'argomento `--name` assegna il nome "nginx" al contenitore Docker, in modo che sia possibile usarlo in un secondo momento.

    L'argomento `-p` inoltra il traffico di rete dalla porta 8080 della macchina virtuale alla porta 80 del contenitore.

1. Eseguire `curl` per verificare che Nginx sia in esecuzione e accessibile.

    ```bash
    curl http://localhost:8080
    ```

1. Chiudere la sessione SSH.

    ```bash
    exit
    ```

## <a name="access-your-web-server-from-a-web-browser"></a>Accedere al server Web da un Web browser

Ricordare che il contenitore è stato configurato in modo che il traffico di rete sulla porta 8080 venga inoltrato alla porta 80 del contenitore.

Per visualizzare il mapping delle porte in azione, ora si apre la porta 8080 per la macchina virtuale attraverso il firewall di Azure. Quindi si accede al server Web da un Web browser.

1. Eseguire questo comando `az vm open-port` per aprire la porta 8080 della macchina virtuale attraverso il firewall.

    ```azurecli
    az vm open-port \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --port 8080 \
      --priority 1001
    ```

1. Da un Web browser passare all'indirizzo IP della macchina virtuale. Assicurarsi di includere la porta 8080, ad esempio **http://40.121.106.164:8080**.

    Viene visualizzata la home page predefinita.

    ![Il sito Web in esecuzione in un browser](../media/2-nginx-browser.png)

## <a name="stop-the-container"></a>Arrestare il contenitore

A questo punto si arresta il contenitore.

1. In primo luogo accedere di nuovo alla macchina virtuale. Di seguito è riportato un rapido ripasso.

    Ottenere l'indirizzo IP.

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

    Accedere tramite SSH alla macchina virtuale. Sostituire **ip-address** con il proprio indirizzo.

    ```bash
    ssh azureuser@ip-address
    ```

1. Eseguire `docker stop nginx` per arrestare il contenitore con nome "nginx".

    ```bash
    docker stop nginx
    ```

Mantenere la connessione SSH aperta per la parte successiva.

La procedura è stata completata. È stata creata correttamente una macchina virtuale, che è stata usata per ospitare un server Web in contenitori Nginx.
