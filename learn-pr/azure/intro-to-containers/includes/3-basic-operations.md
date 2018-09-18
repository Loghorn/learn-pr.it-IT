<span data-ttu-id="83d2b-101">Dopo aver creato un ambiente di sviluppo contenitori funzionante, verranno brevemente illustrate alcune operazioni di base sui contenitori.</span><span class="sxs-lookup"><span data-stu-id="83d2b-101">Now that you have a functioning container development environment, let's take a quick spin through some basic container operations.</span></span> <span data-ttu-id="83d2b-102">Questa unità non include un elenco completo delle funzionalità di Docker.</span><span class="sxs-lookup"><span data-stu-id="83d2b-102">This unit doesn't include a complete list of Docker capabilities (not even close).</span></span> <span data-ttu-id="83d2b-103">Questa unità descrive come eseguire, elencare ed eliminare i contenitori.</span><span class="sxs-lookup"><span data-stu-id="83d2b-103">This unit will prepare you to run, list, and delete containers.</span></span> <span data-ttu-id="83d2b-104">Nel resto di questo modulo saranno fornite informazioni aggiuntive sulle operazioni sui contenitori.</span><span class="sxs-lookup"><span data-stu-id="83d2b-104">Throughout the remainder of this module, you will gain additional exposure to container operations.</span></span>

## <a name="run-a-basic-container"></a><span data-ttu-id="83d2b-105">Eseguire un contenitore di base</span><span class="sxs-lookup"><span data-stu-id="83d2b-105">Run a basic container</span></span>

<span data-ttu-id="83d2b-106">Prima di esaminare in dettaglio l'esecuzione e la gestione dei contenitori, verrà brevemente illustrato quanto sia facile eseguire un contenitore.</span><span class="sxs-lookup"><span data-stu-id="83d2b-106">Before digging into the details of running and managing containers, let's quickly see just how easy it is to run a container.</span></span>

<span data-ttu-id="83d2b-107">Creare il primo contenitore con il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="83d2b-107">Create your first container with the following command.</span></span>

```bash
docker run alpine echo "Hello World"
```

<span data-ttu-id="83d2b-108">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83d2b-108">You should see output similar to the following:</span></span>

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

<span data-ttu-id="83d2b-109">Il comando `docker run` crea un'istanza di un contenitore.</span><span class="sxs-lookup"><span data-stu-id="83d2b-109">The `docker run` command creates an instance of a container.</span></span> <span data-ttu-id="83d2b-110">In questo caso il contenitore è stato creato da un'immagine del contenitore denominata `alpine`, scaricata nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="83d2b-110">In this case, the container was created from a container image named `alpine`, which was downloaded to your local system.</span></span> <span data-ttu-id="83d2b-111">Dopo l'avvio del contenitore, il comando `echo "Hello World"` è stato eseguito nel contenitore e l'output è stato ripetuto nel terminale.</span><span class="sxs-lookup"><span data-stu-id="83d2b-111">After the container started, the `echo "Hello World"` command was run inside of the container and the output echoed to your terminal.</span></span>

<span data-ttu-id="83d2b-112">A questo punto, i dettagli tecnici di ognuna di queste azioni non sono importanti.</span><span class="sxs-lookup"><span data-stu-id="83d2b-112">At this point, don't worry about the technical details of each of these actions.</span></span> <span data-ttu-id="83d2b-113">Verranno forniti in dettaglio più avanti in questo modulo.</span><span class="sxs-lookup"><span data-stu-id="83d2b-113">They will be detailed throughout this module.</span></span>

## <a name="get-container-images"></a><span data-ttu-id="83d2b-114">Ottenere le immagini dei contenitori</span><span class="sxs-lookup"><span data-stu-id="83d2b-114">Get container images</span></span>

