<span data-ttu-id="fe90b-101">Ora che tutti i requisiti sono soddisfatti, è possibile scrivere codice per creare una nuova coda di archiviazione e aggiungere un messaggio.</span><span class="sxs-lookup"><span data-stu-id="fe90b-101">Now that all the requirements are in place, you can write code that creates a new storage queue and adds a message.</span></span> <span data-ttu-id="fe90b-102">In genere, questo codice viene inserito nelle app front-end che generano i dati.</span><span class="sxs-lookup"><span data-stu-id="fe90b-102">We would typically place this code in our front-end apps that generate the data.</span></span>

## <a name="add-the-client-library-for-azure-storage"></a><span data-ttu-id="fe90b-103">Aggiungere la libreria client di Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="fe90b-103">Add the client library for Azure Storage</span></span>

<span data-ttu-id="fe90b-104">Per iniziare, aggiungere la **libreria client Archiviazione di Azure per .NET** all'app.</span><span class="sxs-lookup"><span data-stu-id="fe90b-104">Let's start by adding the **Azure Storage Client Library for .NET** to our app.</span></span> <span data-ttu-id="fe90b-105">È possibile installarla con NuGet (uno strumento di gestione pacchetti .NET).</span><span class="sxs-lookup"><span data-stu-id="fe90b-105">It can be installed with NuGet (a .NET package manager).</span></span> 

1. <span data-ttu-id="fe90b-106">Installare il pacchetto `WindowsAzure.Storage` nel progetto con il comando `dotnet add package`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-106">Install the `WindowsAzure.Storage` package to the project with the `dotnet add package` command.</span></span> <span data-ttu-id="fe90b-107">È possibile eseguire questa operazione nella finestra del terminale _sotto_ Cloud Shell oppure, se si sta usando il computer locale, in una finestra del terminale o della console nella stessa cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="fe90b-107">You can do this in the terminal window _below_ the Cloud Shell, or if you are working on your local computer, in a terminal/console window in the same folder as the project.</span></span>

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a><span data-ttu-id="fe90b-108">Aggiungere un metodo per inviare un avviso di notizia</span><span class="sxs-lookup"><span data-stu-id="fe90b-108">Add a method to send a news alert</span></span>

<span data-ttu-id="fe90b-109">È possibile creare un nuovo metodo per l'invio di una notizia in una coda.</span><span class="sxs-lookup"><span data-stu-id="fe90b-109">Next, let's create a new method to send a news story into a queue.</span></span>

1. <span data-ttu-id="fe90b-110">Aprire il file `Program.cs` nell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="fe90b-110">Open the `Program.cs` file in your code editor.</span></span>

1. <span data-ttu-id="fe90b-111">Aggiungere gli spazi dei nomi seguenti all'inizio del file.</span><span class="sxs-lookup"><span data-stu-id="fe90b-111">At the top of the file, add the following namespaces.</span></span> <span data-ttu-id="fe90b-112">Verranno usati tipi di entrambi gli spazi per connettersi ad Archiviazione di Azure e quindi usare le code.</span><span class="sxs-lookup"><span data-stu-id="fe90b-112">We'll be using types from both of these to connect to Azure Storage and then to work with queues.</span></span>

    - `System.Threading.Tasks`
    - `Microsoft.WindowsAzure.Storage`
    - `Microsoft.WindowsAzure.Storage.Queue`

