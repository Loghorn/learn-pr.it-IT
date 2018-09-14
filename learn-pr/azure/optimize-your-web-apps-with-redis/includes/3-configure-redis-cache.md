Il team di sviluppo dell'app per le statistiche sportive ha deciso che la memorizzazione nella cache potrebbe migliorare notevolmente le prestazioni per i dati richiesti di recente. Il passaggio successivo consiste nel creare e configurare un'istanza di Cache Redis di Azure.

## <a name="create-and-configure-the-azure-redis-cache-instance"></a>Creare e configurare l'istanza di Cache Redis di Azure

È possibile creare una cache Redis usando il portale di Azure, il comando di Azure o Azure PowerShell. Esistono vari parametri, che è necessario stabilire per poter configurare correttamente la cache per gli scopi desiderati.

### <a name="name"></a>Nome

La cache Redis sarà necessario un nome univoco globale. Il nome deve essere univoco all'interno di Azure perché è usato per generare un URL pubblico per connettersi e comunicare con il servizio.

Il nome deve essere compreso tra 1 e 63 caratteri, costituite da numeri, lettere e '-' caratteri. Il nome della cache non può iniziare o terminare con il carattere '-' e più caratteri '-' consecutivi non sono validi.

### <a name="resource-group"></a>Gruppo di risorse

Cache Redis di Azure è una risorsa gestita e deve un _gruppo di risorse_ proprietario. È possibile creare un nuovo gruppo di risorse o usarne uno esistente in un susbcription di che fanno parte.

### <a name="location"></a>Posizione

È necessario decidere dove verrà vengano posizionata fisicamente la cache Redis selezionando un'area di Azure. È sempre consigliabile inserire l'istanza di cache e l'applicazione nella stessa area. Connessione a una cache in un'area diversa può significativamente aumentare la latenza e ridurre affidabilità. Se ci si connette alla cache all'esterno di Azure, selezionare una località vicina in cui è in esecuzione l'applicazione che utilizza i dati.

> [!IMPORTANT]
> Inserire la cache Redis come vicino ai dati _consumatore_ possibili.

### <a name="pricing-tier"></a>Piano tariffario

Come indicato nell'ultima unità, sono disponibili tre piani tariffari disponibili per la Cache Redis di Azure.

- **Base**: cache Basic ideale per sviluppo/test. È limitato a 20.000 connessioni, 53 GB di memoria e un singolo server. Non vi è alcun contratto di servizio per questo livello di servizio.
- **Standard**: cache di produzione che supporta la replica e include un contratto di servizio del 99,99%. Supporta due server, master/slave, e ha gli stessi limiti di memoria/connessione come il livello Basic.
- **Premium**: livello aziendale che si basa sul livello Standard e include il supporto di persistenza, clustering e scalabilità orizzontale della cache. Questo è il livello di prestazioni più elevato con fino a 530 GB di memoria e 40.000 connessioni simultanee.

