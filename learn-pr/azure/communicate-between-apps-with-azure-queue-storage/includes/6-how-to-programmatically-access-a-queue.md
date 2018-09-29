<span data-ttu-id="11499-101">Le code contengono messaggi, ovvero pacchetti di dati la cui forma è nota al mittente e al destinatario dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="11499-101">Queues hold messages - packets of data whose shape is known to the sender application and receiver application.</span></span> <span data-ttu-id="11499-102">Il mittente crea la coda e aggiunge un messaggio.</span><span class="sxs-lookup"><span data-stu-id="11499-102">The sender creates the queue and adds a message.</span></span> <span data-ttu-id="11499-103">Il destinatario recupera il messaggio, lo elabora e quindi lo elimina dalla coda.</span><span class="sxs-lookup"><span data-stu-id="11499-103">The receiver retrieves a message, processes it, and then deletes the message from the queue.</span></span> <span data-ttu-id="11499-104">L'illustrazione seguente mostra un flusso tipico di questo processo.</span><span class="sxs-lookup"><span data-stu-id="11499-104">The following illustration shows a typical flow of this process.</span></span>

![Illustrazione che mostra un tipico flusso di messaggi attraverso Archiviazione code di Azure.](../media/6-message-flow.png)

<span data-ttu-id="11499-106">Tenere presente che `get` e `delete` sono operazioni distinte.</span><span class="sxs-lookup"><span data-stu-id="11499-106">Notice that `get` and `delete` are separate operations.</span></span> <span data-ttu-id="11499-107">Questa disposizione gestisce i potenziali errori sul lato del destinatario e implementa un concetto noto come _recapito At-Least-Once_.</span><span class="sxs-lookup"><span data-stu-id="11499-107">This arrangement handles potential failures in the receiver and implements a concept called _at-least-once delivery_.</span></span> <span data-ttu-id="11499-108">Dopo che il destinatario riceve un messaggio, questo rimane nella coda ma è invisibile per 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="11499-108">After the receiver gets a message, that message remains in the queue but is invisible for 30 seconds.</span></span> <span data-ttu-id="11499-109">Se sul lato del destinatario si verifica un arresto anomalo del sistema o un'interruzione dell'alimentazione durante l'elaborazione, il messaggio non verrà mai eliminato dalla coda.</span><span class="sxs-lookup"><span data-stu-id="11499-109">If the receiver crashes or experiences a power failure during processing, then it will never delete the message from the queue.</span></span> <span data-ttu-id="11499-110">Dopo 30 secondi, il messaggio ricompare nella coda e un'altra istanza del destinatario può elaborarlo fino al completamento.</span><span class="sxs-lookup"><span data-stu-id="11499-110">After 30 seconds, the message will reappear in the queue and another instance of the receiver can process it to completion.</span></span>

## <a name="the-azure-storage-client-library-for-net"></a><span data-ttu-id="11499-111">Libreria client di archiviazione di Azure per .NET</span><span class="sxs-lookup"><span data-stu-id="11499-111">The Azure Storage Client Library for .NET</span></span>

<span data-ttu-id="11499-112">La **Libreria client di archiviazione di Azure per .NET** fornisce i tipi per rappresentare ognuno degli oggetti con cui è necessario interagire:</span><span class="sxs-lookup"><span data-stu-id="11499-112">The **Azure Storage Client Library for .NET** provides types to represent each of the objects you need to interact with:</span></span>

- <span data-ttu-id="11499-113">`CloudStorageAccount` rappresenta l'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="11499-113">`CloudStorageAccount` represents your Azure storage account.</span></span>
- <span data-ttu-id="11499-114">`CloudQueueClient` rappresenta Archiviazione code di Azure.</span><span class="sxs-lookup"><span data-stu-id="11499-114">`CloudQueueClient` represents Azure Queue storage.</span></span>
- <span data-ttu-id="11499-115">`CloudQueue` rappresenta una delle istanze della coda.</span><span class="sxs-lookup"><span data-stu-id="11499-115">`CloudQueue` represents one of your queue instances.</span></span>
- <span data-ttu-id="11499-116">`CloudQueueMessage` rappresenta un messaggio.</span><span class="sxs-lookup"><span data-stu-id="11499-116">`CloudQueueMessage` represents a message.</span></span>

