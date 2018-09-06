Prima di eseguire un contenitore o un'applicazione integrata nel contenitore in Azure, molto probabilmente si lavorerà in un ambiente di sviluppo locale, ad esempio il portatile. Questa unità consente di preparare il sistema per lo sviluppo di contenitori.

## <a name="docker-for-windows-and-mac"></a>Docker per Windows e Mac

Docker, Inc. ha pubblicato due applicazioni per installare e configurare un ambiente di sviluppo locale per il contenitore. Essenzialmente, ogni applicazione prepara il sistema con gli strumenti di Docker, ad esempio gli strumenti di automazione e dell'interfaccia della riga di comando necessari. Viene anche creata una macchina virtuale che ospita la piattaforma Docker. L'ambiente è configurato in modo che i comandi di Docker vengano passati alla macchina virtuale. La funzionalità di ognuna di queste applicazioni è simile e include le funzioni seguenti:

- Piattaforma Docker: componenti di base necessari per creare ed eseguire i contenitori.
- Interfaccia della riga di comando di Docker: interfaccia della riga di comando per l'interazione con i contenitori Docker
- Docker Compose: strumenti di automazione per definire ed eseguire applicazioni multicontenitore.

Per installare Docker nel sistema, fare clic sul collegamento che corrisponde al sistema operativo.

- Docker per Windows: https://www.docker.com/docker-windows
- Docker per Mac: https://www.docker.com/docker-mac

## <a name="docker-for-windows-environments"></a>Docker per ambienti Windows

Quando si usa Docker per Windows, sono disponibili due ambienti: Linux e Windows. Se si usa l'ambiente Linux, è possibile eseguire i contenitori Linux nel sistema Windows. Per selezionare un ambiente, fare clic con il pulsante destro del mouse sull'icona della barra delle applicazioni di Docker, scegliere **Switch to Linux containers** (Passa a contenitori Linux) e seguire i prompt visualizzati.

![Docker per Windows, passaggio ai contenitori Linux](../media-draft/2-docker-linux.png)

> [!NOTE]
> I passaggi descritti in questa esercitazione presuppongono che il sistema sia configurato per funzionare con i contenitori Linux.

## <a name="docker-on-linux"></a>Docker in Linux

Se si usa un sistema basato su Linux, i componenti server e gli strumenti dell'interfaccia della riga di comando di Docker possono essere installati manualmente. Seguire le istruzioni illustrate in [About Docker CE](https://docs.docker.com/install/#server) (Informazioni su Docker CE) per la distribuzione di Linux specifica.

## <a name="validate-configuration"></a>Convalidare la configurazione

Per convalidare l'installazione e la configurazione di Docker, aprire un terminale ed eseguire il comando seguente:

```bash
docker search nginx
```

Se viene visualizzato un output simile al seguente, l'ambiente è pronto per l'unità successiva.

```bash
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

## <a name="summary"></a>Riepilogo

In questa unità è stato preparato un ambiente di sviluppo del contenitore locale. Nell'unità successiva verranno fornite informazioni su alcune operazioni di Docker di base.