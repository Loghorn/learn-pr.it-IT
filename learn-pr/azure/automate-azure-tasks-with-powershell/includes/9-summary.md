In questo modulo, è stato scritto uno script per automatizzare la creazione di più macchine virtuali. Anche se lo script è relativamente breve, è possibile avere un'idea delle avanzate funzionalità disponibili combinando i cicli, le variabili e le funzioni di PowerShell con i cmdlet di Azure PowerShell.

Azure PowerShell è una scelta ottimale di automazione per gli amministratori che hanno esperienza nell'uso di PowerShell. La combinazione di una sintassi pulita e un potente linguaggio di scripting rende questa soluzione adatta anche per chi non ha familiarità con PowerShell. Questo livello di automazione per attività soggette a errori e che richiedono molto tempo consente di ridurre i tempi di amministrazione e migliorare la qualità.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

Quando si usa la propria sottoscrizione, è possibile eseguire il cmdlet PowerShell seguente per eliminare il gruppo di risorse e tutte le risorse correlate.

```powershell
Remove-AzureRmResourceGroup -Name MyResourceGroupName
```

Alla richiesta di conferma dell'eliminazione, selezionare **Sì**. In alternativa, è possibile aggiungere il parametro `-Force` per ignorare il prompt. Il completamento del comando può richiedere alcuni minuti.