Si supponga di sviluppare un'applicazione di gestione finanziaria per nuove startup. Poiché è necessario assicurarsi che tutti i dati dei clienti siano protetti, si è deciso di implementare Crittografia dischi di Azure in tutti i dischi del sistema operativo e di dati sui server che ospiteranno l'applicazione. Per soddisfare i requisiti di conformità, è anche necessario essere responsabili della gestione delle chiavi di crittografia.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

In questa unità si crittograferanno i dischi di una macchina virtuale esistente e si gestiranno le chiavi di crittografia usando Azure Key Vault.

## <a name="prepare-the-environment"></a>Preparare l'ambiente

Si inizierà distribuendo una nuova macchina virtuale Windows in una macchina virtuale di Azure.

### <a name="deploy-windows-vm"></a>Distribuzione di una macchina virtuale Windows

Utilizzare Powershell di Microsoft Azure per creare e distribuire una nuova macchina virtuale Windows.

1. Per iniziare, decidere dove posizionare le nuove risorse. Selezionare una località vicina dall'elenco seguente.

    <!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]
    

1. Definire una variabile di PowerShell per contenere la località selezionata. In questo caso la località è "Stati Uniti orientali", modificarla scegliendo la località preferita.

    ```powershell
    $location = "eastus"
    ```
    
    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. Successivamente, definire altre comode variabili per acquisire il _nome_ della macchina virtuale e il _gruppo di risorse_. Si noti che in questo caso si sta usando il gruppo di risorse creato in precedenza mentre, in genere, si crea un _nuovo_ gruppo di risorse nella sottoscrizione usando `New-AzureRmResourceGroup`.

    ```powershell
    $vmName = "fmdata-vm01"
    $rgName = "<rgn>[sandbox Resource Group]</rgn>"
    ```
    
1. Usare `New-AzureRmVm` per creare una nuova macchina virtuale.
    
    ```powershell
    New-AzureRmVm `
        -ResourceGroupName $rgName `
        -Name $vmName `
        -Location $location `
        -OpenPorts 3389
    ```
    
    Immettere un nome utente e una password per la macchina virtuale quando Cloud Shell lo richiede. Questo verrà utilizzato come l'account iniziale creato per la macchina virtuale.
    
    > [!NOTE]
    > Questo comando userà alcune impostazioni predefinite perché alcune opzioni non sono state definite. In particolare, verrà creata un'immagine _Windows 2016 Server_ con dimensione _Standard_DS1_v2_. Tenere presente che le macchine virtuali di livello Basic non supportano ADE se si decide di specificare le dimensioni della macchina virtuale.

1. Una volta completata la distribuzione della macchina virtuale, acquisire i dettagli della macchina virtuale in una variabile. Questa variabile consente di esaminare ciò che è stato creato.

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```
    
1. È possibile visualizzare il disco del sistema operativo collegato alla macchina virtuale:

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
        
1. Controllare lo stato corrente della crittografia del disco del sistema operativo e di eventuali dischi dati.

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  `
        -ResourceGroupName $rgName `
        -VMName $vmName
    ```

    Come si può notare, al momento i dischi _non sono crittografati_. 

    ```output
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : NotEncrypted
    OsVolumeEncryptionSettings :
    ProgressMessage            : No Encryption extension or metadata found on the VM
    ```
    
Seguire questa procedura per intervenire.
    
## <a name="encrypt-the-vm-disks-with-azure-disk-encryption"></a>Crittografare i dischi della macchina virtuale con Crittografia dischi di Azure

Per proteggere questi dati è necessario crittografare i dischi. Tenere presente che è necessario eseguire diverse operazioni:

1. Creare un insieme di credenziali delle chiavi.
1. Configurare l'insieme di credenziali delle chiavi per il supporto della crittografia dei dischi.
1. Indicare ad Azure di crittografare i dischi della macchina virtuale con la chiave archiviata nell'insieme di credenziali delle chiavi.

> [!TIP]
> Ora ogni passaggio verrà spiegato singolarmente, ma quando si esegue questa operazione nella propria sottoscrizione, è possibile usare un comodo script di PowerShell indicato come collegamento nel riepilogo di questo modulo.

### <a name="create-a-key-vault"></a>Creare un insieme di credenziali delle chiavi

Per creare un Azure Key Vault, è necessario abilitare il servizio nella sottoscrizione. Si tratta di un'esigenza una tantum.

> [!TIP]
> A seconda della sottoscrizione, potrebbe essere necessario abilitare il provider **Microsoft.KeyVault** con il cmdlet `Register-AzureRmResourceProvider`. Questa operazione non è necessaria nella sottoscrizione dell'ambiente sandbox di Azure.

