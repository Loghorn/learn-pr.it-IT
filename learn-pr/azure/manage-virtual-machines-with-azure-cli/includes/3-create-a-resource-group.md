<span data-ttu-id="1f6e3-101">Il nostro obiettivo consiste nel creare una nuova macchina virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-101">Our goal is to create a new Azure virtual machine.</span></span> <span data-ttu-id="1f6e3-102">È necessario fornire diversi tipi di informazioni per identificare il percorso della risorsa, il sistema operativo da usare e la configurazione hardware necessaria per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-102">We'll need to supply several pieces of information to identify the resource location, OS to use, and the hardware configuration needed for the VM.</span></span> <span data-ttu-id="1f6e3-103">Iniziare con un **gruppo di risorse**.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-103">Let's start with the **resource group**.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="1f6e3-104">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="1f6e3-104">Create a resource group</span></span>

<span data-ttu-id="1f6e3-105">Azure usa _gruppi di risorse_ per raggruppare le risorse correlate, ad esempio macchine virtuali e database.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-105">Azure uses _resource groups_ to group related resources such as virtual machines and databases together.</span></span> <span data-ttu-id="1f6e3-106">Il gruppo di risorse identifica anche un percorso specifico, denominato "area", che deciderà quale data center di risorsa è inserito.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-106">The resource group also identifies a specific location (called a "region") which will decide what data center the resource is placed into.</span></span>

> [!NOTE]
> <span data-ttu-id="1f6e3-107">Il sandbox di Azure mette a disposizione un gruppo di risorse create precedentemente denominato <rgn>[nome gruppo di risorse sandbox]</rgn>.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-107">The Azure sandbox provides a pre-created resource group named <rgn>[Sandbox resource group name]</rgn>.</span></span> <span data-ttu-id="1f6e3-108">Non è necessario eseguire questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-108">You do not need to execute these steps.</span></span> <span data-ttu-id="1f6e3-109">Tuttavia, questi sono i comandi da eseguire per creare le _proprie_ risorse per i progetti reali.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-109">However, when you are creating your _own_ resources for real projects, these will be the commands you will need to perform.</span></span> <span data-ttu-id="1f6e3-110">Sandbox di Azure non consente di creare direttamente gruppi di risorse.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-110">The Azure sandbox does not allow you to create resource groups directly.</span></span>

<span data-ttu-id="1f6e3-111">Come esempio, digitare il seguente comando dell'interfaccia della riga di comando di Azure in Azure Cloud Shell per creare il gruppo di risorse nella regione **Stati Uniti orientali**.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-111">As an example, you could type the following Azure CLI command in Azure Cloud Shell to create a resource group in the **East US** region.</span></span> <span data-ttu-id="1f6e3-112">Sostituire **[resource-group]** con un nome valido che sia univoco nella sottoscrizione attiva.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-112">You would replace **[resource-group]** with a valid name that is unique within the active subscription.</span></span>

```azurecli
az group create --name [resource-group] --location eastus
```

<span data-ttu-id="1f6e3-113">Verrà restituito un blocco JSON che indica che il gruppo di risorse è stato creato.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-113">This would return a JSON block indicating the resource group has been created.</span></span>

```json
{
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/<resourcegroup>",
  "location": "eastus",
  "managedBy": null,
  "name": "<resourcegroup>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

<span data-ttu-id="1f6e3-114">Si noti che viene restituito l'identificatore univoco, la località e il nome della sottoscrizione come parte della risposta.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-114">Notice that it returns the subscription unique identifier, location, and name as part of the response.</span></span> <span data-ttu-id="1f6e3-115">È possibile usare questi elementi per verificare che il gruppo sia stato creato nella sottoscrizione e località appropriate.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-115">You can use these to verify it got created in the proper subscription and location.</span></span>

<span data-ttu-id="1f6e3-116">Ora che si sa creare un gruppo di risorse, è possibile creare una nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="1f6e3-116">Now that we know how to create a resource group, let's create a new virtual machine.</span></span>