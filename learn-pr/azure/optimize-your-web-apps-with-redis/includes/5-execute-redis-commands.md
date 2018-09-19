<span data-ttu-id="4c1fb-101">Come indicato in precedenza, Redis è un database NoSQL in memoria che può essere replicato in più server.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-101">As mentioned earlier, Redis is an in-memory NoSQL database which can be replicated across multiple servers.</span></span> <span data-ttu-id="4c1fb-102">Viene spesso usato come cache, ma può essere usato anche come database formale o anche come broker di messaggi.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-102">It is often used as a cache, but can be used as a formal database or even message-broker.</span></span> 

<span data-ttu-id="4c1fb-103">Può archiviare molti tipi di dati e molte strutture e supporta diversi comandi che possono essere eseguiti per recuperare i dati memorizzati nella cache o informazioni di query sulla cache stessa.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-103">It can store a variety of data types and structures and supports a variety of commands you can issue to retrieve cached data or query information about the cache itself.</span></span> <span data-ttu-id="4c1fb-104">I dati usati vengono sempre archiviati sotto forma di coppie chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-104">The data you work with is always stored as key/value pairs.</span></span>

## <a name="executing-commands-on-the-redis-cache"></a><span data-ttu-id="4c1fb-105">Esecuzione di comandi nella cache Redis</span><span class="sxs-lookup"><span data-stu-id="4c1fb-105">Executing commands on the Redis cache</span></span>

<span data-ttu-id="4c1fb-106">Un'applicazione client userà in genere una _libreria client_ per formare richieste ed eseguire comandi in una cache Redis.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-106">Typically, a client application will use a _client library_ to form requests and execute commands on a Redis cache.</span></span> <span data-ttu-id="4c1fb-107">È possibile ottenere un elenco di librerie client direttamente dalla [pagina dei client Redis](https://redis.io/clients).</span><span class="sxs-lookup"><span data-stu-id="4c1fb-107">You can get a list of client libraries directly from the [Redis clients page](https://redis.io/clients).</span></span> <span data-ttu-id="4c1fb-108">Un client Redis ad alte prestazioni diffuso per il linguaggio .NET è **StackExchange.Redis**.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-108">A popular high-performance Redis client for the .NET language is **StackExchange.Redis**.</span></span> <span data-ttu-id="4c1fb-109">Il pacchetto è disponibile tramite NuGet e può essere aggiunto al codice .NET mediante la riga di comando o l'ambiente di sviluppo integrato.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-109">The package is available through NuGet and can be added to your .NET code using the command line or IDE.</span></span>

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a><span data-ttu-id="4c1fb-110">Connessione alla cache Redis con StackExchange.Redis</span><span class="sxs-lookup"><span data-stu-id="4c1fb-110">Connecting to your Redis cache with StackExchange.Redis</span></span>

<span data-ttu-id="4c1fb-111">È importante ricordare che per la connessione a un server Redis vengono usati l'indirizzo dell'host, il numero della porta e una chiave di accesso.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-111">Recall that we use the host address, port number, and an access key to connect to a Redis server.</span></span> <span data-ttu-id="4c1fb-112">Azure offre anche una _stringa di connessione_ per alcuni client Redis, in modo da riunire questi dati in una singola stringa.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-112">Azure also offers a _connection string_ for some Redis clients which bundles this data together into a single string.</span></span>

### <a name="what-is-a-connection-string"></a><span data-ttu-id="4c1fb-113">Informazioni sulla stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="4c1fb-113">What is a connection string?</span></span>

<span data-ttu-id="4c1fb-114">Una stringa di connessione è una singola riga di testo che include tutte le informazioni necessarie per la connessione e l'autenticazione in una cache Redis in Azure.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-114">A connection string is a single line of text that includes all the required pieces of information to connect and authenticate to a Redis cache in Azure.</span></span> <span data-ttu-id="4c1fb-115">Avrà un aspetto simile al seguente, con i campi **cache-name** e **password-here** compilati con valori effettivi:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-115">It will look something like the following (with the **cache-name** and **password-here** fields filled in with real values):</span></span>

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> <span data-ttu-id="4c1fb-116">La stringa di connessione deve essere protetta nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-116">The connection string should be protected in your application.</span></span> <span data-ttu-id="4c1fb-117">Se l'applicazione è ospitata in Azure, prendere in considerazione l'uso di Azure Key Vault per l'archiviazione del valore.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-117">If the application is hosted on Azure, consider using an Azure Key Vault to store the value.</span></span>

