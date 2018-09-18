<span data-ttu-id="aa93f-101">Il team di sviluppo dell'app per le statistiche sportive ha deciso che la memorizzazione nella cache potrebbe migliorare notevolmente le prestazioni per i dati richiesti di recente.</span><span class="sxs-lookup"><span data-stu-id="aa93f-101">Your sports statistics development team has decided that caching could dramatically improve performance for recently requested data.</span></span> <span data-ttu-id="aa93f-102">Il passaggio successivo consiste nel creare e configurare un'istanza di Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="aa93f-102">Your next step is to create and configure an Azure Redis Cache instance.</span></span>

## <a name="create-and-configure-the-azure-redis-cache-instance"></a><span data-ttu-id="aa93f-103">Creare e configurare l'istanza di Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="aa93f-103">Create and configure the Azure Redis Cache instance</span></span>

1. <span data-ttu-id="aa93f-104">Aprire il [portale di Azure](https://portal.azure.com/?azure-portal=true) nel browser.</span><span class="sxs-lookup"><span data-stu-id="aa93f-104">Open the [Azure Portal](https://portal.azure.com/?azure-portal=true) in your browser.</span></span>

1. <span data-ttu-id="aa93f-105">Fare clic su **Crea una risorsa**.</span><span class="sxs-lookup"><span data-stu-id="aa93f-105">Click on **Create a resource**.</span></span>

1. <span data-ttu-id="aa93f-106">Cercare **Cache Redis**.</span><span class="sxs-lookup"><span data-stu-id="aa93f-106">Search for **Redis Cache**.</span></span>

1. <span data-ttu-id="aa93f-107">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="aa93f-107">Click **Create**.</span></span>

1. <span data-ttu-id="aa93f-108">È possibile aggiungere un'istanza di Cache Redis di Azure dalle risorse del database nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="aa93f-108">You can add an Azure Redis Cache instance from the database resources in the Azure portal.</span></span>

1. <span data-ttu-id="aa93f-109">Specificare un nome globalmente univoco.</span><span class="sxs-lookup"><span data-stu-id="aa93f-109">Provide a globally unique name.</span></span> <span data-ttu-id="aa93f-110">Il nome deve essere una stringa contenente da 1 a 63 caratteri che possono includere solo numeri, lettere e il carattere '-'.</span><span class="sxs-lookup"><span data-stu-id="aa93f-110">The name must be a string between 1 and 63 characters and include only numbers, letters, and the '-' character.</span></span> <span data-ttu-id="aa93f-111">Il nome della cache non può iniziare o terminare con il carattere '-' e più caratteri '-' consecutivi non sono validi.</span><span class="sxs-lookup"><span data-stu-id="aa93f-111">The cache name can't start or end with the '-' character, and consecutive '-' characters aren't valid.</span></span>

1. <span data-ttu-id="aa93f-112">Specificare la sottoscrizione in cui viene creata questa nuova istanza di Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="aa93f-112">Provide the subscription under which this new Azure Redis Cache instance is created.</span></span>

1. <span data-ttu-id="aa93f-113">Assegnare un nome al gruppo di risorse in cui creare la cache.</span><span class="sxs-lookup"><span data-stu-id="aa93f-113">Name the resource group in which to create your cache.</span></span> <span data-ttu-id="aa93f-114">Inserendo tutte le risorse per un'app in un gruppo è possibile gestirle insieme.</span><span class="sxs-lookup"><span data-stu-id="aa93f-114">By putting all the resources for an app in a group, you can manage them together.</span></span>

1. <span data-ttu-id="aa93f-115">Posizionare sempre l'istanza della cache e l'applicazione nella stessa area/località.</span><span class="sxs-lookup"><span data-stu-id="aa93f-115">Always place your cache instance and your application in the same region / location.</span></span> <span data-ttu-id="aa93f-116">La connessione a una cache in un'area diversa può aumentare in modo significativo la latenza e ridurre l'affidabilità.</span><span class="sxs-lookup"><span data-stu-id="aa93f-116">Connecting to a cache in a different region can significantly increase latency and reduce reliability.</span></span> <span data-ttu-id="aa93f-117">Se ci si connette alla cache all'esterno di Azure, selezionare una posizione vicina alla località in cui è in esecuzione l'applicazione che utilizza i dati.</span><span class="sxs-lookup"><span data-stu-id="aa93f-117">If you are connecting to the cache outside of Azure, then select a location close to where the application consuming the data is running.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="aa93f-118">È molto più importante che la cache sia vicino al consumer di dati piuttosto che all'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="aa93f-118">It's far more important that the cache is close to the data consumer than the data store.</span></span>

1. <span data-ttu-id="aa93f-119">Selezionare il piano tariffario.</span><span class="sxs-lookup"><span data-stu-id="aa93f-119">Select the pricing tier.</span></span> 
    - <span data-ttu-id="aa93f-120">È consigliabile usare sempre il livello Premium o Standard per i sistemi di produzione.</span><span class="sxs-lookup"><span data-stu-id="aa93f-120">We recommend you always use Standard or Premium Tier for production systems.</span></span> <span data-ttu-id="aa93f-121">Il livello Basic è un sistema a nodo singolo senza replica dei dati e senza contratto di servizio.</span><span class="sxs-lookup"><span data-stu-id="aa93f-121">The Basic Tier is a single node system with no data replication and no SLA.</span></span> <span data-ttu-id="aa93f-122">Usare almeno una cache di livello C1.</span><span class="sxs-lookup"><span data-stu-id="aa93f-122">Also, use at least a C1 cache.</span></span> <span data-ttu-id="aa93f-123">Le cache di livello C0 sono destinate effettivamente a semplici scenari di sviluppo/test, perché hanno un core di CPU condiviso e una memoria molto ridotta.</span><span class="sxs-lookup"><span data-stu-id="aa93f-123">C0 caches are really meant for simple dev/test scenarios since they have a shared CPU core and very little memory.</span></span>

    - <span data-ttu-id="aa93f-124">Il livello Premium consente di rendere persistenti i dati in due modi per offrire il ripristino di emergenza:</span><span class="sxs-lookup"><span data-stu-id="aa93f-124">The Premium tier allows you to persist data in two ways to provide disaster recovery:</span></span>

        - <span data-ttu-id="aa93f-125">Con la persistenza RDB viene creato uno snapshot periodico ed è possibile ricostruire la cache usando lo snapshot in caso di errore.</span><span class="sxs-lookup"><span data-stu-id="aa93f-125">RDB persistence takes a periodic snapshot and can rebuild the cache using the snapshot in case of failure.</span></span>

            ![Screenshot del portale di Azure che illustra le opzioni di persistenza RDB per una nuova istanza di Cache Redis.](../media/3-redis-persistence-1.png)

        - <span data-ttu-id="aa93f-127">La persistenza AOF salva ogni operazione di scrittura in un log che viene salvato almeno una volta al secondo.</span><span class="sxs-lookup"><span data-stu-id="aa93f-127">AOF persistence saves every write operation to a log that is saved at least once per second.</span></span> <span data-ttu-id="aa93f-128">Vengono così creati file di dimensioni maggiori rispetto a RDB ma con minore perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="aa93f-128">This creates bigger files than RDB but has less data loss.</span></span>

            ![Screenshot del portale di Azure che illustra le opzioni di persistenza AOF per una nuova istanza di Cache Redis.](../media/3-redis-persistence-2.png)

    - <span data-ttu-id="aa93f-130">Proteggere la cache con una rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="aa93f-130">Secure your cache with a virtual network.</span></span>
      <span data-ttu-id="aa93f-131">Se è disponibile una cache Redis di livello Premium, è possibile distribuirla in una rete virtuale nel cloud.</span><span class="sxs-lookup"><span data-stu-id="aa93f-131">If you have a premium tier Redis cache, you can deploy it to a virtual network in the cloud.</span></span> <span data-ttu-id="aa93f-132">La cache sarà disponibile solo per altre macchine virtuali e applicazioni nella stessa rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="aa93f-132">Your cache will be available to only other virtual machines and applications in the same virtual network.</span></span>

    - <span data-ttu-id="aa93f-133">Distribuire la cache con il clustering.</span><span class="sxs-lookup"><span data-stu-id="aa93f-133">Distribute your cache with clustering.</span></span>
      <span data-ttu-id="aa93f-134">Con una cache Redis di livello Premium, è possibile implementare il clustering per suddividere automaticamente i set di dati tra più nodi.</span><span class="sxs-lookup"><span data-stu-id="aa93f-134">With a premium tier Redis cache, you can implement clustering to automatically split your dataset among multiple nodes.</span></span> <span data-ttu-id="aa93f-135">Per implementare il clustering, specificare il numero di partizioni per un massimo di 10.</span><span class="sxs-lookup"><span data-stu-id="aa93f-135">To implement clustering, you specify the number of shards to a maximum of 10.</span></span> <span data-ttu-id="aa93f-136">Il costo addebitato è il costo del nodo originale moltiplicato per il numero di partizioni.</span><span class="sxs-lookup"><span data-stu-id="aa93f-136">The cost incurred is the cost of the original node, multiplied by the number of shards.</span></span>

    - <span data-ttu-id="aa93f-137">Eseguire la migrazione dal Servizio cache gestita di Azure.</span><span class="sxs-lookup"><span data-stu-id="aa93f-137">Migrate from Azure Managed Cache Service.</span></span>
      <span data-ttu-id="aa93f-138">A seconda delle funzionalità del Servizio cache gestita usate, la migrazione delle applicazioni che usano il Servizio cache gestita di Azure in Cache Redis di Azure può essere eseguita con modifiche minime all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aa93f-138">Depending on the Managed Cache Service features you use, migrating applications that use Azure Managed Cache Service to Azure Redis Cache can be accomplished with minimal changes to your application.</span></span> <span data-ttu-id="aa93f-139">Anche se le API non sono esattamente uguali, sono simili.</span><span class="sxs-lookup"><span data-stu-id="aa93f-139">While the APIs aren't exactly the same, they're similar.</span></span> <span data-ttu-id="aa93f-140">Gran parte del codice esistente che accede alla cache con il Servizio cache gestita può essere riutilizzata con modifiche minime.</span><span class="sxs-lookup"><span data-stu-id="aa93f-140">Much of your existing code that accesses the cache with Managed Cache Service can be reused with minimal changes.</span></span>

## <a name="accessing-the-redis-instance"></a><span data-ttu-id="aa93f-141">Accesso all'istanza di Redis</span><span class="sxs-lookup"><span data-stu-id="aa93f-141">Accessing the Redis instance</span></span>

<span data-ttu-id="aa93f-142">Redis supporta un set di [comandi noti](https://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="aa93f-142">Redis supports a set of [known commands](https://redis.io/commands).</span></span> <span data-ttu-id="aa93f-143">Un comando viene in genere eseguito come `COMMAND parameter1 parameter2 parameter3`.</span><span class="sxs-lookup"><span data-stu-id="aa93f-143">A command is typically issued as `COMMAND parameter1 parameter2 parameter3`.</span></span>

<span data-ttu-id="aa93f-144">Ecco alcuni comandi comuni che è possibile usare:</span><span class="sxs-lookup"><span data-stu-id="aa93f-144">Here are some common commands you can use:</span></span>

| <span data-ttu-id="aa93f-145">Comando</span><span class="sxs-lookup"><span data-stu-id="aa93f-145">Command</span></span> | <span data-ttu-id="aa93f-146">Descrizione</span><span class="sxs-lookup"><span data-stu-id="aa93f-146">Description</span></span> |
|---------|-------------|
| `ping` | <span data-ttu-id="aa93f-147">Consente di effettuare il ping del server.</span><span class="sxs-lookup"><span data-stu-id="aa93f-147">Ping the server.</span></span> <span data-ttu-id="aa93f-148">Restituisce "PONG".</span><span class="sxs-lookup"><span data-stu-id="aa93f-148">Returns "PONG".</span></span> |
| `set [key] [value]` | <span data-ttu-id="aa93f-149">Consente di impostare una coppia chiave/valore nella cache.</span><span class="sxs-lookup"><span data-stu-id="aa93f-149">Sets a key/value in the cache.</span></span> <span data-ttu-id="aa93f-150">Restituisce "OK" in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="aa93f-150">Returns "OK" on success.</span></span> |
| `get [key]` | <span data-ttu-id="aa93f-151">Consente di ottenere un valore dalla cache.</span><span class="sxs-lookup"><span data-stu-id="aa93f-151">Gets a value from the cache.</span></span> |
| `exists [key]` | <span data-ttu-id="aa93f-152">Restituisce "1" se il valore **key** esiste nella cache, "0" se non esiste.</span><span class="sxs-lookup"><span data-stu-id="aa93f-152">Returns '1' if the **key** exists in the cache, '0' if it doesn't.</span></span> |
| `type [key]` | <span data-ttu-id="aa93f-153">Restituisce il tipo associato al valore per il valore **key** specificato.</span><span class="sxs-lookup"><span data-stu-id="aa93f-153">Returns the type associated to the value for the given **key**.</span></span> |
| `incr [key]` | <span data-ttu-id="aa93f-154">Consente di incrementare il valore specificato associato a **key** di "1".</span><span class="sxs-lookup"><span data-stu-id="aa93f-154">Increment the given value associated with **key** by \`1'.</span></span> <span data-ttu-id="aa93f-155">Il valore deve essere di tipo Integer o Double.</span><span class="sxs-lookup"><span data-stu-id="aa93f-155">The value must be an integer or double value.</span></span> <span data-ttu-id="aa93f-156">Restituisce il nuovo valore.</span><span class="sxs-lookup"><span data-stu-id="aa93f-156">This returns the new value.</span></span> |
| `incrby [key] [amount]` | <span data-ttu-id="aa93f-157">Consente di incrementare il valore specificato associato a **key** in base alla quantità specificata.</span><span class="sxs-lookup"><span data-stu-id="aa93f-157">Increment the given value associated with **key** by the specified amount.</span></span> <span data-ttu-id="aa93f-158">Il valore deve essere di tipo Integer o Double.</span><span class="sxs-lookup"><span data-stu-id="aa93f-158">The value must be an integer or double value.</span></span> <span data-ttu-id="aa93f-159">Restituisce il nuovo valore.</span><span class="sxs-lookup"><span data-stu-id="aa93f-159">This returns the new value.</span></span> |
| `del [key]` | <span data-ttu-id="aa93f-160">Consente di eliminare il valore associato a **key**.</span><span class="sxs-lookup"><span data-stu-id="aa93f-160">Deletes the value associated with the **key**.</span></span> |
| `flushdb` | <span data-ttu-id="aa93f-161">Consente di eliminare _ogni_ chiave e valore nel database.</span><span class="sxs-lookup"><span data-stu-id="aa93f-161">Delete _all_ keys and values in the database.</span></span> |

<span data-ttu-id="aa93f-162">Redis include uno strumento da riga di comando (**redis-cli**) che può essere usato per sperimentare direttamente con questi comandi.</span><span class="sxs-lookup"><span data-stu-id="aa93f-162">Redis has a command-line tool (**redis-cli**) you can use to experiment directly with these commands.</span></span> <span data-ttu-id="aa93f-163">Di seguito sono riportati alcuni esempi.</span><span class="sxs-lookup"><span data-stu-id="aa93f-163">Here are some examples.</span></span>

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

<span data-ttu-id="aa93f-164">Ecco un esempio relativo all'uso dei comandi `INCR`.</span><span class="sxs-lookup"><span data-stu-id="aa93f-164">Here's an example of working with the `INCR` commands.</span></span> <span data-ttu-id="aa93f-165">Sono utili perché forniscono incrementi atomici _in più applicazioni_ che usano la cache.</span><span class="sxs-lookup"><span data-stu-id="aa93f-165">These are convenient because they provide atomic increments _across multiple applications_ that are using the cache.</span></span>

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

### <a name="adding-an-expiration-time-to-values"></a><span data-ttu-id="aa93f-166">Aggiunta di una scadenza ai valori</span><span class="sxs-lookup"><span data-stu-id="aa93f-166">Adding an expiration time to values</span></span>

<span data-ttu-id="aa93f-167">La memorizzazione nella cache è importante perché consente di archiviare in memoria i valori più usati.</span><span class="sxs-lookup"><span data-stu-id="aa93f-167">Caching is important because it allows us to store commonly used values in memory.</span></span> <span data-ttu-id="aa93f-168">È tuttavia necessario anche un modo per imporre la scadenza dei valori quando sono obsoleti.</span><span class="sxs-lookup"><span data-stu-id="aa93f-168">However, we also need a way to expire values when they are stale.</span></span> <span data-ttu-id="aa93f-169">In Redis è possibile ottenere questo risultato applicando una _durata_ (TTL) a una chiave.</span><span class="sxs-lookup"><span data-stu-id="aa93f-169">In Redis this is done by applying a _time to live_ (TTL) to a key.</span></span>

<span data-ttu-id="aa93f-170">Allo scadere del valore TTL, la chiave viene eliminata automaticamente, esattamente come se fosse stato eseguito il comando DEL.</span><span class="sxs-lookup"><span data-stu-id="aa93f-170">When the TTL elapses, the key is automatically deleted, exactly as if the DEL command were issued.</span></span> <span data-ttu-id="aa93f-171">Ecco alcune note sulle scadenze del valore TTL.</span><span class="sxs-lookup"><span data-stu-id="aa93f-171">Here are some notes on TTL expirations.</span></span>

- <span data-ttu-id="aa93f-172">Le scadenze possono essere impostate con una precisione di secondi o millisecondi.</span><span class="sxs-lookup"><span data-stu-id="aa93f-172">Expirations can be set using seconds or milliseconds precision.</span></span>
- <span data-ttu-id="aa93f-173">La risoluzione dell'ora di scadenza è sempre pari a 1 millisecondo.</span><span class="sxs-lookup"><span data-stu-id="aa93f-173">The expire time resolution is always 1 millisecond.</span></span>
- <span data-ttu-id="aa93f-174">Le informazioni sulle scadenze vengono replicate e salvate in modo permanente su disco. Il tempo trascorre effettivamente quando il server Redis rimane in stato arrestato. Redis salva in effetti la data della scadenza della chiave.</span><span class="sxs-lookup"><span data-stu-id="aa93f-174">Information about expires are replicated and persisted on disk, the time virtually passes when your Redis server remains stopped (this means that Redis saves the date at which a key will expire).</span></span>

<span data-ttu-id="aa93f-175">Ecco un esempio di scadenza:</span><span class="sxs-lookup"><span data-stu-id="aa93f-175">Here is an example of an expiration:</span></span>

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

## <a name="accessing-a-redis-cache-from-a-client"></a><span data-ttu-id="aa93f-176">Accesso a una cache Redis da un client</span><span class="sxs-lookup"><span data-stu-id="aa93f-176">Accessing a Redis cache from a client</span></span>

<span data-ttu-id="aa93f-177">Per connettersi a un'istanza di Cache Redis di Azure, i client necessitano di nome host, porta e chiave di accesso per la cache.</span><span class="sxs-lookup"><span data-stu-id="aa93f-177">When connecting to an Azure Redis Cache instance, clients need the host name, port, and an access key for the cache.</span></span> <span data-ttu-id="aa93f-178">È possibile recuperare queste informazioni nel portale di Azure tramite la pagina **Impostazioni > Chiavi di accesso**.</span><span class="sxs-lookup"><span data-stu-id="aa93f-178">You can retrieve this information in the Azure portal through the **Settings > Access Keys** page.</span></span> 

- <span data-ttu-id="aa93f-179">Il nome host è l'indirizzo Internet pubblico della cache, creato usando il nome della cache.</span><span class="sxs-lookup"><span data-stu-id="aa93f-179">The host name is the public Internet address of your cache, which was created using the name of the cache.</span></span> <span data-ttu-id="aa93f-180">Ad esempio, `sportsresults.redis.cache.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="aa93f-180">For example `sportsresults.redis.cache.windows.net`.</span></span>

- <span data-ttu-id="aa93f-181">La chiave di accesso funge da password per la cache.</span><span class="sxs-lookup"><span data-stu-id="aa93f-181">The access key acts as a password for your cache.</span></span> <span data-ttu-id="aa93f-182">Vengono create due chiavi, primaria e secondaria.</span><span class="sxs-lookup"><span data-stu-id="aa93f-182">There are two keys created: primary and secondary.</span></span> <span data-ttu-id="aa93f-183">È possibile usare una delle due chiavi. Ne vengono fornite due nel caso diventi necessario modificare la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="aa93f-183">You can use either key, two are provided in case you need to change the primary key.</span></span> <span data-ttu-id="aa93f-184">È possibile passare tutti i client alla chiave secondaria e rigenerare la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="aa93f-184">You can switch all of your clients to the secondary key, and regenerate the primary key.</span></span> <span data-ttu-id="aa93f-185">Questa operazione blocca eventuali applicazioni che usano la chiave primaria originale.</span><span class="sxs-lookup"><span data-stu-id="aa93f-185">This would block any applications using the original primary key.</span></span> <span data-ttu-id="aa93f-186">Microsoft consiglia di rigenerare periodicamente le chiavi, analogamente alle password perdonali.</span><span class="sxs-lookup"><span data-stu-id="aa93f-186">Microsoft recommends periodically regenerating the keys - much like you would your personal passwords.</span></span>

> [!WARNING]
> <span data-ttu-id="aa93f-187">Le chiavi di accesso devono essere considerate informazioni riservate, proprio come una password.</span><span class="sxs-lookup"><span data-stu-id="aa93f-187">Your access keys should be considered confidential information, treat them like you would a password.</span></span> <span data-ttu-id="aa93f-188">Chiunque abbia una chiave di accesso può eseguire qualsiasi operazione sulla cache.</span><span class="sxs-lookup"><span data-stu-id="aa93f-188">Anyone who has an access key can perform any operation on your cache!</span></span>
