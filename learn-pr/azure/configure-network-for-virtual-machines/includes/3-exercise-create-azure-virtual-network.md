<span data-ttu-id="b3164-101">In questo esercizio viene creata una rete virtuale in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b3164-101">In this exercise, you will create a virtual network in Microsoft Azure.</span></span> <span data-ttu-id="b3164-102">Quindi vengono create due macchine virtuali e viene usata la rete virtuale per connettere le macchine virtuali tra loro e a Internet.</span><span class="sxs-lookup"><span data-stu-id="b3164-102">You will then create two virtual machines and use the virtual network to connect the virtual machines to each other and to the internet.</span></span>

<span data-ttu-id="b3164-103">Prima di iniziare l'unità è necessario accedere ad [Azure Cloud Shell](https://shell.azure.com) con le credenziali della versione di valutazione gratuita.</span><span class="sxs-lookup"><span data-stu-id="b3164-103">Before you begin this unit, you will need to log into [Azure Cloud Shell](https://shell.azure.com) with your trial subscription credentials.</span></span> <span data-ttu-id="b3164-104">Si usa Azure Cloud Shell per creare i gruppi di risorse, le reti virtuali e le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="b3164-104">We will use Azure Cloud Shell to create the resource groups, virtual networks, and virtual machines.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="b3164-105">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="b3164-105">Create a resource group</span></span>

1. <span data-ttu-id="b3164-106">Nella finestra **Benvenuto in Azure Cloud Shell** fare clic su **PowerShell (Linux)**.</span><span class="sxs-lookup"><span data-stu-id="b3164-106">In the **Welcome to Azure Cloud Shell** window, click **PowerShell (Linux)**.</span></span>

1. <span data-ttu-id="b3164-107">Nella finestra **You have no storage mounted** (Nessuna risorsa di archiviazione montata) fare clic su **Create Storage** (Crea archiviazione).</span><span class="sxs-lookup"><span data-stu-id="b3164-107">In the **you have no storage mounted** window, click **Create Storage**.</span></span>

1. <span data-ttu-id="b3164-108">Nel prompt della riga di comando di Azure PowerShell digitare il codice seguente e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="b3164-108">In the Azure PowerShell command-line prompt, type the following code and press Enter.</span></span>

    ```PowerShell
    az group create --name myResourceGroup --location eastus
    ```

## <a name="create-a-virtual-network"></a><span data-ttu-id="b3164-109">Creare una rete virtuale</span><span class="sxs-lookup"><span data-stu-id="b3164-109">Create a virtual network</span></span>

1. <span data-ttu-id="b3164-110">Per creare una rete virtuale, immettere il comando seguente e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="b3164-110">To create a virtual network, enter the following command and press Enter.</span></span>

    ```PowerShell
    az network vnet create --name myVirtualNetwork --resource-group myResourceGroup --subnet-name default
    ```

## <a name="create-two-virtual-machines"></a><span data-ttu-id="b3164-111">Creare due macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="b3164-111">Create two virtual machines</span></span>

1. <span data-ttu-id="b3164-112">Per creare la prima macchina virtuale, eseguire il comando seguente per creare una VM Windows con un indirizzo IP pubblico accessibile tramite la porta 3389 (Desktop remoto):</span><span class="sxs-lookup"><span data-stu-id="b3164-112">To create the first virtual machine, execute the following command to create a Windows VM with a public IP address that is accessible over port 3389 (Remote Desktop):</span></span>

    ``` PowerShell
    az vm create --name dataProcessingStage1 --resource-group MyResourceGroup --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. <span data-ttu-id="b3164-113">Fornire i valori per la password quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="b3164-113">Supply values for your password at the prompts.</span></span>

1. <span data-ttu-id="b3164-114">Eseguire questo comando per creare una VM Windows senza indirizzo IP pubblico:</span><span class="sxs-lookup"><span data-stu-id="b3164-114">Execute the following command to create a Windows VM without a public IP address:</span></span>

    ```PowerShell
    az vm create -n dataProcessingStage2 -g MyResourceGroup --public-ip-address '' --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. <span data-ttu-id="b3164-115">Al termine, l'output del secondo comando restituirà un valore per publicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="b3164-115">When completed, the output from the second command will return a value for publicIpAddress.</span></span>

## <a name="connect-to-dataprocessingstage1-using-remote-desktop"></a><span data-ttu-id="b3164-116">Connettersi a dataProcessingStage1 mediante Desktop remoto</span><span class="sxs-lookup"><span data-stu-id="b3164-116">Connect to dataProcessingStage1 using Remote Desktop</span></span>

