<span data-ttu-id="99fe4-101">In questa unità verranno eseguite alcune operazioni di risoluzione dei problemi di base, ad esempio l'esecuzione del pull sui log contenitore e sugli eventi del contenitore e la connessione a un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="99fe4-101">In this unit, you will perform some basic troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.</span></span> <span data-ttu-id="99fe4-102">Al termine di questo modulo sarà possibile comprendere le funzionalità di base per la risoluzione dei problemi relativi alle istanze di contenitore.</span><span class="sxs-lookup"><span data-stu-id="99fe4-102">By the end of this module, you should understand basic capabilities for troubleshooting container instances.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="99fe4-103">Creare un contenitore</span><span class="sxs-lookup"><span data-stu-id="99fe4-103">Create a container</span></span>

<span data-ttu-id="99fe4-104">Creare un contenitore da usare in questa unità.</span><span class="sxs-lookup"><span data-stu-id="99fe4-104">Start by creating a container to use in this unit.</span></span> <span data-ttu-id="99fe4-105">Se si dispone ancora del primo contenitore creato in questo modulo, ignorare questo passaggio:</span><span class="sxs-lookup"><span data-stu-id="99fe4-105">If you still have the first container created in this module, skip this step:</span></span>

```azurecli
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --ports 80 --ip-address Public
```

## <a name="get-logs-from-a-container-instance"></a><span data-ttu-id="99fe4-106">Ottenere i log da un'istanza di contenitore</span><span class="sxs-lookup"><span data-stu-id="99fe4-106">Get logs from a container instance</span></span>

<span data-ttu-id="99fe4-107">Per visualizzare i log generati dal codice dell'applicazione all'interno di un contenitore, è possibile usare il comando `az container logs`:</span><span class="sxs-lookup"><span data-stu-id="99fe4-107">To view logs from your application code within a container, you can use the `az container logs` command:</span></span>

```azazurecli
az container logs --resource-group myResourceGroup --name mycontainer
```

<span data-ttu-id="99fe4-108">Di seguito è riportato l'output del log generato dal contenitore di esempio dopo aver eseguito più volte l'accesso all'app Web:</span><span class="sxs-lookup"><span data-stu-id="99fe4-108">The following is log output from the example container after the web app has been accessed a few times:</span></span>

```output
listening on port 80
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://23.101.136.193/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:25 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:27 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
```

## <a name="get-container-events"></a><span data-ttu-id="99fe4-109">Ottenere gli eventi del contenitore</span><span class="sxs-lookup"><span data-stu-id="99fe4-109">Get container events</span></span>

<span data-ttu-id="99fe4-110">Il comando `az container attach` fornisce informazioni diagnostiche durante l'avvio del contenitore.</span><span class="sxs-lookup"><span data-stu-id="99fe4-110">The `az container attach` command provides diagnostic information during container startup.</span></span> <span data-ttu-id="99fe4-111">Dopo l'avvio, il contenitore trasmette inoltre STDOUT e STDERR alla console locale:</span><span class="sxs-lookup"><span data-stu-id="99fe4-111">Once the container has started, it also streams STDOUT and STDERR to your local console:</span></span>

```azazurecli
az container attach --resource-group myResourceGroup --name mycontainer
```

<span data-ttu-id="99fe4-112">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="99fe4-112">Example output:</span></span>


```output
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-08-20 21:43:26+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:32+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Created container
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Started container

Start streaming logs:
listening on port 80
listening on port 80
```

## <a name="execute-a-command-in-a-container"></a><span data-ttu-id="99fe4-113">Eseguire un comando in un contenitore</span><span class="sxs-lookup"><span data-stu-id="99fe4-113">Execute a command in a container</span></span>

<span data-ttu-id="99fe4-114">Istanze di contenitore di Azure supporta l'esecuzione di un comando in un contenitore in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="99fe4-114">Azure Container Instances supports executing a command in a running container.</span></span> <span data-ttu-id="99fe4-115">Eseguire un comando in un contenitore già avviato è particolarmente utile durante lo sviluppo e la risoluzione dei problemi di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="99fe4-115">Running a command in a container you've already started is especially helpful during application development and troubleshooting.</span></span> <span data-ttu-id="99fe4-116">L'uso più comune di questa funzionalità consiste nell'avviare una shell interattiva in modo che sia possibile eseguire il debug di problemi in un contenitore in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="99fe4-116">The most common use of this feature is to launch an interactive shell, so that you can debug issues in a running container.</span></span>

<span data-ttu-id="99fe4-117">Questo esempio avvia una sessione interattiva del terminale con il contenitore in esecuzione:</span><span class="sxs-lookup"><span data-stu-id="99fe4-117">This example starts an interactive terminal session with the running container:</span></span>

```azurecli
az container exec --resource-group myResourceGroup --name mycontainer --exec-command /bin/sh
```

<span data-ttu-id="99fe4-118">Una volta completato il comando, si potrà lavorare all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="99fe4-118">Once the command has completed, you are effectively working inside of the container.</span></span> <span data-ttu-id="99fe4-119">In questo esempio, è stato eseguito il comando `ls` per visualizzare il contenuto della directory di lavoro:</span><span class="sxs-lookup"><span data-stu-id="99fe4-119">In this example, the `ls` command was run to display the contents of the working directory:</span></span>

```output
usr/src/app # ls
index.html         node_modules       package.json
index.js           package-lock.json
```

<span data-ttu-id="99fe4-120">Digitare `exit` per arrestare la sessione remota.</span><span class="sxs-lookup"><span data-stu-id="99fe4-120">Enter `exit` to stop the remote session.</span></span>

