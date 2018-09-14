Ora che abbiamo una cache Redis creata in Azure, è possibile creare un'applicazione per usarla. Assicurarsi di avere le informazioni della stringa di connessione dal portale di Azure.

> [!NOTE]
> Cloud Shell integrata è disponibile sulla destra. È possibile usare il prompt dei comandi per creare ed eseguire qui il codice di esempio che stiamo creando o eseguire questi passaggi in locale se si dispone di una configurazione di ambiente di sviluppo. Se si decide di usare Cloud Shell, selezionare la shell Bash se viene richiesto per una selezione.

## <a name="create-a-console-application"></a>Creare un'applicazione Console

Si userà una semplice applicazione Console in modo che sono tutti concentrati sull'implementazione di Redis.

1. In Cloud Shell, creare una nuova applicazione Console di .NET Core, denominarla "SportsStatsTracker"

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. Questo crea una cartella per il progetto, proseguire e modificare la directory corrente.

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a>Aggiungere la stringa di connessione

Aggiungiamo la stringa di connessione che viene recuperate dal portale di Azure nel codice. Non archiviare mai le credenziali come segue nel codice sorgente. Per semplificare questo esempio, si userà un file di configurazione. Sarebbe un approccio migliore per un'applicazione lato server in Azure per usare Azure Key Vault con i certificati.

1. Creare una nuova **appSettings. JSON** file da aggiungere al progetto.

    ```bash
    touch appsettings.json
    ```

1. Aprire l'editor di codice digitando `code .` nella cartella del progetto. Se si lavora in locale, è consigliabile usare **Visual Studio Code**. La procedura seguente consente di allineare principalmente con relativo utilizzo.

1. Selezionare il **appSettings. JSON** file nell'editor e aggiungere il testo seguente. Incollare la stringa di connessione nel **valore** dell'impostazione.

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. Salvare il file, nell'editor online, è un menu nell'angolo superiore destro con operazioni su file comuni.

1. Aggiungere il seguente blocco di configurazione per includere il nuovo file nel progetto e copiarlo nella cartella di output. In questo modo, il file di configurazione dell'app viene inserito nella directory di output alla creazione/compilazione dell'app.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. Salvare il file. (Assicurarsi che si esegue questa operazione oppure si perderà la modifica quando si aggiunge il pacchetto seguente).

## <a name="add-support-to-read-a-json-configuration-file"></a>Aggiungere il supporto per la lettura di un file di configurazione JSON

Un'applicazione .NET Core richiede altri pacchetti NuGet per leggere un file di configurazione JSON.

1. Nella sezione della finestra del prompt dei comandi, aggiungere un riferimento per la **Microsoft.Extensions.Configuration.Json** pacchetto NuGet.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Aggiungere il codice per leggere il file di configurazione

Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione console.

1. Selezionare **Program.cs** nell'editor.

1. Nella parte superiore del file è presente la riga **using System;**. Al di sotto di questa riga aggiungere le righe di codice seguenti:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Sostituire il contenuto del **Main** metodo con il codice seguente. Questo codice inizializza il sistema di configurazione per la lettura dal file **appsettings.json**.

    ```csharp
    var config = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json")
        .Build();
    ```

