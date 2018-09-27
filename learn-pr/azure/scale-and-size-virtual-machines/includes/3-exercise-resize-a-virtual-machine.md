In questo esercizio si crea una macchina virtuale e quindi la si ridimensiona usando il portale e Azure PowerShell.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-vm-with-powershell"></a>Creare una VM con PowerShell

1. Usare il cmdlet `New-AzureRmVm` in Azure PowerShell per creare una macchina virtuale.
    - Usare il gruppo di risorse **<rgn>[Nome gruppo di risorse sandbox]</rgn>**.
    - Assegnare il nome "my-test-vm".
    - Passare _Standard_DS2_v2_ per il parametro **Dimensioni**.
    - Selezionare una località vicina dall'elenco seguente, disponibile nella sandbox di Azure. Assicurarsi di modificare il valore nel comando di esempio seguente, se si usa Copia e incolla.

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - Usare il cmdlet `Get-Credential` e inviare i risultati nel parametro `Credential` come indicato di seguito.

       Quando viene richiesto di immettere le credenziali, usare LocalAdmin come nome utente e Adm1nPa$$word come password.

    ```powershell
    New-AzureRmVm `
        -ResourceGroupName <rgn>[sandbox resource group name]</rgn> `
        -Name my-test-vm `
        -Credential (Get-Credential) `
        -Size "Standard_DS2_v2" `
        -Location eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


1. Attendere il completamento della distribuzione prima di continuare con l'esercizio.

## <a name="resize-using-the-portal"></a>Eseguire il ridimensionamento tramite il portale

1. Accedere al [portale di Azure per sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Selezionare **Gruppi di risorse** dalla barra laterale sinistra.

1. Selezionare il gruppo di risorse <rgn>[Nome gruppo di risorse sandbox]</rgn> e nella panoramica fare clic sull'oggetto macchina virtuale **my-test-vm**.

1. Nella **panoramica** della macchina virtuale si può notare che la dimensione corrente della macchina virtuale è _DS2 Standard v2 (2 vcpu, 7 GB memory)_ ovvero ciò che si è creato qualche istante fa.

1. Nella sezione delle **impostazioni** selezionare **Dimensioni**.

1. Nel pannello **Scegli una dimensione** provare a trovare la dimensione **F2s_v2**. Questa non verrà visualizzata nell'elenco delle dimensioni disponibili perché la macchina virtuale è attualmente in esecuzione e quella dimensione appartiene a una famiglia diversa. In alcuni casi, è necessario arrestare la macchina virtuale per visualizzare tutte le dimensioni di VM disponibili.

1. Verrà quindi scelta una dimensione che è attualmente disponibile durante l'esecuzione di questa macchina virtuale. Sempre nel pannello **Scegli una dimensione** selezionare _DS3_v2 Standard_ e quindi fare clic su **Seleziona**. Osservare la notifica sul ridimensionamento della macchina virtuale.

1. Nel pannello **Panoramica** verificare che la macchina virtuale sia stata ridimensionata in _Standard DS3 v2 (4 vcpus, 14 GB memory)_.

## <a name="resize-using-powershell"></a>Eseguire il ridimensionamento con PowerShell

1. Nel portale di Azure aprire Azure Cloud Shell facendo clic sul pulsante Cloud Shell nella barra degli strumenti superiore.

    Assicurarsi che Cloud Shell sia impostata in modo da usare PowerShell e non Bash, in alto a sinistra nella finestra di Cloud Shell.

1. Usare il cmdlet seguente per ottenere l'elenco delle dimensioni di macchine virtuali disponibili.

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    ```

1. Usare il cmdlet seguente per ridimensionare la macchina virtuale scegliendo di nuovo la dimensione _DS2_v2_.

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    $vm.HardwareProfile.VmSize = "Standard_DS2_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```

1. Fare clic sul pulsante di **aggiornamento** nel pannello **my-test-vm** mentre si attende il completamento del comando PowerShell. Si noterà che la macchina virtuale viene riavviata per supportare la modifica delle dimensioni.

In questo esercizio è stata creata una macchina virtuale, che è stata ridimensionata con due diversi strumenti. È utile tenere presente che la dimensione di destinazione potrebbe non essere disponibile mentre la macchina virtuale è in esecuzione. Arrestando la macchina virtuale è possibile scegliere altre dimensioni.