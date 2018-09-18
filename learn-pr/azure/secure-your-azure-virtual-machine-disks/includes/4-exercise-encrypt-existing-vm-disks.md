Si supponga di sviluppare un'applicazione di gestione finanziaria per nuove startup. Poiché è necessario assicurarsi che tutti i dati dei clienti siano protetti, si è deciso di implementare Crittografia dischi di Azure in tutti i dischi del sistema operativo e di dati sui server che ospiteranno l'applicazione. Per soddisfare i requisiti di conformità, è anche necessario essere responsabili della gestione delle chiavi di crittografia.

In questa unità si crittograferanno i dischi nelle macchine virtuali Windows esistenti e si gestiranno le chiavi di crittografia usando Azure Key Vault.

> [!IMPORTANT] 
> In questo esercizio si presuppone che Azure PowerShell sia installato nel computer. Per informazioni su come installare Azure PowerShell, vedere [Installare Azure PowerShell in Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0).

## <a name="prepare-the-environment"></a>Preparare l'ambiente

Si inizierà distribuendo una macchina virtuale Windows in un nuovo gruppo di risorse e quindi si aggiungerà un disco dati alla macchina virtuale.

### <a name="deploy-windows-vm-using-the-azure-portal"></a>Distribuire una macchina virtuale Windows con il portale di Azure

Si userà ora il portale di Azure per creare e distribuire una macchina virtuale Windows. Iniziare definendo le informazioni di base sulla macchina virtuale:

1. In un browser passare al [portale di Azure](http://portal.azure.com) e accedere con le credenziali normali.

1. Nella barra laterale fare clic su **Macchine virtuali** e quindi su **Crea macchina virtuale**.

1. Nel pannello Calcolo, nella sezione **Consigliati** fare clic su **Windows Server**.

1. Nel pannello **Windows Server** fare clic su **Windows Server 2016 Datacenter**.

1. Nel pannello **Windows Server 2016 Datacenter** fare clic su **Crea**.

1. Nel pannello **Informazioni di base**, nella casella **Nome** digitare **moneyappsvr01.**

1. Nelle caselle **Nome utente** e **Password** digitare un nome e una password per un account amministratore su questo server.

1. Nella casella **Sottoscrizione** selezionare la sottoscrizione di Azure.

1. In **Gruppo di risorse** selezionare **Crea nuovo**. Nella casella digitare **moneyapprg**.

1. Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.

1. Fare clic su **OK**.

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a>Scegliere una dimensione per la macchina virtuale e avviare la distribuzione

> [!IMPORTANT]
> Tenere presente che le macchine virtuali di livello Basic non supportano Crittografia dischi di Azure.

1. Nel pannello **Scegli una dimensione** selezionare uno SKU **Standard**, ad esempio **B1s**. Fare clic su **Seleziona**.

1. Nel pannello **Impostazioni**, nell'elenco **Selezionare le porte in ingresso pubbliche** fare clic su **RDP**. Scorrere quindi verso il basso e fare clic su **OK**.

1. Nel pannello **Crea** fare clic su **Crea**.

1. Attendere il completamento della distribuzione della macchina virtuale prima di continuare con l'esercizio.

### <a name="add-a-data-disk-to-the-vm"></a>Aggiungere un disco dati alla macchina virtuale

1. Nel menu di sinistra fare clic su **Tutte le risorse** e quindi su **moneyappsvr01**.

1. Nel pannello **Macchina virtuale**, in **IMPOSTAZIONI** fare clic su **Dischi**.

1. Nel pannello **Dischi** si noti che lo stato della crittografia del disco del sistema operativo è attualmente **Non abilitato** e quindi fare clic su **Aggiungi disco dati**.

1. Fare clic nell'elenco **Nome** e quindi su **Crea disco**.

1. Nel pannello **Crea disco gestito**, nella casella **Nome** digitare **moneyappsvr01_data**.

1. In **Gruppo di risorse** selezionare **Usa esistente** e nell'elenco selezionare **moneyapprg**.

1. Fare clic su **Crea**.

1. Attendere il completamento della creazione del disco prima di continuare.

1. Nel pannello **Dischi** fare clic su **Salva**. Si noti che lo stato della crittografia del disco dati è attualmente **Non abilitato**.

## <a name="configure-disk-encryption-prerequisites"></a>Configurare i prerequisiti di crittografia dei dischi

Si userà ora lo script di configurazione dei prerequisiti di Crittografia dischi di Azure per configurare tutti i prerequisiti di crittografia dei dischi. Questo script crea e prepara un insieme di credenziali delle chiavi nella stessa area della macchina virtuale.

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a>Preparare lo script di configurazione dei prerequisiti di Crittografia dischi di Azure

1. Andare alla pagina di GitHub [Azure Disk Encryption prerequisite setup script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1) (Script di configurazione dei prerequisiti di Crittografia dischi di Azure).

1. Nella pagina di GibHub fare clic su **Raw** (Non elaborato).

1. Premere CTRL+A per selezionare tutto il testo della pagina e quindi CTRL+C per copiarlo negli Appunti.

1. Nel computer fare clic su **Start** e quindi passare a **Windows PowerShell ISE**.

1. Fare clic con il pulsante destro del mouse su **Windows PowerShell ISE** e scegliere **Esegui come amministratore**.

1. Nella finestra Amministratore: Windows PowerShell ISE fare clic su **Visualizza** e quindi su **Mostra riquadro di script**.

1. Incollare il testo copiato nel riquadro di script.

1. Nel riquadro di script individuare il blocco di codice seguente:

    ```powershell
    [Parameter(Mandatory = $false,
            HelpMessage="Name of the AAD application that will be used to write secrets to KeyVault. A new application with this name will be created if one doesn't exist. If this app already exists, pass aadClientSecret parameter to the script")]
    [ValidateNotNullOrEmpty()]
    [string]$aadAppName,
    ```
1. Nel blocco di codice impostare `$false` su `$true`.

1. Fare clic su **File**, quindi su **Salva con nome** e passare alla cartella che si vuole usare per salvare lo script.

1. Nella casella **Nome file** digitare **ADEPrereqScript.ps1** e fare clic su **Salva**.

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a>Eseguire lo script di configurazione dei prerequisiti di Crittografia dischi di Azure

1. Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   Se viene visualizzata la finestra di dialogo **Modifica ai criteri di esecuzione** fare clic su **Sì per tutti** o su **Sì** (se non viene visualizzata l'opzione _Sì per tutti_).

1. Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:

   ```powershell
   Login-AzureRmAccount
   ```

1. Immettere le credenziali di Azure.

1. Selezionare la stringa **SubscriptionId** e copiarla negli Appunti.

1. In PowerShell ISE fare clic su **File** e quindi su **Esegui**.

1. Nel riquadro della console, al prompt **resourceGroupName:** digitare **moneyapprg**. Premere **INVIO**.

1. Nel riquadro della console, al prompt **keyVaultName:** digitare **moneyappkv**. Premere **INVIO**.

1. Nel riquadro della console, al prompt **location:** digitare la posizione usata durante la creazione della macchina virtuale.

1. Nel riquadro della console, al prompt **subscriptionId:** incollare l'ID della sottoscrizione.

1. Verrà ora creato l'insieme di credenziali delle chiavi **moneyappkv**. Al termine, selezionare il testo di riepilogo (in verde) e copiarlo nel Blocco note.

1. Premere **INVIO** per continuare.

1. Nel riquadro della console, al prompt **aadAppName:** digitare **moneyapp**. Premere **INVIO**.

1. Verrà ora creata l'applicazione di Azure AD **moneyapp**. Al termine, selezionare il testo di riepilogo (in verde) e copiarlo nel Blocco note.

1. Premere **INVIO** per continuare.

### <a name="encrypt-your-vm-disks-with-powershell"></a>Crittografare i dischi delle macchina virtuale con PowerShell

Verificare lo stato della crittografia dei dischi del sistema operativo e dei dati:

1. Nel riquadro della console di PowerShell ISE digitare il comando seguente e premere **INVIO**:

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > Il nome della macchina virtuale deve essere racchiuso tra virgolette singole.

1. Nel riquadro di script di PowerShell ISE immettere il comando seguente e premere **INVIO**:

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. Nella finestra di dialogo **Enable AzureDiskEncryption on the VM** (Abilita Crittografia dischi di Azure nella VM) fare clic su **Sì** e notare il messaggio indicante che il completamento della crittografia può richiedere 10-15 minuti.

>[!IMPORTANT]
> Attendere il completamento del comando prima di continuare con questo esercizio.

### <a name="verify-the-encryption-status-of-your-vm-disks"></a>Verificare lo stato della crittografia dei dischi delle macchine virtuali

Accedere al portale di Azure. Nel pannello **Dischi** per **moneyappsvr01** si noti che lo stato della crittografia per i dischi del sistema operativo e dei dati ora è **Abilitato**.