1. Specificare un nome per il nuovo insieme di credenziali delle chiavi. Deve essere univoco e contenere da 3 a 24 caratteri, che possono includere numeri, lettere e trattini. Provare ad aggiungere alcuni numeri casuali fino alla fine, sostituendo la stringa "1234" riportata di seguito.

    ```powershell
    $keyVaultName = "mvmdsk-kv-1234"
    ```
        
1. Creare un Azure Key Vault con `New-AzureRmKeyVault`.
    - Assicurarsi di posizionarlo nello stesso gruppo di risorse _e_ nella stessa località della macchina virtuale.
    - Abilitare l'insieme di credenziali delle chiavi per l'uso con la crittografia del disco. 
    - Specificare un nome univoco per l'insieme di credenziali delle chiavi.

    ```powershell
    New-AzureRmKeyVault -VaultName $keyVaultName `
        -Location $location `
        -ResourceGroupName $rgName `
        -EnabledForDiskEncryption
    ```

    Verrà visualizzato un avviso da questo comando perché nessun utente dispone di accesso.

    ```output
    WARNING: Access policy is not set. No user or application have access permission to use this vault. This can happen if the vault was created by a service principal. Please use Set-AzureRmKeyVaultAccessPolicy to set access policies.
    ```

    Questa situazione è accettabile perché l'insieme di credenziali verrà utilizzato solo per archiviare le chiavi di crittografia per la macchina virtuale e gli utenti non avranno bisogno di accedere a questi dati.

### <a name="encrypt-the-disk"></a>Crittografare il disco

Siamo quasi pronti per crittografare i dischi. Prima di farlo, considerare questo avviso relativo alla creazione di copie di backup.

> [!IMPORTANT]
> Se questo fosse un sistema di produzione, sarebbe necessario eseguire una copia di backup dei dischi gestiti usando Backup di Azure o creando uno snapshot. È possibile creare gli snapshot nel portale di Azure o tramite la riga di comando. In PowerShell usare il cmdlet `New-AzureRmSnapshot`. Poiché questo è un semplice esercizio i cui dati alla fine verranno eliminati, è possibile ignorare questo passaggio. 

1. Iniziare definendo una variabile per contenere le informazioni dell'insieme di credenziali delle chiavi.

    ```powershell
    $keyVault = Get-AzureRmKeyVault `
        -VaultName $keyVaultName `
        -ResourceGroupName $rgName
    ```

1. Quindi, usare il cmdlet `Set-AzureRmVmDiskEncryptionExtension` per crittografare i dischi della macchina virtuale.
    - Il parametro `VolumeType` consente di specificare quali dischi crittografare: [_Tutti_ | _SO_ | _Dati_]. L'impostazione predefinita è _Tutti_. È possibile crittografare solo i dischi dati di alcune distribuzioni di Linux.
    - È necessario fornire il flag `SkipVmBackup` per i dischi gestiti altrimenti il comando avrà esito negativo perché non c'è uno snapshot.

    ```powershell
    Set-AzureRmVmDiskEncryptionExtension `
        -ResourceGroupName $rgName `
        -VMName $vmName `
        -VolumeType All `
        -DiskEncryptionKeyVaultId $keyVault.ResourceId `
        -DiskEncryptionKeyVaultUrl $keyVault.VaultUri `
        -SkipVmBackup
    ```

1. Il cmdlet avviserà l'utente che la macchina virtuale deve essere portata offline e che l'attività potrebbe richiedere alcuni minuti. Lasciar proseguire l'operazione.

    ```output
    Enable AzureDiskEncryption on the VM
    This cmdlet prepares the VM and enables encryption which may reboot the machine and takes 10-15 minutes to
    finish. Please save your work on the VM before confirming. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    ```
    
1. Una volta completata, controllare di nuovo lo stato della crittografia.

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
    ```

    Ora il disco del sistema operativo è crittografato. Eventuali dischi dati collegati visibili per Windows saranno ugualmente crittografati.

    ```output
    OsVolumeEncrypted          : Encrypted
    DataVolumesEncrypted       : NoDiskFound
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : Provisioning succeeded
    ```

> [!NOTE]        
> I nuovi dischi aggiunti dopo la crittografia _non_ verranno automaticamente crittografati. Eseguire nuovamente il cmdlet `Set-AzureRmVMDiskEncryptionExtension` per crittografare i nuovi dischi. Se si aggiungono dischi a una macchina virtuale i cui dischi sono già stati crittografati, assicurarsi di specificare un nuovo numero di sequenza. Inoltre, i dischi che non sono visibili per il sistema operativo non verranno crittografati. Per poter essere visto dall'estensione Bitlocker, il disco deve essere correttamente partizionato, formattato e montato.