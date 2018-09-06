<span data-ttu-id="ed152-101">Nell'ultima unità sono state usate immagini dei contenitori già pronte per eseguire alcune operazioni di Docker di base.</span><span class="sxs-lookup"><span data-stu-id="ed152-101">In the last unit, you worked with pre-created container images to perform some basic Docker operations.</span></span> <span data-ttu-id="ed152-102">In questa unità si creeranno immagini dei contenitori personalizzate, si eseguirà il push di queste immagini in un registro contenitori pubblico e si eseguiranno i contenitori da queste immagini.</span><span class="sxs-lookup"><span data-stu-id="ed152-102">In this unit, you will create custom container images, push these images to a public container registry, and run containers from these images.</span></span>

<span data-ttu-id="ed152-103">Le immagini dei contenitori possono essere create manualmente o usando un cosiddetto Dockerfile per automatizzare il processo.</span><span class="sxs-lookup"><span data-stu-id="ed152-103">Container images can be created by hand or using what's called a Dockerfile to automate the process.</span></span> <span data-ttu-id="ed152-104">Il metodo preferito consiste nell'usare un Dockerfile, ma questa unità illustrerà entrambi i metodi.</span><span class="sxs-lookup"><span data-stu-id="ed152-104">The preferred method is using a Dockerfile, but this unit will demonstrate both methods.</span></span> <span data-ttu-id="ed152-105">L'obiettivo è conoscere il processo manuale per poter comprendere meglio cosa accade quando si usa un Dockerfile per l'automazione.</span><span class="sxs-lookup"><span data-stu-id="ed152-105">The intention is that having an understanding of the manual process will help you to better understand what's occurring when using a Dockerfile for automation.</span></span>

## <a name="manual-image-creation"></a><span data-ttu-id="ed152-106">Creazione di un'immagine manuale</span><span class="sxs-lookup"><span data-stu-id="ed152-106">Manual image creation</span></span>

<span data-ttu-id="ed152-107">Quando si crea manualmente un'immagine del contenitore, vengono eseguite le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ed152-107">When manually creating a container image, the following actions are taken:</span></span>

- <span data-ttu-id="ed152-108">Avviare un'istanza di un contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-108">Start an instance of a container.</span></span>
- <span data-ttu-id="ed152-109">Stabilire una sessione terminal con il contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-109">Establish a terminal session with the container.</span></span>
- <span data-ttu-id="ed152-110">Modificare il contenitore installando software e apportando modifiche alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="ed152-110">Modify the container by installing software and making configuration changes.</span></span>
- <span data-ttu-id="ed152-111">Acquisizione del contenitore in una nuova immagine usando il comando `docker capture`.</span><span class="sxs-lookup"><span data-stu-id="ed152-111">Capturing the container into a new image using the `docker capture` command.</span></span>

<span data-ttu-id="ed152-112">In questo primo esempio si avvia un'istanza di un contenitore che esegue Python, si crea un'applicazione hello world e quindi si acquisisce il contenitore in una nuova immagine.</span><span class="sxs-lookup"><span data-stu-id="ed152-112">In this first example, you start an instance of a container that's running Python, create a hello world application, and then capture the container to a new image.</span></span>

<span data-ttu-id="ed152-113">In primo luogo, eseguire un contenitore dall'immagine NGINX.</span><span class="sxs-lookup"><span data-stu-id="ed152-113">First, run a container from the NGINX image.</span></span> <span data-ttu-id="ed152-114">Questo comando è leggermente diverso dai comandi eseguiti nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="ed152-114">This command looks a bit different from the commands that you ran in the previous unit.</span></span> <span data-ttu-id="ed152-115">Poiché si vuole stabilire una sessione terminal con il contenitore in esecuzione, vengono forniti gli argomenti `-t` e `-i`.</span><span class="sxs-lookup"><span data-stu-id="ed152-115">Because you want to establish a terminal session with the running container, the `-t` and `-i` arguments are provided.</span></span> <span data-ttu-id="ed152-116">L'unione di questi argomenti consente di indicare a Docker di allocare uno pseudoterminale che rimarrà in uno stato di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ed152-116">Together, these arguments instruct Docker to allocate a pseudo terminal that will remain in a runnings state.</span></span> <span data-ttu-id="ed152-117">In altre parole, gli argomenti `-t` e `-i` creano una sessione interattiva con il contenitore in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ed152-117">In other words, the `-t` and `-i` arguments create an interactive session with the running container.</span></span>

