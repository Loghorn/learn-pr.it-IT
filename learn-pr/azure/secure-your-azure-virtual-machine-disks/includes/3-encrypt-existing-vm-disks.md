Si supponga che l'azienda abbia deciso di implementare Crittografia dischi di Azure in tutte le macchine virtuali. È necessario valutare come implementare la crittografia nei volumi delle macchine virtuali esistenti. In questo modulo si esamineranno i requisiti per Crittografia dischi di Azure e i passaggi necessari per la crittografia dei dischi nelle macchine virtuali Linux e Windows esistenti.

## <a name="azure-disk-encryption-prerequisites"></a>Prerequisiti di Crittografia dischi di Azure

Per poter crittografare i dischi di una macchina virtuale è necessario:

1. Creare un insieme di credenziali delle chiavi.
1. Impostare criteri di accesso dell'insieme di credenziali delle chiavi per supportare la crittografia dei dischi.
1. Usare l'insieme di credenziali delle chiavi per archiviare le chiavi di crittografia per Crittografia dischi di Azure.

### <a name="azure-key-vault"></a>Azure Key Vault

Le chiavi di crittografia usate da Crittografia dischi di Azure possono essere archiviate in Azure Key Vault. Azure Key Vault è uno strumento che consente di archiviare i segreti e di accedervi in modo sicuro. Un segreto è qualsiasi elemento per cui si vuole controllare rigorosamente l'accesso, ad esempio chiavi API, password o certificati. Questa soluzione garantisce archiviazione sicura, scalabile e a disponibilità elevata in moduli di protezione hardware conformi a FIPS (Federal Information Processing Standards) 140-2, livello 2. Con Azure Key Vault è possibile avere il controllo completo delle chiavi usate per crittografare i dati e gestire e controllare l'uso delle chiavi. 

> [!NOTE]
> Per Crittografia dischi di Azure è necessario che l'istanza dell'insieme di credenziali delle chiavi e le macchine virtuali si trovino nella stessa area di Azure, in modo che i segreti di crittografia non debbano attraversare confini a livello di area.

È possibile configurare e gestire l'insieme di credenziali delle chiavi con:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmKeyVault -Location <location> `
    -ResourceGroupName <resource-group> `
    -VaultName "myKeyVault" `
    -EnabledForDiskEncryption
```

### <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

```azurecli
az keyvault create \
    --name "myKeyVault" \
    --resource-group <resource-group> \
    --location <location> \
    --enabled-for-disk-encryption True
```

### <a name="azure-portal"></a>Portale di Azure

Azure Key Vault è una risorsa che può essere creata nel portale di Azure usando il normale processo di creazione delle risorse.

1. Fare clic su **Creare una risorsa** nella barra laterale a sinistra.

1. Cercare "insieme di credenziali delle chiavi". Fare clic su **Crea** nella finestra dei dettagli.

    ![Screenshot che mostra Azure Key Vault in Azure Marketplace](../media/3-create-keyvault.png)

1. Immettere i dettagli per il nuovo insieme di credenziali delle chiavi:
    - Immettere un **nome** per l'insieme di credenziali delle chiavi
    - Selezionare la sottoscrizione in cui inserirlo (la sottoscrizione corrente è l'impostazione predefinita).
    - Selezionare un **gruppo di risorse** o creare un nuovo gruppo proprietario dell'insieme di credenziali delle chiavi.
    - Selezionare una **posizione** per l'insieme di credenziali delle chiavi. Assicurarsi di selezionare la posizione in cui si trova la VM.
    - È possibile scegliere un piano tariffario Standard o Premium. La differenza principale è che il livello Premium consente chiavi supportate dalla crittografia hardware.

1. È necessario modificare i criteri di accesso per supportare la crittografia dei dischi. Per impostazione predefinita viene aggiunto l'account _personale_ ai criteri.
    - Selezionare **Criteri di accesso**
    - Fare clic su **Criteri di accesso avanzati**.
    - Selezionare la casella **Abilita l'accesso a Crittografia dischi di Azure per la crittografia dei volumi**.
    - È possibile rimuovere l'account, non è necessario se si intende usare l'insieme di credenziali delle chiavi solo per la crittografia dei dischi.
    - Fare clic su **OK** per salvare le modifiche.

    ![Screenshot che mostra le proprietà avanzate per Azure Key Vault con l'opzione Crittografia dischi di Azure selezionata ed evidenziata](../media/3-configure-access-policy.png)

1. Fare clic su **Crea** per creare il nuovo insieme di credenziali delle chiavi.

## <a name="enabling-access-policies-in-the-key-vault"></a>Abilitazione dei criteri di accesso nell'insieme di credenziali delle chiavi
Azure deve avere accesso alle chiavi di crittografia o ai segreti nell'insieme di credenziali delle chiavi per renderli disponibili alla macchina virtuale per l'avvio e la decrittografia dei volumi. Questo argomento è stato affrontato per il portale quando sono stati modificati i **criteri di accesso avanzati** sopra.

È possibile abilitare tre criteri.
1. **Crittografia del disco**: obbligatorio per Crittografia dischi di Azure.
1. **Distribuzione** (facoltativo): in questo modo si consente al provider di risorse Microsoft.Compute di recuperare segreti da questo insieme di credenziali delle chiavi quando vi viene fatto riferimento durante la creazione di risorse, ad esempio quando si crea una macchina virtuale.
1. **Distribuzione modelli** (facoltativo): in questo modo si consente ad Azure Resource Manager di recuperare segreti da questo insieme di credenziali delle chiavi quando vi viene fatto riferimento durante la distribuzione di un modello.

Di seguito viene illustrato come abilitare i criteri di crittografia del disco. Gli altri due sono simili, ma usano flag diversi.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <keyvault-name> -ResourceGroupName <resource-group> -EnabledForDiskEncryption
```

