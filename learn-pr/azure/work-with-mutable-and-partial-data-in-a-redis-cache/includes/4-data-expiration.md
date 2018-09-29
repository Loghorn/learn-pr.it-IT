<span data-ttu-id="88e4b-101">I dati non sempre sono permanenti e talvolta è necessario eliminarli con tempistiche specifiche.</span><span class="sxs-lookup"><span data-stu-id="88e4b-101">Data is not always permanent, and there are times you would like to delete it at a specific time.</span></span> <span data-ttu-id="88e4b-102">Ad esempio, nell'applicazione di messaggistica istantanea gli utenti possono impostare il nome visualizzato della chat di gruppo.</span><span class="sxs-lookup"><span data-stu-id="88e4b-102">For example, in your instant messaging application, users can set the display name of the group chat.</span></span> <span data-ttu-id="88e4b-103">Si vuole tuttavia reimpostare il nome dopo un'ora.</span><span class="sxs-lookup"><span data-stu-id="88e4b-103">However, you want the name to reset after one hour.</span></span> <span data-ttu-id="88e4b-104">È possibile eseguire questa operazione scrivendo logica sul lato server personalizzata per impostare un timer di un'ora ed eliminare il nome.</span><span class="sxs-lookup"><span data-stu-id="88e4b-104">You could accomplish this by writing your own server-side logic that set an hour timer and deleted the name.</span></span> <span data-ttu-id="88e4b-105">Tuttavia, Cache Redis di Azure supporta la scadenza dei dati, una funzionalità che consente di ottenere questo risultato automaticamente senza dover scrivere logica aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="88e4b-105">However, Azure Redis Cache supports data expiration, which is a feature that does this automatically without writing additional logic.</span></span>

<span data-ttu-id="88e4b-106">In questa unità verranno presentati i comandi Redis comuni per implementare la scadenza dei dati.</span><span class="sxs-lookup"><span data-stu-id="88e4b-106">Here, you'll learn about the common Redis commands to implement data expiration.</span></span>

## <a name="what-is-data-expiration"></a><span data-ttu-id="88e4b-107">Che cos'è la scadenza dei dati?</span><span class="sxs-lookup"><span data-stu-id="88e4b-107">What is data expiration?</span></span>

<span data-ttu-id="88e4b-108">La scadenza dei dati è una funzionalità in grado di eliminare automaticamente una chiave e un valore nella cache dopo un determinato periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="88e4b-108">Data expiration is a feature that can automatically delete a key and value in the cache after a set amount of time.</span></span>

## <a name="why-use-data-expiration"></a><span data-ttu-id="88e4b-109">Perché usare la scadenza dei dati?</span><span class="sxs-lookup"><span data-stu-id="88e4b-109">Why use data expiration?</span></span>

<span data-ttu-id="88e4b-110">La scadenza dei dati viene usata comunemente nelle situazioni in cui i dati archiviati sono di breve durata.</span><span class="sxs-lookup"><span data-stu-id="88e4b-110">Data expiration is commonly used in situations where the data you're storing is short-lived.</span></span>  <span data-ttu-id="88e4b-111">Questo è importante poiché Cache Redis di Azure è un database in memoria e non è disponibile la stessa quantità di memoria che sarebbe a disposizione se i dati fossero archiviati su disco.</span><span class="sxs-lookup"><span data-stu-id="88e4b-111">This is important because Azure Redis Cache is an in-memory database and you don't have as much memory available to use as you would if you were storing on disk.</span></span> <span data-ttu-id="88e4b-112">Dato che lo spazio di archiviazione con Cache Redis di Azure è limitato, è opportuno assicurarsi di archiviare solo i dati importanti.</span><span class="sxs-lookup"><span data-stu-id="88e4b-112">Since you have limited storage with Azure Redis Cache, you want to make sure you're only storing data that is important.</span></span> <span data-ttu-id="88e4b-113">Se i dati non devono essere disponibili per molto tempo, assicurarsi di impostare una scadenza.</span><span class="sxs-lookup"><span data-stu-id="88e4b-113">If the data doesn't need to be around for a long time, make sure you set an expiration.</span></span>

## <a name="how-to-use-data-expiration-in-azure-redis-cache"></a><span data-ttu-id="88e4b-114">Come usare la scadenza dei dati in Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="88e4b-114">How to use data expiration in Azure Redis Cache</span></span>

<span data-ttu-id="88e4b-115">Sono disponibili diversi comandi per implementare e gestire la scadenza dei dati in Cache Redis di Azure:</span><span class="sxs-lookup"><span data-stu-id="88e4b-115">There are different commands to implement and manage data expiration in Azure Redis Cache:</span></span>

