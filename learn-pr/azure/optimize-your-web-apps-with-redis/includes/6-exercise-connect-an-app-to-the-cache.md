Dopo avere creato una cache Redis in Azure, è possibile creare un'applicazione per usarla. Assicurarsi che siano disponibili le informazioni sulla stringa di connessione dal portale di Azure.

> [!NOTE]
> Il servizio Cloud Shell integrato è disponibile a destra. È possibile usare tale prompt dei comandi per creare ed eseguire il codice dell'esempio compilato qui oppure è possibile eseguire la procedura in locale, se si ha una configurazione con ambiente di sviluppo. Se si decide di usare Cloud Shell, selezionare la shell Bash se viene richiesta una selezione.

## <a name="create-a-console-application"></a>Creare un'applicazione console

Verrà usata una semplice applicazione console, per consentire di concentrarsi sull'implementazione di Redis.

1. In Cloud Shell creare una nuova applicazione console .NET Core e assegnare all'applicazione il nome "SportsStatsTracker"

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. Verrà creata una cartella per il progetto. Procedere e cambiare la directory corrente.

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a>Aggiungere la stringa di connessione

È ora possibile aggiungere al codice la stringa di connessione ottenuta dal portale di Azure. Non archiviare mai credenziali di questo tipo nel codice sorgente. Per non complicare l'esempio, verrà usato un file di configurazione. Un approccio migliore per un'applicazione lato server in Azure consiste nell'usare Azure Key Vault con certificati.

1. Creare un nuovo file **appsettings.json** da aggiungere al progetto.

    ```bash
    touch appsettings.json
    ```

1. Aprire l'editor di codice digitando `code .` nella cartella del progetto. Se si lavora in locale, è consigliabile usare **Visual Studio Code**. Questa procedura sarà allineata per la maggior parte al rispettivo utilizzo.

1. Selezionare il file **appsettings.json** file nell'editor e aggiungere il testo seguente. Incollare la stringa di connessione nell'elemento **value** dell'impostazione.

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. Salvare il file. Nell'angolo superiore destro dell'editor online è disponibile un menu relativo alle operazioni comuni per i file.

1. Aggiungere il blocco di configurazione seguente per includere il nuovo file nel progetto e copiarlo nella cartella di output. In questo modo, il file di configurazione dell'app viene inserito nella directory di output alla creazione/compilazione dell'app.

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

1. Salvare il file. Assicurarsi di eseguire questa operazione. In caso contrario le modifiche andranno perse quando si aggiunge il pacchetto più avanti.

## <a name="add-support-to-read-a-json-configuration-file"></a>Aggiungere il supporto per la lettura di un file di configurazione JSON

Un'applicazione .NET Core richiede pacchetti NuGet aggiuntivi per leggere un file di configurazione JSON.

1. Nella sezione della finestra relativa al prompt dei comandi aggiungere un riferimento al pacchetto NuGet **Microsoft.Extensions.Configuration.Json**.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Aggiungere codice per leggere il file di configurazione

Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione console.

1. Selezionare **Program.cs** nell'editor.

1. Nella parte superiore del file è presente la riga **using System;**. Al di sotto di questa riga aggiungere le righe di codice seguenti:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Sostituire il contenuto del metodo **Main** con il codice seguente. Questo codice inizializza il sistema di configurazione per la lettura dal file **appsettings.json**.

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

1. In **Program.cs** usare alla fine del metodo **Main** la nuova variabile **config** per recuperare la stringa di connessione e archiviarla in una nuova variabile denominata **connectionString**.
    - La variabile **config** include un indicizzatore, che consente di passare una stringa da recuperare dal file **appSettings.json**.

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a>Aggiungere il supporto per il client .NET della cache Redis

È ora possibile configurare l'applicazione console per usare il client **StackExchange.Redis** per .NET.

1. Aggiungere il pacchetto NuGet **StackExchange.Redis** al progetto usando il prompt dei comandi nella parte inferiore dell'editor di Cloud Shell.

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. Selezionare**Program.cs** nell'editor e aggiungere `using` per lo spazio dei nomi **StackExchange.Redis**

    ```csharp
    using StackExchange.Redis;
    ```
    
Dopo il completamento dell'installazione, il client della cache StackExchange.Redis è disponibile per l'uso con il progetto.

## <a name="connect-to-the-cache"></a>Connettersi alla cache

È ora possibile aggiungere il codice per la connessione alla cache.

1. Selezionare **Program.cs** nell'editor.

1. Creare un oggetto `ConnectionMultiplexer` usando `ConnectionMultiplexer.Connect` mediante il passaggio della stringa di connessione. Assegnare il nome **cache** al valore restituito.

1. Poiché la connessione creata è _eliminabile_, eseguirne il wrapping in un blocco `using`. Il codice dovrebbe essere simile a:

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> La connessione a Cache Redis di Azure è gestita dalla classe `ConnectionMultiplexer`. Questa classe deve essere condivisa e riutilizzata in tutta l'applicazione client. _Non_ creare una nuova connessione per ogni operazione. È consigliabile invece archiviare la connessione come campo nella classe e riutilizzarla per ogni operazione. In questo caso verrà usata solo nel metodo **Main**, ma in un'applicazione di produzione è necessario archiviarla in un campo della classe o in un singleton.

