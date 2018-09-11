<span data-ttu-id="c7516-101">Si supponga di sviluppare un'applicazione di gestione finanziaria per nuove startup.</span><span class="sxs-lookup"><span data-stu-id="c7516-101">Suppose you are developing a financial management application for new business startups.</span></span> <span data-ttu-id="c7516-102">Poiché è necessario assicurarsi che tutti i dati dei clienti siano protetti, si è deciso di implementare Crittografia dischi di Azure in tutti i dischi del sistema operativo e di dati sui server che ospiteranno l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c7516-102">You want to ensure that all your customers' data is secured, so you have decided to implement ADE across all OS and data disks on the servers that will host this application.</span></span> <span data-ttu-id="c7516-103">Per soddisfare i requisiti di conformità, è anche necessario essere responsabili della gestione delle chiavi di crittografia.</span><span class="sxs-lookup"><span data-stu-id="c7516-103">As part of your compliance requirements, you also need to be responsible for your own encryption key management.</span></span>

<span data-ttu-id="c7516-104">In questa unità si crittograferanno i dischi nelle macchine virtuali Windows esistenti e si gestiranno le chiavi di crittografia usando Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="c7516-104">In this unit you'll encrypt disks on existing Windows VMs, and manage the encryption keys using your own Azure Key Vault.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="c7516-105">In questo esercizio si presuppone che Azure PowerShell sia installato nel computer. Per informazioni su come installare Azure PowerShell, vedere [Installare Azure PowerShell in Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0).</span><span class="sxs-lookup"><span data-stu-id="c7516-105">This exercise assumes that Azure PowerShell is installed on your computer; go to [Install Azure PowerShell on Windows with PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) for information on how to install Azure PowerShell.</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="c7516-106">Preparare l'ambiente</span><span class="sxs-lookup"><span data-stu-id="c7516-106">Prepare the environment</span></span>

<span data-ttu-id="c7516-107">Si inizierà distribuendo una macchina virtuale Windows in un nuovo gruppo di risorse e quindi si aggiungerà un disco dati alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="c7516-107">We'll start by deploying a Windows VM to a new resource group and then add a data disk to the VM.</span></span>

### <a name="deploy-windows-vm-using-azure-portal"></a><span data-ttu-id="c7516-108">Distribuire una macchina virtuale Windows con il portale di Azure</span><span class="sxs-lookup"><span data-stu-id="c7516-108">Deploy Windows VM using Azure portal</span></span>

<span data-ttu-id="c7516-109">Si userà ora il portale di Azure per creare e distribuire una macchina virtuale Windows.</span><span class="sxs-lookup"><span data-stu-id="c7516-109">Here you'll install use the Azure portal to create and deploy a Windows VM.</span></span> <span data-ttu-id="c7516-110">Iniziare definendo le informazioni di base sulla macchina virtuale:</span><span class="sxs-lookup"><span data-stu-id="c7516-110">Start by defining the basic VM information:</span></span>

