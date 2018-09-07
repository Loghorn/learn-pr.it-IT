<span data-ttu-id="7f144-101">Ora che tutti i requisiti sono soddisfatti, è possibile scrivere codice per creare una nuova coda di archiviazione e aggiungere un messaggio.</span><span class="sxs-lookup"><span data-stu-id="7f144-101">Now that all the requirements are in place, you can write code that creates a new storage queue and adds a message.</span></span> <span data-ttu-id="7f144-102">In genere, questo codice viene inserito nelle app front-end che generano i dati.</span><span class="sxs-lookup"><span data-stu-id="7f144-102">We would typically place this code in our front-end apps that generate the data.</span></span>

## <a name="add-the-client-library-for-azure-storage"></a><span data-ttu-id="7f144-103">Aggiungere la libreria client di Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="7f144-103">Add the client library for Azure Storage</span></span>

<span data-ttu-id="7f144-104">Per iniziare, aggiungere la **libreria client Archiviazione di Azure per .NET** all'app.</span><span class="sxs-lookup"><span data-stu-id="7f144-104">Let's start by adding the **Azure Storage Client Library for .NET** to our app.</span></span> <span data-ttu-id="7f144-105">È possibile installarla con NuGet (uno strumento di gestione pacchetti .NET).</span><span class="sxs-lookup"><span data-stu-id="7f144-105">It can be installed with NuGet (a .NET package manager).</span></span> 

> [!NOTE]
> <span data-ttu-id="7f144-106">Anche se è stata creata una nuova Cloud Shell, i file sono ancora presenti.</span><span class="sxs-lookup"><span data-stu-id="7f144-106">Even though a new Cloud Shell was created, your files are all still present.</span></span> <span data-ttu-id="7f144-107">Tuttavia è necessario passare alla cartella `QueueApp` poiché ora si trova nella radice dell'area di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7f144-107">However you will need to switch into the `QueueApp` folder since you will now be at the root of your storage area.</span></span> <span data-ttu-id="7f144-108">È sufficiente digitare `cd QueueApp` per passare da una cartella all'altra.</span><span class="sxs-lookup"><span data-stu-id="7f144-108">Just type `cd QueueApp` to switch folders.</span></span> <span data-ttu-id="7f144-109">Assicurarsi di eseguire questa operazione prima di aggiungere il pacchetto, poiché l'app .NET si trova in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="7f144-109">Make sure to do this before attempting to add the package since the .NET app is in that folder.</span></span>

1. <span data-ttu-id="7f144-110">Modificare la directory corrente alla cartella `QueueApp`.</span><span class="sxs-lookup"><span data-stu-id="7f144-110">Change the current directory to the `QueueApp` folder.</span></span>

2. <span data-ttu-id="7f144-111">Installare il pacchetto `WindowsAzure.Storage` sul progetto con il comando `dotnet add package`.</span><span class="sxs-lookup"><span data-stu-id="7f144-111">Install the `WindowsAzure.Storage` package to the project with the `dotnet add package` command.</span></span>

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a><span data-ttu-id="7f144-112">Aggiungere un metodo per inviare un avviso sulle notizie</span><span class="sxs-lookup"><span data-stu-id="7f144-112">Add a method to send a news alert</span></span>

<span data-ttu-id="7f144-113">È possibile creare un nuovo metodo per l'invio di una storia di notizie in una coda.</span><span class="sxs-lookup"><span data-stu-id="7f144-113">Next, let's create a new method to send a news story into a queue.</span></span>

1. <span data-ttu-id="7f144-114">Aprire l'editor di codice digitando `code .`</span><span class="sxs-lookup"><span data-stu-id="7f144-114">Open the code editor by typing `code .`</span></span>

