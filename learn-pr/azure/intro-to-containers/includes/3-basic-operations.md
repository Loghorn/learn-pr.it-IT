A questo punto si dispone di un ambiente di sviluppo contenitori funzionante ed è possibile esaminare alcuni modi comuni per eseguire, elencare ed eliminare i contenitori.

In questa unità si apprenderà a:

* Eseguire contenitori di base.
* Scaricare immagini del contenitore.
* Eliminare i contenitori e le immagini associate.

## <a name="whats-a-container-image"></a>Cos'è un'immagine del contenitore?

L'_immagine_ del contenitore include il sistema operativo di base ed eventuali altri processi, applicazioni e configurazioni. Proprio come una macchina virtuale, un _contenitore_ è un'istanza in esecuzione di un'immagine.

## <a name="connect-to-your-linux-vm"></a>Connettersi alla macchina virtuale Linux

Se ci si è disconnessi dalla sessione SSH creata nell'ultima parte, sarà necessario accedere. Di seguito è riportato un rapido ripasso.

1. Ottenere l'indirizzo IP.

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

## <a name="create-and-run-a-basic-container"></a>Creare ed eseguire un contenitore di base

Di seguito verrà eseguito un contenitore basato su Alpine Linux. Alpine Linux è molto diffuso per le sue dimensioni: le immagini del contenitore possono essere infatti di appena 5 MB.

Eseguire questo comando `docker run` per creare un contenitore basato su Alpine Linux.

```bash
docker run alpine echo "Hello World"
```

L'output sarà simile al seguente:

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

Il comando `docker run` crea un contenitore, esegue il comando specificato e quindi distrugge il contenitore.

Qui Docker scarica l'immagine [alpine](https://hub.docker.com/_/alpine?azure-portal=true) dall'hub Docker, avvia il contenitore e quindi stampa "Hello World" nella console.

In questo caso il comando `echo` viene eseguito brevemente e poi viene chiuso. Nella parte precedente Nginx viene eseguito in primo piano mantenendo quindi attivo il contenitore fino a quando non lo si arresta o non si arresta il servizio Nginx.

## <a name="get-container-images"></a>Ottenere le immagini dei contenitori

Le immagini dei contenitori vengono archiviate in un registro immagini dei contenitori. Nell'esempio precedente Docker effettua il pull dell'immagine **alpine** dall'hub Docker, il registro contenitori pubblico di Docker.

Eseguire `docker images` per visualizzare un elenco delle immagini scaricate nel sistema.

```bash
docker images
```

Verranno visualizzate due immagini &mdash; **alpine** e **nginx**.

```output
REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
alpine              latest              196d12cf6ab1        8 days ago4.41MB
nginx               latest              06144b287844        2 weeks ago109MB
```

Per cercare un'immagine del contenitore, usare il comando `docker search`. Ad esempio, eseguire il comando seguente per elencare tutte le immagini dei contenitori con `nginx` nel nome.

```bash
docker search nginx --limit 5
```

L'argomento `--limit` in questo esempio limita l'operazione di ricerca ai primi cinque risultati.

L'output sarà simile a questo.

```output
NAME                                     DESCRIPTION         STARS               OFFICIAL            AUTOMATED
nginx                                    Official build of Nginx.         9672                [OK]
jwilder/nginx-proxy                      Automated Nginx reverse proxy for docker con…   1415                                    [OK]
richarvey/nginx-php-fpm                  Container running Nginx + PHP-FPM capable of…   615                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion   LetsEncrypt container to use with nginx as p…   411                                     [OK]
bitnami/nginx                            Bitnami nginx Docker Image         58                                      [OK]
```

Comandi quali `docker run` scaricano automaticamente l'immagine se non è disponibile nel computer locale. Scaricare un'immagine prima di usarla è una pratica comune. In questo modo si è sicuri di avere sempre la versione più recente.

A questo scopo, usare il comando `docker pull`. Eseguire il comando seguente per effettuare il pull dell'immagine **nginx** più recente dall'hub Docker.

```bash
docker pull nginx
```

Questo esempio illustra che è disponibile la versione più recente dell'immagine **nginx**.

```output
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
Status: Image is up to date for nginx:latest
```

## <a name="run-and-list-a-container"></a>Eseguire ed elencare i contenitori

Nella parte precedente è stato usato il comando `docker run` per attivare Nginx. Il comando verrà ora eseguito una seconda volta per esaminarlo più attentamente.

1. Dalla connessione SSH, eseguire questo comando per attivare un contenitore che esegue Nginx.

    ```bash
    docker run -d -p 8080:80 nginx
    ```

    * L'argomento `-d` specifica che il contenitore verrà eseguito senza essere collegato, ovvero che verrà eseguito in background. Se il processo Nginx si interrompe o si arresta in modo anomalo, viene arrestato anche il contenitore.
    * L'argomento `-p` specifica che il traffico di rete in arrivo sulla porta 8080 nell'host del contenitore, in questo caso la macchina virtuale Linux, viene inoltrato alla porta 80 nel contenitore. Questo argomento è stato usato nella parte precedente per accedere al server Web dal browser.
    * L'argomento `nginx` è il nome dell'immagine del contenitore da eseguire.

    Più avanti sarà possibile esplorare l'elenco completo delle opzioni `docker run` nelle [informazioni di riferimento su docker run](https://docs.docker.com/engine/reference/run?azure-portal=true).

    Il comando `docker run` visualizza un identificatore univoco per il contenitore. Ad esempio:

    ```output
    9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466
    ```

