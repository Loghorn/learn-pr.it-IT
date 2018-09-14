A questo punto si dispone di un ambiente di sviluppo contenitori funzionante, è possibile esaminare i comandi da eseguire, elenco ed eliminare i contenitori.

## <a name="create-and-run-a-basic-container"></a>Creare ed eseguire un contenitore di base

Creare il primo contenitore con il comando seguente.

```bash
docker run alpine echo "Hello World"
```

Verrà visualizzato un output simile al seguente:

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

Il comando `docker run` crea un'istanza di un contenitore. In questo caso il contenitore è stato creato da un'immagine del contenitore denominata `alpine`, scaricata nel sistema locale. Dopo l'avvio del contenitore, il comando `echo "Hello World"` è stato eseguito nel contenitore e l'output è stato ripetuto nel terminale.

## <a name="get-container-images"></a>Ottenere le immagini dei contenitori

Un'immagine del contenitore sono inclusi il sistema operativo di base e tutti i processi aggiuntivi, applicazioni e configurazioni. Le immagini dei contenitori vengono archiviate in un registro immagini dei contenitori. Nell'esempio "Hello World", il *alpine* immagine è stato effettuato il pull da Docker Hub, ovvero un registro contenitori pubblici.

Eseguire il comando seguente per visualizzare un elenco di immagini scaricate nel sistema.

```bash
docker images
```

Se è stato eseguito il `docker run alpine` comando in precedenza, è possibile vedere immagini alpine elencati.

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

Per cercare un'immagine del contenitore, usare il comando `docker search`. È possibile usare l'esempio seguente per elencare tutte le immagini dei contenitori con `nginx` nel nome.

```bash
docker search nginx
```

L'output dovrebbe essere simile al seguente:

```output
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                                                  Official build of Nginx.                        9071                [OK]
jwilder/nginx-proxy                                    Automated Nginx reverse proxy for docker con…   1365                                    [OK]
richarvey/nginx-php-fpm                                Container running Nginx + PHP-FPM capable of…   593                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion                 LetsEncrypt container to use with nginx as p…   390                                     [OK]
kong                                                   Open-source Microservice & API Management la…   207                 [OK]
webdevops/php-nginx                                    Nginx with PHP-FPM                              106                                     [OK]
kitematic/hello-world-nginx                            A light-weight nginx container that demonstr…   102
zabbix/zabbix-web-nginx-mysql                          Zabbix frontend based on Nginx web-server wi…   60                                      [OK]
bitnami/nginx                                          Bitnami nginx Docker Image                      54                                      [OK]
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5   ubuntu-16-nginx-php-phpmyadmin-mysql-5          38                                      [OK]
linuxserver/nginx                                      An Nginx container, brought to you by LinuxS…   37
tobi312/rpi-nginx                                      NGINX on Raspberry Pi / armhf                   20                                      [OK]
nginxdemos/nginx-ingress                               NGINX Ingress Controller for Kubernetes . Th…   11
blacklabelops/nginx                                    Dockerized Nginx Reverse Proxy Server.          10                                      [OK]
wodby/drupal-nginx                                     Nginx for Drupal container image                9                                       [OK]
webdevops/nginx                                        Nginx container                                 8                                       [OK]
nginxdemos/hello                                       NGINX webserver that serves a simple page co…   7                                       [OK]
centos/nginx-18-centos7                                Platform for running nginx 1.8 or building n…   6
1science/nginx                                         Nginx Docker images that include Consul Temp…   4                                       [OK]
centos/nginx-112-centos7                               Platform for running nginx 1.12 or building …   3
pebbletech/nginx-proxy                                 nginx-proxy sets up a container running ngin…   2                                       [OK]
travix/nginx                                           NGinx reverse proxy                             1                                       [OK]
toccoag/openshift-nginx                                Nginx reverse proxy for Nice running on same…   1                                       [OK]
ansibleplaybookbundle/nginx-apb                        An APB to deploy NGINX                          0                                       [OK]
mailu/nginx                                            Mailu nginx frontend                            0                                       [OK]
```

Per scaricare in anticipo un'immagine prima di eseguirla, usare il comando `docker pull`. L'esempio seguente esegue il pull dell'immagine *nginx* nel sistema.

```bash
docker pull nginx
```

L'output dovrebbe essere simile al seguente:

```output
Using default tag: latest
latest: Pulling from library/nginx
be8881be8156: Pull complete
f2f27ed9664f: Extracting [===============>                                   ]  6.652MB/22.14MB
54ff137eb1b2: Download complete
```

Eseguire di nuovo `docker images` per elencare tutte le immagini nel sistema. Si noterà che l'immagine *nginx* è stata aggiunta al sistema.

```bash
docker images
```

L'output dovrebbe essere simile al seguente:

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              c82521676580        26 hours ago        109MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

