<span data-ttu-id="e3c48-101">Si supponga che l'azienda abbia deciso di implementare Crittografia dischi di Azure in tutte le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="e3c48-101">Suppose your company has decided to implement Azure Disk Encryption (ADE) across all VMs.</span></span> <span data-ttu-id="e3c48-102">È necessario valutare come implementare la crittografia nei volumi delle macchine virtuali esistenti.</span><span class="sxs-lookup"><span data-stu-id="e3c48-102">You need to evaluate how to roll out encryption to existing VM volumes.</span></span> <span data-ttu-id="e3c48-103">In questo modulo si esamineranno i requisiti per Crittografia dischi di Azure e i passaggi necessari per la crittografia dei dischi nelle macchine virtuali Linux e Windows esistenti.</span><span class="sxs-lookup"><span data-stu-id="e3c48-103">Here, we'll look at the requirements for ADE, and the steps involved in encrypting disks on existing Linux and Windows VMs.</span></span>

## <a name="azure-disk-encryption-prerequisites"></a><span data-ttu-id="e3c48-104">Prerequisiti di Crittografia dischi di Azure</span><span class="sxs-lookup"><span data-stu-id="e3c48-104">Azure Disk Encryption prerequisites</span></span>

<span data-ttu-id="e3c48-105">Per poter crittografare i dischi di una macchina virtuale è necessario:</span><span class="sxs-lookup"><span data-stu-id="e3c48-105">Before you can encrypt your VM disks, you need to:</span></span>

1. <span data-ttu-id="e3c48-106">Creare un insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-106">Create a key vault.</span></span>
1. <span data-ttu-id="e3c48-107">Impostare criteri di accesso dell'insieme di credenziali delle chiavi per supportare la crittografia dei dischi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-107">Set the key vault access policy to support disk encryption.</span></span>
1. <span data-ttu-id="e3c48-108">Usare l'insieme di credenziali delle chiavi per archiviare le chiavi di crittografia per Crittografia dischi di Azure.</span><span class="sxs-lookup"><span data-stu-id="e3c48-108">Use the key vault to store the encryption keys for ADE.</span></span>

### <a name="azure-key-vault"></a><span data-ttu-id="e3c48-109">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e3c48-109">Azure Key Vault</span></span>

<span data-ttu-id="e3c48-110">Le chiavi di crittografia usate da Crittografia dischi di Azure possono essere archiviate in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="e3c48-110">The encryption keys used by ADE can be stored in Azure Key Vault.</span></span> <span data-ttu-id="e3c48-111">Azure Key Vault è uno strumento che consente di archiviare i segreti e di accedervi in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="e3c48-111">Azure Key Vault is a tool for securely storing and accessing secrets.</span></span> <span data-ttu-id="e3c48-112">Un segreto è qualsiasi elemento per cui si vuole controllare rigorosamente l'accesso, ad esempio chiavi API, password o certificati.</span><span class="sxs-lookup"><span data-stu-id="e3c48-112">A secret is anything that you want to tightly control access to, such as API keys, passwords, or certificates.</span></span> <span data-ttu-id="e3c48-113">Questa soluzione garantisce archiviazione sicura, scalabile e a disponibilità elevata in moduli di protezione hardware conformi a FIPS (Federal Information Processing Standards) 140-2, livello 2.</span><span class="sxs-lookup"><span data-stu-id="e3c48-113">This provides highly available and scalable secure storage, as defined in Federal Information Processing Standards (FIPS) 140-2 Level 2 validated Hardware Security Modules (HSMs).</span></span> <span data-ttu-id="e3c48-114">Con Azure Key Vault è possibile avere il controllo completo delle chiavi usate per crittografare i dati e gestire e controllare l'uso delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-114">Using Key Vault, you keep full control of the keys used to encrypt your data, and you can manage and audit your key usage.</span></span> 

> [!NOTE]
> <span data-ttu-id="e3c48-115">Per Crittografia dischi di Azure è necessario che l'istanza dell'insieme di credenziali delle chiavi e le macchine virtuali si trovino nella stessa area di Azure, in modo che i segreti di crittografia non debbano attraversare confini a livello di area.</span><span class="sxs-lookup"><span data-stu-id="e3c48-115">Azure Disk Encryption requires that your key vault and your VMs are in the same Azure region; this ensures that encryption secrets do not cross regional boundaries.</span></span>

