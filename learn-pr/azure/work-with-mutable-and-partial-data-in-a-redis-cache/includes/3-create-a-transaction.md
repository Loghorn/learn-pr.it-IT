Per iniziare, verrà creata un'istanza di Cache Redis di Azure e quindi si creerà una semplice transazione che inserisce due valori di dati nella cache.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a>Creare un'istanza di Cache Redis di Azure

Per iniziare, si vedrà come creare una cache Redis di Azure con l'interfaccia della riga di comando di Azure. Usare Cloud Shell sul lato destro della finestra del browser per interagire con Azure.

Si userà il comando `az redis create` per creare una nuova cache Redis. Questo comando accetta vari parametri. Questi sono i più comuni (per un elenco completo, vedere la documentazione).

> [!div class="mx-tableFixed"]
> | Parametro | Descrizione |
> |-----------|-------------|
> | `--name`    | Nome della cache. Deve essere globalmente univoco e composto da lettere, numeri e trattini. |
> | `--resource-group` | Usare il gruppo di risorse creato precedentemente, **<rgn>[Nome gruppo di risorse sandbox]</rgn>**, che fa parte dell'ambiente sandbox di Azure. |
> | `--location` | Specificare la località in cui posizionare la cache. In genere, è opportuno scegliere una località vicina ai consumer di dati. In questo caso, si è limitati alle località disponibili nella sandbox di Azure. Selezionare quella più vicina. |
> | `--size` | Dimensione dell'istanza di Cache Redis di Azure. I valori validi sono [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]. |
> | `--sku` | SKU di Cache Redis di Azure. I valori validi sono [Basic, Standard, Premium]. |

### <a name="select-a-location"></a>Selezionare una località
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Creare una cache con le opzioni seguenti:
    - Dimensioni: C0
    - SKU: Basic

1. Ecco un esempio di riga di comando. Assicurarsi di sostituire il parametro `[name]` con un nome univoco. Se si vuole un'area diversa da Stati Uniti orientali, è possibile sostituire la località.

    ```azurecli
    REDIS_NAME=[name]

    az redis create \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location eastus \
        --vm-size C0 \
        --sku Basic \
    ```

## <a name="create-a-net-core-console-application"></a>Creare un'applicazione console .NET Core

Creare quindi un'applicazione console .NET Core che verrà usata per inserire i valori dei dati in Cache Redis di Azure.

1. Creare una nuova applicazione .NET Core usando il servizio Cloud Shell integrato sul lato destro della finestra. Assegnare il nome "RedisData".

    ```bash
    cd ~
    dotnet new console --name RedisData
    ```

1. Passare alla nuova directory creata per l'app.

    ```bash
    cd RedisData
    ```

1. Compilare ed eseguire l'applicazione. Dovrebbe venire visualizzato l'output "Hello World!"

    ```bash
    dotnet run
    ```

## <a name="add-the-servicestackredis-nuget-package"></a>Aggiungere il pacchetto NuGet ServiceStack.Redis

Ora che è disponibile l'applicazione console, è necessario aggiungere il pacchetto NuGet **ServiceStack.Redis**. Ciò consentirà di connettersi a Cache Redis di Azure e inviare comandi in C#.

1. Aggiungere il pacchetto NuGet **ServiceStack.Redis** usando la shell del terminale.

    ```bash
    dotnet add package ServiceStack.Redis
    ```

1. Compilare ed eseguire nuovamente l'applicazione per assicurarsi che tutto venga compilato. Anche in questo caso dovrebbe venire visualizzato l'output "Hello World!"

## <a name="get-your-azure-redis-cache-connection-string"></a>Ottenere la stringa di connessione di Cache Redis di Azure

Per connettersi a Cache Redis di Azure, è necessaria una stringa di connessione che contiene la password e l'URL. **ServiceStack.Redis** ha il proprio formato di stringa di connessione: `[password]@[hostname]:[sslport]?ssl=true`.

