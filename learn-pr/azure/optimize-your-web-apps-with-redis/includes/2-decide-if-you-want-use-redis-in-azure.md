Dietro le quinte di un sito Web sportivo vi è un database, il quale restituisce i dati eseguendo query. Tuttavia, le prestazioni rallentano quando il carico è elevato, in particolare durante eventi sportivi più seguiti. Negli ambienti ospitati, un aumento nell'uso delle risorse si traduce in costi più elevati. La memorizzazione nella cache dei dati garantisce che il sito Web offra prestazioni adeguate a un costo ridotto.

## <a name="what-is-caching"></a>Che cos'è la memorizzazione nella cache?

La memorizzazione nella cache presuppone l'archiviazione dei dati usati di frequente in una memoria molto vicina all'applicazione che li utilizza. La memorizzazione nella cache consente di migliorare le prestazioni e ridurre il carico sui server. Redis viene usata per creare una cache nella memoria che può offrire una latenza eccellente e aumentare potenzialmente le prestazioni.

## <a name="what-is-a-redis-cache"></a>Che cos'è la cache Redis?

La cache Redis (**RE**mote **DI**ctionary **S**erver) è un archivio open source di coppie chiave/valore in memoria. È una cache molto diffusa perché è veloce e può archiviare e modificare tipi di dati comuni, ad esempio stringhe, hash e set. È adatta agli sviluppatori in quanto supporta più linguaggi, ad esempio Python, C, C++, C#, Java, JavaScript e altri ancora.

## <a name="what-is-azure-redis-cache"></a>Che cos'è la Cache Redis di Azure?

Cache Redis di Microsoft Azure si basa sulla popolare cache Redis open source e consente di accedere a una cache Redis sicura e dedicata gestita da Microsoft. Una cache creata usando Cache Redis di Azure sarà accessibile da qualsiasi applicazione all'interno di Microsoft Azure. In genere, Cache Redis di Azure viene usata per aumentare le prestazioni dei sistemi che si basano su archivi dati back-end.

I dati memorizzati nella cache sono disponibili all'interno della memoria in un server di Azure che esegue la cache Redis, e non vengono caricati dal disco da un database. La cache è inoltre altamente scalabile. È possibile modificare le dimensioni e il piano tariffario in qualsiasi momento.

## <a name="what-type-of-data-can-be-stored-in-the-cache"></a>Quali tipi di dati possono essere archiviati nella cache?

Redis supporta diversi tipi di dati, tutti orientati verso le stringhe _indipendenti dall'aspetto binario_. È infatti possibile usare qualsiasi sequenza binaria per un valore, da una stringa quale "i-love-rocky-road" ai contenuti di un file di immagine. Anche una stringa vuota è un valore valido.

- Stringhe indipendenti dall'aspetto binario (più comuni)
- Elenchi di stringhe
- Set non ordinati di stringhe
- Hash
- Set ordinati di stringhe
- Mappe di stringhe

Ogni valore di dati è associato a un valore _key_, che può essere usato per cercare il valore dalla cache. Redis funziona meglio con valori di dimensioni ridotte (al massimo 100.000), quindi è consigliabile suddividere i dati più grandi in più chiavi. È possibile archiviare valori di dimensioni maggiori (fino a 500 MB), ma ciò comporta l'aumento della latenza di rete e può provocare problemi di memorizzazione nella cache e di esaurimento della memoria se la cache non è configurata per la scadenza dei valori obsoleti.

## <a name="what-is-a-redis-key"></a>Che cos'è una chiave Redis?
Anche le chiavi Redis sono stringhe indipendenti dall'aspetto binario. Ecco alcune indicazioni per la scelta delle chiavi:

- Evitare le chiavi lunghe. Richiedono più memoria e rallentano la ricerca perché è necessario il confronto byte per byte. Se si vuole usare un BLOB binario come chiave, generare un hash univoco e usarlo come chiave. Le dimensioni massime per una chiave sono 512 MB, ma è consigliabile non usare _mai_ una chiave con tali dimensioni.
- Usare chiavi che consentono di identificare i dati. Ad esempio, "sport:football;date:2008-02-02" è una chiave migliore rispetto a "fb:8-2-2". È infatti più leggibile e la differenza a livello di dimensioni è minima. Trovare il giusto equilibrio tra dimensioni e leggibilità.
- Usare una convenzione, ad esempio "object:id", come in "sport:football". 

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
