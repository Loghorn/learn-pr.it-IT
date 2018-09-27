<span data-ttu-id="d7028-101">A questo punto si dispone di un ambiente di sviluppo contenitori funzionante ed è possibile esaminare alcuni modi comuni per eseguire, elencare ed eliminare i contenitori.</span><span class="sxs-lookup"><span data-stu-id="d7028-101">Now that you have a functioning container development environment, let's look at some common ways to run, list, and delete containers.</span></span>

<span data-ttu-id="d7028-102">In questa unità si apprenderà a:</span><span class="sxs-lookup"><span data-stu-id="d7028-102">Here, you'll learn how to:</span></span>

* <span data-ttu-id="d7028-103">Eseguire contenitori di base.</span><span class="sxs-lookup"><span data-stu-id="d7028-103">Run basic containers.</span></span>
* <span data-ttu-id="d7028-104">Scaricare immagini del contenitore.</span><span class="sxs-lookup"><span data-stu-id="d7028-104">Download container images.</span></span>
* <span data-ttu-id="d7028-105">Eliminare i contenitori e le immagini associate.</span><span class="sxs-lookup"><span data-stu-id="d7028-105">Delete containers and their associated images.</span></span>

## <a name="whats-a-container-image"></a><span data-ttu-id="d7028-106">Cos'è un'immagine del contenitore?</span><span class="sxs-lookup"><span data-stu-id="d7028-106">What's a container image?</span></span>

<span data-ttu-id="d7028-107">L'_immagine_ del contenitore include il sistema operativo di base ed eventuali altri processi, applicazioni e configurazioni.</span><span class="sxs-lookup"><span data-stu-id="d7028-107">A container _image_ includes the base operating system and any additional processes, applications, and configurations.</span></span> <span data-ttu-id="d7028-108">Proprio come una macchina virtuale, un _contenitore_ è un'istanza in esecuzione di un'immagine.</span><span class="sxs-lookup"><span data-stu-id="d7028-108">Much like a virtual machine, a _container_ is a running instance of an image.</span></span>

## <a name="connect-to-your-linux-vm"></a><span data-ttu-id="d7028-109">Connettersi alla macchina virtuale Linux</span><span class="sxs-lookup"><span data-stu-id="d7028-109">Connect to your Linux VM</span></span>

<span data-ttu-id="d7028-110">Se ci si è disconnessi dalla sessione SSH creata nell'ultima parte, sarà necessario accedere.</span><span class="sxs-lookup"><span data-stu-id="d7028-110">If you disconnected from the SSH session you created in the last part, you will need to log in.</span></span> <span data-ttu-id="d7028-111">Di seguito è riportato un rapido ripasso.</span><span class="sxs-lookup"><span data-stu-id="d7028-111">Here's a refresher.</span></span>

1. <span data-ttu-id="d7028-112">Ottenere l'indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="d7028-112">Get the IP address.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. <span data-ttu-id="d7028-113">Accedere tramite SSH alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d7028-113">SSH into the VM.</span></span> <span data-ttu-id="d7028-114">Sostituire **ip-address** con il proprio indirizzo.</span><span class="sxs-lookup"><span data-stu-id="d7028-114">Replace **ip-address** with yours.</span></span>

    ```bash
    ssh azureuser@ip-address
    ```

## <a name="create-and-run-a-basic-container"></a><span data-ttu-id="d7028-115">Creare ed eseguire un contenitore di base</span><span class="sxs-lookup"><span data-stu-id="d7028-115">Create and run a basic container</span></span>

<span data-ttu-id="d7028-116">Di seguito verrà eseguito un contenitore basato su Alpine Linux.</span><span class="sxs-lookup"><span data-stu-id="d7028-116">Here you'll run a container that's based on Alpine Linux.</span></span> <span data-ttu-id="d7028-117">Alpine Linux è molto diffuso per le sue dimensioni: le immagini del contenitore possono essere infatti di appena 5 MB.</span><span class="sxs-lookup"><span data-stu-id="d7028-117">Alpine Linux is popular because of its size &mdash; container images can be as small as 5 MB.</span></span>

