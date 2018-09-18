<span data-ttu-id="8320e-101">Ora che tutti i requisiti sono soddisfatti, è possibile scrivere codice per creare una nuova coda di archiviazione e aggiungere un messaggio.</span><span class="sxs-lookup"><span data-stu-id="8320e-101">Now that all the requirements are in place, you can write code that creates a new storage queue and adds a message.</span></span> <span data-ttu-id="8320e-102">In genere, questo codice viene inserito nelle app front-end che generano i dati.</span><span class="sxs-lookup"><span data-stu-id="8320e-102">We would typically place this code in our front-end apps that generate the data.</span></span>

## <a name="add-the-client-library-for-azure-storage"></a><span data-ttu-id="8320e-103">Aggiungere la libreria client di Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="8320e-103">Add the client library for Azure Storage</span></span>

<span data-ttu-id="8320e-104">Per iniziare, aggiungere la **libreria client Archiviazione di Azure per .NET** all'app.</span><span class="sxs-lookup"><span data-stu-id="8320e-104">Let's start by adding the **Azure Storage Client Library for .NET** to our app.</span></span> <span data-ttu-id="8320e-105">È possibile installarla con NuGet (uno strumento di gestione pacchetti .NET).</span><span class="sxs-lookup"><span data-stu-id="8320e-105">It can be installed with NuGet (a .NET package manager).</span></span> 

> [!NOTE]
> <span data-ttu-id="8320e-106">Anche se è stata creata una nuova istanza di Cloud Shell, i file sono ancora presenti.</span><span class="sxs-lookup"><span data-stu-id="8320e-106">Even though a new Cloud Shell was created, your files are all still present.</span></span> <span data-ttu-id="8320e-107">Tuttavia, è necessario passare alla cartella `QueueApp` perché ora ci si trova nella radice dell'area di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="8320e-107">However, you will need to switch into the `QueueApp` folder since you will now be at the root of your storage area.</span></span> <span data-ttu-id="8320e-108">È sufficiente digitare `cd QueueApp` per passare da una cartella all'altra.</span><span class="sxs-lookup"><span data-stu-id="8320e-108">Just type `cd QueueApp` to switch folders.</span></span> <span data-ttu-id="8320e-109">Assicurarsi di eseguire questa operazione prima di aggiungere il pacchetto, poiché l'app .NET si trova in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="8320e-109">Make sure to do this before attempting to add the package since the .NET app is in that folder.</span></span>

1. <span data-ttu-id="8320e-110">Modificare la directory corrente alla cartella `QueueApp`.</span><span class="sxs-lookup"><span data-stu-id="8320e-110">Change the current directory to the `QueueApp` folder.</span></span>

1. <span data-ttu-id="8320e-111">Installare il pacchetto `WindowsAzure.Storage` sul progetto con il comando `dotnet add package`.</span><span class="sxs-lookup"><span data-stu-id="8320e-111">Install the `WindowsAzure.Storage` package to the project with the `dotnet add package` command.</span></span>

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a><span data-ttu-id="8320e-112">Aggiungere un metodo per inviare un avviso sulle notizie</span><span class="sxs-lookup"><span data-stu-id="8320e-112">Add a method to send a news alert</span></span>

<span data-ttu-id="8320e-113">È possibile creare un nuovo metodo per l'invio di una storia di notizie in una coda.</span><span class="sxs-lookup"><span data-stu-id="8320e-113">Next, let's create a new method to send a news story into a queue.</span></span>

1. <span data-ttu-id="8320e-114">Aprire l'editor di codice digitando `code .`</span><span class="sxs-lookup"><span data-stu-id="8320e-114">Open the code editor by typing `code .`</span></span>

