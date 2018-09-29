<span data-ttu-id="c97d2-101">L'accesso e l'elaborazione dei dati sono le attività principali in molte soluzioni software.</span><span class="sxs-lookup"><span data-stu-id="c97d2-101">Accessing and processing data are key tasks in many software solutions.</span></span> <span data-ttu-id="c97d2-102">Prendere in considerazione alcuni di questi scenari:</span><span class="sxs-lookup"><span data-stu-id="c97d2-102">Consider some of these scenarios:</span></span>

* <span data-ttu-id="c97d2-103">È stato richiesto di implementare un modo per spostare i dati in ingresso dall'archiviazione BLOB ad Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c97d2-103">You've been asked to implement a way to move incoming data from Blob storage to Azure Cosmos DB.</span></span>
* <span data-ttu-id="c97d2-104">Si vogliono pubblicare i messaggi in arrivo in una coda per elaborarli tramite un altro componente aziendale.</span><span class="sxs-lookup"><span data-stu-id="c97d2-104">You want to post incoming messages to a queue for processing by another component in your enterprise.</span></span>
* <span data-ttu-id="c97d2-105">Il servizio deve recuperare i punteggi dei giocatori da una coda e aggiornare un tabellone online.</span><span class="sxs-lookup"><span data-stu-id="c97d2-105">Your service needs to grab gamer scores from a queue and update an online scoreboard.</span></span>

<span data-ttu-id="c97d2-106">Tutti questi esempi riguardano lo spostamento dei dati.</span><span class="sxs-lookup"><span data-stu-id="c97d2-106">All of these examples are about moving data.</span></span> <span data-ttu-id="c97d2-107">L'origine dati e le destinazioni sono diverse a seconda dello scenario, ma il modello è simile.</span><span class="sxs-lookup"><span data-stu-id="c97d2-107">The data source and destinations differ from scenario to scenario, but the pattern is similar.</span></span> <span data-ttu-id="c97d2-108">Ci si connette a un'origine dati e si leggono e scrivono dati.</span><span class="sxs-lookup"><span data-stu-id="c97d2-108">You connect to a data source, and you read and write data.</span></span> <span data-ttu-id="c97d2-109">Funzioni di Azure consente l'integrazione con dati e servizi tramite le associazioni.</span><span class="sxs-lookup"><span data-stu-id="c97d2-109">Azure Functions helps you integrate with data and services by using bindings.</span></span> 

## <a name="what-is-a-binding"></a><span data-ttu-id="c97d2-110">Che cos'è un'associazione?</span><span class="sxs-lookup"><span data-stu-id="c97d2-110">What is a binding?</span></span>

<span data-ttu-id="c97d2-111">In Funzioni di Azure le associazioni forniscono una modalità dichiarativa per connettersi ai dati dall'interno del codice.</span><span class="sxs-lookup"><span data-stu-id="c97d2-111">In Azure Functions, bindings provide a declarative way to connect to data from within your code.</span></span> <span data-ttu-id="c97d2-112">Rendono più facile un'integrazione coerente con i flussi dei dati in una funzione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-112">They make it easier to integrate with data streams consistently in a function.</span></span> <span data-ttu-id="c97d2-113">È possibile avere più associazioni che forniscono accesso a elementi dati diversi.</span><span class="sxs-lookup"><span data-stu-id="c97d2-113">You can have multiple bindings providing access to different data elements.</span></span> <span data-ttu-id="c97d2-114">Si tratta di una funzionalità molto efficace perché è possibile connettersi alle origini dati senza dover scrivere codice per una logica di connessione specifica (come connessioni di database o interfacce API Web).</span><span class="sxs-lookup"><span data-stu-id="c97d2-114">This is powerful because you can connect to your data sources without having to code specific connection logic (like database connections or web API interfaces).</span></span>

### <a name="types-of-bindings"></a><span data-ttu-id="c97d2-115">Tipi di associazioni</span><span class="sxs-lookup"><span data-stu-id="c97d2-115">Types of bindings</span></span>

