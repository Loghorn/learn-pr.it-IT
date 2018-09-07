<span data-ttu-id="bf7c3-101">La semplicità e la velocità della distribuzione di contenitori in Istanze di contenitore di Azure consentono di ottenere una piattaforma interessante per l'esecuzione di attività eseguite una sola volta come la compilazione, il test e il rendering di immagini in un'istanza del contenitore.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-101">The ease and speed of deploying containers in Azure Container Instances provides a compelling platform for executing run-once tasks like build, test, and image rendering in a container instance.</span></span>

<span data-ttu-id="bf7c3-102">Con un criterio di riavvio configurabile, è possibile specificare l'arresto dei contenitori al completamento dei processi.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-102">With a configurable restart policy, you can specify that your containers are stopped when their processes have completed.</span></span> <span data-ttu-id="bf7c3-103">Poiché le istanze del contenitore vengono fatturate al secondo, vengono addebitate solo le risorse di calcolo usate mentre il contenitore che esegue l'attività è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-103">Because container instances are billed by the second, you're charged only for the compute resources used while the container executing your task is running.</span></span>

## <a name="container-restart-policies"></a><span data-ttu-id="bf7c3-104">Criteri di riavvio del contenitore</span><span class="sxs-lookup"><span data-stu-id="bf7c3-104">Container restart policies</span></span>

<span data-ttu-id="bf7c3-105">Quando si crea un contenitore in Istanze di contenitore di Azure, è possibile specificare una delle tre impostazioni dei criteri di riavvio.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-105">When you create a container in Azure Container Instances, you can specify one of three restart policy settings.</span></span>

| <span data-ttu-id="bf7c3-106">Criterio di riavvio</span><span class="sxs-lookup"><span data-stu-id="bf7c3-106">Restart policy</span></span>   | <span data-ttu-id="bf7c3-107">Descrizione</span><span class="sxs-lookup"><span data-stu-id="bf7c3-107">Description</span></span> |
| ---------------- | :---------- |
| `Always` | <span data-ttu-id="bf7c3-108">I contenitori nel gruppo contenitore vengono sempre riavviati.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-108">Containers in the container group are always restarted.</span></span> <span data-ttu-id="bf7c3-109">Questa è l'impostazione **predefinita** applicata quando non si specifica alcun criterio di riavvio al momento della creazione del contenitore.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-109">This is the **default** setting applied when no restart policy is specified at container creation.</span></span> |
| `Never` | <span data-ttu-id="bf7c3-110">I contenitori nel gruppo contenitore non vengono riavviati mai.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-110">Containers in the container group are never restarted.</span></span> <span data-ttu-id="bf7c3-111">I contenitori vengono eseguiti al massimo una volta.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-111">The containers run at most once.</span></span> |
| `OnFailure` | <span data-ttu-id="bf7c3-112">I contenitori nel gruppo contenitore vengono riavviati solo quando il processo eseguito nel contenitore ha esito negativo, ovvero quando termina con un codice di uscita diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-112">Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code).</span></span> <span data-ttu-id="bf7c3-113">I contenitori vengono eseguiti almeno una volta.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-113">The containers are run at least once.</span></span> |

<span data-ttu-id="bf7c3-114">Nell'unità di precedente di questo modulo è stato creato un contenitore senza un criterio di riavvio specifico.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-114">In the previous unit of this module a container was created without a specified restart policy.</span></span> <span data-ttu-id="bf7c3-115">Per impostazione predefinita, a questo contenitore è stato applicato il criterio di riavvio *Always*.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-115">By default, this container received the *Always* restart policy.</span></span> <span data-ttu-id="bf7c3-116">Poiché il carico di lavoro presuppone un'esecuzione prolungata (server Web), questo criterio ha senso.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-116">Because the workload in the container is long running (a webserver), this policy makes sense.</span></span>

## <a name="run-to-completion"></a><span data-ttu-id="bf7c3-117">Eseguire fino al completamento</span><span class="sxs-lookup"><span data-stu-id="bf7c3-117">Run to completion</span></span>

<span data-ttu-id="bf7c3-118">Per visualizzare i criteri di riavvio in azione, creare un'istanza del contenitore dall'immagine *microsoft/aci-wordcount* e specificare il criterio di riavvio *OnFailure*.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-118">To see the restart policy in action, create a container instance from the *microsoft/aci-wordcount* image, and specify the *OnFailure* restart policy.</span></span> <span data-ttu-id="bf7c3-119">Questo contenitore di esempio esegue uno script di Python che analizza il testo Amleto di Shakespeare, scrive le 10 parole più comuni in STDOUT ed esce.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-119">This example container runs a Python script that analyzes the text of Shakespeare's Hamlet, writes the 10 most common words to STDOUT, and then exits.</span></span>

<span data-ttu-id="bf7c3-120">Eseguire il contenitore di esempio con il comando `az container create` seguente:</span><span class="sxs-lookup"><span data-stu-id="bf7c3-120">Run the example container with the following `az container create` command:</span></span>

```azureclu
az container create \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

<span data-ttu-id="bf7c3-121">Istanze di contenitore di Azure avvia il contenitore e lo interrompe quando la sua applicazione, o lo script in questo caso, esce.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-121">Azure Container Instances starts the container, and then stops it when its application (or script, in this case) exits.</span></span> <span data-ttu-id="bf7c3-122">Quando Istanze di contenitore di Azure arresta un contenitore i cui criteri di riavvio sono *Never* o *OnFailure*, lo stato del contenitore viene impostato su Terminato.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-122">When Azure Container Instances stops a container whose restart policy is *Never* or *OnFailure*, the container's status is set to Terminated.</span></span>

<span data-ttu-id="bf7c3-123">È possibile controllare lo stato del contenitore usando il comando `az container show`:</span><span class="sxs-lookup"><span data-stu-id="bf7c3-123">You can check a container's status with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

<span data-ttu-id="bf7c3-124">Quando lo stato del contenitore di esempio mostra Terminato, è possibile visualizzare l'output dell'attività visualizzando i log dei contenitori.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-124">Once the example container's status shows Terminated, you can see its task output by viewing the container logs.</span></span> <span data-ttu-id="bf7c3-125">Eseguire il comando az container logs per visualizzare l'output dello script:</span><span class="sxs-lookup"><span data-stu-id="bf7c3-125">Run the az container logs command to view the script's output:</span></span>

```azurecli
az container logs --resource-group myResourceGroup --name mycontainer-restart-demo
```

<span data-ttu-id="bf7c3-126">Output:</span><span class="sxs-lookup"><span data-stu-id="bf7c3-126">Output:</span></span>

```bash
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

## <a name="summary"></a><span data-ttu-id="bf7c3-127">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="bf7c3-127">Summary</span></span>

<span data-ttu-id="bf7c3-128">In questa unità è stata creata un'istanza di contenitore con il criterio di riavvio *OnFailure*.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-128">In this unit, you created a container instance with a restart policy of *OnFailure*.</span></span> <span data-ttu-id="bf7c3-129">Questa configurazione funziona per i contenitori che eseguono attività di breve durata.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-129">This configuration works well for containers that run short lived tasks.</span></span>

<span data-ttu-id="bf7c3-130">Nell'unità successiva sarà possibile impostare variabili di ambiente in un'Istanza di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="bf7c3-130">In the next unit, you will set environment variables in an Azure Container Instance.</span></span>
