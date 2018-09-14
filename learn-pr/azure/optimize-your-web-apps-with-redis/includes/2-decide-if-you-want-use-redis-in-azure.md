Dietro le quinte di un sito Web sportivo vi è un database, il quale restituisce i dati eseguendo query. Tuttavia, le prestazioni rallenta quando il carico è elevato, in particolare durante eventi sportivi di grandi dimensioni. Negli ambienti ospitati, un aumento nell'uso delle risorse si traduce in costi più elevati. La memorizzazione nella cache dei dati garantisce che il sito Web offra prestazioni adeguate a un costo ridotto.

## <a name="what-is-caching"></a>Che cos'è la memorizzazione nella cache?

La memorizzazione nella cache è l'atto di archiviare i dati utilizzati di frequente in memoria che è molto vicino all'applicazione che utilizza i dati. La memorizzazione nella cache consente di migliorare le prestazioni e ridurre il carico sui server. Utilizziamo Redis per creare una cache in memoria che può offrire una latenza eccellente e potenziale miglioramento delle prestazioni.

## <a name="what-is-a-redis-cache"></a>Che cos'è una cache Redis?

La cache Redis (**RE**mote **DI**ctionary **S**erver) è un archivio open source di coppie di valori chiave nella memoria. È comune perché è veloce e può archiviare e modificare tipi di dati comuni, ad esempio stringhe, hash e set. Viene considerata anche per gli sviluppatori descrittivo in quanto supporta più lingue, ad esempio Python, C, C++, c#, Java e JavaScript tra gli altri.

## <a name="what-is-azure-redis-cache"></a>Che cos'è Cache Redis di Azure?

Cache Redis di Microsoft Azure si basa sulla nota cache Redis open source e consente di accedere a una cache Redis sicura e dedicata gestita da Microsoft. Una cache creata con Cache Redis di Azure è accessibile da qualsiasi applicazione all'interno di Microsoft Azure. In genere, Cache Redis di Azure viene usata per aumentare le prestazioni dei sistemi che si basano su archivi dati back-end.

I dati memorizzati nella cache sono disponibili all'interno della memoria in un server di Azure che esegue la cache Redis, e non vengono caricati dal disco da un database. La cache è inoltre altamente scalabile. È possibile modificare le dimensioni e il piano tariffario in qualsiasi momento.

## <a name="what-type-of-data-can-be-stored-in-the-cache"></a>Tipo di dati può essere archiviato nella cache?

Redis supporta una varietà di tutti i tipi di dati orientata _binario sicuro_ stringhe. Ciò significa che è possibile usare qualsiasi sequenza binario per un valore, da una stringa, ad esempio "i-amore--strada con pietre" per il contenuto di un file di immagine. Una stringa vuota è anche un valore valido.

- Stringhe indipendente dai file binario (più comuni)
- Elenchi di stringhe
- Set non ordinato di stringhe
- Hash
- Set ordinato di stringhe
- Esegue il mapping di stringhe

Ogni valore è associato a un _chiave_ che può essere usato per cercare il valore dalla cache. Redis funziona meglio con i valori più bassi (100 KB o inferiore), quindi considerare di suddividere i dati più grandi in più chiavi. L'archiviazione di valori più grandi è possibile (fino a 500 MB), ma aumenta la latenza di rete e può causare problemi di memoria insufficiente e memorizzazione nella cache se la cache non è configurata per far scadere i valori precedenti.

## <a name="what-is-a-redis-key"></a>Che cos'è una chiave Redis?
Redis codici sono stringhe binarie-safe. Di seguito sono riportate alcune linee guida per la scelta delle chiavi:

- Evitare le chiavi lunghe. Che occupano più memoria e richiedono ricerca più volte perché essi devono essere confrontati byte per byte. Se si desidera usare un blob binario come chiave, genera un hash univoco e usare invece che come chiave. Le dimensioni massime di una chiave sono 512 MB, ma devi _mai_ usare una chiave di tale dimensione.
- Usare le chiavi che identificano i dati. Ad esempio, "sport: football; data: 2008-02-02" potrebbe essere una chiave migliorata rispetto a "fb:8-2-2". Nel primo caso è più leggibile e di maggiori dimensioni sono irrilevante. Trovare l'equilibrio tra dimensioni e migliorare la leggibilità.
- Usare una convenzione. Una buona scelta è ": id di oggetto", come in "sport: football". 

## <a name="how-is-data-stored-in-a-redis-cache"></a>Come vengono archiviati i dati in una cache Redis?

I dati in Redis vengono archiviati sotto forma di _**nodi**_ e _**cluster**_.

I **nodi** sono uno spazio in Redis in cui vengono archiviati i dati.

I **cluster** sono costituiti da tre o più nodi su cui è suddiviso il set di dati. I cluster sono utili perché consento la prosecuzione delle operazioni se un nodo presenta un errore o non è in grado di comunicare con il resto del cluster.

## <a name="what-are-redis-caching-architectures"></a>Cosa sono le architetture di memorizzazione nella cache Redis?

L'architettura di memorizzazione nella cache Redis rappresenta la modalità di distribuzione dei dati nella cache. Redis distribuisce i dati in tre modi principali:

1. **Nodo singolo**
1. **Nodo multiplo**
1. **Cluster**

Le architetture di memorizzazione nella cache Redis vengono suddivise in Azure in base a livelli:

### <a name="basic-cache"></a>Cache Basic

Una cache Basic offre una cache Redis a _**nodo singolo**_. Il set di dati completo verrà archiviato in un singolo nodo. Questo livello è ideale per lo sviluppo, il test e i carichi di lavoro non critici.

### <a name="standard-cache"></a>Cache Standard

La cache Standard crea architetture _**a nodo multiplo**_. Redis replica una cache in una configurazione a due nodi primaria/secondaria. Azure gestisce la replica tra i due nodi. Si tratta di una cache di produzione con replica master/slave.

### <a name="premium-tier"></a>Livello Premium

Il livello Premium include le funzionalità del livello Standard, offre inoltre la possibilità di rendere persistenti i dati, acquisire snapshot ed eseguire il backup dei dati. Con questo livello è possibile creare un cluster Redis che partiziona i dati tra più nodi Redis per aumentare la memoria disponibile. Il livello Premium supporta inoltre una rete virtuale di Azure per offrire il controllo completo sulle connessioni, le subnet, gli indirizzi IP e l'isolamento rete. Questo livello include anche la replica geografica, in modo da garantire che i dati siano vicini all'app che li usa.

## <a name="summary"></a>Riepilogo

Un database è ideale per archiviare grandi quantità di dati, ma presenta una latenza intrinseca durante la ricerca di dati. Si invia una query. Il server interpreta la query, cerca i dati e li restituisce. I server presentano limiti anche in termini di capacità per la gestione delle richieste. Se vengono effettuate troppe richieste, il recupero dei dati verrà rallentato. Grazie alla cache è possibile archiviare i dati richiesti con maggiore frequenza nella memoria, la quale può restituirli più velocemente rispetto alle query di un database. Ciò dovrebbe assicurare una minore latenza e prestazioni migliori. Cache Redis di Azure consente di accedere a una cache Redis sicura, dedicata e scalabile, ospitata in Azure e gestita da Microsoft.