<span data-ttu-id="4c1fb-118">È possibile passare questa stringa a **StackExchange.Redis** per creare una connessione al server.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-118">You can pass this string to **StackExchange.Redis** to create a connection the server.</span></span> 

<span data-ttu-id="4c1fb-119">Si noti che ala fine sono presenti due parametri aggiuntivi:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-119">Notice that there are two additional parameters at the end:</span></span> 

- <span data-ttu-id="4c1fb-120">**ssl**: assicura che le comunicazioni siano crittografate.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-120">**ssl** - ensures that communication is encrypted.</span></span>
- <span data-ttu-id="4c1fb-121">**abortConnection**: consente la creazione di una connessione anche se il server non è disponibile in quel momento.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-121">**abortConnection** - allows a connection to be created even if the server is unavailable at that moment.</span></span>

<span data-ttu-id="4c1fb-122">Sono disponibili altri [parametri facoltativi](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options) da aggiungere alla fine della stringa per configurare la libreria client.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-122">There are several other [optional parameters](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options) you can append to the string to configure the client library.</span></span>

### <a name="creating-a-connection"></a><span data-ttu-id="4c1fb-123">Creazione di una connessione</span><span class="sxs-lookup"><span data-stu-id="4c1fb-123">Creating a connection</span></span>

<span data-ttu-id="4c1fb-124">L'oggetto principale della connessione in **StackExchange.Redis** è la classe `StackExchange.Redis.ConnectionMultiplexer`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-124">The main connection object in **StackExchange.Redis** is the `StackExchange.Redis.ConnectionMultiplexer` class.</span></span> <span data-ttu-id="4c1fb-125">Questo oggetto astrae il processo di connessione a un server Redis o a un gruppo di server.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-125">This object abstracts the process of connecting to a Redis server (or group of servers).</span></span> <span data-ttu-id="4c1fb-126">È ottimizzato per gestire le connessioni in modo efficiente e deve essere disponibile quando è necessario accedere alla cache.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-126">It's optimized to manage connections efficiently and intended to be kept around while you need access to the cache.</span></span>

<span data-ttu-id="4c1fb-127">È possibile creare un'istanza di `ConnectionMultiplexer` usando il metodo statico `ConnectionMultiplexer.Connect` o `ConnectionMultiplexer.ConnectAsync`, passando una stringa di connessione o un oggetto `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-127">You create a `ConnectionMultiplexer` instance using the static `ConnectionMultiplexer.Connect` or `ConnectionMultiplexer.ConnectAsync` method, passing in either a connection string or a `ConfigurationOptions` object.</span></span> 

<span data-ttu-id="4c1fb-128">Ecco un semplice esempio:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-128">Here's a simple example:</span></span>

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

<span data-ttu-id="4c1fb-129">Quando è disponibile un'istanza di `ConnectionMultiplexer`, è necessario eseguire tre operazioni principali:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-129">Once you have a `ConnectionMultiplexer`, there are 3 primary things you might want to do:</span></span>

1. <span data-ttu-id="4c1fb-130">Accedere a un database Redis.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-130">Access a Redis Database.</span></span> <span data-ttu-id="4c1fb-131">Questa operazione verrà illustrata in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-131">This is what we will focus on here.</span></span>
2. <span data-ttu-id="4c1fb-132">Usare le funzionalità server di pubblicazione/sottoscrizione di Redis.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-132">Make use of the publisher/subscript features of Redis.</span></span> <span data-ttu-id="4c1fb-133">Questo argomento non rientra nell'ambito del modulo.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-133">This is outside the scope of this module.</span></span>
3. <span data-ttu-id="4c1fb-134">Accedere a un singolo server per finalità di manutenzione o monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-134">Access an individual server for maintenance or monitoring purposes.</span></span>

### <a name="accessing-a-redis-database"></a><span data-ttu-id="4c1fb-135">Accesso a un database Redis</span><span class="sxs-lookup"><span data-stu-id="4c1fb-135">Accessing a Redis database</span></span>

