È possibile iniziare creando un'istanza di Cache Redis di Azure e quindi creare una semplice transazione che inserisce i due valori di dati nella cache.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a>Creare una cache Redis di Azure

Iniziamo creando una Cache Redis di Azure con la CLI di Azure. Usare Cloud Shell sul lato destro della finestra del browser per interagire con Azure.

Si userà il `azure rediscache create` comando per creare una nuova Cache Redis di Azure. Richiede diversi parametri. Ecco i più comuni (per ottenere un elenco completo, consultare la documentazione).

> [!div class="mx-tableFixed"]
> | Parametro | Descrizione |
> |-----------|-------------|
> | `--name`    | Il nome della cache - deve essere globalmente univoco e composto da lettere, numeri e trattini. |
> | `--resource-group` | Usare il gruppo di risorse creato in precedenza <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>, che fa parte della Sandbox di Azure. |
> | `--location` | Specificare il percorso in cui la cache deve essere disponibile. In genere, è opportuno scegliere una località vicina i consumer di dati. In questo caso, si è limitati ai percorsi disponibili nell'ambiente Sandbox di Azure. Selezionare quella più vicina all'utente. |
> | `--size` | Le dimensioni della Cache Redis di Azure. I valori validi sono [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]. |
> | `--sku` | La Cache Redis di Azure lo SKU. I valori validi sono [Basic, Standard, Premium]. |
> | `--enable-non-ssl-port` | Aggiungere questo flag se si desidera abilitare la porta non SSL per la cache. |

### <a name="selecting-a-location"></a>Selezione di un percorso
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Creare una cache usando le opzioni seguenti:
    - Dimensioni: C0
    - SKU: Basic
    - EnableNonSslPort
    
1. Ecco un esempio di riga di comando. Assicurarsi di sostituire il **[nome]** e **[percorso]** con i valori validi.

    ```azurecli
    azure rediscache create \
        --name [name] \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location [location] \
        --size C0 --sku Basic
    ```

Richiederà alcuni minuti per creare la cache.

## <a name="create-a-net-core-console-application"></a>Creare un'applicazione console .NET Core

Successivamente, creare un'applicazione console basata su .NET Core in c#, che verrà usata per inserire i valori dei dati in Cache Redis di Azure.

1. Creare una nuova applicazione .NET Core Usa Cloud Shell integrata su lato destro della finestra. Denominarla "RedisData".

    ```bash
    dotnet new console --name RedisData
    ```
    
1. Passare alla nuova directory creata per l'app.

    ```bash
    cd RedisData
    ```
    
1. È necessario trovare un file di progetto e una singola **Program.cs** file di origine.

1. Compilare ed eseguire l'applicazione, deve restituire come output "Hello, World!"

    ```bash
    dotnet run
    ```
    
## <a name="add-the-servicestackredis-nuget-package"></a>Aggiungere il pacchetto ServiceStack.Redis NuGet

Ora che abbiamo l'applicazione console, è necessario aggiungere il **ServiceStack.Redis** pacchetto NuGet. In questo modo sarà possibile connettersi per la Cache Redis di Azure ed eseguire comandi nel linguaggio c#.

1. Aggiungere il pacchetto NuGet **ServiceStack.Redis** usando la shell del terminale.

    ```bash
    dotnet add package ServiceStack.Redis
    ```
    
1. Compilare ed eseguire nuovamente l'applicazione per assicurarsi che tutto venga compilato. Deve comunque restituire come output "Hello, World!"

## <a name="get-your-azure-redis-cache-connection-string"></a>Ottenere la stringa di connessione di Cache Redis di Azure

Per connettersi a Cache Redis di Azure, è necessaria una stringa di connessione che contiene la password e l'URL. È univoca per questa stringa di connessione **ServiceStack.Redis**ed è nel formato: `[password]@[host name]:[port]`.