1. <span data-ttu-id="b3164-117">Nel computer client premere il tasto Windows e digitare RDP.</span><span class="sxs-lookup"><span data-stu-id="b3164-117">On your client computer, press the Windows key and type RDP.</span></span>

1. <span data-ttu-id="b3164-118">Assicurarsi che l'app **Connessione Desktop remoto** sia selezionata e quindi premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="b3164-118">Ensure that **Remote Desktop Connection** app is selected, and then press Enter.</span></span>

1. <span data-ttu-id="b3164-119">Nella finestra di dialogo **Connessione desktop remoto** immettere il valore di dataProcessingStage1PublicIPAddress nel campo **Computer** e quindi fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="b3164-119">In the **Remote Desktop Connection** dialog box, in the **Computer** field, enter the value of dataProcessingStage1PublicIPAddress, and then click **Connect**.</span></span>

1. <span data-ttu-id="b3164-120">Nella finestra di dialogo **La connessione remota è attendibile?** fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="b3164-120">In the **Do you trust this remote connection?** dialog box, click **Connect**.</span></span>

1. <span data-ttu-id="b3164-121">Nella finestra di dialogo **Sicurezza di Windows** immettere il nome utente e la password usati durante la creazione di dataProcessingStage1.</span><span class="sxs-lookup"><span data-stu-id="b3164-121">In the **Windows Security** dialog box, enter the user name and password you used when you created dataProcessingStage1.</span></span>

1. <span data-ttu-id="b3164-122">Nella finestra di dialogo **Connessione Desktop remoto** fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3164-122">In the **Remote Desktop Connection** dialog box, click **OK**.</span></span>

1. <span data-ttu-id="b3164-123">Accedere al computer remoto in Azure.</span><span class="sxs-lookup"><span data-stu-id="b3164-123">Sign in to the remote computer in Azure.</span></span>

1. <span data-ttu-id="b3164-124">Quando viene visualizzato il messaggio **Reti**, fare clic su **No**.</span><span class="sxs-lookup"><span data-stu-id="b3164-124">When the **Networks** message appears, click **No**.</span></span>

1. <span data-ttu-id="b3164-125">Chiudere Server Manager.</span><span class="sxs-lookup"><span data-stu-id="b3164-125">Close Server Manager.</span></span>

1. <span data-ttu-id="b3164-126">Nella sessione remota fare clic con il pulsante destro del mouse sul tasto Windows e quindi scegliere **Prompt dei comandi**.</span><span class="sxs-lookup"><span data-stu-id="b3164-126">In the remote session, right-click the Windows key and click **Command Prompt**.</span></span>

1. <span data-ttu-id="b3164-127">Nella finestra del prompt dei comandi digitare il comando seguente e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="b3164-127">In the command prompt window, type the following command and press Enter.</span></span>

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. <span data-ttu-id="b3164-128">Non dovrebbe essere restituita alcuna risposta dal computer remoto.</span><span class="sxs-lookup"><span data-stu-id="b3164-128">There should be no response from the remote computer.</span></span> <span data-ttu-id="b3164-129">Windows Firewall impedisce infatti per impostazione predefinita le risposte ICMP.</span><span class="sxs-lookup"><span data-stu-id="b3164-129">This is because by default, Windows Firewall prevents ICMP responses.</span></span>

## <a name="connect-to-dataprocessingstage2-using-remote-desktop"></a><span data-ttu-id="b3164-130">Connettersi a dataProcessingStage2 mediante Desktop remoto</span><span class="sxs-lookup"><span data-stu-id="b3164-130">Connect to dataProcessingStage2 using Remote Desktop</span></span>

1. <span data-ttu-id="b3164-131">Nel computer client premere il tasto Windows e digitare **RDP**.</span><span class="sxs-lookup"><span data-stu-id="b3164-131">On your client computer, press the Windows key and type **RDP**.</span></span> <span data-ttu-id="b3164-132">Selezionare l'app **Connessione Desktop remoto** e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="b3164-132">Select the **Remote Desktop Connection** app and press Enter.</span></span>

1. <span data-ttu-id="b3164-133">Nel campo **Computer** immettere il valore di dataProcessingStage2PublicIPAddress e quindi fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="b3164-133">In the **Computer** field, enter the value of dataProcessingStage2PublicIPAddress, and then click **Connect**.</span></span>

1. <span data-ttu-id="b3164-134">Nella finestra di dialogo **La connessione remota è attendibile?** fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="b3164-134">In the **Do you trust this remote connection?** dialog box, click **Connect**.</span></span>