<span data-ttu-id="4c1fb-136">Il database Redis è rappresentato dal tipo `IDatabase`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-136">The Redis database is represented by the `IDatabase` type.</span></span> <span data-ttu-id="4c1fb-137">È possibile recuperarne uno mediante il metodo `GetDatabase()`:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-137">You can retrieve one using the `GetDatabase()` method:</span></span>

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> <span data-ttu-id="4c1fb-138">L'oggetto restituito da `GetDatabase` è un oggetto leggero e non è necessario archiviarlo.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-138">The object returned from `GetDatabase` is a lightweight object, and does not need to be stored.</span></span> <span data-ttu-id="4c1fb-139">È necessario mantenere attivo solo `ConnectionMultiplexer`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-139">Only the `ConnectionMultiplexer` needs to be kept alive.</span></span>

<span data-ttu-id="4c1fb-140">Quando l'oggetto `IDatabase` è disponibile, è possibile eseguire metodi per interagire con la cache.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-140">Once you have a `IDatabase` object, you can execute methods to interact with the cache.</span></span> <span data-ttu-id="4c1fb-141">Tutti i metodi hanno versioni sincrone e asincrone che restituiscono oggetti `Task` per renderli compatibili con le parole chiave `async` e `await`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-141">All methods have synchronous and asynchronous versions which return `Task` objects to make them compatible with the `async` and `await` keywords.</span></span>

<span data-ttu-id="4c1fb-142">Ecco un esempio di archiviazione di una coppia chiave/valore nella cache:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-142">Here is an example of storing a key/value in the cache:</span></span>

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

<span data-ttu-id="4c1fb-143">Il metodo `StringSet` restituisce un valore `bool` per indicare che il valore è stato impostato (`true`) o non è stato impostato (`false`).</span><span class="sxs-lookup"><span data-stu-id="4c1fb-143">The `StringSet` method returns a `bool` indicating whether the value was set (`true`) or not (`false`).</span></span> <span data-ttu-id="4c1fb-144">È quindi possibile recuperare il valore con il metodo `StringGet`:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-144">We can then retrieve the value with the `StringGet` method:</span></span>

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a><span data-ttu-id="4c1fb-145">Recupero e impostazione di valori binari</span><span class="sxs-lookup"><span data-stu-id="4c1fb-145">Getting and Setting binary values</span></span>

<span data-ttu-id="4c1fb-146">È importante ricordare che le chiavi e i valori di Redis sono _indipendenti dall'aspetto binario_.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-146">Recall that Redis keys and values are _binary safe_.</span></span> <span data-ttu-id="4c1fb-147">Gli stessi metodi possono essere usati per archiviare dati binari.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-147">These same methods can be used to store binary data.</span></span> <span data-ttu-id="4c1fb-148">Sono disponibili operatori di conversione impliciti da usare con i tipi `byte[]` in modo da potere usare i dati normalmente:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-148">There are implicit conversion operators to work with `byte[]` types so you can work with the data naturally:</span></span>

```csharp
byte[] key = ...;
byte[] value = ...;

db.StringSet(key, value);
```

```csharp
byte[] key = ...;
byte[] value = db.StringGet(key);
```

> [!TIP]
> <span data-ttu-id="4c1fb-149">**StackExchange.Redis** rappresenta le chiavi mediante il tipo `RedisKey`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-149">**StackExchange.Redis** represents keys using the `RedisKey` type.</span></span> <span data-ttu-id="4c1fb-150">Questa classe include conversioni implicite verso e da `string` e `byte[]`, consentendo l'uso di chiavi di testo e binarie senza complicazioni.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-150">This class has implicit conversions to and from both `string` and `byte[]`, allowing both text and binary keys to be used without any complication.</span></span> <span data-ttu-id="4c1fb-151">I valori sono rappresentati dal tipo `RedisValue`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-151">Values are represented by the `RedisValue` type.</span></span> <span data-ttu-id="4c1fb-152">Analogamente a `RedisKey`, sono disponibili conversioni implicite che consentono di passare `string` o `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-152">As with `RedisKey`, there are implicit conversions in place to allow you to pass `string` or `byte[]`.</span></span>

#### <a name="other-common-operations"></a><span data-ttu-id="4c1fb-153">Altre operazioni comuni</span><span class="sxs-lookup"><span data-stu-id="4c1fb-153">Other common operations</span></span>

