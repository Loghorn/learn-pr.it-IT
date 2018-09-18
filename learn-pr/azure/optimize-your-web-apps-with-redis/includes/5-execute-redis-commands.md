Come indicato in precedenza, Redis è un database NoSQL in memoria che può essere replicato in più server. Viene spesso usato come cache, ma può essere usato anche come database formale o anche come broker di messaggi. 

Può archiviare molti tipi di dati e molte strutture e supporta diversi comandi che possono essere eseguiti per recuperare i dati memorizzati nella cache o informazioni di query sulla cache stessa. I dati usati vengono sempre archiviati sotto forma di coppie chiave/valore.

## <a name="executing-commands-on-the-redis-cache"></a>Esecuzione di comandi nella cache Redis

Un'applicazione client userà in genere una _libreria client_ per formare richieste ed eseguire comandi in una cache Redis. È possibile ottenere un elenco di librerie client direttamente dalla [pagina dei client Redis](https://redis.io/clients). Un client Redis ad alte prestazioni diffuso per il linguaggio .NET è **StackExchange.Redis**. Il pacchetto è disponibile tramite NuGet e può essere aggiunto al codice .NET mediante la riga di comando o l'ambiente di sviluppo integrato.

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a>Connessione alla cache Redis con StackExchange.Redis

È importante ricordare che per la connessione a un server Redis vengono usati l'indirizzo dell'host, il numero della porta e una chiave di accesso. Azure offre anche una _stringa di connessione_ per alcuni client Redis, in modo da riunire questi dati in una singola stringa.

### <a name="what-is-a-connection-string"></a>Informazioni sulla stringa di connessione

Una stringa di connessione è una singola riga di testo che include tutte le informazioni necessarie per la connessione e l'autenticazione in una cache Redis in Azure. Avrà un aspetto simile al seguente, con i campi **cache-name** e **password-here** compilati con valori effettivi:

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> La stringa di connessione deve essere protetta nell'applicazione. Se l'applicazione è ospitata in Azure, prendere in considerazione l'uso di Azure Key Vault per l'archiviazione del valore.

È possibile passare questa stringa a **StackExchange.Redis** per creare una connessione al server. 

Si noti che ala fine sono presenti due parametri aggiuntivi: 

- **ssl**: assicura che le comunicazioni siano crittografate.
- **abortConnection**: consente la creazione di una connessione anche se il server non è disponibile in quel momento.

Sono disponibili altri [parametri facoltativi](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options) da aggiungere alla fine della stringa per configurare la libreria client.

### <a name="creating-a-connection"></a>Creazione di una connessione

L'oggetto principale della connessione in **StackExchange.Redis** è la classe `StackExchange.Redis.ConnectionMultiplexer`. Questo oggetto astrae il processo di connessione a un server Redis o a un gruppo di server. È ottimizzato per gestire le connessioni in modo efficiente e deve essere disponibile quando è necessario accedere alla cache.

È possibile creare un'istanza di `ConnectionMultiplexer` usando il metodo statico `ConnectionMultiplexer.Connect` o `ConnectionMultiplexer.ConnectAsync`, passando una stringa di connessione o un oggetto `ConfigurationOptions`. 

Ecco un semplice esempio:

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

Quando è disponibile un'istanza di `ConnectionMultiplexer`, è necessario eseguire tre operazioni principali:

1. Accedere a un database Redis. Questa operazione verrà illustrata in questa sezione.
2. Usare le funzionalità server di pubblicazione/sottoscrizione di Redis. Questo argomento non rientra nell'ambito del modulo.
3. Accedere a un singolo server per finalità di manutenzione o monitoraggio.

### <a name="accessing-a-redis-database"></a>Accesso a un database Redis

Il database Redis è rappresentato dal tipo `IDatabase`. È possibile recuperarne uno mediante il metodo `GetDatabase()`:

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> L'oggetto restituito da `GetDatabase` è un oggetto leggero e non è necessario archiviarlo. È necessario mantenere attivo solo `ConnectionMultiplexer`.

Quando l'oggetto `IDatabase` è disponibile, è possibile eseguire metodi per interagire con la cache. Tutti i metodi hanno versioni sincrone e asincrone che restituiscono oggetti `Task` per renderli compatibili con le parole chiave `async` e `await`.

Ecco un esempio di archiviazione di una coppia chiave/valore nella cache:

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