- <span data-ttu-id="88e4b-116">`EXPIRE`: imposta il timeout di una chiave in secondi</span><span class="sxs-lookup"><span data-stu-id="88e4b-116">`EXPIRE`: Sets the timeout of a key in seconds</span></span>
- <span data-ttu-id="88e4b-117">`PEXPIRE`: imposta il timeout di una chiave in millisecondi</span><span class="sxs-lookup"><span data-stu-id="88e4b-117">`PEXPIRE`: Sets the timeout of a key in milliseconds</span></span>
- <span data-ttu-id="88e4b-118">`EXPIREAT`: imposta il timeout di una chiave usando un timestamp Unix assoluto in secondi</span><span class="sxs-lookup"><span data-stu-id="88e4b-118">`EXPIREAT`: Sets the timeout of a key using an absolute Unix timestamp in seconds</span></span>
- <span data-ttu-id="88e4b-119">`PEXPIREAT`: imposta il timeout di una chiave usando un timestamp Unix assoluto in millisecondi</span><span class="sxs-lookup"><span data-stu-id="88e4b-119">`PEXPIREAT`: Sets the timeout of a key using an absolute Unix timestamp in milliseconds</span></span>
- <span data-ttu-id="88e4b-120">`TTL`: restituisce il tempo rimanente di durata di una chiave in secondi</span><span class="sxs-lookup"><span data-stu-id="88e4b-120">`TTL`: Returns the remaining time a key has to live in seconds</span></span>
- <span data-ttu-id="88e4b-121">`PTTL`: restituisce il tempo rimanente di durata di una chiave in millisecondi</span><span class="sxs-lookup"><span data-stu-id="88e4b-121">`PTTL`: Returns the remaining time a key has to live in milliseconds</span></span>
- <span data-ttu-id="88e4b-122">`PERSIST`: imposta una chiave senza scadenza</span><span class="sxs-lookup"><span data-stu-id="88e4b-122">`PERSIST`: Makes a key never expire</span></span>

<span data-ttu-id="88e4b-123">Il comando più comune è `EXPIRE`, che verrà usato in questo modulo.</span><span class="sxs-lookup"><span data-stu-id="88e4b-123">The most common command is `EXPIRE`, and we'll use it throughout this module.</span></span>

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a><span data-ttu-id="88e4b-124">Esempio di scadenza dei dati con C# e ServiceStack.Redis</span><span class="sxs-lookup"><span data-stu-id="88e4b-124">Example of data expiration using C# and ServiceStack.Redis</span></span>

<span data-ttu-id="88e4b-125">Tenere presente che, quando si usa una libreria client come **ServiceStack.Redis**, non è necessario preoccuparsi di ricordare i comandi di Cache Redis di Azure di basso livello.</span><span class="sxs-lookup"><span data-stu-id="88e4b-125">Remember, when using a client library like **ServiceStack.Redis**, we don't have to worry about remembering the low-level Azure Redis Cache commands.</span></span> <span data-ttu-id="88e4b-126">È invece possibile usare metodi C# semplici.</span><span class="sxs-lookup"><span data-stu-id="88e4b-126">Instead, we can use simple C# methods.</span></span>

```csharp
public static void SetGroupChatName(string groupChatID, string chatName)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create a key for group chat display names
        string key = groupChatID + "displayName";

        //Set the group chat display name
        redisClient.SetValue(key, chatName);

        //Set the expiration for one hour
        redisClient.Expire(key, 3600);
    }
}
```

<span data-ttu-id="88e4b-127">Cache Redis di Azure consente di eliminare automaticamente i dati dopo un determinato periodo di tempo usando la scadenza dei dati.</span><span class="sxs-lookup"><span data-stu-id="88e4b-127">Azure Redis Cache allows you to delete data automatically after a set amount of time using data expiration.</span></span> <span data-ttu-id="88e4b-128">Questo è importante poiché Cache Redis di Azure è un database in memoria e non è disponibile la stessa quantità di memoria che sarebbe a disposizione se i dati fossero archiviati su disco.</span><span class="sxs-lookup"><span data-stu-id="88e4b-128">This is important because Azure Redis Cache is an in-memory database, and you don't have as much memory available as you would with storing data on disk.</span></span> <span data-ttu-id="88e4b-129">Se non è necessario che i dati archiviati siano disponibili per molto tempo, assicurarsi di impostare una scadenza.</span><span class="sxs-lookup"><span data-stu-id="88e4b-129">If the data you're storing doesn't need to be around for a long time, make sure you set an expiration.</span></span>