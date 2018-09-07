<span data-ttu-id="fe898-101">Per completare l'applicazione, scrivere il codice per leggere il messaggio successivo in coda, elaborarlo e quindi eliminarlo dalla coda.</span><span class="sxs-lookup"><span data-stu-id="fe898-101">Now we want to complete the application by writing code to read the next message in the queue, processes it, and then delete it from the queue.</span></span> 

<span data-ttu-id="fe898-102">Il codice verrà inserito nella stessa applicazione ed eseguito quando non viene passato alcun parametro, ma in questo scenario del servizio di notizie, il codice dovrebbe essere inserito nei server di livello intermedio per elaborare le storie.</span><span class="sxs-lookup"><span data-stu-id="fe898-102">We're going to place this code into the same application and execute it when you don't pass any parameters, however in our news service scenario, we would really place this code into our middle-tier servers to process the stories.</span></span>

## <a name="dequeue-a-message"></a><span data-ttu-id="fe898-103">Rimuovere un messaggio dalla coda</span><span class="sxs-lookup"><span data-stu-id="fe898-103">Dequeue a message</span></span>

<span data-ttu-id="fe898-104">È possibile aggiungere un nuovo metodo che recupera il messaggio successivo dalla coda.</span><span class="sxs-lookup"><span data-stu-id="fe898-104">Let's add a new method that retrieves the next message from the queue.</span></span>

1. <span data-ttu-id="fe898-105">Passare alla cartella `QueueApp` in Cloud Shell e digitare `code .` per aprire l'editor online.</span><span class="sxs-lookup"><span data-stu-id="fe898-105">Switch to the `QueueApp` folder in the Cloud Shell and type `code .` to open the online editor.</span></span>
 
2. <span data-ttu-id="fe898-106">Aprire il file di origine `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="fe898-106">Open the `Program.cs` source file.</span></span>

3. <span data-ttu-id="fe898-107">Creare un metodo statico nella classe `Program` denominato `ReceiveArticleAsync` che non accetta parametri e restituisce `Task<string>`.</span><span class="sxs-lookup"><span data-stu-id="fe898-107">Create a static method in the `Program` class named `ReceiveArticleAsync` that takes no parameters and returns a `Task<string>`.</span></span> <span data-ttu-id="fe898-108">Si userà questo metodo per estrarre un articolo dalla coda e restituirlo.</span><span class="sxs-lookup"><span data-stu-id="fe898-108">We'll use this method to pull a news article from the queue and return it.</span></span>
    - <span data-ttu-id="fe898-109">Procedere e aggiungere la parola chiave `async` al metodo, poiché verranno usati alcuni metodi asincroni basati su `Task`.</span><span class="sxs-lookup"><span data-stu-id="fe898-109">Go ahead and add the `async` keyword to the method since we'll be using some asynchronous `Task`-based methods.</span></span>

    ```csharp
    static async Task<string> ReceiveArticleAsync()
    {
    }

4. All of the setup code to get a `CloudQueue` will be identical to what we did in the last exercise. Code duplication is a bad habit, even in samples so go ahead and, refactor the code that obtains the `CloudQueue` to a new method named `GetQueue` and change the `SendArticleAsync` to use your new method.
     - Make sure to leave the code that _creates_ the queue in the `SendArticleAsync` method; remember only the **publisher** should create the queue.

    ```csharp
    const string ConnectionString = ...;
    // ...

    static CloudQueue GetQueue()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        return queueClient.GetQueueReference("newsqueue");
    }
    ```
    
5. <span data-ttu-id="fe898-110">Nel metodo `ReceiveArticleAsync`, chiamare il nuovo metodo `GetQueue` per recuperare il riferimento di coda e assegnarlo a una variabile.</span><span class="sxs-lookup"><span data-stu-id="fe898-110">In your `ReceiveArticleAsync` method, call the new `GetQueue` method to retrieve your queue reference and assign it to a variable.</span></span>