```azurecli
az keyvault update --name <keyvault-name> --resource-group <resource-group> --enabled-for-disk-encryption "true"
```

## <a name="encrypting-an-existing-vm-disk"></a>Crittografia di un disco di una macchina virtuale esistente

Una volta configurato l'insieme di credenziali delle chiavi, sarà possibile crittografare la macchina virtuale usando l'interfaccia della riga di comando di Azure o Azure PowerShell. La prima volta che si crittografa una macchina virtuale Windows, è possibile scegliere di crittografare tutti i dischi o solo il disco del sistema operativo. In alcune distribuzioni Linux è possibile crittografare solo i dischi dati. Per essere idonei per la crittografia, i dischi di Windows devono essere formattati come volumi NTFS.

> [!WARNING]
> Per attivare la crittografia, è necessario eseguire uno snapshot o un backup dei dischi gestiti. Il flag `SkipVmBackup` specificato di seguito indica allo strumento che il backup è stato completato nei dischi gestiti. Senza il backup, non sarà possibile ripristinare la macchina virtuale se la crittografia ha esito negativo per qualche motivo.

In PowerShell abilitare la crittografia con il cmdlet `Set-AzureRmVmDiskEncryptionExtension`.

```powershell

Set-AzureRmVmDiskEncryptionExtension `
    -ResourceGroupName <resource-group> `
    -VMName <vm-name> `
    -VolumeType [All | OS | Data]
    -DiskEncryptionKeyVaultId <keyVault.ResourceId> `
    -DiskEncryptionKeyVaultUrl <keyVault.VaultUri> `
     -SkipVmBackup
```

Nell'interfaccia della riga di comando di Azure abilitare la crittografia con il comando `az vm encryption enable`.

```azurecli
az vm encryption enable \
    --resource-group <resource-group> \
    --name <vm-name> \
    --disk-encryption-keyvault <keyvault-name> \
    --volume-type [all | os | data] \
    --skipvmbackup
```

## <a name="viewing-the-status-of-the-disk"></a>Visualizzazione dello stato del disco

È possibile controllare se dischi specifici sono crittografati o meno.

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName <resource-group> -VMName <vm-name>
```

```azurecli
az vm encryption status --resource-group <resource-group> --name <vm-name>
```

Entrambi questi comandi restituiranno lo stato di ogni disco collegato alla macchina virtuale specificata.

## <a name="decrypting-drives"></a>Decrittografia di unità

È possibile invertire la crittografia tramite PowerShell usando `Disable-AzureRmVMDiskEncryption`.

```powershell
Disable-AzureRmVMDiskEncryption -ResourceGroupName <resource-group> -VMName <vm-name>
```

Per l'interfaccia della riga di comando di Azure usare il comando `vm encryption disable`.

```azurecli
az vm encryption disable --resource-group <resource-group> --name <vm-name>
```

Questi comandi disabilitano la crittografia per i volumi di tipo all per la macchina virtuale specificata. Proprio come per la versione di crittografia, è possibile specificare un parametro `-VolumeType` `[All | OS | Data]` per decidere quali dischi decrittografare. L'impostazione predefinita è `All`, se non ne è specificata un'altra.

> [!WARNING]
> La disabilitazione della crittografia dei dischi dati nella macchina virtuale Windows quando sia i dischi dati che il disco del sistema operativo sono stati crittografati non funziona come previsto. È infatti necessario disabilitare la crittografia su tutti i dischi.

Ora è possibile provare questi comandi in una nuova macchina virtuale.