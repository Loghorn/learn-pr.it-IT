<span data-ttu-id="eeb2d-101">Le applicazioni distribuite usano le code, ad esempio le code dei bus di servizio, come posizioni di archiviazione temporanea per i messaggi in attesa di essere recapitati a un componente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-101">Distributed applications use queues, such as Service Bus queues, as temporary storage locations for messages that are awaiting delivery to a destination component.</span></span> <span data-ttu-id="eeb2d-102">Per inviare e ricevere messaggi tramite una coda, è necessario scrivere codice nei componenti di origine e di destinazione.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-102">To send and receive messages through a queue, you must write code in the source and destination components.</span></span>

<span data-ttu-id="eeb2d-103">Si consideri l'applicazione Contoso Slices.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-103">Consider the Contoso Slices application.</span></span> <span data-ttu-id="eeb2d-104">Il cliente effettua l'ordine tramite un sito Web o un'app per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-104">The customer places the order through a website or mobile app.</span></span> <span data-ttu-id="eeb2d-105">Dal momento che vengono effettuati su dispositivi del cliente, non esiste alcun limite al numero di ordini che è possibile inviare contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-105">Because websites and mobile apps run on customer devices, there is really no limit to how many orders could come in at once.</span></span> <span data-ttu-id="eeb2d-106">Se l'app per dispositivi mobili e il sito Web depositano gli ordini in una coda, il componente di back-end (un'app Web) può elaborare gli ordini da tale coda in base ai propri tempi.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-106">By having the mobile app and website deposit the orders in a queue, we can allow the back-end component (a web app) to process orders from that queue at its own pace.</span></span>

<span data-ttu-id="eeb2d-107">L'applicazione Contoso Slices prevede diversi passaggi per gestire un nuovo ordine.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-107">The Contoso Slices application actually has several steps to handle a new order.</span></span> <span data-ttu-id="eeb2d-108">Ma per tutti i passaggi è prima richiesta l'autorizzazione del pagamento, di conseguenza decidiamo di usare una coda.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-108">But all of them are dependent on first authorizing payment, so we decide to use a queue.</span></span> <span data-ttu-id="eeb2d-109">Il primo processo del componente di ricezione consiste nell'elaborazione del pagamento.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-109">Our receiving component's first job will be processing the payment.</span></span>

<span data-ttu-id="eeb2d-110">Sia per l'app per dispositivi mobili che per il sito Web, Contoso deve scrivere il codice per aggiungere un messaggio alla coda.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-110">In the mobile app and website, Contoso needs to write code that adds a message to the queue.</span></span> <span data-ttu-id="eeb2d-111">Nell'app Web di back-end è invece necessario scrivere il codice che preleva i messaggi dalla coda.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-111">In the back-end web app, they'll write code that picks up messages from the queue.</span></span>

<span data-ttu-id="eeb2d-112">In questa esercitazione verrà spiegato come scrivere tale codice.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-112">Here, you will learn how to write that code.</span></span>

## <a name="the-microsoftazureservicebus-nuget-package"></a><span data-ttu-id="eeb2d-113">Pacchetto NuGet Microsoft.Azure.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="eeb2d-113">The Microsoft.Azure.ServiceBus NuGet package</span></span>

<span data-ttu-id="eeb2d-114">Per semplificare la scrittura di codice che invia e riceve i messaggi tramite il bus di servizio, Microsoft offre una libreria di classi .NET, che è possibile usare in qualsiasi linguaggio .NET Framework per interagire con una coda, argomento o inoltro del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-114">To make it easy to write code that sends and receives messages through Service Bus, Microsoft provides a library of .NET classes, which you can use in any .NET Framework language to interact with a Service Bus queue, topic, or relay.</span></span> <span data-ttu-id="eeb2d-115">È possibile includere questa libreria nell'applicazione aggiungendo il pacchetto NuGet **Microsoft.Azure.ServiceBus**.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-115">You can include this library in your application by adding the **Microsoft.Azure.ServiceBus** NuGet package.</span></span>

<span data-ttu-id="eeb2d-116">La classe più importante di questa libreria è la classe `QueueClient`.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-116">The most important class in this library for queues is the `QueueClient` class.</span></span> <span data-ttu-id="eeb2d-117">Per iniziare, è necessario creare un'istanza di questa classe nei componenti di invio e ricezione.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-117">You must start by instantiating this class both in sending and receiving components.</span></span>

## <a name="connection-strings-and-keys"></a><span data-ttu-id="eeb2d-118">Stringhe di connessione e chiavi</span><span class="sxs-lookup"><span data-stu-id="eeb2d-118">Connection strings and keys</span></span>

<span data-ttu-id="eeb2d-119">I componenti di origine e di destinazione hanno entrambi bisogno di due elementi per connettersi a una coda in uno spazio dei nomi del bus di servizio:</span><span class="sxs-lookup"><span data-stu-id="eeb2d-119">Source components and destination components both need two pieces of information to connect to a queue in a Service Bus namespace:</span></span>

- <span data-ttu-id="eeb2d-120">La posizione dello spazio dei nomi del bus di servizio, conosciuta anche come **endpoint**.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-120">The location of the Service Bus namespace, also known as an **endpoint**.</span></span> <span data-ttu-id="eeb2d-121">La posizione viene specificata come nome di dominio completo all'interno del dominio **servicebus.windows.net**,</span><span class="sxs-lookup"><span data-stu-id="eeb2d-121">The location is specified as a fully qualified domain name within the **servicebus.windows.net** domain.</span></span> <span data-ttu-id="eeb2d-122">ad esempio **pizzaService.servicebus.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-122">For example: **pizzaService.servicebus.windows.net**.</span></span>
- <span data-ttu-id="eeb2d-123">Una chiave di accesso.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-123">An access key.</span></span> <span data-ttu-id="eeb2d-124">Il bus di servizio limita l'accesso a code, argomenti e inoltri richiedendo una chiave di accesso.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-124">Service Bus restricts access to queues, topics, and relays by requiring an access key.</span></span>

<span data-ttu-id="eeb2d-125">Entrambi questi elementi vengono forniti all'oggetto `QueueClient` sotto forma di stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-125">Both of these pieces of information are provided to the `QueueClient` object in the form of a connection string.</span></span> <span data-ttu-id="eeb2d-126">È possibile ottenere la stringa di connessione corretta per lo spazio dei nomi dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-126">You can obtain the correct connection string for your namespace from the Azure portal.</span></span>

## <a name="calling-methods-asynchronously"></a><span data-ttu-id="eeb2d-127">Chiamata dei metodi in modalità asincrona</span><span class="sxs-lookup"><span data-stu-id="eeb2d-127">Calling methods asynchronously</span></span>

<span data-ttu-id="eeb2d-128">La coda di Azure può trovarsi a migliaia di chilometri dai componenti di invio e ricezione.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-128">The queue in Azure may be located thousands of miles away from sending and receiving components.</span></span> <span data-ttu-id="eeb2d-129">Anche se è fisicamente vicina, connessioni lente e la contesa della larghezza di banda possono causare ritardi quando un componente chiama un metodo sulla coda.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-129">Even if it is physically close, slow connections and bandwidth contention may cause delays when a component calls a method on the queue.</span></span> <span data-ttu-id="eeb2d-130">Per questo motivo la libreria client del bus di servizio fa in modo che i metodi `async` siano disponibili per l'interazione con le code.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-130">For this reason, the Service Bus client library makes `async` methods available for interacting with the queues.</span></span> <span data-ttu-id="eeb2d-131">Useremo questi metodi per evitare di bloccare un thread in attesa del completamento delle chiamate.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-131">We'll use these methods to avoid blocking a thread while waiting for calls to complete.</span></span>

<span data-ttu-id="eeb2d-132">Quando si invia un messaggio a una coda, ad esempio, usare il metodo `QueueClient.SendAsync()` con la parola chiave `await`.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-132">When sending a message to a queue, for example, use the `QueueClient.SendAsync()` method with the `await` keyword.</span></span>

## <a name="write-code-that-sends-to-queues"></a><span data-ttu-id="eeb2d-133">Scrivere codice che invia messaggi alle code</span><span class="sxs-lookup"><span data-stu-id="eeb2d-133">Write code that sends to queues</span></span> 

<span data-ttu-id="eeb2d-134">In qualsiasi componente di invio o di ricezione è necessario aggiungere le istruzioni `using` seguenti a qualsiasi file di codice che chiama una coda del bus di servizio:</span><span class="sxs-lookup"><span data-stu-id="eeb2d-134">In any sending or receiving component, you should add the following `using` statements to any code file that calls a Service Bus queue:</span></span>

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

<span data-ttu-id="eeb2d-135">Creare quindi un nuovo oggetto `QueueClient` e passarvi la stringa di connessione e il nome della coda:</span><span class="sxs-lookup"><span data-stu-id="eeb2d-135">Next, create a new `QueueClient` object and pass it the connection string and the name of the queue:</span></span>

```C#
queueClient = new QueueClient(TextAppConnectionString, "PrivateMessageQueue");
```

<span data-ttu-id="eeb2d-136">Per inviare un messaggio alla coda, è possibile chiamare il metodo `QueueClient.SendAsync()` e passare il messaggio sotto forma di stringa con codifica UTF-8:</span><span class="sxs-lookup"><span data-stu-id="eeb2d-136">You can send a message to the queue by calling the `QueueClient.SendAsync()` method and passing the message in the form of a UTF-8 encoded string:</span></span>

```C#
string message = "Sure would like a large pepperoni!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await queueClient.SendAsync(encodedMessage);
```

## <a name="receive-messages-from-queue"></a><span data-ttu-id="eeb2d-137">Ricevere messaggi dalla coda</span><span class="sxs-lookup"><span data-stu-id="eeb2d-137">Receive messages from queue</span></span>

<span data-ttu-id="eeb2d-138">Per ricevere i messaggi, è prima necessario registrare un gestore di messaggi. Si tratta del metodo nel codice che verrà richiamato quando nella coda è presente un messaggio.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-138">To receive messages, you must first register a message handler - this is the method in your code that will be invoked when a message is available on the queue.</span></span>

```C#
queueClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

<span data-ttu-id="eeb2d-139">Eseguire l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="eeb2d-139">Do your processing work.</span></span> <span data-ttu-id="eeb2d-140">Nel gestore di messaggi chiamare il metodo `QueueClient.CompleteAsync()` per rimuovere il messaggio dalla coda:</span><span class="sxs-lookup"><span data-stu-id="eeb2d-140">Then, within the message handler, call the `QueueClient.CompleteAsync()` method to remove the message from the queue:</span></span>

```C#
await queueClient.CompleteAsync(message.SystemProperties.LockToken);
```
    