6. <span data-ttu-id="fe898-111">Successivamente, chiamare il metodo `ExistsAsync` sull'oggetto `CloudQueue`; verrà restituito se la coda è stata creata.</span><span class="sxs-lookup"><span data-stu-id="fe898-111">Next, call the `ExistsAsync` method on the `CloudQueue` object; this will return whether the queue has been created.</span></span> <span data-ttu-id="fe898-112">Se si prova a recuperare un messaggio da una coda inesistente, l'API genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="fe898-112">If we attempt to retrieve a message from a non-existent queue, the API will throw an exception.</span></span>
    - <span data-ttu-id="fe898-113">Questo metodo è asincrono, pertanto usare `await` per ottenere il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="fe898-113">This method is asynchronous so use `await` to get the return value.</span></span>
    - <span data-ttu-id="fe898-114">Si dovrebbe già disporre della parola chiave `async` sul metodo `ReceiveArticleAsync`, ma in caso contrario, aggiungerla.</span><span class="sxs-lookup"><span data-stu-id="fe898-114">You should already have the `async` keyword on the `ReceiveArticleAsync` method, but if not add it now.</span></span>


7. <span data-ttu-id="fe898-115">Aggiungere un blocco `if` che usa il valore restituito da `ExistsAsync`.</span><span class="sxs-lookup"><span data-stu-id="fe898-115">Add an `if` block that uses the return value from `ExistsAsync`.</span></span> <span data-ttu-id="fe898-116">Verrà aggiunto il codice per leggere un valore dalla coda nel blocco.</span><span class="sxs-lookup"><span data-stu-id="fe898-116">We'll add our code to read a value from the queue into the block.</span></span> <span data-ttu-id="fe898-117">Aggiungere una stringa restituita finale al metodo che indica che nessun valore è stato letto.</span><span class="sxs-lookup"><span data-stu-id="fe898-117">Add a final return string to the method that indicates no value was read.</span></span> <span data-ttu-id="fe898-118">Il metodo dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fe898-118">Your method should be looking something like this:</span></span>

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
    }

    return "<queue empty or not created>";
}
```

8. <span data-ttu-id="fe898-119">Chiamare `GetMessageAsync` sull'oggetto `CloudQueue` per ottenere il primo `CloudQueueMessage` dalla coda.</span><span class="sxs-lookup"><span data-stu-id="fe898-119">Call `GetMessageAsync` on the `CloudQueue` object to get the first `CloudQueueMessage` from the queue.</span></span> <span data-ttu-id="fe898-120">Se la coda è vuota, il valore restituito sarà `null`.</span><span class="sxs-lookup"><span data-stu-id="fe898-120">The return value will be `null` if the queue is empty.</span></span>

9. <span data-ttu-id="fe898-121">Se è diverso da zero, usare la proprietà `AsString` sull'oggetto `CloudQueueMessage` per ottenere il contenuto del messaggio.</span><span class="sxs-lookup"><span data-stu-id="fe898-121">If it's non-null, use the `AsString` property on the `CloudQueueMessage` object to get the contents of the message.</span></span>

10. <span data-ttu-id="fe898-122">Chiamare `DeleteMessageAsync` sull'oggetto `CloudQueue` per eliminare il messaggio dalla coda.</span><span class="sxs-lookup"><span data-stu-id="fe898-122">Call `DeleteMessageAsync` on the `CloudQueue` object to delete the message from the queue.</span></span>

<span data-ttu-id="fe898-123">L'implementazione del metodo finale dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="fe898-123">The final method implementation should resemble:</span></span>

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
        CloudQueueMessage retrievedArticle = await queue.GetMessageAsync();
        if (retrievedArticle != null)
        {
            string newsMessage = retrievedArticle.AsString;
            await queue.DeleteMessageAsync(retrievedArticle);
            return newsMessage;
        }
    }

    return "<queue empty or not created>";
}
```
## <a name="call-the-receivearticleasync-method"></a><span data-ttu-id="fe898-124">Chiamare il metodo ReceiveArticleAsync</span><span class="sxs-lookup"><span data-stu-id="fe898-124">Call the ReceiveArticleAsync method</span></span>

