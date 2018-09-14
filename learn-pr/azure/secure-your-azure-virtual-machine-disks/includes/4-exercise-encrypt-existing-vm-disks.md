Si supponga di sviluppare un'applicazione di gestione finanziaria per nuove startup. Si desidera assicurarsi che tutti i dati dei clienti è protetta, in modo che si è deciso di implementare la crittografia dischi di Azure (ADE) in tutti i dischi del sistema operativo e i dati sui server che ospiterà l'applicazione. Per soddisfare i requisiti di conformità, è anche necessario essere responsabili della gestione delle chiavi di crittografia.

In questa unità, verrà crittografare i dischi di macchine virtuali di Windows esistenti e gestire le chiavi di crittografia usando il proprio insieme di credenziali chiave di Azure.

> [!IMPORTANT] 
> Questo esercizio si presuppone che Azure PowerShell sia installato nel computer. Passare a [installare Azure PowerShell su Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) per informazioni su come installare Azure PowerShell.

## <a name="prepare-the-environment"></a>Preparare l'ambiente

Si verrà inizialmente la distribuzione di una macchina virtuale Windows in un nuovo gruppo di risorse e quindi aggiungere un disco dati alla macchina virtuale.

### <a name="deploy-windows-vm-using-the-azure-portal"></a>Distribuire VM Windows usando il portale di Azure

In questo caso si userà il portale di Azure per creare e distribuire una macchina virtuale Windows. Iniziare definendo le informazioni di base sulla macchina virtuale:

1. In un browser passare al [portale di Azure](http://portal.azure.com) e accedere con le credenziali normali.

1. Nella barra laterale fare clic su **Macchine virtuali** e quindi su **Crea macchina virtuale**.

1. Nel pannello Calcolo, nella sezione **Consigliati** fare clic su **Windows Server**.

1. Nel pannello **Windows Server** fare clic su **Windows Server 2016 Datacenter**.

1. Nel pannello **Windows Server 2016 Datacenter** fare clic su **Crea**.

1. Nel pannello **Informazioni di base**, nella casella **Nome** digitare **moneyappsvr01.**

1. Nelle caselle **Nome utente** e **Password** digitare un nome e una password per un account amministratore su questo server.

1. Nella casella **Sottoscrizione** selezionare la sottoscrizione di Azure.

1. Sotto **gruppo di risorse**, selezionare **Crea nuovo**. Nella casella, digitare **moneyapprg**.

1. Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.

1. Fare clic su **OK**.

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a>Scegliere una dimensione per la macchina virtuale e avviare la distribuzione

> [!IMPORTANT]
> Tenere presente che le macchine virtuali di livello basic non supportano ADE.

1. Nel **Scegli una dimensione** pannello Seleziona un **Standard** SKU, ad esempio **B1s**. Fare clic su **Seleziona**.

1. Nel **impostazioni** pannello nella **selezionare le porte in ingresso pubbliche** elenco, fare clic su **RDP**. Scorrere verso il basso e fare clic su **OK**.

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

1. Nel pannello **Dischi** fare clic su **Salva**. Si noti che lo stato della crittografia del disco di dati è attualmente **non è abilitato**.

## <a name="configure-disk-encryption-prerequisites"></a>Configurare i prerequisiti di crittografia del disco

A questo punto si userà lo script di configurazione dei prerequisiti di crittografia dischi di Azure per configurare tutti i prerequisiti di crittografia del disco. Questo script crea e preparare un insieme di credenziali delle chiavi nella stessa area della macchina virtuale.

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a>Preparare lo script di installazione dei prerequisiti di crittografia dischi di Azure

1. Andare alla [script di installazione dei prerequisiti di crittografia dischi di Azure](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1) pagina su GitHub.

1. Nella pagina di GibHub fare clic su **Raw** (Non elaborato).

1. Usare Ctrl + A per selezionare tutto il testo della pagina e quindi usare Ctrl + C per copiare tutto il testo della pagina negli Appunti.

1. Nel computer, fare clic su **avviare**, quindi selezionare **Windows PowerShell ISE**.

1. Fare doppio clic su **Windows PowerShell ISE**, fare clic su **Esegui come amministratore**.

1. Nell'amministratore: finestra di Windows PowerShell ISE, fare clic su **vista**, quindi fare clic su **Mostra riquadro di Script**.

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

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a>Eseguire lo script di installazione dei prerequisiti di crittografia dischi di Azure

1. In PowerShell ISE console riquadro, digitare il comando seguente e premere **invio**:

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. In PowerShell ISE console riquadro, digitare il comando seguente e premere **invio**:

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   Se viene visualizzata la finestra di dialogo **Modifica ai criteri di esecuzione** fare clic su **Sì per tutti** o su **Sì** (se non viene visualizzata l'opzione _Sì per tutti_).

1. In PowerShell ISE console riquadro, digitare il comando seguente e premere **invio**:

   ```powershell
   Login-AzureRmAccount
   ```

1. Immettere le credenziali di Azure.

1. Selezionare la stringa **SubscriptionId** e copiarla negli Appunti.

1. In PowerShell ISE, fare clic su **File**, quindi fare clic su **eseguire**.

1. Nel riquadro della console, nelle **resourceGroupName:** , digitare **moneyapprg**. Quindi premere **invio**.

1. Nel riquadro della console, nelle **keyVaultName:** , digitare **moneyappkv**. Quindi premere **invio**.

1. Nel riquadro della console, al prompt **location:** digitare la posizione usata durante la creazione della macchina virtuale.

1. Nel riquadro della console, al prompt **subscriptionId:** incollare l'ID della sottoscrizione.

1. Verrà ora creato l'insieme di credenziali delle chiavi **moneyappkv**. Al termine, selezionare il testo di riepilogo (in verde) e copiarlo nel Blocco note.

1. Premere **invio** per continuare.

1. Nel riquadro della console, nelle **aadAppName:** , digitare **moneyapp**. Quindi premere **invio**.

1. Verrà ora creata l'applicazione di Azure AD **moneyapp**. Al termine, selezionare il testo di riepilogo (in verde) e copiarlo nel Blocco note.

1. Premere **invio** per continuare.

### <a name="encrypt-your-vm-disks-with-powershell"></a>Crittografare i dischi delle macchina virtuale con PowerShell

Verificare lo stato della crittografia dei dischi del sistema operativo e dei dati:

1. In PowerShell ISE console riquadro, digitare il comando seguente e premere **invio**:

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > Il nome della macchina virtuale deve essere racchiuso tra virgolette singole.

1. Nel riquadro di script di PowerShell ISE, immettere il seguente comando e premere **invio**:

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. Nella finestra di dialogo **Enable AzureDiskEncryption on the VM** (Abilita Crittografia dischi di Azure nella VM) fare clic su **Sì** e notare il messaggio indicante che il completamento della crittografia può richiedere 10-15 minuti.

>[!IMPORTANT]
> Attendere che il comando è stata completata prima di continuare con questo esercizio.

### <a name="verify-the-encryption-status-of-your-vm-disks"></a>Verificare lo stato della crittografia dei dischi delle macchina virtuale

Passare al portale di Azure. Nel **dischi** pannello **moneyappsvr01**, si noti che lo stato della crittografia del disco per i dischi dati e del sistema operativo è a questo punto **abilitato**.
