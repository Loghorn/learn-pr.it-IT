In questo caso, si aggiungerà un'ora di scadenza ai dati in Cache Redis di Azure.

## <a name="add-an-expiration-time"></a>Aggiungere un'ora di scadenza

Nell'esercizio precedente, è stata interrotta con il codice seguente nel **Program.cs**.

```csharp
using (RedisClient redisClient = new RedisClient(redisConnectionString))
{
    //Create the transaction
    var transaction = redisClient.CreateTransaction();

    //Add multiple operations to the transaction
    transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
    transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

    //Commit and get result of transaction
    var transactionResult = transaction.Commit();

    if(transactionResult)
        Console.WriteLine("Transaction committed");
    else
        Console.WriteLine("Transaction failed to commit");
}
```

Aggiungiamo una scadenza di 15 secondi a entrambe **MyKey1** e **MyKey2**.

Aggiungere il codice seguente prima del commit della transazione:

    ```csharp
    //Add an expiration time
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
    ```

In questo codice, il **Expire** metodo fa parte il **RedisNativeClient**. Per accedere al metodo, è necessario eseguire il cast dell'oggetto.

## <a name="verify-the-expiration"></a>Verificare la scadenza

Ora che abbiamo aggiunto il codice per impostare come scaduti i dati, è possibile eseguire il programma e verificare che i dati vengono rimossi dalla Cache Redis di Azure.

1. Eseguire il programma.

    ```bash
    dotnet run
    ```
    
1. Passare alla console di Cache Redis di Azure nel portale di Azure.

1. Per verificare che i dati sono ancora presenti, eseguire il comando seguente:

    ```
    get MyKey1
    ```

1. Dopo 15 secondi, eseguire nuovamente il comando. Si dovrebbe vedere che i dati non sono più presente.

    ![Screenshot della console di Cache Redis di Azure che mostra il valore di MyKey1 nil](../media/6-redis-console-data-expiration.png)