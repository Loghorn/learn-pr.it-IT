<span data-ttu-id="1ad61-101">La semplicità e rapidità di distribuzione di contenitori in istanze di contenitore di Azure fornisce una piattaforma interessante per l'esecuzione di attività eseguire una sola volta, ad esempio compilazione, test e il rendering delle immagini.</span><span class="sxs-lookup"><span data-stu-id="1ad61-101">The ease and speed of deploying containers in Azure Container Instances provides a compelling platform for executing run-once tasks like build, test, and image rendering.</span></span>

<span data-ttu-id="1ad61-102">Con un criterio di riavvio configurabile, è possibile specificare l'arresto dei contenitori al completamento dei processi.</span><span class="sxs-lookup"><span data-stu-id="1ad61-102">With a configurable restart policy, you can specify that your containers are stopped when their processes have completed.</span></span> <span data-ttu-id="1ad61-103">Poiché le istanze di contenitore vengono fatturate al secondo, ti viene addebitata solo per le risorse di calcolo usate mentre il contenitore è in esecuzione che l'attività è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1ad61-103">Because container instances are billed by the second, you're charged only for the compute resources used while the container is executing your task is running.</span></span>

## <a name="container-restart-policies"></a><span data-ttu-id="1ad61-104">Criteri di riavvio del contenitore</span><span class="sxs-lookup"><span data-stu-id="1ad61-104">Container restart policies</span></span>

<span data-ttu-id="1ad61-105">Istanze di contenitore di Azure sono disponibili tre opzioni di criteri di riavvio:</span><span class="sxs-lookup"><span data-stu-id="1ad61-105">Azure Container Instances has three restart-policy options:</span></span>

| <span data-ttu-id="1ad61-106">Criterio di riavvio</span><span class="sxs-lookup"><span data-stu-id="1ad61-106">Restart policy</span></span>   | <span data-ttu-id="1ad61-107">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1ad61-107">Description</span></span> |
| ---------------- | :---------- |
| `Always` | <span data-ttu-id="1ad61-108">I contenitori nel gruppo contenitore vengono sempre riavviati.</span><span class="sxs-lookup"><span data-stu-id="1ad61-108">Containers in the container group are always restarted.</span></span> <span data-ttu-id="1ad61-109">Questo criterio ha senso per attività a esecuzione prolungata, ad esempio un server web.</span><span class="sxs-lookup"><span data-stu-id="1ad61-109">This policy makes sense for long-running tasks such as a web server.</span></span> <span data-ttu-id="1ad61-110">Questa è l'impostazione **predefinita** applicata quando non si specifica alcun criterio di riavvio al momento della creazione del contenitore.</span><span class="sxs-lookup"><span data-stu-id="1ad61-110">This is the **default** setting applied when no restart policy is specified at container creation.</span></span> |
| `Never` | <span data-ttu-id="1ad61-111">I contenitori nel gruppo contenitore non vengono riavviati mai.</span><span class="sxs-lookup"><span data-stu-id="1ad61-111">Containers in the container group are never restarted.</span></span> <span data-ttu-id="1ad61-112">I contenitori vengono eseguiti al massimo una volta.</span><span class="sxs-lookup"><span data-stu-id="1ad61-112">The containers run at most once.</span></span> |
| `OnFailure` | <span data-ttu-id="1ad61-113">I contenitori nel gruppo contenitore vengono riavviati solo quando il processo eseguito nel contenitore ha esito negativo, ovvero quando termina con un codice di uscita diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="1ad61-113">Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code).</span></span> <span data-ttu-id="1ad61-114">I contenitori vengono eseguiti almeno una volta.</span><span class="sxs-lookup"><span data-stu-id="1ad61-114">The containers are run at least once.</span></span> <span data-ttu-id="1ad61-115">Questo criterio funziona bene per i contenitori che eseguono le attività di breve durate.</span><span class="sxs-lookup"><span data-stu-id="1ad61-115">This policy works well for containers that run short-lived tasks.</span></span> |

## <a name="run-to-completion"></a><span data-ttu-id="1ad61-116">Eseguire fino al completamento</span><span class="sxs-lookup"><span data-stu-id="1ad61-116">Run to completion</span></span>

<span data-ttu-id="1ad61-117">Per visualizzare i criteri di riavvio in azione, creare un'istanza del contenitore dall'immagine *microsoft/aci-wordcount* e specificare il criterio di riavvio *OnFailure*.</span><span class="sxs-lookup"><span data-stu-id="1ad61-117">To see the restart policy in action, create a container instance from the *microsoft/aci-wordcount* image and specify the *OnFailure* restart policy.</span></span> <span data-ttu-id="1ad61-118">Questo contenitore di esempio esegue uno script di Python che analizza il testo Amleto di Shakespeare, scrive le 10 parole più comuni in STDOUT ed esce.</span><span class="sxs-lookup"><span data-stu-id="1ad61-118">This example container runs a Python script that analyzes the text of Shakespeare's Hamlet, writes the 10 most common words to STDOUT, and then exits.</span></span>

<span data-ttu-id="1ad61-119">Eseguire il contenitore di esempio con il comando `az container create` seguente:</span><span class="sxs-lookup"><span data-stu-id="1ad61-119">Run the example container with the following `az container create` command:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

<span data-ttu-id="1ad61-120">Istanze di contenitore di Azure avvia il contenitore e lo interrompe quando la sua applicazione, o lo script in questo caso, esce.</span><span class="sxs-lookup"><span data-stu-id="1ad61-120">Azure Container Instances starts the container and then stops it when its application (or script, in this case) exits.</span></span> <span data-ttu-id="1ad61-121">Quando Istanze di contenitore di Azure arresta un contenitore i cui criteri di riavvio sono *Never* o *OnFailure*, lo stato del contenitore viene impostato su **Terminato**.</span><span class="sxs-lookup"><span data-stu-id="1ad61-121">When Azure Container Instances stops a container whose restart policy is *Never* or *OnFailure*, the container's status is set to **Terminated**.</span></span>

<span data-ttu-id="1ad61-122">È possibile controllare lo stato del contenitore usando il comando `az container show`:</span><span class="sxs-lookup"><span data-stu-id="1ad61-122">You can check a container's status with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

<span data-ttu-id="1ad61-123">Quando lo stato del contenitore di esempio mostra **Terminato**, è possibile visualizzare l'output dell'attività visualizzando i log dei contenitori.</span><span class="sxs-lookup"><span data-stu-id="1ad61-123">Once the example container's status shows **Terminated**, you can see its task output by viewing the container logs.</span></span> <span data-ttu-id="1ad61-124">Eseguire il comando **az container logs** per visualizzare l'output dello script:</span><span class="sxs-lookup"><span data-stu-id="1ad61-124">Run the **az container logs** command to view the script's output:</span></span>

```azurecli
az container logs --resource-group <rgn>[Sandbox resource group name]</rgn> --name mycontainer-restart-demo
```

<span data-ttu-id="1ad61-125">Output:</span><span class="sxs-lookup"><span data-stu-id="1ad61-125">Output:</span></span>

```json
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