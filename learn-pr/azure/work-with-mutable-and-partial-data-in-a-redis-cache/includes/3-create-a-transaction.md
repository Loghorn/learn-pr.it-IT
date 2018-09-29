<span data-ttu-id="45387-101">Per iniziare, verrà creata un'istanza di Cache Redis di Azure e quindi si creerà una semplice transazione che inserisce due valori di dati nella cache.</span><span class="sxs-lookup"><span data-stu-id="45387-101">Let's start by creating an instance of Azure Redis Cache, and then create a simple transaction that inserts two data values into the cache.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="45387-102">Creare un'istanza di Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="45387-102">Create an Azure Redis Cache</span></span>

<span data-ttu-id="45387-103">Per iniziare, si vedrà come creare una cache Redis di Azure con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-103">Let's start by creating an Azure Redis Cache with the Azure CLI.</span></span> <span data-ttu-id="45387-104">Usare Cloud Shell sul lato destro della finestra del browser per interagire con Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-104">Use the Cloud Shell on the right side of the browser window to interact with Azure.</span></span>

<span data-ttu-id="45387-105">Si userà il comando `az redis create` per creare una nuova cache Redis.</span><span class="sxs-lookup"><span data-stu-id="45387-105">We'll use the `az redis create` command to create a new Azure Redis Cache.</span></span> <span data-ttu-id="45387-106">Questo comando accetta vari parametri.</span><span class="sxs-lookup"><span data-stu-id="45387-106">It takes several parameters.</span></span> <span data-ttu-id="45387-107">Questi sono i più comuni (per un elenco completo, vedere la documentazione).</span><span class="sxs-lookup"><span data-stu-id="45387-107">Here are the most common (to get a full list, check the documentation).</span></span>

> [!div class="mx-tableFixed"]
> | <span data-ttu-id="45387-108">Parametro</span><span class="sxs-lookup"><span data-stu-id="45387-108">Parameter</span></span> | <span data-ttu-id="45387-109">Descrizione</span><span class="sxs-lookup"><span data-stu-id="45387-109">Description</span></span> |
> |-----------|-------------|
> | `--name`    | <span data-ttu-id="45387-110">Nome della cache. Deve essere globalmente univoco e composto da lettere, numeri e trattini.</span><span class="sxs-lookup"><span data-stu-id="45387-110">The name of the cache - this must be globally unique and composed of letters, numbers, and dashes.</span></span> |
> | `--resource-group` | <span data-ttu-id="45387-111">Usare il gruppo di risorse creato precedentemente, **<rgn>[Nome gruppo di risorse sandbox]</rgn>**, che fa parte dell'ambiente sandbox di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-111">Use the pre-created Resource Group **<rgn>[sandbox resource group name]</rgn>**, which is part of the Azure sandbox.</span></span> |
> | `--location` | <span data-ttu-id="45387-112">Specificare la località in cui posizionare la cache.</span><span class="sxs-lookup"><span data-stu-id="45387-112">Specify the location where the cache should be located.</span></span> <span data-ttu-id="45387-113">In genere, è opportuno scegliere una località vicina ai consumer di dati.</span><span class="sxs-lookup"><span data-stu-id="45387-113">Normally, you will want to choose a location close to the data consumers.</span></span> <span data-ttu-id="45387-114">In questo caso, si è limitati alle località disponibili nella sandbox di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-114">In this case, you are limited to the locations available in the Azure sandbox.</span></span> <span data-ttu-id="45387-115">Selezionare quella più vicina.</span><span class="sxs-lookup"><span data-stu-id="45387-115">Select the closest one to you.</span></span> |
> | `--size` | <span data-ttu-id="45387-116">Dimensione dell'istanza di Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-116">The size of the Azure Redis Cache.</span></span> <span data-ttu-id="45387-117">I valori validi sono [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4].</span><span class="sxs-lookup"><span data-stu-id="45387-117">Valid values are [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4].</span></span> |
> | `--sku` | <span data-ttu-id="45387-118">SKU di Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-118">The Azure Redis Cache SKU.</span></span> <span data-ttu-id="45387-119">I valori validi sono [Basic, Standard, Premium].</span><span class="sxs-lookup"><span data-stu-id="45387-119">Valid values are [Basic, Standard, Premium].</span></span> |

### <a name="select-a-location"></a><span data-ttu-id="45387-120">Selezionare una località</span><span class="sxs-lookup"><span data-stu-id="45387-120">Select a location</span></span>
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="45387-121">Creare una cache con le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="45387-121">Create a cache using the following options:</span></span>
    - <span data-ttu-id="45387-122">Dimensioni: C0</span><span class="sxs-lookup"><span data-stu-id="45387-122">Size: C0</span></span>
    - <span data-ttu-id="45387-123">SKU: Basic</span><span class="sxs-lookup"><span data-stu-id="45387-123">SKU: Basic</span></span>

