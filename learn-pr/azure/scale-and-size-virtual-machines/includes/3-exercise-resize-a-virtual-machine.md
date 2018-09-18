In questo esercizio si crea una macchina virtuale e quindi la si ridimensiona usando il portale e Azure PowerShell.

## <a name="create-a-vm"></a>Creare una macchina virtuale

1. Nel Web browser passare al [portale di Azure](https://portal.azure.com?azure-portal=true) e accedere al proprio account.

1. Nel portale di Azure creare una nuova risorsa. Nel pannello **Nuovo** digitare **macchina virtuale** nella casella di ricerca e quindi premere INVIO.

1. Nel pannello **Tutto**, in **Risultati** fare clic su **Windows Server 2016 Datacenter**.

1. Nel pannello **Windows Server 2016 Datacenter** fare clic su **Crea**.

1. Nel pannello **Nozioni di base** completare i dettagli usando le informazioni seguenti e quindi fare clic su **OK**.

    |Impostazione|Valore|
    |---|---|
    |Nome|DB01|
    |Nome utente|LocalAdmin|
    |Password e Conferma password|Adm1nPa$$word|
    |Gruppo di risorse|ExerciseRG|
    |Località|Stati Uniti centrali|

1. Nel pannello **Scegli una dimensione** selezionare **D2s_v3** e quindi fare clic su **Seleziona**.

1. Nel pannello **Impostazioni**, in **Selezionare le porte in ingresso pubbliche** selezionare **HTTP**, **HTTPS** e **RDP (3389)**, in **Diagnostica di avvio** fare clic su **Disabilitata**, lasciare il valore predefinito per tutte le altre impostazioni e quindi fare clic su **OK**.

1. Nel pannello **Crea** fare clic su **Crea**.

1. Attendere il completamento della distribuzione prima di continuare con l'esercizio.

## <a name="resize-using-the-portal"></a>Eseguire il ridimensionamento tramite il portale

1. Nel portale di Azure passare al gruppo di risorse ExerciseRG e nel pannello **ExerciseRG** fare clic sull'oggetto macchina virtuale **DB01**.

1. Nel pannello **DB01** fare clic su **Dimensione**. La dimensione attualmente evidenziata è quella selezionata al momento della creazione della macchina virtuale.

1. Nel pannello **Scegli una dimensione** cercare la dimensione **F2s_v2**, che non dovrebbe essere disponibile nell'elenco di dimensioni perché la macchina virtuale è in esecuzione e tale dimensione appartiene a una famiglia diversa. Scegliere il pannello **Scegli una dimensione**.

1. Nel pannello **DB01** fare clic su **Arresta**. Nella finestra di dialogo **Arresta questa macchina virtuale** fare clic su **Sì** e attendere che lo stato della macchina virtuale sia indicato come **Arrestato (deallocato)**.

1. Nel pannello **DB01** fare clic su **Dimensione**. Nel pannello **Scegli una dimensione** selezionare **F2s_v2** e quindi fare clic su **Seleziona**. Osservare la notifica sul ridimensionamento della macchina virtuale.

1. Nel pannello **DB01** fare clic su **Panoramica** e quindi su **Avvia**.

## <a name="resize-using-powershell"></a>Eseguire il ridimensionamento tramite PowerShell

1. Nel portale di Azure aprire Cloud Shell.

1. Usare il cmdlet seguente per ottenere l'elenco delle dimensioni di macchine virtuali disponibili.

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName ExerciseRG -VMName DB01
    ```

1. Usare il cmdlet seguente per ridimensionare la macchina virtuale scegliendo la dimensione F4s_v2.

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName ExerciseRG -VMName DB01
    $vm.HardwareProfile.VmSize = "Standard_F4s_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName ExerciseRG
    ```

1. Fare clic sul pulsante Aggiorna nel pannello DB01 mentre si attende il completamento del comando PowerShell. È possibile notare che la macchina virtuale si sta riavviando per applicare la modifica della dimensione.

In questo esercizio è stata creata una macchina virtuale, che è stata ridimensionata con due diversi strumenti. È utile tenere presente che la dimensione di destinazione potrebbe non essere disponibile mentre la macchina virtuale è in esecuzione. Arrestando la macchina virtuale è possibile scegliere altre dimensioni.