## <a name="monitor-container-cpu-and-memory"></a><span data-ttu-id="99fe4-121">Monitorare CPU e memoria del contenitore</span><span class="sxs-lookup"><span data-stu-id="99fe4-121">Monitor container CPU and memory</span></span>

<span data-ttu-id="99fe4-122">Potrebbe essere utile eseguire il pull di metriche relative all'uso di CPU e memoria.</span><span class="sxs-lookup"><span data-stu-id="99fe4-122">You may want to pull metrics on CPU and memory usage.</span></span> <span data-ttu-id="99fe4-123">A questo scopo, ottenere innanzitutto l'ID dell'Istanza di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="99fe4-123">To do so, first get the ID of the Azure container instance.</span></span> <span data-ttu-id="99fe4-124">In questo esempio, l'ID è inserito in una variabile denominata `CONTAINER_ID`:</span><span class="sxs-lookup"><span data-stu-id="99fe4-124">In this example, the ID is placed in a variable named `CONTAINER_ID`:</span></span>

```azurecli
CONTAINER_ID=$(az container show --resource-group myResourceGroup --name mycontainer --query id --output tsv)
```

<span data-ttu-id="99fe4-125">Usare il comando `az monitor metrics list` per estrarre le informazioni sull'uso della CPU:</span><span class="sxs-lookup"><span data-stu-id="99fe4-125">Now, use the `az monitor metrics list` command to pull back CPU usage information:</span></span>

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric CPUUsage --output table
```

<span data-ttu-id="99fe4-126">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="99fe4-126">Example output:</span></span>

```output
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:39:00  CPU Usage
2018-08-20 21:40:00  CPU Usage
2018-08-20 21:41:00  CPU Usage
2018-08-20 21:42:00  CPU Usage
2018-08-20 21:43:00  CPU Usage      0.375
2018-08-20 21:44:00  CPU Usage      0.875
2018-08-20 21:45:00  CPU Usage      1
2018-08-20 21:46:00  CPU Usage      3.625
2018-08-20 21:47:00  CPU Usage      1.5
2018-08-20 21:48:00  CPU Usage      2.75
2018-08-20 21:49:00  CPU Usage      1.625
2018-08-20 21:50:00  CPU Usage      0.625
2018-08-20 21:51:00  CPU Usage      0.5
2018-08-20 21:52:00  CPU Usage      0.5
2018-08-20 21:53:00  CPU Usage      0.5
```

<span data-ttu-id="99fe4-127">Il comando seguente può essere usato per ottenere informazioni sull'uso della memoria:</span><span class="sxs-lookup"><span data-stu-id="99fe4-127">The following command can be used to get memory usage information:</span></span>

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric MemoryUsage --output table
```

<span data-ttu-id="99fe4-128">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="99fe4-128">Example output:</span></span>

```output
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:43:00  Memory Usage
2018-08-20 21:44:00  Memory Usage  0.0
2018-08-20 21:45:00  Memory Usage  15917056.0
2018-08-20 21:46:00  Memory Usage  16744448.0
2018-08-20 21:47:00  Memory Usage  16842752.0
2018-08-20 21:48:00  Memory Usage  17190912.0
2018-08-20 21:49:00  Memory Usage  17506304.0
2018-08-20 21:50:00  Memory Usage  17702912.0
2018-08-20 21:51:00  Memory Usage  17965056.0
2018-08-20 21:52:00  Memory Usage  18509824.0
2018-08-20 21:53:00  Memory Usage  18649088.0
2018-08-20 21:54:00  Memory Usage  18845696.0
2018-08-20 21:55:00  Memory Usage  19181568.0
```

<span data-ttu-id="99fe4-129">Queste informazioni sono disponibili anche nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="99fe4-129">This information is also available in the Azure portal.</span></span> <span data-ttu-id="99fe4-130">Per visualizzare una rappresentazione grafica delle informazioni relative all'uso di CPU e memoria, visitare la pagina di panoramica del portale di Azure per un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="99fe4-130">To see graphical representation of CPU and memory usage information, visit the Azure portal overview page for a container instance.</span></span>

![Visualizzazione nel portale di Azure delle informazioni relative all'uso di memoria e CPU da parte di Istanze di contenitore di Azure](../media-draft/cpu-memory.png)

## <a name="clean-up"></a><span data-ttu-id="99fe4-132">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="99fe4-132">Clean up</span></span>
<!---TODO: Update for sandbox?--->

<span data-ttu-id="99fe4-133">Questa è l'ultima unità del modulo di apprendimento relativo alle Istanze di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="99fe4-133">This is the last unit of the Azure Container Instances learning module.</span></span> <span data-ttu-id="99fe4-134">A questo punto è possibile pulire le risorse create eliminando il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="99fe4-134">At this point, you can cleanup the created resources by deleting the resource group.</span></span> <span data-ttu-id="99fe4-135">A tale scopo, usare il comando **az group delete**:</span><span class="sxs-lookup"><span data-stu-id="99fe4-135">To do so, use the **az group delete** command:</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a><span data-ttu-id="99fe4-136">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="99fe4-136">Summary</span></span>

<span data-ttu-id="99fe4-137">In questa unità sono state descritte diverse operazioni di risoluzione dei problemi di base, ad esempio l'esecuzione del pull sui log contenitore e sugli eventi del contenitore e la connessione a un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="99fe4-137">In this unit, you learned about several troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.</span></span>