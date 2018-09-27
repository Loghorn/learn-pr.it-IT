In questo caso, verranno eseguite alcune operazioni di risoluzione dei problemi di base, ad esempio l'esecuzione del pull sui log dei contenitori e sugli eventi dei contenitori e la connessione a un'istanza di contenitore. Al termine di questo modulo sarà possibile comprendere le funzionalità di base per la risoluzione dei problemi relativi alle istanze di contenitore.

## <a name="create-a-container"></a>Creare un contenitore

Iniziare dalla creazione del contenitore seguente: 

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/sample-aks-helloworld \
    --ports 80 \
    --ip-address Public \
    --location eastus
```

## <a name="get-logs-from-a-container-instance"></a>Ottenere i log da un'istanza di contenitore

Per visualizzare i log generati dal codice dell'applicazione all'interno di un contenitore, è possibile usare il comando `az container logs`:

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

Output di esempio:

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

## <a name="get-container-events"></a>Ottenere gli eventi del contenitore

Il comando `az container attach` fornisce informazioni diagnostiche durante l'avvio del contenitore. Dopo l'avvio, il contenitore trasmette inoltre STDOUT e STDERR alla console locale:

```azurecli
az container attach \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

Output di esempio:

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
> Per disconnettersi da un contenitore collegato, premere <kbd>Ctrl + C</kbd>.

## <a name="execute-a-command-in-a-container"></a>Eseguire un comando in un contenitore

Istanze di contenitore di Azure supporta l'esecuzione di un comando in un contenitore in esecuzione. Eseguire un comando in un contenitore già avviato è particolarmente utile durante lo sviluppo e la risoluzione dei problemi di un'applicazione. L'uso più comune di questa funzionalità consiste nell'avviare una shell interattiva in modo che sia possibile eseguire il debug di problemi in un contenitore in esecuzione.

Questo esempio avvia una sessione interattiva del terminale con il contenitore in esecuzione:

```azurecli
az container exec \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --exec-command /bin/sh
```

Una volta completato il comando, si potrà lavorare all'interno del contenitore. In questo esempio, è stato eseguito il comando `ls` per visualizzare il contenuto della directory di lavoro:

```output
# ls
__pycache__  main.py  prestart.sh  static  templates  uwsgi.ini
```

Eseguire `exit` per arrestare la sessione remota.

## <a name="monitor-container-cpu-and-memory"></a>Monitorare CPU e memoria del contenitore

Potrebbe essere utile eseguire il pull di metriche relative all'uso di CPU e memoria. A questo scopo, ottenere innanzitutto l'ID dell'istanza di contenitore di Azure. In questo esempio, l'ID è inserito in una variabile denominata `CONTAINER_ID`:

```azurecli
CONTAINER_ID=$(az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name mycontainer --query id --output tsv)
```

Usare il comando `az monitor metrics list` per estrarre le informazioni sull'uso della CPU:

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric CPUUsage \
    --output table
```

Output di esempio:

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

Il comando seguente può essere usato per ottenere informazioni sull'uso della memoria:

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric MemoryUsage \
    --output table
```

Output di esempio:

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

Queste informazioni sono disponibili anche nel portale di Azure. Per visualizzare una rappresentazione grafica delle informazioni relative all'uso di CPU e memoria, visitare la pagina di panoramica del portale di Azure per un'istanza di contenitore.

![Visualizzazione nel portale di Azure delle informazioni relative all'utilizzo di memoria e CPU da parte di Istanze di contenitore di Azure](../media/6-cpu-memory.png)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
