<span data-ttu-id="97b67-101">Viene qui creato un contenitore in Azure, che viene quindi esposto a Internet con un nome di dominio completo (FQDN).</span><span class="sxs-lookup"><span data-stu-id="97b67-101">Here, you create a container in Azure and expose it to the internet with a fully qualified domain name (FQDN).</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="why-use-azure-container-instances"></a><span data-ttu-id="97b67-102">Perché usare Istanze di contenitore di Azure?</span><span class="sxs-lookup"><span data-stu-id="97b67-102">Why use Azure Container Instances?</span></span>

<span data-ttu-id="97b67-103">Istanze di contenitore di Azure è utile per gli scenari che possono funzionare anche in contenitori isolati, inclusi i processi di compilazione, l'automazione di attività e le applicazioni semplici.</span><span class="sxs-lookup"><span data-stu-id="97b67-103">Azure Container Instances is useful for scenarios that can operate in isolated containers, including simple applications, task automation, and build jobs.</span></span> <span data-ttu-id="97b67-104">La soluzione Istanze di contenitore di Azure offre i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="97b67-104">Azure Container Instances offers the following benefits:</span></span>

- <span data-ttu-id="97b67-105">**Avvio rapido**: i contenitori vengono avviati in pochi secondi.</span><span class="sxs-lookup"><span data-stu-id="97b67-105">**Fast startup**: Launch containers in seconds.</span></span>
- <span data-ttu-id="97b67-106">**Fatturazione al secondo**: i costi vengono addebitati solo mentre il contenitore è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="97b67-106">**Per second billing**: Incur costs only while the container is running.</span></span>
- <span data-ttu-id="97b67-107">**Sicurezza a livello di hypervisor**: l'applicazione viene isolata completamente come in una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="97b67-107">**Hypervisor-level security**: Isolate your application as completely as it would be in a VM.</span></span>
- <span data-ttu-id="97b67-108">**Dimensioni personalizzate**: è possibile specificare i valori esatti per memoria e core CPU.</span><span class="sxs-lookup"><span data-stu-id="97b67-108">**Custom sizes**: Specify exact values for CPU cores and memory.</span></span>
- <span data-ttu-id="97b67-109">**Archivio permanente**: è possibile montare le condivisioni di File di Azure direttamente in un contenitore per recuperare e rendere persistente lo stato.</span><span class="sxs-lookup"><span data-stu-id="97b67-109">**Persistent storage**: Mount Azure Files shares directly to a container to retrieve and persist state.</span></span>
- <span data-ttu-id="97b67-110">**Linux e Windows**: consente di pianificare contenitori sia Windows che Linux usando la stessa API.</span><span class="sxs-lookup"><span data-stu-id="97b67-110">**Linux and Windows**: Schedule both Windows and Linux containers using the same API.</span></span>

<span data-ttu-id="97b67-111">Per gli scenari in cui è necessaria l'orchestrazione completa dei contenitori, quali il rilevamento dei servizi tra più contenitori, il ridimensionamento automatico e gli aggiornamenti coordinati delle applicazioni, è consigliabile usare il servizio Kubernetes di Azure (AKS).</span><span class="sxs-lookup"><span data-stu-id="97b67-111">For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend Azure Kubernetes Service (AKS).</span></span>

## <a name="create-a-container"></a><span data-ttu-id="97b67-112">Creare un contenitore</span><span class="sxs-lookup"><span data-stu-id="97b67-112">Create a container</span></span>