Il file **Program.cs** avrà ora un aspetto simile al seguente:

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace SportsStatsTracker
{
    class Program
    {
        static void Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();
        }
    }
}
```

## <a name="get-the-connection-string-from-configuration"></a>Ottenere la stringa di connessione dalla configurazione

1. Nella **Program.cs**, alla fine del **Main** metodo, usare il nuovo **config** variabile per recuperare la stringa di connessione e archiviarlo in una nuova variabile denominata  **connectionString**.
    - Il **config** variabile ha un indicizzatore in cui è possibile passare una stringa da recuperare dalle **appSettings. JSON** file.

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a>Aggiungere il supporto per il client .NET di cache Redis

Successivamente, è possibile configurare l'applicazione console per usare la **stackexchange. Redis** client per .NET.

1. Aggiungere il **stackexchange. Redis** pacchetto NuGet al progetto tramite il prompt dei comandi nella parte inferiore dell'editor di Cloud Shell.

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. Selezionare **Program.cs** nell'editor e aggiungere un `using` dello spazio dei nomi **stackexchange. Redis**

    ```csharp
    using StackExchange.Redis;
    ```
    
Al termine dell'installazione, il client della cache Redis è disponibile per l'uso con il progetto.

## <a name="connect-to-the-cache"></a>Connettersi alla cache

Aggiungere il codice per la connessione alla cache.

1. Selezionare **Program.cs** nell'editor.

1. Creare un `ConnectionMultiplexer` usando `ConnectionMultiplexer.Connect` passando la stringa di connessione. Nome del valore restituito **cache**.

1. Poiché la connessione creata _disposable_, il wrapping in un `using` blocco. Il codice dovrebbe essere simile a:

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> La connessione a Cache Redis di Azure è gestita dal `ConnectionMultiplexer` classe. Questa classe deve essere condivisa e riutilizzata in tutta l'applicazione client. Facciamo _non_ per creare una nuova connessione per ogni operazione. Al contrario, vogliamo archiviarlo come campo nella classe e usarlo di nuovo per ogni operazione. In questo caso solo si intende usarlo nel **Main** (metodo), ma in un'applicazione di produzione devono essere archiviato in un campo della classe o un singleton.

## <a name="add-a-value-to-the-cache"></a>Aggiungere un valore nella cache

A questo punto è che la connessione, è possibile aggiungere un valore nella cache.

1. All'interno di `using` blocchi dopo la connessione è stata creata, usare il `GetDatabase` metodo per recuperare un `IDatabase` istanza.

1. Chiamare `StringSet` nella `IDatabase` oggetto per impostare la chiave "chiave: test" al valore "un valore".
    - il valore restituito da `StringSet` è un `bool` che indica se la chiave è stato aggiunto.

1. Visualizzare il valore restituito da `StringSet` alla console.

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a>Ottenere un valore dalla cache

1. Successivamente, recuperare il valore usando `StringGet`. Si accetta la chiave per recuperare e restituisce il valore.

1. Il valore restituito di output.

    ```csharp
    string getValue = db.StringGet("test:key");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. Il codice dovrebbe essere simile al seguente:

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;
    using StackExchange.Redis;
    
    namespace SportsStatsTracker
    {
        class Program
        {
            static void Main(string[] args)
            {
                var config = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile("appsettings.json")
                    .Build();
    
                string connectionString = config["CacheConnection"];
    
                using (var cache = ConnectionMultiplexer.Connect(connectionString))
                {
                    var db = cache.GetDatabase();
    
                    bool setValue = db.StringSet("test:key", "some value");
                    Console.WriteLine($"SET: {setValue}");
    
                    string getValue = db.StringGet("test:key");
                    Console.WriteLine($"GET: {getValue}");
                }
            }
        }
    }
    ```
    
1. Eseguire l'applicazione per visualizzare il risultato. Tipo `dotnet run` nella finestra del terminale sotto l'editor. Assicurarsi che si è nella cartella del progetto o non troverà il codice per compilare ed eseguire.
    
    ```bash
    dotnet run
    ```
    
## <a name="use-the-async-versions-of-the-methods"></a>Usare le versioni asincrone dei metodi

È stato in grado di ottenere e impostare i valori dalla cache, ma si sta usando le precedenti versioni sincrone. Nelle applicazioni lato server, questi non sono un uso efficiente dei nostri thread. Al contrario, si vuole usare il _asincrona_ versioni dei metodi. È possibile individuare più facilmente: tutti terminano **Async**.

Per rendere più semplice da utilizzare questi metodi, è possibile usare il codice c# `async` e `await` parole chiave. Tuttavia, è necessario usare _almeno_ c# 7.1 per essere in grado di applicare queste parole chiave al nostro **Main** (metodo).

### <a name="switch-to-c-71"></a>Passare al linguaggio c# 7.1

# `async` e `await` parole chiave non sono parole chiave valide in **Main** metodi fino al linguaggio c# 7.1. È possibile passare facilmente alla tramite un flag nel compilatore il **file con estensione csproj** file.

1. Aprire il **SportsStatsTracker.csproj** file nell'editor.

1. Aggiungere `<LangVersion>7.1</LangVersion>` nella prima `PropertyGroup` nel file di compilazione. Dovrebbe essere simile al seguente al termine.
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a>Applicare la parola chiave async

Successivamente, si applicano i `async` parola chiave per il **Main** (metodo). Sarà necessario eseguire tre operazioni.

1. Aggiungere il `async` parola chiave nel **Main** firma del metodo.
1. Modificare il tipo restituito da `void` a `Task`.
1. Aggiungere un `using` istruzione includa `System.Threading.Tasks`.

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using StackExchange.Redis;
using System.Threading.Tasks;

namespace SportsStatsTracker
{
    class Program
    {
        static async Task Main(string[] args)
        {
        ...
```

### <a name="get-and-set-values-asynchronously"></a>Ottenere e impostare i valori in modo asincrono

1. Usare la `StringSetAsync` e `StringGetAsync` metodi per impostare e recuperare una chiave denominata "counter". Impostare il valore su "100".

1. Applicare il `await` parola chiave per ottenere i risultati da ogni metodo.

1. I risultati alla finestra della console - allo stesso modo con le versioni sincrone.

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. Eseguire nuovamente - l'applicazione dovrebbe continuano a funzionare e disporrà di due valori.

#### <a name="increment-the-value"></a>Incrementare il valore

1. Usare la `StringIncrementAsync` metodo per incrementare le **contatore** valore. Passare il numero **50** per il contatore.
    - Si noti che il metodo accetta la chiave _e_ sia un `long` o `double`.
    - A seconda dei parametri passati, restituisce un `long` o `double`.

1. I risultati del metodo nella console.

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a>Altre operazioni

Infine, è possibile provare a eseguire alcuni metodi aggiuntivi con il `ExecuteAsync` supportano.

1. Eseguire il comando "PING" per testare la connessione al server. Deve rispondere con "PONG".
1. Eseguire "FLUSHDB" per cancellare i valori del database. Deve rispondere con "OK".

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

## <a name="challenge"></a>Richiesta di verifica

Come un problema, provare a serializzare un tipo di oggetto nella cache. Ecco i passaggi di base.

1. Creare un nuovo `class` con alcune proprietà pubbliche. Poter inventare uno personalizzato ("Person" o "Automobile" sono più diffusi), oppure usare l'esempio "GameStats" espressa in unità di precedente.
1. Aggiungere il supporto per la **newtonsoft. JSON** pacchetto NuGet con `dotnet add package`.
1. Aggiungere un `using` per il `Newtonsoft.Json` dello spazio dei nomi.
1. Creare uno degli oggetti.
1. Eseguirne la serializzazione con `JsonConvert.SerializeObject` e usare `StringSetAsync` effettuarne il push nella cache.
1. Recuperarlo dalla cache con `StringGetAsync` e quindi come deserializzare con `JsonConvert.DeserializeObject<T>`.

