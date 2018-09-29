<span data-ttu-id="081ca-101">Prima di descrivere alcuni modi comuni per eseguire, elencare ed eliminare i contenitori, si analizzerà una configurazione Nginx di base in esecuzione su un contenitore Docker ospitato in una macchina virtuale (VM) Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="081ca-101">Before we cover some of the common ways to run, list, and delete containers, let's see a basic Nginx configuration running on a Docker container that's hosted on an Ubuntu virtual machine (VM).</span></span>

<span data-ttu-id="081ca-102">In questa parte si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="081ca-102">In this part, you'll learn how to:</span></span>

1. <span data-ttu-id="081ca-103">Creare una macchina virtuale di Linux configurata per l'installazione di Docker al primo avvio della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-103">Create a Linux VM that's configured to install Docker when the VM first boots.</span></span>
1. <span data-ttu-id="081ca-104">Connettersi alla macchina virtuale e avviare un contenitore Docker che esegue Nginx.</span><span class="sxs-lookup"><span data-stu-id="081ca-104">Connect to your VM and start a Docker container running Nginx.</span></span>
1. <span data-ttu-id="081ca-105">Usare il binding delle porte per rendere il server Web accessibile dall'esterno della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-105">Use port binding to make your web server accessible from outside the VM.</span></span>

## <a name="what-are-containers"></a><span data-ttu-id="081ca-106">Che cosa sono i contenitori?</span><span class="sxs-lookup"><span data-stu-id="081ca-106">What are containers?</span></span>

<span data-ttu-id="081ca-107">Un contenitore è un ambiente di virtualizzazione, ma a differenza di una macchina virtuale, non include sempre un sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="081ca-107">A container is a virtualization environment but, unlike a virtual machine, it does not always include a full operating system.</span></span> <span data-ttu-id="081ca-108">Il contenitore fa invece riferimento al sistema operativo dell'ambiente host che esegue il contenitore stesso.</span><span class="sxs-lookup"><span data-stu-id="081ca-108">Instead, it references the operating system of the host environment that runs that container.</span></span> <span data-ttu-id="081ca-109">Se ad esempio si eseguono cinque contenitori su un server con un kernel Linux specifico, tutti e cinque i contenitori sono in esecuzione sullo stesso kernel.</span><span class="sxs-lookup"><span data-stu-id="081ca-109">For example, if you run five containers on a server with a specific Linux kernel, all five containers are running on that same kernel.</span></span>

<span data-ttu-id="081ca-110">I contenitori consentono di creare un pacchetto con l'applicazione e tutti gli elementi necessari per la sua esecuzione in un elemento definito _immagine del contenitore_.</span><span class="sxs-lookup"><span data-stu-id="081ca-110">Containers enable you to package your application and everything that it needs to run into what's known as a _container image_.</span></span> <span data-ttu-id="081ca-111">I contenitori sono isolati, ovvero non interferiscono con altri contenitori in esecuzione nello stesso sistema.</span><span class="sxs-lookup"><span data-stu-id="081ca-111">Containers are isolated, which means they don't interfere with any other container running on the same system.</span></span> <span data-ttu-id="081ca-112">Le immagini del contenitore sono anche portabili.</span><span class="sxs-lookup"><span data-stu-id="081ca-112">Container images are also portable.</span></span> <span data-ttu-id="081ca-113">È possibile caricare le immagini in un registro contenitori, ad esempio nell'hub Docker o nel registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="081ca-113">You can upload your images to a container registry, such as Docker Hub or Azure Container Registry.</span></span> <span data-ttu-id="081ca-114">È quindi possibile eseguire i contenitori nel cloud e aspettarsi che funzionino esattamente come nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="081ca-114">You can then run your containers in the cloud and expect them to behave the exact same way as they do in your development environment.</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

