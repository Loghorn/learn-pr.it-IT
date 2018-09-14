Dietro le quinte di un sito Web sportivo vi è un database, il quale restituisce i dati eseguendo query. Tuttavia, le prestazioni rallentano quando il carico è elevato, in particolare durante eventi sportivi di grandi dimensioni. Negli ambienti ospitati, un aumento nell'uso delle risorse si traduce in costi più elevati. La memorizzazione nella cache dei dati garantisce che il sito Web offra prestazioni adeguate a un costo ridotto.

## <a name="what-is-caching"></a>Che cos'è la memorizzazione nella cache?

La memorizzazione nella cache presuppone l'archiviazione dei dati usati di frequente in una memoria molto vicina all'applicazione che li sfrutta. La memorizzazione nella cache consente di migliorare le prestazioni e ridurre il carico sui server. Redis viene usata per creare una cache nella memoria che può offrire una latenza eccellente e aumentare notevolmente le prestazioni.

## <a name="what-is-a-redis-cache"></a>Che cos'è una cache Redis?

La cache Redis (**RE**mote **DI**ctionary **S**erver) è un archivio open source di coppie di valori chiave nella memoria. Redis è nota perché è veloce e può archiviare e modificare tipi di dati comuni, ad esempio stringhe, hash e set. È adatta agli sviluppatori in quanto supporta più linguaggi, ad esempio ASP.NET, Python, C, C++, C#, Java, JavaScript e Node.js.

## <a name="what-is-azure-redis-cache"></a>Che cos'è Cache Redis di Azure?

Cache Redis di Microsoft Azure si basa sulla nota cache Redis open source e consente di accedere a una cache Redis sicura e dedicata gestita da Microsoft. Una cache creata con Cache Redis di Azure è accessibile da qualsiasi applicazione all'interno di Microsoft Azure. Cache Redis di Azure viene in genere usato per migliorare le prestazioni dei sistemi che si basano su archivi dati back-end.

I dati memorizzati nella cache sono disponibili all'interno della memoria in un server di Azure che esegue la cache Redis, e non vengono caricati dal disco da un database. La cache è inoltre altamente scalabile. È possibile modificare le dimensioni e il piano tariffario in qualsiasi momento.

## <a name="how-is-data-stored-in-a-redis-cache"></a>Come vengono archiviati i dati in una cache Redis?

I dati in Redis vengono archiviati sotto forma di _**nodi**_ e _**cluster**_.

I **nodi** sono uno spazio in Redis in cui vengono archiviati i dati.

I **cluster** sono costituiti da tre o più nodi su cui è suddiviso il set di dati. I cluster sono utili perché consento la prosecuzione delle operazioni se un nodo presenta un errore o non è in grado di comunicare con il resto del cluster.

## <a name="what-are-redis-caching-architectures"></a>Cosa sono le architetture di memorizzazione nella cache Redis?

L'architettura di memorizzazione nella cache Redis rappresenta la modalità di distribuzione dei dati nella cache. Redis offre tre principali modi in cui i dati vengono distribuiti:

1. **Nodo singolo**
1. **Nodo multiplo**
1. **Cluster**

Le architetture di memorizzazione nella cache Redis vengono suddivise in Azure in base a livelli:

### <a name="basic-cache"></a>Cache Basic

Una cache Basic offre una cache Redis a _**nodo singolo**_. Il set di dati completo verrà archiviato in un singolo nodo. Questo livello è ideale per lo sviluppo, il test e i carichi di lavoro non critici.

### <a name="standard-cache"></a>Cache Standard

La cache Standard crea architetture _**a nodo multiplo**_. Redis replica una cache in una configurazione a due nodi primaria/secondaria. Azure gestisce la replica tra i due nodi. Si tratta di una cache di produzione con replica master/slave.

### <a name="premium-tier"></a>Livello Premium

Il livello Premium include le funzionalità del livello Standard, ma offre la possibilità di rendere persistenti i dati, acquisire snapshot ed eseguire il backup dei dati. Con questo livello è possibile creare un cluster Redis che partiziona i dati tra più nodi Redis per aumentare la memoria disponibile. Il livello Premium supporta inoltre una rete virtuale di Azure per offrire il controllo completo sulle connessioni, le subnet, gli indirizzi IP e l'isolamento rete. Questo livello include anche la replica geografica, in modo da garantire che i dati siano vicini all'app che li utilizza.

## <a name="summary"></a>Riepilogo

Un database è ideale per archiviare grandi quantità di dati, ma presenta una latenza intrinseca durante la ricerca di dati. Si invia una query. Il server interpreta la query, cerca i dati e li restituisce. I server presentano limiti anche in termini di capacità per la gestione delle richieste. Se vengono effettuate troppe richieste, il recupero dei dati verrà rallentato. Grazie alla cache è possibile archiviare i dati richiesti con maggiore frequenza nella memoria, la quale può restituirli più velocemente rispetto alle query di un database. Ciò dovrebbe assicurare una minore latenza e prestazioni migliori. Cache Redis di Azure consente di accedere a una cache Redis sicura, dedicata e scalabile, ospitata in Azure e gestita da Microsoft.
