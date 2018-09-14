<span data-ttu-id="995bb-101">In questa unità si creerà un registro contenitori di Azure usando l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="995bb-101">In this unit, you will create an Azure container registry using the Azure CLI.</span></span>

## <a name="create-an-azure-container-registry"></a><span data-ttu-id="995bb-102">Creare un registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="995bb-102">Create an Azure container registry</span></span>

<span data-ttu-id="995bb-103">Prima di creare il registro contenitori di Azure, è necessario un *gruppo di risorse* in cui eseguirne la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="995bb-103">Before you create your Azure container registry, you need a *resource group* to deploy it to.</span></span> <span data-ttu-id="995bb-104">Un gruppo di risorse è una raccolta logica in cui vengono distribuite e gestite tutte le risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="995bb-104">A resource group is a logical collection into which all Azure resources are deployed and managed.</span></span>

<span data-ttu-id="995bb-105">Creare un gruppo di risorse con il comando `az group create`.</span><span class="sxs-lookup"><span data-stu-id="995bb-105">Create a resource group with the `az group create` command.</span></span> <span data-ttu-id="995bb-106">Nell'esempio seguente viene creato un gruppo di risorse denominato *myResourceGroup* nell'area *eastus*:</span><span class="sxs-lookup"><span data-stu-id="995bb-106">In the following example, a resource group named *myResourceGroup* is created in the *eastus* region:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="995bb-107">Dopo aver creato il gruppo di risorse, creare un registro contenitori di Azure con il comando `az acr create`.</span><span class="sxs-lookup"><span data-stu-id="995bb-107">Once you've created the resource group, create an Azure container registry with the `az acr create` command.</span></span> <span data-ttu-id="995bb-108">Il nome del registro contenitori deve essere univoco in Azure e contenere da tra 5 e 50 caratteri alfanumerici.</span><span class="sxs-lookup"><span data-stu-id="995bb-108">The container registry name must be unique within Azure, and contain between 5 and 50 alphanumeric characters.</span></span> <span data-ttu-id="995bb-109">Sostituire `<acrName>` con un nome univoco per il registro.</span><span class="sxs-lookup"><span data-stu-id="995bb-109">Replace `<acrName>` with a unique name for your registry.</span></span>

<span data-ttu-id="995bb-110">Per questo esempio viene distribuito uno SKU del registro Premium.</span><span class="sxs-lookup"><span data-stu-id="995bb-110">For this example, a premium registry SKU is deployed.</span></span> <span data-ttu-id="995bb-111">Lo SKU Premium è obbligatorio per la replica geografica.</span><span class="sxs-lookup"><span data-stu-id="995bb-111">The premium SKU is required for geo-replication.</span></span> <span data-ttu-id="995bb-112">Per altre informazioni sugli SKU del Registro contenitori di Azure, vedere [SKU del Registro contenitori di Azure](https://docs.microsoft.com/azure/container-registry/container-registry-skus)</span><span class="sxs-lookup"><span data-stu-id="995bb-112">For more information on Container Registry SKUs, see, [Azure Container Registry SKUs](https://docs.microsoft.com/azure/container-registry/container-registry-skus)</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Premium
```

<span data-ttu-id="995bb-113">Di seguito è riportato l'output di esempio per un nuovo registro contenitori di Azure:</span><span class="sxs-lookup"><span data-stu-id="995bb-113">Here's example output for a new Azure container registry:</span></span>

```console
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-15T19:19:07.042178+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myACR0007",
  "location": "eastus",
  "loginServer": "myacr.azurecr.io",
  "name": "myACR",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

<span data-ttu-id="995bb-114">Nel resto del modulo viene usato `<acrName>` come segnaposto per il nome del registro contenitori scelto in questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="995bb-114">The rest of this module refers to `<acrName>` as a placeholder for the container registry name that you chose in this step.</span></span>

## <a name="summary"></a><span data-ttu-id="995bb-115">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="995bb-115">Summary</span></span>

<span data-ttu-id="995bb-116">In questa unità è stato creato un registro contenitori di Azure usando l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="995bb-116">In this unit, you created an Azure container registry using the Azure CLI.</span></span>