Sono stati descritti due modi per soddisfare la richiesta usando le macchine virtuali. Se il carico è prevedibile, è possibile usare il portale o gli script per ridimensionare manualmente le macchine virtuali. Per i modelli di domanda imprevedibile, un approccio migliore consiste nell'usare dei set di scalabilità con la scalabilità automatica per aggiungere e rimuovere automaticamente le istanze in base alle fluttuazioni della domanda. Più macchine virtuali offrono il vantaggio di aumentare la disponibilità del sistema dato che una macchina virtuale non funzionante non interromperà il servizio.

## <a name="cleanup"></a>Pulizia

L'esecuzione e il provisioning di una macchina virtuale comportano l'addebito di costi per la sottoscrizione. Poiché tutte le macchine virtuali sono state create nello stesso gruppo di risorse, il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse. Verranno così eliminati tutti i contenuti. Al termine dell'esercizio eseguire questo cmdlet di PowerShell:

   ```powershell
   Remove-AzureRmResourceGroup -Name ExerciseRG
   ```

Quando viene richiesto di confermare l'eliminazione, rispondere **Sì**. Il completamento di questo comando può richiedere alcuni minuti mentre vengono eliminate le risorse.