<span data-ttu-id="83d2b-115">Come illustrato nell'esempio "Hello World", i contenitori vengono eseguiti da un'immagine del contenitore.</span><span class="sxs-lookup"><span data-stu-id="83d2b-115">As you saw in the 'Hello World' example, containers are run from a container image.</span></span> <span data-ttu-id="83d2b-116">Tale immagine include il sistema operativo di base dei contenitori ed eventuali altri processi, applicazioni e configurazioni.</span><span class="sxs-lookup"><span data-stu-id="83d2b-116">These images include the container base operating system and any additional processes, applications, and configurations.</span></span> <span data-ttu-id="83d2b-117">Le immagini dei contenitori vengono archiviate in un registro immagini dei contenitori.</span><span class="sxs-lookup"><span data-stu-id="83d2b-117">Container images are stored in a container image registry.</span></span> <span data-ttu-id="83d2b-118">Nell'esempio "Hello World" è stato eseguito il pull dell'immagine *alpine* da Docker Hub, che è un registro contenitori pubblico.</span><span class="sxs-lookup"><span data-stu-id="83d2b-118">In the 'Hello World' example, the *alpine* image was pulled from Docker Hub, which is a public container registry.</span></span>

<span data-ttu-id="83d2b-119">Verrà ora illustrato come cercare e scaricare un'immagine del contenitore già pronta.</span><span class="sxs-lookup"><span data-stu-id="83d2b-119">Let's see how to search for and download a pre-created container image.</span></span>

<span data-ttu-id="83d2b-120">Eseguire il comando seguente per visualizzare un elenco di immagini scaricate nel sistema.</span><span class="sxs-lookup"><span data-stu-id="83d2b-120">Run the following command to see a list of images that have been downloaded to your system.</span></span>

```bash
docker images
```

<span data-ttu-id="83d2b-121">Se sono state seguite le istruzioni fino a questo punto, dovrebbe essere visualizzata l'immagine alpine.</span><span class="sxs-lookup"><span data-stu-id="83d2b-121">If you've been following along, you should see the alpine image.</span></span> <span data-ttu-id="83d2b-122">Questa immagine è stata scaricata quando è stato eseguito l'esempio "Hello World".</span><span class="sxs-lookup"><span data-stu-id="83d2b-122">This image was downloaded when the 'Hello World' example was run.</span></span>

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

<span data-ttu-id="83d2b-123">Per cercare un'immagine del contenitore, usare il comando `docker search`.</span><span class="sxs-lookup"><span data-stu-id="83d2b-123">To search for a container image, use the `docker search` command.</span></span> <span data-ttu-id="83d2b-124">È possibile usare l'esempio seguente per elencare tutte le immagini dei contenitori con `nginx` nel nome.</span><span class="sxs-lookup"><span data-stu-id="83d2b-124">For instance, use the following example to list all container images that include `nginx` in the name.</span></span>

```bash
docker search nginx
```

<span data-ttu-id="83d2b-125">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83d2b-125">The output should look similar to the following:</span></span>

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

<span data-ttu-id="83d2b-126">Per scaricare in anticipo un'immagine prima di eseguirla, usare il comando `docker pull`.</span><span class="sxs-lookup"><span data-stu-id="83d2b-126">If you'd like to pre-download an image prior to running it, use the `docker pull` command.</span></span> <span data-ttu-id="83d2b-127">L'esempio seguente esegue il pull dell'immagine *nginx* nel sistema.</span><span class="sxs-lookup"><span data-stu-id="83d2b-127">The following example pulls the *nginx* image to your system.</span></span>

```bash
docker pull nginx
```

<span data-ttu-id="83d2b-128">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83d2b-128">The output should look similar to the following:</span></span>

```output
Using default tag: latest
latest: Pulling from library/nginx
be8881be8156: Pull complete
f2f27ed9664f: Extracting [===============>                                   ]  6.652MB/22.14MB
54ff137eb1b2: Download complete
```

<span data-ttu-id="83d2b-129">Eseguire di nuovo `docker images` per elencare tutte le immagini nel sistema.</span><span class="sxs-lookup"><span data-stu-id="83d2b-129">Run `docker images` again to list all of the images on your system.</span></span> <span data-ttu-id="83d2b-130">Si noterà che l'immagine *nginx* è stata aggiunta al sistema.</span><span class="sxs-lookup"><span data-stu-id="83d2b-130">You'll see that the *nginx* image has been added to your system.</span></span>

