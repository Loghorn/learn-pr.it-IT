<span data-ttu-id="1f9ce-101">Il nostro obiettivo consiste nel creare una nuova macchina virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-101">Our goal is to create a new Azure virtual machine.</span></span> <span data-ttu-id="1f9ce-102">È necessario fornire diversi tipi di informazioni per identificare il percorso della risorsa, il sistema operativo da usare e la configurazione hardware necessaria per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-102">We'll need to supply several pieces of information to identify the resource location, OS to use, and the hardware configuration needed for the VM.</span></span> <span data-ttu-id="1f9ce-103">Iniziare con un **gruppo di risorse**.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-103">Let's start with the **resource group**.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="1f9ce-104">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="1f9ce-104">Create a resource group</span></span>

<span data-ttu-id="1f9ce-105">Azure usa _gruppi di risorse_ per raggruppare le risorse correlate, ad esempio macchine virtuali e database.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-105">Azure uses _resource groups_ to group related resources such as virtual machines and databases together.</span></span> <span data-ttu-id="1f9ce-106">Il gruppo di risorse identifica anche un percorso specifico (denominato "area") che deciderà quale data center di risorsa è inserito.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-106">The resource group also identifies a specific location (called a "region") which will decide what data center the resource is placed into.</span></span>

<span data-ttu-id="1f9ce-107">Poiché è un esperimento, iniziare creando un nuovo gruppo di risorse denominato `ExerciseResources` e posizionarlo nell'area `eastus`.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-107">Since we're experimenting, start by creating a new resource group named `ExerciseResources` and place it into the `eastus` region.</span></span>

> [!NOTE]
> <span data-ttu-id="1f9ce-108">Assicurarsi di usare questo nome esatto del nuovo gruppo di risorse, perché il sistema Microsoft Learn cercherà questo nome in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-108">Make sure to use this exact name for your new resource group, because the Microsoft Learn system will be looking for this resource name later.</span></span> 

<span data-ttu-id="1f9ce-109">Digitare il comando di Azure in Azure Cloud Shell per creare il gruppo di risorse nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-109">Type the following Azure CLI command in Azure Cloud Shell to create the resource group in your subscription.</span></span>

```azurecli
az group create --name ExerciseResources --location eastus
```

<span data-ttu-id="1f9ce-110">Verrà restituito un blocco JSON che indica che il gruppo di risorse è stato creato.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-110">This will return a JSON block indicating the resource group has been created.</span></span> <span data-ttu-id="1f9ce-111">Dovrebbe essere simile a:</span><span class="sxs-lookup"><span data-stu-id="1f9ce-111">It should look something like:</span></span>

```json
{
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources",
  "location": "eastus",
  "managedBy": null,
  "name": "ExerciseResources",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

<span data-ttu-id="1f9ce-112">Si noti che viene restituito l'identificatore univoco, il percorso e il nome della sottoscrizione come parte della risposta.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-112">Notice that it returns the subscription unique identifier, location, and name as part of the response.</span></span> <span data-ttu-id="1f9ce-113">È possibile usare questi elementi per verificare che il gruppo sia stato creato nella sottoscrizione e posizione appropriate.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-113">You can use these to verify it got created in the proper subscription and location.</span></span>

<span data-ttu-id="1f9ce-114">Con un gruppo di risorse disponibile, creare una nuova macchina virtuale contenuta in esso.</span><span class="sxs-lookup"><span data-stu-id="1f9ce-114">Now that we have a resource group, let's create a new virtual machine.</span></span>