Come accennato in precedenza, Redis è un database NoSQL in memoria che può essere replicato tra più server. Viene spesso usata come una cache, ma può essere utilizzato come un database formale o persino di broker di messaggi. 

Si può archiviare un'ampia gamma di strutture e tipi di dati che supporta un'ampia gamma di comandi che è possibile eseguire per recuperare informazioni memorizzate nella cache i dati o query alla cache stessa. I dati che si lavora sono sempre archiviati come coppie chiave/valore.

## <a name="executing-commands-on-the-redis-cache"></a>L'esecuzione di comandi sulla cache Redis

In genere, un'applicazione client useranno un _libreria client_ per formare richieste ed eseguire comandi in una cache Redis. È possibile ottenere un elenco delle librerie client direttamente dal [pagina client Redis](https://redis.io/clients). Un client Redis ad alte prestazioni diffuso per il linguaggio .NET è **StackExchange.Redis**. Il pacchetto è disponibile tramite NuGet e può essere aggiunti al codice .NET usare la riga di comando o l'IDE.

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a>La connessione alla cache Redis con stackexchange. Redis

È importante ricordare che utilizziamo l'indirizzo dell'host, il numero di porta e una chiave di accesso per connettersi a un server Redis. Azure offre inoltre una _stringa di connessione_ per alcuni client Redis che vengono raggruppati i dati in un'unica stringa.

### <a name="what-is-a-connection-string"></a>Che cos'è una stringa di connessione?

Una stringa di connessione è una singola riga di testo che include tutte le parti necessarie di informazioni per connettersi ed eseguire l'autenticazione a una cache Redis di Azure. Avrà un aspetto simile al seguente (con il **cache-name** e **password-here** compilato i campi con valori reali):

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> La stringa di connessione deve essere protette nell'applicazione. Se l'applicazione è ospitata in Azure, è consigliabile usando un Azure Key Vault per archiviare il valore.

È possibile passare questa stringa **stackexchange. Redis** per creare una connessione server. 

Si noti che alla fine sono presenti due parametri aggiuntivi: 

- **SSL** -assicura che la comunicazione viene crittografata.
- **abortConnection** -consente una connessione deve essere creato anche se il server è disponibile in quel momento.

Esistono diversi altri [parametri facoltativi](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options) è possibile aggiungere alla stringa di configurare la libreria client.

### <a name="creating-a-connection"></a>Creazione di una connessione

L'oggetto di connessione principale nel **stackexchange. Redis** è il `StackExchange.Redis.ConnectionMultiplexer` classe. Questo oggetto consente di astrarre il processo di connessione a un server Redis (o gruppo di server). Dispone di ottimizzato per gestire le connessioni in modo efficiente e deve essere conservato anche se è necessario l'accesso alla cache.

Si crea una `ConnectionMultiplexer` istanza usando il metodo statico `ConnectionMultiplexer.Connect` oppure `ConnectionMultiplexer.ConnectAsync` metodo, passando una stringa di connessione o un `ConfigurationOptions` oggetto. 

Ecco un semplice esempio:

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

Dopo aver creato un `ConnectionMultiplexer`, esistono 3 operazioni primari si potrebbe voler eseguire:

1. Accedere a un Database di Redis. Questo è ciò che ci concentreremo su qui.
2. Rendere utilizzo delle funzionalità server di pubblicazione/pedice di Redis. Questo rientra nell'ambito di questo modulo.
3. Accedere a un singolo server per la manutenzione o a fini di monitoraggio.

### <a name="accessing-a-redis-database"></a>Accesso a un database di Redis

Il database Redis è rappresentato dal `IDatabase` tipo. È possibile recuperare uno che utilizza il `GetDatabase()` metodo:

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> L'oggetto restituito da `GetDatabase` è un oggetto leggero e non devono essere archiviati. Solo il `ConnectionMultiplexer` deve essere mantenuto attivo.

Dopo aver creato un `IDatabase` dell'oggetto, è possibile eseguire i metodi per interagire con la cache. Tutti i metodi hanno versioni sincrone e asincrone che restituiscono `Task` gli oggetti per renderli compatibili con il `async` e `await` parole chiave.