1. <span data-ttu-id="b3164-135">Nella finestra di dialogo **Sicurezza di Windows** immettere il nome utente e la password usati durante la creazione di dataProcessingStage2.</span><span class="sxs-lookup"><span data-stu-id="b3164-135">In the **Windows Security** dialog box, enter the user name and password you used when you created dataProcessingStage2.</span></span>

1. <span data-ttu-id="b3164-136">Nella finestra di dialogo **Connessione Desktop remoto** fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3164-136">In the **Remote Desktop Connection** dialog box, click **OK**.</span></span> <span data-ttu-id="b3164-137">È ora possibile accedere al computer remoto in Azure.</span><span class="sxs-lookup"><span data-stu-id="b3164-137">You now sign in to the remote computer in Azure.</span></span>

1. <span data-ttu-id="b3164-138">Quando viene visualizzato il messaggio **Reti**, fare clic su **No**.</span><span class="sxs-lookup"><span data-stu-id="b3164-138">When the **Networks** message appears, click **No**.</span></span>

1. <span data-ttu-id="b3164-139">Chiudere Server Manager.</span><span class="sxs-lookup"><span data-stu-id="b3164-139">Close Server Manager.</span></span>

1. <span data-ttu-id="b3164-140">In dataProcessingStage2 premere il tasto Windows, digitare **Firewall** e quindi premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="b3164-140">On dataProcessingStage2, press the Windows key, type **Firewall**, and press Enter.</span></span> <span data-ttu-id="b3164-141">Viene visualizzata la console **Windows Firewall con sicurezza avanzata**.</span><span class="sxs-lookup"><span data-stu-id="b3164-141">The **Windows Firewall with Advanced Security** console appears.</span></span>

1. <span data-ttu-id="b3164-142">Nel riquadro a sinistra fare clic su **Regole in ingresso**.</span><span class="sxs-lookup"><span data-stu-id="b3164-142">In the left-hand pane, click **Inbound Rules**.</span></span>

1. <span data-ttu-id="b3164-143">Nel riquadro a destra scorrere verso il basso e fare clic con il pulsante destro del mouse su **Condivisione file e stampanti (richiesta echo - ICMPv4-In)**, quindi scegliere **Abilita regola**.</span><span class="sxs-lookup"><span data-stu-id="b3164-143">In the right-hand pane, scroll down and right-click **File and Printer Sharing (Echo Request - ICMPv4-In)**, and then click **Enable Rule**.</span></span>

1. <span data-ttu-id="b3164-144">Tornare alla console di dataProcessingStage1, digitare questo comando nella finestra del prompt dei comandi e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="b3164-144">Switch back to the dataProcessingStage1 console and in the command prompt window, type the following command and press Enter.</span></span>

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. <span data-ttu-id="b3164-145">dataProcessingStage2 risponde con quattro risposte, dimostrando la connettività tra le due VM.</span><span class="sxs-lookup"><span data-stu-id="b3164-145">dataProcessingStage2 responds with four replies, demonstrating connectivity between the two VMs.</span></span>

## <a name="summary"></a><span data-ttu-id="b3164-146">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="b3164-146">Summary</span></span>

<span data-ttu-id="b3164-147">È stata creata una rete virtuale, sono state create due VM collegate a tale rete virtuale, è stata eseguita la connessione a una VM ed è stata dimostrata la connettività di rete all'altra VM nella stessa rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="b3164-147">You have successfully created a virtual network, created two VMs that are attached to that virtual network, connected to one of the VMs and shown network connectivity to the other VM within the same virtual network.</span></span> <span data-ttu-id="b3164-148">È possibile usare la rete virtuale di Azure per connettere risorse all'interno della rete di Azure.</span><span class="sxs-lookup"><span data-stu-id="b3164-148">You can use Azure Virtual Network to connect resources within the Azure network.</span></span> <span data-ttu-id="b3164-149">Queste risorse devono essere tuttavia incluse nello stesso gruppo di risorse e nella stessa sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="b3164-149">However, those resources need to be within the same resource group and subscription.</span></span> <span data-ttu-id="b3164-150">Nel prossimo esercizio verranno esaminati i gateway VPN, che consentono di connettere reti virtuali in gruppi di risorse diversi, sottoscrizioni diverse e anche aree geografiche diverse.</span><span class="sxs-lookup"><span data-stu-id="b3164-150">Next, we will look at VPN gateways, which enable you to connect virtual network in different resource groups, subscriptions, and even geographical regions.</span></span>
