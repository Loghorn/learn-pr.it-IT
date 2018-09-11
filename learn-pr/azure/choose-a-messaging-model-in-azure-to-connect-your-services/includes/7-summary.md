In questo modulo sono stati esaminati quattro diversi servizi di Azure che consentono di creare applicazioni distribuite affidabili e resilienti. Per sceglierne uno, è necessario decidere il tipo di dati che devono essere passati tra i componenti (messaggi o eventi) e quindi le funzionalità necessarie per recapitare ed elaborare i dati.

## <a name="clean-up"></a>Eseguire la pulizia

Quando un account di archiviazione contiene dati, nella sottoscrizione di Azure vengono addebitati i costi corrispondenti, anche se probabilmente sono ridotti per le code di piccole dimensioni con pochi messaggi. Quando la coda non è più necessaria, ricordarsi di rimuoverla per evitare addebiti inutili. Poiché tutte le risorse sono state create nello stesso gruppo di risorse, il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse. Verranno così eliminati tutti i contenuti:

```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```

Quando viene richiesto di confermare l'eliminazione, rispondere **Sì**.

Il completamento di questo comando può richiedere alcuni minuti mentre vengono eliminate le risorse.