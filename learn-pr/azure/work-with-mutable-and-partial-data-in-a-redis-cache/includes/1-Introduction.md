Si immagini di dover creare un'applicazione per dispositivi mobili di messaggistica istantanea. L'applicazione consente agli utenti di inviare messaggi a tutti i membri di un gruppo specifico definito dall'utente. Ci sono alcuni dati che devono essere archiviati su ogni utente, come nome utente, indirizzo di posta elettronica e password. Pertanto, si decide di usare SQL Server per creare un database relazionale per tali informazioni. Tuttavia, i messaggi stessi devono essere inviati e disponibili per l'accesso rapidamente e un database relazionale è troppo lento per questi scopi.

Si decide di creare un'istanza di Cache Redis di Azure in considerazione dei numerosi vantaggi offerti. Come illustrato nel modulo **Ottimizzare le applicazioni Web con la memorizzazione nella cache dei dati di sola lettura con Redis**, una cache Redis è un archivio di dati in memoria che è possibile usare come database, cache o broker di messaggi.

Con Cache Redis di Azure è possibile usare le transazioni per assicurarsi che un'immagine e il testo di un messaggio vengano inviati insieme. Usare la scadenza dei dati per reimpostare il nome della chat di gruppo dopo un'ora. Infine, usare i criteri di rimozione per assicurarsi che i messaggi meno recenti vengano eliminati per primi quando si sta esaurendo la memoria.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Raggruppare più operazioni in una transazione
- Impostare un'ora di scadenza per i dati
- Gestire condizioni di memoria insufficiente
- Usare il modello cache-aside
- Usare il pacchetto ServiceStack.Redis in un'applicazione console .NET Core