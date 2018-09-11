Immaginiamo uno scenario in cui un salone di parrucchieri molto attivo riscontri un problema ricorrente: i clienti saltano comunemente agli appuntamenti. Gli appuntamenti sono fasce orarie riservate. Se un cliente perde un appuntamento, il salone perde denaro. Per risolvere questo problema, il salone si rivolge a uno sviluppatore di software. Per migliorare il problema, decide di inviare messaggi di testo di promemoria. Ogni mattina, viene inviato un SMS a tutti i clienti che hanno un appuntamento quel giorno.

Lo sviluppatore Azure decide di risolvere questo problema usando una funzione di Azure. Si conosce già il modo di implementare la logica per inviare un messaggio di testo. A questo punto è necessario avere informazioni su come inviare il messaggio in un momento specifico. Per fortuna, le funzioni di Azure supportano una funzionalità denominata _trigger_. I trigger vengono usati per determinare la modalità di esecuzione di una funzione di Azure.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Determinare quale trigger risulta più adatto alle esigenze aziendali.
- Creare un trigger timer per richiamare una funzione in una pianificazione coerente.
- Creare un trigger HTTP per richiamare una funzione quando viene ricevuta una richiesta HTTP.
- Creare un trigger del BLOB per richiamare una funzione quando viene creato o aggiornato un BLOB nell'archivio di Azure.
