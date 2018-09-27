<span data-ttu-id="8a93b-101">In questo caso, verranno eseguite alcune operazioni di risoluzione dei problemi di base, ad esempio l'esecuzione del pull sui log dei contenitori e sugli eventi dei contenitori e la connessione a un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="8a93b-101">Here, you will perform some basic troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.</span></span> <span data-ttu-id="8a93b-102">Al termine di questo modulo sarà possibile comprendere le funzionalità di base per la risoluzione dei problemi relativi alle istanze di contenitore.</span><span class="sxs-lookup"><span data-stu-id="8a93b-102">By the end of this module, you should understand basic capabilities for troubleshooting container instances.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="8a93b-103">Creare un contenitore</span><span class="sxs-lookup"><span data-stu-id="8a93b-103">Create a container</span></span>

<span data-ttu-id="8a93b-104">Iniziare dalla creazione del contenitore seguente:</span><span class="sxs-lookup"><span data-stu-id="8a93b-104">Start by creating the following container:</span></span> 

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/sample-aks-helloworld \
    --ports 80 \
    --ip-address Public \
    --location eastus
```

## <a name="get-logs-from-a-container-instance"></a><span data-ttu-id="8a93b-105">Ottenere i log da un'istanza di contenitore</span><span class="sxs-lookup"><span data-stu-id="8a93b-105">Get logs from a container instance</span></span>

<span data-ttu-id="8a93b-106">Per visualizzare i log generati dal codice dell'applicazione all'interno di un contenitore, è possibile usare il comando `az container logs`:</span><span class="sxs-lookup"><span data-stu-id="8a93b-106">To view logs from your application code within a container, you can use the `az container logs` command:</span></span>

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

<span data-ttu-id="8a93b-107">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="8a93b-107">Example output:</span></span>

```output
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
Running inside /app/prestart.sh, you could add migrations to this file, e.g.:

#! /usr/bin/env bash

# Let the DB start
sleep 10;
# Run migrations
alembic upgrade head
```

## <a name="get-container-events"></a><span data-ttu-id="8a93b-108">Ottenere gli eventi del contenitore</span><span class="sxs-lookup"><span data-stu-id="8a93b-108">Get container events</span></span>

<span data-ttu-id="8a93b-109">Il comando `az container attach` fornisce informazioni diagnostiche durante l'avvio del contenitore.</span><span class="sxs-lookup"><span data-stu-id="8a93b-109">The `az container attach` command provides diagnostic information during container startup.</span></span> <span data-ttu-id="8a93b-110">Dopo l'avvio, il contenitore trasmette inoltre STDOUT e STDERR alla console locale:</span><span class="sxs-lookup"><span data-stu-id="8a93b-110">Once the container has started, it also streams STDOUT and STDERR to your local console:</span></span>

```azurecli
az container attach \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

<span data-ttu-id="8a93b-111">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="8a93b-111">Example output:</span></span>

```output
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-09-21 23:48:14+00:00) pulling image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:09+00:00) Successfully pulled image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:12+00:00) Created container
(count: 1) (last timestamp: 2018-09-21 23:49:13+00:00) Started container

Start streaming logs:
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
```

> [!TIP]
> <span data-ttu-id="8a93b-112">Per disconnettersi da un contenitore collegato, premere <kbd>Ctrl + C</kbd>.</span><span class="sxs-lookup"><span data-stu-id="8a93b-112">To disconnect from an attached container, press <kbd>Ctrl+C</kbd>.</span></span>

## <a name="execute-a-command-in-a-container"></a><span data-ttu-id="8a93b-113">Eseguire un comando in un contenitore</span><span class="sxs-lookup"><span data-stu-id="8a93b-113">Execute a command in a container</span></span>

<span data-ttu-id="8a93b-114">Istanze di contenitore di Azure supporta l'esecuzione di un comando in un contenitore in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="8a93b-114">Azure Container Instances supports executing a command in a running container.</span></span> <span data-ttu-id="8a93b-115">Eseguire un comando in un contenitore già avviato è particolarmente utile durante lo sviluppo e la risoluzione dei problemi di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8a93b-115">Running a command in a container you've already started is especially helpful during application development and troubleshooting.</span></span> <span data-ttu-id="8a93b-116">L'uso più comune di questa funzionalità consiste nell'avviare una shell interattiva in modo che sia possibile eseguire il debug di problemi in un contenitore in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="8a93b-116">The most common use of this feature is to launch an interactive shell so that you can debug issues in a running container.</span></span>

