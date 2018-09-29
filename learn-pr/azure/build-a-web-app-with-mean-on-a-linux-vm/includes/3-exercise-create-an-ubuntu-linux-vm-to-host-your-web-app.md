<span data-ttu-id="ac11b-101">Lo stack MEAN di componenti richiede un server.</span><span class="sxs-lookup"><span data-stu-id="ac11b-101">The MEAN stack of components requires a server.</span></span> <span data-ttu-id="ac11b-102">Può essere un computer Linux o una macchina virtuale in esecuzione nella sala server oppure può essere configurato in una macchina virtuale basata sul cloud.</span><span class="sxs-lookup"><span data-stu-id="ac11b-102">It could be a Linux machine or virtual machine running in your own server room, or it can be configured on a cloud-based virtual machine.</span></span> <span data-ttu-id="ac11b-103">In questo modulo lo stack verrà configurato per l'esecuzione in una macchina virtuale Ubuntu Linux in esecuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="ac11b-103">In this module, we will set up the stack to run on an Ubuntu Linux virtual machine running on Azure.</span></span>

<span data-ttu-id="ac11b-104">In questa unità si creerà una nuova macchina virtuale Ubuntu Linux ospitata in Azure.</span><span class="sxs-lookup"><span data-stu-id="ac11b-104">In this unit, you will be creating a new Ubuntu Linux virtual machine hosted on Azure.</span></span> <span data-ttu-id="ac11b-105">È anche possibile installare i componenti dello stack MEAN in una macchina virtuale esistente o in un computer host fisico.</span><span class="sxs-lookup"><span data-stu-id="ac11b-105">You could also install your MEAN stack components on an existing virtual machine or physical host machine.</span></span> <span data-ttu-id="ac11b-106">Creandone uno nuovo con questo esercizio, è possibile collegare tra loro tutti i componenti in un gruppo di risorse di Azure per semplificare la gestione e la pulizia dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="ac11b-106">By creating a new one with this exercise, you can tie together all the components into an Azure resource group for easier management and clean-up after you complete the exercises.</span></span>

## <a name="provision-an-ubuntu-linux-vm"></a><span data-ttu-id="ac11b-107">Effettuare il provisioning di una macchina virtuale Ubuntu Linux</span><span class="sxs-lookup"><span data-stu-id="ac11b-107">Provision an Ubuntu Linux VM</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="creating-a-resource-group"></a><span data-ttu-id="ac11b-108">Creazione di un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="ac11b-108">Creating a resource group</span></span>

<span data-ttu-id="ac11b-109">In genere, la prima cosa che occorre fare quando si crea un nuovo set di risorse è creare un _gruppo di risorse_ per contenerle tutte.</span><span class="sxs-lookup"><span data-stu-id="ac11b-109">Normally, the first thing you'll do when creating a new set of resources is create a _resource group_ to own them all.</span></span> <span data-ttu-id="ac11b-110">Questo è un passaggio non necessario nell'ambiente sandbox di Azure, ma quando si lavora nella propria sottoscrizione usare il comando seguente per creare un gruppo di risorse in una località nelle vicinanze.</span><span class="sxs-lookup"><span data-stu-id="ac11b-110">This is an unnecessary step in the Azure sandbox, but when you are working in your own subscription use the following command to create a resource group in a location near you.</span></span>

```azurecli
az group create --name <resource-group-name> --location <resource-group-location>
```

> [!IMPORTANT]
> <span data-ttu-id="ac11b-111">Non si deve creare un gruppo di risorse quando si usa l'ambiente sandox di Azure.</span><span class="sxs-lookup"><span data-stu-id="ac11b-111">You do not need to create a resource group with the Azure sandbox.</span></span> <span data-ttu-id="ac11b-112">Usare invece il gruppo di risorse creato in precedenza denominato **<rgn>[gruppo di risorse sandbox]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="ac11b-112">Instead use the pre-created resource group named **<rgn>[sandbox Resource Group]</rgn>**.</span></span>

