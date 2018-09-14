Hub eventi di Azure consente alle applicazioni Big Data di elaborare volumi elevati di dati. Offre anche la possibilità di scalare orizzontalmente il sistema durante i periodi in cui la richiesta è estremamente elevata, in base alle esigenze. Hub eventi di Azure separa l'invio e la ricezione dei messaggi per gestire l'elaborazione dei dati. Questo consente di eliminare il rischio di sovraccaricare l'applicazione consumer e di subire perdite di dati a causa di interruzioni non pianificate.

In questo modulo è stato descritto come distribuire Hub eventi di Azure nell'ambito di una soluzione per l'elaborazione di eventi. Si è appreso come:

- Usare i comandi dell'interfaccia della riga di comando di Azure per creare uno spazio dei nomi di Hub eventi e un hub eventi nello spazio dei nomi. 
- Configurare le applicazioni mittente e ricevitore per l'invio e la ricezione dei messaggi tramite l'hub eventi.
- Usare il portale di Azure per visualizzare lo stato e le prestazioni dell'hub eventi.

## <a name="clean-up"></a>Eseguire la pulizia 

Le risorse usate per i test degli hub eventi comportano l'addebito di costi per la sottoscrizione. Quando l'hub eventi non è più necessario, ricordarsi di rimuovere le risorse per evitare addebiti inutili.

Poiché l'hub, lo spazio dei nomi e la risorsa di archiviazione sono stati creati nello stesso gruppo di risorse, il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse. Verranno così eliminati tutti i contenuti. 

Eseguire il comando seguente per rimuovere il gruppo di risorse, lo spazio dei nomi, l'account di archiviazione e tutte le risorse correlate. Sostituire `myResourceGroup` con il nome del gruppo di risorse creato:

```azurecli
az group delete --resource-group myResourceGroup
```

Quando viene richiesto di confermare l'eliminazione, rispondere **Sì**.

Il completamento di questo comando può richiedere alcuni minuti mentre vengono eliminate le risorse.