2. <span data-ttu-id="7f144-115">Aprire il file `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="7f144-115">Open the `Program.cs` file.</span></span>

3. <span data-ttu-id="7f144-116">Aggiungere gli spazi dei nomi seguenti all'inizio del file.</span><span class="sxs-lookup"><span data-stu-id="7f144-116">At the top of the file, add the following namespaces.</span></span> <span data-ttu-id="7f144-117">Verranno usati tipi di entrambi gli spazi per connettersi ad Archiviazione di Azure e quindi lavorare con le code.</span><span class="sxs-lookup"><span data-stu-id="7f144-117">We'll be using types from both of these to connect to Azure Storage and then to work with queues.</span></span>
    - <span data-ttu-id="7f144-118">Microsoft.WindowsAzure.Storage</span><span class="sxs-lookup"><span data-stu-id="7f144-118">Microsoft.WindowsAzure.Storage</span></span>
    - <span data-ttu-id="7f144-119">Microsoft.WindowsAzure.Storage.Queue</span><span class="sxs-lookup"><span data-stu-id="7f144-119">Microsoft.WindowsAzure.Storage.Queue</span></span>


3. <span data-ttu-id="7f144-120">Creare un metodo statico nella classe `Program` denominato `SendArticleAsync` che accetta un `string` e restituisce `Task`.</span><span class="sxs-lookup"><span data-stu-id="7f144-120">Create a static method in the `Program` class named `SendArticleAsync` that takes a `string` and returns a `Task`.</span></span> <span data-ttu-id="7f144-121">Questo metodo verrà usato per inviare un articolo al servizio.</span><span class="sxs-lookup"><span data-stu-id="7f144-121">We'll use this method to send a news article in to our service.</span></span> <span data-ttu-id="7f144-122">Assegnare un nome al parametro di input `newsMesasage` come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7f144-122">Name the input parameter `newsMesasage` as shown below.</span></span>

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
    
4. <span data-ttu-id="7f144-123">Nel nuovo metodo, usare il metodo statico `CloudStorageAccount.Parse` per analizzare la stringa di connessione (tenere presente che è stata inserita in una stringa costante).</span><span class="sxs-lookup"><span data-stu-id="7f144-123">In your new method, use the static `CloudStorageAccount.Parse` method to parse your connection string (recall we placed it into a constant string).</span></span> <span data-ttu-id="7f144-124">Questo metodo restituisce un oggetto `CloudStorageAccount` che deve essere archiviato in una variabile.</span><span class="sxs-lookup"><span data-stu-id="7f144-124">This method returns a `CloudStorageAccount` object that needs to be stored in a variable.</span></span>

5. <span data-ttu-id="7f144-125">Chiamare il metodo `CreateCloudQueueClient()` nell'oggetto account di archiviazione per ottenere un oggetto client e archiviarlo in una variabile.</span><span class="sxs-lookup"><span data-stu-id="7f144-125">Call the `CreateCloudQueueClient()` method on the storage account object to get a client object and store that in a variable.</span></span>

6. <span data-ttu-id="7f144-126">Successivamente, chiamare il metodo `GetQueueReference` sull'oggetto client e passare la stringa `"newsqueue"` per il nome della coda.</span><span class="sxs-lookup"><span data-stu-id="7f144-126">Next, call `GetQueueReference` method on the client object and pass the string `"newsqueue"` for the queue name.</span></span> <span data-ttu-id="7f144-127">In questo modo verrà restituito un oggetto `CloudQueue` che è possibile usare per lavorare con la coda.</span><span class="sxs-lookup"><span data-stu-id="7f144-127">This returns a `CloudQueue` object that we can use to work with the queue.</span></span> <span data-ttu-id="7f144-128">Non importa se la coda non esiste ancora.</span><span class="sxs-lookup"><span data-stu-id="7f144-128">It's OK if the queue does not exist yet.</span></span>

7. <span data-ttu-id="7f144-129">Chiamare `CreateIfNotExistsAsync()` sull'oggetto `CloudQueue` per verificare che la coda sia pronta per l'uso.</span><span class="sxs-lookup"><span data-stu-id="7f144-129">Call `CreateIfNotExistsAsync()` on the `CloudQueue` object to ensure the queue is ready for use.</span></span> <span data-ttu-id="7f144-130">Verrà creata la coda se necessario.</span><span class="sxs-lookup"><span data-stu-id="7f144-130">This will create the queue if necessary.</span></span>
    - <span data-ttu-id="7f144-131">Poiché si tratta di un metodo asincrono, usare la parola chiave `await` di C# per verificarne il corretto funzionamento.</span><span class="sxs-lookup"><span data-stu-id="7f144-131">Since this is an asynchronous method, use the C# `await` keyword to ensure we work properly with it.</span></span> <span data-ttu-id="7f144-132">Inoltre, sarà necessario decorare il _metodo_ con la parola chiave `async`.</span><span class="sxs-lookup"><span data-stu-id="7f144-132">That also means you need to decorate the _method_ with the `async` keyword.</span></span> 
    - <span data-ttu-id="7f144-133">`CreateIfNotExistsAsync` restituisce il valore `bool` che diventerà `true` se la coda è stata creata, oppure `false` se esiste già.</span><span class="sxs-lookup"><span data-stu-id="7f144-133">`CreateIfNotExistsAsync` returns a `bool` value that will be `true` if the queue was created and `false` if it already exists.</span></span> <span data-ttu-id="7f144-134">Inviare un messaggio alla console se la coda è stata creata.</span><span class="sxs-lookup"><span data-stu-id="7f144-134">Output a message to the console if we created the queue.</span></span>
    - <span data-ttu-id="7f144-135">L'esempio seguente fornisce supporto.</span><span class="sxs-lookup"><span data-stu-id="7f144-135">Here's an example if you need some help.</span></span>

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

8. <span data-ttu-id="7f144-136">Creare un'istanza di un `CloudQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="7f144-136">Create an instance of a `CloudQueueMessage`.</span></span> 
    - <span data-ttu-id="7f144-137">Richiede un parametro `string`, quindi passare il parametro del metodo (`newsMessage`).</span><span class="sxs-lookup"><span data-stu-id="7f144-137">It takes a `string` parameter, pass in the method parameter (`newsMessage`).</span></span> <span data-ttu-id="7f144-138">Questo sarà il _corpo_ del messaggio.</span><span class="sxs-lookup"><span data-stu-id="7f144-138">This will be the _body_ of the message.</span></span> <span data-ttu-id="7f144-139">È inoltre disponibile un metodo statico denominato che può creare un payload del messaggio binario.</span><span class="sxs-lookup"><span data-stu-id="7f144-139">There is also a static method named  that can create a binary message payload.</span></span>
    