## <a name="add-a-value-to-the-cache"></a>Aggiungere un valore alla cache

Dopo avere creato la connessione è possibile aggiungere un valore alla cache.

1. Al termine della creazione della connessione, nel blocco `using` usare il metodo `GetDatabase` per recuperare un'istanza di `IDatabase`.

1. Chiamare `StringSet` nell'oggetto `IDatabase` per impostare la chiave "test:key" sul valore "some value".
    - Il valore restituito da `StringSet` è un valore `bool`, che indica se la chiave è stata aggiunta.

1. Visualizzare il valore restituito da `StringSet` nella console.

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a>Ottenere un valore dalla cache

1. Recuperare quindi il valore usando `StringGet`. Viene acquisita la chiave da recuperare e viene restituito il valore.

1. Usare come output il valore restituito.

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
    
1. Eseguire l'applicazione per visualizzare il risultato. Digitare `dotnet run` nella finestra del terminale sotto l'editor. Assicurarsi di trovarsi nella cartella del progetto. In caso contrario, non sarà possibile trovare il codice da compilare ed eseguire.
    
    ```bash
    dotnet run
    ```
    
## <a name="use-the-async-versions-of-the-methods"></a>Usare le versioni asincrone dei metodi

È stato possibile ottenere e configurare i valori dalla cache, ma vengono usate le versioni sincrone meno recenti. Nelle applicazioni lato server tali versioni non consentono un uso efficiente dei thread. È invece consigliabile usare le versioni _asincrone_ dei metodi. È possibile individuarle con facilità, perché terminano tutte con **Async**.

Per semplificare l'utilizzo di questi metodi, è possibile usare le parole chiave `async` e `await` di C#. Sarà tuttavia necessario usare _almeno_ C# 7.1 per potere applicare tali parole chiave al metodo **Main**.

### <a name="switch-to-c-71"></a>Passare a C# 7.1

Le parole chiave `async` e `await` di C# non sono parole chiave valide nei metodi **Main** nelle versioni precedenti a C# 7.1. È possibile passare con facilità a tale compilatore tramite un flag nel file **.csproj**.

1. Aprire il file **SportsStatsTracker.csproj** nell'editor.

1. Aggiungere `<LangVersion>7.1</LangVersion>` nel primo elemento `PropertyGroup` del file di compilazione. Al termine il risultato dovrebbe essere simile al seguente.
    
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

Applicare quindi la parola chiave `async` al metodo **Main**. Sarà necessario eseguire tre operazioni.

1. Aggiungere la parola chiave `async` alla firma del metodo **Main**.
1. Cambiare il tipo restituito da `void` a `Task`.
1. Aggiungere un'istruzione `using` per includere `System.Threading.Tasks`.

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

1. Usare i metodi `StringSetAsync` e `StringGetAsync` per impostare e recuperare una chiave denominata "counter". Impostare il valore su "100".

1. Applicare la parola chiave `await` per ottenere i risultati da ogni metodo.

1. Restituire come output i risultati alla finestra della console, analogamente alle versioni sincrone.

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. Eseguire di nuovo l'applicazione. Dovrebbe funzionare comunque e includere ora due valori.

#### <a name="increment-the-value"></a>Incrementare il valore

1. Usare il metodo `StringIncrementAsync` per incrementare il valore **counter**. Passare il numero **50** da aggiungere al valore counter.
    - Si noti che il metodo accetta la chiave _e_ anche `long` o `double`.
    - In base ai parametri passati, restituisce `long` o `double`.

1. Restituire come output i risultati del metodo alla console.

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a>Altre operazioni

È infine possibile provare a eseguire altri metodi con il supporto `ExecuteAsync`.

1. Eseguire "PING" per testare la connessione al server. Dovrebbe restituire una risposta "PONG".
1. Eseguire "FLUSHDB" per cancellare i valori del database. Dovrebbe restituire una risposta "OK".

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

## <a name="challenge"></a>Verifica

Come verifica, provare a serializzare un tipo di oggetto nella cache. Ecco la procedura di base.

1. Creare un nuovo valore `class` con alcune proprietà pubbliche. È possibile inventare una proprietà personalizzata, come "Person" o "Car", oppure usare l'esempio "GameStats" fornito nell'unità precedente.
1. Aggiungere il supporto per il pacchetto NuGet **Newtonsoft.Json** mediante `dotnet add package`.
1. Aggiungere `using` per lo spazio dei nomi `Newtonsoft.Json`.
1. Creare uno degli oggetti.
1. Serializzarlo con `JsonConvert.SerializeObject` e usare `StringSetAsync` per eseguirne il push nella cache.
1. Recuperarlo dalla cache con `StringGetAsync` e quindi deserializzarlo con `JsonConvert.DeserializeObject<T>`.