1. <span data-ttu-id="fe90b-113">Creare un metodo statico nella classe `Program` denominato `SendArticleAsync` che accetta `string` e restituisce `Task`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-113">Create a static method in the `Program` class named `SendArticleAsync` that takes a `string` and returns a `Task`.</span></span> <span data-ttu-id="fe90b-114">Questo metodo verrà usato per inviare un articolo al servizio.</span><span class="sxs-lookup"><span data-stu-id="fe90b-114">We'll use this method to send a news article in to our service.</span></span> <span data-ttu-id="fe90b-115">Assegnare un nome al parametro di input `newsMessage` come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="fe90b-115">Name the input parameter `newsMessage` as shown below.</span></span>

    ```csharp
    ...
    using System.Threading.Tasks;
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
    
1. <span data-ttu-id="fe90b-116">Nel nuovo metodo, usare il metodo statico `CloudStorageAccount.Parse` per analizzare la stringa di connessione (tenere presente che è stata inserita in una stringa costante).</span><span class="sxs-lookup"><span data-stu-id="fe90b-116">In your new method, use the static `CloudStorageAccount.Parse` method to parse your connection string (recall we placed it into a constant string).</span></span> <span data-ttu-id="fe90b-117">Questo metodo restituisce un oggetto `CloudStorageAccount` che deve essere archiviato in una variabile.</span><span class="sxs-lookup"><span data-stu-id="fe90b-117">This method returns a `CloudStorageAccount` object that needs to be stored in a variable.</span></span>

1. <span data-ttu-id="fe90b-118">Chiamare il metodo `CreateCloudQueueClient()` nell'oggetto account di archiviazione per ottenere un oggetto client e archiviarlo in una variabile.</span><span class="sxs-lookup"><span data-stu-id="fe90b-118">Call the `CreateCloudQueueClient()` method on the storage account object to get a client object and store that in a variable.</span></span>

1. <span data-ttu-id="fe90b-119">Successivamente, chiamare il metodo `GetQueueReference` sull'oggetto client e passare la stringa "newsqueue" per il nome della coda.</span><span class="sxs-lookup"><span data-stu-id="fe90b-119">Next, call `GetQueueReference` method on the client object and pass the string "newsqueue" for the queue name.</span></span> <span data-ttu-id="fe90b-120">In questo modo verrà restituito un oggetto `CloudQueue` che è possibile usare per lavorare con la coda.</span><span class="sxs-lookup"><span data-stu-id="fe90b-120">This returns a `CloudQueue` object that we can use to work with the queue.</span></span> <span data-ttu-id="fe90b-121">Non importa se la coda non esiste ancora.</span><span class="sxs-lookup"><span data-stu-id="fe90b-121">It's OK if the queue does not exist yet.</span></span>

1. <span data-ttu-id="fe90b-122">Chiamare `CreateIfNotExistsAsync()` sull'oggetto `CloudQueue` per verificare che la coda sia pronta per l'uso.</span><span class="sxs-lookup"><span data-stu-id="fe90b-122">Call `CreateIfNotExistsAsync()` on the `CloudQueue` object to ensure the queue is ready for use.</span></span> <span data-ttu-id="fe90b-123">Verrà creata la coda se necessario.</span><span class="sxs-lookup"><span data-stu-id="fe90b-123">This will create the queue if necessary.</span></span>
    - <span data-ttu-id="fe90b-124">Poiché si tratta di un metodo asincrono, usare la parola chiave `await` di C# per verificarne il corretto funzionamento.</span><span class="sxs-lookup"><span data-stu-id="fe90b-124">Since this is an asynchronous method, use the C# `await` keyword to ensure we work properly with it.</span></span> <span data-ttu-id="fe90b-125">Inoltre, sarà necessario decorare il _metodo_ con la parola chiave `async`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-125">That also means you need to decorate the _method_ with the `async` keyword.</span></span> 
    - <span data-ttu-id="fe90b-126">`CreateIfNotExistsAsync` restituisce il valore `bool` che diventerà `true` se la coda è stata creata, oppure `false` se esiste già.</span><span class="sxs-lookup"><span data-stu-id="fe90b-126">`CreateIfNotExistsAsync` returns a `bool` value that will be `true` if the queue was created and `false` if it already exists.</span></span> <span data-ttu-id="fe90b-127">Inviare un messaggio alla console se la coda è stata creata.</span><span class="sxs-lookup"><span data-stu-id="fe90b-127">Output a message to the console if we created the queue.</span></span>
    - <span data-ttu-id="fe90b-128">L'esempio seguente fornisce supporto.</span><span class="sxs-lookup"><span data-stu-id="fe90b-128">Here's an example if you need some help.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        // Not Shown here - code from prior steps
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. <span data-ttu-id="fe90b-129">Creare un'istanza di un `CloudQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-129">Create an instance of a `CloudQueueMessage`.</span></span> 
    - <span data-ttu-id="fe90b-130">Richiede un parametro `string`, quindi passare il parametro del metodo (`newsMessage`).</span><span class="sxs-lookup"><span data-stu-id="fe90b-130">It takes a `string` parameter, pass in the method parameter (`newsMessage`).</span></span> <span data-ttu-id="fe90b-131">Questo sarà il _corpo_ del messaggio.</span><span class="sxs-lookup"><span data-stu-id="fe90b-131">This will be the _body_ of the message.</span></span> <span data-ttu-id="fe90b-132">È inoltre disponibile un metodo statico denominato che può creare un payload del messaggio binario.</span><span class="sxs-lookup"><span data-stu-id="fe90b-132">There is also a static method named  that can create a binary message payload.</span></span>