<span data-ttu-id="97b67-113">Si crea un contenitore specificando un nome, un'immagine Docker e un gruppo di risorse di Azure nel comando **az container create**.</span><span class="sxs-lookup"><span data-stu-id="97b67-113">You create a container by providing a name, a Docker image, and an Azure resource group to the **az container create** command.</span></span> <span data-ttu-id="97b67-114">Facoltativamente, è possibile esporre il contenitore in Internet specificando un'etichetta del nome DNS.</span><span class="sxs-lookup"><span data-stu-id="97b67-114">You can optionally expose the container to the Internet by specifying a DNS name label.</span></span> <span data-ttu-id="97b67-115">In questo esempio si distribuisce un contenitore che ospita un'app Web di piccole dimensioni.</span><span class="sxs-lookup"><span data-stu-id="97b67-115">In this example, you deploy a container that hosts a small web app.</span></span> <span data-ttu-id="97b67-116">È anche possibile selezionare la località in cui posizionare l'immagine: per impostazione predefinita qui viene usata l'opzione "Stati Uniti orientali", ma è possibile modificarla selezionando dall'elenco una località più vicina.</span><span class="sxs-lookup"><span data-stu-id="97b67-116">You can also select the location to place the image - we're defaulting to "East US" below, but you can change it to a location close to you from the following list.</span></span>

<span data-ttu-id="97b67-117"><!-- TODO: fix region list so it's not hardcoded here --> L'ambiente sandbox gratuito consente di creare risorse in un subset di aree globali di Azure.</span><span class="sxs-lookup"><span data-stu-id="97b67-117"><!-- TODO: fix region list so it's not hardcoded here --> The free sandbox allows you to create resources in a subset of Azure's global regions.</span></span> <span data-ttu-id="97b67-118">Selezionare un'area nell'elenco seguente durante la creazione delle risorse:</span><span class="sxs-lookup"><span data-stu-id="97b67-118">Select a region from the following list when creating any resources:</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="97b67-119">- westus2 - centralus - eastus - westeurope - southeastasia</span><span class="sxs-lookup"><span data-stu-id="97b67-119">- westus2 - centralus - eastus - westeurope - southeastasia</span></span> :::column-end:::
:::row-end:::

<span data-ttu-id="97b67-120">Eseguire il comando seguente in Cloud Shell per avviare un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="97b67-120">Execute the following command in the Cloud Shell to start a container instance.</span></span> <span data-ttu-id="97b67-121">Il valore `--dns-name-label` deve essere univoco all'interno dell'area di Azure in cui si crea l'istanza, quindi sarà necessario sostituire `[dns-name]` con un valore univoco.</span><span class="sxs-lookup"><span data-stu-id="97b67-121">The `--dns-name-label` value must be unique within the Azure region you create the instance, so you will need to replace `[dns-name]` with something unique.</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label [dns-name] \
    --location eastus
```

<span data-ttu-id="97b67-122">In pochi secondi, verrà visualizzata una risposta alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="97b67-122">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="97b67-123">Il contenitore inizialmente presenta lo stato **Creazione in corso**, ma viene avviato entro alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="97b67-123">Initially, the container is in the **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="97b67-124">È possibile controllare lo stato usando il comando `az container show`:</span><span class="sxs-lookup"><span data-stu-id="97b67-124">You can check the status using the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

<span data-ttu-id="97b67-125">Quando si esegue il comando, vengono visualizzati il nome di dominio completo (FQDN) del contenitore e il relativo stato di provisioning:</span><span class="sxs-lookup"><span data-stu-id="97b67-125">When you run the command, the container's fully qualified domain name (FQDN) and its provisioning state are displayed:</span></span>

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

<span data-ttu-id="97b67-126">Quando il contenitore passa allo stato **Completato**, passare al relativo nome FQDN nel browser per verificare l'esito positivo.</span><span class="sxs-lookup"><span data-stu-id="97b67-126">Once the container moves to the **Succeeded** state, navigate to its FQDN in your browser to verify success.</span></span>

<span data-ttu-id="97b67-127">È stata creata un'istanza di contenitore di Azure per eseguire un server Web e un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="97b67-127">Here, you created an Azure container instance to run a web server and application.</span></span> <span data-ttu-id="97b67-128">L'accesso all'applicazione è stato inoltre eseguito mediante il nome FQDN dell'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="97b67-128">You also accessed this application using the FQDN of the container instance.</span></span>