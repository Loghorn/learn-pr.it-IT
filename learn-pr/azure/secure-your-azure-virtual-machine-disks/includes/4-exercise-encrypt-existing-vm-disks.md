<span data-ttu-id="2a968-101">Si supponga di sviluppare un'applicazione di gestione finanziaria per nuove startup.</span><span class="sxs-lookup"><span data-stu-id="2a968-101">Suppose you are developing a financial management application for new business startups.</span></span> <span data-ttu-id="2a968-102">Poiché è necessario assicurarsi che tutti i dati dei clienti siano protetti, si è deciso di implementare Crittografia dischi di Azure in tutti i dischi del sistema operativo e di dati sui server che ospiteranno l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2a968-102">You want to ensure that all your customers' data is secured, so you have decided to implement Azure Disk Encryption (ADE) across all OS and data disks on the servers that will host this application.</span></span> <span data-ttu-id="2a968-103">Per soddisfare i requisiti di conformità, è anche necessario essere responsabili della gestione delle chiavi di crittografia.</span><span class="sxs-lookup"><span data-stu-id="2a968-103">As part of your compliance requirements, you also need to be responsible for your own encryption key management.</span></span>

<span data-ttu-id="2a968-104">In questa unità si crittograferanno i dischi nelle macchine virtuali Windows esistenti e si gestiranno le chiavi di crittografia usando Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2a968-104">In this unit, you'll encrypt disks on existing Windows VMs, and manage the encryption keys using your own Azure Key Vault.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="2a968-105">In questo esercizio si presuppone che Azure PowerShell sia installato nel computer.</span><span class="sxs-lookup"><span data-stu-id="2a968-105">This exercise assumes that Azure PowerShell is installed on your computer.</span></span> <span data-ttu-id="2a968-106">Per informazioni su come installare Azure PowerShell, vedere [Installare Azure PowerShell in Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0).</span><span class="sxs-lookup"><span data-stu-id="2a968-106">Go to [Install Azure PowerShell on Windows with PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) for information on how to install Azure PowerShell.</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="2a968-107">Preparare l'ambiente</span><span class="sxs-lookup"><span data-stu-id="2a968-107">Prepare the environment</span></span>

<span data-ttu-id="2a968-108">Si inizierà distribuendo una macchina virtuale Windows in un nuovo gruppo di risorse e quindi si aggiungerà un disco dati alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="2a968-108">We'll start by deploying a Windows VM to a new resource group, and then add a data disk to the VM.</span></span>

### <a name="deploy-windows-vm-using-the-azure-portal"></a><span data-ttu-id="2a968-109">Distribuire una macchina virtuale Windows con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="2a968-109">Deploy Windows VM using the Azure portal</span></span>

<span data-ttu-id="2a968-110">Si userà ora il portale di Azure per creare e distribuire una macchina virtuale Windows.</span><span class="sxs-lookup"><span data-stu-id="2a968-110">Here you'll use the Azure portal to create and deploy a Windows VM.</span></span> <span data-ttu-id="2a968-111">Iniziare definendo le informazioni di base sulla macchina virtuale:</span><span class="sxs-lookup"><span data-stu-id="2a968-111">Start by defining the basic VM information:</span></span>