<span data-ttu-id="4c1fb-154">L'interfaccia `IDatabase` include alcuni metodi che consentono di usare la cache Redis.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-154">The `IDatabase` interface includes several other methods to work with the Redis cache.</span></span> <span data-ttu-id="4c1fb-155">Sono disponibili metodi che consentono di usare hash, elenchi, set e set ordinati.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-155">There are methods to work with hashes, lists, sets, and ordered sets.</span></span>

<span data-ttu-id="4c1fb-156">Ecco alcuni dei più comuni che possono essere usati con le chiavi singole. Per visualizzare l'elenco completo, è possibile [leggere il codice sorgente](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) per l'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-156">Here are some of the more common ones that work with single keys, you can [read the source code](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) for the interface to see the full list.</span></span>

| <span data-ttu-id="4c1fb-157">Metodo</span><span class="sxs-lookup"><span data-stu-id="4c1fb-157">Method</span></span> | <span data-ttu-id="4c1fb-158">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4c1fb-158">Description</span></span> |
|--------|-------------|
| `CreateBatch` | <span data-ttu-id="4c1fb-159">Consente di creare un _gruppo di operazioni_ che verrà inviato al server come unità singola, ma non verrà necessariamente elaborato come unità.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-159">Creates a _group of operations_ that will be sent to the server as a single unit, but not necessarily processed as a unit.</span></span> |
| `CreateTransaction` | <span data-ttu-id="4c1fb-160">Consente di creare un gruppo di operazioni che verrà inviato al server come unità singola _e_ verrà elaborato sul server come unità singola.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-160">Creates a group of operations that will be sent to the server as a single unit _and_ processed on the server as a single unit.</span></span> |
| `KeyDelete` | <span data-ttu-id="4c1fb-161">Eliminare la coppia chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-161">Delete the key/value.</span></span> |
| `KeyExists` | <span data-ttu-id="4c1fb-162">Consente di restituire un valore che indica se la chiave specificata esiste nella cache.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-162">Returns whether the given key exists in cache.</span></span> |
| `KeyExpire` | <span data-ttu-id="4c1fb-163">Consente di impostare una scadenza TTL per una chiave.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-163">Sets a time-to-live (TTL) expiration on a key.</span></span> |
| `KeyRename` | <span data-ttu-id="4c1fb-164">Consente di rinominare una chiave.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-164">Renames a key.</span></span> |
| `KeyTimeToLive` | <span data-ttu-id="4c1fb-165">Consente di restituire il valore TTL per una chiave.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-165">Returns the TTL for a key.</span></span> |
| `KeyType` | <span data-ttu-id="4c1fb-166">Consente di restituire la rappresentazione di stringa del tipo del valore archiviato nella chiave.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-166">Returns the string representation of the type of the value stored at key.</span></span> <span data-ttu-id="4c1fb-167">Possono essere restituiti tipi diversi, ovvero stringa, elenco,set, zset e hash.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-167">The different types that can be returned are: string, list, set, zset and hash.</span></span> |
       
### <a name="executing-other-commands"></a><span data-ttu-id="4c1fb-168">Esecuzione di altri comandi</span><span class="sxs-lookup"><span data-stu-id="4c1fb-168">Executing other commands</span></span>

<span data-ttu-id="4c1fb-169">L'oggetto `IDatabase` ha un metodo `Execute` e `ExecuteAsync` che può essere usato per passare comandi testuali nel server Redis.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-169">The `IDatabase` object has an `Execute` and `ExecuteAsync` method which can be used to pass textual commands to the Redis server.</span></span> <span data-ttu-id="4c1fb-170">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-170">For example:</span></span>

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

<span data-ttu-id="4c1fb-171">I metodi `Execute` e `ExecuteAsync` restituiscono un oggetto `RedisResult` che è un contenitore di dati che include due proprietà:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-171">The `Execute` and `ExecuteAsync` methods return a `RedisResult` object which is a data holder that includes two properties:</span></span>

- <span data-ttu-id="4c1fb-172">`Type` che restituisce un valore `string`, indicando il tipo del risultato, ovvero "STRING", "INTEGER" e così via.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-172">`Type` which returns a `string` indicating the type of the result - "STRING", "INTEGER", etc.</span></span>
- <span data-ttu-id="4c1fb-173">`IsNull` è un valore true/false che consente di rilevare quando il risultato è `null`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-173">`IsNull` a true/false value to detect when the result is `null`.</span></span>

