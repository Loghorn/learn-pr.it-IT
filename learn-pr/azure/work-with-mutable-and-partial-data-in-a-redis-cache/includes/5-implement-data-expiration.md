Di seguito si vedrà come aggiungere un'ora di scadenza ai dati in Cache Redis di Azure.

## <a name="add-an-expiration-time"></a>Aggiungere un'ora di scadenza

Dall'ultimo esercizio è rimasto il codice seguente in **Program.cs**.

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

Verrà aggiunta una scadenza di 15 secondi sia per **MyKey1** che per **MyKey2**.

Aggiungere il codice seguente prima di eseguire il commit della transazione:

```csharp
//Add an expiration time
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
```

In questo codice, il metodo **Expire** fa parte di **RedisNativeClient**. Per accedere al metodo, è prima necessario eseguire il cast dell'oggetto.

## <a name="verify-the-expiration"></a>Verificare la scadenza

Ora che è stato aggiunto il codice per impostare la scadenza dei dati, è possibile eseguire il programma e controllare che i dati vengano rimossi da Cache Redis di Azure.

1. Eseguire il programma.

    ```bash
    dotnet run
    ```

1. Tornare alla console di Cache Redis di Azure nel portale di Azure.

1. Per verificare che i dati siano ancora presenti, eseguire il comando seguente:

    ```console
    get MyKey1
    ```

1. Dopo 15 secondi, eseguire di nuovo il comando. I dati non dovrebbero essere più presenti.

    ![Screenshot della console di Cache Redis di Azure che mostra il valore vuoto per MyKey1](../media/6-redis-console-data-expiration.png)