9. <span data-ttu-id="7f144-140">Chiamare `AddMessageAsync` sull'oggetto `CloudQueue` per aggiungere il messaggio alla coda.</span><span class="sxs-lookup"><span data-stu-id="7f144-140">Call `AddMessageAsync` on the `CloudQueue` object to add the message to the queue.</span></span> <span data-ttu-id="7f144-141">Questo è anche un metodo asincrono e sarà necessario usare la parola chiave `await` per verificarne la corretta interazione con l'utente.</span><span class="sxs-lookup"><span data-stu-id="7f144-141">This is also an asynchronous method and you will need to use the `await` keyword to ensure we properly interact with it.</span></span>

10. <span data-ttu-id="7f144-142">Salvare il file e compilarlo digitando `dotnet build` nella finestra di comando.</span><span class="sxs-lookup"><span data-stu-id="7f144-142">Save the file and build it by typing `dotnet build` into the command window.</span></span> <span data-ttu-id="7f144-143">Risolvere eventuali errori. È possibile usare il codice seguente per controllare il lavoro.</span><span class="sxs-lookup"><span data-stu-id="7f144-143">Fix any errors, you can use the following code to check your work.</span></span>

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

## <a name="add-code-to-send-a-message"></a><span data-ttu-id="7f144-144">Aggiungere codice per inviare un messaggio</span><span class="sxs-lookup"><span data-stu-id="7f144-144">Add code to send a message</span></span>

<span data-ttu-id="7f144-145">Si modificherà il metodo `Main` per passare i parametri ricevuti nel nuovo metodo, in modo che sia possibile testare il nuovo metodo di invio.</span><span class="sxs-lookup"><span data-stu-id="7f144-145">Let's modify the `Main` method to pass any parameters received into our new method so we can test our new send method.</span></span>

1. <span data-ttu-id="7f144-146">Individuare il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="7f144-146">Locate the `Main` method.</span></span>

2. <span data-ttu-id="7f144-147">Controllare il parametro passato `args` per verificare se alcuni dati sono stati passati alla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="7f144-147">Check the passed `args` parameter to see if any data was passed to the command line.</span></span> <span data-ttu-id="7f144-148">In caso affermativo, usare `String.Join` per creare una singola stringa con tutte le parole usando uno spazio come separatore.</span><span class="sxs-lookup"><span data-stu-id="7f144-148">If so, use `String.Join` to create a single string from all the words using a space as the separator.</span></span>

3. <span data-ttu-id="7f144-149">Passare al nuovo metodo `SendArticleAsync`.</span><span class="sxs-lookup"><span data-stu-id="7f144-149">Pass that to the new `SendArticleAsync` method.</span></span> 

4. <span data-ttu-id="7f144-150">Una volta che viene restituito, usare `Console.WriteLine` per inviare il messaggio inviato.</span><span class="sxs-lookup"><span data-stu-id="7f144-150">Once it returns, use `Console.WriteLine` to output the message we sent.</span></span>

5. <span data-ttu-id="7f144-151">Salvare il file e compilare il programma (`dotnet build`). È possibile usare il codice seguente per controllare il lavoro.</span><span class="sxs-lookup"><span data-stu-id="7f144-151">Save the file and build the program (`dotnet build`), you can use the code below to check your work.</span></span>

