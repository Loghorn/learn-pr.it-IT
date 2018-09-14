Prima di eseguire un contenitore o un'applicazione integrata nel contenitore in Azure, molto probabilmente si lavorerà in un ambiente di sviluppo locale, ad esempio il portatile. In questo caso, si preparerà il sistema per lo sviluppo dei contenitori con Docker.

## <a name="why-use-containers"></a>Perché usare i contenitori

I contenitori e le immagini dei contenitori usano in modo efficiente risorse host quali CPU, memoria e spazio su disco. Grazie a queste efficienze, i contenitori vengono avviati velocemente. In alcuni casi, l'avvio di una nuova istanza di un contenitore è quasi immediata. Ciò consente il provisioning rapido delle applicazioni e consente a un nuovo modello di operazioni di elaborazione e la scalabilità on demand.

Si immagini questo scenario: si esegue un servizio di elaborazione batch che in alcuni casi rileva un picco elevato nella domanda. I contenitori consentono di compilare un sistema che reagisce all'aumento della domanda eseguendo rapidamente il provisioning di nuove istanze. Si tratta di un sistema avanzato e non facile da realizzare con le macchine virtuali tradizionali.

I contenitori consentono anche di ottenere "density hyper". Ciò significa che è possibile eseguire altre applicazioni e processi con minori risorse fisiche o virtuali.

I contenitori non sono solo una piattaforma ideale per l'esecuzione di un carico di lavoro tradizionale, ad esempio i server Web, ma offrono anche nuove opportunità, come l'elaborazione batch con possibilità di burst, applicazioni compilate con un'architettura moderna e distribuita e qualsiasi elemento richieda la scalabilità on demand.

## <a name="docker-for-windows-and-mac"></a>Docker per Windows e Mac

Docker, Inc. ha pubblicato due applicazioni per installare e configurare gli ambienti di sviluppo contenitore locale. Le applicazioni di preparare il sistema con Docker degli strumenti, ad esempio gli strumenti di automazione e della riga di comando necessari. Viene anche creata una macchina virtuale che ospita la piattaforma Docker. L'ambiente è configurato in modo che i comandi di Docker vengano passati alla macchina virtuale. La funzionalità di ognuna di queste applicazioni è simile e include le funzioni seguenti:

- **Piattaforma docker:** i componenti di base necessari per creare ed eseguire i contenitori.
- **Docker CLI:** l'interfaccia della riga di comando per l'interazione con i contenitori Docker.
- **Docker Compose:** gli strumenti per definire ed eseguire applicazioni multicontenitore di automazione.

Aprire il collegamento appropriato seguente in una nuova scheda per installare Docker nel sistema operativo. 

- [Docker per Windows](https://www.docker.com/docker-windows)
- [Docker per Mac](https://www.docker.com/docker-mac)

## <a name="docker-for-windows-environments"></a>Docker per ambienti Windows

Quando si usa Docker per Windows, sono disponibili due ambienti: Linux e Windows. Se si usa l'ambiente Linux, è possibile eseguire i contenitori Linux nel sistema Windows. Per selezionare un ambiente, fare clic con il pulsante destro del mouse sull'icona della barra delle applicazioni di Docker, scegliere **Switch to Linux containers** (Passa a contenitori Linux) e seguire i prompt visualizzati.

> [!NOTE]
> I passaggi descritti in questa esercitazione presuppongono che il sistema sia configurato per funzionare con i contenitori Linux.

## <a name="docker-on-linux"></a>Docker in Linux

Non è attualmente alcuna applicazione di installazione per Linux. Se si lavora in un sistema basato su Linux, i componenti di server di Docker e gli strumenti dell'interfaccia della riga devono essere installati manualmente. Seguire le istruzioni disponibili nel [su Docker CE](https://docs.docker.com/install/#server) per la distribuzione Linux specifica.

## <a name="validate-configuration"></a>Convalidare la configurazione

Per convalidare l'installazione e la configurazione di Docker, aprire un terminale ed eseguire il comando seguente:

```bash
docker search nginx
```

Se viene visualizzato output simile al seguente, l'ambiente è pronto per l'unità successiva.

```output
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                                                  Official build of Nginx.                        9034                [OK]
jwilder/nginx-proxy                                    Automated Nginx reverse proxy for docker con…   1362                                    [OK]
richarvey/nginx-php-fpm                                Container running Nginx + PHP-FPM capable of…   589                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion                 LetsEncrypt container to use with nginx as p…   390                                     [OK]
kong                                                   Open-source Microservice & API Management la…   204                 [OK]
webdevops/php-nginx                                    Nginx with PHP-FPM                              106                                     [OK]
kitematic/hello-world-nginx                            A light-weight nginx container that demonstr…   102
zabbix/zabbix-web-nginx-mysql                          Zabbix frontend based on Nginx web-server wi…   59                                      [OK]
bitnami/nginx                                          Bitnami nginx Docker Image                      54                                      [OK]
linuxserver/nginx                                      An Nginx container, brought to you by LinuxS…   37
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5   ubuntu-16-nginx-php-phpmyadmin-mysql-5          36                                      [OK]
tobi312/rpi-nginx                                      NGINX on Raspberry Pi / armhf                   20                                      [OK]
nginxdemos/nginx-ingress                               NGINX Ingress Controller for Kubernetes . Th…   11
wodby/drupal-nginx                                     Nginx for Drupal container image                9                                       [OK]
blacklabelops/nginx                                    Dockerized Nginx Reverse Proxy Server.          9                                       [OK]
webdevops/nginx                                        Nginx container                                 8                                       [OK]
nginxdemos/hello                                       NGINX webserver that serves a simple page co…   7                                       [OK]
centos/nginx-18-centos7                                Platform for running nginx 1.8 or building n…   6
1science/nginx                                         Nginx Docker images that include Consul Temp…   4                                       [OK]
centos/nginx-112-centos7                               Platform for running nginx 1.12 or building …   3
pebbletech/nginx-proxy                                 nginx-proxy sets up a container running ngin…   2                                       [OK]
toccoag/openshift-nginx                                Nginx reverse proxy for Nice running on same…   1                                       [OK]
travix/nginx                                           NGinx reverse proxy                             1                                       [OK]
ansibleplaybookbundle/nginx-apb                        An APB to deploy NGINX                          0                                       [OK]
mailu/nginx                                            Mailu nginx frontend                            0                                       [OK]
```