<span data-ttu-id="8a93b-117">Questo esempio avvia una sessione interattiva del terminale con il contenitore in esecuzione:</span><span class="sxs-lookup"><span data-stu-id="8a93b-117">This example starts an interactive terminal session with the running container:</span></span>

```azurecli
az container exec \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --exec-command /bin/sh
```

<span data-ttu-id="8a93b-118">Una volta completato il comando, si potrà lavorare all'interno del contenitore.</span><span class="sxs-lookup"><span data-stu-id="8a93b-118">Once the command has completed, you are effectively working inside of the container.</span></span> <span data-ttu-id="8a93b-119">In questo esempio, è stato eseguito il comando `ls` per visualizzare il contenuto della directory di lavoro:</span><span class="sxs-lookup"><span data-stu-id="8a93b-119">In this example, the `ls` command was run to display the contents of the working directory:</span></span>

```output
# ls
__pycache__  main.py  prestart.sh  static  templates  uwsgi.ini
```

<span data-ttu-id="8a93b-120">Eseguire `exit` per arrestare la sessione remota.</span><span class="sxs-lookup"><span data-stu-id="8a93b-120">Run `exit` to stop the remote session.</span></span>

## <a name="monitor-container-cpu-and-memory"></a><span data-ttu-id="8a93b-121">Monitorare CPU e memoria del contenitore</span><span class="sxs-lookup"><span data-stu-id="8a93b-121">Monitor container CPU and memory</span></span>

<span data-ttu-id="8a93b-122">Potrebbe essere utile eseguire il pull di metriche relative all'uso di CPU e memoria.</span><span class="sxs-lookup"><span data-stu-id="8a93b-122">You may want to pull metrics on CPU and memory usage.</span></span> <span data-ttu-id="8a93b-123">A questo scopo, ottenere innanzitutto l'ID dell'istanza di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="8a93b-123">To do so, first get the ID of the Azure container instance.</span></span> <span data-ttu-id="8a93b-124">In questo esempio, l'ID è inserito in una variabile denominata `CONTAINER_ID`:</span><span class="sxs-lookup"><span data-stu-id="8a93b-124">In this example, the ID is placed in a variable named `CONTAINER_ID`:</span></span>

```azurecli
CONTAINER_ID=$(az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name mycontainer --query id --output tsv)
```

<span data-ttu-id="8a93b-125">Usare il comando `az monitor metrics list` per estrarre le informazioni sull'uso della CPU:</span><span class="sxs-lookup"><span data-stu-id="8a93b-125">Now, use the `az monitor metrics list` command to pull back CPU usage information:</span></span>

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric CPUUsage \
    --output table
```

<span data-ttu-id="8a93b-126">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="8a93b-126">Example output:</span></span>

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

<span data-ttu-id="8a93b-127">Il comando seguente può essere usato per ottenere informazioni sull'uso della memoria:</span><span class="sxs-lookup"><span data-stu-id="8a93b-127">The following command can be used to get memory usage information:</span></span>

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric MemoryUsage \
    --output table
```

<span data-ttu-id="8a93b-128">Output di esempio:</span><span class="sxs-lookup"><span data-stu-id="8a93b-128">Example output:</span></span>

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

<span data-ttu-id="8a93b-129">Queste informazioni sono disponibili anche nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="8a93b-129">This information is also available in the Azure portal.</span></span> <span data-ttu-id="8a93b-130">Per visualizzare una rappresentazione grafica delle informazioni relative all'uso di CPU e memoria, visitare la pagina di panoramica del portale di Azure per un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="8a93b-130">To see a visual representation of CPU and memory usage information, visit the Azure portal overview page for a container instance.</span></span>

![Visualizzazione nel portale di Azure delle informazioni relative all'utilizzo di memoria e CPU da parte di Istanze di contenitore di Azure](../media/6-cpu-memory.png)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
