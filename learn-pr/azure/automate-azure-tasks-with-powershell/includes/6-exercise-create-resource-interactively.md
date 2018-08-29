Si supponga di lavorare presso una società che sviluppa una suite di strumenti di amministrazione per Linux. La propria mansione è aiutare i potenziali clienti a provare il software prima dell'acquisto. Poiché il software apporta modifiche al sistema operativo a livello di radice, si è deciso di creare una macchina virtuale Linux per ogni cliente con una versione di valutazione. Le macchine virtuali vengono create in base alle esigenze ed eliminate al termine della sottoscrizione di valutazione. In questo modo, ogni cliente inizia con una versione pulita del sistema operativo. 

Per mantenere queste macchine virtuali separate dalle macchine virtuali usate nell'azienda per i test interni, verrà creato un gruppo di risorse dedicato per ospitarle. Poiché è necessario un solo gruppo di risorse, l'uso di Azure PowerShell in modalità interattiva rappresenta una scelta ragionevole per questa attività.

## <a name="steps-to-create-a-resource-group"></a>Procedura per creare un gruppo di risorse

1. Avviare PowerShell.

1. Importare il modulo nella sessione corrente, in modo da avere accesso ai cmdlet di Azure.

   ```powershell
   Import-Module AzureRM
   ```

1. Collegarsi ad Azure usando il comando seguente. Dopo aver immesso il comando, eseguire l'autenticazione fornendo le credenziali di Azure.

   ```powershell
   Connect-AzureRmAccount
   ```

1. Creare un gruppo di risorse.

    ```powershell
    New-AzureRmResourceGroup -Name "TrialsResourceGroup" -Location "East US"
    ```

1. Verificare che il gruppo di risorse sia stato creato correttamente.

    ```powershell
    Get-AzureRmResource | Format-Table
    ```
Un altro modo per verificare se il gruppo di risorse è stato creato correttamente è usare il portale di Azure. A tale scopo, accedere al portale e passare alla sezione **Gruppi di risorse** (vedere di seguito). Il nuovo gruppo di risorse dovrebbe essere visualizzato nell'elenco.

![Uso del portale per elencare i gruppi di risorse](../media-drafts/6-listing-resource-groups.png)

## <a name="summary"></a>Riepilogo
In questo esercizio viene illustrato un modello comune per una sessione di PowerShell interattiva. Sono stati usati un cmdlet standard per importare il modulo AzureRM, quindi i cmdlet di Azure PowerShell per eseguire un'attività specifica. A questo punto, è disponibile un gruppo di risorse nella sottoscrizione e si è pronti per creare le macchine virtuali.