È possibile recuperare la chiave con il portale di Azure o con la riga di comando. Utilizziamo qui quest'ultima, poiché è stato usato l'approccio basato sul portale nel modulo "Ottimizzare le applicazioni web per la memorizzazione nella cache i dati di sola lettura con Redis".

Usare il `azure rediscache list-keys` comando per ottenere le chiavi di accesso. È necessario fornire il nome e gruppo di risorse, come illustrato di seguito:

```azurecli
azure rediscache list-keys \
    --name [name] \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

Restituirà quanto segue:

```output
Primary Key   : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
Secondary Key : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
```

1. Copia il **primaria** chiave negli Appunti.

1. Il nome host corrisponderà al nome scelto per la cache quando è stato creato, con il suffisso `.redis.cache.windows.net`.

1. La porta deve essere **6379**.

1. È possibile verificare tutte le informazioni nella console con il `azure rediscache list` comando. Questo comando Mostra tutte le istanze di Cache Redis di Azure nella sottoscrizione attiva (in questo caso, l'ambiente Sandbox di Azure).

## <a name="add-the-connection-string-to-your-app"></a>Aggiungere la stringa di connessione all'app

1. Assicurarsi che si è nella cartella dell'app. Il **Program.cs** devono trovarsi nella cartella corrente, se si digita `ls` o `dir`.

1. Aprire l'editor predefinito digitando `code .` nella cartella dell'app.

1. Selezionare il **Program.cs** file di origine.

1. Creare il campo seguente nel `Program` classe.

    ```csharp
    static string redisConnectionString = "";
    ```

1. Incollare la stringa di connessione nel **redisConnectionString** campo appena creato e usare **6379** come il numero di porta. Tenere presente che la stringa di connessione è nel formato: `[password]@[host name]:[port]`.

Una stringa di connessione di esempio sarebbe simile al seguente:

```output
ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6379
```
    
## <a name="insert-two-data-values-into-your-azure-redis-cache"></a>Inserire due valori di dati in Cache Redis di Azure

Infine, dobbiamo aggiungere dati in Cache Redis di Azure.

1. Aggiungere il codice seguente `using` all'inizio dell'istruzione il **Program.cs** file.

    ```csharp
    using ServiceStack.Redis;
    ```

1. Aggiungere il seguente frammento di codice nel **Main** (metodo). Consente di aggiungere due valori in termini di transazioni.

    ```csharp
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create the transaction
        var transaction = redisClient.CreateTransaction();

        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        var transactionResult = transaction.Commit();

        if(transactionResult)
            Console.WriteLine("Transaction committed");
        else
            Console.WriteLine("Transaction failed to commit");
    }
    ```
1. Eseguire l'applicazione tramite il prompt dei comandi nella parte inferiore della finestra dell'editor e verificare che la console viene visualizzato **commit della transazione**. 

    ```bash
    dotnet run
    ```
    
## <a name="verify-your-data"></a>Verificare i dati

Per concludere, è possibile verificare che i dati che è stata aggiunta in Cache Redis di Azure.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Individuare Cache Redis di Azure selezionando **tutte le risorse** nella barra laterale sinistra e usando la casella di filtro a sinistra per selezionare le istanze di Cache Redis di Azure. In alternativa, è possibile usare la casella di ricerca nella parte superiore e digitare il nome della cache.

1. Selezionare l'istanza di Cache Redis di Azure.

1. Nel **Overview** pannello Cache Redis di Azure, selezionare **Console**. Verrà aperta una console di Cache Redis di Azure che consente di immettere i comandi di Cache Redis di Azure a basso livello.

1. Tipo di **ottenere MyKey1**. Verificare che il valore restituito sia **MyValue1**.

1. Tipo di **ottenere MyKey2**. Verificare che il valore restituito sia **MyValue2**.

    ![Screenshot della console di Cache Redis di Azure che mostra i valori di MyKey1 e MyKey2.](../media/4-redis-console.png)