<span data-ttu-id="081ca-115">I contenitori supportano l'iterazione rapida.</span><span class="sxs-lookup"><span data-stu-id="081ca-115">Containers enable rapid iteration.</span></span> <span data-ttu-id="081ca-116">Dato che non devono includere un sistema operativo, i contenitori sono in genere molto più piccoli delle macchine virtuali complete.</span><span class="sxs-lookup"><span data-stu-id="081ca-116">Because containers don't need to include an operating system, they're typically much smaller than full VMs.</span></span> <span data-ttu-id="081ca-117">Di conseguenza è possibile avviare e arrestare i contenitori rapidamente, spesso in pochi secondi.</span><span class="sxs-lookup"><span data-stu-id="081ca-117">This enables you to start and stop containers quickly, often in seconds.</span></span>

## <a name="understand-the-setup"></a><span data-ttu-id="081ca-118">Informazioni sulla configurazione</span><span class="sxs-lookup"><span data-stu-id="081ca-118">Understand the setup</span></span>

<span data-ttu-id="081ca-119">Ai fini dell'apprendimento, viene reso disponibile un ambiente sandbox in cui lavorare.</span><span class="sxs-lookup"><span data-stu-id="081ca-119">For learning purposes, we provide a sandbox environment for you to work in.</span></span> <span data-ttu-id="081ca-120">Questo ambiente include Azure Cloud Shell, un'esperienza della riga di comando basata su browser per la gestione e lo sviluppo di risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="081ca-120">This environment includes Azure Cloud Shell, a browser-based command-line experience for managing and developing Azure resources.</span></span>

<span data-ttu-id="081ca-121">Da Cloud Shell si creerà una VM Linux che include Docker.</span><span class="sxs-lookup"><span data-stu-id="081ca-121">From Cloud Shell, you'll create a Linux VM that includes Docker.</span></span> <span data-ttu-id="081ca-122">Quindi ci si connetterà alla VM Linux tramite SSH per creare e usare i contenitori Docker in modo interattivo.</span><span class="sxs-lookup"><span data-stu-id="081ca-122">You'll connect to your Linux VM over SSH so that you can interactively create and use Docker containers.</span></span>

<span data-ttu-id="081ca-123">Si consideri questa VM Linux come un ambiente di sviluppo e uno spazio per apprendere il funzionamento dei contenitori.</span><span class="sxs-lookup"><span data-stu-id="081ca-123">Think of this Linux VM as your development environment and a space to learn about containers.</span></span> <span data-ttu-id="081ca-124">In pratica Docker viene installato nel computer che si usa per lo sviluppo delle app.</span><span class="sxs-lookup"><span data-stu-id="081ca-124">In practice, you would install Docker on the computer you use to develop your apps.</span></span> <span data-ttu-id="081ca-125">Docker viene eseguito in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="081ca-125">Docker runs on Windows, macOS, and Linux.</span></span>

<span data-ttu-id="081ca-126">Alla fine di questo modulo vengono rese disponibili risorse per ottenere altre informazioni sull'esecuzione di Docker nell'ambiente di sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="081ca-126">We'll provide resources where you can learn more about running Docker in your local development environment at the end of this module.</span></span>

## <a name="what-is-nginx"></a><span data-ttu-id="081ca-127">Che cos'è Nginx?</span><span class="sxs-lookup"><span data-stu-id="081ca-127">What is Nginx?</span></span>

<span data-ttu-id="081ca-128">Nginx (pronunciato "engine-x") è un server Web diffuso, gratuito e open source che può essere eseguito in Unix, Linux, macOS e Windows.</span><span class="sxs-lookup"><span data-stu-id="081ca-128">Nginx (pronounced "engine-x") is a popular, free, open-source web server that runs on Unix, Linux, macOS, and Windows.</span></span> <span data-ttu-id="081ca-129">La configurazione di un server Web è un buon metodo per visualizzare i contenitori in azione.</span><span class="sxs-lookup"><span data-stu-id="081ca-129">Configuring a web server is a good way to see containers in action.</span></span> <span data-ttu-id="081ca-130">In questo caso si userà Nginx per presentare una semplice pagina Web.</span><span class="sxs-lookup"><span data-stu-id="081ca-130">Here you'll use Nginx to serve a basic web page.</span></span>

