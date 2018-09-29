<span data-ttu-id="b7f13-101">Fino a questo punto sono state scaricate ed eseguite immagini Docker create da altri utenti.</span><span class="sxs-lookup"><span data-stu-id="b7f13-101">Up until now, you downloaded and ran Docker images created by others.</span></span> <span data-ttu-id="b7f13-102">Ora si creeranno direttamente le immagini contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-102">Here, you'll build your own container images.</span></span> <span data-ttu-id="b7f13-103">Si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="b7f13-103">You'll learn how to:</span></span>

* <span data-ttu-id="b7f13-104">Creare un'immagine mediante un processo manuale.</span><span class="sxs-lookup"><span data-stu-id="b7f13-104">Create an image using a manual process.</span></span>
* <span data-ttu-id="b7f13-105">Creare la stessa immagine usando un processo più automatizzato, detto Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="b7f13-105">Create the same image using a more automated process, called a Dockerfile.</span></span>
* <span data-ttu-id="b7f13-106">Pubblicare l'immagine in un hub Docker, un registro contenitori pubblico.</span><span class="sxs-lookup"><span data-stu-id="b7f13-106">Publish your image to Docker Hub, a public container registry.</span></span>

## <a name="what-is-a-dockerfile"></a><span data-ttu-id="b7f13-107">Che cos'è un Dockerfile?</span><span class="sxs-lookup"><span data-stu-id="b7f13-107">What is a Dockerfile?</span></span>

<span data-ttu-id="b7f13-108">Un Dockerfile è un file di testo che descrive tutti gli elementi necessari per l'esecuzione del contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-108">A Dockerfile is a text file that describes everything your container needs to run.</span></span>

<span data-ttu-id="b7f13-109">Un'istruzione nel Dockerfile può definire:</span><span class="sxs-lookup"><span data-stu-id="b7f13-109">An instruction in the Dockerfile can define:</span></span>

* <span data-ttu-id="b7f13-110">L'immagine padre sulla quale si basa l'immagine.</span><span class="sxs-lookup"><span data-stu-id="b7f13-110">The parent image your image is based on.</span></span>
* <span data-ttu-id="b7f13-111">Un pacchetto software da installare.</span><span class="sxs-lookup"><span data-stu-id="b7f13-111">A software package to install.</span></span>
* <span data-ttu-id="b7f13-112">Un file di configurazione o un altro file che l'app deve eseguire.</span><span class="sxs-lookup"><span data-stu-id="b7f13-112">A configuration or other file your app needs to run.</span></span>
* <span data-ttu-id="b7f13-113">Impostazioni di rete, ad esempio una porta che si vuole rendere disponibile.</span><span class="sxs-lookup"><span data-stu-id="b7f13-113">Networking settings, such as a port you want to make available.</span></span>
* <span data-ttu-id="b7f13-114">Il comando da eseguire all'avvio del contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-114">The command to run when the container starts.</span></span>

<span data-ttu-id="b7f13-115">È possibile creare un'immagine del contenitore usando un processo manuale o usare un Dockerfile per automatizzare il processo.</span><span class="sxs-lookup"><span data-stu-id="b7f13-115">You can create a container image using a manual process or use a Dockerfile to automate the process.</span></span> <span data-ttu-id="b7f13-116">La creazione manuale di un'immagine Docker è un buon metodo per comprendere il processo.</span><span class="sxs-lookup"><span data-stu-id="b7f13-116">Creating a Docker image manually is a good way to understand the process.</span></span> <span data-ttu-id="b7f13-117">L'uso di un Dockerfile rende il processo più veloce e semplice da ripetere.</span><span class="sxs-lookup"><span data-stu-id="b7f13-117">Using a Dockerfile makes the process faster and easier to repeat.</span></span>

## <a name="what-is-docker-hub"></a><span data-ttu-id="b7f13-118">Che cos'è l'hub Docker?</span><span class="sxs-lookup"><span data-stu-id="b7f13-118">What is Docker Hub?</span></span>