1. <span data-ttu-id="8320e-115">Aprire il file `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="8320e-115">Open the `Program.cs` file.</span></span>

1. <span data-ttu-id="8320e-116">Aggiungere gli spazi dei nomi seguenti all'inizio del file.</span><span class="sxs-lookup"><span data-stu-id="8320e-116">At the top of the file, add the following namespaces.</span></span> <span data-ttu-id="8320e-117">Verranno usati tipi di entrambi gli spazi per connettersi ad Archiviazione di Azure e quindi usare le code.</span><span class="sxs-lookup"><span data-stu-id="8320e-117">We'll be using types from both of these to connect to Azure Storage and then to work with queues.</span></span>

    - <span data-ttu-id="8320e-118">System.threading.task</span><span class="sxs-lookup"><span data-stu-id="8320e-118">System.threading.task</span></span>
    - <span data-ttu-id="8320e-119">Microsoft.WindowsAzure.Storage</span><span class="sxs-lookup"><span data-stu-id="8320e-119">Microsoft.WindowsAzure.Storage</span></span>
    - <span data-ttu-id="8320e-120">Microsoft.WindowsAzure.Storage.Queue</span><span class="sxs-lookup"><span data-stu-id="8320e-120">Microsoft.WindowsAzure.Storage.Queue</span></span>

1. <span data-ttu-id="8320e-121">Creare un metodo statico nella classe `Program` denominato `SendArticleAsync` che accetta un `string` e restituisce `Task`.</span><span class="sxs-lookup"><span data-stu-id="8320e-121">Create a static method in the `Program` class named `SendArticleAsync` that takes a `string` and returns a `Task`.</span></span> <span data-ttu-id="8320e-122">Questo metodo verrà usato per inviare un articolo al servizio.</span><span class="sxs-lookup"><span data-stu-id="8320e-122">We'll use this method to send a news article in to our service.</span></span> <span data-ttu-id="8320e-123">Assegnare un nome al parametro di input `newsMesasage` come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="8320e-123">Name the input parameter `newsMesasage` as shown below.</span></span>

    ```csharp
    ...
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue; 
    
    class Program
    {
        ...
    
        static async Task SendArticleAsync(string newsMessage)
        {
        }
    }
    ```
    
1. <span data-ttu-id="8320e-124">Nel nuovo metodo, usare il metodo statico `CloudStorageAccount.Parse` per analizzare la stringa di connessione (tenere presente che è stata inserita in una stringa costante).</span><span class="sxs-lookup"><span data-stu-id="8320e-124">In your new method, use the static `CloudStorageAccount.Parse` method to parse your connection string (recall we placed it into a constant string).</span></span> <span data-ttu-id="8320e-125">Questo metodo restituisce un oggetto `CloudStorageAccount` che deve essere archiviato in una variabile.</span><span class="sxs-lookup"><span data-stu-id="8320e-125">This method returns a `CloudStorageAccount` object that needs to be stored in a variable.</span></span>

1. <span data-ttu-id="8320e-126">Chiamare il metodo `CreateCloudQueueClient()` nell'oggetto account di archiviazione per ottenere un oggetto client e archiviarlo in una variabile.</span><span class="sxs-lookup"><span data-stu-id="8320e-126">Call the `CreateCloudQueueClient()` method on the storage account object to get a client object and store that in a variable.</span></span>

1. <span data-ttu-id="8320e-127">Successivamente, chiamare il metodo `GetQueueReference` sull'oggetto client e passare la stringa `"newsqueue"` per il nome della coda.</span><span class="sxs-lookup"><span data-stu-id="8320e-127">Next, call `GetQueueReference` method on the client object and pass the string `"newsqueue"` for the queue name.</span></span> <span data-ttu-id="8320e-128">In questo modo verrà restituito un oggetto `CloudQueue` che è possibile usare per lavorare con la coda.</span><span class="sxs-lookup"><span data-stu-id="8320e-128">This returns a `CloudQueue` object that we can use to work with the queue.</span></span> <span data-ttu-id="8320e-129">Non importa se la coda non esiste ancora.</span><span class="sxs-lookup"><span data-stu-id="8320e-129">It's OK if the queue does not exist yet.</span></span>

1. <span data-ttu-id="8320e-130">Chiamare `CreateIfNotExistsAsync()` sull'oggetto `CloudQueue` per verificare che la coda sia pronta per l'uso.</span><span class="sxs-lookup"><span data-stu-id="8320e-130">Call `CreateIfNotExistsAsync()` on the `CloudQueue` object to ensure the queue is ready for use.</span></span> <span data-ttu-id="8320e-131">Verrà creata la coda se necessario.</span><span class="sxs-lookup"><span data-stu-id="8320e-131">This will create the queue if necessary.</span></span>
    - <span data-ttu-id="8320e-132">Poiché si tratta di un metodo asincrono, usare la parola chiave `await` di C# per verificarne il corretto funzionamento.</span><span class="sxs-lookup"><span data-stu-id="8320e-132">Since this is an asynchronous method, use the C# `await` keyword to ensure we work properly with it.</span></span> <span data-ttu-id="8320e-133">Inoltre, sarà necessario decorare il _metodo_ con la parola chiave `async`.</span><span class="sxs-lookup"><span data-stu-id="8320e-133">That also means you need to decorate the _method_ with the `async` keyword.</span></span> 
    - <span data-ttu-id="8320e-134">`CreateIfNotExistsAsync` restituisce il valore `bool` che diventerà `true` se la coda è stata creata, oppure `false` se esiste già.</span><span class="sxs-lookup"><span data-stu-id="8320e-134">`CreateIfNotExistsAsync` returns a `bool` value that will be `true` if the queue was created and `false` if it already exists.</span></span> <span data-ttu-id="8320e-135">Inviare un messaggio alla console se la coda è stata creata.</span><span class="sxs-lookup"><span data-stu-id="8320e-135">Output a message to the console if we created the queue.</span></span>
    - <span data-ttu-id="8320e-136">L'esempio seguente fornisce supporto.</span><span class="sxs-lookup"><span data-stu-id="8320e-136">Here's an example if you need some help.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. <span data-ttu-id="8320e-137">Creare un'istanza di un `CloudQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="8320e-137">Create an instance of a `CloudQueueMessage`.</span></span> 
    - <span data-ttu-id="8320e-138">Richiede un parametro `string`, quindi passare il parametro del metodo (`newsMessage`).</span><span class="sxs-lookup"><span data-stu-id="8320e-138">It takes a `string` parameter, pass in the method parameter (`newsMessage`).</span></span> <span data-ttu-id="8320e-139">Questo sarà il _corpo_ del messaggio.</span><span class="sxs-lookup"><span data-stu-id="8320e-139">This will be the _body_ of the message.</span></span> <span data-ttu-id="8320e-140">È inoltre disponibile un metodo statico denominato che può creare un payload del messaggio binario.</span><span class="sxs-lookup"><span data-stu-id="8320e-140">There is also a static method named  that can create a binary message payload.</span></span>
    