## <a name="what-is-cloud-init"></a><span data-ttu-id="081ca-131">Che cos'è cloud-init?</span><span class="sxs-lookup"><span data-stu-id="081ca-131">What is cloud-init?</span></span>

<span data-ttu-id="081ca-132">Cloud-init consente di personalizzare una macchina virtuale Linux al primo avvio.</span><span class="sxs-lookup"><span data-stu-id="081ca-132">Cloud-init enables you to customize a Linux VM as it boots for the first time.</span></span> <span data-ttu-id="081ca-133">Cloud-init consente di installare pacchetti e scrivere file o configurare utenti e impostazioni di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="081ca-133">You can use cloud-init to install packages and write files, or to configure users and security.</span></span> <span data-ttu-id="081ca-134">Nel contesto corrente si userà uno script cloud-init per installare Docker nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-134">Here you'll use a cloud-init script to install Docker on your VM.</span></span>

## <a name="what-is-port-binding"></a><span data-ttu-id="081ca-135">Che cos'è il binding della porta?</span><span class="sxs-lookup"><span data-stu-id="081ca-135">What is port binding?</span></span>

<span data-ttu-id="081ca-136">Il binding della porta consente di inoltrare il traffico di rete a un contenitore dall'host corrispondente.</span><span class="sxs-lookup"><span data-stu-id="081ca-136">Port binding enables you to forward network traffic to a container from its host.</span></span>

<span data-ttu-id="081ca-137">Un contenitore Docker viene eseguito in una rete locale nel sistema host del contenitore.</span><span class="sxs-lookup"><span data-stu-id="081ca-137">A Docker container runs on a local network on the container's host system.</span></span> <span data-ttu-id="081ca-138">In questo caso la rete del contenitore si trova nella VM Linux.</span><span class="sxs-lookup"><span data-stu-id="081ca-138">In this case, the container's network will be on your Linux VM.</span></span>

<span data-ttu-id="081ca-139">Le richieste Web vengono in genere eseguite sulla porta 80 (HTTP).</span><span class="sxs-lookup"><span data-stu-id="081ca-139">You know that web requests typically run over port 80 (HTTP).</span></span> <span data-ttu-id="081ca-140">Poiché il contenitore è in esecuzione in una rete locale all'interno della macchina virtuale, non è accessibile dall'esterno.</span><span class="sxs-lookup"><span data-stu-id="081ca-140">Because the container is running on a local network inside your VM, the container is not accessible to the outside world.</span></span>

<span data-ttu-id="081ca-141">Docker consente di pubblicare (o esporre) una porta per rendere accessibile il contenitore dall'esterno della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-141">Docker enables you to publish, or expose, a port to make it accessible from outside the VM.</span></span> <span data-ttu-id="081ca-142">In questo caso si configura il contenitore in modo che il traffico di rete sulla porta 8080 della macchina virtuale venga inoltrato alla porta 80 del contenitore.</span><span class="sxs-lookup"><span data-stu-id="081ca-142">Here, you'll configure your container so that network traffic to port 8080 on your VM will be forwarded to port 80 on your container.</span></span>

## <a name="create-a-virtual-machine-to-host-docker"></a><span data-ttu-id="081ca-143">Creare una macchina virtuale per ospitare Docker</span><span class="sxs-lookup"><span data-stu-id="081ca-143">Create a virtual machine to host Docker</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="create-the-cloud-init-script"></a><span data-ttu-id="081ca-144">Creare lo script cloud-init</span><span class="sxs-lookup"><span data-stu-id="081ca-144">Create the cloud-init script</span></span>

1. <span data-ttu-id="081ca-145">Passare alla cartella **clouddrive**.</span><span class="sxs-lookup"><span data-stu-id="081ca-145">Navigate to the **clouddrive** folder.</span></span>

    ```azurecli
    cd clouddrive
    ```