<span data-ttu-id="11499-117">Queste classi vengono usate per ottenere l'accesso a livello di codice alla coda.</span><span class="sxs-lookup"><span data-stu-id="11499-117">You will use these classes to get programmatic access to your queue.</span></span> <span data-ttu-id="11499-118">La libreria dispone di metodi sia sincroni che asincroni. Le versioni asincrone sono preferibili perché evitano di bloccare l'app client.</span><span class="sxs-lookup"><span data-stu-id="11499-118">The library has both synchronous and asynchronous methods; you should prefer to use the asynchronous versions to avoid blocking the client app.</span></span>

> [!NOTE]
> <span data-ttu-id="11499-119">La libreria client di archiviazione di Azure per .NET è disponibile nel pacchetto NuGet **WindowsAzure.Storage**.</span><span class="sxs-lookup"><span data-stu-id="11499-119">The Azure Storage Client Library for .NET is available in the **WindowsAzure.Storage** NuGet package.</span></span> <span data-ttu-id="11499-120">È possibile installarla tramite un ambiente di sviluppo integrato, l'interfaccia della riga di comando di Azure o `Install-Package WindowsAzure.Storage` di PowerShell.</span><span class="sxs-lookup"><span data-stu-id="11499-120">You can install it through an IDE, Azure CLI, or through PowerShell `Install-Package WindowsAzure.Storage`.</span></span>

## <a name="how-to-connect-to-a-queue"></a><span data-ttu-id="11499-121">Come connettersi a una coda</span><span class="sxs-lookup"><span data-stu-id="11499-121">How to connect to a queue</span></span>

<span data-ttu-id="11499-122">Per connettersi a una coda occorre prima creare un `CloudStorageAccount` con la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="11499-122">To connect to a queue, you first create a `CloudStorageAccount` with your connection string.</span></span> <span data-ttu-id="11499-123">L'oggetto risultante può quindi creare un `CloudQueueClient`, che a sua volta può aprire un'istanza di `CloudQueue`.</span><span class="sxs-lookup"><span data-stu-id="11499-123">The resulting object can then create a `CloudQueueClient`, which in turn can open a `CloudQueue` instance.</span></span> <span data-ttu-id="11499-124">Di seguito è illustrato il flusso di base del codice.</span><span class="sxs-lookup"><span data-stu-id="11499-124">The basic code flow is shown below.</span></span>

```csharp
CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);

CloudQueueClient client = account.CreateCloudQueueClient();

CloudQueue queue = client.GetQueueReference("myqueue");
```

<span data-ttu-id="11499-125">La creazione di una `CloudQueue` non implica necessariamente l'esistenza della coda di archiviazione _effettiva_.</span><span class="sxs-lookup"><span data-stu-id="11499-125">Creating a `CloudQueue` doesn't necessarily mean the _actual_ storage queue exists.</span></span> <span data-ttu-id="11499-126">È comunque possibile usare questo oggetto per creare, eliminare e verificare la presenza di una coda esistente.</span><span class="sxs-lookup"><span data-stu-id="11499-126">However, you can use this object to create, delete, and check for an existing queue.</span></span> <span data-ttu-id="11499-127">Come già accennato, tutti i metodi supportano le versioni sia sincrone che asincrone, ma useremo solo le versioni asincrone basate su `Task`.</span><span class="sxs-lookup"><span data-stu-id="11499-127">As mentioned above, all methods support both synchronous and asynchronous versions, but we will only be using the `Task`-based asynchronous versions.</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="11499-128">Come creare una coda</span><span class="sxs-lookup"><span data-stu-id="11499-128">How to create a queue</span></span>

<span data-ttu-id="11499-129">Per la creazione della coda si userà un criterio comune: l'applicazione mittente deve sempre essere responsabile della creazione della coda.</span><span class="sxs-lookup"><span data-stu-id="11499-129">You will use a common pattern for queue creation: the sender application should always be responsible for creating the queue.</span></span> <span data-ttu-id="11499-130">Questo criterio mantiene l'applicazione più autonoma e meno dipendente da attività di configurazione dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="11499-130">This keeps your application more self-contained and less dependent on administrative set-up.</span></span> 

<span data-ttu-id="11499-131">Per semplificare la creazione, la libreria client espone un metodo `CreateIfNotExistsAsync` che crea la coda se necessario o restituisce `false` se la coda esiste già.</span><span class="sxs-lookup"><span data-stu-id="11499-131">To make the creation simple, the client library exposes a `CreateIfNotExistsAsync` method that will create the queue if necessary, or return `false` if the queue already exists.</span></span> 

<span data-ttu-id="11499-132">Di seguito è illustrato il codice tipico.</span><span class="sxs-lookup"><span data-stu-id="11499-132">Typical code is shown below.</span></span>