<span data-ttu-id="e3c48-116">È possibile configurare e gestire l'insieme di credenziali delle chiavi con:</span><span class="sxs-lookup"><span data-stu-id="e3c48-116">You can configure and manage your key vault with:</span></span>

#### <a name="powershell"></a><span data-ttu-id="e3c48-117">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e3c48-117">Powershell</span></span>

```powershell
New-AzureRmKeyVault -Location <location> `
    -ResourceGroupName <resource-group> `
    -VaultName "myKeyVault" `
    -EnabledForDiskEncryption
```

### <a name="azure-cli"></a><span data-ttu-id="e3c48-118">Interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="e3c48-118">Azure CLI</span></span>

```azurecli
az keyvault create \
    --name "myKeyVault" \
    --resource-group <resource-group> \
    --location <location> \
    --enabled-for-disk-encryption True
```

### <a name="azure-portal"></a><span data-ttu-id="e3c48-119">Portale di Azure</span><span class="sxs-lookup"><span data-stu-id="e3c48-119">Azure portal</span></span>

<span data-ttu-id="e3c48-120">Azure Key Vault è una risorsa che può essere creata nel portale di Azure usando il normale processo di creazione delle risorse.</span><span class="sxs-lookup"><span data-stu-id="e3c48-120">An Azure Key Vault is a resource that can be created in the Azure portal using the normal resource creation process.</span></span>

1. <span data-ttu-id="e3c48-121">Fare clic su **Creare una risorsa** nella barra laterale a sinistra.</span><span class="sxs-lookup"><span data-stu-id="e3c48-121">Click **Create a resource** in the sidebar on the left.</span></span>

1. <span data-ttu-id="e3c48-122">Cercare "insieme di credenziali delle chiavi".</span><span class="sxs-lookup"><span data-stu-id="e3c48-122">Search for "Key vault".</span></span> <span data-ttu-id="e3c48-123">Fare clic su **Crea** nella finestra dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="e3c48-123">Click **Create** in the details window.</span></span>

    ![Screenshot che mostra Azure Key Vault in Azure Marketplace](../media/3-create-keyvault.png)

1. <span data-ttu-id="e3c48-125">Immettere i dettagli per il nuovo insieme di credenziali delle chiavi:</span><span class="sxs-lookup"><span data-stu-id="e3c48-125">Enter the details for the new Key Vault:</span></span>
    - <span data-ttu-id="e3c48-126">Immettere un **nome** per l'insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="e3c48-126">Enter a **Name** for the Key Vault</span></span>
    - <span data-ttu-id="e3c48-127">Selezionare la sottoscrizione in cui inserirlo (la sottoscrizione corrente è l'impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="e3c48-127">Select the subscription to place it in (defaults to your current subscription).</span></span>
    - <span data-ttu-id="e3c48-128">Selezionare un **gruppo di risorse** o creare un nuovo gruppo proprietario dell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-128">Select a **Resource Group**, or create a new resource group to own the Key Vault.</span></span>
    - <span data-ttu-id="e3c48-129">Selezionare una **posizione** per l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-129">Select a **Location** for the Key Vault.</span></span> <span data-ttu-id="e3c48-130">Assicurarsi di selezionare la posizione in cui si trova la VM.</span><span class="sxs-lookup"><span data-stu-id="e3c48-130">Make sure to select the location the VM is in.</span></span>
    - <span data-ttu-id="e3c48-131">È possibile scegliere un piano tariffario Standard o Premium.</span><span class="sxs-lookup"><span data-stu-id="e3c48-131">You can choose either Standard or Premium for the pricing tier.</span></span> <span data-ttu-id="e3c48-132">La differenza principale è che il livello Premium consente chiavi supportate dalla crittografia hardware.</span><span class="sxs-lookup"><span data-stu-id="e3c48-132">The main difference is that the premium tier allows for Hardware-encryption backed keys.</span></span>

1. <span data-ttu-id="e3c48-133">È necessario modificare i criteri di accesso per supportare la crittografia dei dischi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-133">You must change the Access policies to support Disk Encryption.</span></span> <span data-ttu-id="e3c48-134">Per impostazione predefinita viene aggiunto l'account _personale_ ai criteri.</span><span class="sxs-lookup"><span data-stu-id="e3c48-134">By default it adds _your_ account to the policy.</span></span>
    - <span data-ttu-id="e3c48-135">Selezionare **Criteri di accesso**</span><span class="sxs-lookup"><span data-stu-id="e3c48-135">Select **Access policies**</span></span>
    - <span data-ttu-id="e3c48-136">Fare clic su **Criteri di accesso avanzati**.</span><span class="sxs-lookup"><span data-stu-id="e3c48-136">Click **Advanced access policies**.</span></span>
    - <span data-ttu-id="e3c48-137">Selezionare la casella **Abilita l'accesso a Crittografia dischi di Azure per la crittografia dei volumi**.</span><span class="sxs-lookup"><span data-stu-id="e3c48-137">Check the **Enable access to Azure Disk Encryption for volume encryption**.</span></span>
    - <span data-ttu-id="e3c48-138">È possibile rimuovere l'account, non è necessario se si intende usare l'insieme di credenziali delle chiavi solo per la crittografia dei dischi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-138">You can remove your account if you like, it's not necessary if you only intend to use the Key Vault for disk encryption.</span></span>
    - <span data-ttu-id="e3c48-139">Fare clic su **OK** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e3c48-139">Click **OK** to save the changes.</span></span>

    ![Screenshot che mostra le proprietà avanzate per Azure Key Vault con l'opzione Crittografia dischi di Azure selezionata ed evidenziata](../media/3-configure-access-policy.png)

1. <span data-ttu-id="e3c48-141">Fare clic su **Crea** per creare il nuovo insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-141">Click **Create** to create the new Key Vault.</span></span>

## <a name="enabling-access-policies-in-the-key-vault"></a><span data-ttu-id="e3c48-142">Abilitazione dei criteri di accesso nell'insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="e3c48-142">Enabling access policies in the key vault</span></span>
<span data-ttu-id="e3c48-143">Azure deve avere accesso alle chiavi di crittografia o ai segreti nell'insieme di credenziali delle chiavi per renderli disponibili alla macchina virtuale per l'avvio e la decrittografia dei volumi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-143">Azure needs access to the encryption keys or secrets in your key vault to make them available to the VM for booting and decrypting the volumes.</span></span> <span data-ttu-id="e3c48-144">Questo argomento è stato affrontato per il portale quando sono stati modificati i **criteri di accesso avanzati** sopra.</span><span class="sxs-lookup"><span data-stu-id="e3c48-144">We covered this for the portal when we changed the **Advanced access policies** above.</span></span>

<span data-ttu-id="e3c48-145">È possibile abilitare tre criteri.</span><span class="sxs-lookup"><span data-stu-id="e3c48-145">There are three policies you can enable.</span></span>
1. <span data-ttu-id="e3c48-146">**Crittografia del disco**: obbligatorio per Crittografia dischi di Azure.</span><span class="sxs-lookup"><span data-stu-id="e3c48-146">**Disk encryption** - Required for Azure Disk encryption.</span></span>
1. <span data-ttu-id="e3c48-147">**Distribuzione** (facoltativo): in questo modo si consente al provider di risorse Microsoft.Compute di recuperare segreti da questo insieme di credenziali delle chiavi quando vi viene fatto riferimento durante la creazione di risorse, ad esempio quando si crea una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="e3c48-147">**Deployment** - (Optional) Enables the Microsoft.Compute resource provider to retrieve secrets from this key vault when this key vault is referenced in resource creation, for example when creating a virtual machine.</span></span>
1. <span data-ttu-id="e3c48-148">**Distribuzione modelli** (facoltativo): in questo modo si consente ad Azure Resource Manager di recuperare segreti da questo insieme di credenziali delle chiavi quando vi viene fatto riferimento durante la distribuzione di un modello.</span><span class="sxs-lookup"><span data-stu-id="e3c48-148">**Template deployment** - (Optional) Enables Azure Resource Manager to get secrets from this key vault when this key vault is referenced in a template deployment.</span></span>

<span data-ttu-id="e3c48-149">Di seguito viene illustrato come abilitare i criteri di crittografia del disco.</span><span class="sxs-lookup"><span data-stu-id="e3c48-149">Here's how to enable the disk encryption policy.</span></span> <span data-ttu-id="e3c48-150">Gli altri due sono simili, ma usano flag diversi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-150">The other two are similar but use different flags.</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <keyvault-name> -ResourceGroupName <resource-group> -EnabledForDiskEncryption
```

