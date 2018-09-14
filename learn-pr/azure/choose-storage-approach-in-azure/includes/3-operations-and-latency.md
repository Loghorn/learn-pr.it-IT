Dopo aver identificato il tipo di dati ha a che fare con (strutturati, semistrutturati o non strutturati), il passaggio successivo è stabilire come si userà i dati. Ad esempio, come un rivenditore online conosci i clienti desiderano accedere rapidamente ai dati dei prodotti e gli utenti di business necessaria eseguire query analitiche complesse. Durante l'utilizzo di tali requisiti, richiede la classificazione dei dati nell'account, iniziare a pianificare la soluzione di archiviazione dei dati.

In questo caso, verrà esaminato alcune delle domande da porre quando determinare cosa fare con i dati.

## <a name="operations-and-latency"></a>Operazioni e latenza

Quali sono le principali operazioni che sarà completato per ogni tipo di dati e quali sono i requisiti di prestazioni?

È opportuno porsi le seguenti domande:
* È necessario eseguire ricerche semplici usando un ID? 
* È necessario eseguire query sul database per uno o più campi? 
* Quante operazioni di creazione, aggiornamento ed eliminazione si prevede che verranno eseguite? 
* È necessario eseguire query analitiche complesse? 
* Rapidità con cui queste operazioni sono necessario completare?

Verrà ora illustrato ognuno dei set di dati con queste domande presente e discutere i requisiti.

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

Per i dati del catalogo di prodotti nello scenario di vendita online, le esigenze dei clienti sono la priorità più alta. I clienti dovranno eseguire una query del catalogo di prodotti per trovare, ad esempio, tutti calzature, quindi calzature in vendita e quindi calzature in vendita in una determinata dimensione. Pertanto, le esigenze del cliente possono richiedere un numero elevato di operazioni di lettura, con la possibilità di eseguire query su determinati campi.

Inoltre, quando i clienti effettuano ordini, l'applicazione deve aggiornare quantità di prodotto. Le operazioni di aggiornamento devono essere eseguite solo più rapidamente le operazioni di lettura in modo che gli utenti non inserire un elemento nel carrello acquisti quando tale elemento ha appena esauriti. Non solo sono quindi necessarie numerose operazioni di lettura, ma anche numerose operazioni di scrittura per i dati nel catalogo prodotti. Assicurarsi di determinare le priorità di tutti gli utenti del database, non solo di quelli principali.

## <a name="photos-and-videos"></a>Foto e video

La foto e video che vengono visualizzati nelle pagine di prodotto hanno requisiti diversi. Sono necessari tempi di recupero rapidi in modo che vengono visualizzati nel sito nello stesso momento come dati del catalogo prodotti, ma non devono essere sottoposti a query in modo indipendente. Al contrario, è possibile fare affidamento sui risultati della query del prodotto e includono l'ID o l'URL video come proprietà nei dati del prodotto. Pertanto, foto e video devono essere recuperati solo dal rispettivo ID.

Inoltre, gli utenti non apporterà aggiornamenti a esistenti foto o video. È tuttavia possibile che gli utenti aggiungano nuove foto per le recensioni dei prodotti. Ad esempio, un utente può caricare un'immagine di se stessi valorizzazione suoi panni di nuovo. Come dipendente, anche caricare ed eliminare foto del prodotto del fornitore, ma tale aggiornamento non deve essere eseguita più velocemente di altri aggiornamenti del prodotto data. 

In sintesi, foto e video è possibile eseguire query dall'ID per restituire l'intero file, ma consente di creare e degli aggiornamenti saranno meno frequenti e sono minori di priorità.  

## <a name="business-data"></a>Dati aziendali

Per i di business, le analisi vengono eseguite sui dati cronologici. Nessun dato originale viene aggiornato in base all'analisi, in modo che i dati aziendali sono di sola lettura. Non si aspettano loro analitica complessa per l'esecuzione immediatamente, quindi è bene disporre di una certa latenza nei risultati. I dati di business verranno inoltre archiviati in più set di dati, in modo che gli utenti abbiano livelli di accesso diversi per la scrittura in ogni set di dati, Tuttavia, gli analisti aziendali sarà in grado di leggere da tutti i database.

## <a name="summary"></a>Riepilogo

Quando si decide quale soluzione di archiviazione da usare, considerare come verranno usati i dati. Con che frequenza verrà eseguito l'accesso ai dati? I dati sono di sola lettura? Il tempo di query è importante? Le risposte a queste domande aiuteranno a scegliere la soluzione di archiviazione più adatta per i dati.