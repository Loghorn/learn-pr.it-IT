<span data-ttu-id="6134e-101">Prima di eseguire un contenitore o un'applicazione integrata nel contenitore in Azure, molto probabilmente si lavorerà in un ambiente di sviluppo locale, ad esempio il portatile.</span><span class="sxs-lookup"><span data-stu-id="6134e-101">Before you run a container or container-integrated application in Azure, you'll most likely work in a local development environment like your laptop.</span></span> <span data-ttu-id="6134e-102">Questa unità consente di preparare il sistema per lo sviluppo di contenitori.</span><span class="sxs-lookup"><span data-stu-id="6134e-102">This unit helps you prepare your system for container development.</span></span>

## <a name="docker-for-windows-and-mac"></a><span data-ttu-id="6134e-103">Docker per Windows e Mac</span><span class="sxs-lookup"><span data-stu-id="6134e-103">Docker for Windows and Mac</span></span>

<span data-ttu-id="6134e-104">Docker, Inc. ha pubblicato due applicazioni per installare e configurare un ambiente di sviluppo locale per il contenitore.</span><span class="sxs-lookup"><span data-stu-id="6134e-104">Docker, Inc. has published two applications to install and configure a local container development environment.</span></span> <span data-ttu-id="6134e-105">Essenzialmente, ogni applicazione prepara il sistema con gli strumenti di Docker, ad esempio gli strumenti di automazione e dell'interfaccia della riga di comando necessari.</span><span class="sxs-lookup"><span data-stu-id="6134e-105">Essentially, each application prepares your system with Docker tooling, such as the necessary CLI and automation tools.</span></span> <span data-ttu-id="6134e-106">Viene anche creata una macchina virtuale che ospita la piattaforma Docker.</span><span class="sxs-lookup"><span data-stu-id="6134e-106">A virtual machine is also created that hosts the Docker platform.</span></span> <span data-ttu-id="6134e-107">L'ambiente è configurato in modo che i comandi di Docker vengano passati alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="6134e-107">The environment is configured such that Docker commands are passed through to the virtual machine.</span></span> <span data-ttu-id="6134e-108">La funzionalità di ognuna di queste applicazioni è simile e include le funzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6134e-108">Each of these applications is similar in functionality and includes the following features:</span></span>

- <span data-ttu-id="6134e-109">Piattaforma Docker: componenti di base necessari per creare ed eseguire i contenitori.</span><span class="sxs-lookup"><span data-stu-id="6134e-109">Docker platform - The core components necessary to create and run containers.</span></span>
- <span data-ttu-id="6134e-110">Interfaccia della riga di comando di Docker: interfaccia della riga di comando per l'interazione con i contenitori Docker</span><span class="sxs-lookup"><span data-stu-id="6134e-110">Docker CLI - The command-line interface for interacting with Docker containers</span></span>
- <span data-ttu-id="6134e-111">Docker Compose: strumenti di automazione per definire ed eseguire applicazioni multicontenitore.</span><span class="sxs-lookup"><span data-stu-id="6134e-111">Docker Compose - Automation tooling for defining and running multi-container applications.</span></span>

<span data-ttu-id="6134e-112">Per installare Docker nel sistema, fare clic sul collegamento che corrisponde al sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="6134e-112">To install Docker on your system, follow the link that matches your operating system.</span></span>

- <span data-ttu-id="6134e-113">Docker per Windows: https://www.docker.com/docker-windows</span><span class="sxs-lookup"><span data-stu-id="6134e-113">Docker for Windows - https://www.docker.com/docker-windows</span></span>
- <span data-ttu-id="6134e-114">Docker per Mac: https://www.docker.com/docker-mac</span><span class="sxs-lookup"><span data-stu-id="6134e-114">Docker for Mac - https://www.docker.com/docker-mac</span></span>