<span data-ttu-id="4c1fb-174">È quindi possibile usare `ToString()` in `RedisResult` per ottenere il valore restituito effettivo.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-174">You can then use `ToString()` on the `RedisResult` to get the actual return value.</span></span>

<span data-ttu-id="4c1fb-175">È possibile usare `Execute` per eseguire qualsiasi comando supportato, ad esempio è possibile fare in modo che tutti i client siano connessi alla cache ("CLIENT LIST"):</span><span class="sxs-lookup"><span data-stu-id="4c1fb-175">You can use `Execute` to perform any supported commands - for example, we can get all the clients connected to the cache ("CLIENT LIST"):</span></span>

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

<span data-ttu-id="4c1fb-176">Verranno restituiti come output tutti i client connessi:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-176">This would output all the connected clients:</span></span>

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a><span data-ttu-id="4c1fb-177">Archiviazione di valori più complessi</span><span class="sxs-lookup"><span data-stu-id="4c1fb-177">Storing more complex values</span></span>
<span data-ttu-id="4c1fb-178">Redis è basato su stringhe indipendenti dall'aspetto binario, ma è possibile memorizzare nella cache oggetti grafici mediante la serializzazione di tali oggetti in un formato testuale, in genere XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-178">Redis is oriented around binary safe strings, but you can cache off object graphs by serializing them to a textual format - typically XML or JSON.</span></span> <span data-ttu-id="4c1fb-179">Per le statistiche è ad esempio possibile che sia presente un oggetto `GameStats`, con un aspetto analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-179">For example, perhaps for our statistics, we have a `GameStats` object which looks like:</span></span>

```csharp
public class GameStat
{
    public string Id { get; set; }
    public string Sport { get; set; }
    public DateTimeOffset DatePlayed { get; set; }
    public string Game { get; set; }
    public IReadOnlyList<string> Teams { get; set; }
    public IReadOnlyList<(string team, int score)> Results { get; set; }

    public GameStat(string sport, DateTimeOffset datePlayed, string game, string[] teams, IEnumerable<(string team, int score)> results)
    {
        Id = Guid.NewGuid().ToString();
        Sport = sport;
        DatePlayed = datePlayed;
        Game = game;
        Teams = teams.ToList();
        Results = results.ToList();
    }

    public override string ToString()
    {
        return $"{Sport} {Game} played on {DatePlayed.Date.ToShortDateString()} - " +
               $"{String.Join(',', Teams)}\r\n\t" + 
               $"{String.Join('\t', Results.Select(r => $"{r.team } - {r.score}\r\n"))}";
    }
}
```

<span data-ttu-id="4c1fb-180">È possibile usare la libreria **Newtonsoft.Json** per trasformare un'istanza dell'oggetto in una stringa:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-180">We could use the **Newtonsoft.Json** library to turn an instance of this object into a string:</span></span>

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

<span data-ttu-id="4c1fb-181">È possibile recuperarla e trasformarla di nuovo in un oggetto mediante il processo inverso:</span><span class="sxs-lookup"><span data-stu-id="4c1fb-181">We could retrieve it and turn it back into an object using the reverse process:</span></span>

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a><span data-ttu-id="4c1fb-182">Pulizia della connessione</span><span class="sxs-lookup"><span data-stu-id="4c1fb-182">Cleaning up the connection</span></span>
<span data-ttu-id="4c1fb-183">Dopo avere terminato l'uso della connessione Redis, è possibile **eliminare** l'istanza di `ConnectionMultiplexer`.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-183">Once you are done with the Redis connection, you can **Dispose** the `ConnectionMultiplexer`.</span></span> <span data-ttu-id="4c1fb-184">L'eliminazione consentirà di chiudere tutte le connessioni e di arrestare le comunicazioni con il server.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-184">This will close all connections and shutdown the communication to the server.</span></span>

```csharp
redisConnection.Dispose();
redisConnection = null;
```

<span data-ttu-id="4c1fb-185">È ora possibile creare un'applicazione ed eseguire alcune operazioni semplici con la cache Redis.</span><span class="sxs-lookup"><span data-stu-id="4c1fb-185">Let's create an application and do some simple work with our Redis cache.</span></span>