1. <span data-ttu-id="c7516-111">In un browser passare al [portale di Azure](http://portal.azure.com) e accedere con le credenziali normali.</span><span class="sxs-lookup"><span data-stu-id="c7516-111">In a browser, navigate to the [Azure portal](http://portal.azure.com) and sign in with your normal credentials.</span></span>

1. <span data-ttu-id="c7516-112">Nella barra laterale fare clic su **Macchine virtuali** e quindi su **Crea macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="c7516-112">In the sidebar, click **Virtual machines**, and then click **Create virtual machine**.</span></span>

1. <span data-ttu-id="c7516-113">Nel pannello Calcolo, nella sezione **Consigliati** fare clic su **Windows Server**.</span><span class="sxs-lookup"><span data-stu-id="c7516-113">On the Compute blade, in the **Recommended** section, click **Windows Server**.</span></span>

1. <span data-ttu-id="c7516-114">Nel pannello **Windows Server** fare clic su **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="c7516-114">In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="c7516-115">Nel pannello **Windows Server 2016 Datacenter** fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c7516-115">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="c7516-116">Nel pannello **Informazioni di base**, nella casella **Nome** digitare **moneyappsvr01.**</span><span class="sxs-lookup"><span data-stu-id="c7516-116">In the **Basics** blade, in the **Name** box, type **moneyappsvr01.**</span></span>

1. <span data-ttu-id="c7516-117">Nelle caselle **Nome utente** e **Password** digitare un nome e una password per un account amministratore su questo server.</span><span class="sxs-lookup"><span data-stu-id="c7516-117">In the **Username** and **Password boxes**, type a name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="c7516-118">Nella casella **Sottoscrizione** selezionare la sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="c7516-118">In the **Subscription** box, select your Azure subscription.</span></span>

1. <span data-ttu-id="c7516-119">In **Gruppo di risorse** selezionare **Crea nuovo** e digitare **moneyapprg** nella casella.</span><span class="sxs-lookup"><span data-stu-id="c7516-119">Under **Resource Group**, select **Create new**, and in the box, type **moneyapprg**.</span></span>

1. <span data-ttu-id="c7516-120">Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.</span><span class="sxs-lookup"><span data-stu-id="c7516-120">In the **Location** drop-down list, select a region near you.</span></span>

1. <span data-ttu-id="c7516-121">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7516-121">Click **OK**.</span></span>

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a><span data-ttu-id="c7516-122">Scegliere una dimensione per la macchina virtuale e avviare la distribuzione</span><span class="sxs-lookup"><span data-stu-id="c7516-122">Choose a size for the VM, and start the deployment</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7516-123">Tenere presente che le macchine virtuali di livello Basic non supportano Crittografia dischi di Azure</span><span class="sxs-lookup"><span data-stu-id="c7516-123">Remember that basic tier VMs do not support ADE</span></span>

1. <span data-ttu-id="c7516-124">Nel pannello **Scegli una dimensione** selezionare uno SKU **Standard**, ad esempio **B1s** e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="c7516-124">On the **Choose a size** blade, select a **Standard** SKU, such as **B1s**, and then click **Select**.</span></span>

1. <span data-ttu-id="c7516-125">Nel pannello **Impostazioni**, nell'elenco **Selezionare le porte in ingresso pubbliche** fare clic su **RDP**, quindi scorrere verso il basso e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7516-125">On the **Settings** blade, in the **Select public inbound ports** list, click **RDP**, and then scroll down and click **OK**.</span></span>

1. <span data-ttu-id="c7516-126">Nel pannello **Crea** fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c7516-126">On the **Create** blade, click **Create**.</span></span>

1. <span data-ttu-id="c7516-127">Attendere il completamento della distribuzione della macchina virtuale prima di continuare con l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="c7516-127">Wait until the VM has deployed before continuing with the exercise.</span></span>

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="c7516-128">Aggiungere un disco dati alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="c7516-128">Add a data disk to the VM</span></span>

1. <span data-ttu-id="c7516-129">Nel menu di sinistra fare clic su **Tutte le risorse** e quindi su **moneyappsvr01**.</span><span class="sxs-lookup"><span data-stu-id="c7516-129">In the left menu, click **All resources**, and then click **moneyappsvr01**.</span></span>

1. <span data-ttu-id="c7516-130">Nel pannello **Macchina virtuale**, in **IMPOSTAZIONI** fare clic su **Dischi**.</span><span class="sxs-lookup"><span data-stu-id="c7516-130">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="c7516-131">Nel pannello **Dischi** si noti che lo stato della crittografia del disco del sistema operativo è attualmente **Non abilitato** e quindi fare clic su **Aggiungi disco dati**.</span><span class="sxs-lookup"><span data-stu-id="c7516-131">On the **Disks** blade, note that the OS disk encryption status is currently **Not enabled**, and then click **Add data disk**.</span></span>

1. <span data-ttu-id="c7516-132">Fare clic nell'elenco **Nome** e quindi su **Crea disco**.</span><span class="sxs-lookup"><span data-stu-id="c7516-132">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="c7516-133">Nel pannello **Crea disco gestito**, nella casella **Nome** digitare **moneyappsvr01_data**.</span><span class="sxs-lookup"><span data-stu-id="c7516-133">In the **Create managed disk** blade, in the **Name** box, type **moneyappsvr01_data**.</span></span>

1. <span data-ttu-id="c7516-134">In **Gruppo di risorse** selezionare **Usa esistente** e nell'elenco selezionare **moneyapprg**.</span><span class="sxs-lookup"><span data-stu-id="c7516-134">Under **Resource Group**, select **Use existing**, and in the list, select **moneyapprg**.</span></span>

1. <span data-ttu-id="c7516-135">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c7516-135">Click **Create**.</span></span>

1. <span data-ttu-id="c7516-136">Attendere il completamento della creazione del disco prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="c7516-136">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="c7516-137">Nel pannello **Dischi** fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="c7516-137">On the **Disks** blade, click **Save**.</span></span> <span data-ttu-id="c7516-138">Si noti che lo stato della crittografia del disco dati è attualmente **_Non abilitato_**.</span><span class="sxs-lookup"><span data-stu-id="c7516-138">Note that the data disk encryption status is currently **_Not enabled_**.</span></span>

## <a name="configure-disk-encryption-pre-requisites"></a><span data-ttu-id="c7516-139">Configurare i prerequisiti di crittografia dei dischi</span><span class="sxs-lookup"><span data-stu-id="c7516-139">Configure disk encryption pre-requisites</span></span>

<span data-ttu-id="c7516-140">Si userà ora lo script di configurazione dei prerequisiti di Crittografia dischi di Azure per configurare tutti i prerequisiti di crittografia dei dischi.</span><span class="sxs-lookup"><span data-stu-id="c7516-140">You'll now use the Azure disk encryption prerequisites configuration script to configure all the disk encryption pre-requisites.</span></span> <span data-ttu-id="c7516-141">Questo script crea e prepara un insieme di credenziali delle chiavi nella stessa area della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="c7516-141">This script will create and prepare a Key Vault in the same region as your VM.</span></span>

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="c7516-142">Preparare lo script di configurazione dei prerequisiti di Crittografia dischi di Azure</span><span class="sxs-lookup"><span data-stu-id="c7516-142">Prepare the Azure Disk Encryption Prerequisite Setup Script</span></span>

1. <span data-ttu-id="c7516-143">Andare alla pagina di GitHub [Azure Disk Encryption Prerequisite Setup Script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1) (Script di configurazione dei prerequisiti di Crittografia dischi di Azure).</span><span class="sxs-lookup"><span data-stu-id="c7516-143">Go to the [Azure Disk Encryption Prerequisite Setup Script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1)  GitHub page.</span></span>

1. <span data-ttu-id="c7516-144">Nella pagina di GibHub fare clic su **Raw** (Non elaborato).</span><span class="sxs-lookup"><span data-stu-id="c7516-144">On the GibHub page, click **Raw**.</span></span>

1. <span data-ttu-id="c7516-145">Premere CTRL+A per selezionare tutto il testo della pagina e quindi CTRL+C per copiarlo negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="c7516-145">Use CTRL-A to select all the text on the page and then use CTRL-C to copy all the text on the page to the clipboard.</span></span>

1. <span data-ttu-id="c7516-146">Nel computer fare clic su **Start**, quindi passare a **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="c7516-146">On your computer, click **Start**, then browse to **Windows PowerShell ISE**.</span></span>

1. <span data-ttu-id="c7516-147">Fare clic con il pulsante destro del mouse su **Windows PowerShell ISE** e scegliere **Esegui come amministratore**.</span><span class="sxs-lookup"><span data-stu-id="c7516-147">Right click **Windows PowerShell ISE** and click **Run as administrator**.</span></span>

1. <span data-ttu-id="c7516-148">Nella finestra Amministratore: Windows PowerShell ISE fare clic su **Visualizza** e quindi su **Mostra riquadro di script**.</span><span class="sxs-lookup"><span data-stu-id="c7516-148">In the Administrator: Windows PowerShell ISE window, click **View** and then click **Show Script Pane**.</span></span>

1. <span data-ttu-id="c7516-149">Incollare il testo copiato nel riquadro di script.</span><span class="sxs-lookup"><span data-stu-id="c7516-149">Paste the copied text into the script pane.</span></span>

1. <span data-ttu-id="c7516-150">Nel riquadro di script individuare il blocco di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c7516-150">In the script pane, locate the following block of code:</span></span>

    ```powershell
    [Parameter(Mandatory = $false,
            HelpMessage="Name of the AAD application that will be used to write secrets to KeyVault. A new application with this name will be created if one doesn't exist. If this app already exists, pass aadClientSecret parameter to the script")]
    [ValidateNotNullOrEmpty()]
    [string]$aadAppName,
    ```
1. <span data-ttu-id="c7516-151">Nel blocco di codice impostare `$false` su `$true`.</span><span class="sxs-lookup"><span data-stu-id="c7516-151">In the code block, change `$false` to `$true`.</span></span>

1. <span data-ttu-id="c7516-152">Fare clic su **File**, quindi su **Salva con nome** e passare alla cartella che si vuole usare per salvare lo script.</span><span class="sxs-lookup"><span data-stu-id="c7516-152">Click **File**, then click **Save As**, and navigate to the folder you'd like to use to save the script.</span></span>

1. <span data-ttu-id="c7516-153">Nella casella **Nome file** digitare **ADEPrereqScript.ps1** e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="c7516-153">In the **File name** box, type **ADEPrereqScript.ps1**, and click **Save**.</span></span>

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="c7516-154">Eseguire lo script di configurazione dei prerequisiti di Crittografia dischi di Azure</span><span class="sxs-lookup"><span data-stu-id="c7516-154">Run the Azure Disk Encryption Prerequisite Setup Script</span></span>

1. <span data-ttu-id="c7516-155">Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:</span><span class="sxs-lookup"><span data-stu-id="c7516-155">In the  PowerShell ISE console pane, type the following command, and press **ENTER**:</span></span>

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. <span data-ttu-id="c7516-156">Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:</span><span class="sxs-lookup"><span data-stu-id="c7516-156">In the PowerShell ISE console pane, type the following command, and press **ENTER**:</span></span>

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   <span data-ttu-id="c7516-157">Se viene visualizzata la finestra di dialogo **Modifica ai criteri di esecuzione** fare clic su **Sì per tutti** o su **Sì** (se non viene visualizzata l'opzione _Sì per tutti_).</span><span class="sxs-lookup"><span data-stu-id="c7516-157">If you get an **Execution Policy Change** dialog box, click either **Yes to all** or **Yes** (if you do not get a _Yes to all_ option).</span></span>

1. <span data-ttu-id="c7516-158">Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:</span><span class="sxs-lookup"><span data-stu-id="c7516-158">In the  PowerShell ISE console pane, type the following command, and press **ENTER**:</span></span>

   ```powershell
   Login-AzureRmAccount
   ```

1. <span data-ttu-id="c7516-159">Immettere le credenziali di Azure.</span><span class="sxs-lookup"><span data-stu-id="c7516-159">Enter your Azure credentials.</span></span>

1. <span data-ttu-id="c7516-160">Selezionare la stringa **SubscriptionId** e copiarla negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="c7516-160">Select your **SubscriptionId** string, and copy it to the clipboard.</span></span>

1. <span data-ttu-id="c7516-161">In PowerShell ISE fare clic su **File** e quindi su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="c7516-161">In the  PowerShell ISE, click **File**, and then click **Run**.</span></span>

1. <span data-ttu-id="c7516-162">Nel riquadro della console, al prompt **resourceGroupName:** digitare **moneyapprg** e premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="c7516-162">In the console pane, at the **resourceGroupName:** prompt, type  **moneyapprg**, and press **ENTER**.</span></span>

1. <span data-ttu-id="c7516-163">Nel riquadro della console, al prompt **keyVaultName:** digitare **moneyappkv** e premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="c7516-163">In the console pane, at the **keyVaultName:** prompt, type **moneyappkv**, and press **ENTER**.</span></span>

1. <span data-ttu-id="c7516-164">Nel riquadro della console, al prompt **location:** digitare la posizione usata durante la creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="c7516-164">In the console pane, at the **location:** prompt, type the location you used when creating your VM.</span></span>

1. <span data-ttu-id="c7516-165">Nel riquadro della console, al prompt **subscriptionId:** incollare l'ID della sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="c7516-165">In the console pane, at the **subscriptionId:** prompt, paste your subscription ID.</span></span>

1. <span data-ttu-id="c7516-166">Verrà ora creato l'insieme di credenziali delle chiavi **moneyappkv**.</span><span class="sxs-lookup"><span data-stu-id="c7516-166">The **moneyappkv** key vault will now be created.</span></span> <span data-ttu-id="c7516-167">Al termine, selezionare il testo di riepilogo (in verde) e copiarlo nel Blocco note.</span><span class="sxs-lookup"><span data-stu-id="c7516-167">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="c7516-168">Premere **INVIO** per continuare.</span><span class="sxs-lookup"><span data-stu-id="c7516-168">Press **ENTER** to continue.</span></span>

1. <span data-ttu-id="c7516-169">Nel riquadro della console, al prompt **aadAppName:** digitare **moneyapp** e premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="c7516-169">In the console pane, at the **aadAppName:**  prompt, type **moneyapp**, and press **ENTER**.</span></span>

1. <span data-ttu-id="c7516-170">Verrà ora creata l'applicazione di Azure AD **moneyapp**.</span><span class="sxs-lookup"><span data-stu-id="c7516-170">The **moneyapp** Azure AD application will now be created.</span></span> <span data-ttu-id="c7516-171">Al termine, selezionare il testo di riepilogo (in verde) e copiarlo nel Blocco note.</span><span class="sxs-lookup"><span data-stu-id="c7516-171">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="c7516-172">Premere **INVIO** per continuare.</span><span class="sxs-lookup"><span data-stu-id="c7516-172">Press **ENTER** to continue.</span></span>

### <a name="encrypt-your-vm-disks-with-powershell"></a><span data-ttu-id="c7516-173">Crittografare i dischi delle macchina virtuale con PowerShell</span><span class="sxs-lookup"><span data-stu-id="c7516-173">Encrypt your VM disks with PowerShell</span></span>

<span data-ttu-id="c7516-174">Verificare lo stato della crittografia dei dischi del sistema operativo e dei dati:</span><span class="sxs-lookup"><span data-stu-id="c7516-174">Verify the encryption status of the OS and data disks:</span></span>

1. <span data-ttu-id="c7516-175">Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c7516-175">In the PowerShell ISE console pane, type the following command, and press ENTER:</span></span>

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > <span data-ttu-id="c7516-176">Il nome della macchina virtuale deve essere racchiuso tra virgolette singole.</span><span class="sxs-lookup"><span data-stu-id="c7516-176">The VM name must be enclosed in single quotes.</span></span>

1. <span data-ttu-id="c7516-177">Nel riquadro di script di PowerShell ISE immettere il comando seguente e premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c7516-177">In the PowerShell ISE script pane, enter the following command, and press ENTER:</span></span>

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. <span data-ttu-id="c7516-178">Nella finestra di dialogo **Enable AzureDiskEncryption on the VM** (Abilita Crittografia dischi di Azure nella VM) fare clic su **Sì** e notare il messaggio indicante che il completamento della crittografia può richiedere 10-15 minuti.</span><span class="sxs-lookup"><span data-stu-id="c7516-178">In the **Enable AzureDiskEncryption on the VM** dialog box, click **Yes**, and note the message that encryption may take 10-15 minutes to complete.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="c7516-179">Attendere il completamento del comando prima di continuare con questo esercizio</span><span class="sxs-lookup"><span data-stu-id="c7516-179">Wait until the command has completed before continuing with this exercise</span></span>

### <a name="verify-the-encryption-status-of-your-vm-disks"></a><span data-ttu-id="c7516-180">Verificare lo stato della crittografia dei dischi delle macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="c7516-180">Verify the encryption status of your VM disks</span></span>

<span data-ttu-id="c7516-181">Passare al portale di Azure e nel pannello **Dischi** per **moneyappsvr01** si noti che lo stato della crittografia per i dischi del sistema operativo e dei dati ora è **Abilitato**.</span><span class="sxs-lookup"><span data-stu-id="c7516-181">Switch to the Azure portal, and on the **Disks** blade for **moneyappsvr01**, note that the disk encryption status for the OS and Data disks is now **Enabled**.</span></span>
