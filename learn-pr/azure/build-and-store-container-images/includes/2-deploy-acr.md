<span data-ttu-id="857f9-101">In questa unità si creerà un registro contenitori di Azure usando l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="857f9-101">In this unit, you will create an Azure container registry using the Azure CLI.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]
 
## <a name="create-an-azure-container-registry"></a><span data-ttu-id="857f9-102">Creare un registro contenitori di Azure</span><span class="sxs-lookup"><span data-stu-id="857f9-102">Create an Azure container registry</span></span>

<span data-ttu-id="857f9-103">Si lavorerà nell'ambiente sandbox gratuito, pertanto non occorre creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="857f9-103">We'll be working in the free sandbox, so no need to create your own Resource Group.</span></span> <span data-ttu-id="857f9-104">Creare un registro contenitori di Azure con il comando `az acr create`.</span><span class="sxs-lookup"><span data-stu-id="857f9-104">Create an Azure container registry with the `az acr create` command.</span></span> <span data-ttu-id="857f9-105">Il nome del registro contenitori deve essere univoco in Azure e contenere tra 5 e 50 caratteri alfanumerici.</span><span class="sxs-lookup"><span data-stu-id="857f9-105">The container registry name must be unique within Azure, and contain between 5 and 50 alphanumeric characters.</span></span> <span data-ttu-id="857f9-106">Sostituire `<acrName>` con un nome univoco per il registro.</span><span class="sxs-lookup"><span data-stu-id="857f9-106">Replace `<acrName>` with a unique name for your registry.</span></span>

<span data-ttu-id="857f9-107">Per questo esempio viene distribuito uno SKU del registro Premium.</span><span class="sxs-lookup"><span data-stu-id="857f9-107">For this example, a premium registry SKU is deployed.</span></span> <span data-ttu-id="857f9-108">Lo SKU Premium è obbligatorio per la replica geografica.</span><span class="sxs-lookup"><span data-stu-id="857f9-108">The premium SKU is required for geo-replication.</span></span> <span data-ttu-id="857f9-109">Digitare questo comando nell'editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="857f9-109">Enter this command into the cloud shell editor.</span></span>

```azurecli
az acr create --resource-group <rgn>[Sandbox resource group name]</rgn> --name <acrName> --sku Premium
```

<span data-ttu-id="857f9-110">Di seguito è riportato l'output di esempio per un nuovo registro contenitori di Azure:</span><span class="sxs-lookup"><span data-stu-id="857f9-110">Here's example output for a new Azure container registry:</span></span>

```output
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

<span data-ttu-id="857f9-111">Nel resto del modulo viene usato `<acrName>` come segnaposto per il nome del registro contenitori scelto in questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="857f9-111">The rest of this module refers to `<acrName>` as a placeholder for the container registry name that you chose in this step.</span></span>

## <a name="summary"></a><span data-ttu-id="857f9-112">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="857f9-112">Summary</span></span>

<span data-ttu-id="857f9-113">In questa unità è stato creato un registro contenitori di Azure usando l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="857f9-113">In this unit, you created an Azure container registry using the Azure CLI.</span></span>