```bash
docker images
```

<span data-ttu-id="83d2b-131">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83d2b-131">The output should look similar to the following:</span></span>

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              c82521676580        26 hours ago        109MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

## <a name="run-containers"></a><span data-ttu-id="83d2b-132">Eseguire i contenitori</span><span class="sxs-lookup"><span data-stu-id="83d2b-132">Run containers</span></span>

<span data-ttu-id="83d2b-133">Dopo aver identificato e scaricato l'immagine *nginx*, eseguire un contenitore dall'immagine.</span><span class="sxs-lookup"><span data-stu-id="83d2b-133">Now that you've identified and downloaded the *nginx* image, run a container from the image.</span></span> <span data-ttu-id="83d2b-134">Quando si usa l'interfaccia della riga di comando di Docker per eseguire un'immagine del contenitore, usare il comando `docker run`.</span><span class="sxs-lookup"><span data-stu-id="83d2b-134">When using the Docker CLI to run a container image, use the `docker run` command.</span></span>

<span data-ttu-id="83d2b-135">Nell'esempio seguente l'argomento `-d` specifica che il contenitore verrà eseguito senza essere collegato.</span><span class="sxs-lookup"><span data-stu-id="83d2b-135">In the following example, the `-d` argument specifies that the container will run in a detached mode.</span></span> <span data-ttu-id="83d2b-136">In questa configurazione il contenitore viene eseguito un processo specificato.</span><span class="sxs-lookup"><span data-stu-id="83d2b-136">In this configuration, the container runs a specified process.</span></span> <span data-ttu-id="83d2b-137">Se il processo si interrompe o si arresta in modo anomalo, viene arrestato anche il contenitore.</span><span class="sxs-lookup"><span data-stu-id="83d2b-137">If that process stops or crashes, the container itself is stopped.</span></span> <span data-ttu-id="83d2b-138">L'argomento `-p 8080:80` specifica che il traffico di rete in arrivo alla porta 8080 nell'host del contenitore, in questo caso il sistema di sviluppo, viene inoltrato alla porta 80 del contenitore.</span><span class="sxs-lookup"><span data-stu-id="83d2b-138">The `-p 8080:80` argument specifies that network traffic arriving to port 8080 on the container host, your development system in this case, is forwarded to port 80 of the container.</span></span> <span data-ttu-id="83d2b-139">L'argomento `ngingx`, infine, è il nome dell'immagine del contenitore da eseguire.</span><span class="sxs-lookup"><span data-stu-id="83d2b-139">Finally, the `ngingx` argument is the name of the container image to run.</span></span>