1. <span data-ttu-id="ac11b-113">In Cloud Shell a destra, digitare il comando seguente per creare una nuova macchina virtuale Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="ac11b-113">In the Cloud Shell on the right, type the following command to create a new Ubuntu Linux VM.</span></span> <span data-ttu-id="ac11b-114">Sostituire `<vm-admin-username>` e `<vm-admin-password>` con il nome utente e la password preferiti dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="ac11b-114">Substitute your preferred admin username and password for `<vm-admin-username>` and `<vm-admin-password>`.</span></span>

    ```azurecli
    az vm create \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    <span data-ttu-id="ac11b-115">Prendere nota del nome utente e della password amministratore per potersi connettere a questa macchina virtuale in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="ac11b-115">Take note of your admin username and password to allow you to connect to this VM later.</span></span>

    <span data-ttu-id="ac11b-116">Il completamento di questo comando richiede circa 2 minuti.</span><span class="sxs-lookup"><span data-stu-id="ac11b-116">This command takes about two minutes to complete.</span></span> <span data-ttu-id="ac11b-117">Al termine del comando, l'output risultante sarà simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="ac11b-117">When the command finishes, the resulting output will look similar to this.</span></span>

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<resource group location>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<rgn>[sandbox resource group name]</rgn>",
        "zones": ""
    }
    ```

    <span data-ttu-id="ac11b-118">È consigliabile salvare anche l'indirizzo IP pubblico della VM appena creata per potersi connettere alla VM.</span><span class="sxs-lookup"><span data-stu-id="ac11b-118">You will also want to save the public IP address of the newly created VM in order to connect to the VM.</span></span>

1. <span data-ttu-id="ac11b-119">Provare a connettersi alla nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="ac11b-119">Try connecting to your new VM.</span></span>

    <span data-ttu-id="ac11b-120">In Cloud Shell eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="ac11b-120">From the Cloud Shell, run the following command.</span></span> <span data-ttu-id="ac11b-121">Sostituire i segnaposto `<vm-admin-username>` e `<vm-public-ip>` con il nome utente amministratore e l'indirizzo IP pubblico della VM precedenti.</span><span class="sxs-lookup"><span data-stu-id="ac11b-121">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    <span data-ttu-id="ac11b-122">La prima volta che ci si connette al computer, verrà chiesto se si considera attendibile il computer remoto.</span><span class="sxs-lookup"><span data-stu-id="ac11b-122">The first time you connect to the machine, you'll be asked if you trust the remote machine.</span></span> <span data-ttu-id="ac11b-123">Rispondendo `yes`, l'impronta digitale della chiave ECDSA del computer verrà salvata in locale, quindi le connessioni successive saranno considerate attendibili.</span><span class="sxs-lookup"><span data-stu-id="ac11b-123">By answering `yes`, the machine's ECDSA key fingerprint will be saved locally, so subsequent connections will be trusted.</span></span> <span data-ttu-id="ac11b-124">Verrà quindi richiesta la password, che verrà visualizzata ogni volta che ci si connette.</span><span class="sxs-lookup"><span data-stu-id="ac11b-124">You'll then be prompted for your password, which you'll see every time you connect.</span></span>

    <span data-ttu-id="ac11b-125">Se è tutto corretto, digitare `exit` per chiudere la sessione SSH.</span><span class="sxs-lookup"><span data-stu-id="ac11b-125">If everything looks fine, type `exit` to close the SSH session.</span></span>

1. <span data-ttu-id="ac11b-126">Aprire la porta 80 nella macchina virtuale per consentire il traffico HTTP in ingresso verso la nuova applicazione Web che verrà creata.</span><span class="sxs-lookup"><span data-stu-id="ac11b-126">Open port 80 on the VM to allow incoming HTTP traffic to the new web application that you will create.</span></span>

    ```azurecli
    az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name MeanDemo
    ```

    <span data-ttu-id="ac11b-127">Il comando aprirà la porta HTTP nella macchina virtuale denominata "MeanDemo" al momento della creazione.</span><span class="sxs-lookup"><span data-stu-id="ac11b-127">This command will open up the HTTP port on your VM that was named "MeanDemo" when it was created.</span></span>

## <a name="summary"></a><span data-ttu-id="ac11b-128">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="ac11b-128">Summary</span></span>

<span data-ttu-id="ac11b-129">Quando la nuova VM Ubuntu Linux è pronta, è possibile connettersi a essa per avviare l'installazione dei diversi componenti dello stack MEAN.</span><span class="sxs-lookup"><span data-stu-id="ac11b-129">With your new Ubuntu Linux VM ready to go, we can now connect to it to start installing the various components of the MEAN stack.</span></span>