<span data-ttu-id="ed152-118">Si specifica quindi che viene usata l'immagine del contenitore `python` e il processo da eseguire nel contenitore è `bash`.</span><span class="sxs-lookup"><span data-stu-id="ed152-118">You then specify that the `python` container image is used, and the process to run inside of the container is `bash`.</span></span>

```bash
docker run --name python-demo -ti python bash
```

<span data-ttu-id="ed152-119">Dopo che il comando viene eseguito, la sessione terminal passerà allo pseudoterminale del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-119">After the command is run, your terminal session should switch to the containers pseudo terminal.</span></span> <span data-ttu-id="ed152-120">Questo passaggio può essere visualizzato dal prompt del terminale, che dovrebbe ora essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ed152-120">This can be seen by the terminal prompt, which should have changed to something similar to the following:</span></span>

```bash
root@d8ccada9c61e:/#
```

<span data-ttu-id="ed152-121">A questo punto, si lavorerà all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-121">At this point, you're working inside the container.</span></span> <span data-ttu-id="ed152-122">Si noterà che lavorare all'interno di un contenitore è molto simile a lavorare all'interno di un sistema fisico o virtuale.</span><span class="sxs-lookup"><span data-stu-id="ed152-122">You should find that working inside a container is much like working inside a virtual or physical system.</span></span> <span data-ttu-id="ed152-123">È ad esempio possibile elencare, creare ed eliminare file, installare software e apportare modifiche alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="ed152-123">For instance, you can list, create, and delete files, install software, and make configuration changes.</span></span> <span data-ttu-id="ed152-124">Per questo semplice esempio, viene creato uno script hello world basato su Python.</span><span class="sxs-lookup"><span data-stu-id="ed152-124">For this simple example, a Python-based hello world script is created.</span></span> <span data-ttu-id="ed152-125">A tale scopo, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ed152-125">This can be done with the following command:</span></span>

```bash
echo 'print("Hello World!")' > hello.py
```

<span data-ttu-id="ed152-126">Per testare lo script mentre si è ancora nel contenitore, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ed152-126">To test the script while you're still in the container, run the following command:</span></span>

```bash
python hello.py
```

<span data-ttu-id="ed152-127">L'output sarà il seguente:</span><span class="sxs-lookup"><span data-stu-id="ed152-127">This will produce the following output:</span></span>

```bash
Hello World!
```

<span data-ttu-id="ed152-128">Dopo aver verificato che lo script funziona come previsto, uscire dal contenitore digitando `exit`:</span><span class="sxs-lookup"><span data-stu-id="ed152-128">When you're satisfied that the script functions as expected, exit out of the container by typing `exit`:</span></span>

```bash
exit
```

<span data-ttu-id="ed152-129">Nel terminale del sistema locale usare il comando `docker ps` per elencare tutti i contenitori in esecuzione:</span><span class="sxs-lookup"><span data-stu-id="ed152-129">Back in the terminal of your local system, use the `docker ps` command to list all running containers:</span></span>

```bash
docker ps
```

<span data-ttu-id="ed152-130">Si noti che nessuno è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ed152-130">Notice that nothing is running.</span></span> <span data-ttu-id="ed152-131">Quando è stato immesso `exit` nel contenitore in esecuzione, il processo Bash è stato completato e il contenitore è stato quindi arrestato.</span><span class="sxs-lookup"><span data-stu-id="ed152-131">When you entered `exit` in the running container, the Bash process completed, which then stopped the container.</span></span> <span data-ttu-id="ed152-132">Si tratta del comportamento previsto ed è corretto.</span><span class="sxs-lookup"><span data-stu-id="ed152-132">This is the expected behavior and is ok.</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

<span data-ttu-id="ed152-133">Usare di nuovo `docker ps` e includere l'argomento `-a`.</span><span class="sxs-lookup"><span data-stu-id="ed152-133">Use `docker ps` again and include the `-a` argument.</span></span> <span data-ttu-id="ed152-134">Questo comando restituirà un elenco di tutti i contenitori, indipendentemente dal fatto che siano in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ed152-134">This command will return a list of all containers, regardless if they're running.</span></span>