È possibile controllare la quantità di memoria cache disponibile in ogni livello: questa opzione è selezionata, scegliendo un livello di cache C0 C6 per P0-P4 Premium e Basic o Standard. Verificare i [pagina dei prezzi](https://azure.microsoft.com/en-us/pricing/details/cache/) per i dettagli completi.

> [!TIP]
> Microsoft consiglia di che utilizzare sempre livello Standard o Premium per i sistemi di produzione. Il livello Basic è un sistema a nodo singolo senza replica dei dati e senza contratto di servizio. Usare almeno una cache di livello C1. Cache C0 sono destinate a scenari di sviluppo e test semplice poiché presentano una condivisione dei core CPU e memoria minima.

Il livello Premium consente di rendere persistenti i dati in due modi per fornire il ripristino di emergenza:

1. Con la persistenza RDB viene creato uno snapshot periodico ed è possibile ricostruire la cache usando lo snapshot in caso di errore.

    ![Screenshot del portale di Azure che illustra le opzioni di persistenza RDB per una nuova istanza di Cache Redis.](../media/3-redis-persistence-1.png)

2. La persistenza AOF salva ogni operazione di scrittura in un log che viene salvato almeno una volta al secondo. Vengono così creati file di dimensioni maggiori rispetto a RDB ma con minore perdita di dati.

    ![Screenshot del portale di Azure che illustra le opzioni di persistenza AOF per una nuova istanza di Cache Redis.](../media/3-redis-persistence-2.png)

Esistono diverse altre impostazioni che sono disponibili solo per i **Premium** livello.

### <a name="virtual-network-support"></a>Supporto della rete virtuale

Se si crea un livello premium della cache Redis, è possibile distribuirlo a una rete virtuale nel cloud. La cache sarà disponibile solo per altre macchine virtuali e applicazioni nella stessa rete virtuale. Ciò offre un livello superiore di sicurezza quando il servizio e cache sono ospitati in Azure oppure sono connessi tramite una rete virtuale di Azure VPN.

### <a name="clustering-support"></a>Supporto per il clustering

Con una cache Redis di livello Premium, è possibile implementare il clustering per suddividere automaticamente i set di dati tra più nodi. Per implementare il clustering, specificare il numero di partizioni per un massimo di 10. Il costo viene addebitato è il costo del nodo originale, moltiplicato per il numero di partizioni.

## <a name="accessing-the-redis-instance"></a>Che accedono all'istanza di Redis

Redis supporta un set di [noto comandi](https://redis.io/commands). In genere viene eseguito un comando come `COMMAND parameter1 parameter2 parameter3`.

Ecco alcuni comandi comuni che è possibile usare:

| Comando | Descrizione |
|---------|-------------|
| `ping` | Il ping del server. Restituisce "PONG". |
| `set [key] [value]` | Imposta un chiave/valore nella cache. Se l'operazione riesce, restituisce "OK". |
| `get [key]` | Ottiene un valore dalla cache. |
| `exists [key]` | Restituisce '1' se il **chiave** se non è presente nella cache, '0'. |
| `type [key]` | Restituisce il tipo associato al valore per il dato **chiave**. |
| `incr [key]` | Incrementare il valore specificato associato **chiave** da '1'. Il valore deve essere un valore integer o double. Restituisce il nuovo valore. |
| `incrby [key] [amount]` | Incrementare il valore specificato associato **chiave** della quantità specificata. Il valore deve essere un valore integer o double. Restituisce il nuovo valore. |
| `del [key]` | Elimina il valore associato con il **chiave**. |
| `flushdb` | Eliminare _tutti_ chiavi e valori nel database. |

Redis è uno strumento da riga di comando (**redis-cli**) è possibile usare per sperimentare direttamente con questi comandi. Di seguito sono riportati alcuni esempi.

```output
> set somekey somevalue
OK
> get somekey
"somevalue"
> exists somekey
(string) 1
> del somekey
(string) 1
> exists somekey
(string) 0
```

Di seguito è riportato un esempio di utilizzo di `INCR` comandi. Queste sono utili perché forniscono incrementi atomici _tra più applicazioni_ che sta utilizzando la cache.

```output
> set counter 100
OK
> incr counter
(integer) 101
> incrby counter 50
(integer) 151
> type counter
(integer)
```

### <a name="adding-an-expiration-time-to-values"></a>Aggiunta di un'ora di scadenza per i valori

La memorizzazione nella cache è importante quanto ci consente di archiviare comunemente usati i valori in memoria. Tuttavia, è necessario anche un modo per scadere i valori quando sono obsoleti. Redis questa operazione viene eseguita applicando una _durata (TTL)_ (durata TTL) a una chiave.

Se la durata (TTL) scade, viene eliminata automaticamente la chiave, esattamente come se il comando DEL sono stato rilasciato. Di seguito sono indicate alcune note sulle scadenze TTL.

- È possibile impostare le scadenze con precisione secondi o millisecondi.
- La risoluzione di tempo di scadenza è sempre 1 millisecondo.
- Informazioni sulla scadono vengono replicati e resi persistenti su disco, il tempo praticamente ha esito positivo quando i server Redis rimane arrestato (Ciò significa che Redis Salva la data di scadenza una chiave).

Di seguito è riportato un esempio di una data di scadenza:

```output
> set counter 100
OK
> expire counter 5
(integer) 1
> get counter
100
... wait ...
> get counter
(nil)
```

## <a name="accessing-a-redis-cache-from-a-client"></a>L'accesso a una cache Redis da un client

Per connettersi a un'istanza di Cache Redis di Azure, è necessario di varie informazioni. I client devono il nome host, porta e una chiave di accesso per la cache. È possibile recuperare queste informazioni nel portale di Azure tramite il **Impostazioni > chiavi di accesso** pagina. 

- Il nome host è l'indirizzo Internet pubblico della cache, che è stata creata usando il nome della cache. Ad esempio, `sportsresults.redis.cache.windows.net`.

- La chiave di accesso funge da password per la cache. Sono disponibili due chiavi create: primario e secondario. È possibile usare una delle due chiavi, due vengono forniti nel caso in cui è necessario modificare la chiave primaria. È possibile passare tutti i client per la chiave secondaria e rigenerare la chiave primaria. Ciò bloccherà le applicazioni usando la chiave primaria originale. Microsoft consiglia di rigenerazione periodicamente le chiavi - molto come si farebbe le password personale.

> [!WARNING]
> Le chiavi di accesso devono essere considerate informazioni riservate, treat li, ad esempio si sarebbe una password. Chiunque abbia una chiave di accesso può eseguire qualsiasi operazione nella cache.
