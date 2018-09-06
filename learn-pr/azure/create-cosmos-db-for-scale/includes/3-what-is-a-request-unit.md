Viene quindi valutata la velocità effettiva dei dati per il database in questione. È un elemento importante per garantire che sia possibile gestire il volume delle transazioni richiesto dalle esigenze aziendali. I requisiti relativi alla velocità effettiva non sono sempre uniformi. Si consideri ad esempio un sito Web per gli acquisti, che deve essere ridimensionato durante il periodo dei saldi o delle festività natalizie. È necessario stimare innanzitutto i requisiti di velocità effettiva del database in questione.

## <a name="what-is-database-throughput"></a>Che cos'è la velocità effettiva del database? 

La velocità effettiva del database è il numero di operazioni di lettura e scrittura che il database può eseguire in un secondo.

Per ridimensionare la velocità effettiva in modo strategico, è necessario stimare la velocità effettiva necessaria valutando il numero di operazioni di lettura e scrittura da supportare in momenti diversi e per documenti con diverse dimensioni. Una stima corretta contribuisce a non deludere gli utenti nei momenti di picco della domanda. Una stima non accurata può invece limitare la velocità delle richieste, causando operazioni in attesa e tentativi ripetuti e conseguenti latenze elevate e clienti insoddisfatti.

## <a name="what-is-a-request-unit"></a>Cos'è un'unità richiesta?

Azure Cosmos DB misura la velocità effettiva usando l'**unità richiesta (UR)**. L'utilizzo delle unità richiesta è misurato al secondo, pertanto l'unità di misura è l'**unità richiesta al secondo (UR/sec)**. È necessario prevedere in anticipo il numero di UR/sec di cui Azure Cosmos DB dovrà eseguire il provisioning, in modo che la soluzione possa gestire il carico stimato e aumentare o diminuire le UR/sec in qualsiasi momento per soddisfare la domanda corrente.

## <a name="request-unit-basics"></a>Nozioni di base sulle unità richiesta

Una singola unità richiesta, ovvero 1 UR, equivale al costo approssimativo dell'esecuzione di una singola richiesta GET su un documento di 1 KB usando l'ID del documento. L'esecuzione di una richiesta GET con l'ID di un documento è un metodo efficiente per recuperare un documento e implica un costo contenuto. Una richiesta di creazione, sostituzione o eliminazione dello stesso elemento usa più capacità di elaborazione del servizio e quindi richiede più unità richiesta.

Il numero di unità richiesta impiegato per un'operazione varia in funzione delle dimensioni e del numero di proprietà del documento, dell'operazione eseguita e di altri concetti complessi come la coerenza e i criteri di indicizzazione.

La tabella riportata di seguito mostra il numero di unità richiesta per elementi con tre dimensioni diverse, ovvero 1 KB, 4 KB e 64 KB, e due livelli di prestazioni diversi, ovvero 500 letture al secondo + 100 scritture al secondo e 500 letture al secondo + 500 scritture al secondo. In questo esempio la coerenza dei dati è impostata su **Sessione** e i criteri di indicizzazione sono impostati su **Nessuno**.

| Dimensioni dell'elemento | Letture al secondo | Scritture al secondo | Unità richiesta
| --- | --- | --- | --- |
| 1 KB | 500 | 100 | (500 * 1) + (100 * 5) = 1.000 UR/sec
| 1 KB | 500 | 500 | (500 * 1) + (500 * 5) = 3.000 UR/sec
| 4 KB | 500 | 100 | (500 * 1,3) + (100 * 7) = 1.350 UR/sec
| 4 KB | 500 | 500 | (500 * 1,3) + (500 * 7) = 4.150 UR/sec
| 64 KB | 500 | 100 | (500 * 10) + (100 * 48) = 9.800 UR/sec
| 64 KB | 500 | 500 | (500 * 10) + (500 * 48) = 29.000 UR/sec
 
Come si può notare, maggiori sono le dimensioni dell'elemento, maggiore è il numero di letture e scritture necessarie e delle unità richiesta da riservare. Per stimare le esigenze di velocità effettiva di un'applicazione, è utile [Capacity Planner](https://www.documentdb.com/capacityplanner) di Azure Cosmos DB, uno strumento online che consente di caricare un documento JSON di esempio e stabilire il numero di operazioni da completare al secondo. Lo strumento offre una stima del totale da riservare.

## <a name="exceeding-throughput-limits"></a>Superamento dei limiti di velocità effettiva

Se non viene riservata una quantità sufficiente di unità richiesta e si tenta di leggere o scrivere più dati di quelli consentiti dalla velocità effettiva di cui viene eseguito il provisioning, la velocità della richiesta risulterà limitata. In questo caso, è necessario eseguire un nuovo tentativo di richiesta dopo un intervallo specificato. Se si usa .NET SDK, il tentativo viene eseguito automaticamente dopo l'attesa specificata nell'intestazione retry-after.

## <a name="creating-an-account-built-to-scale"></a>Creazione di un account per la scalabilità

È possibile modificare il numero di unità richiesta di cui viene eseguito il provisioning in un database in qualsiasi momento. Nei periodi di picco è possibile aumentare tale valore per venire incontro alle maggiori esigenze, per poi ridurre la velocità effettiva di cui viene eseguito il provisioning nelle ore non di punta, riducendo di conseguenza i costi.

Quando si crea un account, nel portale è possibile eseguire il provisioning di un minimo di 400 UR/sec e di un massimo di 250.000 UR/sec. Se la velocità effettiva necessaria è maggiore, compilare un ticket nel portale di Azure. L'impostazione della velocità effettiva iniziale su 1.000 UR/sec è consigliabile per quasi tutti gli account, essendo questo il valore minimo in base al quale il database viene ridimensionato automaticamente nel caso in cui siano necessari più di 10 GB di spazio di archiviazione. Se si imposta la velocità effettiva iniziale su un valore inferiore a 1.000 UR/sec, il database non sarà in grado di raggiungere dimensioni superiori ai 10 GB a meno che non venga eseguito un nuovo provisioning del database e fornita una chiave di partizione. Le chiavi di partizione consentono la ricerca rapida dei dati in Azure Cosmos DB e la scalabilità automatica del database quando necessario. Le chiavi di partizione sono illustrate più avanti nel modulo.

## <a name="summary"></a>Riepilogo

A questo punto si è compreso come usare le unità richiesta per stimare e definire la velocità effettiva di un database di Azure Cosmos DB. È possibile modificare le unità di richiesta in qualsiasi momento. Quando si crea un account, l'impostazione su 1.000 UR/sec aiuta a garantire che il database possa essere ridimensionato in un secondo momento.