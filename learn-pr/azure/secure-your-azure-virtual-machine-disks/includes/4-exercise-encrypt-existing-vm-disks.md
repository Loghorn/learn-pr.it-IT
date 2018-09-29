<span data-ttu-id="80420-101">Si supponga di sviluppare un'applicazione di gestione finanziaria per nuove startup.</span><span class="sxs-lookup"><span data-stu-id="80420-101">Suppose you are developing a financial management application for new business startups.</span></span> <span data-ttu-id="80420-102">Poiché è necessario assicurarsi che tutti i dati dei clienti siano protetti, si è deciso di implementare Crittografia dischi di Azure in tutti i dischi del sistema operativo e di dati sui server che ospiteranno l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="80420-102">You want to ensure that all your customers' data is secured, so you have decided to implement Azure Disk Encryption (ADE) across all OS and data disks on the servers that will host this application.</span></span> <span data-ttu-id="80420-103">Per soddisfare i requisiti di conformità, è anche necessario essere responsabili della gestione delle chiavi di crittografia.</span><span class="sxs-lookup"><span data-stu-id="80420-103">As part of your compliance requirements, you also need to be responsible for your own encryption key management.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="80420-104">In questa unità si crittograferanno i dischi di una macchina virtuale esistente e si gestiranno le chiavi di crittografia usando Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="80420-104">In this unit, you'll encrypt disks on an existing virtual machine, and manage the encryption keys using your own Azure Key Vault.</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="80420-105">Preparare l'ambiente</span><span class="sxs-lookup"><span data-stu-id="80420-105">Prepare the environment</span></span>

<span data-ttu-id="80420-106">Si inizierà distribuendo una nuova macchina virtuale Windows in una macchina virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="80420-106">We'll start by deploying a new Windows VM in an Azure Virtual Machine.</span></span>

### <a name="deploy-windows-vm"></a><span data-ttu-id="80420-107">Distribuzione di una macchina virtuale Windows</span><span class="sxs-lookup"><span data-stu-id="80420-107">Deploy Windows VM</span></span>

<span data-ttu-id="80420-108">Utilizzare Powershell di Microsoft Azure per creare e distribuire una nuova macchina virtuale Windows.</span><span class="sxs-lookup"><span data-stu-id="80420-108">Use the Azure PowerShell to create and deploy a new Windows virtual machine.</span></span>

1. <span data-ttu-id="80420-109">Per iniziare, decidere dove posizionare le nuove risorse.</span><span class="sxs-lookup"><span data-stu-id="80420-109">Start by deciding where to place the new resources.</span></span> <span data-ttu-id="80420-110">Selezionare una località vicina dall'elenco seguente.</span><span class="sxs-lookup"><span data-stu-id="80420-110">Select a location near you from the following list.</span></span>

    <span data-ttu-id="80420-111"><!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]</span><span class="sxs-lookup"><span data-stu-id="80420-111"><!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]</span></span>
    

