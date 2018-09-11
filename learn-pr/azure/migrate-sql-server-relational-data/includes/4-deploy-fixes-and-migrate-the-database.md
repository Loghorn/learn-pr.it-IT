È stato valutato il database locale per verificarne la compatibilità e la parità delle funzionalità rispetto al database SQL di Azure. Perché la base clienti per l'azienda di vendita online di biciclette è limitata a una sola posizione geografica, è possibile portare offline il database durante un orario di scarso traffico, ad esempio nelle prime ore della mattina.

L'approccio migliore per la migrazione del database in questo scenario consiste nell'uso di Data Migration Assistant, che fornisce una procedura guidata che semplifica i passaggi della migrazione. In questa unità verrà descritto come Data Migration Assistant esegue la migrazione effettiva.

## <a name="migrate-the-database-using-data-migration-assistant"></a>Eseguire la migrazione del database usando Data Migration Assistant

Quando si usa Data Migration Assistant per eseguire la migrazione del database, scegliere un periodo di tempo in cui l'attività transazionale nel database è minima o del tutto assente.

Prima di avviare il processo di migrazione, correggere eventuali errori identificati durante la fase di valutazione. I report di valutazione forniscono i dettagli sulle correzioni da eseguire prima di procedere con la migrazione effettiva.

Per ridurre il tempo totale necessario per la migrazione, è possibile modificare il livello di prestazioni del database SQL di Azure di destinazione per la durata della migrazione. È possibile aumentare le prestazioni a un livello superiore, ad esempio P15, per ridurre il tempo di inattività causato dalla migrazione. Tuttavia, per ridurre i costi, assicurarsi di riportare il livello di prestazioni al valore precedente dopo la migrazione.

Il processo di migrazione effettivo include i passaggi seguenti:

1. Creazione di un database SQL di Azure vuoto.

1. Creazione di un nuovo progetto di migrazione.

1. Definizione dei server e dei database di origine e di destinazione.

1. Selezione degli oggetti di cui eseguire la migrazione. Non è necessario eseguire la migrazione di tutti gli oggetti, tuttavia è importante assicurarsi di non lasciare alcun oggetto dipendente. Se ad esempio si esegue la migrazione di una vista che accede a una tabella, assicurarsi di eseguire anche la migrazione della tabella.

1. Distribuzione dello schema. Durante questo passaggio viene eseguita la migrazione della struttura del database, ma non dei dati.

1. Migrazione dei dati. Durante questo passaggio, che è quello che richiede più tempo, viene eseguita la migrazione del contenuto delle tabelle nel database.