1. <span data-ttu-id="fe90b-133">Chiamare `AddMessageAsync` sull'oggetto `CloudQueue` per aggiungere il messaggio alla coda.</span><span class="sxs-lookup"><span data-stu-id="fe90b-133">Call `AddMessageAsync` on the `CloudQueue` object to add the message to the queue.</span></span> <span data-ttu-id="fe90b-134">Questo è anche un metodo asincrono e sarà necessario usare la parola chiave `await` per verificarne la corretta interazione con l'utente.</span><span class="sxs-lookup"><span data-stu-id="fe90b-134">This is also an asynchronous method and you will need to use the `await` keyword to ensure we properly interact with it.</span></span>

1. <span data-ttu-id="fe90b-135">Salvare il file e compilarlo digitando `dotnet build` nella finestra di comando.</span><span class="sxs-lookup"><span data-stu-id="fe90b-135">Save the file and build it by typing `dotnet build` into the command window.</span></span> <span data-ttu-id="fe90b-136">Risolvere eventuali errori. È possibile usare il codice seguente per controllare il lavoro.</span><span class="sxs-lookup"><span data-stu-id="fe90b-136">Fix any errors, you can use the following code to check your work.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference("newsqueue");
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a><span data-ttu-id="fe90b-137">Aggiungere codice per inviare un messaggio</span><span class="sxs-lookup"><span data-stu-id="fe90b-137">Add code to send a message</span></span>

<span data-ttu-id="fe90b-138">Si modificherà il metodo `Main` per passare i parametri ricevuti nel nuovo metodo, in modo che sia possibile testare il nuovo metodo di invio.</span><span class="sxs-lookup"><span data-stu-id="fe90b-138">Let's modify the `Main` method to pass any parameters received into our new method so we can test our new send method.</span></span>

1. <span data-ttu-id="fe90b-139">Individuare il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-139">Locate the `Main` method.</span></span>

1. <span data-ttu-id="fe90b-140">Controllare il parametro passato `args` per verificare se alcuni dati sono stati passati alla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fe90b-140">Check the passed `args` parameter to see if any data was passed to the command line.</span></span> <span data-ttu-id="fe90b-141">In caso affermativo, usare `String.Join` per creare una singola stringa con tutte le parole usando uno spazio come separatore.</span><span class="sxs-lookup"><span data-stu-id="fe90b-141">If so, use `String.Join` to create a single string from all the words using a space as the separator.</span></span>

1. <span data-ttu-id="fe90b-142">Passare al nuovo metodo `SendArticleAsync`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-142">Pass that to the new `SendArticleAsync` method.</span></span> 

1. <span data-ttu-id="fe90b-143">Una volta che viene restituito, usare `Console.WriteLine` per inviare il messaggio inviato.</span><span class="sxs-lookup"><span data-stu-id="fe90b-143">Once it returns, use `Console.WriteLine` to output the message we sent.</span></span>

1. <span data-ttu-id="fe90b-144">Salvare il file e compilare il programma (`dotnet build`). È possibile usare il codice seguente per controllare il lavoro.</span><span class="sxs-lookup"><span data-stu-id="fe90b-144">Save the file and build the program (`dotnet build`), you can use the code below to check your work.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fe90b-145">Poiché il metodo è tecnicamente asincrono, è preferibile usare la parola chiave `await`, tuttavia questa funzionalità non è disponibile nel metodo `Main` a meno che non si usi C# 7.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="fe90b-145">Since our method is technically asynchronous, we will want to use the `await` keyword, however that feature isn't available on your `Main` method unless you are using C# 7.1 or later.</span></span> <span data-ttu-id="fe90b-146">Chiamare `Wait()` sul metodo per bloccare l'attesa della restituzione del metodo. Questo problema verrà risolto tra poco.</span><span class="sxs-lookup"><span data-stu-id="fe90b-146">Just call `Wait()` on the method to actually block waiting for the method to return, we'll fix that in a minute.</span></span>

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

## <a name="execute-the-application"></a><span data-ttu-id="fe90b-147">Eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="fe90b-147">Execute the application</span></span>