1. <span data-ttu-id="80420-112">Definire una variabile di PowerShell per contenere la località selezionata.</span><span class="sxs-lookup"><span data-stu-id="80420-112">Define a PowerShell variable to hold the selected location.</span></span> <span data-ttu-id="80420-113">In questo caso la località è "Stati Uniti orientali", modificarla scegliendo la località preferita.</span><span class="sxs-lookup"><span data-stu-id="80420-113">It's defined as "East US" here, change it to your preferred location.</span></span>

    ```powershell
    $location = "eastus"
    ```
    
    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. <span data-ttu-id="80420-114">Successivamente, definire altre comode variabili per acquisire il _nome_ della macchina virtuale e il _gruppo di risorse_.</span><span class="sxs-lookup"><span data-stu-id="80420-114">Next, define a few more convenient variables to capture the _name_ of the VM and the _resource group_.</span></span> <span data-ttu-id="80420-115">Si noti che in questo caso si sta usando il gruppo di risorse creato in precedenza mentre, in genere, si crea un _nuovo_ gruppo di risorse nella sottoscrizione usando `New-AzureRmResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="80420-115">Note that we are using the pre-created resource group here, normally you would create a _new_ resource group in your subscription using `New-AzureRmResourceGroup`.</span></span>

    ```powershell
    $vmName = "fmdata-vm01"
    $rgName = "<rgn>[sandbox Resource Group]</rgn>"
    ```
    
1. <span data-ttu-id="80420-116">Usare `New-AzureRmVm` per creare una nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="80420-116">Use `New-AzureRmVm` to create a new virtual machine.</span></span>
    
    ```powershell
    New-AzureRmVm `
        -ResourceGroupName $rgName `
        -Name $vmName `
        -Location $location `
        -OpenPorts 3389
    ```
    
    <span data-ttu-id="80420-117">Immettere un nome utente e una password per la macchina virtuale quando Cloud Shell lo richiede.</span><span class="sxs-lookup"><span data-stu-id="80420-117">Enter a username and password for the VM when you are prompted by the Cloud Shell.</span></span> <span data-ttu-id="80420-118">Questo verrà utilizzato come l'account iniziale creato per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="80420-118">This will be used as the initial account created for the VM.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="80420-119">Questo comando userà alcune impostazioni predefinite perché alcune opzioni non sono state definite.</span><span class="sxs-lookup"><span data-stu-id="80420-119">This command will use some defaults since we didn't supply a bunch of options.</span></span> <span data-ttu-id="80420-120">In particolare, verrà creata un'immagine _Windows 2016 Server_ con dimensione _Standard_DS1_v2_.</span><span class="sxs-lookup"><span data-stu-id="80420-120">Specifically, this will create a _Windows 2016 Server_ image with the size to _Standard_DS1_v2_.</span></span> <span data-ttu-id="80420-121">Tenere presente che le macchine virtuali di livello Basic non supportano ADE se si decide di specificare le dimensioni della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="80420-121">Remember that the Basic tier VMs do not support ADE if you decide to specify the VM size.</span></span>

1. <span data-ttu-id="80420-122">Una volta completata la distribuzione della macchina virtuale, acquisire i dettagli della macchina virtuale in una variabile.</span><span class="sxs-lookup"><span data-stu-id="80420-122">Once the VM finishes deploying, capture the VM details in a variable.</span></span> <span data-ttu-id="80420-123">Questa variabile consente di esaminare ciò che è stato creato.</span><span class="sxs-lookup"><span data-stu-id="80420-123">You can use this variable to explore what was created.</span></span>

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```
    
1. <span data-ttu-id="80420-124">È possibile visualizzare il disco del sistema operativo collegato alla macchina virtuale:</span><span class="sxs-lookup"><span data-stu-id="80420-124">You can see the OS disk attached to the VM:</span></span>

    ```powershell
    $vm.StorageProfile.OSDisk
    ```

    ```output
    OsType                  : Windows
    EncryptionSettings      :
    Name                    : fmdata-vm01_OsDisk_1_6bcf8dcd49794aa785bad45221ec4433
    Vhd                     :
    Image                   :
    Caching                 : ReadWrite
    WriteAcceleratorEnabled :
    CreateOption            : FromImage
    DiskSizeGB              : 127
    ManagedDisk             : Microsoft.Azure.Management.Compute.Models.ManagedDiskP
                              arameters
    ```
        
