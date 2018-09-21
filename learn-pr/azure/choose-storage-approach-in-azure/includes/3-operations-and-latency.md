Dopo l'identificazione del tipo di dati da gestire (strutturati, semistrutturati o non strutturati), il passaggio successivo consiste nel determinare come si useranno i dati. Un rivenditore online, ad esempio, sa che i clienti devono poter accedere rapidamente ai dati dei prodotti e che gli utenti aziendali devono eseguire query analitiche complesse. Durante l'esame di questi requisiti, tenendo conto della classificazione dei dati, si può iniziare a pianificare una soluzione di archiviazione dei dati.

Di seguito sono illustrate alcune domande da porsi al momento di determinare come verranno usati i dati.

## <a name="operations-and-latency"></a>Operazioni e latenza

Quali sono le principali operazioni che verranno eseguite su ogni tipo di dati e quali sono i requisiti relativi alle prestazioni?

Le domande da porsi sono:

* Verranno eseguite semplici ricerche usando un ID?
* È necessario eseguire query sul database per uno o più campi?
* Quante operazioni di creazione, aggiornamento ed eliminazione si prevede che verranno eseguite?
* È necessario eseguire query analitiche complesse?
* Con quale velocità devono essere completate queste operazioni?

Tenendo a mente queste domande verranno ora analizzati i singoli set di dati e discussi i requisiti.

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

Per i dati del catalogo prodotti nello scenario di vendita online, le esigenze del cliente costituiscono la massima priorità. I clienti vorranno eseguire query sul catalogo prodotti per trovare, ad esempio, tutte le scarpe da uomo, quindi tutte le scarpe da uomo in saldo e infine le scarpe da uomo in saldo di una misura specifica. Per soddisfare le esigenze del cliente possono quindi essere necessarie numerose operazioni di lettura, con la possibilità di eseguire query su alcuni campi.

Quando i clienti effettuano gli ordini, inoltre, l'applicazione deve aggiornare le quantità del prodotto. Le operazioni di aggiornamento devono essere rapide quanto le operazioni di lettura, in modo tale che gli utenti non possano inserire nel carrello un articolo appena esaurito. Questo non solo comporterà un gran numero di operazioni di lettura, ma richiederà anche un aumento delle operazioni di scrittura per i dati del catalogo prodotti. Assicurarsi di determinare le priorità di tutti gli utenti del database, non solo di quelli principali.

## <a name="photos-and-videos"></a>Foto e video

Le foto e i video visualizzati nelle pagine dei prodotti hanno requisiti diversi. Necessitano di tempi di recupero rapidi in modo che siano visualizzati nel sito contemporaneamente ai dati del catalogo prodotti, ma non è necessario poter eseguire query indipendenti su di essi. È invece possibile fare affidamento sui risultati della query del prodotto e includere l'ID o l'URL del video come proprietà nei dati del prodotto. Foto e video devono quindi essere solo recuperati in base all'ID.

Gli utenti non apporteranno inoltre aggiornamenti alle foto o ai video esistenti. È tuttavia possibile che aggiungano nuove foto per le recensioni dei prodotti. Un utente potrebbe ad esempio caricare una foto in cui indossa le nuove scarpe. I dipendenti, inoltre, caricano ed eliminano le foto dei prodotti messe a disposizione dal fornitore, ma questo tipo di aggiornamento non richiede la stessa velocità degli aggiornamenti di altri dati dei prodotti. 

In sintesi, è possibile eseguire query su foto e video in base all'ID per recuperare l'intero file, ma le operazioni di creazione e aggiornamento saranno meno frequenti e avranno una priorità inferiore.  

## <a name="business-data"></a>Dati di business

Per i dati di business, le analisi vengono eseguite sui dati cronologici. I dati originali non vengono aggiornati in base all'analisi, quindi i dati di business sono di sola lettura. Gli utenti non si aspettano che le analisi complesse vengano eseguite istantaneamente, quindi una certa latenza nei risultati è tollerabile. I dati di business verranno inoltre archiviati in più set di dati poiché gli utenti avranno livelli di accesso diversi per scrivere in ogni set di dati. I business analyst avranno tuttavia l'accesso in lettura a tutti i database.

## <a name="summary"></a>Riepilogo

Quando si decide quale soluzione di archiviazione usare, riflettere su come verranno usati i dati. Con quale frequenza verranno usati i dati? I dati sono di sola lettura? Il tempo di query è importante? Le risposte a queste domande aiuteranno a scegliere la soluzione di archiviazione più adatta per i dati.