```azurecli
az keyvault update --name <keyvault-name> --resource-group <resource-group> --enabled-for-disk-encryption "true"
```

## <a name="encrypting-an-existing-vm-disk"></a><span data-ttu-id="e3c48-151">Crittografia di un disco di una macchina virtuale esistente</span><span class="sxs-lookup"><span data-stu-id="e3c48-151">Encrypting an existing VM disk</span></span>

<span data-ttu-id="e3c48-152">Una volta configurato l'insieme di credenziali delle chiavi, sarà possibile crittografare la macchina virtuale usando l'interfaccia della riga di comando di Azure o Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e3c48-152">Once you have the Key Vault setup, you can encrypt the VM using either Azure CLI or Azure PowerShell.</span></span> <span data-ttu-id="e3c48-153">La prima volta che si crittografa una macchina virtuale Windows, è possibile scegliere di crittografare tutti i dischi o solo il disco del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="e3c48-153">The first time you encrypt a Windows VM, you can choose to encrypt either all disks or the OS disk only.</span></span> <span data-ttu-id="e3c48-154">In alcune distribuzioni Linux è possibile crittografare solo i dischi dati.</span><span class="sxs-lookup"><span data-stu-id="e3c48-154">On some Linux distributions, only the data disks may be encrypted.</span></span> <span data-ttu-id="e3c48-155">Per essere idonei per la crittografia, i dischi di Windows devono essere formattati come volumi NTFS.</span><span class="sxs-lookup"><span data-stu-id="e3c48-155">To be eligible for encryption, your Windows disks must be formatted as NTFS volumes.</span></span>