1. <span data-ttu-id="081ca-146">Creare una nuova cartella denominata **vm-config**.</span><span class="sxs-lookup"><span data-stu-id="081ca-146">Create a new folder named **vm-config**.</span></span>

    ```azurecli
    mkdir vm-config
    ```

1. <span data-ttu-id="081ca-147">Passare alla nuova cartella.</span><span class="sxs-lookup"><span data-stu-id="081ca-147">Navigate to the new folder.</span></span>

    ```azurecli
    cd vm-config
    ```

1. <span data-ttu-id="081ca-148">Creare un nuovo file usando l'editor integrato di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="081ca-148">Create a new file using Cloud Shell's integrated editor.</span></span>

    ```azurecli
    code .
    ```

1. <span data-ttu-id="081ca-149">Incollare questo script cloud-init nell'editor.</span><span class="sxs-lookup"><span data-stu-id="081ca-149">Paste this cloud-init script into the editor.</span></span>

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

1. <span data-ttu-id="081ca-150">Salvare il file con il nome **docker-vm-config.txt** (<kbd>CTRL+S</kbd> o fare clic con il pulsante destro del mouse e selezionare **Salva**).</span><span class="sxs-lookup"><span data-stu-id="081ca-150">Save the file as **docker-vm-config.txt** (<kbd>Ctrl+S</kbd> or right click and select **Save**).</span></span>

1. <span data-ttu-id="081ca-151">Chiudere l'editor (<kbd>CTRL+Q</kbd> o fare clic con il pulsante destro del mouse e selezionare **Esci**).</span><span class="sxs-lookup"><span data-stu-id="081ca-151">Close the editor (<kbd>Ctrl+Q</kbd> or right click and select **Quit**).</span></span>

### <a name="create-the-vm"></a><span data-ttu-id="081ca-152">Creare la VM</span><span class="sxs-lookup"><span data-stu-id="081ca-152">Create the VM</span></span>

> [!IMPORTANT]
> <span data-ttu-id="081ca-153">In genere si crea un gruppo di risorse per tutte le risorse di Azure correlate con il comando `az group create`, ma per questi esercizi il gruppo di risorse è già stato creato.</span><span class="sxs-lookup"><span data-stu-id="081ca-153">Normally, you would create a resource group for all your related Azure resources with the `az group create` command, but for these exercises one has been created for you.</span></span> <span data-ttu-id="081ca-154">Usare "**<rgn>[nome gruppo di risorse sandbox]</rgn>**" come gruppo di risorse in tutti i passaggi.</span><span class="sxs-lookup"><span data-stu-id="081ca-154">Use "**<rgn>[sandbox resource group name]</rgn>**" as your resource group in all the steps.</span></span>

1. <span data-ttu-id="081ca-155">Eseguire il comando `az vm create` per creare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-155">Run this `az vm create` command to create your Linux VM.</span></span>

    ```azurecli
    az vm create \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys \
      --custom-data docker-vm-config.txt
    ```

<span data-ttu-id="081ca-156">L'argomento `--custom-data` specifica lo script cloud-init che installa Docker al primo avvio della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-156">The `--custom-data` argument specifies the cloud-init script that installs Docker when the VM first boots.</span></span> <span data-ttu-id="081ca-157">Questa operazione richiede qualche minuto.</span><span class="sxs-lookup"><span data-stu-id="081ca-157">The process will take a few minutes to complete.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="081ca-158">Connettersi alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="081ca-158">Connect to the VM</span></span>

<span data-ttu-id="081ca-159">Ora si creerà una connessione SSH alla macchina virtuale per poterla configurare.</span><span class="sxs-lookup"><span data-stu-id="081ca-159">Here you'll create an SSH connection to your VM so that you can configure it.</span></span>

1. <span data-ttu-id="081ca-160">Eseguire questo comando `az vm show` per ottenere l'indirizzo IP della VM.</span><span class="sxs-lookup"><span data-stu-id="081ca-160">Run this `az vm show` command to get your VM's IP address.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. <span data-ttu-id="081ca-161">Accedere tramite SSH alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-161">SSH into the VM.</span></span> <span data-ttu-id="081ca-162">Sostituire **ip-address** con il proprio indirizzo.</span><span class="sxs-lookup"><span data-stu-id="081ca-162">Replace **ip-address** with yours.</span></span>

    ```bash
    ssh azureuser@ip-address
    ```