<span data-ttu-id="b7f13-119">L'hub Docker è una posizione in cui archiviare e scaricare le immagini Docker.</span><span class="sxs-lookup"><span data-stu-id="b7f13-119">Docker Hub is a place for you to store and download Docker images.</span></span> <span data-ttu-id="b7f13-120">È possibile scaricare le immagini Docker create da Docker e dalla community Docker, ad esempio l'immagine Nginx usata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b7f13-120">You can download Docker images that have been created by Docker and the Docker community, such as the Nginx image you used previously.</span></span> <span data-ttu-id="b7f13-121">È anche possibile condividere le proprie immagini con la community Docker o solo con il proprio team.</span><span class="sxs-lookup"><span data-stu-id="b7f13-121">You can also share your own images with the Docker community or just with your team.</span></span>

## <a name="create-an-image-manually"></a><span data-ttu-id="b7f13-122">Creare un'immagine manualmente</span><span class="sxs-lookup"><span data-stu-id="b7f13-122">Create an image manually</span></span>

<span data-ttu-id="b7f13-123">Ora si creerà un'immagine Docker che esegue un'applicazione Python di base.</span><span class="sxs-lookup"><span data-stu-id="b7f13-123">Here you'll create a Docker image that runs a basic Python application.</span></span>

1. <span data-ttu-id="b7f13-124">Per iniziare, attivare un'istanza di contenitore dall'immagine [python](https://hub.docker.com/_/python/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="b7f13-124">Start by bringing up a container instance from the [python](https://hub.docker.com/_/python/?azure-portal=true) image.</span></span>

    ```bash
    docker run --name python-demo -it python bash
    ```

    <span data-ttu-id="b7f13-125">L'estrazione dell'immagine dall'hub Docker richiederà qualche istante.</span><span class="sxs-lookup"><span data-stu-id="b7f13-125">It'll take a few moments for Docker to pull down this image from Docker Hub.</span></span> <span data-ttu-id="b7f13-126">Nel frattempo è possibile analizzare ciò che esegue il comando.</span><span class="sxs-lookup"><span data-stu-id="b7f13-126">While you wait, let's look at what the command does.</span></span>

    * <span data-ttu-id="b7f13-127">L'argomento `--name` specifica il nome del contenitore, "python-demo".</span><span class="sxs-lookup"><span data-stu-id="b7f13-127">The `--name` argument specifies your container's name, "python-demo".</span></span>
    * <span data-ttu-id="b7f13-128">Gli argomenti `-i` e `-t` vengono combinati in un solo argomento, `-it`.</span><span class="sxs-lookup"><span data-stu-id="b7f13-128">The `-i` and `-t` arguments are combined into one argument, `-it`.</span></span> <span data-ttu-id="b7f13-129">Questi argomenti creano una sessione interattiva con il contenitore in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b7f13-129">These arguments create an interactive session with the running container.</span></span>
      * <span data-ttu-id="b7f13-130">`-i` crea una sessione interattiva con il contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-130">`-i` creates an interactive session with the container.</span></span>
      * <span data-ttu-id="b7f13-131">`-t` crea un pseudo terminale che resta in stato di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b7f13-131">`-t` creates a pseudo terminal that remains in a running state.</span></span>
    * <span data-ttu-id="b7f13-132">`python` specifica l'immagine di base.</span><span class="sxs-lookup"><span data-stu-id="b7f13-132">`python` specifies the base image.</span></span> <span data-ttu-id="b7f13-133">Docker scarica l'immagine dall'hub Docker.</span><span class="sxs-lookup"><span data-stu-id="b7f13-133">Docker downloads this image from Docker Hub.</span></span> <span data-ttu-id="b7f13-134">Questa immagine è preconfigurata con un ambiente per la programmazione Python.</span><span class="sxs-lookup"><span data-stu-id="b7f13-134">This image is preconfigued with an environment for Python programming.</span></span>
    * <span data-ttu-id="b7f13-135">`bash` specifica il programma da eseguire.</span><span class="sxs-lookup"><span data-stu-id="b7f13-135">`bash` specifies the program to run.</span></span>

    <span data-ttu-id="b7f13-136">Questa configurazione può essere paragonata a un ambiente interattivo per la scrittura di app Python.</span><span class="sxs-lookup"><span data-stu-id="b7f13-136">Think of this setup as an interactive environment for you to write Python apps.</span></span> <span data-ttu-id="b7f13-137">Quando l'app è operativa è possibile acquisire un'immagine (o istantanea) dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="b7f13-137">Once you have your app up and running, you can capture an image, or snapshot, of your environment.</span></span> <span data-ttu-id="b7f13-138">Man mano che si migliora l'app è possibile creare immagini aggiuntive per uso personale o per il team.</span><span class="sxs-lookup"><span data-stu-id="b7f13-138">As you improve your app, you can create additional images for you or your team.</span></span>

    <span data-ttu-id="b7f13-139">La sessione del terminale passa al pseudo terminale del contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-139">Your terminal session switches to the container's pseudo terminal.</span></span> <span data-ttu-id="b7f13-140">Il prompt dei comandi è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b7f13-140">Your prompt resembles this:</span></span>

    ```docker
    root@d8ccada9c61e:/#
    ```

1. <span data-ttu-id="b7f13-141">Dalla sessione di Docker eseguire questa istruzione per creare una directory denominata `./app` e passare a tale directory.</span><span class="sxs-lookup"><span data-stu-id="b7f13-141">From your Docker session, run this to create a directory named `./app` and move to it.</span></span>

    ```bash
    mkdir ./app && cd ./app
    ```

    <span data-ttu-id="b7f13-142">Sebbene questa operazione non sia obbligatoria, la directory `./app` offre una posizione in cui aggiungere il codice Python.</span><span class="sxs-lookup"><span data-stu-id="b7f13-142">Although not required, the `./app` directory gives you a place to add your Python code.</span></span>

1. <span data-ttu-id="b7f13-143">Eseguire questa istruzione per creare un programma Python di base.</span><span class="sxs-lookup"><span data-stu-id="b7f13-143">Run this to create a very basic Python program.</span></span>

    ```bash
    echo 'print("Hello World!")' > hello.py
    ```

    <span data-ttu-id="b7f13-144">Questo programma visualizza "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="b7f13-144">This program prints "Hello World!"</span></span> <span data-ttu-id="b7f13-145">nel terminale.</span><span class="sxs-lookup"><span data-stu-id="b7f13-145">to the terminal.</span></span>

    > [!TIP]
    > <span data-ttu-id="b7f13-146">In pratica è possibile _montare_ una directory della workstation in una directory nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-146">In practice, you can _mount_ a directory on your workstation to a directory on your container.</span></span> <span data-ttu-id="b7f13-147">In questo modo è possibile scrivere codice nella workstation usando l'editor preferito.</span><span class="sxs-lookup"><span data-stu-id="b7f13-147">This enables you to use your favorite editor on your workstation to write code.</span></span> <span data-ttu-id="b7f13-148">Quando si salva il file, anche questo file viene visualizzato nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-148">When you save your file, that file also appears on your container.</span></span>

1. <span data-ttu-id="b7f13-149">Eseguire il codice seguente per provare il programma Python.</span><span class="sxs-lookup"><span data-stu-id="b7f13-149">Run the following to try out your Python program.</span></span>

    ```bash
    python hello.py
    ```

    <span data-ttu-id="b7f13-150">Verrà visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="b7f13-150">You see this.</span></span>

    ```output
    Hello World!
    ```

1. <span data-ttu-id="b7f13-151">Uscire dalla sessione interattiva.</span><span class="sxs-lookup"><span data-stu-id="b7f13-151">Exit your interactive session.</span></span>

    ```bash
    exit
    ```

1. <span data-ttu-id="b7f13-152">Nella VM Linux eseguire `docker ps` per elencare tutti i contenitori in esecuzione:</span><span class="sxs-lookup"><span data-stu-id="b7f13-152">From your Linux VM, run `docker ps` to list all running containers:</span></span>

    ```bash
    docker ps
    ```

    <span data-ttu-id="b7f13-153">Si noti che nessun elemento è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b7f13-153">You see that nothing is running.</span></span> 

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    ```

    <span data-ttu-id="b7f13-154">Questo si verifica perché con l'uscita dal contenitore termina la sessione di Bash.</span><span class="sxs-lookup"><span data-stu-id="b7f13-154">That's because exiting the container ends your Bash session.</span></span> <span data-ttu-id="b7f13-155">Quando l'applicazione o il servizio eseguito dal contenitore viene chiuso, Docker interrompe l'esecuzione del contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-155">When the application or service your container runs exits, Docker stops the container.</span></span>

1. <span data-ttu-id="b7f13-156">Eseguire `docker ps` una seconda volta e includere l'argomento `-a`.</span><span class="sxs-lookup"><span data-stu-id="b7f13-156">Run `docker ps` a second time and include the `-a` argument.</span></span> <span data-ttu-id="b7f13-157">Questo comando restituisce tutti i contenitori, in esecuzione o arrestati.</span><span class="sxs-lookup"><span data-stu-id="b7f13-157">This command returns all containers, running or stopped.</span></span>

    ```bash
    docker ps -a
    ```

    <span data-ttu-id="b7f13-158">Viene visualizzata una schermata simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="b7f13-158">You see something like this:</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
    cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
    ```

   <span data-ttu-id="b7f13-159">Il contenitore **python-demo** visualizza lo stato **Chiuso**.</span><span class="sxs-lookup"><span data-stu-id="b7f13-159">Your container, **python-demo**, has a status of **Exited**.</span></span>

1. <span data-ttu-id="b7f13-160">Eseguire `docker commit` per creare un'immagine dal contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-160">Run `docker commit` to create an image from your container.</span></span>

   ```bash
   docker commit python-demo python-custom
   ```

   <span data-ttu-id="b7f13-161">In questo caso, Docker crea un'immagine con nome **python-custom** dal contenitore con nome **python-demo**.</span><span class="sxs-lookup"><span data-stu-id="b7f13-161">Here, Docker creates an image named **python-custom** from your container named **python-demo**.</span></span> <span data-ttu-id="b7f13-162">Il nome dell'immagine può essere qualsiasi nome.</span><span class="sxs-lookup"><span data-stu-id="b7f13-162">The image name can be whatever you want.</span></span>

1. <span data-ttu-id="b7f13-163">Eseguire `docker images` per elencare le immagini.</span><span class="sxs-lookup"><span data-stu-id="b7f13-163">Run `docker images` to list your images.</span></span>

    ```bash
    docker images
    ```

    <span data-ttu-id="b7f13-164">Viene visualizzata l'immagine **python-custom**.</span><span class="sxs-lookup"><span data-stu-id="b7f13-164">You see the **python-custom** image.</span></span>

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-custom       latest              1f231e7127a1        6 seconds ago       922MB
    python              latest              638817465c7d        24 hours ago        922MB
    ```

1. <span data-ttu-id="b7f13-165">Usare `docker run` per eseguire il test dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="b7f13-165">Use `docker run` to test out your image.</span></span>

    ```bash
    docker run python-custom python app/hello.py
    ```

    <span data-ttu-id="b7f13-166">Tenere presente che `docker run` accetta come argomento finale il comando da eseguire.</span><span class="sxs-lookup"><span data-stu-id="b7f13-166">Recall that `docker run` takes the command to run as its final argument.</span></span> <span data-ttu-id="b7f13-167">In questo caso il comando è `python app/hello.py`.</span><span class="sxs-lookup"><span data-stu-id="b7f13-167">Here, the command is `python app/hello.py`.</span></span>

    <span data-ttu-id="b7f13-168">Verrà visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="b7f13-168">You see this.</span></span>

    ```output
    Hello World!
    ```

    <span data-ttu-id="b7f13-169">Dato che questo contenitore non è eseguito in modo interattivo, viene eseguito il programma Python e il contenitore si arresta.</span><span class="sxs-lookup"><span data-stu-id="b7f13-169">Because you're not running this container interactively, the Python program runs and the container stops.</span></span>

## <a name="use-a-dockerfile-to-create-an-image-automatically"></a><span data-ttu-id="b7f13-170">Usare un Dockerfile per creare automaticamente un'immagine</span><span class="sxs-lookup"><span data-stu-id="b7f13-170">Use a Dockerfile to create an image automatically</span></span>

<span data-ttu-id="b7f13-171">Ora si userà un processo più automatico per creare la stessa immagine Docker che esegue il programma Python.</span><span class="sxs-lookup"><span data-stu-id="b7f13-171">Here you'll use a more automated process to create the same Docker image that runs your Python program.</span></span>

<span data-ttu-id="b7f13-172">La creazione manuale di un'immagine è un ottimo modo per provare ed esplorare le nuove funzionalità.</span><span class="sxs-lookup"><span data-stu-id="b7f13-172">Building an image manually is a great way to experiment and explore new features.</span></span> <span data-ttu-id="b7f13-173">Tuttavia si supponga di voler condividere il processo con il team o di volerlo rendere ripetibile.</span><span class="sxs-lookup"><span data-stu-id="b7f13-173">But say you want to share the process with your team or make it repeatable.</span></span> <span data-ttu-id="b7f13-174">Si supponga ad esempio di voler creare ogni sera una nuova immagine che esegue la versione più recente del software che il team sta compilando.</span><span class="sxs-lookup"><span data-stu-id="b7f13-174">For example, say you want to create a new image each evening that runs the latest version of the software your team is building.</span></span>

<span data-ttu-id="b7f13-175">In questo caso il Dockerfile è la miglior soluzione.</span><span class="sxs-lookup"><span data-stu-id="b7f13-175">That's where the Dockerfile comes in.</span></span> <span data-ttu-id="b7f13-176">Un Dockerfile può essere considerato come un modo per descrivere istruzioni che in alternativa andrebbero eseguite manualmente.</span><span class="sxs-lookup"><span data-stu-id="b7f13-176">Think of a Dockerfile as a way to describe instructions you would otherwise do manually.</span></span>

1. <span data-ttu-id="b7f13-177">Dalla macchina virtuale di Linux eseguire questo comando per creare un Dockerfile che esegue il programma Python.</span><span class="sxs-lookup"><span data-stu-id="b7f13-177">From your Linux VM, run this command to create a Dockerfile that runs your Python program.</span></span>

    ```bash
    cat << EOF > Dockerfile
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    EOF
    ```

    <span data-ttu-id="b7f13-178">Questo esempio è un modo semplice per creare il file.</span><span class="sxs-lookup"><span data-stu-id="b7f13-178">This example is just an easy way for you to create the file.</span></span> <span data-ttu-id="b7f13-179">Nella pratica, per la creazione si usa l'editor di codice preferito.</span><span class="sxs-lookup"><span data-stu-id="b7f13-179">In practice, you would use your favorite code editor to create it.</span></span>

    <span data-ttu-id="b7f13-180">Stampare il Dockerfile nella console.</span><span class="sxs-lookup"><span data-stu-id="b7f13-180">Print your Dockerfile to the console.</span></span>

    ```bash
    cat Dockerfile
    ```

    <span data-ttu-id="b7f13-181">Verrà visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="b7f13-181">You see this.</span></span>

    ```output
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    ```

    <span data-ttu-id="b7f13-182">Ora si procederà ad analizzare in dettaglio il funzionamento del Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="b7f13-182">Let's break down what the Dockerfile does.</span></span>

    * <span data-ttu-id="b7f13-183">`FROM` specifica l'immagine padre sulla quale si basa l'immagine corrente.</span><span class="sxs-lookup"><span data-stu-id="b7f13-183">`FROM` specifies the parent image to base this image on.</span></span> <span data-ttu-id="b7f13-184">In questo caso l'immagine padre è **python**.</span><span class="sxs-lookup"><span data-stu-id="b7f13-184">Here, the parent image is **python**.</span></span>
    * <span data-ttu-id="b7f13-185">`WORKDIR` specifica la directory di lavoro per qualsiasi istruzione `RUN` e `CMD` visualizzata più avanti nel Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="b7f13-185">`WORKDIR` specifies the working directory for any `RUN` and `CMD` statements that appear later in the Dockerfile.</span></span> <span data-ttu-id="b7f13-186">Docker crea automaticamente la directory, in questo caso `./app`.</span><span class="sxs-lookup"><span data-stu-id="b7f13-186">Docker creates this directory for you, in this case, `./app`.</span></span>
    * <span data-ttu-id="b7f13-187">`RUN` specifica un comando da eseguire nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-187">`RUN` specifies a command to run in the container.</span></span> <span data-ttu-id="b7f13-188">È possibile usare `RUN` per installare software, apportare modifiche alla configurazione e pulire il contenitore prima che venga acquisito.</span><span class="sxs-lookup"><span data-stu-id="b7f13-188">You can use `RUN` to install software, make configuration changes, and cleanup the container before it's captured.</span></span> <span data-ttu-id="b7f13-189">In questo caso si crea un programma Python di base per `./app/hello.py`.</span><span class="sxs-lookup"><span data-stu-id="b7f13-189">Here, we write a basic Python program to `./app/hello.py`.</span></span>
    * <span data-ttu-id="b7f13-190">`CMD` specifica il processo da eseguire all'avvio del contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-190">`CMD` specifies the process to run when the container starts.</span></span> <span data-ttu-id="b7f13-191">In questo caso viene eseguito `python hello.py` dalla directory `/.app`.</span><span class="sxs-lookup"><span data-stu-id="b7f13-191">Here, we run `python hello.py` from the `/.app` directory.</span></span>

    <span data-ttu-id="b7f13-192">Si sarà notato che la procedura è quasi identica a quella eseguita per creare l'immagine manualmente.</span><span class="sxs-lookup"><span data-stu-id="b7f13-192">You've probably noticed that these are nearly the exact same steps you took to build the image manually.</span></span> <span data-ttu-id="b7f13-193">La conversione della procedura in codice semplifica la condivisione e la ripetizione del processo.</span><span class="sxs-lookup"><span data-stu-id="b7f13-193">Expressing these steps as code makes the process easier to share and repeat.</span></span>

1. <span data-ttu-id="b7f13-194">Eseguire `docker build` per creare l'immagine usando le istruzioni specificate nel Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="b7f13-194">Run `docker build` to create the image, using the instructions specified in the Dockerfile.</span></span>

    ```bash
    docker build -t python-dockerfile .
    ```

    <span data-ttu-id="b7f13-195">In genere Docker crea l'immagine in pochi secondi.</span><span class="sxs-lookup"><span data-stu-id="b7f13-195">It will typically take only a few seconds for Docker to build your image.</span></span>

    <span data-ttu-id="b7f13-196">Viene visualizzato un output simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="b7f13-196">You see output like the following.</span></span>

    ```output
    Sending build context to Docker daemon  2.048kB
    Step 1/4 : FROM python
     ---> 638817465c7d
    Step 2/4 : WORKDIR ./app
     ---> Running in 990d17e86466
    Removing intermediate container 990d17e86466
     ---> 59a074a092cc
    Step 3/4 : RUN echo 'print("Hello World!")' > hello.py
     ---> Running in aed707c53bc5
    Removing intermediate container aed707c53bc5
     ---> d7f55a9d0e85
    Step 4/4 : CMD python hello.py
     ---> Running in e87ec55a8d36
    Removing intermediate container e87ec55a8d36
     ---> 98c39b91770f
    Successfully built 98c39b91770f
    Successfully tagged python-dockerfile:latest
    ```

1. <span data-ttu-id="b7f13-197">Usare `docker images` per elencare le immagini.</span><span class="sxs-lookup"><span data-stu-id="b7f13-197">Use the `docker images` to list your images.</span></span>

    ```bash
    docker images
    ```

    <span data-ttu-id="b7f13-198">Viene visualizzata l'immagine **python-dockerfile** appena compilata.</span><span class="sxs-lookup"><span data-stu-id="b7f13-198">You see the **python-dockerfile** image you just built.</span></span>

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-dockerfile   latest              8ed5f85c5e5b        24 seconds ago      923MB
    python-custom       latest              6789cbe2cceb        4 minutes ago       923MB
    python              latest              a9d071760c82        2 weeks ago         923MB
    ```

1. <span data-ttu-id="b7f13-199">Usare `docker run` per eseguire il test del contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-199">Use `docker run` to test out your container.</span></span>

    ```bash
    docker run python-dockerfile
    ```

    <span data-ttu-id="b7f13-200">Verrà visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="b7f13-200">You see this.</span></span>

    ```output
    Hello World!
    ```

    <span data-ttu-id="b7f13-201">A differenza di quanto avviene nel test dell'immagine creata manualmente, in questo caso non si specifica il comando da eseguire, `python app/hello.py`.</span><span class="sxs-lookup"><span data-stu-id="b7f13-201">Unlike when you tested the image you created manually, here you don't specify the command to run, `python app/hello.py`.</span></span> <span data-ttu-id="b7f13-202">Il comando viene specificato dal Dockerfile, pertanto è incorporato nell'immagine.</span><span class="sxs-lookup"><span data-stu-id="b7f13-202">Your Dockerfile specifies that, so the command is built into the image.</span></span>

## <a name="publish-your-image-to-docker-hub"></a><span data-ttu-id="b7f13-203">Pubblicare l'immagine nell'hub Docker</span><span class="sxs-lookup"><span data-stu-id="b7f13-203">Publish your image to Docker Hub</span></span>

<span data-ttu-id="b7f13-204">Per pubblicare un'immagine nell'hub Docker è necessario disporre di un account.</span><span class="sxs-lookup"><span data-stu-id="b7f13-204">To publish an image to Docker Hub, you need an account.</span></span> <span data-ttu-id="b7f13-205">Ora si imposterà l'account dell'hub Docker e si pubblicherà l'immagine **python-dockerfile** nell'hub Docker.</span><span class="sxs-lookup"><span data-stu-id="b7f13-205">Here you'll set up your Docker Hub account and publish your **python-dockerfile** image to Docker Hub.</span></span>

1. <span data-ttu-id="b7f13-206">[Creare il proprio account hub Docker](https://hub.docker.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="b7f13-206">[Create your Docker Hub account](https://hub.docker.com?azure-portal=true).</span></span>

1. <span data-ttu-id="b7f13-207">Esportare il nome dell'account hub Docker come variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="b7f13-207">Export your Docker Hub account name as an environment variable.</span></span> <span data-ttu-id="b7f13-208">Sostituire **account-name** con il nome dell'account.</span><span class="sxs-lookup"><span data-stu-id="b7f13-208">Replace **account-name** with your account name.</span></span>

    ```bash
    export docker_account=account-name
    ```

    <span data-ttu-id="b7f13-209">La variabile di ambiente renderà più facile l'esecuzione dei comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="b7f13-209">This environment variable will make it easier for you to run the commands that follow.</span></span>

1. <span data-ttu-id="b7f13-210">Eseguire `docker tag` per contrassegnare l'immagine con il nome del proprio account hub Docker.</span><span class="sxs-lookup"><span data-stu-id="b7f13-210">Run `docker tag` to tag your image with your Docker Hub account name.</span></span>

    ```bash
    docker tag python-dockerfile $docker_account/python-dockerfile
    ```

1. <span data-ttu-id="b7f13-211">Eseguire `docker login` per accedere all'hub Docker.</span><span class="sxs-lookup"><span data-stu-id="b7f13-211">Run `docker login` to log in to Docker Hub.</span></span>

    ```bash
    docker login
    ```

    <span data-ttu-id="b7f13-212">Quando richiesto, immettere nome utente e password.</span><span class="sxs-lookup"><span data-stu-id="b7f13-212">When prompted, enter your username and password.</span></span>

1. <span data-ttu-id="b7f13-213">Eseguire `docker push` per eseguire il push dell'immagine **python-dockerfile** nell'hub Docker (caricare l'immagine).</span><span class="sxs-lookup"><span data-stu-id="b7f13-213">Run `docker push` to push, or upload, your **python-dockerfile** image to Docker Hub.</span></span>

    ```bash
    docker push $docker_account/python-dockerfile
    ```

    <span data-ttu-id="b7f13-214">L'output sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b7f13-214">You see output similar to the following:</span></span>

    ```output
    The push refers to repository [docker.io/account/python-dockerfile]
    f39073ca4d5a: Pushed
    9dfcec2738a9: Pushed
    ffab8273c674: Mounted from account/python
    e6f6cbe5e14e: Mounted from account/python
    3eb255f8a6ac: Mounted from account/python
    b860b6c48eec: Mounted from account/python
    1fa8778eb779: Mounted from account/python
    fa0c3f992cbd: Mounted from account/python
    ce6466f43b11: Mounted from account/python
    719d45669b35: Mounted from account/python
    3b10514a95be: Mounted from account/python
    latest: digest: sha256:b816c32382d06ac74530d56f2a7b83000c86174218f430c6bb5039fdcfba38aa size: 2631
    ```

    <span data-ttu-id="b7f13-215">L'immagine Docker è ora archiviata nell'hub Docker.</span><span class="sxs-lookup"><span data-stu-id="b7f13-215">Your Docker image is now stored in Docker Hub.</span></span> <span data-ttu-id="b7f13-216">È possibile usare `docker pull` o `docker run` da un altro computer per scaricare o eseguire l'immagine dall'hub Docker.</span><span class="sxs-lookup"><span data-stu-id="b7f13-216">You can use `docker pull` or `docker run` from another computer to download or run your image from Docker Hub.</span></span> <span data-ttu-id="b7f13-217">Ecco due esempi:</span><span class="sxs-lookup"><span data-stu-id="b7f13-217">Here are two examples:</span></span>

    1. <span data-ttu-id="b7f13-218">Questo esempio estrae l'immagine più recente dall'hub Docker.</span><span class="sxs-lookup"><span data-stu-id="b7f13-218">This example pulls the latest image from Docker Hub.</span></span>

        ```bash
        docker pull $docker_account/python-dockerfile
        ```

    1. <span data-ttu-id="b7f13-219">Questo esempio esegue il contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-219">This example runs the container.</span></span>

        ```bash
        docker run $docker_account/python-dockerfile
        ```

1. <span data-ttu-id="b7f13-220">Eseguire il test del contenitore.</span><span class="sxs-lookup"><span data-stu-id="b7f13-220">Test out your container.</span></span>

    <span data-ttu-id="b7f13-221">Accedere al proprio [account hub Docker](https://hub.docker.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="b7f13-221">Navigate to [your Docker Hub account](https://hub.docker.com?azure-portal=true).</span></span> <span data-ttu-id="b7f13-222">Nella scheda **Repository** viene visualizzata l'immagine Docker.</span><span class="sxs-lookup"><span data-stu-id="b7f13-222">From the **Repositories** tab, you see your Docker image.</span></span>

    ![Immagine Docker nell'hub Docker](../media/4-docker-hub-python-dockerfile.png)