## <a name="docker-for-windows-environments"></a><span data-ttu-id="6134e-115">Docker per ambienti Windows</span><span class="sxs-lookup"><span data-stu-id="6134e-115">Docker for Windows environments</span></span>

<span data-ttu-id="6134e-116">Quando si usa Docker per Windows, sono disponibili due ambienti: Linux e Windows.</span><span class="sxs-lookup"><span data-stu-id="6134e-116">When you use Docker for Windows, two environments are available: Linux and Windows.</span></span> <span data-ttu-id="6134e-117">Se si usa l'ambiente Linux, è possibile eseguire i contenitori Linux nel sistema Windows.</span><span class="sxs-lookup"><span data-stu-id="6134e-117">Using the Linux environment allows you to run Linux containers on your Windows system.</span></span> <span data-ttu-id="6134e-118">Per selezionare un ambiente, fare clic con il pulsante destro del mouse sull'icona della barra delle applicazioni di Docker, scegliere **Switch to Linux containers** (Passa a contenitori Linux) e seguire i prompt visualizzati.</span><span class="sxs-lookup"><span data-stu-id="6134e-118">You can select an environment by right-clicking on the Docker task bar icon, selecting **Switch to Linux containers**, and following the on-screen prompts.</span></span>

![Docker per Windows, passaggio ai contenitori Linux](../media-draft/2-docker-linux.png)

> [!NOTE]
> <span data-ttu-id="6134e-120">I passaggi descritti in questa esercitazione presuppongono che il sistema sia configurato per funzionare con i contenitori Linux.</span><span class="sxs-lookup"><span data-stu-id="6134e-120">The steps in this tutorial assume that your system is configured to work with Linux containers.</span></span>

## <a name="docker-on-linux"></a><span data-ttu-id="6134e-121">Docker in Linux</span><span class="sxs-lookup"><span data-stu-id="6134e-121">Docker on Linux</span></span>

<span data-ttu-id="6134e-122">Se si usa un sistema basato su Linux, i componenti server e gli strumenti dell'interfaccia della riga di comando di Docker possono essere installati manualmente.</span><span class="sxs-lookup"><span data-stu-id="6134e-122">If you're working on a Linux-based system, the Docker server components and CLI tools can be manually installed.</span></span> <span data-ttu-id="6134e-123">Seguire le istruzioni illustrate in [About Docker CE](https://docs.docker.com/install/#server) (Informazioni su Docker CE) per la distribuzione di Linux specifica.</span><span class="sxs-lookup"><span data-stu-id="6134e-123">Follow the instructions found on [About Dokcer CE](https://docs.docker.com/install/#server) for your specific Linux distribution.</span></span>

## <a name="validate-configuration"></a><span data-ttu-id="6134e-124">Convalidare la configurazione</span><span class="sxs-lookup"><span data-stu-id="6134e-124">Validate configuration</span></span>

<span data-ttu-id="6134e-125">Per convalidare l'installazione e la configurazione di Docker, aprire un terminale ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="6134e-125">To validate that Docker has been successfully installed and configured, open a terminal and run the following command:</span></span>

```bash
docker search nginx
```

<span data-ttu-id="6134e-126">Se viene visualizzato un output simile al seguente, l'ambiente è pronto per l'unità successiva.</span><span class="sxs-lookup"><span data-stu-id="6134e-126">If you see output similar to the following, your environment is read for the next unit.</span></span>

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

## <a name="summary"></a><span data-ttu-id="6134e-127">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="6134e-127">Summary</span></span>

<span data-ttu-id="6134e-128">In questa unità è stato preparato un ambiente di sviluppo del contenitore locale.</span><span class="sxs-lookup"><span data-stu-id="6134e-128">In this unit, you prepared a local container development environment.</span></span> <span data-ttu-id="6134e-129">Nell'unità successiva verranno fornite informazioni su alcune operazioni di Docker di base.</span><span class="sxs-lookup"><span data-stu-id="6134e-129">In the next unit, you will learn about some basic Docker operations.</span></span>