1. Eseguire `docker ps` per elencare i contenitori in esecuzione nel sistema.

    ```bash
    docker ps
    ```

    Verrà visualizzato il contenitore Nginx appena avviato. Si noti che il contenitore ha un ID e un nome. È possibile usare uno di questi due valori per gestire il contenitore. Prendere nota dell'ID del contenitore. Verrà usato più avanti.

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS              PORTS                  NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   4 minutes ago     Up 4 minutes        0.0.0.0:8080->80/tcp   compassionate_allen
    ```

1. Come in precedenza, è possibile passare all'indirizzo IP della macchina virtuale in un browser per vedere il server Web in esecuzione. Assicurarsi di specificare la porta 8080 nell'URL.

    ![Il sito Web in esecuzione in un browser](../media/2-nginx-browser.png)

## <a name="delete-containers"></a>Eliminare i contenitori

Per eliminare un contenitore usare il comando `docker rm`. È possibile specificare il contenitore usando il nome o l'ID.

1. Provare a eseguire `docker rm` per eliminare il contenitore che esegue Nginx. Eseguire `docker ps` se si ha la necessità di reperire l'ID. Ecco un esempio.

    ```bash
    docker rm 9d7327e31212
    ```

    Si noti che il contenitore non può essere rimosso perché è in esecuzione.

    ```output
    Error response from daemon: You cannot remove a running container 9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466. Stop the container before attempting removal or force remove
    ```

1. Eseguire `docker stop` per arrestare il contenitore. Ecco un esempio.

    ```bash
    docker stop 9d7327e31212
    ```
    
1. Eseguire `docker ps` per verificare che il contenitore non sia più in esecuzione.

    ```bash
    docker ps
    ```

    Verrà visualizzato quanto segue.

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATEDSTATUS              PORTS               NAMES
    ```

1. Eseguire `docker ps` una seconda volta specificando il flag `-a`. Verranno elencati tutti i contenitori, anche quelli arrestati.

    ```bash
    docker ps -a
    ```

    L'output sarà simile al seguente:

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS                          PORTS               NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   11 minutes ago     Exited (0) About a minute ago                       compassionate_allen
    df8aeea0320f        alpine              "echo 'Hello World'"     About an hour ago   Exited (0) About an hour ago                        objective_payne
    3a7efb75c288        nginx               "nginx -g 'daemon of…"   About an hour ago   Exited (0) About an hour ago                        nginx
    ```

    L'output include il contenitore Nginx appena arrestato e gli altri contenitori eseguiti in precedenza.

1. Eseguire `docker rm` una seconda volta. Sostituire l'ID visualizzato con uno dei propri.

    ```bash
    docker rm 9d7327e31212
    ```

1. Eseguire il comando `docker rm` seguente per eliminare _tutti_ i contenitori attivi.

    ```bash
    docker rm $(docker ps -aq)
    ```

1. Eseguire di nuovo `docker ps -a`. Si noterà che non sono presenti contenitori attivi in esecuzione o arrestati.

    ```bash
    docker ps -a
    ```

## <a name="delete-a-container-image"></a>Eliminare un'immagine del contenitore

Per eliminare un'immagine del contenitore usare il comando `docker rmi`.

È possibile eliminare un'immagine solo se nessun contenitore avviato dall'immagine, in esecuzione o arrestato, è attivo. L'argomento `-f`, tuttavia, forza la rimozione di tutti i contenitori associati e quindi l'immagine del contenitore viene rimossa.

1. Eseguire `docker rmi nginx` per rimuovere l'immagine Nginx dalla macchina virtuale Linux.

    ```bash
    docker rmi nginx
    ```

    L'output visualizzato sarà simile al seguente:

    ```output
    Untagged: nginx:latest
    Untagged: nginx@sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
    Deleted: sha256:06144b2878448774e55577ae7d66b5f43a87c2e44322b3884e4e6c70d070b262
    Deleted: sha256:824a442ee3c96744d75be3737a22cc6a47aebad1b30be67f3c2f8f29cb0aa879
    Deleted: sha256:8e3d1e9e4945f930c93c30617512998437f6edafd86676770d29b1581f2520bb
    Deleted: sha256:8b15606a9e3e430cb7ba739fde2fbb3734a19f8a59a825ffa877f9be49059817
    ```

1. Eseguire `docker images` per elencare le immagini presenti nella macchina virtuale. 

    ```bash
    docker images
    ```

    Verrà visualizzata l'immagine **alpine**.

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
    alpine              latest              196d12cf6ab1        8 days ago4.41MB
    ```

1. Eseguire il comando `docker rmi` seguente per eliminare _tutte_ le immagini dalla macchina virtuale.

    ```bash
    docker rmi $(docker images -q)
    ```

1. Eseguire di nuovo `docker images`. Si noterà che non sono presenti immagini nella macchina virtuale.

    ```bash
    docker images
    ```