```bash
docker ps -a
```

<span data-ttu-id="ed152-135">Si noti che un contenitore con il nome *python-demo* ha lo stato *Exited*.</span><span class="sxs-lookup"><span data-stu-id="ed152-135">Notice that a container with the name *python-demo* has a status of *Exited*.</span></span> <span data-ttu-id="ed152-136">Questo contenitore è l'istanza arrestata del contenitore da cui si è appena usciti.</span><span class="sxs-lookup"><span data-stu-id="ed152-136">This container is the stopped instance of the container that you just exited from.</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
```

<span data-ttu-id="ed152-137">Per creare una nuova immagine del contenitore da questo contenitore, usare il comando `docker commit`.</span><span class="sxs-lookup"><span data-stu-id="ed152-137">To create a new container image from this container, use the `docker commit` command.</span></span> <span data-ttu-id="ed152-138">L'esempio seguente indica a *docker commit* di creare un'immagine denominata *python-custom* dai contenitori *python-demo*.</span><span class="sxs-lookup"><span data-stu-id="ed152-138">The following example instructs *docker commit* to create an image named *python-custom* from the *python-demo* containers.</span></span>

```bash
docker commit python-demo python-custom
```

<span data-ttu-id="ed152-139">Dopo il completamento del comando, l'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ed152-139">After the command completes, you should see output similar to the following:</span></span>

```bash
sha256:91a0cf9aa9857bebcd7ebec3418970f97f043e31987fd4a257c8ac8c8418dc38
```

<span data-ttu-id="ed152-140">Eseguire ora `docker images` per visualizzare un elenco di immagini dei contenitori:</span><span class="sxs-lookup"><span data-stu-id="ed152-140">Now run `docker images` to see a list of container images:</span></span>

```bash
docker images
```

<span data-ttu-id="ed152-141">Si noterà ora l'immagine personalizzata di Python.</span><span class="sxs-lookup"><span data-stu-id="ed152-141">You should now see the custom Python image.</span></span>

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python-custom       latest              1f231e7127a1        6 seconds ago       922MB
python              latest              638817465c7d        24 hours ago        922MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

<span data-ttu-id="ed152-142">Eseguire un contenitore dalla nuova immagine.</span><span class="sxs-lookup"><span data-stu-id="ed152-142">Run a container from the new image.</span></span> <span data-ttu-id="ed152-143">È anche necessario specificare quale comando o processo eseguire all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-143">You also need to specify what command or process to run inside of the container.</span></span> <span data-ttu-id="ed152-144">Con questo esempio, eseguire `python hello.py`.</span><span class="sxs-lookup"><span data-stu-id="ed152-144">With this example, run `python hello.py`.</span></span>


```bash
docker run python-custom python hello.py
```

<span data-ttu-id="ed152-145">Il contenitore verrà avviato e restituirà il messaggio hello world.</span><span class="sxs-lookup"><span data-stu-id="ed152-145">The container will start and output the hello world message.</span></span> <span data-ttu-id="ed152-146">Il processo di Python viene quindi completato e il contenitore viene arrestato.</span><span class="sxs-lookup"><span data-stu-id="ed152-146">The Python process is then complete and the container stops.</span></span>

```bash
Hello World!
```

## <a name="automated-image-creation"></a><span data-ttu-id="ed152-147">Creazione di un'immagine automatizzata</span><span class="sxs-lookup"><span data-stu-id="ed152-147">Automated image creation</span></span>

<span data-ttu-id="ed152-148">Nell'ultima sezione è stata creata manualmente un'immagine del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-148">In the last section, a container image was manually created.</span></span> <span data-ttu-id="ed152-149">Questo metodo funziona perfettamente per la creazione occasionale o pratica di immagini, ma non è sostenibile in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="ed152-149">While this method works great for one-off or experiential image creation, it's not sustainable in a production environment.</span></span> <span data-ttu-id="ed152-150">La creazione di un'immagine del contenitore può essere automatizzata usando un *Dockerfile* e il comando `docker build`.</span><span class="sxs-lookup"><span data-stu-id="ed152-150">Container image creation can be automated using a *Dockerfile* and the `docker build` command.</span></span> <span data-ttu-id="ed152-151">Il comando *docker build* esegue, in pratica, le istruzioni trovate nel *Dockerfile* e quindi acquisisce i risultati in una nuova immagine del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-151">The *docker build* command essentially starts a container, runs the instructions found in the *Dockerfile*, and then captures the results to a new container image.</span></span>

<span data-ttu-id="ed152-152">Creare un file denominato *Dockerfile* e immettere il testo seguente:</span><span class="sxs-lookup"><span data-stu-id="ed152-152">Create a file named *Dockerfile* and enter the following text:</span></span>

```bash
FROM python

