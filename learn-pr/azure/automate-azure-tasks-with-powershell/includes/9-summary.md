In questo modulo, è stato scritto uno script per automatizzare la creazione di più macchine virtuali. Anche se lo script è relativamente breve, è possibile avere un'idea delle avanzate funzionalità disponibili combinando i cicli, le variabili e le funzioni di PowerShell con i cmdlet di Azure PowerShell.

Azure PowerShell è una scelta ottimale di automazione per gli amministratori che hanno esperienza nell'uso di PowerShell. La combinazione di una sintassi pulita e un potente linguaggio di scripting rende questa soluzione adatta anche per chi non ha familiarità con PowerShell. Questo livello di automazione per attività soggette a errori e che richiedono molto tempo consente di ridurre i tempi di amministrazione e migliorare la qualità.

## <a name="clean-up"></a>Eseguire la pulizia
<!---TODO: Update for sandbox?--->

Le macchine virtuali di cui è stato effettuato il provisioning e in esecuzione comportano l'addebito di costi per la sottoscrizione. È consigliabile rimuovere le macchine virtuali non necessarie per evitare addebiti non necessari. Il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse associato. Verranno così eliminate anche tutte le macchine virtuali nel gruppo. È possibile eseguire questa operazione con PowerShell. Al termine, eseguire il cmdlet seguente di Azure PowerShell:

```powershell
Remove-AzureRmResourceGroup -Name TrialsResourceGroup
```

Quando viene richiesto di confermare l'eliminazione, rispondere **Sì**. Il completamento del comando può richiedere alcuni minuti.