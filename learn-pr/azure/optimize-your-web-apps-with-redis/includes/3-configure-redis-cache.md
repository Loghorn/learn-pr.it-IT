Il team di sviluppo dell'app per le statistiche sportive ha deciso che la memorizzazione nella cache potrebbe migliorare notevolmente le prestazioni per i dati richiesti di recente. Il passaggio successivo consiste nel creare e configurare un'istanza di Cache Redis di Azure.

## <a name="create-and-configure-the-azure-redis-cache-instance"></a>Creare e configurare l'istanza di Cache Redis di Azure

È possibile creare una cache Redis con il portale di Azure, l'interfaccia della riga di comando di Azure o Azure PowerShell. Per configurare correttamente la cache per gli scopi desiderati è necessario impostare vari parametri.

### <a name="name"></a>Nome

Cache Redis dovrà avere un nome univoco a livello globale. Il nome deve essere univoco all'interno di Azure perché è usato per generare un URL pubblico per connettersi e comunicare con il servizio.

Il nome deve contenere da 1 a 63 caratteri, che possono includere numeri, lettere e il carattere "-". Il nome della cache non può iniziare o terminare con il carattere "-" e più caratteri "-" consecutivi non sono validi.

### <a name="resource-group"></a>Gruppo di risorse

Cache Redis di Azure è una risorsa gestita e deve disporre di un proprietario del _gruppo di risorse_. È possibile creare un nuovo gruppo di risorse o selezionarne uno esistente all'interno della propria sottoscrizione.

### <a name="location"></a>Località

È necessario stabilire dove verrà posizionata fisicamente la Cache Redis selezionando un'area di Azure. Posizionare sempre l'istanza della cache e l'applicazione nella stessa area. La connessione a una cache in un'area diversa può aumentare in modo significativo la latenza e ridurre l'affidabilità. Se ci si connette alla cache all'esterno di Azure, selezionare una posizione vicina alla località in cui è in esecuzione l'applicazione che utilizza i dati.

> [!IMPORTANT]
> Inserire la Cache Redis il più vicino possibile al _consumer_ di dati.

### <a name="pricing-tier"></a>Piano tariffario

Come indicato nell'ultima unità, per la Cache Redis di Azure sono disponibili tre piani tariffari.

- **Basic**: cache di base ideale per sviluppo e testing. Prevede un limite di 20.000 connessioni, 53 GB di memoria e un singolo server. Questo livello di servizio non prevede alcun contratto di servizio.
- **Standard**: cache di produzione che supporta la replica e include un contratto di servizio del 99,99%. Supporta due server, master e slave, e ha gli stessi limiti di memoria/connessione del livello Basic.
- **Premium**: livello aziendale che si basa sul livello Standard e include il supporto di persistenza, clustering e scalabilità orizzontale della cache. Questo è il livello di prestazioni più elevato, con 40.000 connessioni simultanee e fino a 530 GB di memoria.

