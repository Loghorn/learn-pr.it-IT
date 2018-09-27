<span data-ttu-id="83ced-101">In questo esercizio si crea una macchina virtuale e quindi la si ridimensiona usando il portale e Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="83ced-101">In this exercise, you will create a virtual machine and then resize it using the portal and Azure PowerShell.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-vm-with-powershell"></a><span data-ttu-id="83ced-102">Creare una VM con PowerShell</span><span class="sxs-lookup"><span data-stu-id="83ced-102">Create a VM with PowerShell</span></span>

1. <span data-ttu-id="83ced-103">Usare il cmdlet `New-AzureRmVm` in Azure PowerShell per creare una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="83ced-103">Let's use the `New-AzureRmVm` cmdlet in Azure PowerShell to create a VM.</span></span>
    - <span data-ttu-id="83ced-104">Usare il gruppo di risorse **<rgn>[Nome gruppo di risorse sandbox]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="83ced-104">Use the Resource Group **<rgn>[sandbox resource group name]</rgn>**.</span></span>
    - <span data-ttu-id="83ced-105">Assegnare il nome "my-test-vm".</span><span class="sxs-lookup"><span data-stu-id="83ced-105">Name it "my-test-vm".</span></span>
    - <span data-ttu-id="83ced-106">Passare _Standard_DS2_v2_ per il parametro **Dimensioni**.</span><span class="sxs-lookup"><span data-stu-id="83ced-106">Pass in _Standard_DS2_v2_ for the **Size** parameter.</span></span>
    - <span data-ttu-id="83ced-107">Selezionare una località vicina dall'elenco seguente, disponibile nella sandbox di Azure.</span><span class="sxs-lookup"><span data-stu-id="83ced-107">Select a location close to you from the following list available in the Azure sandbox.</span></span> <span data-ttu-id="83ced-108">Assicurarsi di modificare il valore nel comando di esempio seguente, se si usa Copia e incolla.</span><span class="sxs-lookup"><span data-stu-id="83ced-108">Make sure to change the value in the below example command if you are using copy and paste.</span></span>

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - <span data-ttu-id="83ced-109">Usare il cmdlet `Get-Credential` e inviare i risultati nel parametro `Credential` come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="83ced-109">Use the `Get-Credential` cmdlet and feed the results into the `Credential` parameter as shown below.</span></span>

       <span data-ttu-id="83ced-110">Quando viene richiesto di immettere le credenziali, usare LocalAdmin come nome utente e Adm1nPa$$word come password.</span><span class="sxs-lookup"><span data-stu-id="83ced-110">When prompted to enter credentials, use LocalAdmin as the user name and Adm1nPa$$word as the password.</span></span>

    ```powershell
    New-AzureRmVm `
        -ResourceGroupName <rgn>[sandbox resource group name]</rgn> `
        -Name my-test-vm `
        -Credential (Get-Credential) `
        -Size "Standard_DS2_v2" `
        -Location eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


1. <span data-ttu-id="83ced-111">Attendere il completamento della distribuzione prima di continuare con l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="83ced-111">Wait until the deployment is complete before continuing the exercise.</span></span>

## <a name="resize-using-the-portal"></a><span data-ttu-id="83ced-112">Eseguire il ridimensionamento tramite il portale</span><span class="sxs-lookup"><span data-stu-id="83ced-112">Resize using the portal</span></span>