Il metodo `StringSet` restituisce un valore `bool` per indicare che il valore è stato impostato (`true`) o non è stato impostato (`false`). È quindi possibile recuperare il valore con il metodo `StringGet`:

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a>Recupero e impostazione di valori binari

È importante ricordare che le chiavi e i valori di Redis sono _indipendenti dall'aspetto binario_. Gli stessi metodi possono essere usati per archiviare dati binari. Sono disponibili operatori di conversione impliciti da usare con i tipi `byte[]` in modo da potere usare i dati normalmente:

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
> **StackExchange.Redis** rappresenta le chiavi mediante il tipo `RedisKey`. Questa classe include conversioni implicite verso e da `string` e `byte[]`, consentendo l'uso di chiavi di testo e binarie senza complicazioni. I valori sono rappresentati dal tipo `RedisValue`. Analogamente a `RedisKey`, sono disponibili conversioni implicite che consentono di passare `string` o `byte[]`.

#### <a name="other-common-operations"></a>Altre operazioni comuni

L'interfaccia `IDatabase` include alcuni metodi che consentono di usare la cache Redis. Sono disponibili metodi che consentono di usare hash, elenchi, set e set ordinati.

Ecco alcuni dei più comuni che possono essere usati con le chiavi singole. Per visualizzare l'elenco completo, è possibile [leggere il codice sorgente](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) per l'interfaccia.

| Metodo | Descrizione |
|--------|-------------|
| `CreateBatch` | Consente di creare un _gruppo di operazioni_ che verrà inviato al server come unità singola, ma non verrà necessariamente elaborato come unità. |
| `CreateTransaction` | Consente di creare un gruppo di operazioni che verrà inviato al server come unità singola _e_ verrà elaborato sul server come unità singola. |
| `KeyDelete` | Eliminare la coppia chiave/valore. |
| `KeyExists` | Consente di restituire un valore che indica se la chiave specificata esiste nella cache. |
| `KeyExpire` | Consente di impostare una scadenza TTL per una chiave. |
| `KeyRename` | Consente di rinominare una chiave. |
| `KeyTimeToLive` | Consente di restituire il valore TTL per una chiave. |
| `KeyType` | Consente di restituire la rappresentazione di stringa del tipo del valore archiviato nella chiave. Possono essere restituiti tipi diversi, ovvero stringa, elenco,set, zset e hash. |
       
### <a name="executing-other-commands"></a>Esecuzione di altri comandi

L'oggetto `IDatabase` ha un metodo `Execute` e `ExecuteAsync` che può essere usato per passare comandi testuali nel server Redis. Ad esempio:

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

I metodi `Execute` e `ExecuteAsync` restituiscono un oggetto `RedisResult` che è un contenitore di dati che include due proprietà:

- `Type` che restituisce un valore `string`, indicando il tipo del risultato, ovvero "STRING", "INTEGER" e così via.
- `IsNull` è un valore true/false che consente di rilevare quando il risultato è `null`.

È quindi possibile usare `ToString()` in `RedisResult` per ottenere il valore restituito effettivo.

È possibile usare `Execute` per eseguire qualsiasi comando supportato, ad esempio è possibile fare in modo che tutti i client siano connessi alla cache ("CLIENT LIST"):

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

Verranno restituiti come output tutti i client connessi:

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a>Archiviazione di valori più complessi
Redis è basato su stringhe indipendenti dall'aspetto binario, ma è possibile memorizzare nella cache oggetti grafici mediante la serializzazione di tali oggetti in un formato testuale, in genere XML o JSON. Per le statistiche è ad esempio possibile che sia presente un oggetto `GameStats`, con un aspetto analogo al seguente:

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

È possibile usare la libreria **Newtonsoft.Json** per trasformare un'istanza dell'oggetto in una stringa:

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

È possibile recuperarla e trasformarla di nuovo in un oggetto mediante il processo inverso:

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a>Pulizia della connessione
Dopo avere terminato l'uso della connessione Redis, è possibile **eliminare** l'istanza di `ConnectionMultiplexer`. L'eliminazione consentirà di chiudere tutte le connessioni e di arrestare le comunicazioni con il server.

```csharp
redisConnection.Dispose();
redisConnection = null;
```

È ora possibile creare un'applicazione ed eseguire alcune operazioni semplici con la cache Redis.