<span data-ttu-id="c97d2-116">Ci sono due tipi di associazioni che è possibile usare con le funzioni:</span><span class="sxs-lookup"><span data-stu-id="c97d2-116">There are two kinds of bindings you can use with functions:</span></span>

1. <span data-ttu-id="c97d2-117">**Associazione di input** Un'associazione di input è una connessione a un'**origine** dati.</span><span class="sxs-lookup"><span data-stu-id="c97d2-117">**Input binding** An input binding is a connection to a data **source**.</span></span> <span data-ttu-id="c97d2-118">La funzione può leggere i dati da tali input.</span><span class="sxs-lookup"><span data-stu-id="c97d2-118">Our function can read data from these inputs.</span></span>

1. <span data-ttu-id="c97d2-119">**Associazione di output** Un'associazione di output è una connessione a una **destinazione** dati.</span><span class="sxs-lookup"><span data-stu-id="c97d2-119">**Output binding** An output binding is a connection to a data **destination**.</span></span> <span data-ttu-id="c97d2-120">La funzione scrive i dati in tali destinazioni.</span><span class="sxs-lookup"><span data-stu-id="c97d2-120">Our function can write data to these destinations.</span></span>

<span data-ttu-id="c97d2-121">Ci sono anche i trigger.</span><span class="sxs-lookup"><span data-stu-id="c97d2-121">There are also triggers.</span></span> <span data-ttu-id="c97d2-122">I trigger sono tipi speciali di associazioni di input che causano l'esecuzione di una funzione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-122">Triggers are special types of input bindings that cause a function to execute.</span></span> <span data-ttu-id="c97d2-123">Ad esempio, una notifica di Griglia di eventi di Azure può essere configurata come trigger.</span><span class="sxs-lookup"><span data-stu-id="c97d2-123">For example, an Azure Event Grid notification can be configured as a trigger.</span></span> <span data-ttu-id="c97d2-124">Quando si verifica un evento, la funzione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="c97d2-124">When an event occurs, the function will run.</span></span>

### <a name="types-of-supported-bindings"></a><span data-ttu-id="c97d2-125">Tipi di associazioni supportate</span><span class="sxs-lookup"><span data-stu-id="c97d2-125">Types of supported bindings</span></span>

<span data-ttu-id="c97d2-126">Il *tipo* di associazione definisce dove si leggono o si inviano i dati.</span><span class="sxs-lookup"><span data-stu-id="c97d2-126">The *type* of binding defines where we are reading or sending data.</span></span> <span data-ttu-id="c97d2-127">Sono disponibili un'associazione per rispondere alle richieste Web e un'ampia gamma di associazioni per interagire direttamente con diversi servizi di Azure e servizi di terze parti.</span><span class="sxs-lookup"><span data-stu-id="c97d2-127">There is a binding to respond to web requests and a large selection of bindings to interact directly with various Azure services as well as third-party services.</span></span>

<span data-ttu-id="c97d2-128">Un tipo di associazione può essere usato come input, output o entrambi.</span><span class="sxs-lookup"><span data-stu-id="c97d2-128">A binding type can be used as an input, an output or both.</span></span> <span data-ttu-id="c97d2-129">Una funzione, ad esempio, può scrivere nell'associazione di output di Archiviazione BLOB di Azure, ma un aggiornamento dell'archiviazione BLOB può attivare un'altra funzione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-129">For example, a function can write to Azure Blob Storage output binding, but a blob storage update could trigger another function.</span></span>

<span data-ttu-id="c97d2-130">Di seguito sono elencati alcuni tipi di associazioni comuni:</span><span class="sxs-lookup"><span data-stu-id="c97d2-130">Some common binding types are listed below:</span></span>
- <span data-ttu-id="c97d2-131">Archiviazione BLOB</span><span class="sxs-lookup"><span data-stu-id="c97d2-131">Blob Storage</span></span>
- <span data-ttu-id="c97d2-132">Code del bus di servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="c97d2-132">Azure Service Bus Queues</span></span>
- <span data-ttu-id="c97d2-133">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c97d2-133">Azure Cosmos DB</span></span>
- <span data-ttu-id="c97d2-134">Hub eventi di Azure</span><span class="sxs-lookup"><span data-stu-id="c97d2-134">Azure Event Hubs</span></span>
- <span data-ttu-id="c97d2-135">File esterni</span><span class="sxs-lookup"><span data-stu-id="c97d2-135">External Files</span></span>
- <span data-ttu-id="c97d2-136">Tabelle esterne</span><span class="sxs-lookup"><span data-stu-id="c97d2-136">External Tables</span></span>
- <span data-ttu-id="c97d2-137">Endpoint HTTP</span><span class="sxs-lookup"><span data-stu-id="c97d2-137">HTTP endpoints</span></span>

