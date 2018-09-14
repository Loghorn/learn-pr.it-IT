<span data-ttu-id="e51f6-101">In questo caso, si crea un contenitore in Azure ed esporlo a internet con un nome di dominio completo (FQDN).</span><span class="sxs-lookup"><span data-stu-id="e51f6-101">Here, you create a container in Azure and expose it to the internet with a fully qualified domain name (FQDN).</span></span>

## <a name="why-use-azure-container-instances"></a><span data-ttu-id="e51f6-102">Perché usare istanze di contenitore di Azure?</span><span class="sxs-lookup"><span data-stu-id="e51f6-102">Why use Azure Container Instances?</span></span>

<span data-ttu-id="e51f6-103">Istanze di contenitore di Azure è utile per scenari in cui possono operare in contenitori isolati, tra cui applicazioni semplici, automazione di attività e i processi di compilazione.</span><span class="sxs-lookup"><span data-stu-id="e51f6-103">Azure Container Instances is useful for scenarios that can operate in isolated containers, including simple applications, task automation, and build jobs.</span></span> <span data-ttu-id="e51f6-104">La soluzione Istanze di contenitore di Azure offre i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e51f6-104">Azure Container Instances offers the following benefits:</span></span>

- <span data-ttu-id="e51f6-105">**Avvio rapido**: avviare i contenitori in pochi secondi.</span><span class="sxs-lookup"><span data-stu-id="e51f6-105">**Fast startup**: Launch containers in seconds.</span></span>
- <span data-ttu-id="e51f6-106">**Fatturazione al secondo**: addebitato un costo solo mentre è in esecuzione il contenitore.</span><span class="sxs-lookup"><span data-stu-id="e51f6-106">**Per second billing**: Incur costs only while the container is running.</span></span>
- <span data-ttu-id="e51f6-107">**Sicurezza a livello di hypervisor**: isolare l'applicazione completamente come lo sarebbe in una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="e51f6-107">**Hypervisor-level security**: Isolate your application as completely as it would be in a VM.</span></span>
- <span data-ttu-id="e51f6-108">**Dimensioni personalizzate**: specificare valori esatti per la memoria e core CPU.</span><span class="sxs-lookup"><span data-stu-id="e51f6-108">**Custom sizes**: Specify exact values for CPU cores and memory.</span></span>
- <span data-ttu-id="e51f6-109">**Un archivio permanente**: montare file di Azure condivide direttamente a un contenitore per recuperare e rendere persistente lo stato.</span><span class="sxs-lookup"><span data-stu-id="e51f6-109">**Persistent storage**: Mount Azure Files shares directly to a container to retrieve and persist state.</span></span>
- <span data-ttu-id="e51f6-110">**Linux e Windows**: pianificare contenitori sia Windows che Linux usando l'API stessa.</span><span class="sxs-lookup"><span data-stu-id="e51f6-110">**Linux and Windows**: Schedule both Windows and Linux containers using the same API.</span></span>

<span data-ttu-id="e51f6-111">Per gli scenari in cui è necessaria l'orchestrazione completa dei contenitori, quali il rilevamento dei servizi tra più contenitori, la scalabilità automatica e gli aggiornamenti coordinati delle applicazioni, è consigliabile usare Azure Kubernetes Service (AKS).</span><span class="sxs-lookup"><span data-stu-id="e51f6-111">For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend Azure Kubernetes Service (AKS).</span></span>

## <a name="create-a-container"></a><span data-ttu-id="e51f6-112">Creare un contenitore</span><span class="sxs-lookup"><span data-stu-id="e51f6-112">Create a container</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="e51f6-113">Per creare un contenitore specificando un nome, un'immagine Docker e un gruppo di risorse di Azure per il **contenitore di az creare** comando.</span><span class="sxs-lookup"><span data-stu-id="e51f6-113">You create a container by providing a name, a Docker image, and an Azure resource group to the **az container create** command.</span></span> <span data-ttu-id="e51f6-114">Facoltativamente, è possibile esporre il contenitore in Internet specificando un'etichetta del nome DNS.</span><span class="sxs-lookup"><span data-stu-id="e51f6-114">You can optionally expose the container to the internet by specifying a DNS name label.</span></span> <span data-ttu-id="e51f6-115">In questo esempio si distribuisce un contenitore che ospita un'app Web di piccole dimensioni.</span><span class="sxs-lookup"><span data-stu-id="e51f6-115">In this example, you deploy a container that hosts a small web app.</span></span>

<span data-ttu-id="e51f6-116">Eseguire il comando seguente in Cloud Shell per avviare un'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="e51f6-116">Execute the following command in the Cloud Shell to start a container instance.</span></span> <span data-ttu-id="e51f6-117">Il valore *--dns-name-label* deve essere univoco all'interno dell'area di Azure in cui si crea l'istanza, quindi potrebbe essere necessario modificare questo valore per garantire l'univocità:</span><span class="sxs-lookup"><span data-stu-id="e51f6-117">The *--dns-name-label* value must be unique within the Azure region you create the instance, so you might need to modify this value to ensure uniqueness:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

<span data-ttu-id="e51f6-118">In pochi secondi, verrà visualizzata una risposta alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="e51f6-118">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="e51f6-119">Il contenitore inizialmente presenta lo stato **Creazione in corso**, ma viene avviato entro alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="e51f6-119">Initially, the container is in the **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="e51f6-120">È possibile controllare lo stato usando il comando `az container show`:</span><span class="sxs-lookup"><span data-stu-id="e51f6-120">You can check the status using the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

<span data-ttu-id="e51f6-121">Quando si esegue il comando, vengono visualizzati il nome di dominio completo (FQDN) del contenitore e il relativo stato di provisioning:</span><span class="sxs-lookup"><span data-stu-id="e51f6-121">When you run the command, the container's fully qualified domain name (FQDN) and its provisioning state are displayed:</span></span>

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

<span data-ttu-id="e51f6-122">Quando il contenitore passa per la **Succeeded** nello stato, passare al suo FQDN nel browser per verificare l'esito positivo.</span><span class="sxs-lookup"><span data-stu-id="e51f6-122">Once the container moves to the **Succeeded** state, navigate to its FQDN in your browser to verify success.</span></span>

<span data-ttu-id="e51f6-123">In questo caso, è creata un'istanza di contenitore di Azure per l'esecuzione di un server web e un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e51f6-123">Here, you created an Azure container instance to run a web server and application.</span></span> <span data-ttu-id="e51f6-124">L'accesso all'applicazione è stato inoltre eseguito mediante l'FQDN dell'istanza di contenitore.</span><span class="sxs-lookup"><span data-stu-id="e51f6-124">You also accessed this application using the FQDN of the container instance.</span></span>