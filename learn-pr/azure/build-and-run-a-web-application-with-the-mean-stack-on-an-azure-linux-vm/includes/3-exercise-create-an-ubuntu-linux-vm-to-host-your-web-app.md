<span data-ttu-id="01985-101">Lo stack MEAN di componenti richiede un server.</span><span class="sxs-lookup"><span data-stu-id="01985-101">The MEAN stack of components requires a server.</span></span> <span data-ttu-id="01985-102">Potrebbe essere un computer Linux o una macchina virtuale in esecuzione nella sala server oppure può essere configurato in una macchina virtuale basata sul cloud.</span><span class="sxs-lookup"><span data-stu-id="01985-102">It could be a Linux machine or virtual machine running in your own server room, or it can be configured on a cloud-based virtual machine.</span></span> <span data-ttu-id="01985-103">In questo modulo lo stack verrà configurato per l'esecuzione in una macchina virtuale Ubuntu Linux in esecuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="01985-103">In this module, we will be setting up the stack to run on an Ubuntu Linux virtual machine running on Azure.</span></span>

<span data-ttu-id="01985-104">In questo esercizio si creerà una nuova macchina virtuale Ubuntu Linux ospitata in Azure.</span><span class="sxs-lookup"><span data-stu-id="01985-104">In this exercise, you will be creating a new Ubuntu Linux virtual machine hosted on Azure.</span></span> <span data-ttu-id="01985-105">È anche possibile installare i componenti dello stack MEAN in una macchina virtuale esistente o in un computer host fisico.</span><span class="sxs-lookup"><span data-stu-id="01985-105">You could also install your MEAN stack components on an existing virtual machine or physical host machine.</span></span> <span data-ttu-id="01985-106">Creandone uno nuovo con questo esercizio, è possibile collegare tra loro tutti i componenti in un gruppo di risorse di Azure per semplificare la gestione e la pulizia dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="01985-106">By creating a new one with this exercise, you can tie together all the components into an Azure resource group for easier management and clean-up after you complete the exercises.</span></span>

<span data-ttu-id="01985-107">Per creare la VM Linux, verrà usata la riga di comando di Cloud Shell integrata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="01985-107">We will use the Cloud Shell command-line integrated into the Azure portal to create the Linux VM.</span></span>

## <a name="provision-an-ubuntu-linux-vm"></a><span data-ttu-id="01985-108">Effettuare il provisioning di una VM Ubuntu Linux</span><span class="sxs-lookup"><span data-stu-id="01985-108">Provision an Ubuntu Linux VM</span></span>

