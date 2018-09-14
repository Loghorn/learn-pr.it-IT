In questo esercizio, si crea una macchina virtuale e quindi ridimensionarlo usando il portale e Azure PowerShell.

## <a name="create-a-vm"></a>Creare una macchina virtuale

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Nel portale di Azure, creare una nuova risorsa. Nel **New** blade, digitare **macchina virtuale** nella casella di ricerca e quindi premere INVIO.

1. Nel **tutti gli elementi** pannello, in **risultati**, fare clic su **Windows Server 2016 Datacenter**.

1. Nel **Windows Server 2016 Datacenter** pannello, fare clic su **crea**.

1. Nel **nozioni di base** blade, completare i dettagli usando le informazioni seguenti e quindi fare clic su **OK**.

    |Impostazione|Valore|
    |---|---|
    |Nome|DB01|
    |Username|LocalAdmin|
    |Password e Conferma password|Adm1nPa$$word|
    |Gruppo di risorse|<rgn>[Nome gruppo di risorse di tipo sandbox]</rgn>|
    |Posizione|*Selezionare un'area nell'elenco*|

1. Nel **Scegli una dimensione** pannello, seleziona **D2s_v3**, quindi fare clic su **selezionare**.

1. Nel **le impostazioni** pannello, in **selezionare le porte in ingresso pubbliche** seleziona **HTTP**, **HTTPS**, e **RDP (3389)**. Sotto **diagnostica di avvio**, fare clic su **disabilitato**. Tutte le altre impostazioni lasciare il valore predefinito e quindi fare clic su **OK**.

1. Nel pannello **Crea** fare clic su **Crea**.

1. Attendere che la distribuzione è stata completata prima di continuare l'esercizio.

## <a name="resize-using-the-portal"></a>Ridimensionare con il portale

1. Nel portale di Azure, passare al gruppo di risorse ExerciseRG e il **ExerciseRG** pannello, fare clic sui **DB01** oggetto macchina virtuale.

1. Nel **DB01** pannello, fare clic su **dimensioni**. Si noti che la dimensione attualmente evidenziata è di dimensione selezionato al momento della creazione della macchina virtuale.

1. Nel **Scegli una dimensione** pannello, provare a trovare il **F2s_v2** dimensioni - non deve essere disponibile nell'elenco delle dimensioni in quanto la macchina virtuale è attualmente in esecuzione e che la dimensione è una famiglia diversa. Chiudi il **Scegli una dimensione** pannello.

1. Nel **DB01** pannello, fare clic su **arrestare**. Nel **arresta questa macchina virtuale** della finestra di dialogo fare clic su **Yes**e attendere che lo stato della macchina virtuale mostrare **arrestato (deallocato)**.

1. Nel **DB01** pannello, fare clic su **dimensioni**. Nel **Scegli una dimensione** blade, selezionare **F2s_v2** e quindi fare clic su **selezionare**. Si noti che la notifica sul ridimensionamento della macchina virtuale.

1. Nel **DB01** pannello, fare clic su **Cenni preliminari sulla**e quindi fare clic su **avviare**.

## <a name="resize-using-powershell"></a>Ridimensionare con PowerShell

1. Nel portale di Azure, aprire Azure Cloud Shell.

1. Usare il cmdlet seguente per ottenere l'elenco delle dimensioni delle macchine virtuali disponibili.

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName ExerciseRG -VMName DB01
    ```

1. Usare il cmdlet seguente per ridimensionare la macchina virtuale a una dimensione F4s_v2.

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName ExerciseRG -VMName DB01
    $vm.HardwareProfile.VmSize = "Standard_F4s_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName ExerciseRG
    ```

1. Fare clic sul pulsante Aggiorna il pannello DB01 mentre si attende il completamento del comando PowerShell. Si noterà che la macchina virtuale verrà riavviata per supportare la modifica delle dimensioni.

In questo esercizio, viene creata una macchina virtuale e viene ridimensionato con due diversi strumenti. Un buon suggerimento da tenere a mente è che le dimensioni di destinazione potrebbero non essere disponibile mentre è in esecuzione la macchina virtuale. arresto della macchina virtuale è possibile scegliere altre dimensioni.