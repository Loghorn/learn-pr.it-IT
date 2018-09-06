<span data-ttu-id="a58bb-101">Come procedura consigliata, l'inserimento di un registro contenitori in ogni area in cui vengono eseguite le immagini consente l'esecuzione di operazioni in posizioni di rete vicine e quindi di trasferimenti di livelli di immagine più veloci e affidabili.</span><span class="sxs-lookup"><span data-stu-id="a58bb-101">As a best practice, placing a container registry in each region where images are run allows network-close operations, enabling fast, reliable image layer transfers.</span></span> <span data-ttu-id="a58bb-102">La replica geografica consente al registro contenitori di Azure di fungere da singolo registro in modo da servire più aree con registri regionali multimaster.</span><span class="sxs-lookup"><span data-stu-id="a58bb-102">Geo-replication enables an Azure container registry to function as a single registry, serving multiple regions with multi-master regional registries.</span></span>

<span data-ttu-id="a58bb-103">Un registro con replica geografica è caratterizzato dai vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a58bb-103">A geo-replicated registry provides the following benefits:</span></span>

* <span data-ttu-id="a58bb-104">I nomi di registro/immagine/tag singoli possono essere usati in più aree</span><span class="sxs-lookup"><span data-stu-id="a58bb-104">Single registry/image/tag names can be used across multiple regions</span></span>
* <span data-ttu-id="a58bb-105">Accesso al registro in una posizione di rete vicina da distribuzioni regionali</span><span class="sxs-lookup"><span data-stu-id="a58bb-105">Network-close registry access from regional deployments</span></span>
* <span data-ttu-id="a58bb-106">Niente corrispettivi aggiuntivi per il traffico in uscita perché il pull delle immagini viene eseguito da un registro replicato in locale nella stessa area dell'host del contenitore</span><span class="sxs-lookup"><span data-stu-id="a58bb-106">No additional egress fees because images are pulled from a local, replicated registry in the same region as your container host</span></span>
* <span data-ttu-id="a58bb-107">Gestione unica di un registro in più aree</span><span class="sxs-lookup"><span data-stu-id="a58bb-107">Single management of a registry across multiple regions</span></span>

## <a name="replicate-image-to-multiple-locations"></a><span data-ttu-id="a58bb-108">Replica di immagini in più posizioni</span><span class="sxs-lookup"><span data-stu-id="a58bb-108">Replicate image to multiple locations</span></span>

<span data-ttu-id="a58bb-109">Usare il comando `az acr replication create` per replicare le immagini del contenitore in un'altra area.</span><span class="sxs-lookup"><span data-stu-id="a58bb-109">Use the `az acr replication create` command to replicate your container images to another region.</span></span> <span data-ttu-id="a58bb-110">In questo esempio viene creata una replica per l'area `japaneast`.</span><span class="sxs-lookup"><span data-stu-id="a58bb-110">In this example, a replication is created for the `japaneast` region.</span></span> <span data-ttu-id="a58bb-111">Aggiornare `<acrName>` con il nome del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="a58bb-111">Update `<acrName>` with the name of your container registry.</span></span>

```azurecli
az acr replication create --registry <acrName> --location japaneast
```

<span data-ttu-id="a58bb-112">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a58bb-112">The output should look similar to the following:</span></span>

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

<span data-ttu-id="a58bb-113">Per visualizzare un elenco di tutte le repliche dell'immagine del contenitore, usare il comando `az acr replication list`.</span><span class="sxs-lookup"><span data-stu-id="a58bb-113">To see a list of all container image replicas, use the `az acr replication list` command.</span></span> <span data-ttu-id="a58bb-114">Aggiornare `<acrName>` con il nome del registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="a58bb-114">Update `<acrName>` with the name of your container registry.</span></span>

```azurecli
az acr replication list --registry <acrName> --output table
```

<span data-ttu-id="a58bb-115">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a58bb-115">The output should look similar to the following:</span></span>

```console
NAME       LOCATION    PROVISIONING STATE    STATUS
---------  ----------  --------------------  --------
japaneast  japaneast   Succeeded             Ready
eastus     eastus      Succeeded             Ready
```

<span data-ttu-id="a58bb-116">Anche se in questo esercizio non è stato usato il portale di Azure, è utile vedere l'esperienza del portale.</span><span class="sxs-lookup"><span data-stu-id="a58bb-116">While the Azure portal was not used in this exercise, it is neat to see the portal experience.</span></span>

<span data-ttu-id="a58bb-117">Nel portale di Azure selezionare `Replications` per un registro contenitori di Azure per visualizzare una mappa che illustra in dettaglio le repliche correnti.</span><span class="sxs-lookup"><span data-stu-id="a58bb-117">From within the Azure portal, selecting `Replications` for an Azure container registry displays a map that details current replications.</span></span> <span data-ttu-id="a58bb-118">Le immagini del contenitore possono essere replicate in altre aree geografiche selezionando le aree sulla mappa.</span><span class="sxs-lookup"><span data-stu-id="a58bb-118">Container images can be replicated to additional regions by selecting the regions on the map.</span></span>

![Mappa di replica dei contenitori come visualizzata nel portale di Azure](../media/replication-map.png)

## <a name="clean-up"></a><span data-ttu-id="a58bb-120">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="a58bb-120">Clean up</span></span>

<span data-ttu-id="a58bb-121">Questa è l'ultima unità del modulo di apprendimento sul Registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="a58bb-121">This is the last unit of the Azure Container Registry learning module.</span></span> <span data-ttu-id="a58bb-122">A questo punto è possibile pulire le risorse create eliminando il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="a58bb-122">At this point, you can clean up the created resources by deleting the resource group.</span></span> <span data-ttu-id="a58bb-123">A questo scopo, usare il comando `az group delete`.</span><span class="sxs-lookup"><span data-stu-id="a58bb-123">To do so, use the `az group delete` command.</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a><span data-ttu-id="a58bb-124">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a58bb-124">Summary</span></span>

<span data-ttu-id="a58bb-125">In questo modulo si è eseguita la replica di un'immagine del contenitore in più aree di Azure tramite l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="a58bb-125">In this module, you replicated a container image to multiple Azure regions using the Azure CLI.</span></span>