<span data-ttu-id="83d2b-140">Per un elenco completo degli argomenti `docker run`, vedere le [informazioni di riferimento su docker run](https://docs.docker.com/engine/reference/run/).</span><span class="sxs-lookup"><span data-stu-id="83d2b-140">For a complete list of `docker run` arguments, see the [docker run reference](https://docs.docker.com/engine/reference/run/).</span></span>

```bash
docker run -d -p 8080:80 nginx
```

<span data-ttu-id="83d2b-141">Questa operazione restituisce l'ID del contenitore completo.</span><span class="sxs-lookup"><span data-stu-id="83d2b-141">This operation returns the full container ID.</span></span>

```output
bd2424bfe7a5423d7d65efdf0b1622770d59e212db7b82862c3129fb630b5721
```

<span data-ttu-id="83d2b-142">Elencare i contenitori in esecuzione nel sistema usando il comando `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="83d2b-142">List the running containers on your system using the `docker ps` command.</span></span>

```bash
docker ps
```

<span data-ttu-id="83d2b-143">Verrà visualizzato un solo contenitore in esecuzione, ovvero il contenitore NGINX eseguito nell'ultimo passaggio.</span><span class="sxs-lookup"><span data-stu-id="83d2b-143">You should see a single running container, which is the NGINX container run in the last step.</span></span> <span data-ttu-id="83d2b-144">Si noti che il contenitore include un ID e un nome.</span><span class="sxs-lookup"><span data-stu-id="83d2b-144">Notice that the container has both an ID and a Name.</span></span> <span data-ttu-id="83d2b-145">Uno di questi due valori può essere usato per gestire il contenitore.</span><span class="sxs-lookup"><span data-stu-id="83d2b-145">Either one of these values can be used to manage the container.</span></span> <span data-ttu-id="83d2b-146">Prendere nota dell'ID del contenitore.</span><span class="sxs-lookup"><span data-stu-id="83d2b-146">Take note of the container ID.</span></span> <span data-ttu-id="83d2b-147">Questo valore verrà usato più avanti nell'unità.</span><span class="sxs-lookup"><span data-stu-id="83d2b-147">This value will be used later in the unit.</span></span>

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   37 minutes ago      Up 37 minutes       0.0.0.0:8080->80/tcp   gallant_engelbart
```

<span data-ttu-id="83d2b-148">Per testare il contenitore, aprire un browser e immettere http://localhost:8080 come indirizzo.</span><span class="sxs-lookup"><span data-stu-id="83d2b-148">To test the container, open a browser and enter http://localhost:8080 for the address.</span></span> <span data-ttu-id="83d2b-149">Al termine, verrà visualizzato il sito Web predefinito NGINX.</span><span class="sxs-lookup"><span data-stu-id="83d2b-149">After it completes, you should see the NGINX default website.</span></span>

![Microsoft Edge con la schermata iniziale di NGINX](../media-draft/3-nginx.png)

## <a name="delete-containers"></a><span data-ttu-id="83d2b-151">Eliminare i contenitori</span><span class="sxs-lookup"><span data-stu-id="83d2b-151">Delete containers</span></span>

<span data-ttu-id="83d2b-152">Quando un contenitore non è più necessario, può essere eliminato specificando il nome o l'ID del contenitore nel comando `docker rm`.</span><span class="sxs-lookup"><span data-stu-id="83d2b-152">When you're done working with a container, it can be deleted by providing the container name or ID to the `docker rm` command.</span></span> <span data-ttu-id="83d2b-153">Provare questa operazione con l'ID del contenitore che esegue NGINX.</span><span class="sxs-lookup"><span data-stu-id="83d2b-153">Try out this operation with the container ID of the container running NGINX.</span></span> <span data-ttu-id="83d2b-154">Sostituire l'ID in questo esempio con l'ID dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="83d2b-154">Replace the ID in this example with the ID from your environment.</span></span>

```bash
docker rm bd2424bfe7a5
```

<span data-ttu-id="83d2b-155">Si noti che il contenitore non può essere rimosso perché è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="83d2b-155">Notice that the container can't be removed because it's in a running state.</span></span>

```output
Error response from daemon: You cannot remove a running container a31c5a5f2a8d6e420435bfcadbe158fa6a26ed29c005a892171505cc0c2861b2. Stop the container before attempting removal or force remove
```

<span data-ttu-id="83d2b-156">Arrestare il contenitore con il comando `docker stop`.</span><span class="sxs-lookup"><span data-stu-id="83d2b-156">Stop the container with the `docker stop` command.</span></span>

```bash
docker stop bd2424bfe7a5
```

<span data-ttu-id="83d2b-157">Si noti che, a questo punto, se si esegue `docker ps` per elencare tutti i contenitori, il contenitore nginx non è elencato.</span><span class="sxs-lookup"><span data-stu-id="83d2b-157">Notice at this point, if you run `docker ps` to list all containers, the nginx container is not listed.</span></span>

```bash
docker ps
```

<span data-ttu-id="83d2b-158">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83d2b-158">The output should look similar to the following:</span></span>

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

<span data-ttu-id="83d2b-159">Per restituire un elenco di tutti i contenitori, inclusi i contenitori arrestati, aggiungere l'argomento `-a` al comando `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="83d2b-159">To return a list of all containers, including containers in a stopped state, add the `-a` argument to the `docker ps` command.</span></span>

```bash
docker ps -a
```

<span data-ttu-id="83d2b-160">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83d2b-160">The output should look similar to the following:</span></span>

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   13 seconds ago      Exited (0) 3 seconds ago                       focused_spence
```

<span data-ttu-id="83d2b-161">Provare a eseguire di nuovo l'operazione di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="83d2b-161">Try the delete operation again.</span></span> <span data-ttu-id="83d2b-162">Sostituire l'ID in questo esempio con l'ID dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="83d2b-162">Replace the ID in this example with the ID from your environment.</span></span>

```bash
docker rm bd2424bfe7a5
```

<span data-ttu-id="83d2b-163">Questa operazione restituisce l'ID del contenitore.</span><span class="sxs-lookup"><span data-stu-id="83d2b-163">This operation returns the container ID.</span></span>

```output
bd2424bfe7a5
```

## <a name="delete-a-container-image"></a><span data-ttu-id="83d2b-164">Eliminare un'immagine del contenitore</span><span class="sxs-lookup"><span data-stu-id="83d2b-164">Delete a container image</span></span>

<span data-ttu-id="83d2b-165">Quando un'immagine del contenitore non è più necessaria, può essere rimossa con il comando `docker rmi`.</span><span class="sxs-lookup"><span data-stu-id="83d2b-165">When you're done working with a container image, it can be removed with the `docker rmi` command.</span></span> <span data-ttu-id="83d2b-166">Se un contenitore (in esecuzione o arrestato) è stato avviato dall'immagine del contenitore, l'immagine non può essere eliminata.</span><span class="sxs-lookup"><span data-stu-id="83d2b-166">If any container (running or stopped) has been started from the container image, the image can't be deleted.</span></span> <span data-ttu-id="83d2b-167">Prima è necessario rimuovere i contenitori.</span><span class="sxs-lookup"><span data-stu-id="83d2b-167">The containers first need to be removed.</span></span> <span data-ttu-id="83d2b-168">Aggiungendo l'argomento `-f` al comando `docker rmi`, verrà forzata la rimozione di tutti i contenitori associati e quindi l'immagine del contenitore verrà rimossa.</span><span class="sxs-lookup"><span data-stu-id="83d2b-168">Adding the `-f` argument to the `docker rmi` command will force the removal of all associated containers, and will then remove the container image.</span></span>

<span data-ttu-id="83d2b-169">Rimuovere l'immagine del contenitore NGINX con il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="83d2b-169">Remove the NGINX container image with the following command.</span></span>

```bash
docker rmi nginx
```

<span data-ttu-id="83d2b-170">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83d2b-170">The output should look similar to the following:</span></span>

```output
Untagged: nginx:latest
Untagged: nginx@sha256:4a5573037f358b6cdfa2f3e8a9c33a5cf11bcd1675ca72ca76fbe5bd77d0d682
Deleted: sha256:8b89e48b5f157d9455c963b57c85d21e2337c58b8c983bc06f88476610adc129
Deleted: sha256:119ded3eca5e85ef43ee966e74564c604ccda064d955a8c5ed762e1d5e87f428
Deleted: sha256:6ece91c2763d826487e707f7b8ec063742ad0ee56cc9e605465cce95550c9a7f
Deleted: sha256:cdb3f9544e4c61d45da1ea44f7d92386639a052c620d1550376f22f5b46981af
```

## <a name="summary"></a><span data-ttu-id="83d2b-171">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="83d2b-171">Summary</span></span>

<span data-ttu-id="83d2b-172">In questa unità sono state illustrate alcune operazioni di Docker di base.</span><span class="sxs-lookup"><span data-stu-id="83d2b-172">In this unit, you learned about some basic Docker operations.</span></span> <span data-ttu-id="83d2b-173">Nell'unità successiva si creerà un'immagine del contenitore personalizzata.</span><span class="sxs-lookup"><span data-stu-id="83d2b-173">In the next unit, you will create a custom container image.</span></span>