WORKDIR ./app

RUN echo 'print("Hello World!")' > hello.py

CMD python hello.py
```

<span data-ttu-id="ed152-153">L'istruzione *FROM* specifica l'immagine di base da usare durante le creazioni delle immagini.</span><span class="sxs-lookup"><span data-stu-id="ed152-153">The *FROM* statement specifies the base image to be used during image creations.</span></span> <span data-ttu-id="ed152-154">L'istruzione *RUN* esegue i comandi all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-154">The *RUN* statement runs commands inside of the container.</span></span> <span data-ttu-id="ed152-155">*RUN* può essere usata per installare software, apportare modifiche alla configurazione e pulire il contenitore prima dell'evento di acquisizione.</span><span class="sxs-lookup"><span data-stu-id="ed152-155">*RUN* can be used to install software, make configuration changes, and clean up the container before the capture event.</span></span> <span data-ttu-id="ed152-156">L'istruzione *CMD* specifica il processo da eseguire quando viene avviato un contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-156">The *CMD* statement specifies the process that should run when a container is started.</span></span>

<span data-ttu-id="ed152-157">Usare il comando `docker build` per creare una nuova immagine del contenitore con le istruzioni specificate nel Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="ed152-157">Use the `docker build` command to create a new container image using the instructions specified in the Dockerfile.</span></span>

```bash
docker build -t python-dockerfile .
```

<span data-ttu-id="ed152-158">L'output dovrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="ed152-158">You should see output similar to the following.</span></span>

```bash
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

<span data-ttu-id="ed152-159">Usare il comando `docker images` per restituire un elenco di immagini dei contenitori.</span><span class="sxs-lookup"><span data-stu-id="ed152-159">Use the `docker images` command to return a list of container images.</span></span>

```bash
docker images
```