1. <span data-ttu-id="01985-109">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="01985-109">Go to the [Azure Portal](https://portal.azure.com?azure-portal=true).</span></span>
1. <span data-ttu-id="01985-110">Aprire Cloud Shell dall'icona `>_` nella barra degli strumenti del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="01985-110">Open the Cloud Shell from the `>_` icon in the Azure portal toolbar.</span></span>
1. <span data-ttu-id="01985-111">In Cloud Shell eseguire il comando per creare un gruppo di risorse di Azure, che includerà la VM.</span><span class="sxs-lookup"><span data-stu-id="01985-111">Within the Cloud Shell, execute the command to create an Azure resource group, which will include our VM.</span></span> <span data-ttu-id="01985-112">Sostituire `<resource-group-name>` con il nome del proprio gruppo di risorse e `<resource-group-location>` con la località di Azure desiderata (ad esempio, `westus`).</span><span class="sxs-lookup"><span data-stu-id="01985-112">Substitute your own resource group name for `<resource-group-name>` and your desired Azure location for `<resource-group-location>` (`westus`, for example).</span></span>

    ```bash
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    <span data-ttu-id="01985-113">Ricordare il nome del gruppo di risorse perché verrà usato in altri comandi.</span><span class="sxs-lookup"><span data-stu-id="01985-113">Remember your resource group name as we will use it in other commands.</span></span>

1. <span data-ttu-id="01985-114">In Cloud Shell eseguire questo comando per creare una nuova VM Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="01985-114">In the Cloud Shell, run the following command to create a new Ubuntu Linux VM.</span></span> <span data-ttu-id="01985-115">Sostituire `<resource-group-name>` con il nome del proprio gruppo di risorse e `<vm-admin-username>` e `<vm-admin-password>` con il nome utente e la password amministratore preferiti.</span><span class="sxs-lookup"><span data-stu-id="01985-115">Substitute your own resource group name for `<resource-group-name>` and your preferred admin username and password for `<vm-admin-username>` and `<vm-admin-password>`.</span></span>

    ```bash
    az vm create \
        --resource-group <resource-group-name> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    <span data-ttu-id="01985-116">Prendere nota del nome utente e della password amministratore per potersi connettere a questa VM in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="01985-116">Take note of your admin username and password to allow you to connect to this VM later.</span></span>

    <span data-ttu-id="01985-117">Questo comando richiede circa 2 minuti per il completamento.</span><span class="sxs-lookup"><span data-stu-id="01985-117">This command takes about 2 minutes to complete.</span></span> <span data-ttu-id="01985-118">Quando il comando termina, l'output risultante sarà simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="01985-118">When the command finishes, the resulting output will look similar to this.</span></span>

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<location you chose for the resource group>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<name you chose for thr resource group>",
        "zones": ""
    }
    ```

    <span data-ttu-id="01985-119">È consigliabile salvare anche l'indirizzo IP pubblico della VM appena creata per potersi connettere alla VM.</span><span class="sxs-lookup"><span data-stu-id="01985-119">You will also want to save the public IP address of the newly created VM in order to connect to the VM.</span></span>

1. <span data-ttu-id="01985-120">Provare a connettersi alla nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="01985-120">Try connecting to your new VM.</span></span>

    <span data-ttu-id="01985-121">Aprire un prompt dei comandi o una finestra del terminale ed eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="01985-121">Open a command prompt/terminal window and run the following command.</span></span> <span data-ttu-id="01985-122">Sostituire i segnaposto `<vm-admin-username>` e `<vm-public-ip>` con il nome utente amministratore e l'indirizzo IP pubblico della VM precedenti.</span><span class="sxs-lookup"><span data-stu-id="01985-122">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    <span data-ttu-id="01985-123">La prima volta che ci si connette al computer, verrà chiesto se si considera attendibile il computer remoto.</span><span class="sxs-lookup"><span data-stu-id="01985-123">The first time you connect to the machine, you'll be asked if you trust the remote machine.</span></span> <span data-ttu-id="01985-124">Rispondendo `yes`, l'impronta digitale della chiave ECDSA del computer verrà salvata in locale, quindi le connessioni successive saranno considerate attendibili.</span><span class="sxs-lookup"><span data-stu-id="01985-124">By answering `yes`, the machine's ECDSA key fingerprint will be saved locally so subsequent connections will be trusted.</span></span>

    <span data-ttu-id="01985-125">Se è tutto a posto, digitare `exit` per chiudere la sessione SSH.</span><span class="sxs-lookup"><span data-stu-id="01985-125">If everything looks fine, type `exit` to close the SSH session.</span></span>

1. <span data-ttu-id="01985-126">Aprire la porta 80 per consentire il traffico HTTP in ingresso nella nuova applicazione Web che verrà creata.</span><span class="sxs-lookup"><span data-stu-id="01985-126">Open port 80 to allow incoming HTTP traffic to your new web application you will create.</span></span>

    <span data-ttu-id="01985-127">Tornare a Cloud Shell nel portale di Azure e inviare il comando seguente, usando il nome del gruppo di risorse originale per `<resource-group-name>`.</span><span class="sxs-lookup"><span data-stu-id="01985-127">Go back to the Cloud Shell on the Azure portal and issue the following command, using your original resource group name for `<resource-group-name>`.</span></span>

    ``` bash
    az vm open-port --port 80 --resource-group <resource-group-name> --name MeanDemo
    ```

    <span data-ttu-id="01985-128">Verrà aperta la porta HTTP nella VM denominata "MeanDemo" al momento della creazione.</span><span class="sxs-lookup"><span data-stu-id="01985-128">This will open up the HTTP port on your VM that was named "MeanDemo" when it was created.</span></span>

## <a name="summary"></a><span data-ttu-id="01985-129">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="01985-129">Summary</span></span>

<span data-ttu-id="01985-130">Quando la nuova VM Ubuntu Linux è pronta, è possibile connettersi a essa per avviare l'installazione dei diversi componenti dello stack MEAN.</span><span class="sxs-lookup"><span data-stu-id="01985-130">With your new Ubuntu Linux VM ready to go, we can now connect to it to start installing the various components of the MEAN stack.</span></span>