<span data-ttu-id="c97d2-138">Questi tipi rappresentano solo alcuni esempi.</span><span class="sxs-lookup"><span data-stu-id="c97d2-138">These types are just a sample.</span></span> <span data-ttu-id="c97d2-139">Ci sono altri tipi e le funzioni hanno inoltre un modello di estendibilità che permette di aggiungere altre associazioni.</span><span class="sxs-lookup"><span data-stu-id="c97d2-139">There are more, plus functions have an extensibility model to add more bindings.</span></span>

### <a name="binding-properties"></a><span data-ttu-id="c97d2-140">Proprietà delle associazioni</span><span class="sxs-lookup"><span data-stu-id="c97d2-140">Binding properties</span></span>

<span data-ttu-id="c97d2-141">Ci sono tre proprietà obbligatorie in tutte le associazioni.</span><span class="sxs-lookup"><span data-stu-id="c97d2-141">Three properties are required in all bindings.</span></span> <span data-ttu-id="c97d2-142">Può essere necessario specificare proprietà aggiuntive in base al tipo di associazione e alla risorsa di archiviazione usata.</span><span class="sxs-lookup"><span data-stu-id="c97d2-142">You may have to supply additional properties based on the type of binding and storage you are using.</span></span>

1. <span data-ttu-id="c97d2-143">**Name** Definisce il parametro della funzione tramite cui si accede ai dati.</span><span class="sxs-lookup"><span data-stu-id="c97d2-143">**Name** Defines the function parameter through which you access the data.</span></span> <span data-ttu-id="c97d2-144">In un'associazione di input di coda, ad esempio, si tratta del nome del parametro della funzione che riceve il contenuto del messaggio della coda.</span><span class="sxs-lookup"><span data-stu-id="c97d2-144">For example, in a queue input binding, this is the name of the function parameter that receives the queue message content.</span></span> 

1. <span data-ttu-id="c97d2-145">**Type** Identifica il tipo di associazione, ad esempio il tipo di dati o il servizio con cui si vuole interagire.</span><span class="sxs-lookup"><span data-stu-id="c97d2-145">**Type** Identifies the type of binding, i.e., the type of data or service we want to interact with.</span></span>

1. <span data-ttu-id="c97d2-146">**Direction** Indica la direzione del flusso dei dati, ovvero se si tratta di un'associazione di input o di output.</span><span class="sxs-lookup"><span data-stu-id="c97d2-146">**Direction** Indicates the direction data is flowing, i.e., is it an input or output binding?</span></span>

<span data-ttu-id="c97d2-147">La maggior parte dei tipi di associazione necessita anche di una quarta proprietà:</span><span class="sxs-lookup"><span data-stu-id="c97d2-147">Additionally, most binding types also need a fourth property:</span></span> 

4. <span data-ttu-id="c97d2-148">**Connection** Fornisce il nome di una chiave di impostazione dell'app che contiene la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-148">**Connection** Provides the name of an app setting key that contains the connection string.</span></span> <span data-ttu-id="c97d2-149">Le associazioni usano stringhe di connessione archiviate nelle impostazioni dell'app per mantenere i segreti all'esterno del codice della funzione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-149">Bindings use connection strings stored in app settings to keep secrets out of the function code.</span></span> <span data-ttu-id="c97d2-150">Ciò agevola la configurazione del codice e lo rende più sicuro.</span><span class="sxs-lookup"><span data-stu-id="c97d2-150">This makes your code more configurable and secure.</span></span>

