<span data-ttu-id="e7717-101">La società include carichi di lavoro di calcolo distribuiti in diverse aree geografiche, in modo da assicurare una presenza locale per la gestione della clientela distribuita.</span><span class="sxs-lookup"><span data-stu-id="e7717-101">Your company has compute workloads deployed to several regions to make sure you have a local presence to serve your distributed customer base.</span></span> 

<span data-ttu-id="e7717-102">L'obiettivo consiste nell'inserire un registro contenitori in ogni area in cui vengono eseguite le immagini.</span><span class="sxs-lookup"><span data-stu-id="e7717-102">Your aim is to place a container registry in each region where images are run.</span></span> <span data-ttu-id="e7717-103">Questa strategia consentirà di eseguire operazioni di rete vicina, offrendo trasferimenti a livello di immagine veloci e affidabili.</span><span class="sxs-lookup"><span data-stu-id="e7717-103">This strategy will allow for network-close operations, enabling fast, reliable image layer transfers.</span></span> 

<span data-ttu-id="e7717-104">La replica geografica consente al registro contenitori di Azure di fungere da singolo registro in modo da servire più aree con registri regionali multimaster.</span><span class="sxs-lookup"><span data-stu-id="e7717-104">Geo-replication enables an Azure container registry to function as a single registry, serving several regions with multi-master regional registries.</span></span>

<span data-ttu-id="e7717-105">Un registro con replica geografica è caratterizzato dai vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e7717-105">A geo-replicated registry provides the following benefits:</span></span>

- <span data-ttu-id="e7717-106">I nomi di registro/immagine/tag singoli possono essere usati in più aree</span><span class="sxs-lookup"><span data-stu-id="e7717-106">Single registry/image/tag names can be used across multiple regions</span></span>
- <span data-ttu-id="e7717-107">Accesso al registro in una posizione di rete vicina da distribuzioni internazionali</span><span class="sxs-lookup"><span data-stu-id="e7717-107">Network-close registry access from regional deployments</span></span>
- <span data-ttu-id="e7717-108">Nessun costo aggiuntivo per il traffico in uscita perché viene eseguito il pull delle immagini da un registro replicato in locale nella stessa area dell'host del contenitore</span><span class="sxs-lookup"><span data-stu-id="e7717-108">No additional egress fees, as images are pulled from a local, replicated registry in the same region as your container host</span></span>
- <span data-ttu-id="e7717-109">Gestione unica di un registro in più aree</span><span class="sxs-lookup"><span data-stu-id="e7717-109">Single management of a registry across multiple regions</span></span>

## <a name="replicate-an-image-to-multiple-locations"></a><span data-ttu-id="e7717-110">Replica di un'immagine in più posizioni</span><span class="sxs-lookup"><span data-stu-id="e7717-110">Replicate an image to multiple locations</span></span>

<span data-ttu-id="e7717-111">Verrà usato il comando `az acr replication create` dell'interfaccia della riga di comando di Azure per replicare le immagini del contenitore da un'area a un'altra.</span><span class="sxs-lookup"><span data-stu-id="e7717-111">You'll use the `az acr replication create` Azure CLI command to replicate your container images from one region to another.</span></span> <span data-ttu-id="e7717-112">In questo esempio verrà creata una replica per l'area `japaneast`.</span><span class="sxs-lookup"><span data-stu-id="e7717-112">In this example, you'll create a replication for the `japaneast` region.</span></span> <span data-ttu-id="e7717-113">Aggiornare `<acrName>` con il nome del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="e7717-113">Update `<acrName>` with the name of your Container Registry.</span></span>

```azurecli
az acr replication create --registry <acrName> --location japaneast
```

<span data-ttu-id="e7717-114">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e7717-114">The output should look similar to the following:</span></span>

```console
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myACR0007/replications/japaneast",
  "location": "japaneast",
  "name": "japaneast",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "status": {
    "displayStatus": "Syncing",
    "message": null,
    "timestamp": "2018-08-15T20:22:09.275792+00:00"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries/replications"
}
```

<span data-ttu-id="e7717-115">È necessario un passaggio finale.</span><span class="sxs-lookup"><span data-stu-id="e7717-115">As a final step.</span></span> <span data-ttu-id="e7717-116">È possibile recuperare tutte le repliche delle immagini del contenitore create.</span><span class="sxs-lookup"><span data-stu-id="e7717-116">You are able to retrieve all container image replicas created.</span></span> <span data-ttu-id="e7717-117">Verrà usato il comando `az acr replication list` per recuperare l'elenco.</span><span class="sxs-lookup"><span data-stu-id="e7717-117">You'll use the `az acr replication list` command to retrieve this list.</span></span> <span data-ttu-id="e7717-118">Aggiornare `<acrName>` con il nome del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="e7717-118">Update `<acrName>` with the name of your Container Registry.</span></span>

```azurecli
az acr replication list --registry <acrName> --output table
```

<span data-ttu-id="e7717-119">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e7717-119">The output should look similar to the following:</span></span>

```console
NAME       LOCATION    PROVISIONING STATE    STATUS
---------  ----------  --------------------  --------
japaneast  japaneast   Succeeded             Ready
eastus     eastus      Succeeded             Ready
```

<span data-ttu-id="e7717-120">Occorre ricordare che non si è limitati all'interfaccia della riga di comando di Azure per elencare le repliche delle immagini.</span><span class="sxs-lookup"><span data-stu-id="e7717-120">Keep in mind that you are not limited to the Azure CLI to list your image replicas.</span></span> <span data-ttu-id="e7717-121">Nel portale di Azure selezionare `Replications` per un registro contenitori di Azure per visualizzare una mappa che illustra in dettaglio le repliche correnti.</span><span class="sxs-lookup"><span data-stu-id="e7717-121">From within the Azure portal, selecting `Replications` for an Azure Container Registry displays a map that details current replications.</span></span> <span data-ttu-id="e7717-122">Le immagini del contenitore possono essere replicate in altre aree geografiche selezionando le aree sulla mappa.</span><span class="sxs-lookup"><span data-stu-id="e7717-122">Container images can be replicated to additional regions by selecting the regions on the map.</span></span>

![Mappa di replica dei contenitori come visualizzata nel portale di Azure](../media/replication-map.png)

## <a name="clean-up-your-resources"></a><span data-ttu-id="e7717-124">Pulire le risorse</span><span class="sxs-lookup"><span data-stu-id="e7717-124">Clean up your resources</span></span>
<!---TODO: Do we need to include cleanup for the free education tier?--->

<span data-ttu-id="e7717-125">A questo punto è possibile pulire le risorse create eliminando il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e7717-125">At this point, you can cleanup the created resources by deleting the resource group.</span></span> <span data-ttu-id="e7717-126">A questo scopo, usare il comando `az group delete`.</span><span class="sxs-lookup"><span data-stu-id="e7717-126">To do so, use the `az group delete` command.</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a><span data-ttu-id="e7717-127">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e7717-127">Summary</span></span>

<span data-ttu-id="e7717-128">È stata completata la replica di un'immagine del contenitore in più data center di Azure mediante l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="e7717-128">You've now successfully replicated a container image to multiple Azure datacenters using the Azure CLI.</span></span> 