<span data-ttu-id="fe898-125">Infine, aggiungere il supporto per richiamare il nuovo metodo.</span><span class="sxs-lookup"><span data-stu-id="fe898-125">Finally, let's add support to invoke our new method.</span></span> <span data-ttu-id="fe898-126">Questa operazione viene eseguita quando non vengono passati parametri al programma.</span><span class="sxs-lookup"><span data-stu-id="fe898-126">We'll do this when we don't pass any parameters into the program.</span></span>

1. <span data-ttu-id="fe898-127">Individuare il metodo `Main` e in particolare il blocco `if` aggiunto in precedenza per cercare i parametri.</span><span class="sxs-lookup"><span data-stu-id="fe898-127">Locate the `Main` method and specifically the `if` block you added earlier to look for parameters.</span></span>
1. <span data-ttu-id="fe898-128">Aggiungere una condizione `else` e chiamare il metodo `ReceiveArticleAsync`.</span><span class="sxs-lookup"><span data-stu-id="fe898-128">Add an `else` condition and call the `ReceiveArticleAsync` method.</span></span> 
1. <span data-ttu-id="fe898-129">Poiché è asincrono, è in genere preferibile usare `await`, tuttavia, come spiegato in precedenza, non funziona in tutte le versioni di C#; per questo motivo, usare semplicemente la proprietà `Result` per ottenere il valore restituito e stamparlo nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="fe898-129">Since it's asynchronous, we would normally want to use `await`, however as explained earlier that doesn't work in all versions of C# - so just use the `Result` property to get the return value and print it to the console window.</span></span>

<span data-ttu-id="fe898-130">Il codice dovrebbe essere simile a:</span><span class="sxs-lookup"><span data-stu-id="fe898-130">Your code should look something like:</span></span>

```csharp
if (args.Length > 0)
{
    // ...
}
else
{
    string value = ReceiveArticleAsync().Result;
    Console.WriteLine($"Received {value}");
}
```

## <a name="execute-the-application"></a><span data-ttu-id="fe898-131">Eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="fe898-131">Execute the application</span></span>

<span data-ttu-id="fe898-132">Il codice è ora completo.</span><span class="sxs-lookup"><span data-stu-id="fe898-132">The code is now complete.</span></span> <span data-ttu-id="fe898-133">Può inviare e recuperare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="fe898-133">It can now send and retrieve messages.</span></span> <span data-ttu-id="fe898-134">Per testarlo, usare `dotnet run` e passare i parametri per inviare messaggi, quindi omettere i parametri per la lettura di un singolo messaggio.</span><span class="sxs-lookup"><span data-stu-id="fe898-134">To test it, use `dotnet run` and pass parameters to send messages, and leave off parameters to read a single message.</span></span>

<span data-ttu-id="fe898-135">Se si desidera testare quando una coda non esiste, è possibile eliminare la coda (e tutti i dati) con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="fe898-135">If you want to test when a queue doesn't exist, you can delete the queue (and all the data) with the Azure CLI.</span></span> <span data-ttu-id="fe898-136">Assicurarsi di sostituire il parametro `<connection-string>` (o impostare la variabile di ambiente).</span><span class="sxs-lookup"><span data-stu-id="fe898-136">Make sure to replace the `<connection-string>` parameter (or set the environment variable).</span></span>

```azurecli
az storage queue delete --name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="fe898-137">Quando si crea un nuovo messaggio, la coda dovrebbe essere ricreata.</span><span class="sxs-lookup"><span data-stu-id="fe898-137">The next time you add a message, the queue should be re-created.</span></span>