```csharp
CloudQueue queue;
//...

await queue.CreateIfNotExistsAsync();
```

> [!NOTE]
> <span data-ttu-id="11499-133">Per consentire all'account di archiviazione di usare questa API sono necessarie le autorizzazioni `Write` o `Create`.</span><span class="sxs-lookup"><span data-stu-id="11499-133">You must have `Write` or `Create` permissions for the storage account to use this API.</span></span> <span data-ttu-id="11499-134">Questa condizione è sempre vera se si usa il modello di sicurezza **Chiave di accesso**, ma è possibile bloccare le autorizzazioni per l'account con altri approcci che consentano solo operazioni di lettura sulla coda.</span><span class="sxs-lookup"><span data-stu-id="11499-134">This is always true if you use the **Access Key** security model, but you can lock down permissions to the account with other approaches that will only allow read operations against the queue.</span></span>

## <a name="how-to-send-a-message"></a><span data-ttu-id="11499-135">Come inviare un messaggio</span><span class="sxs-lookup"><span data-stu-id="11499-135">How to send a message</span></span>

<span data-ttu-id="11499-136">Per inviare un messaggio, è necessario creare un'istanza di un oggetto `CloudQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="11499-136">To send a message, you instantiate a `CloudQueueMessage` object.</span></span> <span data-ttu-id="11499-137">La classe dispone di alcuni costruttori di overload che caricano i dati nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="11499-137">The class has a few overloaded constructors that load your data into the message.</span></span> <span data-ttu-id="11499-138">Useremo il costruttore che accetta una `string`.</span><span class="sxs-lookup"><span data-stu-id="11499-138">We will use the constructor that takes a `string`.</span></span> <span data-ttu-id="11499-139">Dopo aver creato il messaggio, si userà un oggetto `CloudQueue` per inviarlo.</span><span class="sxs-lookup"><span data-stu-id="11499-139">After creating the message, you use a `CloudQueue` object to send it.</span></span>

<span data-ttu-id="11499-140">Ecco un esempio tipico:</span><span class="sxs-lookup"><span data-stu-id="11499-140">Here's a typical example:</span></span>

```csharp
var message = new CloudQueueMessage("your message here");

CloudQueue queue;
//...

await queue.AddMessageAsync(message);
```

> [!NOTE]
> <span data-ttu-id="11499-141">Mentre le dimensioni totali della coda possono raggiungere i 500 TB, le dimensioni dei singoli messaggi non possono superare i 64 KB (48 KB se si usa la codifica Base64).</span><span class="sxs-lookup"><span data-stu-id="11499-141">While the total queue size can be up to 500 TB, the individual messages in it can only be up to 64 KB in size (48 KB when using Base64 encoding).</span></span> <span data-ttu-id="11499-142">Se occorre un payload superiore, è possibile combinare code e BLOB, passando l'URL ai dati effettivi (archiviati come BLOB) nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="11499-142">If you need a larger payload you can combine queues and blobs – passing the URL to the actual data (stored as a Blob) in the message.</span></span> <span data-ttu-id="11499-143">Questo approccio consentirebbe di accodare fino a 200 GB per un singolo elemento.</span><span class="sxs-lookup"><span data-stu-id="11499-143">This approach would allow you to enqueue up to 200 GB for a single item.</span></span>

## <a name="how-to-receive-and-delete-a-message"></a><span data-ttu-id="11499-144">Come ricevere ed eliminare un messaggio</span><span class="sxs-lookup"><span data-stu-id="11499-144">How to receive and delete a message</span></span>

<span data-ttu-id="11499-145">Sul lato del destinatario il messaggio successivo viene ricevuto, elaborato e quindi eliminato dopo che l'elaborazione è riuscita.</span><span class="sxs-lookup"><span data-stu-id="11499-145">In the receiver, you get the next message, process it, and then delete it after processing succeeds.</span></span> <span data-ttu-id="11499-146">Ecco un semplice esempio:</span><span class="sxs-lookup"><span data-stu-id="11499-146">Here's a simple example:</span></span>

```C#
CloudQueue queue;
//...

CloudQueueMessage message = await queue.GetMessageAsync();

if (message != null)
{
    // Process the message
    //...

    await queue.DeleteMessageAsync(message);
}
```

<span data-ttu-id="11499-147">Ora è possibile applicare quanto appreso alla nostra applicazione.</span><span class="sxs-lookup"><span data-stu-id="11499-147">Let's now apply this new knowledge to our application!</span></span>