1. <span data-ttu-id="2a968-112">In un browser passare al [portale di Azure](http://portal.azure.com) e accedere con le credenziali normali.</span><span class="sxs-lookup"><span data-stu-id="2a968-112">In a browser, navigate to the [Azure portal](http://portal.azure.com) and sign in with your normal credentials.</span></span>

1. <span data-ttu-id="2a968-113">Nella barra laterale fare clic su **Macchine virtuali** e quindi su **Crea macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="2a968-113">In the sidebar, click **Virtual machines**, and then click **Create virtual machine**.</span></span>

1. <span data-ttu-id="2a968-114">Nel pannello Calcolo, nella sezione **Consigliati** fare clic su **Windows Server**.</span><span class="sxs-lookup"><span data-stu-id="2a968-114">On the Compute blade, in the **Recommended** section, click **Windows Server**.</span></span>

1. <span data-ttu-id="2a968-115">Nel pannello **Windows Server** fare clic su **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="2a968-115">In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="2a968-116">Nel pannello **Windows Server 2016 Datacenter** fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2a968-116">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="2a968-117">Nel pannello **Informazioni di base**, nella casella **Nome** digitare **moneyappsvr01.**</span><span class="sxs-lookup"><span data-stu-id="2a968-117">In the **Basics** blade, in the **Name** box, type **moneyappsvr01.**</span></span>

1. <span data-ttu-id="2a968-118">Nelle caselle **Nome utente** e **Password** digitare un nome e una password per un account amministratore su questo server.</span><span class="sxs-lookup"><span data-stu-id="2a968-118">In the **Username** and **Password boxes**, type a name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="2a968-119">Nella casella **Sottoscrizione** selezionare la sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="2a968-119">In the **Subscription** box, select your Azure subscription.</span></span>

1. <span data-ttu-id="2a968-120">In **Gruppo di risorse** selezionare **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="2a968-120">Under **Resource Group**, select **Create new**.</span></span> <span data-ttu-id="2a968-121">Nella casella digitare **moneyapprg**.</span><span class="sxs-lookup"><span data-stu-id="2a968-121">In the box, type **moneyapprg**.</span></span>

1. <span data-ttu-id="2a968-122">Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.</span><span class="sxs-lookup"><span data-stu-id="2a968-122">In the **Location** drop-down list, select a region near you.</span></span>

1. <span data-ttu-id="2a968-123">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a968-123">Click **OK**.</span></span>

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a><span data-ttu-id="2a968-124">Scegliere una dimensione per la macchina virtuale e avviare la distribuzione</span><span class="sxs-lookup"><span data-stu-id="2a968-124">Choose a size for the VM, and start the deployment</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a968-125">Tenere presente che le macchine virtuali di livello Basic non supportano Crittografia dischi di Azure.</span><span class="sxs-lookup"><span data-stu-id="2a968-125">Remember that basic tier VMs do not support ADE.</span></span>

1. <span data-ttu-id="2a968-126">Nel pannello **Scegli una dimensione** selezionare uno SKU **Standard**, ad esempio **B1s**.</span><span class="sxs-lookup"><span data-stu-id="2a968-126">On the **Choose a size** blade, select a **Standard** SKU, such as **B1s**.</span></span> <span data-ttu-id="2a968-127">Fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="2a968-127">Then click **Select**.</span></span>

1. <span data-ttu-id="2a968-128">Nel pannello **Impostazioni**, nell'elenco **Selezionare le porte in ingresso pubbliche** fare clic su **RDP**.</span><span class="sxs-lookup"><span data-stu-id="2a968-128">On the **Settings** blade, in the **Select public inbound ports** list, click **RDP**.</span></span> <span data-ttu-id="2a968-129">Scorrere quindi verso il basso e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a968-129">Then scroll down and click **OK**.</span></span>

1. <span data-ttu-id="2a968-130">Nel pannello **Crea** fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2a968-130">On the **Create** blade, click **Create**.</span></span>

1. <span data-ttu-id="2a968-131">Attendere il completamento della distribuzione della macchina virtuale prima di continuare con l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="2a968-131">Wait until the VM has deployed before continuing with the exercise.</span></span>

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="2a968-132">Aggiungere un disco dati alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="2a968-132">Add a data disk to the VM</span></span>

1. <span data-ttu-id="2a968-133">Nel menu di sinistra fare clic su **Tutte le risorse** e quindi su **moneyappsvr01**.</span><span class="sxs-lookup"><span data-stu-id="2a968-133">In the left menu, click **All resources**, and then click **moneyappsvr01**.</span></span>

1. <span data-ttu-id="2a968-134">Nel pannello **Macchina virtuale**, in **IMPOSTAZIONI** fare clic su **Dischi**.</span><span class="sxs-lookup"><span data-stu-id="2a968-134">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="2a968-135">Nel pannello **Dischi** si noti che lo stato della crittografia del disco del sistema operativo è attualmente **Non abilitato** e quindi fare clic su **Aggiungi disco dati**.</span><span class="sxs-lookup"><span data-stu-id="2a968-135">On the **Disks** blade, note that the OS disk encryption status is currently **Not enabled**, and then click **Add data disk**.</span></span>

1. <span data-ttu-id="2a968-136">Fare clic nell'elenco **Nome** e quindi su **Crea disco**.</span><span class="sxs-lookup"><span data-stu-id="2a968-136">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="2a968-137">Nel pannello **Crea disco gestito**, nella casella **Nome** digitare **moneyappsvr01_data**.</span><span class="sxs-lookup"><span data-stu-id="2a968-137">In the **Create managed disk** blade, in the **Name** box, type **moneyappsvr01_data**.</span></span>

1. <span data-ttu-id="2a968-138">In **Gruppo di risorse** selezionare **Usa esistente** e nell'elenco selezionare **moneyapprg**.</span><span class="sxs-lookup"><span data-stu-id="2a968-138">Under **Resource Group**, select **Use existing**, and in the list, select **moneyapprg**.</span></span>

1. <span data-ttu-id="2a968-139">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2a968-139">Click **Create**.</span></span>

1. <span data-ttu-id="2a968-140">Attendere il completamento della creazione del disco prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="2a968-140">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="2a968-141">Nel pannello **Dischi** fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="2a968-141">On the **Disks** blade, click **Save**.</span></span> <span data-ttu-id="2a968-142">Si noti che lo stato della crittografia del disco dati è attualmente **Non abilitato**.</span><span class="sxs-lookup"><span data-stu-id="2a968-142">Note that the data disk encryption status is currently **Not enabled**.</span></span>

## <a name="configure-disk-encryption-prerequisites"></a><span data-ttu-id="2a968-143">Configurare i prerequisiti di crittografia dei dischi</span><span class="sxs-lookup"><span data-stu-id="2a968-143">Configure disk encryption prerequisites</span></span>

<span data-ttu-id="2a968-144">Si userà ora lo script di configurazione dei prerequisiti di Crittografia dischi di Azure per configurare tutti i prerequisiti di crittografia dei dischi.</span><span class="sxs-lookup"><span data-stu-id="2a968-144">You'll now use the Azure Disk Encryption prerequisites configuration script to configure all the disk encryption prerequisites.</span></span> <span data-ttu-id="2a968-145">Questo script crea e prepara un insieme di credenziali delle chiavi nella stessa area della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="2a968-145">This script will create and prepare a key vault in the same region as your VM.</span></span>

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="2a968-146">Preparare lo script di configurazione dei prerequisiti di Crittografia dischi di Azure</span><span class="sxs-lookup"><span data-stu-id="2a968-146">Prepare the Azure Disk Encryption prerequisite setup script</span></span>

1. <span data-ttu-id="2a968-147">Andare alla pagina di GitHub [Azure Disk Encryption prerequisite setup script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1) (Script di configurazione dei prerequisiti di Crittografia dischi di Azure).</span><span class="sxs-lookup"><span data-stu-id="2a968-147">Go to the [Azure Disk Encryption prerequisite setup script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1) GitHub page.</span></span>

1. <span data-ttu-id="2a968-148">Nella pagina di GibHub fare clic su **Raw** (Non elaborato).</span><span class="sxs-lookup"><span data-stu-id="2a968-148">On the GibHub page, click **Raw**.</span></span>

1. <span data-ttu-id="2a968-149">Premere CTRL+A per selezionare tutto il testo della pagina e quindi CTRL+C per copiarlo negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="2a968-149">Use Ctrl-A to select all the text on the page, and then use Ctrl-C to copy all the text on the page to the clipboard.</span></span>

1. <span data-ttu-id="2a968-150">Nel computer fare clic su **Start** e quindi passare a **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="2a968-150">On your computer, click **Start**, and then browse to **Windows PowerShell ISE**.</span></span>

1. <span data-ttu-id="2a968-151">Fare clic con il pulsante destro del mouse su **Windows PowerShell ISE** e scegliere **Esegui come amministratore**.</span><span class="sxs-lookup"><span data-stu-id="2a968-151">Right-click **Windows PowerShell ISE**, and click **Run as administrator**.</span></span>

1. <span data-ttu-id="2a968-152">Nella finestra Amministratore: Windows PowerShell ISE fare clic su **Visualizza** e quindi su **Mostra riquadro di script**.</span><span class="sxs-lookup"><span data-stu-id="2a968-152">In the Administrator: Windows PowerShell ISE window, click **View**, and then click **Show Script Pane**.</span></span>

1. <span data-ttu-id="2a968-153">Incollare il testo copiato nel riquadro di script.</span><span class="sxs-lookup"><span data-stu-id="2a968-153">Paste the copied text into the script pane.</span></span>

1. <span data-ttu-id="2a968-154">Nel riquadro di script individuare il blocco di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2a968-154">In the script pane, locate the following block of code:</span></span>

    ```powershell
    [Parameter(Mandatory = $false,
            HelpMessage="Name of the AAD application that will be used to write secrets to KeyVault. A new application with this name will be created if one doesn't exist. If this app already exists, pass aadClientSecret parameter to the script")]
    [ValidateNotNullOrEmpty()]
    [string]$aadAppName,
    ```
1. <span data-ttu-id="2a968-155">Nel blocco di codice impostare `$false` su `$true`.</span><span class="sxs-lookup"><span data-stu-id="2a968-155">In the code block, change `$false` to `$true`.</span></span>

1. <span data-ttu-id="2a968-156">Fare clic su **File**, quindi su **Salva con nome** e passare alla cartella che si vuole usare per salvare lo script.</span><span class="sxs-lookup"><span data-stu-id="2a968-156">Click **File**, then click **Save As**, and navigate to the folder you'd like to use to save the script.</span></span>

1. <span data-ttu-id="2a968-157">Nella casella **Nome file** digitare **ADEPrereqScript.ps1** e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="2a968-157">In the **File name** box, type **ADEPrereqScript.ps1**, and click **Save**.</span></span>

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="2a968-158">Eseguire lo script di configurazione dei prerequisiti di Crittografia dischi di Azure</span><span class="sxs-lookup"><span data-stu-id="2a968-158">Run the Azure Disk Encryption prerequisite setup script</span></span>

1. <span data-ttu-id="2a968-159">Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:</span><span class="sxs-lookup"><span data-stu-id="2a968-159">In the  PowerShell ISE console pane, type the following command, and press **Enter**:</span></span>

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. <span data-ttu-id="2a968-160">Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:</span><span class="sxs-lookup"><span data-stu-id="2a968-160">In the PowerShell ISE console pane, type the following command, and press **Enter**:</span></span>

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   <span data-ttu-id="2a968-161">Se viene visualizzata la finestra di dialogo **Modifica ai criteri di esecuzione** fare clic su **Sì per tutti** o su **Sì** (se non viene visualizzata l'opzione _Sì per tutti_).</span><span class="sxs-lookup"><span data-stu-id="2a968-161">If you get an **Execution Policy Change** dialog box, click either **Yes to all** or **Yes** (if you do not get a _Yes to all_ option).</span></span>

1. <span data-ttu-id="2a968-162">Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:</span><span class="sxs-lookup"><span data-stu-id="2a968-162">In the  PowerShell ISE console pane, type the following command, and press **Enter**:</span></span>

   ```powershell
   Login-AzureRmAccount
   ```

1. <span data-ttu-id="2a968-163">Immettere le credenziali di Azure.</span><span class="sxs-lookup"><span data-stu-id="2a968-163">Enter your Azure credentials.</span></span>

1. <span data-ttu-id="2a968-164">Selezionare la stringa **SubscriptionId** e copiarla negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="2a968-164">Select your **SubscriptionId** string, and copy it to the clipboard.</span></span>

1. <span data-ttu-id="2a968-165">In PowerShell ISE fare clic su **File** e quindi su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="2a968-165">In the PowerShell ISE, click **File**, and then click **Run**.</span></span>

1. <span data-ttu-id="2a968-166">Nel riquadro della console, al prompt **resourceGroupName:** digitare **moneyapprg**.</span><span class="sxs-lookup"><span data-stu-id="2a968-166">In the console pane, at the **resourceGroupName:** prompt, type  **moneyapprg**.</span></span> <span data-ttu-id="2a968-167">Premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="2a968-167">Then press **Enter**.</span></span>

1. <span data-ttu-id="2a968-168">Nel riquadro della console, al prompt **keyVaultName:** digitare **moneyappkv**.</span><span class="sxs-lookup"><span data-stu-id="2a968-168">In the console pane, at the **keyVaultName:** prompt, type **moneyappkv**.</span></span> <span data-ttu-id="2a968-169">Premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="2a968-169">Then press **Enter**.</span></span>

1. <span data-ttu-id="2a968-170">Nel riquadro della console, al prompt **location:** digitare la posizione usata durante la creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="2a968-170">In the console pane, at the **location:** prompt, type the location you used when creating your VM.</span></span>

1. <span data-ttu-id="2a968-171">Nel riquadro della console, al prompt **subscriptionId:** incollare l'ID della sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="2a968-171">In the console pane, at the **subscriptionId:** prompt, paste your subscription ID.</span></span>

1. <span data-ttu-id="2a968-172">Verrà ora creato l'insieme di credenziali delle chiavi **moneyappkv**.</span><span class="sxs-lookup"><span data-stu-id="2a968-172">The **moneyappkv** key vault will now be created.</span></span> <span data-ttu-id="2a968-173">Al termine, selezionare il testo di riepilogo (in verde) e copiarlo nel Blocco note.</span><span class="sxs-lookup"><span data-stu-id="2a968-173">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="2a968-174">Premere **INVIO** per continuare.</span><span class="sxs-lookup"><span data-stu-id="2a968-174">Press **Enter** to continue.</span></span>

1. <span data-ttu-id="2a968-175">Nel riquadro della console, al prompt **aadAppName:** digitare **moneyapp**.</span><span class="sxs-lookup"><span data-stu-id="2a968-175">In the console pane, at the **aadAppName:**  prompt, type **moneyapp**.</span></span> <span data-ttu-id="2a968-176">Premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="2a968-176">Then press **Enter**.</span></span>

1. <span data-ttu-id="2a968-177">Verrà ora creata l'applicazione di Azure AD **moneyapp**.</span><span class="sxs-lookup"><span data-stu-id="2a968-177">The **moneyapp** Azure AD application will now be created.</span></span> <span data-ttu-id="2a968-178">Al termine, selezionare il testo di riepilogo (in verde) e copiarlo nel Blocco note.</span><span class="sxs-lookup"><span data-stu-id="2a968-178">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="2a968-179">Premere **INVIO** per continuare.</span><span class="sxs-lookup"><span data-stu-id="2a968-179">Press **Enter** to continue.</span></span>

### <a name="encrypt-your-vm-disks-with-powershell"></a><span data-ttu-id="2a968-180">Crittografare i dischi delle macchina virtuale con PowerShell</span><span class="sxs-lookup"><span data-stu-id="2a968-180">Encrypt your VM disks with PowerShell</span></span>

<span data-ttu-id="2a968-181">Verificare lo stato della crittografia dei dischi del sistema operativo e dei dati:</span><span class="sxs-lookup"><span data-stu-id="2a968-181">Verify the encryption status of the OS and data disks:</span></span>

1. <span data-ttu-id="2a968-182">Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:</span><span class="sxs-lookup"><span data-stu-id="2a968-182">In the PowerShell ISE console pane, type the following command, and press **Enter**:</span></span>

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > <span data-ttu-id="2a968-183">Il nome della macchina virtuale deve essere racchiuso tra virgolette singole.</span><span class="sxs-lookup"><span data-stu-id="2a968-183">The VM name must be enclosed in single quotes.</span></span>

1. <span data-ttu-id="2a968-184">Nel riquadro di script di PowerShell ISE immettere il comando seguente e premere **INVIO**:</span><span class="sxs-lookup"><span data-stu-id="2a968-184">In the PowerShell ISE script pane, enter the following command, and press **Enter**:</span></span>

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. <span data-ttu-id="2a968-185">Nella finestra di dialogo **Enable AzureDiskEncryption on the VM** (Abilita Crittografia dischi di Azure nella VM) fare clic su **Sì** e notare il messaggio indicante che il completamento della crittografia può richiedere 10-15 minuti.</span><span class="sxs-lookup"><span data-stu-id="2a968-185">In the **Enable AzureDiskEncryption on the VM** dialog box, click **Yes**, and note the message that encryption may take 10-15 minutes to complete.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="2a968-186">Attendere il completamento del comando prima di continuare con questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="2a968-186">Wait until the command has completed before continuing with this exercise.</span></span>

### <a name="verify-the-encryption-status-of-your-vm-disks"></a><span data-ttu-id="2a968-187">Verificare lo stato della crittografia dei dischi delle macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="2a968-187">Verify the encryption status of your VM disks</span></span>

<span data-ttu-id="2a968-188">Accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="2a968-188">Switch to the Azure portal.</span></span> <span data-ttu-id="2a968-189">Nel pannello **Dischi** per **moneyappsvr01** si noti che lo stato della crittografia per i dischi del sistema operativo e dei dati ora è **Abilitato**.</span><span class="sxs-lookup"><span data-stu-id="2a968-189">On the **Disks** blade for **moneyappsvr01**, note that the disk encryption status for the OS and Data disks is now **Enabled**.</span></span>