1. <span data-ttu-id="80420-125">Controllare lo stato corrente della crittografia del disco del sistema operativo e di eventuali dischi dati.</span><span class="sxs-lookup"><span data-stu-id="80420-125">Check the current status of encryption on the OS disk (and any data disks).</span></span>

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  `
        -ResourceGroupName $rgName `
        -VMName $vmName
    ```

    <span data-ttu-id="80420-126">Come si può notare, al momento i dischi _non sono crittografati_.</span><span class="sxs-lookup"><span data-stu-id="80420-126">As you can see the disks are current _unencrypted_.</span></span> 

    ```output
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : NotEncrypted
    OsVolumeEncryptionSettings :
    ProgressMessage            : No Encryption extension or metadata found on the VM
    ```
    
<span data-ttu-id="80420-127">Seguire questa procedura per intervenire.</span><span class="sxs-lookup"><span data-stu-id="80420-127">Let's change that.</span></span>
    
## <a name="encrypt-the-vm-disks-with-azure-disk-encryption"></a><span data-ttu-id="80420-128">Crittografare i dischi della macchina virtuale con Crittografia dischi di Azure</span><span class="sxs-lookup"><span data-stu-id="80420-128">Encrypt the VM disks with Azure Disk Encryption</span></span>

<span data-ttu-id="80420-129">Per proteggere questi dati è necessario crittografare i dischi.</span><span class="sxs-lookup"><span data-stu-id="80420-129">We need to protect this data, so let's encrypt the disks.</span></span> <span data-ttu-id="80420-130">Tenere presente che è necessario eseguire diverse operazioni:</span><span class="sxs-lookup"><span data-stu-id="80420-130">Recall that there are several steps we need to perform:</span></span>

1. <span data-ttu-id="80420-131">Creare un insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="80420-131">Create a key vault.</span></span>
1. <span data-ttu-id="80420-132">Configurare l'insieme di credenziali delle chiavi per il supporto della crittografia dei dischi.</span><span class="sxs-lookup"><span data-stu-id="80420-132">Set the key vault up to support disk encryption.</span></span>
1. <span data-ttu-id="80420-133">Indicare ad Azure di crittografare i dischi della macchina virtuale con la chiave archiviata nell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="80420-133">Tell Azure to encrypt the VM disks using the key stored in the Key Vault.</span></span>

> [!TIP]
> <span data-ttu-id="80420-134">Ora ogni passaggio verrà spiegato singolarmente, ma quando si esegue questa operazione nella propria sottoscrizione, è possibile usare un comodo script di PowerShell indicato come collegamento nel riepilogo di questo modulo.</span><span class="sxs-lookup"><span data-stu-id="80420-134">We're going to walk through the steps individually, but when you're doing this task in your own subscription, you can use a handy PowerShell script which is linked in the Summary of this module.</span></span>

### <a name="create-a-key-vault"></a><span data-ttu-id="80420-135">Creare un insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="80420-135">Create a key vault</span></span>

<span data-ttu-id="80420-136">Per creare un Azure Key Vault, è necessario abilitare il servizio nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="80420-136">To create an Azure Key Vault, we need to enable the service in our subscription.</span></span> <span data-ttu-id="80420-137">Si tratta di un'esigenza una tantum.</span><span class="sxs-lookup"><span data-stu-id="80420-137">This is a one-time requirement.</span></span>

> [!TIP]
> <span data-ttu-id="80420-138">A seconda della sottoscrizione, potrebbe essere necessario abilitare il provider **Microsoft.KeyVault** con il cmdlet `Register-AzureRmResourceProvider`.</span><span class="sxs-lookup"><span data-stu-id="80420-138">Depending on your subscription, you might need to enable the **Microsoft.KeyVault** provider with the `Register-AzureRmResourceProvider` cmdlet.</span></span> <span data-ttu-id="80420-139">Questa operazione non è necessaria nella sottoscrizione dell'ambiente sandbox di Azure.</span><span class="sxs-lookup"><span data-stu-id="80420-139">This is not necessary in the Azure sandbox subscription.</span></span>

1. <span data-ttu-id="80420-140">Specificare un nome per il nuovo insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="80420-140">Decide on a name for your new key vault.</span></span> <span data-ttu-id="80420-141">Deve essere univoco e contenere da 3 a 24 caratteri, che possono includere numeri, lettere e trattini.</span><span class="sxs-lookup"><span data-stu-id="80420-141">It must be unique and can be between 3 and 24 characters, composed of numbers, letters, and and dashes.</span></span> <span data-ttu-id="80420-142">Provare ad aggiungere alcuni numeri casuali fino alla fine, sostituendo la stringa "1234" riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="80420-142">Try adding some random numbers to the end, replacing the "1234" below.</span></span>

    ```powershell
    $keyVaultName = "mvmdsk-kv-1234"
    ```
        
1. <span data-ttu-id="80420-143">Creare un Azure Key Vault con `New-AzureRmKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="80420-143">Create an Azure Key Vault with `New-AzureRmKeyVault`.</span></span>
    - <span data-ttu-id="80420-144">Assicurarsi di posizionarlo nello stesso gruppo di risorse _e_ nella stessa località della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="80420-144">Make sure it's placed in the same resource group _and_ location as your VM.</span></span>
    - <span data-ttu-id="80420-145">Abilitare l'insieme di credenziali delle chiavi per l'uso con la crittografia del disco.</span><span class="sxs-lookup"><span data-stu-id="80420-145">Enable the Key Vault for use with disk encryption.</span></span> 
    - <span data-ttu-id="80420-146">Specificare un nome univoco per l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="80420-146">Specify a unique Key Vault name.</span></span>

    ```powershell
    New-AzureRmKeyVault -VaultName $keyVaultName `
        -Location $location `
        -ResourceGroupName $rgName `
        -EnabledForDiskEncryption
    ```

    <span data-ttu-id="80420-147">Verrà visualizzato un avviso da questo comando perché nessun utente dispone di accesso.</span><span class="sxs-lookup"><span data-stu-id="80420-147">You will get a warning from this command about no users having access.</span></span>

    ```output
    WARNING: Access policy is not set. No user or application have access permission to use this vault. This can happen if the vault was created by a service principal. Please use Set-AzureRmKeyVaultAccessPolicy to set access policies.
    ```

    <span data-ttu-id="80420-148">Questa situazione è accettabile perché l'insieme di credenziali verrà utilizzato solo per archiviare le chiavi di crittografia per la macchina virtuale e gli utenti non avranno bisogno di accedere a questi dati.</span><span class="sxs-lookup"><span data-stu-id="80420-148">This is ok since we are just using the vault to store the encryption keys for the VM and users won't need to access this data.</span></span>

### <a name="encrypt-the-disk"></a><span data-ttu-id="80420-149">Crittografare il disco</span><span class="sxs-lookup"><span data-stu-id="80420-149">Encrypt the disk</span></span>

<span data-ttu-id="80420-150">Siamo quasi pronti per crittografare i dischi.</span><span class="sxs-lookup"><span data-stu-id="80420-150">We are almost ready to encrypt the disks.</span></span> <span data-ttu-id="80420-151">Prima di farlo, considerare questo avviso relativo alla creazione di copie di backup.</span><span class="sxs-lookup"><span data-stu-id="80420-151">Before we do, a warning about creating backups.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80420-152">Se questo fosse un sistema di produzione, sarebbe necessario eseguire una copia di backup dei dischi gestiti usando Backup di Azure o creando uno snapshot.</span><span class="sxs-lookup"><span data-stu-id="80420-152">If this were a production system, we would need to perform a backup of the managed disks - either using Azure Backup, or by creating a snapshot.</span></span> <span data-ttu-id="80420-153">È possibile creare gli snapshot nel portale di Azure o tramite la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="80420-153">You can create snapshots in the Azure portal, or through the command line.</span></span> <span data-ttu-id="80420-154">In PowerShell usare il cmdlet `New-AzureRmSnapshot`.</span><span class="sxs-lookup"><span data-stu-id="80420-154">In PowerShell, use the `New-AzureRmSnapshot` cmdlet.</span></span> <span data-ttu-id="80420-155">Poiché questo è un semplice esercizio i cui dati alla fine verranno eliminati, è possibile ignorare questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="80420-155">Since this is a simple exercise and we're going to throw this data away when you're done, we're going to skip this step.</span></span> 

1. <span data-ttu-id="80420-156">Iniziare definendo una variabile per contenere le informazioni dell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="80420-156">Start by defining a variable to hold the Key Vault information.</span></span>

    ```powershell
    $keyVault = Get-AzureRmKeyVault `
        -VaultName $keyVaultName `
        -ResourceGroupName $rgName
    ```

1. <span data-ttu-id="80420-157">Quindi, usare il cmdlet `Set-AzureRmVmDiskEncryptionExtension` per crittografare i dischi della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="80420-157">Then, use the `Set-AzureRmVmDiskEncryptionExtension` cmdlet to encrypt the VM disks.</span></span>
    - <span data-ttu-id="80420-158">Il parametro `VolumeType` consente di specificare quali dischi crittografare: [_Tutti_ | _SO_ | _Dati_].</span><span class="sxs-lookup"><span data-stu-id="80420-158">The `VolumeType` parameter allows you to specify which disks to encrypt: [_All_ | _OS_ | _Data_].</span></span> <span data-ttu-id="80420-159">L'impostazione predefinita è _Tutti_.</span><span class="sxs-lookup"><span data-stu-id="80420-159">It will default to _All_.</span></span> <span data-ttu-id="80420-160">È possibile crittografare solo i dischi dati di alcune distribuzioni di Linux.</span><span class="sxs-lookup"><span data-stu-id="80420-160">You can only encrypt data disks for some distributions of Linux.</span></span>
    - <span data-ttu-id="80420-161">È necessario fornire il flag `SkipVmBackup` per i dischi gestiti altrimenti il comando avrà esito negativo perché non c'è uno snapshot.</span><span class="sxs-lookup"><span data-stu-id="80420-161">You have to supply the `SkipVmBackup` flag for managed disks or the command will fail because there is no snapshot.</span></span>

    ```powershell
    Set-AzureRmVmDiskEncryptionExtension `
        -ResourceGroupName $rgName `
        -VMName $vmName `
        -VolumeType All `
        -DiskEncryptionKeyVaultId $keyVault.ResourceId `
        -DiskEncryptionKeyVaultUrl $keyVault.VaultUri `
        -SkipVmBackup
    ```

1. <span data-ttu-id="80420-162">Il cmdlet avviserà l'utente che la macchina virtuale deve essere portata offline e che l'attività potrebbe richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="80420-162">The cmdlet will warn you that the VM must be taken offline, and that the task may take several minutes to complete.</span></span> <span data-ttu-id="80420-163">Lasciar proseguire l'operazione.</span><span class="sxs-lookup"><span data-stu-id="80420-163">Go ahead and let it continue.</span></span>

    ```output
    Enable AzureDiskEncryption on the VM
    This cmdlet prepares the VM and enables encryption which may reboot the machine and takes 10-15 minutes to
    finish. Please save your work on the VM before confirming. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    ```
    
1. <span data-ttu-id="80420-164">Una volta completata, controllare di nuovo lo stato della crittografia.</span><span class="sxs-lookup"><span data-stu-id="80420-164">Once it's complete, check the encryption status again.</span></span>

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
    ```

    <span data-ttu-id="80420-165">Ora il disco del sistema operativo è crittografato.</span><span class="sxs-lookup"><span data-stu-id="80420-165">Now the OS disk should be encrypted.</span></span> <span data-ttu-id="80420-166">Eventuali dischi dati collegati visibili per Windows saranno ugualmente crittografati.</span><span class="sxs-lookup"><span data-stu-id="80420-166">Any attached data disks that are visible to Windows will also be encrypted.</span></span>

    ```output
    OsVolumeEncrypted          : Encrypted
    DataVolumesEncrypted       : NoDiskFound
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : Provisioning succeeded
    ```

