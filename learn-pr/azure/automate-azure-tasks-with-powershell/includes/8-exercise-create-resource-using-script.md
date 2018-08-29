In questo esercizio, si continuerà con l'esempio di un'azienda che sviluppa strumenti di amministrazione per Linux. È importante ricordare che si prevede di usare macchine virtuali Linux per consentire ai potenziali clienti di testare il software. È stato predisposto un gruppo di risorse e a questo punto è possibile creare le macchine virtuali.

L'azienda presso cui si lavora prevede di organizzare uno stand nel corso una grande fiera dedicata a Linux. È stata pianificata un'area demo con tre terminali, ognuno connesso a una macchina virtuale Linux distinta. Alla fine di ogni giornata, si vuole eliminare e ricreare le macchine virtuali, in modo che ogni mattina siano nuove. Ricreare manualmente le macchine virtuali dopo una giornata di lavoro potrebbe dare luogo a errori. Si vuole scrivere uno script di PowerShell per automatizzare il processo di creazione delle macchine virtuali.

## <a name="write-a-script-that-creates-virtual-machines"></a>Scrivere uno script per la creazione delle macchine virtuali

Per scrivere lo script, seguire questa procedura:

1. Creare un nuovo file di testo denominato **ConferenceDailyReset.ps1**.

2. Acquisire il parametro in una variabile:

    ```powershell
    param([string]$resourceGroup)
    ```

3. Eseguire l'autenticazione in Azure con le proprie credenziali:

    ```powershell
    Connect-AzureRmAccount
    ```

4. Richiedere il nome utente e la password per l'account di amministratore della macchina virtuale e acquisire il risultato in una variabile:

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

5. Creare un ciclo da eseguire tre volte:

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

6. Nel corpo del ciclo, creare un nome per ogni macchina virtuale e archiviarlo in una variabile:

    ```powershell
    $vmName = "ConferenceDemo" + $i
    ```

7. Creare quindi una macchina virtuale usando la variabile `$vmName`:

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" 
   ```

8. Salvare il file.

Lo script completato avrà l'aspetto seguente:

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

Connect-AzureRmAccount

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i

    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a>Eseguire lo script

Avviare PowerShell e passare alla directory in cui è stato salvato il file di script. Per eseguire lo script, eseguire il comando seguente:

```powershell
.\ConferenceDailyReset.ps1 TrialsResourceGroup
```

Per il completamento dello script possono essere necessari alcuni minuti. Al termine, verificare che sia stato eseguito correttamente:

1. Da un browser accedere al portale di Azure.
2. Nel riquadro di spostamento a sinistra fare clic su **Gruppi di risorse**.
3. Nell'elenco dei gruppi di risorse fare clic su **TrialsResourceGroup**. Nell'elenco delle risorse dovrebbero essere visibili le macchine virtuali appena create e le risorse associate.

## <a name="summary"></a>Riepilogo
È stato scritto uno script per automatizzare la creazione di tre macchine virtuali nel gruppo di risorse indicato da un parametro dello script. Lo script è breve e semplice ma consente di automatizzare un processo il cui completamento manuale tramite il portale richiederebbe molto tempo.