> [!WARNING]
> <span data-ttu-id="e3c48-156">Per attivare la crittografia, è necessario eseguire uno snapshot o un backup dei dischi gestiti.</span><span class="sxs-lookup"><span data-stu-id="e3c48-156">You must take a snapshot or a backup of managed disks before you can turn on encryption.</span></span> <span data-ttu-id="e3c48-157">Il flag `SkipVmBackup` specificato di seguito indica allo strumento che il backup è stato completato nei dischi gestiti.</span><span class="sxs-lookup"><span data-stu-id="e3c48-157">The `SkipVmBackup` flag specified below tells the tool that the backup is complete on managed disks.</span></span> <span data-ttu-id="e3c48-158">Senza il backup, non sarà possibile ripristinare la macchina virtuale se la crittografia ha esito negativo per qualche motivo.</span><span class="sxs-lookup"><span data-stu-id="e3c48-158">Without the backup, you will be unable to recover the VM if the encryption fails for some reason.</span></span>

<span data-ttu-id="e3c48-159">In PowerShell abilitare la crittografia con il cmdlet `Set-AzureRmVmDiskEncryptionExtension`.</span><span class="sxs-lookup"><span data-stu-id="e3c48-159">With PowerShell, use the `Set-AzureRmVmDiskEncryptionExtension` cmdlet to enable encryption.</span></span>

```powershell

Set-AzureRmVmDiskEncryptionExtension `
    -ResourceGroupName <resource-group> `
    -VMName <vm-name> `
    -VolumeType [All | OS | Data]
    -DiskEncryptionKeyVaultId <keyVault.ResourceId> `
    -DiskEncryptionKeyVaultUrl <keyVault.VaultUri> `
     -SkipVmBackup