1. <span data-ttu-id="081ca-163">Verificare la versione di Docker.</span><span class="sxs-lookup"><span data-stu-id="081ca-163">Verify the Docker version.</span></span>

    ```bash
    docker version
    ```

    > [!NOTE]
    > <span data-ttu-id="081ca-164">Se il comando non riesce o viene visualizzato un messaggio di errore relativo alla connessione al socket del daemon Docker, l'esecuzione dello script cloud-init non è stata ancora completata.</span><span class="sxs-lookup"><span data-stu-id="081ca-164">If the command fails, or you see an error message about connecting to the Docker daemon socket, that means that the cloud-init script hasn't yet completed.</span></span> <span data-ttu-id="081ca-165">È possibile attendere il completamento dello script, ma per il momento eseguire `exit` per uscire dalla sessione SSH, quindi riprovare la connessione dopo uno o due minuti.</span><span class="sxs-lookup"><span data-stu-id="081ca-165">Although there are ways to wait for the script to complete, for now run `exit` to leave your SSH session and try reconnecting in a minute or two.</span></span>

## <a name="start-nginx"></a><span data-ttu-id="081ca-166">Avviare Nginx</span><span class="sxs-lookup"><span data-stu-id="081ca-166">Start Nginx</span></span>

<span data-ttu-id="081ca-167">Ora si avvierà il contenitore Docker che esegue Nginx.</span><span class="sxs-lookup"><span data-stu-id="081ca-167">Here you'll start a Docker container that's running Nginx.</span></span>