1. <span data-ttu-id="45387-124">Ecco un esempio di riga di comando.</span><span class="sxs-lookup"><span data-stu-id="45387-124">Here's an example command line.</span></span> <span data-ttu-id="45387-125">Assicurarsi di sostituire il parametro `[name]` con un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="45387-125">Make sure to replace the `[name]` parameter with a unique name.</span></span> <span data-ttu-id="45387-126">Se si vuole un'area diversa da Stati Uniti orientali, è possibile sostituire la località.</span><span class="sxs-lookup"><span data-stu-id="45387-126">You can replace the location if you want a different region than East US.</span></span>

    ```azurecli
    REDIS_NAME=[name]

    az redis create \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location eastus \
        --vm-size C0 \
        --sku Basic \
    ```

## <a name="create-a-net-core-console-application"></a><span data-ttu-id="45387-127">Creare un'applicazione console .NET Core</span><span class="sxs-lookup"><span data-stu-id="45387-127">Create a .NET Core console application</span></span>

<span data-ttu-id="45387-128">Creare quindi un'applicazione console .NET Core che verrà usata per inserire i valori dei dati in Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-128">Next, create a .NET Core console application, which will be used to insert data values into our Azure Redis Cache.</span></span>

1. <span data-ttu-id="45387-129">Creare una nuova applicazione .NET Core usando il servizio Cloud Shell integrato sul lato destro della finestra.</span><span class="sxs-lookup"><span data-stu-id="45387-129">Create a new .NET Core application using the integrated Cloud Shell on the right hand side of the window.</span></span> <span data-ttu-id="45387-130">Assegnare il nome "RedisData".</span><span class="sxs-lookup"><span data-stu-id="45387-130">Name it "RedisData".</span></span>

    ```bash
    cd ~
    dotnet new console --name RedisData
    ```

1. <span data-ttu-id="45387-131">Passare alla nuova directory creata per l'app.</span><span class="sxs-lookup"><span data-stu-id="45387-131">Change into the new directory created for your app.</span></span>

    ```bash
    cd RedisData
    ```

1. <span data-ttu-id="45387-132">Compilare ed eseguire l'applicazione. Dovrebbe venire visualizzato l'output "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="45387-132">Build and run the application - it should output "Hello World!"</span></span>

    ```bash
    dotnet run
    ```

## <a name="add-the-servicestackredis-nuget-package"></a><span data-ttu-id="45387-133">Aggiungere il pacchetto NuGet ServiceStack.Redis</span><span class="sxs-lookup"><span data-stu-id="45387-133">Add the ServiceStack.Redis NuGet package</span></span>

<span data-ttu-id="45387-134">Ora che è disponibile l'applicazione console, è necessario aggiungere il pacchetto NuGet **ServiceStack.Redis**.</span><span class="sxs-lookup"><span data-stu-id="45387-134">Now that we have our console application, we need to add the **ServiceStack.Redis** NuGet package.</span></span> <span data-ttu-id="45387-135">Ciò consentirà di connettersi a Cache Redis di Azure e inviare comandi in C#.</span><span class="sxs-lookup"><span data-stu-id="45387-135">This will allow us to connect to the Azure Redis Cache and issue commands in C#.</span></span>

1. <span data-ttu-id="45387-136">Aggiungere il pacchetto NuGet **ServiceStack.Redis** usando la shell del terminale.</span><span class="sxs-lookup"><span data-stu-id="45387-136">Add the NuGet package **ServiceStack.Redis** using the terminal shell.</span></span>

    ```bash
    dotnet add package ServiceStack.Redis
    ```

1. <span data-ttu-id="45387-137">Compilare ed eseguire nuovamente l'applicazione per assicurarsi che tutto venga compilato.</span><span class="sxs-lookup"><span data-stu-id="45387-137">Build and run the application again to make sure it all compiles.</span></span> <span data-ttu-id="45387-138">Anche in questo caso dovrebbe venire visualizzato l'output "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="45387-138">It should still output "Hello World!"</span></span>

## <a name="get-your-azure-redis-cache-connection-string"></a><span data-ttu-id="45387-139">Ottenere la stringa di connessione di Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="45387-139">Get your Azure Redis Cache connection string</span></span>