<span data-ttu-id="ed152-160">Si noterà ora l'immagine personalizzata.</span><span class="sxs-lookup"><span data-stu-id="ed152-160">You should now see the custom image.</span></span>

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
python-dockerfile   latest              98c39b91770f        About a minute ago   922MB
python              latest              638817465c7d        26 hours ago         922MB
alpine              latest              11cd0b38bc3c        2 weeks ago          4.41MB
```

<span data-ttu-id="ed152-161">Usare il comando `docker run` per eseguire un contenitore dall'immagine personalizzata.</span><span class="sxs-lookup"><span data-stu-id="ed152-161">Use the `docker run` command to run a container from the custom image.</span></span>

<span data-ttu-id="ed152-162">Si noti che non sono stati forniti argomenti al comando `docker run`.</span><span class="sxs-lookup"><span data-stu-id="ed152-162">Notice here that no arguments have been provided to the `docker run` command.</span></span> <span data-ttu-id="ed152-163">A differenza di quando si crea manualmente un'immagine del contenitore, un Dockerfile consente di includere un comando da eseguire all'avvio del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-163">Unlike when you manually create a container image, a Dockerfile allows you to include a command to run when the container starts.</span></span> <span data-ttu-id="ed152-164">In questo caso, il comando specificato è `python hello.py`, che fa eseguire al contenitore lo script Python.</span><span class="sxs-lookup"><span data-stu-id="ed152-164">In this case, the specified command is `python hello.py`, which causes the container to run the Python script.</span></span>

```bash
docker run python-dockerfile
```

<span data-ttu-id="ed152-165">Dopo aver eseguito il comando, verrà visualizzato l'output del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-165">After you run the command, you should see the container output.</span></span>

```bash
Hello World!
```

## <a name="push-the-image-to-a-public-registry"></a><span data-ttu-id="ed152-166">Eseguire il push dell'immagine in un registro pubblico</span><span class="sxs-lookup"><span data-stu-id="ed152-166">Push the image to a public registry</span></span>

<span data-ttu-id="ed152-167">Docker Hub è un registro contenitori pubblico.</span><span class="sxs-lookup"><span data-stu-id="ed152-167">Docker Hub is a public container registry.</span></span> <span data-ttu-id="ed152-168">Per eseguire il push delle immagini dei contenitori in Docker Hub, è necessario un account Docker.</span><span class="sxs-lookup"><span data-stu-id="ed152-168">In order to push container images to Docker Hub, you must have a Docker account.</span></span> <span data-ttu-id="ed152-169">Se necessario, visitare il [sito di Docker Hub](https://hub.docker.com/) per eseguire la registrazione per un account.</span><span class="sxs-lookup"><span data-stu-id="ed152-169">If needed, visit the [Docker Hub site](https://hub.docker.com/) to register for an account.</span></span>

<span data-ttu-id="ed152-170">Dopo aver creato un account Docker Hub, è necessario applicare un tag con il nome dell'account all'immagine del contenitore.</span><span class="sxs-lookup"><span data-stu-id="ed152-170">After you have a Docker Hub account, the container image must be tagged with the account name.</span></span> <span data-ttu-id="ed152-171">A questo scopo, usare il comando `docker tag`.</span><span class="sxs-lookup"><span data-stu-id="ed152-171">To do so, use the `docker tag` command.</span></span>

<span data-ttu-id="ed152-172">Nell'esempio seguente all'immagine *python-dockerfile* viene applicato un tag con un nome dell'account Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="ed152-172">In the following example, the *python-dockerfile* image is tagged with a Docker Hub account name.</span></span> <span data-ttu-id="ed152-173">Sostituire `<account name>` con i nome dell'account Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="ed152-173">Replace `<account name>` with your Docker Hub account name.</span></span>

```bash
docker tag python-dockerfile <account name>/python-dockerfile
```

<span data-ttu-id="ed152-174">Assicurarsi quindi di essere connessi a Docker Hub usando il comando `docker login`.</span><span class="sxs-lookup"><span data-stu-id="ed152-174">Next, make sure that you are logged into Docker Hub using the `docker login` command.</span></span>

```bash
docker login
```

<span data-ttu-id="ed152-175">Eseguire infine il push dell'immagine *python-dockerfile* in Docker Hub con il comando `docker push`.</span><span class="sxs-lookup"><span data-stu-id="ed152-175">Finally push the *python-dockerfile* image to Docker Hub using the `docker push` command.</span></span> <span data-ttu-id="ed152-176">Sostituire `<account name>` con i nome dell'account Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="ed152-176">Replace `<account name>` with your Docker Hub account name.</span></span>

```bash
docker push <account name>/python-dockerfile
```

<span data-ttu-id="ed152-177">Mentre l'immagine del contenitore viene caricata in Docker Hub, verrà visualizzato un output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ed152-177">While the container image is being uploaded to Docker Hub, you will see output similar to the following:</span></span>

```bash
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
```

<span data-ttu-id="ed152-178">L'immagine del contenitore è ora archiviata in Docker Hub ed è accessibile da qualsiasi computer connesso a Internet tramite `docker pull` o `docker run`.</span><span class="sxs-lookup"><span data-stu-id="ed152-178">The container image is now stored in Docker Hub and can be accessed from any internet-connected machine using `docker pull` or `docker run`.</span></span>

## <a name="summary"></a><span data-ttu-id="ed152-179">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="ed152-179">Summary</span></span>

<span data-ttu-id="ed152-180">In questa unità sono state create due immagini dei contenitori.</span><span class="sxs-lookup"><span data-stu-id="ed152-180">In this unit, you created two container images.</span></span> <span data-ttu-id="ed152-181">La prima è stata creata manualmente e la seconda è stata automatizzata usando un Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="ed152-181">The first was created manually and the second was automated using a Dockerfile.</span></span>