```

<span data-ttu-id="e3c48-160">Nell'interfaccia della riga di comando di Azure abilitare la crittografia con il comando `az vm encryption enable`.</span><span class="sxs-lookup"><span data-stu-id="e3c48-160">For the Azure CLI, use the `az vm encryption enable` command to enable encryption.</span></span>

```azurecli
az vm encryption enable \
    --resource-group <resource-group> \
    --name <vm-name> \
    --disk-encryption-keyvault <keyvault-name> \
    --volume-type [all | os | data] \
    --skipvmbackup
```

## <a name="viewing-the-status-of-the-disk"></a><span data-ttu-id="e3c48-161">Visualizzazione dello stato del disco</span><span class="sxs-lookup"><span data-stu-id="e3c48-161">Viewing the status of the disk</span></span>

<span data-ttu-id="e3c48-162">È possibile controllare se dischi specifici sono crittografati o meno.</span><span class="sxs-lookup"><span data-stu-id="e3c48-162">You can check whether specific disks are encrypted or not.</span></span>

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName <resource-group> -VMName <vm-name>
```

```azurecli
az vm encryption status --resource-group <resource-group> --name <vm-name>
```

<span data-ttu-id="e3c48-163">Entrambi questi comandi restituiranno lo stato di ogni disco collegato alla macchina virtuale specificata.</span><span class="sxs-lookup"><span data-stu-id="e3c48-163">Both of these commands will return the status of each disk attached to the specified VM.</span></span>

## <a name="decrypting-drives"></a><span data-ttu-id="e3c48-164">Decrittografia di unità</span><span class="sxs-lookup"><span data-stu-id="e3c48-164">Decrypting drives</span></span>

<span data-ttu-id="e3c48-165">È possibile invertire la crittografia tramite PowerShell usando `Disable-AzureRmVMDiskEncryption`.</span><span class="sxs-lookup"><span data-stu-id="e3c48-165">You can reverse the encryption through PowerShell using `Disable-AzureRmVMDiskEncryption`.</span></span>

```powershell
Disable-AzureRmVMDiskEncryption -ResourceGroupName <resource-group> -VMName <vm-name>
```

<span data-ttu-id="e3c48-166">Per l'interfaccia della riga di comando di Azure usare il comando `vm encryption disable`.</span><span class="sxs-lookup"><span data-stu-id="e3c48-166">For the Azure CLI, use the `vm encryption disable` command.</span></span>

```azurecli
az vm encryption disable --resource-group <resource-group> --name <vm-name>
```

<span data-ttu-id="e3c48-167">Questi comandi disabilitano la crittografia per i volumi di tipo all per la macchina virtuale specificata.</span><span class="sxs-lookup"><span data-stu-id="e3c48-167">These commands disable encryption for volumes of type all for the specified virtual machine.</span></span> <span data-ttu-id="e3c48-168">Proprio come per la versione di crittografia, è possibile specificare un parametro `-VolumeType` `[All | OS | Data]` per decidere quali dischi decrittografare.</span><span class="sxs-lookup"><span data-stu-id="e3c48-168">Just like the encrypt version, you can specify a `-VolumeType` parameter `[All | OS | Data]` to decide what disks to decrypt.</span></span> <span data-ttu-id="e3c48-169">L'impostazione predefinita è `All`, se non ne è specificata un'altra.</span><span class="sxs-lookup"><span data-stu-id="e3c48-169">It defaults to `All` if not supplied.</span></span>

> [!WARNING]
> <span data-ttu-id="e3c48-170">La disabilitazione della crittografia dei dischi dati nella macchina virtuale Windows quando sia i dischi dati che il disco del sistema operativo sono stati crittografati non funziona come previsto.</span><span class="sxs-lookup"><span data-stu-id="e3c48-170">Disabling data disk encryption on Windows VM when both OS and data disks have been encrypted doesn't work as expected.</span></span> <span data-ttu-id="e3c48-171">È infatti necessario disabilitare la crittografia su tutti i dischi.</span><span class="sxs-lookup"><span data-stu-id="e3c48-171">You must disable encryption on all disks instead.</span></span>

<span data-ttu-id="e3c48-172">Ora è possibile provare questi comandi in una nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="e3c48-172">Let's try some of these commands out on a new VM.</span></span>