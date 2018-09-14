Ora si vuole completare l'applicazione scrivendo codice per leggere il messaggio successivo in coda, elaborarli ed eliminarlo dalla coda. 

Il codice verrà inserito nella stessa applicazione ed eseguito quando non viene passato alcun parametro, ma in questo scenario del servizio di notizie, il codice dovrebbe essere inserito nei server di livello intermedio per elaborare le storie.

## <a name="dequeue-a-message"></a>Rimuovere un messaggio dalla coda

È possibile aggiungere un nuovo metodo che recupera il messaggio successivo dalla coda.

1. Passare alla cartella `QueueApp` in Cloud Shell e digitare `code .` per aprire l'editor online.
 
1. Aprire il file di origine `Program.cs`.

1. Creare un metodo statico nella classe `Program` denominato `ReceiveArticleAsync` che non accetta parametri e restituisce `Task<string>`. Si userà questo metodo per estrarre un articolo dalla coda e restituirlo.
    - Procedere e aggiungere la parola chiave `async` al metodo, poiché verranno usati alcuni metodi asincroni basati su `Task`.

    ```csharp
    static async Task<string> ReceiveArticleAsync()
    {
    }

1. All of the setup code to get a `CloudQueue` will be identical to what we did in the last exercise. Code duplication is a bad habit, even in samples so go ahead and, refactor the code that obtains the `CloudQueue` to a new method named `GetQueue` and change the `SendArticleAsync` to use your new method.
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
    
1. Nel metodo `ReceiveArticleAsync`, chiamare il nuovo metodo `GetQueue` per recuperare il riferimento di coda e assegnarlo a una variabile.

1. Successivamente, chiamare il metodo `ExistsAsync` sull'oggetto `CloudQueue`; verrà restituito se la coda è stata creata. Se si prova a recuperare un messaggio da una coda inesistente, l'API genererà un'eccezione.
    - Questo metodo è asincrono, pertanto usare `await` per ottenere il valore restituito.
    - Si dovrebbe già disporre della parola chiave `async` sul metodo `ReceiveArticleAsync`, ma in caso contrario, aggiungerla.


1. Aggiungere un blocco `if` che usa il valore restituito da `ExistsAsync`. Verrà aggiunto il codice per leggere un valore dalla coda nel blocco. Aggiungere una stringa restituita finale al metodo che indica che nessun valore è stato letto. Il metodo dovrebbe essere simile al seguente:

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

1. Chiamare `GetMessageAsync` sull'oggetto `CloudQueue` per ottenere il primo `CloudQueueMessage` dalla coda. Se la coda è vuota, il valore restituito sarà `null`.

1. Se è diverso da zero, usare la proprietà `AsString` sull'oggetto `CloudQueueMessage` per ottenere il contenuto del messaggio.

1. Chiamare `DeleteMessageAsync` sull'oggetto `CloudQueue` per eliminare il messaggio dalla coda.

L'implementazione del metodo finale dovrebbe essere simile alla seguente:

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

## <a name="call-the-receivearticleasync-method"></a>Chiamare il metodo ReceiveArticleAsync

Infine, aggiungere il supporto per richiamare il nuovo metodo. Questa operazione viene eseguita quando non vengono passati parametri al programma.

1. Individuare il metodo `Main` e in particolare il blocco `if` aggiunto in precedenza per cercare i parametri.

1. Aggiungere una condizione `else` e chiamare il metodo `ReceiveArticleAsync`. 

1. Poiché è asincrono, è in genere preferibile usare `await`, tuttavia, come spiegato in precedenza, non funziona in tutte le versioni di C#; per questo motivo, usare semplicemente la proprietà `Result` per ottenere il valore restituito e stamparlo nella finestra della console.

Il codice dovrebbe essere simile a:

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

## <a name="execute-the-application"></a>Eseguire l'applicazione

Il codice è ora completo. Può inviare e recuperare i messaggi. Per testarlo, usare `dotnet run` e passare i parametri per inviare messaggi, quindi omettere i parametri per la lettura di un singolo messaggio.

Se si desidera testare quando una coda non esiste, è possibile eliminare la coda (e tutti i dati) con l'interfaccia della riga di comando di Azure. Assicurarsi di sostituire il parametro `<connection-string>` (o impostare la variabile di ambiente).

```azurecli
az storage queue delete --name newsqueue --connection-string <connection-string> 
```

Quando si crea un nuovo messaggio, la coda dovrebbe essere ricreata.

> [!NOTE]
> L'operazione di eliminazione viene effettivamente eseguita in modo asincrono. Se non è stata completata è possibile ricevere un'eccezione quando si tenta di ricreare la coda.