## <a name="create-a-binding"></a><span data-ttu-id="c97d2-151">Creare un'associazione</span><span class="sxs-lookup"><span data-stu-id="c97d2-151">Create a binding</span></span>

<span data-ttu-id="c97d2-152">Le associazioni sono definite in JSON.</span><span class="sxs-lookup"><span data-stu-id="c97d2-152">Bindings are defined in JSON.</span></span> <span data-ttu-id="c97d2-153">Un'associazione viene configurata nel file di configurazione della funzione, denominato *function.json*, e si trova nella stessa cartella del codice della funzione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-153">A binding is configured in your function's configuration file, which is named *function.json* and lives in the same folder as your function code.</span></span>

 <span data-ttu-id="c97d2-154">Verrà ora esaminato un esempio di *associazione di input*:</span><span class="sxs-lookup"><span data-stu-id="c97d2-154">Let's examine a sample *input binding*:</span></span>

```json
    ...
    {
      "name": "headshotBlob",
      "type": "blob",
      "path": "thumbnail-images/{filename}",
      "connection": "HeadshotStorageConnection",
      "direction": "in"
    },
    ...
```

<span data-ttu-id="c97d2-155">Per creare questa associazione, è necessario:</span><span class="sxs-lookup"><span data-stu-id="c97d2-155">To create this binding, we:</span></span>

1. <span data-ttu-id="c97d2-156">Creare un'associazione nel file *function.json*.</span><span class="sxs-lookup"><span data-stu-id="c97d2-156">Create a binding in our *function.json* file.</span></span>

1. <span data-ttu-id="c97d2-157">Specificare il valore per la variabile `name`.</span><span class="sxs-lookup"><span data-stu-id="c97d2-157">Provide the value for the `name` variable.</span></span> <span data-ttu-id="c97d2-158">In questo esempio la variabile contiene i dati BLOB.</span><span class="sxs-lookup"><span data-stu-id="c97d2-158">In this example, the variable holds the blob data.</span></span>

1. <span data-ttu-id="c97d2-159">Specificare la proprietà `type` per l'associazione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-159">Provide the storage `type`.</span></span> <span data-ttu-id="c97d2-160">Nell'esempio precedente viene usata l'archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="c97d2-160">In the preceding example, we are using Blob storage.</span></span>

1. <span data-ttu-id="c97d2-161">Specificare la proprietà `path`, che indica il contenitore e il nome dell'elemento inserito.</span><span class="sxs-lookup"><span data-stu-id="c97d2-161">Provide the `path`, which specifies the container and the item name that goes in it.</span></span> <span data-ttu-id="c97d2-162">La proprietà `path` è obbligatoria per i BLOB.</span><span class="sxs-lookup"><span data-stu-id="c97d2-162">The `path` property is required for blobs.</span></span>

1. <span data-ttu-id="c97d2-163">Specificare il nome dell'impostazione della stringa `connection` definito nel file delle impostazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-163">Provide the `connection` string setting name defined in the application's settings file.</span></span> <span data-ttu-id="c97d2-164">Viene usato come chiave per trovare la stringa di connessione per connettersi all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-164">It's used as a key to find the connection string to connect to your storage account.</span></span>

1. <span data-ttu-id="c97d2-165">Definire `direction` come `in`.</span><span class="sxs-lookup"><span data-stu-id="c97d2-165">Define the `direction` as `in`.</span></span> <span data-ttu-id="c97d2-166">Vengono letti i dati dal BLOB.</span><span class="sxs-lookup"><span data-stu-id="c97d2-166">It reads data from the blob.</span></span>

<span data-ttu-id="c97d2-167">Le associazioni vengono usate per connettersi ai dati nella funzione.</span><span class="sxs-lookup"><span data-stu-id="c97d2-167">Bindings are used to connect to data in your function.</span></span> <span data-ttu-id="c97d2-168">In questo esempio è stata usata un'associazione di input per connettere le immagini degli utenti da elaborare con la funzione come anteprime.</span><span class="sxs-lookup"><span data-stu-id="c97d2-168">In this example, we used an input binding to connect user images to be processed by our function as thumbnails.</span></span>
