<span data-ttu-id="a32be-101">Istanze di contenitore di Azure semplifica la creazione e gestione di contenitori Docker in Azure, senza dover eseguire il provisioning di macchine virtuali o adottare un servizio di livello superiore.</span><span class="sxs-lookup"><span data-stu-id="a32be-101">Azure Container Instances makes it easy to create and manage Docker containers in Azure, without having to provision virtual machines or adopt a higher-level service.</span></span> <span data-ttu-id="a32be-102">In questa unità viene creato un contenitore in Azure, che viene quindi esposto a Internet con un nome di dominio completo (FQDN).</span><span class="sxs-lookup"><span data-stu-id="a32be-102">In this unit, you create a container in Azure and expose it to the internet with a fully qualified domain name (FQDN).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="a32be-103">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="a32be-103">Create a resource group</span></span>

<span data-ttu-id="a32be-104">Le istanze di contenitore di Azure, come tutte le risorse di Azure, devono essere inserite in un gruppo di risorse, una raccolta logica in cui le risorse di Azure vengono distribuite e gestite.</span><span class="sxs-lookup"><span data-stu-id="a32be-104">Azure Container Instances, like all Azure resources, must be placed in a resource group, a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="a32be-105">Creare un gruppo di risorse con il comando `az group create`.</span><span class="sxs-lookup"><span data-stu-id="a32be-105">Create a resource group with the `az group create` command.</span></span>

<span data-ttu-id="a32be-106">L'esempio seguente crea un gruppo di risorse denominato *myResourceGroup* nella posizione *eastus*:</span><span class="sxs-lookup"><span data-stu-id="a32be-106">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a><span data-ttu-id="a32be-107">Creare un contenitore</span><span class="sxs-lookup"><span data-stu-id="a32be-107">Create a container</span></span>

<span data-ttu-id="a32be-108">È possibile creare un contenitore specificando un nome, un'immagine Docker e un gruppo di risorse di Azure al comando **az container create**.</span><span class="sxs-lookup"><span data-stu-id="a32be-108">You can create a container by providing a name, a Docker image, and an Azure resource group to the **az container create** command.</span></span> <span data-ttu-id="a32be-109">Facoltativamente, è possibile esporre il contenitore in Internet specificando un'etichetta del nome DNS.</span><span class="sxs-lookup"><span data-stu-id="a32be-109">You can optionally expose the container to the internet by specifying a DNS name label.</span></span> <span data-ttu-id="a32be-110">In questo esempio si distribuisce un contenitore che ospita un'app Web di piccole dimensioni.</span><span class="sxs-lookup"><span data-stu-id="a32be-110">In this example, you deploy a container that hosts a small web app.</span></span>

<span data-ttu-id="a32be-111">Eseguire il comando seguente per avviare un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="a32be-111">Execute the following command to start a container instance.</span></span> <span data-ttu-id="a32be-112">Il valore *--dns-name-label* deve essere univoco all'interno dell'area di Azure in cui si crea l'istanza, quindi potrebbe essere necessario modificare questo valore per garantire l'univocità:</span><span class="sxs-lookup"><span data-stu-id="a32be-112">The *--dns-name-label* value must be unique within the Azure region you create the instance, so you might need to modify this value to ensure uniqueness:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

<span data-ttu-id="a32be-113">In pochi secondi, verrà visualizzata una risposta alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="a32be-113">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="a32be-114">Il contenitore inizialmente presenta lo stato **Creazione in corso**, ma viene avviato entro alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="a32be-114">Initially, the container is in the **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="a32be-115">È possibile controllare lo stato usando il comando `az container show`:</span><span class="sxs-lookup"><span data-stu-id="a32be-115">You can check the status using the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

<span data-ttu-id="a32be-116">Quando si esegue il comando, vengono visualizzati il nome di dominio completo (FQDN) del contenitore e il relativo stato di provisioning:</span><span class="sxs-lookup"><span data-stu-id="a32be-116">When you run the command, the container's fully qualified domain name (FQDN) and its provisioning state are displayed:</span></span>

```bash
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

<span data-ttu-id="a32be-117">Quando il contenitore passa allo stato **Completato**, passare al relativo FQDN nel browser:</span><span class="sxs-lookup"><span data-stu-id="a32be-117">Once the container moves to the **Succeeded** state, navigate to its FQDN in your browser:</span></span>

![Screenshot del browser che mostra un'applicazione in esecuzione in un'istanza di contenitore di Azure](../media-draft/aci-app-browser.png)

## <a name="summary"></a><span data-ttu-id="a32be-119">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a32be-119">Summary</span></span>

<span data-ttu-id="a32be-120">In questa unità è stata creata un'istanza di contenitore di Azure che esegue un server Web e un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a32be-120">In this unit, you created an Azure container instance that is running a web server and application.</span></span> <span data-ttu-id="a32be-121">L'accesso all'applicazione è stato inoltre eseguito mediante l'FQDN dell'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="a32be-121">You also accessed this application using the FQDN of the container instance.</span></span>

<span data-ttu-id="a32be-122">Nell'unità successiva si configureranno i criteri di riavvio dell'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="a32be-122">In the next unit, you will configure container instance restart policies.</span></span>