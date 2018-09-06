Una volta identificato il tipo di dati disponibili (strutturati, semistrutturati o non strutturati), l'altro passaggio importante consiste nel determinare come si useranno i dati. L'obiettivo di un rivenditore online, ad esempio, potrebbe essere quello di consentire ai clienti di accedere rapidamente ai dati dei prodotti e agli utenti aziendali di eseguire query analitiche complesse. Iniziando a esaminare questi requisiti e a prendere in considerazione la classificazione dei dati, è possibile iniziare a delineare un approccio all'archiviazione dei dati.

Di seguito sono illustrate alcune domande che è necessario porsi per determinare come verranno usati i dati.

## <a name="operations-and-latency"></a>Operazioni e latenza

Quali sono le principali operazioni che verranno eseguite su ogni tipo di dati e quali sono i requisiti relativi alle prestazioni?

In particolare, verranno eseguite ricerche semplici in base a un ID? È necessario eseguire query sul database per uno o più campi? Quante operazioni di creazione, aggiornamento ed eliminazione si prevede che verranno eseguite? È necessario eseguire query analitiche complesse? Con che velocità devono essere completate queste operazioni?

Verranno ora analizzati i singoli set di dati e i requisiti.

### <a name="product-catalog-data"></a>Dati del catalogo prodotti

In uno scenario di vendita online, la massima priorità degli utenti riguarderà i dati del catalogo prodotti. Gli utenti dovranno eseguire query sul catalogo prodotti per trovare, ad esempio, tutte le scarpe da uomo in saldo oppure le scarpe da uomo di una misura specifica. Si potrebbe pertanto stabilire che le esigenze dei clienti richiedono numerose operazioni di lettura, con la possibilità di eseguire query su determinati campi.

Quando i clienti effettuano gli ordini, inoltre, l'applicazione deve aggiornare le quantità dei prodotti. Tali operazioni di aggiornamento devono avvenire alla stessa velocità delle operazioni di lettura, affinché gli utenti non possano inserire articoli nel carrello quando il prodotto è appena stato esaurito. Non solo sono quindi necessarie numerose operazioni di lettura, ma anche numerose operazioni di scrittura per i dati nel catalogo prodotti. Assicurarsi di determinare le priorità di tutti gli utenti del database, non solo di quelli principali.

### <a name="photos-and-videos"></a>Foto e video

Le foto e i video visualizzati nelle pagine dei prodotti hanno tuttavia requisiti diversi. Richiedono tempi di recupero rapidi per la visualizzazione nel sito, ma non devono essere sottoposti a query in modo indipendente. È invece possibile fare affidamento sulla query sul prodotto e impostare solo l'ID o l'URL del video come proprietà dei dati del prodotto. Pertanto, le query su foto e video non saranno basate su altri elementi oltre all'ID.

Gli utenti, inoltre, non aggiorneranno le foto o i video esistenti, ma potrebbero aggiungere nuove foto per la recensione del prodotto. Potrebbero quindi caricare un'immagine per mostrare le loro nuove scarpe. È anche necessario caricare ed eliminare le foto dei prodotti del fornitore, ma non è necessario che tale aggiornamento venga eseguito alla stessa velocità degli altri aggiornamenti dei dati dei prodotti. Riepilogando, quindi, le query su foto e video devono essere eseguite solo in base all'ID per restituire l'intero file, mentre le operazioni di creazione e aggiornamento saranno meno frequenti e avranno una priorità inferiore.  

### <a name="business-data"></a>Dati di business

Per i di business, le analisi vengono eseguite sui dati cronologici. I dati originali non vengono aggiornati in base all'analisi. I dati di business sono quindi di sola lettura e gli utenti non si aspettano che le loro analisi complesse vengano eseguite immediatamente, quindi una certa latenza nei risultati è tollerabile. I dati di business verranno inoltre archiviati in più set di dati, in modo che gli utenti abbiano livelli di accesso diversi per la scrittura in ogni set di dati, mentre i business analyst potranno leggere in tutti i database.

## <a name="summary"></a>Riepilogo

Per scegliere la soluzione di archiviazione da usare, è necessario pensare a come verranno usati i dati. Con che frequenza verrà eseguito l'accesso ai dati? I dati sono di sola lettura? Il tempo di query è importante? Le risposte a queste domande aiuteranno a scegliere la soluzione di archiviazione più adatta per i dati.