È possibile controllare la quantità di memoria cache disponibile in ogni livello, selezionando un livello di cache C0 a C6 per Basic e Standard e da P0 a P4 per Premium. Per tutti i dettagli, vedere la pagina [relativa ai prezzi](https://azure.microsoft.com/pricing/details/cache/).

> [!TIP]
> È consigliabile usare sempre il livello Premium o Standard per i sistemi di produzione. Il livello Basic è un sistema a nodo singolo senza replica dei dati e senza contratto di servizio. Usare almeno una cache di livello C1. Le cache di livello C0 sono destinate effettivamente a semplici scenari di sviluppo/test, perché hanno un core di CPU condiviso e una memoria molto ridotta.

Il livello Premium consente di rendere persistenti i dati in due modi per offrire il ripristino di emergenza:

1. Con la persistenza RDB viene creato uno snapshot periodico ed è possibile ricostruire la cache usando lo snapshot in caso di errore.

    ![Screenshot del portale di Azure che illustra le opzioni di persistenza RDB per una nuova istanza di Cache Redis.](../media/3-redis-persistence-1.png)

2. La persistenza AOF salva ogni operazione di scrittura in un log che viene salvato almeno una volta al secondo. Vengono così creati file di dimensioni maggiori rispetto a RDB ma con minore perdita di dati.

    ![Screenshot del portale di Azure che illustra le opzioni di persistenza AOF per una nuova istanza di Cache Redis.](../media/3-redis-persistence-2.png)

Esistono diverse altre impostazioni, disponibili solo per il livello **Premium**.

### <a name="virtual-network-support"></a>Supporto della rete virtuale

Se si crea una cache Redis di livello Premium, è possibile distribuirla in una rete virtuale nel cloud. La cache sarà disponibile solo per altre macchine virtuali e applicazioni nella stessa rete virtuale. Questo offre un livello di sicurezza superiore quando sia il servizio che la cache sono ospitati in Azure oppure sono connessi tramite una VPN di rete virtuale di Azure.

### <a name="clustering-support"></a>Supporto del clustering

Con una cache Redis di livello Premium, è possibile implementare il clustering per suddividere automaticamente i set di dati tra più nodi. Per implementare il clustering, specificare il numero di partizioni per un massimo di 10. Il costo addebitato è il costo del nodo originale moltiplicato per il numero di partizioni.

## <a name="accessing-the-redis-instance"></a>Accesso all'istanza di Redis

Redis supporta un set di [comandi noti](https://redis.io/commands). Un comando viene in genere eseguito come `COMMAND parameter1 parameter2 parameter3`.

Ecco alcuni comandi comuni che è possibile usare:

| Comando | Descrizione |
|---------|-------------|
| `ping` | Consente di effettuare il ping del server. Restituisce "PONG". |
| `set [key] [value]` | Consente di impostare una coppia chiave/valore nella cache. Restituisce "OK" in caso di esito positivo. |
| `get [key]` | Consente di ottenere un valore dalla cache. |
| `exists [key]` | Restituisce "1" se il valore **key** esiste nella cache, "0" se non esiste. |
| `type [key]` | Restituisce il tipo associato al valore per il valore **key** specificato. |
| `incr [key]` | Consente di incrementare il valore specificato associato a **key** di "1". Il valore deve essere di tipo Integer o Double. Restituisce il nuovo valore. |
| `incrby [key] [amount]` | Consente di incrementare il valore specificato associato a **key** in base alla quantità specificata. Il valore deve essere di tipo Integer o Double. Restituisce il nuovo valore. |
| `del [key]` | Consente di eliminare il valore associato a **key**. |
| `flushdb` | Consente di eliminare _ogni_ chiave e valore nel database. |

Redis include uno strumento da riga di comando (**redis-cli**) che può essere usato per sperimentare direttamente con questi comandi. Di seguito sono riportati alcuni esempi.

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

Ecco un esempio relativo all'uso dei comandi `INCR`. Sono utili perché forniscono incrementi atomici _in più applicazioni_ che usano la cache.

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

### <a name="adding-an-expiration-time-to-values"></a>Aggiunta di una scadenza ai valori

La memorizzazione nella cache è importante perché consente di archiviare in memoria i valori più usati. È tuttavia necessario anche un modo per imporre la scadenza dei valori quando sono obsoleti. In Redis è possibile ottenere questo risultato applicando una _durata_ (TTL) a una chiave.

Allo scadere del valore TTL, la chiave viene eliminata automaticamente, esattamente come se fosse stato eseguito il comando DEL. Ecco alcune note sulle scadenze del valore TTL.

- Le scadenze possono essere impostate con una precisione di secondi o millisecondi.
- La risoluzione dell'ora di scadenza è sempre pari a 1 millisecondo.
- Le informazioni sulle scadenze vengono replicate e salvate in modo permanente su disco. Il tempo trascorre effettivamente quando il server Redis rimane in stato arrestato. Redis salva in effetti la data della scadenza della chiave.

Ecco un esempio di scadenza:

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

## <a name="accessing-a-redis-cache-from-a-client"></a>Accesso a una cache Redis da un client

Per connettersi a un'istanza di Cache Redis di Azure, è necessario specificare varie informazioni. I client devono conoscere il nome host, la porta e la chiave di accesso per la cache. È possibile recuperare queste informazioni nel portale di Azure tramite la pagina **Impostazioni > Chiavi di accesso**. 

- Il nome host è l'indirizzo Internet pubblico della cache, creato usando il nome della cache. Ad esempio, `sportsresults.redis.cache.windows.net`.

- La chiave di accesso funge da password per la cache. Vengono create due chiavi, primaria e secondaria. È possibile usare una delle due chiavi. Ne vengono fornite due nel caso diventi necessario modificare la chiave primaria. È possibile passare tutti i client alla chiave secondaria e rigenerare la chiave primaria. Questa operazione blocca eventuali applicazioni che usano la chiave primaria originale. Microsoft consiglia di rigenerare periodicamente le chiavi, analogamente alle password perdonali.

> [!WARNING]
> Le chiavi di accesso devono essere considerate informazioni riservate, proprio come una password. Chiunque abbia una chiave di accesso può eseguire qualsiasi operazione sulla cache.
