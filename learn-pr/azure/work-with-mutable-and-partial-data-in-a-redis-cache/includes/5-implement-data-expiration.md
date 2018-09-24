<span data-ttu-id="642a1-101">Di seguito si vedrà come aggiungere un'ora di scadenza ai dati in Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="642a1-101">Here, you'll add an expiration time to our data in the Azure Redis Cache.</span></span>

## <a name="add-an-expiration-time"></a><span data-ttu-id="642a1-102">Aggiungere un'ora di scadenza</span><span class="sxs-lookup"><span data-stu-id="642a1-102">Add an expiration time</span></span>

<span data-ttu-id="642a1-103">Dall'ultimo esercizio è rimasto il codice seguente in **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="642a1-103">In the last exercise, we left off with the following code in **Program.cs**.</span></span>

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

if(transactionResult)
{
    Console.WriteLine("Transaction committed");
}
else
{
    Console.WriteLine("Transaction failed to commit");
}
```

<span data-ttu-id="642a1-104">Verrà aggiunta una scadenza di 15 secondi sia per **MyKey1** che per **MyKey2**.</span><span class="sxs-lookup"><span data-stu-id="642a1-104">Let’s add an expiration of 15 seconds to both **MyKey1** and **MyKey2**.</span></span>

<span data-ttu-id="642a1-105">Aggiungere il codice seguente prima di eseguire il commit della transazione:</span><span class="sxs-lookup"><span data-stu-id="642a1-105">Add the following code before you commit the transaction:</span></span>

```csharp
//Add an expiration time
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
```

<span data-ttu-id="642a1-106">In questo codice, il metodo **Expire** fa parte di **RedisNativeClient**.</span><span class="sxs-lookup"><span data-stu-id="642a1-106">In this code, the **Expire** method is a part of the **RedisNativeClient**.</span></span> <span data-ttu-id="642a1-107">Per accedere al metodo, è prima necessario eseguire il cast dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="642a1-107">To access the method, we must first cast our object.</span></span>

## <a name="verify-the-expiration"></a><span data-ttu-id="642a1-108">Verificare la scadenza</span><span class="sxs-lookup"><span data-stu-id="642a1-108">Verify the expiration</span></span>

<span data-ttu-id="642a1-109">Ora che è stato aggiunto il codice per impostare la scadenza dei dati, è possibile eseguire il programma e controllare che i dati vengano rimossi da Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="642a1-109">Now that we added the code to expire our data, let's run the program and check that the data is removed from Azure Redis Cache.</span></span>

1. <span data-ttu-id="642a1-110">Eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="642a1-110">Run the program.</span></span>

    ```bash
    dotnet run
    ```

1. <span data-ttu-id="642a1-111">Tornare alla console di Cache Redis di Azure nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="642a1-111">Switch back to the Azure Redis Cache console in the Azure portal.</span></span>

1. <span data-ttu-id="642a1-112">Per verificare che i dati siano ancora presenti, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="642a1-112">To verify that the data is still there, issue the following command:</span></span>

    ```console
    get MyKey1
    ```

1. <span data-ttu-id="642a1-113">Dopo 15 secondi, eseguire di nuovo il comando.</span><span class="sxs-lookup"><span data-stu-id="642a1-113">After 15 seconds, issue the command again.</span></span> <span data-ttu-id="642a1-114">I dati non dovrebbero essere più presenti.</span><span class="sxs-lookup"><span data-stu-id="642a1-114">You should see that the data is no longer there.</span></span>

    ![Screenshot della console di Cache Redis di Azure che mostra il valore vuoto per MyKey1](../media/6-redis-console-data-expiration.png)