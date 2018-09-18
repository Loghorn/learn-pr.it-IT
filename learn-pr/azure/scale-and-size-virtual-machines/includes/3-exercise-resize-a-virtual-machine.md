<span data-ttu-id="d6cd8-101">In questo esercizio si crea una macchina virtuale e quindi la si ridimensiona usando il portale e Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-101">In this exercise, you will create a virtual machine and then resize it using the portal and Azure PowerShell.</span></span>

## <a name="create-a-vm"></a><span data-ttu-id="d6cd8-102">Creare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="d6cd8-102">Create a VM</span></span>

1. <span data-ttu-id="d6cd8-103">Nel Web browser passare al [portale di Azure](https://portal.azure.com?azure-portal=true) e accedere al proprio account.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-103">In your web browser, navigate to the [Azure Portal](https://portal.azure.com?azure-portal=true) and sign into your account.</span></span>

1. <span data-ttu-id="d6cd8-104">Nel portale di Azure creare una nuova risorsa.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-104">In the Azure portal, create a new resource.</span></span> <span data-ttu-id="d6cd8-105">Nel pannello **Nuovo** digitare **macchina virtuale** nella casella di ricerca e quindi premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-105">In the **New** blade, type **virtual machine** in the search box, and then press ENTER.</span></span>

1. <span data-ttu-id="d6cd8-106">Nel pannello **Tutto**, in **Risultati** fare clic su **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-106">In the **Everything** blade, under **Results**, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="d6cd8-107">Nel pannello **Windows Server 2016 Datacenter** fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-107">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="d6cd8-108">Nel pannello **Nozioni di base** completare i dettagli usando le informazioni seguenti e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-108">In the **Basics** blade, complete the details using the following information, and then click **OK**.</span></span>

    |<span data-ttu-id="d6cd8-109">Impostazione</span><span class="sxs-lookup"><span data-stu-id="d6cd8-109">Setting</span></span>|<span data-ttu-id="d6cd8-110">Valore</span><span class="sxs-lookup"><span data-stu-id="d6cd8-110">Value</span></span>|
    |---|---|
    |<span data-ttu-id="d6cd8-111">Nome</span><span class="sxs-lookup"><span data-stu-id="d6cd8-111">Name</span></span>|<span data-ttu-id="d6cd8-112">DB01</span><span class="sxs-lookup"><span data-stu-id="d6cd8-112">DB01</span></span>|
    |<span data-ttu-id="d6cd8-113">Nome utente</span><span class="sxs-lookup"><span data-stu-id="d6cd8-113">Username</span></span>|<span data-ttu-id="d6cd8-114">LocalAdmin</span><span class="sxs-lookup"><span data-stu-id="d6cd8-114">LocalAdmin</span></span>|
    |<span data-ttu-id="d6cd8-115">Password e Conferma password</span><span class="sxs-lookup"><span data-stu-id="d6cd8-115">Password and Confirm password</span></span>|<span data-ttu-id="d6cd8-116">Adm1nPa$$word</span><span class="sxs-lookup"><span data-stu-id="d6cd8-116">Adm1nPa$$word</span></span>|
    |<span data-ttu-id="d6cd8-117">Gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="d6cd8-117">Resource group</span></span>|<span data-ttu-id="d6cd8-118">ExerciseRG</span><span class="sxs-lookup"><span data-stu-id="d6cd8-118">ExerciseRG</span></span>|
    |<span data-ttu-id="d6cd8-119">Località</span><span class="sxs-lookup"><span data-stu-id="d6cd8-119">Location</span></span>|<span data-ttu-id="d6cd8-120">Stati Uniti centrali</span><span class="sxs-lookup"><span data-stu-id="d6cd8-120">Central US</span></span>|

1. <span data-ttu-id="d6cd8-121">Nel pannello **Scegli una dimensione** selezionare **D2s_v3** e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-121">On the **Choose a size** blade, select **D2s_v3**, and then click **Select**.</span></span>

1. <span data-ttu-id="d6cd8-122">Nel pannello **Impostazioni**, in **Selezionare le porte in ingresso pubbliche** selezionare **HTTP**, **HTTPS** e **RDP (3389)**, in **Diagnostica di avvio** fare clic su **Disabilitata**, lasciare il valore predefinito per tutte le altre impostazioni e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-122">On the **Settings** blade, under **Select public inbound ports** select **HTTP**, **HTTPS**, and **RDP (3389)**, under **Boot diagnostics** click **Disabled**, leave all other settings at the default value, and then click **OK**.</span></span>

1. <span data-ttu-id="d6cd8-123">Nel pannello **Crea** fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-123">On the **Create** blade, click **Create**.</span></span>

1. <span data-ttu-id="d6cd8-124">Attendere il completamento della distribuzione prima di continuare con l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-124">Wait until the deployment is complete before continuing the exercise.</span></span>

## <a name="resize-using-the-portal"></a><span data-ttu-id="d6cd8-125">Eseguire il ridimensionamento tramite il portale</span><span class="sxs-lookup"><span data-stu-id="d6cd8-125">Resize using the portal</span></span>

1. <span data-ttu-id="d6cd8-126">Nel portale di Azure passare al gruppo di risorse ExerciseRG e nel pannello **ExerciseRG** fare clic sull'oggetto macchina virtuale **DB01**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-126">In the Azure portal, browse to the ExerciseRG resource group, and in the **ExerciseRG** blade, click the **DB01** virtual machine object.</span></span>

1. <span data-ttu-id="d6cd8-127">Nel pannello **DB01** fare clic su **Dimensione**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-127">On the **DB01** blade, click **Size**.</span></span> <span data-ttu-id="d6cd8-128">La dimensione attualmente evidenziata è quella selezionata al momento della creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-128">Note the currently highlighted size is the size you selected when creating the virtual machine.</span></span>

1. <span data-ttu-id="d6cd8-129">Nel pannello **Scegli una dimensione** cercare la dimensione **F2s_v2**, che non dovrebbe essere disponibile nell'elenco di dimensioni perché la macchina virtuale è in esecuzione e tale dimensione appartiene a una famiglia diversa.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-129">On the **Choose a size** blade, try to find the **F2s_v2** size - it should not be available in the list of sizes because the VM is currently running and that size is from a different family.</span></span> <span data-ttu-id="d6cd8-130">Scegliere il pannello **Scegli una dimensione**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-130">Close the **Choose a size** blade.</span></span>

1. <span data-ttu-id="d6cd8-131">Nel pannello **DB01** fare clic su **Arresta**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-131">In the **DB01** blade, click **Stop**.</span></span> <span data-ttu-id="d6cd8-132">Nella finestra di dialogo **Arresta questa macchina virtuale** fare clic su **Sì** e attendere che lo stato della macchina virtuale sia indicato come **Arrestato (deallocato)**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-132">In the **Stop this virtual machine** dialog box, click **Yes**, and wait for the virtual machine status to show **Stopped (deallocated)**.</span></span>

1. <span data-ttu-id="d6cd8-133">Nel pannello **DB01** fare clic su **Dimensione**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-133">On the **DB01** blade, click **Size**.</span></span> <span data-ttu-id="d6cd8-134">Nel pannello **Scegli una dimensione** selezionare **F2s_v2** e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-134">On the **Choose a size** blade, select **F2s_v2** and then click **Select**.</span></span> <span data-ttu-id="d6cd8-135">Osservare la notifica sul ridimensionamento della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-135">Notice the notification about resizing the virtual machine.</span></span>

1. <span data-ttu-id="d6cd8-136">Nel pannello **DB01** fare clic su **Panoramica** e quindi su **Avvia**.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-136">On the **DB01** blade, click **Overview**, then click **Start**.</span></span>

## <a name="resize-using-powershell"></a><span data-ttu-id="d6cd8-137">Eseguire il ridimensionamento tramite PowerShell</span><span class="sxs-lookup"><span data-stu-id="d6cd8-137">Resize using PowerShell</span></span>

1. <span data-ttu-id="d6cd8-138">Nel portale di Azure aprire Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-138">In the Azure portal, open the Cloud Shell.</span></span>

1. <span data-ttu-id="d6cd8-139">Usare il cmdlet seguente per ottenere l'elenco delle dimensioni di macchine virtuali disponibili.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-139">Use the following cmdlet to get the list of available virtual machine sizes.</span></span>

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName ExerciseRG -VMName DB01
    ```

1. <span data-ttu-id="d6cd8-140">Usare il cmdlet seguente per ridimensionare la macchina virtuale scegliendo la dimensione F4s_v2.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-140">Use the following cmdlet to resize the virtual machine to an F4s_v2 size.</span></span>

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName ExerciseRG -VMName DB01
    $vm.HardwareProfile.VmSize = "Standard_F4s_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName ExerciseRG
    ```

1. <span data-ttu-id="d6cd8-141">Fare clic sul pulsante Aggiorna nel pannello DB01 mentre si attende il completamento del comando PowerShell. È possibile notare che la macchina virtuale si sta riavviando per applicare la modifica della dimensione.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-141">Click the Refresh button in the DB01 blade while you are waiting for the PowerShell command to complete - you should notice that the virtual machine is restarting to accommodate the change in size.</span></span>

<span data-ttu-id="d6cd8-142">In questo esercizio è stata creata una macchina virtuale, che è stata ridimensionata con due diversi strumenti.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-142">In this exercise, you created a virtual machine and resized it with two different tools.</span></span> <span data-ttu-id="d6cd8-143">È utile tenere presente che la dimensione di destinazione potrebbe non essere disponibile mentre la macchina virtuale è in esecuzione. Arrestando la macchina virtuale è possibile scegliere altre dimensioni.</span><span class="sxs-lookup"><span data-stu-id="d6cd8-143">A good tip to keep in mind is that the target size may not be available while the virtual machine is running; stopping the virtual machine lets you choose more sizes.</span></span>