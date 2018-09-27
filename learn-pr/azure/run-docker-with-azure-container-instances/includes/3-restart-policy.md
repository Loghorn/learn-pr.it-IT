<span data-ttu-id="60b07-101">La semplicità e la velocità della distribuzione di contenitori in Istanze di contenitore di Azure lo rendono particolarmente adatto per le attività che vengono eseguite una sola volta, come la compilazione, il test e il rendering di immagini.</span><span class="sxs-lookup"><span data-stu-id="60b07-101">The ease and speed of deploying containers in Azure Container Instances makes it a great fit for executing run-once tasks like build, test, and image rendering.</span></span>

<span data-ttu-id="60b07-102">Con un criterio di riavvio configurabile, è possibile specificare l'arresto dei contenitori al completamento dei processi.</span><span class="sxs-lookup"><span data-stu-id="60b07-102">With a configurable restart policy, you can specify that your containers are stopped when their processes have completed.</span></span> <span data-ttu-id="60b07-103">Poiché le istanze del contenitore vengono fatturate al secondo, vengono addebitate solo le risorse di calcolo usate mentre il contenitore che esegue l'attività è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="60b07-103">Because container instances are billed by the second, you're charged only for the compute resources used while the container executing your task is running.</span></span>

## <a name="container-restart-policies"></a><span data-ttu-id="60b07-104">Criteri di riavvio del contenitore</span><span class="sxs-lookup"><span data-stu-id="60b07-104">Container restart policies</span></span>

<span data-ttu-id="60b07-105">Istanze di contenitore di Azure ha tre opzioni per i criteri di riavvio:</span><span class="sxs-lookup"><span data-stu-id="60b07-105">Azure Container Instances has three restart-policy options:</span></span>

| <span data-ttu-id="60b07-106">Criterio di riavvio</span><span class="sxs-lookup"><span data-stu-id="60b07-106">Restart policy</span></span>   | <span data-ttu-id="60b07-107">Descrizione</span><span class="sxs-lookup"><span data-stu-id="60b07-107">Description</span></span> |
| ---------------- | :---------- |
| `Always` | <span data-ttu-id="60b07-108">I contenitori nel gruppo di contenitori vengono sempre riavviati.</span><span class="sxs-lookup"><span data-stu-id="60b07-108">Containers in the container group are always restarted.</span></span> <span data-ttu-id="60b07-109">Questo criterio ha senso per le attività con esecuzione prolungata, ad esempio un server Web.</span><span class="sxs-lookup"><span data-stu-id="60b07-109">This policy makes sense for long-running tasks such as a web server.</span></span> <span data-ttu-id="60b07-110">Questa è l'impostazione **predefinita** applicata quando non si specifica alcun criterio di riavvio al momento della creazione del contenitore.</span><span class="sxs-lookup"><span data-stu-id="60b07-110">This is the **default** setting applied when no restart policy is specified at container creation.</span></span> |
| `Never` | <span data-ttu-id="60b07-111">I contenitori nel gruppo contenitore non vengono riavviati mai.</span><span class="sxs-lookup"><span data-stu-id="60b07-111">Containers in the container group are never restarted.</span></span> <span data-ttu-id="60b07-112">I contenitori vengono eseguiti al massimo una volta.</span><span class="sxs-lookup"><span data-stu-id="60b07-112">The containers run at most once.</span></span> |
| `OnFailure` | <span data-ttu-id="60b07-113">I contenitori nel gruppo contenitore vengono riavviati solo quando il processo eseguito nel contenitore ha esito negativo, ovvero quando termina con un codice di uscita diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="60b07-113">Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code).</span></span> <span data-ttu-id="60b07-114">I contenitori vengono eseguiti almeno una volta.</span><span class="sxs-lookup"><span data-stu-id="60b07-114">The containers are run at least once.</span></span> <span data-ttu-id="60b07-115">Questo criterio funziona per i contenitori che eseguono attività di breve durata.</span><span class="sxs-lookup"><span data-stu-id="60b07-115">This policy works well for containers that run short-lived tasks.</span></span> |

## <a name="run-to-completion"></a><span data-ttu-id="60b07-116">Eseguire fino al completamento</span><span class="sxs-lookup"><span data-stu-id="60b07-116">Run to completion</span></span>

<span data-ttu-id="60b07-117">Per visualizzare i criteri di riavvio in azione, creare un'istanza del contenitore dall'immagine *microsoft/aci-wordcount* e specificare il criterio di riavvio *OnFailure*.</span><span class="sxs-lookup"><span data-stu-id="60b07-117">To see the restart policy in action, create a container instance from the *microsoft/aci-wordcount* image and specify the *OnFailure* restart policy.</span></span> <span data-ttu-id="60b07-118">Questo contenitore di esempio esegue uno script di Python che analizza il testo Amleto di Shakespeare, scrive le 10 parole più comuni in STDOUT ed esce.</span><span class="sxs-lookup"><span data-stu-id="60b07-118">This example container runs a Python script that analyzes the text of Shakespeare's Hamlet, writes the 10 most common words to STDOUT, and then exits.</span></span>

<span data-ttu-id="60b07-119">Eseguire il contenitore di esempio con il comando `az container create` seguente:</span><span class="sxs-lookup"><span data-stu-id="60b07-119">Run the example container with the following `az container create` command:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --location eastus
```

<span data-ttu-id="60b07-120">Istanze di contenitore di Azure avvia il contenitore e lo interrompe quando la sua applicazione, o lo script in questo caso, esce.</span><span class="sxs-lookup"><span data-stu-id="60b07-120">Azure Container Instances starts the container then stops it when its application (or script, in this case) exits.</span></span> <span data-ttu-id="60b07-121">Quando Istanze di contenitore di Azure arresta un contenitore i cui criteri di riavvio sono *Never* o *OnFailure*, lo stato del contenitore viene impostato su **Terminato**.</span><span class="sxs-lookup"><span data-stu-id="60b07-121">When Azure Container Instances stops a container whose restart policy is *Never* or *OnFailure*, the container's status is set to **Terminated**.</span></span>

<span data-ttu-id="60b07-122">È possibile controllare lo stato del contenitore usando il comando `az container show`:</span><span class="sxs-lookup"><span data-stu-id="60b07-122">You can check a container's status with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

<span data-ttu-id="60b07-123">Quando lo stato del contenitore di esempio mostra **Terminato**, è possibile visualizzare l'output dell'attività visualizzando i log dei contenitori.</span><span class="sxs-lookup"><span data-stu-id="60b07-123">Once the example container's status shows **Terminated**, you can see its task output by viewing the container logs.</span></span> <span data-ttu-id="60b07-124">Eseguire il comando `az container logs` per visualizzare l'output dello script:</span><span class="sxs-lookup"><span data-stu-id="60b07-124">Run the `az container logs` command to view the script's output:</span></span>

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo
```

<span data-ttu-id="60b07-125">Output:</span><span class="sxs-lookup"><span data-stu-id="60b07-125">Output:</span></span>

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