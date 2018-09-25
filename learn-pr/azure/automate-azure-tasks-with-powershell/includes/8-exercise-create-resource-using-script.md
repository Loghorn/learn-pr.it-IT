In questa unità si continuerà con l'esempio di un'azienda che sviluppa strumenti di amministrazione per Linux. È importante ricordare che si prevede di usare macchine virtuali Linux per consentire ai potenziali clienti di testare il software. È stato predisposto un gruppo di risorse e a questo punto è possibile creare le macchine virtuali.

L'azienda presso cui si lavora prevede di organizzare uno stand nel corso una grande fiera dedicata a Linux. È stata pianificata un'area demo con tre terminali, ognuno connesso a una macchina virtuale Linux distinta. Alla fine di ogni giornata, si vuole eliminare e ricreare le macchine virtuali, in modo che ogni mattina siano nuove. Ricreare manualmente le macchine virtuali dopo una giornata di lavoro potrebbe dare luogo a errori. Si vuole scrivere uno script di PowerShell per automatizzare il processo di creazione delle macchine virtuali.

## <a name="write-a-script-that-creates-virtual-machines"></a>Scrivere uno script per la creazione delle macchine virtuali

Seguire questi passaggi in Cloud Shell a destra per scrivere lo script:

1. Passare alla cartella principale in Cloud Shell.

    ```powershell
    cd $HOME\clouddrive
    ```

1. Creare un nuovo file di testo denominato **ConferenceDailyReset.ps1**.

    ```powershell
    touch "./ConferenceDailyReset.ps1"
    ```

1. Aprire l'editor integrato e selezionare il file **ConferenceDailyReset.ps1**.

    ```powershell
    code "./ConferenceDailyReset.ps1"
    ```
    > [!TIP]
    > Il servizio Cloud Shell integrato supporta anche vim, nano ed emacs, se si preferisce usare uno di questi editor.

1. Per iniziare, acquisire il parametro di input in una variabile. Aggiungere la riga seguente allo script:

    ```powershell
    param([string]$resourceGroup)
    ```

    > [!NOTE]
    > In genere, è necessario eseguire l'autenticazione con Azure usando le credenziali con `Connect-AzureRmAccount` e questa operazione può essere eseguita nello script. Tuttavia, nell'ambiente Cloud Shell l'utente è già autenticato quindi l'operazione non è necessaria.

1. Richiedere il nome utente e la password per l'account di amministratore della macchina virtuale e acquisire il risultato in una variabile:

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. Creare un ciclo da eseguire tre volte:

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. Nel corpo del ciclo creare un nome per ogni macchina virtuale, archiviarlo in una variabile e inviarlo alla console:

    ```powershell
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    ```

1. Creare quindi una macchina virtuale usando la variabile `$vmName`:

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
   ```

1. Salvare il file. È possibile usare il menu "..." nell'angolo superiore destro dell'editor. Sono disponibili anche i tasti di scelta rapida comuni per salvare.

Lo script completato avrà l'aspetto seguente:

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a>Eseguire lo script

Chiudere l'editor usando il menu di scelta rapida "...".

1. Eseguire lo script.

    ```powershell
    .\ConferenceDailyReset.ps1 <rgn>[sandbox resource group name]</rgn>
    ```
    
Il completamento dello script può richiedere alcuni minuti. Al termine, verificare se è stato eseguito correttamente esaminando le risorse che ora sono disponibili nel gruppo di risorse:

```powershell
Get-AzureRmResource -ResourceType Microsoft.Compute/virtualMachines
```

Dovrebbero essere visualizzate tre macchine virtuali, ognuna con un nome univoco.

È stato scritto uno script per automatizzare la creazione di tre macchine virtuali nel gruppo di risorse indicato da un parametro dello script. Lo script è breve e semplice ma consente di automatizzare un processo il cui completamento manuale tramite il portale richiederebbe molto tempo.