1. <span data-ttu-id="8320e-141">Chiamare `AddMessageAsync` sull'oggetto `CloudQueue` per aggiungere il messaggio alla coda.</span><span class="sxs-lookup"><span data-stu-id="8320e-141">Call `AddMessageAsync` on the `CloudQueue` object to add the message to the queue.</span></span> <span data-ttu-id="8320e-142">Questo è anche un metodo asincrono e sarà necessario usare la parola chiave `await` per verificarne la corretta interazione con l'utente.</span><span class="sxs-lookup"><span data-stu-id="8320e-142">This is also an asynchronous method and you will need to use the `await` keyword to ensure we properly interact with it.</span></span>

1. <span data-ttu-id="8320e-143">Salvare il file e compilarlo digitando `dotnet build` nella finestra di comando.</span><span class="sxs-lookup"><span data-stu-id="8320e-143">Save the file and build it by typing `dotnet build` into the command window.</span></span> <span data-ttu-id="8320e-144">Risolvere eventuali errori. È possibile usare il codice seguente per controllare il lavoro.</span><span class="sxs-lookup"><span data-stu-id="8320e-144">Fix any errors, you can use the following code to check your work.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference(QueueName);
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a><span data-ttu-id="8320e-145">Aggiungere codice per inviare un messaggio</span><span class="sxs-lookup"><span data-stu-id="8320e-145">Add code to send a message</span></span>

