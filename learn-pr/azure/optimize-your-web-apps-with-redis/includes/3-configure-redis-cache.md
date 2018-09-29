<span data-ttu-id="8063d-101">Il team di sviluppo dell'app per le statistiche sportive ha deciso che la memorizzazione nella cache potrebbe migliorare notevolmente le prestazioni per i dati richiesti di recente.</span><span class="sxs-lookup"><span data-stu-id="8063d-101">Your sports statistics development team has decided that caching could dramatically improve performance for recently requested data.</span></span> <span data-ttu-id="8063d-102">Il passaggio successivo consiste nel creare e configurare un'istanza di Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="8063d-102">Your next step is to create and configure an Azure Redis Cache instance.</span></span>

## <a name="create-and-configure-the-azure-redis-cache-instance"></a><span data-ttu-id="8063d-103">Creare e configurare l'istanza di Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="8063d-103">Create and configure the Azure Redis Cache instance</span></span>

<span data-ttu-id="8063d-104">È possibile creare una cache Redis con il portale di Azure, l'interfaccia della riga di comando di Azure o Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8063d-104">You can create a Redis cache using the Azure portal, the Azure CLI, or Azure PowerShell.</span></span> <span data-ttu-id="8063d-105">Per configurare correttamente la cache per gli scopi desiderati è necessario impostare vari parametri.</span><span class="sxs-lookup"><span data-stu-id="8063d-105">There are several parameters you will need to decide in order to configure the cache properly for your purposes.</span></span>

### <a name="name"></a><span data-ttu-id="8063d-106">Nome</span><span class="sxs-lookup"><span data-stu-id="8063d-106">Name</span></span>

<span data-ttu-id="8063d-107">Cache Redis dovrà avere un nome univoco a livello globale.</span><span class="sxs-lookup"><span data-stu-id="8063d-107">The Redis cache will need a globally unique name.</span></span> <span data-ttu-id="8063d-108">Il nome deve essere univoco all'interno di Azure perché è usato per generare un URL pubblico per connettersi e comunicare con il servizio.</span><span class="sxs-lookup"><span data-stu-id="8063d-108">The name has to be unique within Azure because it is used to generate a public-facing URL to connect and communicate with the service.</span></span>

<span data-ttu-id="8063d-109">Il nome deve contenere da 1 a 63 caratteri, che possono includere numeri, lettere e il carattere "-".</span><span class="sxs-lookup"><span data-stu-id="8063d-109">The name must be between 1 and 63 characters, composed of numbers, letters, and the '-' character.</span></span> <span data-ttu-id="8063d-110">Il nome della cache non può iniziare o terminare con il carattere "-" e più caratteri "-" consecutivi non sono validi.</span><span class="sxs-lookup"><span data-stu-id="8063d-110">The cache name can't start or end with the '-' character, and consecutive '-' characters aren't valid.</span></span>

### <a name="resource-group"></a><span data-ttu-id="8063d-111">Gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="8063d-111">Resource Group</span></span>

<span data-ttu-id="8063d-112">Cache Redis di Azure è una risorsa gestita e deve disporre di un proprietario del _gruppo di risorse_.</span><span class="sxs-lookup"><span data-stu-id="8063d-112">The Azure Redis Cache is a managed resource and needs a _resource group_ owner.</span></span> <span data-ttu-id="8063d-113">È possibile creare un nuovo gruppo di risorse o selezionarne uno esistente all'interno della propria sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="8063d-113">You can either create a new resource group, or use an existing one in a subscription you are part of.</span></span>

### <a name="location"></a><span data-ttu-id="8063d-114">Località</span><span class="sxs-lookup"><span data-stu-id="8063d-114">Location</span></span>

