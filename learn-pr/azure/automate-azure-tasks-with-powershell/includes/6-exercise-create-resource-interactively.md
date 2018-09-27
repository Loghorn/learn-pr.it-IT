Ricordare lo scenario originale: creazione di macchine virtuali per testare il software CRM. Quando è disponibile una nuova build, si vuole creare rapidamente una nuova macchina virtuale in modo da poter testare l'esperienza di installazione completa da un'immagine pulita. Una volta completati i test, la macchina virtuale dovrà essere eliminata.

Si proveranno ora i comandi da usare per creare una macchina virtuale.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm-with-azure-powershell"></a>Creare una macchina virtuale Linux con Azure PowerShell

Poiché si sta usando l'ambiente sandbox di Azure, non è necessario creare un gruppo di risorse. Usare invece il gruppo di risorse **<rgn>[Nome gruppo di risorse sandbox]</rgn>**. Tenere anche presenti le restrizioni per la posizione.

È possibile creare una nuova macchina virtuale di Azure con PowerShell.

1. Usare il cmdlet `New-AzureRmVm` per creare una macchina virtuale.
    - Usare il gruppo di risorse **<rgn>[Nome gruppo di risorse sandbox]</rgn>**.
    - Assegnare un nome alla macchina virtuale: in genere si usa un nome significativo che identifichi gli scopi della macchina virtuale, la posizione e il numero di istanza (se ne esiste più di una). Si userà "testvm-eus-01" per "Test VM in East US, instance 1". Scegliere un nome appropriato in base a dove è stata posizionata la macchina virtuale.
    - Selezionare una località vicina dall'elenco seguente, disponibile nella sandbox di Azure. Assicurarsi di modificare il valore nel comando di esempio seguente, se si usa Copia e incolla.

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - Usare "UbuntuLTS" per l'immagine (Ubuntu Linux).
    - Usare il cmdlet `Get-Credential` e inviare i risultati nel parametro `Credential`.
    - Aggiungere il parametro `-OpenPorts` e passare "22" come porta. In questo modo sarà possibile connettersi con SSH alla macchina.
 
    ```powershell
    New-AzureRmVm -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -Name "testvm-eus-01" -Credential (Get-Credential) -Location "East US" -Image UbuntuLTS -OpenPorts 22
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
    
1. Questa operazione richiederà qualche minuto. Al termine, è possibile eseguire una query e assegnare l'oggetto macchina virtuale a una variabile (`$vm`).

    ```powershell
    $vm = Get-AzureRmVM -Name "testvm-eus-01" -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```
    
1. Eseguire quindi una query per ottenere il valore per eseguire il dump delle informazioni relative alla macchina virtuale:

    ```powershell
    $vm
    ```

    L'output dovrebbe essere simile al seguente:

    ```output
    ResourceGroupName : <rgn>[sandbox resource group name]</rgn>
    Id                : /subscriptions/xxxxxxxx-xxxx-aaaa-bbbb-cccccccccccc/resourceGroups/<rgn>[sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/testvm-eus-01
    VmId              : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    Name              : testvm-eus-01
    Type              : Microsoft.Compute/virtualMachines
    Location          : eastus
    Tags              : {}
    HardwareProfile   : {VmSize}
    NetworkProfile    : {NetworkInterfaces}
    OSProfile         : {ComputerName, AdminUsername, LinuxConfiguration, Secrets}
    ProvisioningState : Succeeded
    StorageProfile    : {ImageReference, OsDisk, DataDisks}
    ```
    
1. È possibile accedere a oggetti complessi tramite la sintassi con punto ("."). Ad esempio, per visualizzare le proprietà nell'oggetto `VMSize` associato alla sezione HardwareProfile è possibile digitare:

    ```powershell
    $vm.HardwareProfile
    ```

1. Oppure, per ottenere informazioni su uno dei dischi:

    ```powershell
    $vm.StorageProfile.OsDisk
    ```

1. È anche possibile passare l'oggetto macchina virtuale in altri cmdlet. Ad esempio, questo comando consentirà di recupererà l'indirizzo IP pubblico della macchina virtuale:

    ```powershell
    $vm | Get-AzureRmPublicIpAddress
    ```

1. Con l'indirizzo IP è possibile connettersi alla macchina virtuale con SSH. Ad esempio, se è stato usato il nome utente "bob" e l'indirizzo IP è "205.22.16.5", questo comando consente alla macchina Linux di connettersi:

    ```powershell
    ssh bob@205.22.16.5
    ```

    Disconnettersi quindi digitando `exit`.


## <a name="delete-a-vm"></a>Eliminare una macchina virtuale

Per provare alcuni altri comandi, si vedrà come eliminare la macchina virtuale. È prima di tutto necessario arrestarla.

```powershell
Stop-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

Eliminare quindi la macchina virtuale con il cmdlet `Remove-AzureRmVM`:

```powershell
Remove-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

Provare questo comando per elencare tutte le risorse nel gruppo di risorse:

```powershell
Get-AzureRmResource -ResourceGroupName $vm.ResourceGroupName | ft
```

Verrà visualizzata una serie di risorse (dischi, reti virtuali e così via) ancora esistenti. 

```output
Microsoft.Compute/disks
Microsoft.Network/networkInterfaces
Microsoft.Network/networkSecurityGroups
Microsoft.Network/publicIPAddresses
Microsoft.Network/virtualNetworks
```

Questo perché il comando `Remove-AzureRmVM` _elimina solo la macchina virtuale_ e non pulisce tutte le altre risorse. A questo punto, sarebbe probabilmente sufficiente eliminare il gruppo di risorse stesso per fare pulizia. Si vedrà invece come eseguire manualmente l'operazione eseguendo l'esercizio. Si noterà uno schema nei comandi.

1. Eliminare l'interfaccia di rete.

    ```powershell
    $vm | Remove-AzureRmNetworkInterface –Force
    ```
    
1. Eliminare i dischi del sistema operativo gestiti e l'account di archiviazione

    ```powershell
    Get-AzureRmDisk -ResourceGroupName $vm.ResourceGroupName -DiskName $vm.StorageProfile.OSDisk.Name | Remove-AzureRmDisk -Force
    ```

1. Eliminare poi la rete virtuale.

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmVirtualNetwork -Force
    ```

1. Eliminare il gruppo di sicurezza di rete.

    ```powershell
    Get-AzureRmNetworkSecurityGroup -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmNetworkSecurityGroup -Force
    ```

1. E infine l'indirizzo IP pubblico.

    ```powershell
    Get-AzureRmPublicIpAddress -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmPublicIpAddress -Force
    ```

In questo modo si dovrebbe essere riusciti a intercettare tutte le risorse create. Per essere sicuri, controllare il gruppo di risorse. Sono stati eseguiti numerosi comandi manuali, ma un approccio migliore sarebbe scrivere uno _script_ in modo da poter riutilizzare questa logica in un secondo momento per creare o eliminare una macchina virtuale. Si vedranno ora le tecniche di scripting con PowerShell.