<span data-ttu-id="8320e-146">Si modificherà il metodo `Main` per passare i parametri ricevuti nel nuovo metodo, in modo che sia possibile testare il nuovo metodo di invio.</span><span class="sxs-lookup"><span data-stu-id="8320e-146">Let's modify the `Main` method to pass any parameters received into our new method so we can test our new send method.</span></span>

1. <span data-ttu-id="8320e-147">Individuare il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="8320e-147">Locate the `Main` method.</span></span>

1. <span data-ttu-id="8320e-148">Controllare il parametro passato `args` per verificare se alcuni dati sono stati passati alla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="8320e-148">Check the passed `args` parameter to see if any data was passed to the command line.</span></span> <span data-ttu-id="8320e-149">In caso affermativo, usare `String.Join` per creare una singola stringa con tutte le parole usando uno spazio come separatore.</span><span class="sxs-lookup"><span data-stu-id="8320e-149">If so, use `String.Join` to create a single string from all the words using a space as the separator.</span></span>

1. <span data-ttu-id="8320e-150">Passare al nuovo metodo `SendArticleAsync`.</span><span class="sxs-lookup"><span data-stu-id="8320e-150">Pass that to the new `SendArticleAsync` method.</span></span> 

1. <span data-ttu-id="8320e-151">Una volta che viene restituito, usare `Console.WriteLine` per inviare il messaggio inviato.</span><span class="sxs-lookup"><span data-stu-id="8320e-151">Once it returns, use `Console.WriteLine` to output the message we sent.</span></span>

1. <span data-ttu-id="8320e-152">Salvare il file e compilare il programma (`dotnet build`). È possibile usare il codice seguente per controllare il lavoro.</span><span class="sxs-lookup"><span data-stu-id="8320e-152">Save the file and build the program (`dotnet build`), you can use the code below to check your work.</span></span>

> [!NOTE]
> <span data-ttu-id="8320e-153">Poiché il metodo è tecnicamente asincrono, in genere è preferibile usare la parola chiave `await`, tuttavia questa funzionalità non è disponibile nel metodo `Main` a meno che non si usi C# 7.2 (si tratta di una nuova funzionalità).</span><span class="sxs-lookup"><span data-stu-id="8320e-153">Since our method is technically asynchronous, we normally would want to use the `await` keyword, however that feature isn't available on your `Main` method unless you are using C# 7.2 (it's a new feature).</span></span> <span data-ttu-id="8320e-154">Per assicurarsi di poterla usare nelle versioni precedenti del linguaggio, è sufficiente chiamare `Wait()` sul metodo per evitare di attendere la restituzione del metodo.</span><span class="sxs-lookup"><span data-stu-id="8320e-154">To ensure we can use this on older versions of the language, just call `Wait()` on the method to actually block waiting for the method to return.</span></span>

    ```csharp
    static void Main(string[] args)
    {
        if (args.Length > 0)
        {
            string value = String.Join(" ", args);
            SendArticleAsync(value).Wait();
            Console.WriteLine($"Sent: {value}");
        }
    }
    ```

## <a name="execute-the-application"></a><span data-ttu-id="8320e-155">Eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="8320e-155">Execute the application</span></span>