> [!NOTE]
> <span data-ttu-id="7f144-152">Poiché il metodo è tecnicamente asincrono, in genere è preferibile usare la parola chiave `await`, tuttavia questa funzionalità non è disponibile nel metodo `Main` a meno che non si usi C# 7.2 (si tratta di una nuova funzionalità).</span><span class="sxs-lookup"><span data-stu-id="7f144-152">Since our method is technically asynchronous, we normally would want to use the `await` keyword, however that feature isn't available on your `Main` method unless you are using C# 7.2 (it's a new feature).</span></span> <span data-ttu-id="7f144-153">Per assicurarsi di poterla usare nelle versioni precedenti del linguaggio, è sufficiente chiamare `Wait()` sul metodo per evitare di attendere la restituzione del metodo.</span><span class="sxs-lookup"><span data-stu-id="7f144-153">To ensure we can use this on older versions of the language, just call `Wait()` on the method to actually block waiting for the method to return.</span></span>

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

## <a name="execute-the-application"></a><span data-ttu-id="7f144-154">Eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="7f144-154">Execute the application</span></span>

<span data-ttu-id="7f144-155">L'applicazione può ora inviare messaggi.</span><span class="sxs-lookup"><span data-stu-id="7f144-155">The application can now send messages.</span></span> <span data-ttu-id="7f144-156">Per testarla, è possibile eseguirla dalla riga di comando con il comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="7f144-156">To test it, you can run it from the command line with the `dotnet run` command.</span></span> <span data-ttu-id="7f144-157">Eventuali stringhe aggiuntive vengono passate all'applicazione come parametri.</span><span class="sxs-lookup"><span data-stu-id="7f144-157">Any additional strings are passed as parameters to the application.</span></span> <span data-ttu-id="7f144-158">Ad esempio, è possibile digitare:</span><span class="sxs-lookup"><span data-stu-id="7f144-158">As an example, you can type:</span></span>

```azurecli
dotnet run Send this message
```

> [!WARNING]
> <span data-ttu-id="7f144-159">Assicurarsi di aver salvato tutti i file nell'editor online prima di compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="7f144-159">Make sure you have saved all the files in the online editor before you build and run the program.</span></span>

<span data-ttu-id="7f144-160">In questo modo, la stringa `"Send this message"` dovrebbe venire aggiunta alla coda.</span><span class="sxs-lookup"><span data-stu-id="7f144-160">This should add the string `"Send this message"` into the queue.</span></span>

## <a name="check-your-results"></a><span data-ttu-id="7f144-161">Controllare i risultati</span><span class="sxs-lookup"><span data-stu-id="7f144-161">Check your results</span></span>

<span data-ttu-id="7f144-162">È possibile controllare le code nel portale di Azure usando **Storage Explorer**. Se si apre questo prodotto, sarà possibile esplorare i dati in tutti gli account di archiviazione di cui si è proprietari.</span><span class="sxs-lookup"><span data-stu-id="7f144-162">You can check queues in the Azure portal using the **Storage Explorer**, if you open that product it will let you explore the data in each storage account you own.</span></span>

<span data-ttu-id="7f144-163">In alternativa, è possibile usare l'interfaccia della riga di comando di Azure o PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7f144-163">Alternatively, you can use the Azure CLI or PowerShell.</span></span> <span data-ttu-id="7f144-164">Provare a eseguire questo comando nella shell, sostituendo il valore `<connection-string>` con la stringa di connessione specifica:</span><span class="sxs-lookup"><span data-stu-id="7f144-164">Try this command in the shell, replacing the `<connection-string>` value with your specific connection string:</span></span>

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="7f144-165">Ciò dovrebbe avviare il dump delle informazioni per il messaggio, che avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7f144-165">This should dump the information for your message, which will look something like this:</span></span>

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

<span data-ttu-id="7f144-166">Sono disponibili numerosi altri comandi che è possibile provare con gli strumenti; eseguire il checkout di `az storage queue --help` e `az storage message --help` per esplorarli.</span><span class="sxs-lookup"><span data-stu-id="7f144-166">There are several other commands available that you can try with the tools - check out both `az storage queue --help` and `az storage message --help` to explore them.</span></span>

> [!NOTE]
> <span data-ttu-id="7f144-167">Inserire il valore della stringa di connessione in una variabile di ambiente denominata `AZURE_STORAGE_CONNECTION_STRING` per non dover digitare ogni volta il parametro `--connection-string`.</span><span class="sxs-lookup"><span data-stu-id="7f144-167">Put your connection string value into an environment variable named `AZURE_STORAGE_CONNECTION_STRING` to save yourself from having to type the `--connection-string` parameter every time!</span></span>

<span data-ttu-id="7f144-168">Ora che il messaggio è stato inviato, l'ultimo passaggio consiste nell'aggiungere il supporto per la _ricezione_ del messaggio.</span><span class="sxs-lookup"><span data-stu-id="7f144-168">Now that we have sent a message, the last step is to add support to _receive_ the message.</span></span>