<span data-ttu-id="45387-140">Per connettersi a Cache Redis di Azure, è necessaria una stringa di connessione che contiene la password e l'URL.</span><span class="sxs-lookup"><span data-stu-id="45387-140">To connect to your Azure Redis Cache, you need a connection string that contains your password and URL.</span></span> <span data-ttu-id="45387-141">**ServiceStack.Redis** ha il proprio formato di stringa di connessione: `[password]@[hostname]:[sslport]?ssl=true`.</span><span class="sxs-lookup"><span data-stu-id="45387-141">**ServiceStack.Redis** has its own connection string format: `[password]@[hostname]:[sslport]?ssl=true`.</span></span>

<span data-ttu-id="45387-142">È possibile recuperare la password con il portale di Azure o con la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="45387-142">You can retrieve your password with the Azure portal, or with the command line.</span></span> <span data-ttu-id="45387-143">Verrà usata la riga di comando perché l'approccio con il portale è stato usato nel modulo "Ottimizzare le applicazioni Web con la memorizzazione nella cache dei dati di sola lettura con Redis".</span><span class="sxs-lookup"><span data-stu-id="45387-143">Let's use the latter here, since we used the portal approach in the "Optimize your web applications by caching read-only data with Redis" module.</span></span>

<span data-ttu-id="45387-144">Usare il comando `az redis list-keys` per ottenere le chiavi di accesso.</span><span class="sxs-lookup"><span data-stu-id="45387-144">Use the `az redis list-keys` command to get the access keys.</span></span> <span data-ttu-id="45387-145">Eseguire questi comandi per ottenere la chiave primaria, archiviarla in una variabile denominata `REDIS_KEY` e visualizzarla:</span><span class="sxs-lookup"><span data-stu-id="45387-145">Run these commands to get the primary key, store it in a variable called `REDIS_KEY`, and display it:</span></span>

```azurecli
REDIS_KEY=$(az redis list-keys \
    --name "$REDIS_NAME" \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query primaryKey \
    --output tsv)

echo $REDIS_KEY
```

<span data-ttu-id="45387-146">Successivamente, eseguire questo comando per creare la stringa di connessione e visualizzarla nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="45387-146">Next, run this command to put the connection string together and display it on the command line.</span></span> <span data-ttu-id="45387-147">Si noti che il nome host è il nome della cache seguito da `.redis.cache.windows.net` e la porta è **6380**, la porta SSL predefinita di Redis.</span><span class="sxs-lookup"><span data-stu-id="45387-147">Note that the hostname is the name of the cache followed by `.redis.cache.windows.net`, and the port is **6380**, the default Redis SSL port.</span></span>

```bash
echo "$REDIS_KEY"@"$REDIS_NAME".redis.cache.windows.net:6380?ssl=true
```

<span data-ttu-id="45387-148">Copiare la stringa di connessione negli Appunti. Verrà usata nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="45387-148">Copy the connection string to the clipboard &mdash; you'll be using it in the next step.</span></span>

## <a name="add-the-connection-string-to-your-app"></a><span data-ttu-id="45387-149">Aggiungere la stringa di connessione all'app</span><span class="sxs-lookup"><span data-stu-id="45387-149">Add the connection string to your app</span></span>

1. <span data-ttu-id="45387-150">Aprire l'editor di Cloud Shell dalla cartella dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="45387-150">Open the Cloud Shell editor from the application folder.</span></span>

    ```bash
    cd ~/RedisData
    code .
    ```

1. <span data-ttu-id="45387-151">Selezionare il file di origine **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="45387-151">Select the **Program.cs** source file.</span></span>

1. <span data-ttu-id="45387-152">Creare il campo seguente nella classe `Program` e incollarlo nella stringa di connessione come valore.</span><span class="sxs-lookup"><span data-stu-id="45387-152">Create the following field in the `Program` class and paste in your connection string as the value.</span></span>

    ```csharp
    static string redisConnectionString = "<connection string>";
    ```

    <span data-ttu-id="45387-153">Ecco un esempio:</span><span class="sxs-lookup"><span data-stu-id="45387-153">Here's an example:</span></span>

    ```csharp
    static string redisConnectionString = "ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6380?ssl=true";
    ```

## <a name="insert-two-data-values-into-your-azure-redis-cache"></a><span data-ttu-id="45387-154">Inserire due valori di dati in Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="45387-154">Insert two data values into your Azure Redis Cache</span></span>

<span data-ttu-id="45387-155">Infine, verranno aggiunti i dati in Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-155">Finally, we're going to add data into your Azure Redis Cache.</span></span>

1. <span data-ttu-id="45387-156">Aggiungere l'istruzione `using` seguente all'inizio del file **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="45387-156">Add the following `using` statement to the top of the **Program.cs** file.</span></span>

    ```csharp
    using ServiceStack.Redis;
    ```

