Immaginiamo uno scenario in cui un salone di parrucchieri molto attivo riscontri un problema ricorrente: i clienti saltano comunemente agli appuntamenti. Gli appuntamenti sono fasce orarie riservate. Se un cliente perde un appuntamento, il salone perde denaro. Per risolvere questo problema, il salone si rivolge a uno sviluppatore di software. Per migliorare il problema, decide di inviare messaggi di testo di promemoria. È stato possibile inviare, non appena l'appuntamento viene pianificata o modificato e ogni mattina, si invieremo un messaggio di testo per ogni cliente con un appuntamento nello stesso giorno.

È necessario creare un servizio che può essere facilmente pianificato, aggiornato e ridimensionato. Si decide di risolvere questo problema usando un'app per le funzioni. Si conosce già il modo di implementare la logica per inviare un messaggio di testo. Si verifica a questo punto è necessario imparare a inviare il messaggio in un momento specifico o quando un evento specifico. Per fortuna, le funzioni di Azure supportano una funzionalità denominata _trigger_. I trigger vengono usati per determinare la modalità di esecuzione di una funzione di Azure.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:
- Determinare quali trigger risulta più adatta alle esigenze aziendali
- Creare un trigger timer per richiamare una funzione in una pianificazione coerente
- Creare un trigger HTTP per richiamare una funzione quando viene ricevuta una richiesta HTTP
- Creare un trigger di blob per richiamare una funzione quando un blob viene creato o aggiornato in archiviazione di Azure