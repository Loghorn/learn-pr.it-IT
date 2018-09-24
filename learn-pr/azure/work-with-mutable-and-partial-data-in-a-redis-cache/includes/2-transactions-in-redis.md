A volte è necessario garantire che più operazioni vengano eseguite insieme. Ad esempio, in un'applicazione di messaggistica istantanea, gli utenti possono inviare solo un'immagine, solo un messaggio di testo oppure un messaggio di testo e un'immagine insieme. Quando l'utente sceglie di inviare insieme un'immagine e un messaggio di testo, è necessario assicurarsi che gli altri membri del gruppo li ricevano nello stesso momento. Questo è importante perché se l'immagine e il messaggio di testo non vengono ricevuti insieme, è possibile che venga inviato un altro messaggio che si interpone tra l'immagine e il messaggio di testo. Ciò potrebbe generare confusione nella conversazione generale.

In questa unità si vedrà come creare una transazione in Cache Redis di Azure per garantire che più operazioni vengano eseguite contemporaneamente.

## <a name="creating-and-running-transactions"></a>Creazione ed esecuzione di transazioni

Le transazioni in Redis funzionano accodando più comandi da eseguire come gruppo. Quando viene eseguita una transazione, i comandi in coda all'interno della transazione vengono sempre eseguiti senza che si interpongano altri comandi di altri client.

Per avviare un blocco di transazione, immettere il comando `MULTI`. Ulteriori comandi verranno accodati e non eseguiti immediatamente. Eseguendo il comando `EXEC`, vengono eseguiti tutti i comandi in coda come un'unità transazionale. Se si decide di interrompere una transazione aperta mentre si accodano i comandi, l'esecuzione del comando `DISCARD` consente di chiudere il blocco di transazione senza eseguire _nessuno_ dei comandi in coda.

Le transazioni Redis non supportano il concetto di ripristino dello stato precedente. Se si accoda un comando con sintassi non corretta in un blocco di transazione, il blocco rimane aperto, ma viene automaticamente rimosso se si cerca di eseguirlo con `EXEC`. I comandi in una transazione che ha esito negativo _durante_ l'esecuzione (dopo la chiamata di `EXEC`) non provocano l'interruzione o il rollback di una transazione: Redis esegue comunque tutti i comandi e considera la transazione completata correttamente.

## <a name="redis-transactions-with-servicestackredis"></a>Transazioni Redis con ServiceStack.Redis

**ServiceStack.Redis** è una libreria client C# per l'interazione con Cache Redis di Azure.

Le transazioni in ServiceStack.Redis vengono create chiamando `IRedisClient.CreateTransaction()`. L'oggetto `IRedisTransaction` restituito può avere più comandi accodati con `QueueCommand()`. La chiamata di `Commit()` sull'oggetto transazione comporta la sua esecuzione.

Gli oggetti `IRedisTransaction` possono essere eliminati e viene inviato automaticamente un comando `DISCARD` se l'eliminazione avviene prima di chiamare `Commit()`. Questa funzionalità è ideale con i blocchi `using` C#: se non si esegue il commit di una transazione per qualsiasi motivo, è possibile avere la certezza che la transazione verrà rimossa automaticamente in modo che sia possibile continuare a usare la connessione Redis.

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a>Creare una transazione con C# e il client ServiceStack.Redis

Ecco un esempio dell'uso di ServiceStack.Redis per creare una transazione che può inviare un messaggio che include un URL di un'immagine e i contenuti di un messaggio di testo.

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    return transactionResult;
}
```

Le transazioni garantiscono che più operazioni vengano eseguite insieme senza interposizione di operazioni di altri client. È possibile creare una transazione usando il comando `MULTI`. Con ServiceStack.Redis, si usa il metodo `CreateTransaction()`.