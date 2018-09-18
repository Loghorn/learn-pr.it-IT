Il team di sviluppo dell'app per le statistiche sportive ha deciso che la memorizzazione nella cache potrebbe migliorare notevolmente le prestazioni per i dati richiesti di recente. Il passaggio successivo consiste nel creare e configurare un'istanza di Cache Redis di Azure.

## <a name="create-and-configure-the-azure-redis-cache-instance"></a>Creare e configurare l'istanza di Cache Redis di Azure

1. Aprire il [portale di Azure](https://portal.azure.com/?azure-portal=true) nel browser.

1. Fare clic su **Crea una risorsa**.

1. Cercare **Cache Redis**.

1. Fare clic su **Crea**.

1. È possibile aggiungere un'istanza di Cache Redis di Azure dalle risorse del database nel portale di Azure.

1. Specificare un nome globalmente univoco. Il nome deve essere una stringa contenente da 1 a 63 caratteri che possono includere solo numeri, lettere e il carattere '-'. Il nome della cache non può iniziare o terminare con il carattere '-' e più caratteri '-' consecutivi non sono validi.

1. Specificare la sottoscrizione in cui viene creata questa nuova istanza di Cache Redis di Azure.

1. Assegnare un nome al gruppo di risorse in cui creare la cache. Inserendo tutte le risorse per un'app in un gruppo è possibile gestirle insieme.

1. Posizionare sempre l'istanza della cache e l'applicazione nella stessa area/località. La connessione a una cache in un'area diversa può aumentare in modo significativo la latenza e ridurre l'affidabilità. Se ci si connette alla cache all'esterno di Azure, selezionare una posizione vicina alla località in cui è in esecuzione l'applicazione che utilizza i dati.

    > [!IMPORTANT]
    > È molto più importante che la cache sia vicino al consumer di dati piuttosto che all'archivio dati.

1. Selezionare il piano tariffario. 
    - È consigliabile usare sempre il livello Premium o Standard per i sistemi di produzione. Il livello Basic è un sistema a nodo singolo senza replica dei dati e senza contratto di servizio. Usare almeno una cache di livello C1. Le cache di livello C0 sono destinate effettivamente a semplici scenari di sviluppo/test, perché hanno un core di CPU condiviso e una memoria molto ridotta.

    - Il livello Premium consente di rendere persistenti i dati in due modi per offrire il ripristino di emergenza:

        - Con la persistenza RDB viene creato uno snapshot periodico ed è possibile ricostruire la cache usando lo snapshot in caso di errore.

            ![Screenshot del portale di Azure che illustra le opzioni di persistenza RDB per una nuova istanza di Cache Redis.](../media/3-redis-persistence-1.png)

        - La persistenza AOF salva ogni operazione di scrittura in un log che viene salvato almeno una volta al secondo. Vengono così creati file di dimensioni maggiori rispetto a RDB ma con minore perdita di dati.

            ![Screenshot del portale di Azure che illustra le opzioni di persistenza AOF per una nuova istanza di Cache Redis.](../media/3-redis-persistence-2.png)

    - Proteggere la cache con una rete virtuale.
      Se è disponibile una cache Redis di livello Premium, è possibile distribuirla in una rete virtuale nel cloud. La cache sarà disponibile solo per altre macchine virtuali e applicazioni nella stessa rete virtuale.

    - Distribuire la cache con il clustering.
      Con una cache Redis di livello Premium, è possibile implementare il clustering per suddividere automaticamente i set di dati tra più nodi. Per implementare il clustering, specificare il numero di partizioni per un massimo di 10. Il costo addebitato è il costo del nodo originale moltiplicato per il numero di partizioni.

    - Eseguire la migrazione dal Servizio cache gestita di Azure.
      A seconda delle funzionalità del Servizio cache gestita usate, la migrazione delle applicazioni che usano il Servizio cache gestita di Azure in Cache Redis di Azure può essere eseguita con modifiche minime all'applicazione. Anche se le API non sono esattamente uguali, sono simili. Gran parte del codice esistente che accede alla cache con il Servizio cache gestita può essere riutilizzata con modifiche minime.

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

Per connettersi a un'istanza di Cache Redis di Azure, i client necessitano di nome host, porta e chiave di accesso per la cache. È possibile recuperare queste informazioni nel portale di Azure tramite la pagina **Impostazioni > Chiavi di accesso**. 

- Il nome host è l'indirizzo Internet pubblico della cache, creato usando il nome della cache. Ad esempio, `sportsresults.redis.cache.windows.net`.

- La chiave di accesso funge da password per la cache. Vengono create due chiavi, primaria e secondaria. È possibile usare una delle due chiavi. Ne vengono fornite due nel caso diventi necessario modificare la chiave primaria. È possibile passare tutti i client alla chiave secondaria e rigenerare la chiave primaria. Questa operazione blocca eventuali applicazioni che usano la chiave primaria originale. Microsoft consiglia di rigenerare periodicamente le chiavi, analogamente alle password perdonali.

> [!WARNING]
> Le chiavi di accesso devono essere considerate informazioni riservate, proprio come una password. Chiunque abbia una chiave di accesso può eseguire qualsiasi operazione sulla cache.