Di seguito è riportato un esempio di archiviare un chiave/valore nella cache:

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

Il `StringSet` metodo restituisce un `bool` che indica se è stato impostato il valore (`true`) o No (`false`). È quindi possibile recuperare il valore con il `StringGet` metodo:

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a>Ottenere e impostare i valori binari

Tenere presente che le chiavi di Redis e i valori sono _binario sicuro_. Questi stessi metodi sono utilizzabile per archiviare i dati binari. Sono disponibili gli operatori di conversione implicita per lavorare con `byte[]` tipi per poter lavorare con i dati normalmente:

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
> **Stackexchange. Redis** rappresenta le chiavi usando il `RedisKey` tipo. Questa classe ha le conversioni implicite da e verso entrambe `string` e `byte[]`, consentendo di chiavi di testo e binaria essere usata senza le complicazioni. I valori sono rappresentati dal `RedisValue` tipo. Come per gli `RedisKey`, sono disponibili le conversioni implicite per consentire il passaggio `string` o `byte[]`.

#### <a name="other-common-operations"></a>Altre operazioni comuni

Il `IDatabase` interfaccia include diversi altri metodi per lavorare con la cache Redis. Sono disponibili metodi per lavorare con gli hash, elenchi, set e set ordinati.

Ecco alcune delle più comuni che funzionano con singole chiavi, è possibile [leggere il codice sorgente](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) per l'interfaccia visualizzare l'elenco completo.

| Metodo | Descrizione |
|--------|-------------|
| `CreateBatch` | Crea una _gruppo di operazioni_ che vengono inviati al server come singola unità, ma non necessariamente elaborate come unità. |
| `CreateTransaction` | Crea un gruppo di operazioni che verranno inviati al server come singola unità _e_ elaborato nel server come singola unità. |
| `KeyDelete` | Eliminare il chiave/valore. |
| `KeyExists` | Restituisce se la chiave specificata esiste nella cache. |
| `KeyExpire` | Consente di impostare una scadenza di (durata TTL) time-to-live su una chiave. |
| `KeyRename` | Rinomina una chiave. |
| `KeyTimeToLive` | Restituisce il valore TTL per una chiave. |
| `KeyType` | Restituisce la rappresentazione di stringa del tipo del valore archiviato nella chiave. I diversi tipi che possono essere restituiti sono: stringa, elencare, impostare, zset e hash. |
       
### <a name="executing-other-commands"></a>L'esecuzione di altri comandi

Il `IDatabase` oggetto ha un `Execute` e `ExecuteAsync` metodo che può essere usato per passare comandi testuali al server Redis. Ad esempio:

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

Il `Execute` e `ExecuteAsync` metodi restituiscono un `RedisResult` oggetto che è un contenitore di dati che include due proprietà:

- `Type` che restituisce un `string` che indica il tipo del risultato: "STRING", "INTEGER" e così via.
- `IsNull` un valore true/false per rilevare quando il risultato è `null`.

È quindi possibile usare `ToString()` nella `RedisResult` per ottenere l'effettivo valore restituito.

È possibile usare `Execute` eseguire tutti i comandi supportati, ad esempio, è possibile ottenere tutti i client connessi alla cache ("LIST CLIENT"):

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

Questo genera tutti i client connessi:

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a>L'archiviazione dei valori più complessi
Redis è orientato stringhe binarie sicure, ma è possibile memorizzare nella cache disattivata oggetti grafici serializzando loro in un formato testuale: in genere XML o JSON. Ad esempio, ad esempio per le statistiche, abbiamo un `GameStats` oggetto che è simile a:

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

È possibile usare la **newtonsoft. JSON** libreria per attivare un'istanza di questo oggetto in una stringa:

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

È stato possibile recuperarlo e convertito nuovamente in un oggetto usando il processo inverso:

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a>Pulizia della connessione
Dopo aver completato la connessione Redis, è possibile **Dispose** il `ConnectionMultiplexer`. Questo chiuderà tutte le connessioni e arresto la comunicazione al server.

```csharp
redisConnection.Dispose();
redisConnection = null;
```

È possibile creare un'applicazione ed eseguire alcune semplici operazioni con la cache Redis.