> [!NOTE]        
> <span data-ttu-id="80420-167">I nuovi dischi aggiunti dopo la crittografia _non_ verranno automaticamente crittografati.</span><span class="sxs-lookup"><span data-stu-id="80420-167">New disks added after encryption will _not_ be automatically encrypted.</span></span> <span data-ttu-id="80420-168">Eseguire nuovamente il cmdlet `Set-AzureRmVMDiskEncryptionExtension` per crittografare i nuovi dischi.</span><span class="sxs-lookup"><span data-stu-id="80420-168">You can re-run the `Set-AzureRmVMDiskEncryptionExtension` cmdlet to encrypt new disks.</span></span> <span data-ttu-id="80420-169">Se si aggiungono dischi a una macchina virtuale i cui dischi sono già stati crittografati, assicurarsi di specificare un nuovo numero di sequenza.</span><span class="sxs-lookup"><span data-stu-id="80420-169">Make sure to provide a new sequence number if you add disks to a VM that has already had disks encrypted.</span></span> <span data-ttu-id="80420-170">Inoltre, i dischi che non sono visibili per il sistema operativo non verranno crittografati. Per poter essere visto dall'estensione Bitlocker, il disco deve essere correttamente partizionato, formattato e montato.</span><span class="sxs-lookup"><span data-stu-id="80420-170">In addition, disks that are not visible to the operating system will not be encrypted - the disk must be properly partitioned, formatted, and mounted to be seen by the Bitlocker extension.</span></span>