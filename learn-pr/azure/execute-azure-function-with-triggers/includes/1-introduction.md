Immaginiamo uno scenario in cui un salone di parrucchieri molto attivo riscontri un problema ricorrente: i clienti saltano comunemente agli appuntamenti. Gli appuntamenti sono fasce orarie riservate. Se un cliente perde un appuntamento, il salone perde denaro. Per risolvere questo problema, il salone si rivolge a uno sviluppatore di software. Per migliorare il problema, si decide di inviare messaggi di testo di promemoria, che possono essere inviati non appena l'appuntamento viene pianificato o modificato. In alternativa, ogni mattina è possibile inviare un messaggio di testo a tutti i clienti che hanno un appuntamento per quel giorno.

È necessario creare un servizio che possa essere facilmente pianificato, aggiornato e ridimensionato. Si decide quindi di risolvere il problema usando un'app per le funzioni. Si conosce già la procedura per implementare la logica per l'invio di un messaggio di testo. È necessario ora imparare a inviare il messaggio in un momento specifico o quando si verifica un evento. Per fortuna, Funzioni di Azure supporta una funzionalità denominata _trigger_. I trigger vengono usati per determinare la modalità di esecuzione di una funzione di Azure.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:
- Determinare quali trigger risultano più adatti alle esigenze aziendali
- Creare un trigger timer per richiamare una funzione in base a una pianificazione regolare
- Creare un trigger HTTP per richiamare una funzione quando viene ricevuta una richiesta HTTP
- Creare un trigger del BLOB per richiamare una funzione quando viene creato o aggiornato un BLOB in Archiviazione di Azure.