<span data-ttu-id="8320e-156">L'applicazione può ora inviare messaggi.</span><span class="sxs-lookup"><span data-stu-id="8320e-156">The application can now send messages.</span></span> <span data-ttu-id="8320e-157">Per testarla, è possibile eseguirla dalla riga di comando con il comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="8320e-157">To test it, you can run it from the command line with the `dotnet run` command.</span></span> <span data-ttu-id="8320e-158">Eventuali stringhe aggiuntive vengono passate all'applicazione come parametri.</span><span class="sxs-lookup"><span data-stu-id="8320e-158">Any additional strings are passed as parameters to the application.</span></span> <span data-ttu-id="8320e-159">Ad esempio, è possibile digitare:</span><span class="sxs-lookup"><span data-stu-id="8320e-159">As an example, you can type:</span></span>

    ```azurecli
    dotnet run Send this message
    ```

> [!WARNING]
> <span data-ttu-id="8320e-160">Assicurarsi di aver salvato tutti i file nell'editor online prima di compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="8320e-160">Make sure you have saved all the files in the online editor before you build and run the program.</span></span>

<span data-ttu-id="8320e-161">In questo modo, la stringa `"Send this message"` dovrebbe venire aggiunta alla coda.</span><span class="sxs-lookup"><span data-stu-id="8320e-161">This should add the string `"Send this message"` into the queue.</span></span>

## <a name="check-your-results"></a><span data-ttu-id="8320e-162">Controllare i risultati</span><span class="sxs-lookup"><span data-stu-id="8320e-162">Check your results</span></span>

<span data-ttu-id="8320e-163">È possibile controllare le code nel portale di Azure usando **Storage Explorer**. Se si apre questo prodotto, sarà possibile esplorare i dati in tutti gli account di archiviazione di cui si è proprietari.</span><span class="sxs-lookup"><span data-stu-id="8320e-163">You can check queues in the Azure portal using the **Storage Explorer**, if you open that product it will let you explore the data in each storage account you own.</span></span>

<span data-ttu-id="8320e-164">In alternativa, è possibile usare l'interfaccia della riga di comando di Azure o PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8320e-164">Alternatively, you can use the Azure CLI or PowerShell.</span></span> <span data-ttu-id="8320e-165">Provare a eseguire questo comando nella shell, sostituendo il valore `<connection-string>` con la stringa di connessione specifica:</span><span class="sxs-lookup"><span data-stu-id="8320e-165">Try this command in the shell, replacing the `<connection-string>` value with your specific connection string:</span></span>

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="8320e-166">Ciò dovrebbe avviare il dump delle informazioni per il messaggio, che avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8320e-166">This should dump the information for your message, which will look something like this:</span></span>

```json
[
  {
    "content": "U2VuZCB0aGlzIG1lc3NhZ2U=",
    "dequeueCount": 0,
    "expirationTime": "2018-09-05T02:43:40+00:00",
    "id": "b47cbe9f-a246-4a81-86ae-fa02ea8d98bc",
    "insertionTime": "2018-09-24T02:43:40+00:00",
    "popReceipt": null,
    "timeNextVisible": null
  }
]
```

<span data-ttu-id="8320e-167">Sono disponibili numerosi altri comandi che è possibile provare con gli strumenti; eseguire il checkout di `az storage queue --help` e `az storage message --help` per esplorarli.</span><span class="sxs-lookup"><span data-stu-id="8320e-167">There are several other commands available that you can try with the tools - check out both `az storage queue --help` and `az storage message --help` to explore them.</span></span>

> [!TIP]
> <span data-ttu-id="8320e-168">Inserire il valore della stringa di connessione in una variabile di ambiente denominata `AZURE_STORAGE_CONNECTION_STRING` per non dover digitare ogni volta il parametro `--connection-string`.</span><span class="sxs-lookup"><span data-stu-id="8320e-168">Put your connection string value into an environment variable named `AZURE_STORAGE_CONNECTION_STRING` to save yourself from having to type the `--connection-string` parameter every time!</span></span>

<span data-ttu-id="8320e-169">Ora che il messaggio è stato inviato, l'ultimo passaggio consiste nell'aggiungere il supporto per la _ricezione_ del messaggio.</span><span class="sxs-lookup"><span data-stu-id="8320e-169">Now that we have sent a message, the last step is to add support to _receive_ the message.</span></span>