1. <span data-ttu-id="45387-157">Sostituire il contenuto del metodo `Main` con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="45387-157">Replace the contents of the `Main` method with the following code.</span></span> <span data-ttu-id="45387-158">Verrà usata una transazione per aggiungere due valori.</span><span class="sxs-lookup"><span data-stu-id="45387-158">This will use a transaction to add two values.</span></span>

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

1. <span data-ttu-id="45387-159">Prima di compilare ed eseguire l'applicazione, verificare che il provisioning della cache Redis sia stato completato.</span><span class="sxs-lookup"><span data-stu-id="45387-159">Before building and running the application, check to make sure that the Redis cache has been fully provisioned.</span></span> <span data-ttu-id="45387-160">In alcuni casi, per il completamento di `az redis create` possono essere necessari alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="45387-160">It can sometimes take a few minutes after `az redis create` completes.</span></span> <span data-ttu-id="45387-161">Eseguire il comando seguente per controllare lo stato.</span><span class="sxs-lookup"><span data-stu-id="45387-161">Run the following command to check the status.</span></span>

    ```azcli
    az redis show \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --query provisioningState
    ```

    <span data-ttu-id="45387-162">Se lo stato è `Creating`, provare a controllarlo di nuovo dopo un paio di minuti.</span><span class="sxs-lookup"><span data-stu-id="45387-162">If the status is `Creating`, try checking again in a couple of minutes.</span></span> <span data-ttu-id="45387-163">Al termine, verrà visualizzato `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="45387-163">It will show `Succeeded` when finished.</span></span>

1. <span data-ttu-id="45387-164">Eseguire l'applicazione e verificare che la console indichi **Commit della transazione eseguito**.</span><span class="sxs-lookup"><span data-stu-id="45387-164">Run the application and verify that the console says **Transaction committed**.</span></span>

    ```bash
    dotnet run
    ```

## <a name="verify-your-data"></a><span data-ttu-id="45387-165">Verificare i dati</span><span class="sxs-lookup"><span data-stu-id="45387-165">Verify your data</span></span>

<span data-ttu-id="45387-166">Per concludere, è possibile verificare che i dati siano stati aggiunti in Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-166">To finish off, let's verify that the data we added is in our Azure Redis Cache.</span></span>

1. <span data-ttu-id="45387-167">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="45387-167">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="45387-168">Individuare l'istanza di Cache Redis di Azure selezionando **Tutte le risorse** nella barra laterale sinistra e usando la casella di filtro a sinistra per selezionare le istanze di Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-168">Locate your Azure Redis Cache by selecting **All Resources** in the left-hand sidebar, and using the filter box on the left to select Azure Redis Cache instances.</span></span> <span data-ttu-id="45387-169">In alternativa, è possibile usare la casella di ricerca nella parte superiore e digitare il nome della cache.</span><span class="sxs-lookup"><span data-stu-id="45387-169">Alternatively, you can use the search box at the top, and type the name of the cache.</span></span>

1. <span data-ttu-id="45387-170">Selezionare l'istanza di Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="45387-170">Select your Azure Redis Cache instance.</span></span>

1. <span data-ttu-id="45387-171">Nel pannello **Panoramica** per Cache Redis di Azure selezionare **Console**.</span><span class="sxs-lookup"><span data-stu-id="45387-171">In the **Overview** blade for your Azure Redis Cache, select **Console**.</span></span> <span data-ttu-id="45387-172">Verrà aperta una console di Cache Redis di Azure, che consente di immettere i comandi di Cache Redis di Azure di basso livello.</span><span class="sxs-lookup"><span data-stu-id="45387-172">This will open an Azure Redis Cache console, which allows you to enter low-level Azure Redis Cache commands.</span></span>

1. <span data-ttu-id="45387-173">Digitare **get MyKey1**.</span><span class="sxs-lookup"><span data-stu-id="45387-173">Type **get MyKey1**.</span></span> <span data-ttu-id="45387-174">Verificare che il valore restituito sia **MyValue1**.</span><span class="sxs-lookup"><span data-stu-id="45387-174">Verify that the value returned is **MyValue1**.</span></span>

1. <span data-ttu-id="45387-175">Digitare **get MyKey2**.</span><span class="sxs-lookup"><span data-stu-id="45387-175">Type **get MyKey2**.</span></span> <span data-ttu-id="45387-176">Verificare che il valore restituito sia **MyValue2**.</span><span class="sxs-lookup"><span data-stu-id="45387-176">Verify that the value returned is **MyValue2**.</span></span>

    ![Screenshot della console di Cache Redis di Azure che mostra i valori di MyKey1 e MyKey2.](../media/4-redis-console.png)