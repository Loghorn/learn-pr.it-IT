Vi sono casi quando è necessario garantire che più operazioni vengono eseguite insieme. Ad esempio, nell'applicazione di messaggistica istantanea, gli utenti possono inviare una singola immagine, un messaggio di testo singolo o un messaggio di testo e immagine insieme. Quando l'utente sceglie di inviare un messaggio di testo e immagine tra loro, è necessario assicurarsi che gli altri membri del gruppo di riceveranno nello stesso momento. Questo è importante perché se un messaggio di immagine e testo non viene ricevuto tra loro, è possibile che tra il messaggio di testo e immagine è stato possibile inviare un messaggio separato. Che è stato possibile orientare la discussione generale confusione.

In questo caso, esamineremo come creare una transazione in Cache Redis di Azure per garantire che più operazioni vengono eseguite contemporaneamente.

## <a name="how-to-create-a-transaction"></a>Come creare una transazione

Per creare una transazione, è necessario disporre di un blocco di transazioni. Si tratta di una coda che contiene i comandi che verranno eseguiti insieme. Il `MULTI` comando viene usato per creare un blocco della transazione e tutti i comandi successivi vengono accodati per l'esecuzione atomica.

## <a name="how-to-execute-a-transaction"></a>Come eseguire una transazione

Per eseguire i comandi in un blocco della transazione, si utilizza il `EXEC` comando. Questo sarà eseguire tutti i comandi in coda e ripristinare lo stato di connessione alla normalità. Se si decide che non si vuole eseguire una transazione, è possibile usare il `DISCARD` comando, che cancellerà il blocco delle transazioni e impostare lo stato di connessione alla normalità.

Una cosa importante da capire le transazioni di Cache Redis di Azure è che non supportano i ripristini. Ciò significa che, se un comando all'interno di una transazione non riesce, verranno eseguiti comunque i comandi rimanenti.

## <a name="what-is-servicestackredis"></a>Che cos'è ServiceStack.Redis?

**ServiceStack.Redis** è una libreria di client in c# per l'interazione con Cache Redis di Azure. Ciò significa che invece di usare i comandi di Cache Redis di Azure a basso livello, è possibile usare i metodi e classi c#. Ad esempio, invece di usare la `MULTI` e `EXEC` comandi da eseguire una transazione, con **ServiceStack.Redis** creiamo un `IRedisTransaction` dell'oggetto usando il `CreateTransaction()` (metodo).

```csharp
var transaction = redisClient.CreateTransaction();
```

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a>Creare una transazione con c# e il client ServiceStack.Redis

Ecco un esempio d'uso **ServiceStack.Redis** per creare una transazione che può inviare un messaggio che include un'immagine e testo.

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create the transaction
        var transaction = redisClient.CreateTransaction();

        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        var transactionResult = transaction.Commit();

        return transactionResult;
    }
}
```
Le transazioni garantiscono che più operazioni vengono eseguite contemporaneamente. Si crea una transazione che utilizza il `MULTI` comando. Se si usa una libreria client, ad esempio **ServiceStack.Redis**, è possibile usare il `CreateTransaction()` (metodo).