<span data-ttu-id="fe90b-148">L'applicazione può ora inviare messaggi.</span><span class="sxs-lookup"><span data-stu-id="fe90b-148">The application can now send messages.</span></span> <span data-ttu-id="fe90b-149">Per testarla, è possibile eseguirla dalla riga di comando con il comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-149">To test it, you can run it from the command line with the `dotnet run` command.</span></span> <span data-ttu-id="fe90b-150">Eventuali stringhe aggiuntive vengono passate all'applicazione come parametri.</span><span class="sxs-lookup"><span data-stu-id="fe90b-150">Any additional strings are passed as parameters to the application.</span></span> 

> [!WARNING]
> <span data-ttu-id="fe90b-151">Assicurarsi di aver salvato tutti i file nell'editor online prima di compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="fe90b-151">Make sure you have saved all the files in the online editor before you build and run the program.</span></span>

<span data-ttu-id="fe90b-152">Ad esempio, è possibile digitare:</span><span class="sxs-lookup"><span data-stu-id="fe90b-152">As an example, you can type:</span></span>

```azurecli
dotnet run Send this message
```

<span data-ttu-id="fe90b-153">In questo modo, la stringa `"Send this message"` dovrebbe venire aggiunta alla coda.</span><span class="sxs-lookup"><span data-stu-id="fe90b-153">This should add the string `"Send this message"` into the queue.</span></span>

## <a name="check-your-results"></a><span data-ttu-id="fe90b-154">Controllare i risultati</span><span class="sxs-lookup"><span data-stu-id="fe90b-154">Check your results</span></span>

<span data-ttu-id="fe90b-155">È possibile controllare le code nel portale di Azure usando **Storage Explorer**. Se si apre questo prodotto, sarà possibile esplorare i dati in tutti gli account di archiviazione di cui si è proprietari.</span><span class="sxs-lookup"><span data-stu-id="fe90b-155">You can check queues in the Azure portal using the **Storage Explorer**, if you open that product it will let you explore the data in each storage account you own.</span></span>

<span data-ttu-id="fe90b-156">In alternativa, è possibile usare l'interfaccia della riga di comando di Azure o PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe90b-156">Alternatively, you can use the Azure CLI or PowerShell.</span></span> <span data-ttu-id="fe90b-157">Provare a eseguire questo comando nella shell, sostituendo il valore `<connection-string>` con la stringa di connessione specifica:</span><span class="sxs-lookup"><span data-stu-id="fe90b-157">Try this command in the shell, replacing the `<connection-string>` value with your specific connection string:</span></span>

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="fe90b-158">Ciò dovrebbe avviare il dump delle informazioni per il messaggio, che avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fe90b-158">This should dump the information for your message, which will look something like this:</span></span>

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

<span data-ttu-id="fe90b-159">Sono disponibili numerosi altri comandi che è possibile provare con gli strumenti; eseguire il checkout di `az storage queue --help` e `az storage message --help` per esplorarli.</span><span class="sxs-lookup"><span data-stu-id="fe90b-159">There are several other commands available that you can try with the tools - check out both `az storage queue --help` and `az storage message --help` to explore them.</span></span>

> [!TIP]
> <span data-ttu-id="fe90b-160">Inserire il valore della stringa di connessione in una variabile di ambiente denominata `AZURE_STORAGE_CONNECTION_STRING` per non dover digitare ogni volta il parametro `--connection-string`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-160">Put your connection string value into an environment variable named `AZURE_STORAGE_CONNECTION_STRING` to save yourself from having to type the `--connection-string` parameter every time!</span></span>

## <a name="optional---use-the-async-versions-of-the-methods"></a><span data-ttu-id="fe90b-161">Facoltativo: usare le versioni asincrone dei metodi</span><span class="sxs-lookup"><span data-stu-id="fe90b-161">Optional - Use the async versions of the methods</span></span>

