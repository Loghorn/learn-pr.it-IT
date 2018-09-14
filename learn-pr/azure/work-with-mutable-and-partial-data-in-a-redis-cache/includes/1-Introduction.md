Immagini di che compilare un'applicazione per dispositivi mobili di messaggistica immediata. L'applicazione consente agli utenti di inviare messaggi a tutti i membri di un gruppo specifico definito dall'utente. Ci sono alcuni dati che devono essere archiviati su ciascun utente, ad esempio il nome utente, indirizzo di posta elettronica e la password. Pertanto, si decide di usare SQL Server per creare un database relazionale per tali informazioni. Tuttavia, i messaggi stessi devono essere inviate e accedere rapidamente ai e un database relazionale è troppo lento adatto.

Si decide di creare una Cache Redis di Azure a causa del numero dei suoi vantaggi. Ad esempio, si userà le transazioni per garantire un messaggio con un'immagine e testo vengono inviate insieme. Scadenza dei dati si userà per reimpostare il nome della chat di gruppo dopo un'ora. Infine, si userà criteri di rimozione per verificare che i messaggi meno recenti vengono eliminati prima di tutto se sta per esaurirsi la memoria.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Raggruppare più operazioni in una transazione
- Impostare una scadenza sui dati
- Gestire le condizioni di memoria insufficiente
- Usare il modello cache-aside