<span data-ttu-id="8063d-115">È necessario stabilire dove verrà posizionata fisicamente la Cache Redis selezionando un'area di Azure.</span><span class="sxs-lookup"><span data-stu-id="8063d-115">You will need to decide where the Redis cache will be physically located by selecting an Azure region.</span></span> <span data-ttu-id="8063d-116">Posizionare sempre l'istanza della cache e l'applicazione nella stessa area.</span><span class="sxs-lookup"><span data-stu-id="8063d-116">You should always place your cache instance and your application in the same region.</span></span> <span data-ttu-id="8063d-117">La connessione a una cache in un'area diversa può aumentare in modo significativo la latenza e ridurre l'affidabilità.</span><span class="sxs-lookup"><span data-stu-id="8063d-117">Connecting to a cache in a different region can significantly increase latency and reduce reliability.</span></span> <span data-ttu-id="8063d-118">Se ci si connette alla cache all'esterno di Azure, selezionare una posizione vicina alla località in cui è in esecuzione l'applicazione che utilizza i dati.</span><span class="sxs-lookup"><span data-stu-id="8063d-118">If you are connecting to the cache outside of Azure, then select a location close to where the application consuming the data is running.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8063d-119">Inserire la Cache Redis il più vicino possibile al _consumer_ di dati.</span><span class="sxs-lookup"><span data-stu-id="8063d-119">Put the Redis cache as close to the data _consumer_ as you can.</span></span>

### <a name="pricing-tier"></a><span data-ttu-id="8063d-120">Piano tariffario</span><span class="sxs-lookup"><span data-stu-id="8063d-120">Pricing tier</span></span>

<span data-ttu-id="8063d-121">Come indicato nell'ultima unità, per la Cache Redis di Azure sono disponibili tre piani tariffari.</span><span class="sxs-lookup"><span data-stu-id="8063d-121">As mentioned in the last unit, there are three pricing tiers available for an Azure Redis Cache.</span></span>

- <span data-ttu-id="8063d-122">**Basic**: cache di base ideale per sviluppo e testing.</span><span class="sxs-lookup"><span data-stu-id="8063d-122">**Basic**: Basic cache ideal for development/testing.</span></span> <span data-ttu-id="8063d-123">Prevede un limite di 20.000 connessioni, 53 GB di memoria e un singolo server.</span><span class="sxs-lookup"><span data-stu-id="8063d-123">Is limited to a single server, 53 GB of memory, and 20,000 connections.</span></span> <span data-ttu-id="8063d-124">Questo livello di servizio non prevede alcun contratto di servizio.</span><span class="sxs-lookup"><span data-stu-id="8063d-124">There is no SLA for this service tier.</span></span>
- <span data-ttu-id="8063d-125">**Standard**: cache di produzione che supporta la replica e include un contratto di servizio del 99,99%.</span><span class="sxs-lookup"><span data-stu-id="8063d-125">**Standard**: Production cache which supports replication and includes an 99.99% SLA.</span></span> <span data-ttu-id="8063d-126">Supporta due server, master e slave, e ha gli stessi limiti di memoria/connessione del livello Basic.</span><span class="sxs-lookup"><span data-stu-id="8063d-126">It supports two servers (master/slave), and has the same memory/connection limits as the Basic tier.</span></span>
- <span data-ttu-id="8063d-127">**Premium**: livello aziendale che si basa sul livello Standard e include il supporto di persistenza, clustering e scalabilità orizzontale della cache.</span><span class="sxs-lookup"><span data-stu-id="8063d-127">**Premium**: Enterprise tier which builds on the Standard tier and includes persistence, clustering, and scale-out cache support.</span></span> <span data-ttu-id="8063d-128">Questo è il livello di prestazioni più elevato, con 40.000 connessioni simultanee e fino a 530 GB di memoria.</span><span class="sxs-lookup"><span data-stu-id="8063d-128">This is the highest performing tier with up to 530 GB of memory and 40,000 simultaneous connections.</span></span>