<span data-ttu-id="fe90b-162">Sul metodo send precedente è stato usato `Wait()`, che non corrisponde tuttavia a un uso efficiente delle risorse di calcolo.</span><span class="sxs-lookup"><span data-stu-id="fe90b-162">We used `Wait()` on the send method above but that's not an efficient use of our computing resources.</span></span> <span data-ttu-id="fe90b-163">È preferibile usare invece i metodi `async` e `await` di C#.</span><span class="sxs-lookup"><span data-stu-id="fe90b-163">Instead, we want to use the C# `async` and `await` methods.</span></span> <span data-ttu-id="fe90b-164">Sarà tuttavia necessario usare _almeno_ C# 7.1 per poter applicare queste parole chiave al metodo **Main**.</span><span class="sxs-lookup"><span data-stu-id="fe90b-164">However, we will need to be using _at least_ C# 7.1 to be able to apply these keywords to our **Main** method.</span></span>

### <a name="switch-to-c-71"></a><span data-ttu-id="fe90b-165">Passare a C# 7.1</span><span class="sxs-lookup"><span data-stu-id="fe90b-165">Switch to C# 7.1</span></span>

<span data-ttu-id="fe90b-166">Le parole chiave `async` e `await` di C# non sono parole chiave valide nei metodi **Main** nelle versioni precedenti a C# 7.1.</span><span class="sxs-lookup"><span data-stu-id="fe90b-166">C#'s `async` and `await` keywords were not valid keywords in **Main** methods until C# 7.1.</span></span> <span data-ttu-id="fe90b-167">È possibile passare con facilità a questo compilatore tramite un flag nel file con estensione **csproj**.</span><span class="sxs-lookup"><span data-stu-id="fe90b-167">We can easily switch to that compiler through a flag in the **.csproj** file.</span></span>

1. <span data-ttu-id="fe90b-168">Aprire il file **QueueApp.csproj** nell'editor.</span><span class="sxs-lookup"><span data-stu-id="fe90b-168">Open the **QueueApp.csproj** file in the editor.</span></span>

1. <span data-ttu-id="fe90b-169">Aggiungere `<LangVersion>7.1</LangVersion>` nel primo elemento `PropertyGroup` del file di compilazione.</span><span class="sxs-lookup"><span data-stu-id="fe90b-169">Add `<LangVersion>7.1</LangVersion>` into the first `PropertyGroup` in the build file.</span></span> <span data-ttu-id="fe90b-170">Al termine il risultato dovrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="fe90b-170">It should look like the following when you are finished.</span></span>
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a><span data-ttu-id="fe90b-171">Applicare la parola chiave async</span><span class="sxs-lookup"><span data-stu-id="fe90b-171">Apply the async keyword</span></span>

<span data-ttu-id="fe90b-172">Applicare quindi la parola chiave `async` al metodo **Main**.</span><span class="sxs-lookup"><span data-stu-id="fe90b-172">Next, apply the `async` keyword to the **Main** method.</span></span> <span data-ttu-id="fe90b-173">Sarà necessario eseguire tre operazioni.</span><span class="sxs-lookup"><span data-stu-id="fe90b-173">We will have to do three things.</span></span>

1. <span data-ttu-id="fe90b-174">Aggiungere la parola chiave `async` alla firma del metodo **Main**.</span><span class="sxs-lookup"><span data-stu-id="fe90b-174">Add the `async` keyword onto the **Main** method signature.</span></span>
1. <span data-ttu-id="fe90b-175">Cambiare il tipo restituito da `void` a `Task`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-175">Change the return type from `void` to `Task`.</span></span>
1. <span data-ttu-id="fe90b-176">Rimuovere la chiamata a `Wait()` nella chiamata a `SendArticleAsync` e sostituirla con `await`.</span><span class="sxs-lookup"><span data-stu-id="fe90b-176">Remove the call to `Wait()` on the call to `SendArticleAsync` and replace it with `await`.</span></span>

<span data-ttu-id="fe90b-177">Provare a eseguire di nuovo l'app. Dovrebbe funzionare esattamente come prima, ma ora il codice è in grado di rilasciare il thread al pool di thread .NET durante l'attesa dell'inserimento del messaggio nella coda.</span><span class="sxs-lookup"><span data-stu-id="fe90b-177">Try running the app again - it should still work exactly as before, but now the code is able to release the thread back to the .NET threadpool while we wait for the message to be queued.</span></span>

<span data-ttu-id="fe90b-178">Ora che il messaggio è stato inviato, l'ultimo passaggio consiste nell'aggiungere il supporto per la _ricezione_ del messaggio.</span><span class="sxs-lookup"><span data-stu-id="fe90b-178">Now that we have sent a message, the last step is to add support to _receive_ the message.</span></span>