1. <span data-ttu-id="83ced-113">Accedere al [portale di Azure per sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="83ced-113">Sign into the [Azure portal for sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="83ced-114">Selezionare **Gruppi di risorse** dalla barra laterale sinistra.</span><span class="sxs-lookup"><span data-stu-id="83ced-114">Select **Resource Groups** from the left side bar.</span></span>

1. <span data-ttu-id="83ced-115">Selezionare il gruppo di risorse <rgn>[Nome gruppo di risorse sandbox]</rgn> e nella panoramica fare clic sull'oggetto macchina virtuale **my-test-vm**.</span><span class="sxs-lookup"><span data-stu-id="83ced-115">Select the <rgn>[sandbox resource group name]</rgn> resource group, and in the overview, click the **my-test-vm** virtual machine object.</span></span>

1. <span data-ttu-id="83ced-116">Nella **panoramica** della macchina virtuale si può notare che la dimensione corrente della macchina virtuale è _DS2 Standard v2 (2 vcpu, 7 GB memory)_ ovvero ciò che si è creato qualche istante fa.</span><span class="sxs-lookup"><span data-stu-id="83ced-116">On the **Overview** of the virtual machine, notice that the current size of the VM is _Standard DS2 v2 (2 vcpus, 7 GB memory)_ which is what we created a moment ago.</span></span>

1. <span data-ttu-id="83ced-117">Nella sezione delle **impostazioni** selezionare **Dimensioni**.</span><span class="sxs-lookup"><span data-stu-id="83ced-117">In the **Settings** section, select **Size**.</span></span>

1. <span data-ttu-id="83ced-118">Nel pannello **Scegli una dimensione** provare a trovare la dimensione **F2s_v2**.</span><span class="sxs-lookup"><span data-stu-id="83ced-118">On the **Choose a size** blade, try to find the **F2s_v2** size.</span></span> <span data-ttu-id="83ced-119">Questa non verrà visualizzata nell'elenco delle dimensioni disponibili perché la macchina virtuale è attualmente in esecuzione e quella dimensione appartiene a una famiglia diversa.</span><span class="sxs-lookup"><span data-stu-id="83ced-119">You will not see it in the list of available sizes because the VM is currently running and that size is from a different family.</span></span> <span data-ttu-id="83ced-120">In alcuni casi, è necessario arrestare la macchina virtuale per visualizzare tutte le dimensioni di VM disponibili.</span><span class="sxs-lookup"><span data-stu-id="83ced-120">In some cases, you will need to stop the VM in order to see all available VM sizes.</span></span>

1. <span data-ttu-id="83ced-121">Verrà quindi scelta una dimensione che è attualmente disponibile durante l'esecuzione di questa macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="83ced-121">Let's choose a size that is currently available while this VM is running.</span></span> <span data-ttu-id="83ced-122">Sempre nel pannello **Scegli una dimensione** selezionare _DS3_v2 Standard_ e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="83ced-122">While still on the **Choose a size** blade, select _DS3_v2 Standard_ and then click **Select**.</span></span> <span data-ttu-id="83ced-123">Osservare la notifica sul ridimensionamento della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="83ced-123">Notice the notification about resizing the virtual machine.</span></span>

1. <span data-ttu-id="83ced-124">Nel pannello **Panoramica** verificare che la macchina virtuale sia stata ridimensionata in _Standard DS3 v2 (4 vcpus, 14 GB memory)_.</span><span class="sxs-lookup"><span data-stu-id="83ced-124">On the **Overview** panel, confirm that the VM has been resized to _Standard DS3 v2 (4 vcpus, 14 GB memory)_.</span></span>

## <a name="resize-using-powershell"></a><span data-ttu-id="83ced-125">Eseguire il ridimensionamento con PowerShell</span><span class="sxs-lookup"><span data-stu-id="83ced-125">Resize using PowerShell</span></span>

1. <span data-ttu-id="83ced-126">Nel portale di Azure aprire Azure Cloud Shell facendo clic sul pulsante Cloud Shell nella barra degli strumenti superiore.</span><span class="sxs-lookup"><span data-stu-id="83ced-126">In the Azure portal, open Azure Cloud Shell by clicking the Cloud Shell button in the top toolbar.</span></span>

    <span data-ttu-id="83ced-127">Assicurarsi che Cloud Shell sia impostata in modo da usare PowerShell e non Bash, in alto a sinistra nella finestra di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="83ced-127">Ensure that the Cloud Shell is set to use PowerShell and not Bash, in the top left of the Cloud Shell window.</span></span>

1. <span data-ttu-id="83ced-128">Usare il cmdlet seguente per ottenere l'elenco delle dimensioni di macchine virtuali disponibili.</span><span class="sxs-lookup"><span data-stu-id="83ced-128">Use the following cmdlet to get the list of available virtual machine sizes.</span></span>

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    ```

1. <span data-ttu-id="83ced-129">Usare il cmdlet seguente per ridimensionare la macchina virtuale scegliendo di nuovo la dimensione _DS2_v2_.</span><span class="sxs-lookup"><span data-stu-id="83ced-129">Use the following cmdlet to resize the virtual machine back to a _DS2_v2_ size.</span></span>

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    $vm.HardwareProfile.VmSize = "Standard_DS2_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```

1. <span data-ttu-id="83ced-130">Fare clic sul pulsante di **aggiornamento** nel pannello **my-test-vm** mentre si attende il completamento del comando PowerShell.</span><span class="sxs-lookup"><span data-stu-id="83ced-130">Click the **Refresh** button in the **my-test-vm** blade while you are waiting for the PowerShell command to finish.</span></span> <span data-ttu-id="83ced-131">Si noterà che la macchina virtuale viene riavviata per supportare la modifica delle dimensioni.</span><span class="sxs-lookup"><span data-stu-id="83ced-131">You should notice that the virtual machine is restarting to accommodate the change in size.</span></span>

<span data-ttu-id="83ced-132">In questo esercizio è stata creata una macchina virtuale, che è stata ridimensionata con due diversi strumenti.</span><span class="sxs-lookup"><span data-stu-id="83ced-132">In this exercise, you created a virtual machine and resized it with two different tools.</span></span> <span data-ttu-id="83ced-133">È utile tenere presente che la dimensione di destinazione potrebbe non essere disponibile mentre la macchina virtuale è in esecuzione. Arrestando la macchina virtuale è possibile scegliere altre dimensioni.</span><span class="sxs-lookup"><span data-stu-id="83ced-133">A good tip to keep in mind is that the target size may not be available while the virtual machine is running; stopping the virtual machine lets you choose more sizes.</span></span>