1. <span data-ttu-id="081ca-168">Eseguire questo commando `docker run` per avviare un contenitore Docker che esegue Nginx.</span><span class="sxs-lookup"><span data-stu-id="081ca-168">Run this `docker run` command to start a Docker container running Nginx.</span></span>

    ```bash
    docker run -d -p 8080:80 --name nginx nginx
    ```

    <span data-ttu-id="081ca-169">Il comando scarica un'immagine Docker preconfigurata con Nginx dall'[hub Docker](https://hub.docker.com/_/nginx?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="081ca-169">The command downloads a Docker image that's preconfigured with Nginx from [Docker Hub](https://hub.docker.com/_/nginx?azure-portal=true).</span></span>

    <span data-ttu-id="081ca-170">L'argomento `--name` assegna il nome "nginx" al contenitore Docker, in modo che sia possibile usarlo in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="081ca-170">The `--name` argument assigns the name "nginx" to your Docker container so you can work with it later.</span></span>

    <span data-ttu-id="081ca-171">L'argomento `-p` inoltra il traffico di rete dalla porta 8080 della macchina virtuale alla porta 80 del contenitore.</span><span class="sxs-lookup"><span data-stu-id="081ca-171">The `-p` argument forwards network traffic to port 8080 on your VM to port 80 on your container.</span></span>

1. <span data-ttu-id="081ca-172">Eseguire `curl` per verificare che Nginx sia in esecuzione e accessibile.</span><span class="sxs-lookup"><span data-stu-id="081ca-172">Run `curl` to verify that Nginx is running and accessible.</span></span>

    ```bash
    curl http://localhost:8080
    ```

1. <span data-ttu-id="081ca-173">Chiudere la sessione SSH.</span><span class="sxs-lookup"><span data-stu-id="081ca-173">Exit your SSH session.</span></span>

    ```bash
    exit
    ```

## <a name="access-your-web-server-from-a-web-browser"></a><span data-ttu-id="081ca-174">Accedere al server Web da un Web browser</span><span class="sxs-lookup"><span data-stu-id="081ca-174">Access your web server from a web browser</span></span>

<span data-ttu-id="081ca-175">Ricordare che il contenitore è stato configurato in modo che il traffico di rete sulla porta 8080 venga inoltrato alla porta 80 del contenitore.</span><span class="sxs-lookup"><span data-stu-id="081ca-175">Recall that you configured your container so that network traffic on port 8080 is forwarded to port 80 on your container.</span></span>

<span data-ttu-id="081ca-176">Per visualizzare il mapping delle porte in azione, ora si apre la porta 8080 per la macchina virtuale attraverso il firewall di Azure.</span><span class="sxs-lookup"><span data-stu-id="081ca-176">To see port mapping in action, here you'll open port 8080 to your VM through the Azure firewall.</span></span> <span data-ttu-id="081ca-177">Quindi si accede al server Web da un Web browser.</span><span class="sxs-lookup"><span data-stu-id="081ca-177">Then you'll access your web server from a browser.</span></span>

1. <span data-ttu-id="081ca-178">Eseguire questo comando `az vm open-port` per aprire la porta 8080 della macchina virtuale attraverso il firewall.</span><span class="sxs-lookup"><span data-stu-id="081ca-178">Run this `az vm open-port` command to open port 8080 on your VM through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --port 8080 \
      --priority 1001
    ```

1. <span data-ttu-id="081ca-179">Da un Web browser passare all'indirizzo IP della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-179">From a web browser, navigate to your VM's IP address.</span></span> <span data-ttu-id="081ca-180">Assicurarsi di includere la porta 8080, ad esempio **http://40.121.106.164:8080**.</span><span class="sxs-lookup"><span data-stu-id="081ca-180">Be sure to include port 8080, for example,  **http://40.121.106.164:8080**.</span></span>

    <span data-ttu-id="081ca-181">Viene visualizzata la home page predefinita.</span><span class="sxs-lookup"><span data-stu-id="081ca-181">You see the default home page.</span></span>

    ![Il sito Web in esecuzione in un browser](../media/2-nginx-browser.png)

## <a name="stop-the-container"></a><span data-ttu-id="081ca-183">Arrestare il contenitore</span><span class="sxs-lookup"><span data-stu-id="081ca-183">Stop the container</span></span>

<span data-ttu-id="081ca-184">A questo punto si arresta il contenitore.</span><span class="sxs-lookup"><span data-stu-id="081ca-184">Now let's stop the container.</span></span>

1. <span data-ttu-id="081ca-185">In primo luogo accedere di nuovo alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-185">First, log back in to your VM.</span></span> <span data-ttu-id="081ca-186">Di seguito è riportato un rapido ripasso.</span><span class="sxs-lookup"><span data-stu-id="081ca-186">Here's a refresher.</span></span>

    <span data-ttu-id="081ca-187">Ottenere l'indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="081ca-187">Get the IP address.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

    <span data-ttu-id="081ca-188">Accedere tramite SSH alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="081ca-188">SSH into the VM.</span></span> <span data-ttu-id="081ca-189">Sostituire **ip-address** con il proprio indirizzo.</span><span class="sxs-lookup"><span data-stu-id="081ca-189">Replace **ip-address** with yours.</span></span>

    ```bash
    ssh azureuser@ip-address
    ```

1. <span data-ttu-id="081ca-190">Eseguire `docker stop nginx` per arrestare il contenitore con nome "nginx".</span><span class="sxs-lookup"><span data-stu-id="081ca-190">Run `docker stop nginx` to stop the container named "nginx".</span></span>

    ```bash
    docker stop nginx
    ```

<span data-ttu-id="081ca-191">Mantenere la connessione SSH aperta per la parte successiva.</span><span class="sxs-lookup"><span data-stu-id="081ca-191">Keep your SSH connection open for the next part.</span></span>

<span data-ttu-id="081ca-192">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="081ca-192">Congratulations!</span></span> <span data-ttu-id="081ca-193">È stata creata correttamente una macchina virtuale, che è stata usata per ospitare un server Web in contenitori Nginx.</span><span class="sxs-lookup"><span data-stu-id="081ca-193">You've successfully created a virtual machine and used it to host an Nginx containerized web server.</span></span>