<span data-ttu-id="d7028-118">Eseguire questo comando `docker run` per creare un contenitore basato su Alpine Linux.</span><span class="sxs-lookup"><span data-stu-id="d7028-118">Run this `docker run` command to create a container based on Alpine Linux.</span></span>

```bash
docker run alpine echo "Hello World"
```

<span data-ttu-id="d7028-119">L'output sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d7028-119">You see output similar to this:</span></span>

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

<span data-ttu-id="d7028-120">Il comando `docker run` crea un contenitore, esegue il comando specificato e quindi distrugge il contenitore.</span><span class="sxs-lookup"><span data-stu-id="d7028-120">The `docker run` command creates a container, runs the provided command, and then destroys the container.</span></span>

<span data-ttu-id="d7028-121">Qui Docker scarica l'immagine [alpine](https://hub.docker.com/_/alpine?azure-portal=true) dall'hub Docker, avvia il contenitore e quindi stampa "Hello World" nella console.</span><span class="sxs-lookup"><span data-stu-id="d7028-121">Here, Docker downloads the [alpine](https://hub.docker.com/_/alpine?azure-portal=true) image from Docker Hub, starts the container, and then prints "Hello World" to the console.</span></span>

<span data-ttu-id="d7028-122">In questo caso il comando `echo` viene eseguito brevemente e poi viene chiuso.</span><span class="sxs-lookup"><span data-stu-id="d7028-122">In this case, the `echo` command runs briefly and then exits.</span></span> <span data-ttu-id="d7028-123">Nella parte precedente Nginx viene eseguito in primo piano mantenendo quindi attivo il contenitore fino a quando non lo si arresta o non si arresta il servizio Nginx.</span><span class="sxs-lookup"><span data-stu-id="d7028-123">In the previous part, Nginx runs in the foreground and therefore keeps your container alive until you stop the container or stop the Nginx service.</span></span>

## <a name="get-container-images"></a><span data-ttu-id="d7028-124">Ottenere le immagini dei contenitori</span><span class="sxs-lookup"><span data-stu-id="d7028-124">Get container images</span></span>

<span data-ttu-id="d7028-125">Le immagini dei contenitori vengono archiviate in un registro immagini dei contenitori.</span><span class="sxs-lookup"><span data-stu-id="d7028-125">Container images are stored in a container image registry.</span></span> <span data-ttu-id="d7028-126">Nell'esempio precedente Docker effettua il pull dell'immagine **alpine** dall'hub Docker, il registro contenitori pubblico di Docker.</span><span class="sxs-lookup"><span data-stu-id="d7028-126">In the previous example, Docker pulls the **alpine** image from Docker Hub, Docker's public container registry.</span></span>

<span data-ttu-id="d7028-127">Eseguire `docker images` per visualizzare un elenco delle immagini scaricate nel sistema.</span><span class="sxs-lookup"><span data-stu-id="d7028-127">Run `docker images` to see a list of images that have been downloaded to your system.</span></span>

```bash
docker images
```

<span data-ttu-id="d7028-128">Verranno visualizzate due immagini &mdash; **alpine** e **nginx**.</span><span class="sxs-lookup"><span data-stu-id="d7028-128">You see two images &mdash; **alpine** and **nginx**.</span></span>

```output
REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
alpine              latest              196d12cf6ab1        8 days ago4.41MB
nginx               latest              06144b287844        2 weeks ago109MB
```

<span data-ttu-id="d7028-129">Per cercare un'immagine del contenitore, usare il comando `docker search`.</span><span class="sxs-lookup"><span data-stu-id="d7028-129">To search for a container image, use the `docker search` command.</span></span> <span data-ttu-id="d7028-130">Ad esempio, eseguire il comando seguente per elencare tutte le immagini dei contenitori con `nginx` nel nome.</span><span class="sxs-lookup"><span data-stu-id="d7028-130">For example, run the following to list all container images that include `nginx` in their names.</span></span>

```bash
docker search nginx --limit 5
```

<span data-ttu-id="d7028-131">L'argomento `--limit` in questo esempio limita l'operazione di ricerca ai primi cinque risultati.</span><span class="sxs-lookup"><span data-stu-id="d7028-131">The `--limit` argument in this example restricts the search operation to the first five results.</span></span>

<span data-ttu-id="d7028-132">L'output sarà simile a questo.</span><span class="sxs-lookup"><span data-stu-id="d7028-132">You see something similar to this.</span></span>

```output
NAME                                     DESCRIPTION         STARS               OFFICIAL            AUTOMATED
nginx                                    Official build of Nginx.         9672                [OK]
jwilder/nginx-proxy                      Automated Nginx reverse proxy for docker con…   1415                                    [OK]
richarvey/nginx-php-fpm                  Container running Nginx + PHP-FPM capable of…   615                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion   LetsEncrypt container to use with nginx as p…   411                                     [OK]
bitnami/nginx                            Bitnami nginx Docker Image         58                                      [OK]
```

<span data-ttu-id="d7028-133">Comandi quali `docker run` scaricano automaticamente l'immagine se non è disponibile nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="d7028-133">Commands like `docker run` pull down the image for you if you don't have it locally.</span></span> <span data-ttu-id="d7028-134">Scaricare un'immagine prima di usarla è una pratica comune.</span><span class="sxs-lookup"><span data-stu-id="d7028-134">But it's common to download an image before you use it.</span></span> <span data-ttu-id="d7028-135">In questo modo si è sicuri di avere sempre la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="d7028-135">This ensures you have the latest version.</span></span>

<span data-ttu-id="d7028-136">A questo scopo, usare il comando `docker pull`.</span><span class="sxs-lookup"><span data-stu-id="d7028-136">To do so, you use the `docker pull` command.</span></span> <span data-ttu-id="d7028-137">Eseguire il comando seguente per effettuare il pull dell'immagine **nginx** più recente dall'hub Docker.</span><span class="sxs-lookup"><span data-stu-id="d7028-137">Run the following to pull the latest **nginx** image from Docker Hub.</span></span>

```bash
docker pull nginx
```

<span data-ttu-id="d7028-138">Questo esempio illustra che è disponibile la versione più recente dell'immagine **nginx**.</span><span class="sxs-lookup"><span data-stu-id="d7028-138">This example shows that you have the latest version of the **nginx** image.</span></span>

```output
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
Status: Image is up to date for nginx:latest
```

## <a name="run-and-list-a-container"></a><span data-ttu-id="d7028-139">Eseguire ed elencare i contenitori</span><span class="sxs-lookup"><span data-stu-id="d7028-139">Run and list a container</span></span>

<span data-ttu-id="d7028-140">Nella parte precedente è stato usato il comando `docker run` per attivare Nginx.</span><span class="sxs-lookup"><span data-stu-id="d7028-140">In the previous part, you used the `docker run` command to bring up Nginx.</span></span> <span data-ttu-id="d7028-141">Il comando verrà ora eseguito una seconda volta per esaminarlo più attentamente.</span><span class="sxs-lookup"><span data-stu-id="d7028-141">Let's run that command a second time and take a closer look into what's happening.</span></span>

1. <span data-ttu-id="d7028-142">Dalla connessione SSH, eseguire questo comando per attivare un contenitore che esegue Nginx.</span><span class="sxs-lookup"><span data-stu-id="d7028-142">From your SSH connection, run this command to bring up a container running Nginx.</span></span>

    ```bash
    docker run -d -p 8080:80 nginx
    ```

    * <span data-ttu-id="d7028-143">L'argomento `-d` specifica che il contenitore verrà eseguito senza essere collegato, ovvero che verrà eseguito in background.</span><span class="sxs-lookup"><span data-stu-id="d7028-143">The `-d` argument specifies that the container will run in a detached mode, which means the container runs in the background.</span></span> <span data-ttu-id="d7028-144">Se il processo Nginx si interrompe o si arresta in modo anomalo, viene arrestato anche il contenitore.</span><span class="sxs-lookup"><span data-stu-id="d7028-144">If the Nginx process stops or crashes, the container itself is also stopped.</span></span>
    * <span data-ttu-id="d7028-145">L'argomento `-p` specifica che il traffico di rete in arrivo sulla porta 8080 nell'host del contenitore, in questo caso la macchina virtuale Linux, viene inoltrato alla porta 80 nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="d7028-145">The `-p` argument specifies that network traffic arriving to port 8080 on the container host, your Linux VM in this case, is forwarded to port 80 on the container.</span></span> <span data-ttu-id="d7028-146">Questo argomento è stato usato nella parte precedente per accedere al server Web dal browser.</span><span class="sxs-lookup"><span data-stu-id="d7028-146">You used this argument in the previous part to access the web server from your browser.</span></span>
    * <span data-ttu-id="d7028-147">L'argomento `nginx` è il nome dell'immagine del contenitore da eseguire.</span><span class="sxs-lookup"><span data-stu-id="d7028-147">The `nginx` argument is the name of the container image to run.</span></span>

    <span data-ttu-id="d7028-148">Più avanti sarà possibile esplorare l'elenco completo delle opzioni `docker run` nelle [informazioni di riferimento su docker run](https://docs.docker.com/engine/reference/run?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="d7028-148">Later, you can explore the complete list of `docker run` options in the [docker run reference](https://docs.docker.com/engine/reference/run?azure-portal=true).</span></span>

    <span data-ttu-id="d7028-149">Il comando `docker run` visualizza un identificatore univoco per il contenitore.</span><span class="sxs-lookup"><span data-stu-id="d7028-149">The `docker run` command displays a unique identifier for the container.</span></span> <span data-ttu-id="d7028-150">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d7028-150">For example:</span></span>

    ```output
    9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466
    ```

1. <span data-ttu-id="d7028-151">Eseguire `docker ps` per elencare i contenitori in esecuzione nel sistema.</span><span class="sxs-lookup"><span data-stu-id="d7028-151">Run `docker ps` to list the running containers on your system.</span></span>

    ```bash
    docker ps
    ```

    <span data-ttu-id="d7028-152">Verrà visualizzato il contenitore Nginx appena avviato.</span><span class="sxs-lookup"><span data-stu-id="d7028-152">You see the Nginx container you just started.</span></span> <span data-ttu-id="d7028-153">Si noti che il contenitore ha un ID e un nome.</span><span class="sxs-lookup"><span data-stu-id="d7028-153">Notice that the container has both an ID and a name.</span></span> <span data-ttu-id="d7028-154">È possibile usare uno di questi due valori per gestire il contenitore.</span><span class="sxs-lookup"><span data-stu-id="d7028-154">You can use either one of these values to manage the container.</span></span> <span data-ttu-id="d7028-155">Prendere nota dell'ID del contenitore.</span><span class="sxs-lookup"><span data-stu-id="d7028-155">Note the container ID.</span></span> <span data-ttu-id="d7028-156">Verrà usato più avanti.</span><span class="sxs-lookup"><span data-stu-id="d7028-156">You'll use it later.</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS              PORTS                  NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   4 minutes ago     Up 4 minutes        0.0.0.0:8080->80/tcp   compassionate_allen
    ```

1. <span data-ttu-id="d7028-157">Come in precedenza, è possibile passare all'indirizzo IP della macchina virtuale in un browser per vedere il server Web in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d7028-157">As you did previously, you can navigate to your VM's IP address in a browser to see the running web server.</span></span> <span data-ttu-id="d7028-158">Assicurarsi di specificare la porta 8080 nell'URL.</span><span class="sxs-lookup"><span data-stu-id="d7028-158">Don't forget to specify port 8080 as part of the URL.</span></span>

    ![Il sito Web in esecuzione in un browser](../media/2-nginx-browser.png)

## <a name="delete-containers"></a><span data-ttu-id="d7028-160">Eliminare i contenitori</span><span class="sxs-lookup"><span data-stu-id="d7028-160">Delete containers</span></span>

<span data-ttu-id="d7028-161">Per eliminare un contenitore usare il comando `docker rm`.</span><span class="sxs-lookup"><span data-stu-id="d7028-161">You use the `docker rm` command to delete a container.</span></span> <span data-ttu-id="d7028-162">È possibile specificare il contenitore usando il nome o l'ID.</span><span class="sxs-lookup"><span data-stu-id="d7028-162">You can specify the container by its name or ID.</span></span>

1. <span data-ttu-id="d7028-163">Provare a eseguire `docker rm` per eliminare il contenitore che esegue Nginx.</span><span class="sxs-lookup"><span data-stu-id="d7028-163">Try running `docker rm` to delete your container running Nginx.</span></span> <span data-ttu-id="d7028-164">Eseguire `docker ps` se si ha la necessità di reperire l'ID.</span><span class="sxs-lookup"><span data-stu-id="d7028-164">Run `docker ps` if you need to find the ID.</span></span> <span data-ttu-id="d7028-165">Ecco un esempio.</span><span class="sxs-lookup"><span data-stu-id="d7028-165">Here's an example.</span></span>

    ```bash
    docker rm 9d7327e31212
    ```

    <span data-ttu-id="d7028-166">Si noti che il contenitore non può essere rimosso perché è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d7028-166">You see that the container can't be removed because it's in the running state.</span></span>

    ```output
    Error response from daemon: You cannot remove a running container 9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466. Stop the container before attempting removal or force remove
    ```

1. <span data-ttu-id="d7028-167">Eseguire `docker stop` per arrestare il contenitore.</span><span class="sxs-lookup"><span data-stu-id="d7028-167">Run `docker stop` to stop the container.</span></span> <span data-ttu-id="d7028-168">Ecco un esempio.</span><span class="sxs-lookup"><span data-stu-id="d7028-168">Here's an example.</span></span>

    ```bash
    docker stop 9d7327e31212
    ```
    
1. <span data-ttu-id="d7028-169">Eseguire `docker ps` per verificare che il contenitore non sia più in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d7028-169">Run `docker ps` to verify that the container is no longer running.</span></span>

    ```bash
    docker ps
    ```

    <span data-ttu-id="d7028-170">Verrà visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="d7028-170">You see this.</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATEDSTATUS              PORTS               NAMES
    ```

1. <span data-ttu-id="d7028-171">Eseguire `docker ps` una seconda volta specificando il flag `-a`.</span><span class="sxs-lookup"><span data-stu-id="d7028-171">Run `docker ps` a second time, but this time provide th `-a` flag.</span></span> <span data-ttu-id="d7028-172">Verranno elencati tutti i contenitori, anche quelli arrestati.</span><span class="sxs-lookup"><span data-stu-id="d7028-172">This lists all containers, even those that are stopped.</span></span>

    ```bash
    docker ps -a
    ```

    <span data-ttu-id="d7028-173">L'output sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d7028-173">You see output similar to this:</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS                          PORTS               NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   11 minutes ago     Exited (0) About a minute ago                       compassionate_allen
    df8aeea0320f        alpine              "echo 'Hello World'"     About an hour ago   Exited (0) About an hour ago                        objective_payne
    3a7efb75c288        nginx               "nginx -g 'daemon of…"   About an hour ago   Exited (0) About an hour ago                        nginx
    ```

    <span data-ttu-id="d7028-174">L'output include il contenitore Nginx appena arrestato e gli altri contenitori eseguiti in precedenza.</span><span class="sxs-lookup"><span data-stu-id="d7028-174">The output includes the Nginx container you just stopped as well as the other containers you ran prior.</span></span>

1. <span data-ttu-id="d7028-175">Eseguire `docker rm` una seconda volta.</span><span class="sxs-lookup"><span data-stu-id="d7028-175">Run `docker rm` a second time.</span></span> <span data-ttu-id="d7028-176">Sostituire l'ID visualizzato con uno dei propri.</span><span class="sxs-lookup"><span data-stu-id="d7028-176">Replace the ID shown with one of yours.</span></span>

    ```bash
    docker rm 9d7327e31212
    ```

1. <span data-ttu-id="d7028-177">Eseguire il comando `docker rm` seguente per eliminare _tutti_ i contenitori attivi.</span><span class="sxs-lookup"><span data-stu-id="d7028-177">Run the following `docker rm` command to delete _all_ active containers.</span></span>

    ```bash
    docker rm $(docker ps -aq)
    ```

1. <span data-ttu-id="d7028-178">Eseguire di nuovo `docker ps -a`. Si noterà che non sono presenti contenitori attivi in esecuzione o arrestati.</span><span class="sxs-lookup"><span data-stu-id="d7028-178">Run `docker ps -a` again and you see that there are no active containers running or stopped.</span></span>

    ```bash
    docker ps -a
    ```

## <a name="delete-a-container-image"></a><span data-ttu-id="d7028-179">Eliminare un'immagine del contenitore</span><span class="sxs-lookup"><span data-stu-id="d7028-179">Delete a container image</span></span>

<span data-ttu-id="d7028-180">Per eliminare un'immagine del contenitore usare il comando `docker rmi`.</span><span class="sxs-lookup"><span data-stu-id="d7028-180">You use the `docker rmi` command to delete a container image.</span></span>

<span data-ttu-id="d7028-181">È possibile eliminare un'immagine solo se nessun contenitore avviato dall'immagine, in esecuzione o arrestato, è attivo.</span><span class="sxs-lookup"><span data-stu-id="d7028-181">You can only delete an image if no container started from that image, running or stopped, is active.</span></span> <span data-ttu-id="d7028-182">L'argomento `-f`, tuttavia, forza la rimozione di tutti i contenitori associati e quindi l'immagine del contenitore viene rimossa.</span><span class="sxs-lookup"><span data-stu-id="d7028-182">However, the `-f` argument forces the removal of all associated containers and will then remove the container image.</span></span>

1. <span data-ttu-id="d7028-183">Eseguire `docker rmi nginx` per rimuovere l'immagine Nginx dalla macchina virtuale Linux.</span><span class="sxs-lookup"><span data-stu-id="d7028-183">Run `docker rmi nginx` to remove the Nginx image from your Linux VM.</span></span>

    ```bash
    docker rmi nginx
    ```

    <span data-ttu-id="d7028-184">L'output visualizzato sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d7028-184">The output you see resembles this:</span></span>

    ```output
    Untagged: nginx:latest
    Untagged: nginx@sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
    Deleted: sha256:06144b2878448774e55577ae7d66b5f43a87c2e44322b3884e4e6c70d070b262
    Deleted: sha256:824a442ee3c96744d75be3737a22cc6a47aebad1b30be67f3c2f8f29cb0aa879
    Deleted: sha256:8e3d1e9e4945f930c93c30617512998437f6edafd86676770d29b1581f2520bb
    Deleted: sha256:8b15606a9e3e430cb7ba739fde2fbb3734a19f8a59a825ffa877f9be49059817
    ```

1. <span data-ttu-id="d7028-185">Eseguire `docker images` per elencare le immagini presenti nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d7028-185">Run `docker images` to list the images on your VM.</span></span> 

    ```bash
    docker images
    ```

    <span data-ttu-id="d7028-186">Verrà visualizzata l'immagine **alpine**.</span><span class="sxs-lookup"><span data-stu-id="d7028-186">You see the **alpine** image.</span></span>

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
    alpine              latest              196d12cf6ab1        8 days ago4.41MB
    ```

1. <span data-ttu-id="d7028-187">Eseguire il comando `docker rmi` seguente per eliminare _tutte_ le immagini dalla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d7028-187">Run the following `docker rmi` command to delete _all_ images from your VM.</span></span>

    ```bash
    docker rmi $(docker images -q)
    ```

1. <span data-ttu-id="d7028-188">Eseguire di nuovo `docker images`. Si noterà che non sono presenti immagini nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d7028-188">Run `docker images` again and you see that there are no images on the VM.</span></span>

    ```bash
    docker images
    ```