<span data-ttu-id="8063d-129">È possibile controllare la quantità di memoria cache disponibile in ogni livello, selezionando un livello di cache C0 a C6 per Basic e Standard e da P0 a P4 per Premium.</span><span class="sxs-lookup"><span data-stu-id="8063d-129">You can control the amount of cache memory available on each tier - this is selected by choosing a cache level from C0-C6 for Basic/Standard and P0-P4 for Premium.</span></span> <span data-ttu-id="8063d-130">Per tutti i dettagli, vedere la pagina [relativa ai prezzi](https://azure.microsoft.com/pricing/details/cache/).</span><span class="sxs-lookup"><span data-stu-id="8063d-130">Check the [pricing page](https://azure.microsoft.com/pricing/details/cache/) for full details.</span></span>

> [!TIP]
> <span data-ttu-id="8063d-131">È consigliabile usare sempre il livello Premium o Standard per i sistemi di produzione.</span><span class="sxs-lookup"><span data-stu-id="8063d-131">Microsoft recommends you always use Standard or Premium Tier for production systems.</span></span> <span data-ttu-id="8063d-132">Il livello Basic è un sistema a nodo singolo senza replica dei dati e senza contratto di servizio.</span><span class="sxs-lookup"><span data-stu-id="8063d-132">The Basic Tier is a single node system with no data replication and no SLA.</span></span> <span data-ttu-id="8063d-133">Usare almeno una cache di livello C1.</span><span class="sxs-lookup"><span data-stu-id="8063d-133">Also, use at least a C1 cache.</span></span> <span data-ttu-id="8063d-134">Le cache di livello C0 sono destinate effettivamente a semplici scenari di sviluppo/test, perché hanno un core di CPU condiviso e una memoria molto ridotta.</span><span class="sxs-lookup"><span data-stu-id="8063d-134">C0 caches are really meant for simple dev/test scenarios since they have a shared CPU core and very little memory.</span></span>

<span data-ttu-id="8063d-135">Il livello Premium consente di rendere persistenti i dati in due modi per offrire il ripristino di emergenza:</span><span class="sxs-lookup"><span data-stu-id="8063d-135">The Premium tier allows you to persist data in two ways to provide disaster recovery:</span></span>

1. <span data-ttu-id="8063d-136">Con la persistenza RDB viene creato uno snapshot periodico ed è possibile ricostruire la cache usando lo snapshot in caso di errore.</span><span class="sxs-lookup"><span data-stu-id="8063d-136">RDB persistence takes a periodic snapshot and can rebuild the cache using the snapshot in case of failure.</span></span>

    ![Screenshot del portale di Azure che illustra le opzioni di persistenza RDB per una nuova istanza di Cache Redis.](../media/3-redis-persistence-1.png)

2. <span data-ttu-id="8063d-138">La persistenza AOF salva ogni operazione di scrittura in un log che viene salvato almeno una volta al secondo.</span><span class="sxs-lookup"><span data-stu-id="8063d-138">AOF persistence saves every write operation to a log that is saved at least once per second.</span></span> <span data-ttu-id="8063d-139">Vengono così creati file di dimensioni maggiori rispetto a RDB ma con minore perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="8063d-139">This creates bigger files than RDB but has less data loss.</span></span>

    ![Screenshot del portale di Azure che illustra le opzioni di persistenza AOF per una nuova istanza di Cache Redis.](../media/3-redis-persistence-2.png)

<span data-ttu-id="8063d-141">Esistono diverse altre impostazioni, disponibili solo per il livello **Premium**.</span><span class="sxs-lookup"><span data-stu-id="8063d-141">There are several other settings which are only available to the **Premium** tier.</span></span>

### <a name="virtual-network-support"></a><span data-ttu-id="8063d-142">Supporto della rete virtuale</span><span class="sxs-lookup"><span data-stu-id="8063d-142">Virtual Network support</span></span>

<span data-ttu-id="8063d-143">Se si crea una Cache Redis di livello Premium, è possibile distribuirla in una rete virtuale nel cloud.</span><span class="sxs-lookup"><span data-stu-id="8063d-143">If you create a premium tier Redis cache, you can deploy it to a virtual network in the cloud.</span></span> <span data-ttu-id="8063d-144">La cache sarà disponibile solo per altre macchine virtuali e applicazioni nella stessa rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="8063d-144">Your cache will be available to only other virtual machines and applications in the same virtual network.</span></span> <span data-ttu-id="8063d-145">Questo offre un livello di sicurezza superiore quando sia il servizio che la cache sono ospitati in Azure oppure sono connessi tramite una VPN di rete virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="8063d-145">This provides a higher level of security when your service and cache are both hosted in Azure, or are connected through an Azure virtual network VPN.</span></span>

### <a name="clustering-support"></a><span data-ttu-id="8063d-146">Supporto del clustering</span><span class="sxs-lookup"><span data-stu-id="8063d-146">Clustering support</span></span>

<span data-ttu-id="8063d-147">Con una cache Redis di livello Premium, è possibile implementare il clustering per suddividere automaticamente i set di dati tra più nodi.</span><span class="sxs-lookup"><span data-stu-id="8063d-147">With a premium tier Redis cache, you can implement clustering to automatically split your dataset among multiple nodes.</span></span> <span data-ttu-id="8063d-148">Per implementare il clustering, specificare il numero di partizioni per un massimo di 10.</span><span class="sxs-lookup"><span data-stu-id="8063d-148">To implement clustering, you specify the number of shards to a maximum of 10.</span></span> <span data-ttu-id="8063d-149">Il costo addebitato è il costo del nodo originale moltiplicato per il numero di partizioni.</span><span class="sxs-lookup"><span data-stu-id="8063d-149">The cost incurred is the cost of the original node, multiplied by the number of shards.</span></span>

## <a name="accessing-the-redis-instance"></a><span data-ttu-id="8063d-150">Accesso all'istanza di Redis</span><span class="sxs-lookup"><span data-stu-id="8063d-150">Accessing the Redis instance</span></span>

<span data-ttu-id="8063d-151">Redis supporta un set di [comandi noti](https://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="8063d-151">Redis supports a set of [known commands](https://redis.io/commands).</span></span> <span data-ttu-id="8063d-152">Un comando viene in genere eseguito come `COMMAND parameter1 parameter2 parameter3`.</span><span class="sxs-lookup"><span data-stu-id="8063d-152">A command is typically issued as `COMMAND parameter1 parameter2 parameter3`.</span></span>

<span data-ttu-id="8063d-153">Ecco alcuni comandi comuni che è possibile usare:</span><span class="sxs-lookup"><span data-stu-id="8063d-153">Here are some common commands you can use:</span></span>

| <span data-ttu-id="8063d-154">Comando</span><span class="sxs-lookup"><span data-stu-id="8063d-154">Command</span></span> | <span data-ttu-id="8063d-155">Descrizione</span><span class="sxs-lookup"><span data-stu-id="8063d-155">Description</span></span> |
|---------|-------------|
| `ping` | <span data-ttu-id="8063d-156">Consente di effettuare il ping del server.</span><span class="sxs-lookup"><span data-stu-id="8063d-156">Ping the server.</span></span> <span data-ttu-id="8063d-157">Restituisce "PONG".</span><span class="sxs-lookup"><span data-stu-id="8063d-157">Returns "PONG".</span></span> |
| `set [key] [value]` | <span data-ttu-id="8063d-158">Consente di impostare una coppia chiave/valore nella cache.</span><span class="sxs-lookup"><span data-stu-id="8063d-158">Sets a key/value in the cache.</span></span> <span data-ttu-id="8063d-159">Restituisce "OK" in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="8063d-159">Returns "OK" on success.</span></span> |
| `get [key]` | <span data-ttu-id="8063d-160">Consente di ottenere un valore dalla cache.</span><span class="sxs-lookup"><span data-stu-id="8063d-160">Gets a value from the cache.</span></span> |
| `exists [key]` | <span data-ttu-id="8063d-161">Restituisce "1" se il valore **key** esiste nella cache, "0" se non esiste.</span><span class="sxs-lookup"><span data-stu-id="8063d-161">Returns '1' if the **key** exists in the cache, '0' if it doesn't.</span></span> |
| `type [key]` | <span data-ttu-id="8063d-162">Restituisce il tipo associato al valore per il valore **key** specificato.</span><span class="sxs-lookup"><span data-stu-id="8063d-162">Returns the type associated to the value for the given **key**.</span></span> |
| `incr [key]` | <span data-ttu-id="8063d-163">Consente di incrementare il valore specificato associato a **key** di "1".</span><span class="sxs-lookup"><span data-stu-id="8063d-163">Increment the given value associated with **key** by '1'.</span></span> <span data-ttu-id="8063d-164">Il valore deve essere di tipo Integer o Double.</span><span class="sxs-lookup"><span data-stu-id="8063d-164">The value must be an integer or double value.</span></span> <span data-ttu-id="8063d-165">Restituisce il nuovo valore.</span><span class="sxs-lookup"><span data-stu-id="8063d-165">This returns the new value.</span></span> |
| `incrby [key] [amount]` | <span data-ttu-id="8063d-166">Consente di incrementare il valore specificato associato a **key** in base alla quantità specificata.</span><span class="sxs-lookup"><span data-stu-id="8063d-166">Increment the given value associated with **key** by the specified amount.</span></span> <span data-ttu-id="8063d-167">Il valore deve essere di tipo Integer o Double.</span><span class="sxs-lookup"><span data-stu-id="8063d-167">The value must be an integer or double value.</span></span> <span data-ttu-id="8063d-168">Restituisce il nuovo valore.</span><span class="sxs-lookup"><span data-stu-id="8063d-168">This returns the new value.</span></span> |
| `del [key]` | <span data-ttu-id="8063d-169">Consente di eliminare il valore associato a **key**.</span><span class="sxs-lookup"><span data-stu-id="8063d-169">Deletes the value associated with the **key**.</span></span> |
| `flushdb` | <span data-ttu-id="8063d-170">Consente di eliminare _ogni_ chiave e valore nel database.</span><span class="sxs-lookup"><span data-stu-id="8063d-170">Delete _all_ keys and values in the database.</span></span> |

<span data-ttu-id="8063d-171">Redis include uno strumento da riga di comando (**redis-cli**) che può essere usato per sperimentare direttamente con questi comandi.</span><span class="sxs-lookup"><span data-stu-id="8063d-171">Redis has a command-line tool (**redis-cli**) you can use to experiment directly with these commands.</span></span> <span data-ttu-id="8063d-172">Di seguito sono riportati alcuni esempi.</span><span class="sxs-lookup"><span data-stu-id="8063d-172">Here are some examples.</span></span>

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

<span data-ttu-id="8063d-173">Ecco un esempio relativo all'uso dei comandi `INCR`.</span><span class="sxs-lookup"><span data-stu-id="8063d-173">Here's an example of working with the `INCR` commands.</span></span> <span data-ttu-id="8063d-174">Sono utili perché forniscono incrementi atomici _in più applicazioni_ che usano la cache.</span><span class="sxs-lookup"><span data-stu-id="8063d-174">These are convenient because they provide atomic increments _across multiple applications_ that are using the cache.</span></span>

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

### <a name="adding-an-expiration-time-to-values"></a><span data-ttu-id="8063d-175">Aggiunta di una scadenza ai valori</span><span class="sxs-lookup"><span data-stu-id="8063d-175">Adding an expiration time to values</span></span>

<span data-ttu-id="8063d-176">La memorizzazione nella cache è importante perché consente di archiviare in memoria i valori più usati.</span><span class="sxs-lookup"><span data-stu-id="8063d-176">Caching is important because it allows us to store commonly used values in memory.</span></span> <span data-ttu-id="8063d-177">È tuttavia necessario anche un modo per imporre la scadenza dei valori quando sono obsoleti.</span><span class="sxs-lookup"><span data-stu-id="8063d-177">However, we also need a way to expire values when they are stale.</span></span> <span data-ttu-id="8063d-178">In Redis è possibile ottenere questo risultato applicando una _durata_ (TTL) a una chiave.</span><span class="sxs-lookup"><span data-stu-id="8063d-178">In Redis this is done by applying a _time to live_ (TTL) to a key.</span></span>

<span data-ttu-id="8063d-179">Allo scadere del valore TTL, la chiave viene eliminata automaticamente, esattamente come se fosse stato eseguito il comando DEL.</span><span class="sxs-lookup"><span data-stu-id="8063d-179">When the TTL elapses, the key is automatically deleted, exactly as if the DEL command were issued.</span></span> <span data-ttu-id="8063d-180">Ecco alcune note sulle scadenze del valore TTL.</span><span class="sxs-lookup"><span data-stu-id="8063d-180">Here are some notes on TTL expirations.</span></span>

- <span data-ttu-id="8063d-181">Le scadenze possono essere impostate con una precisione di secondi o millisecondi.</span><span class="sxs-lookup"><span data-stu-id="8063d-181">Expirations can be set using seconds or milliseconds precision.</span></span>
- <span data-ttu-id="8063d-182">La risoluzione dell'ora di scadenza è sempre pari a 1 millisecondo.</span><span class="sxs-lookup"><span data-stu-id="8063d-182">The expire time resolution is always 1 millisecond.</span></span>
- <span data-ttu-id="8063d-183">Le informazioni sulle scadenze vengono replicate e salvate in modo permanente su disco. Il tempo trascorre effettivamente quando il server Redis rimane in stato arrestato. Redis salva in effetti la data della scadenza della chiave.</span><span class="sxs-lookup"><span data-stu-id="8063d-183">Information about expires are replicated and persisted on disk, the time virtually passes when your Redis server remains stopped (this means that Redis saves the date at which a key will expire).</span></span>

<span data-ttu-id="8063d-184">Ecco un esempio di scadenza:</span><span class="sxs-lookup"><span data-stu-id="8063d-184">Here is an example of an expiration:</span></span>

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

## <a name="accessing-a-redis-cache-from-a-client"></a><span data-ttu-id="8063d-185">Accesso a una cache Redis da un client</span><span class="sxs-lookup"><span data-stu-id="8063d-185">Accessing a Redis cache from a client</span></span>

<span data-ttu-id="8063d-186">Per connettersi a un'istanza di Cache Redis di Azure, è necessario specificare varie informazioni.</span><span class="sxs-lookup"><span data-stu-id="8063d-186">To connect to an Azure Redis Cache instance, you'll need several pieces of information.</span></span> <span data-ttu-id="8063d-187">I client devono conoscere il nome host, la porta e la chiave di accesso per la cache.</span><span class="sxs-lookup"><span data-stu-id="8063d-187">Clients need the host name, port, and an access key for the cache.</span></span> <span data-ttu-id="8063d-188">È possibile recuperare queste informazioni nel portale di Azure tramite la pagina **Impostazioni > Chiavi di accesso**.</span><span class="sxs-lookup"><span data-stu-id="8063d-188">You can retrieve this information in the Azure portal through the **Settings > Access Keys** page.</span></span> 

- <span data-ttu-id="8063d-189">Il nome host è l'indirizzo Internet pubblico della cache, creato usando il nome della cache.</span><span class="sxs-lookup"><span data-stu-id="8063d-189">The host name is the public Internet address of your cache, which was created using the name of the cache.</span></span> <span data-ttu-id="8063d-190">Ad esempio, `sportsresults.redis.cache.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="8063d-190">For example `sportsresults.redis.cache.windows.net`.</span></span>

- <span data-ttu-id="8063d-191">La chiave di accesso funge da password per la cache.</span><span class="sxs-lookup"><span data-stu-id="8063d-191">The access key acts as a password for your cache.</span></span> <span data-ttu-id="8063d-192">Vengono create due chiavi, primaria e secondaria.</span><span class="sxs-lookup"><span data-stu-id="8063d-192">There are two keys created: primary and secondary.</span></span> <span data-ttu-id="8063d-193">È possibile usare una delle due chiavi. Ne vengono fornite due nel caso diventi necessario modificare la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="8063d-193">You can use either key, two are provided in case you need to change the primary key.</span></span> <span data-ttu-id="8063d-194">È possibile passare tutti i client alla chiave secondaria e rigenerare la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="8063d-194">You can switch all of your clients to the secondary key, and regenerate the primary key.</span></span> <span data-ttu-id="8063d-195">Questa operazione blocca eventuali applicazioni che usano la chiave primaria originale.</span><span class="sxs-lookup"><span data-stu-id="8063d-195">This would block any applications using the original primary key.</span></span> <span data-ttu-id="8063d-196">Microsoft consiglia di rigenerare periodicamente le chiavi, analogamente alle password perdonali.</span><span class="sxs-lookup"><span data-stu-id="8063d-196">Microsoft recommends periodically regenerating the keys - much like you would your personal passwords.</span></span>

> [!WARNING]
> <span data-ttu-id="8063d-197">Le chiavi di accesso devono essere considerate informazioni riservate, proprio come una password.</span><span class="sxs-lookup"><span data-stu-id="8063d-197">Your access keys should be considered confidential information, treat them like you would a password.</span></span> <span data-ttu-id="8063d-198">Chiunque abbia una chiave di accesso può eseguire qualsiasi operazione sulla cache.</span><span class="sxs-lookup"><span data-stu-id="8063d-198">Anyone who has an access key can perform any operation on your cache!</span></span>