## <a name="run-containers"></a>Eseguire i contenitori

Dopo aver identificato e scaricato l'immagine *nginx*, eseguire un contenitore dall'immagine. Quando si usa l'interfaccia della riga di comando di Docker per eseguire un'immagine del contenitore, usare il comando `docker run`.

Il `-d` argomento specifica che il contenitore verrà eseguito in modalità detached. In questa configurazione il contenitore viene eseguito un processo specificato. Se il processo si interrompe o si arresta in modo anomalo, viene arrestato anche il contenitore. Il `-p 8080:80` argomento specifica che il traffico di rete in arrivo alla porta 8080 nell'host del contenitore, il sistema di sviluppo in questo caso, viene inoltrato alla porta 80 del contenitore. L'argomento `ngingx`, infine, è il nome dell'immagine del contenitore da eseguire.

Per un elenco completo degli argomenti `docker run`, vedere le [informazioni di riferimento su docker run](https://docs.docker.com/engine/reference/run/).

```bash
docker run -d -p 8080:80 nginx
```

Questa operazione restituisce l'ID del contenitore completo.

```output
bd2424bfe7a5423d7d65efdf0b1622770d59e212db7b82862c3129fb630b5721
```

Elencare i contenitori in esecuzione nel sistema usando il comando `docker ps`.

```bash
docker ps
```

Verrà visualizzato un solo contenitore in esecuzione, ovvero il contenitore NGINX eseguito nell'ultimo passaggio. Si noti che il contenitore include un ID e un nome. Uno di questi due valori può essere usato per gestire il contenitore. Prendere nota dell'ID del contenitore. Questo valore verrà usato successivamente.

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   37 minutes ago      Up 37 minutes       0.0.0.0:8080->80/tcp   gallant_engelbart
```

Per testare il contenitore, aprire un browser e immettere `http://localhost:8080` per l'indirizzo. Al termine, si dovrebbe vedere la pagina web predefinita NGINX che contiene un messaggio di benvenuto.

## <a name="delete-containers"></a>Eliminare i contenitori

Si elimina un contenitore passando il nome o ID per il `docker rm` comando. Ripetere questa operazione con l'ID del contenitore del contenitore che esegue NGINX.

```bash
docker rm bd2424bfe7a5
```

Si noti che il contenitore non può essere rimosso perché è in esecuzione.

```output
Error response from daemon: You cannot remove a running container a31c5a5f2a8d6e420435bfcadbe158fa6a26ed29c005a892171505cc0c2861b2. Stop the container before attempting removal or force remove
```

Arrestare il contenitore con il comando `docker stop`.

```bash
docker stop bd2424bfe7a5
```

Si noti che, a questo punto, se si esegue `docker ps` per elencare tutti i contenitori, il contenitore nginx non è elencato.

```bash
docker ps
```

L'output dovrebbe essere simile al seguente:

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Per restituire un elenco di tutti i contenitori, inclusi i contenitori arrestati, aggiungere l'argomento `-a` al comando `docker ps`.

```bash
docker ps -a
```

L'output dovrebbe essere simile al seguente:

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   13 seconds ago      Exited (0) 3 seconds ago                       focused_spence
```

Provare a eseguire di nuovo l'operazione di eliminazione. Sostituire l'ID in questo esempio con l'ID dell'ambiente.

```bash
docker rm bd2424bfe7a5
```

Questa operazione restituisce l'ID del contenitore.

```output
bd2424bfe7a5
```

## <a name="delete-a-container-image"></a>Eliminare un'immagine del contenitore

Si elimina un'immagine contenitore usando il `docker rmi` comando. Se un contenitore (in esecuzione o arrestato) è stato avviato dall'immagine del contenitore, l'immagine non può essere eliminata. Prima è necessario rimuovere i contenitori. Aggiunta il `-f` argomento per il `docker rmi` comando forzerà la rimozione di tutti i relativi contenitori e verrà quindi rimossa l'immagine del contenitore.

Rimuovere l'immagine del contenitore NGINX con il comando seguente.

```bash
docker rmi nginx
```

L'output dovrebbe essere simile al seguente:

```output
Untagged: nginx:latest
Untagged: nginx@sha256:4a5573037f358b6cdfa2f3e8a9c33a5cf11bcd1675ca72ca76fbe5bd77d0d682
Deleted: sha256:8b89e48b5f157d9455c963b57c85d21e2337c58b8c983bc06f88476610adc129
Deleted: sha256:119ded3eca5e85ef43ee966e74564c604ccda064d955a8c5ed762e1d5e87f428
Deleted: sha256:6ece91c2763d826487e707f7b8ec063742ad0ee56cc9e605465cce95550c9a7f
Deleted: sha256:cdb3f9544e4c61d45da1ea44f7d92386639a052c620d1550376f22f5b46981af
```