È possibile recuperare la password con il portale di Azure o con la riga di comando. Verrà usata la riga di comando perché l'approccio con il portale è stato usato nel modulo "Ottimizzare le applicazioni Web con la memorizzazione nella cache dei dati di sola lettura con Redis".

Usare il comando `az redis list-keys` per ottenere le chiavi di accesso. Eseguire questi comandi per ottenere la chiave primaria, archiviarla in una variabile denominata `REDIS_KEY` e visualizzarla:

```azurecli
REDIS_KEY=$(az redis list-keys \
    --name "$REDIS_NAME" \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query primaryKey \
    --output tsv)

echo $REDIS_KEY
```

Successivamente, eseguire questo comando per creare la stringa di connessione e visualizzarla nella riga di comando. Si noti che il nome host è il nome della cache seguito da `.redis.cache.windows.net` e la porta è **6380**, la porta SSL predefinita di Redis.

```bash
echo "$REDIS_KEY"@"$REDIS_NAME".redis.cache.windows.net:6380?ssl=true
```

Copiare la stringa di connessione negli Appunti. Verrà usata nel passaggio successivo.

## <a name="add-the-connection-string-to-your-app"></a>Aggiungere la stringa di connessione all'app

1. Aprire l'editor di Cloud Shell dalla cartella dell'applicazione.

    ```bash
    cd ~/RedisData
    code .
    ```

1. Selezionare il file di origine **Program.cs**.

1. Creare il campo seguente nella classe `Program` e incollarlo nella stringa di connessione come valore.

    ```csharp
    static string redisConnectionString = "<connection string>";
    ```

    Ecco un esempio:

    ```csharp
    static string redisConnectionString = "ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6380?ssl=true";
    ```

## <a name="insert-two-data-values-into-your-azure-redis-cache"></a>Inserire due valori di dati in Cache Redis di Azure

Infine, verranno aggiunti i dati in Cache Redis di Azure.

1. Aggiungere l'istruzione `using` seguente all'inizio del file **Program.cs**.

    ```csharp
    using ServiceStack.Redis;
    ```

1. Sostituire il contenuto del metodo `Main` con il codice seguente. Verrà usata una transazione per aggiungere due valori.

    ```csharp
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    if (transactionResult)
    {
        Console.WriteLine("Transaction committed");
    }
    else
    {
        Console.WriteLine("Transaction failed to commit");
    }
    ```

1. Prima di compilare ed eseguire l'applicazione, verificare che il provisioning della cache Redis sia stato completato. In alcuni casi, per il completamento di `az redis create` possono essere necessari alcuni minuti. Eseguire il comando seguente per controllare lo stato.

    ```azcli
    az redis show \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --query provisioningState
    ```

    Se lo stato è `Creating`, provare a controllarlo di nuovo dopo un paio di minuti. Al termine, verrà visualizzato `Succeeded`.

1. Eseguire l'applicazione e verificare che la console indichi **Commit della transazione eseguito**.

    ```bash
    dotnet run
    ```

## <a name="verify-your-data"></a>Verificare i dati

Per concludere, è possibile verificare che i dati siano stati aggiunti in Cache Redis di Azure.

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Individuare l'istanza di Cache Redis di Azure selezionando **Tutte le risorse** nella barra laterale sinistra e usando la casella di filtro a sinistra per selezionare le istanze di Cache Redis di Azure. In alternativa, è possibile usare la casella di ricerca nella parte superiore e digitare il nome della cache.

1. Selezionare l'istanza di Cache Redis di Azure.

1. Nel pannello **Panoramica** per Cache Redis di Azure selezionare **Console**. Verrà aperta una console di Cache Redis di Azure, che consente di immettere i comandi di Cache Redis di Azure di basso livello.

1. Digitare **get MyKey1**. Verificare che il valore restituito sia **MyValue1**.

1. Digitare **get MyKey2**. Verificare che il valore restituito sia **MyValue2**.

    ![Screenshot della console di Cache Redis di Azure che mostra i valori di MyKey1 e MyKey2.](../media/4-redis-console.png)