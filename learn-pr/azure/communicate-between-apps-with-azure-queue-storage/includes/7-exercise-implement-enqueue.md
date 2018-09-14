Ora che tutti i requisiti sono soddisfatti, è possibile scrivere codice per creare una nuova coda di archiviazione e aggiungere un messaggio. In genere, questo codice viene inserito nelle app front-end che generano i dati.

## <a name="add-the-client-library-for-azure-storage"></a>Aggiungere la libreria client di Archiviazione di Azure

Per iniziare, aggiungere la **libreria client Archiviazione di Azure per .NET** all'app. È possibile installarla con NuGet (uno strumento di gestione pacchetti .NET). 

> [!NOTE]
> Anche se è stata creata una nuova Cloud Shell, i file sono ancora presenti. Tuttavia è necessario passare alla cartella `QueueApp` poiché ora ci si trova nella radice dell'area di archiviazione. È sufficiente digitare `cd QueueApp` per passare da una cartella all'altra. Assicurarsi di eseguire questa operazione prima di aggiungere il pacchetto, poiché l'app .NET si trova in tale cartella.

1. Modificare la directory corrente alla cartella `QueueApp`.

1. Installare il pacchetto `WindowsAzure.Storage` sul progetto con il comando `dotnet add package`.

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a>Aggiungere un metodo per inviare un avviso sulle notizie

È possibile creare un nuovo metodo per l'invio di una storia di notizie in una coda.

1. Aprire l'editor di codice digitando `code .`

1. Aprire il file `Program.cs`.

1. Aggiungere gli spazi dei nomi seguenti all'inizio del file. Verranno usati tipi di entrambi gli spazi per connettersi ad Archiviazione di Azure e quindi lavorare con le code.

    - System.threading.task
    - Microsoft.WindowsAzure.Storage
    - Microsoft.WindowsAzure.Storage.Queue

1. Creare un metodo statico nella classe `Program` denominato `SendArticleAsync` che accetta un `string` e restituisce `Task`. Questo metodo verrà usato per inviare un articolo al servizio. Assegnare un nome al parametro di input `newsMesasage` come illustrato di seguito.

    ```csharp
    ...
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue; 
    
    class Program
    {
        ...
    
        static async Task SendArticleAsync(string newsMessage)
        {
        }
    }
    ```
    
1. Nel nuovo metodo, usare il metodo statico `CloudStorageAccount.Parse` per analizzare la stringa di connessione (tenere presente che è stata inserita in una stringa costante). Questo metodo restituisce un oggetto `CloudStorageAccount` che deve essere archiviato in una variabile.

1. Chiamare il metodo `CreateCloudQueueClient()` nell'oggetto account di archiviazione per ottenere un oggetto client e archiviarlo in una variabile.

1. Successivamente, chiamare il metodo `GetQueueReference` sull'oggetto client e passare la stringa `"newsqueue"` per il nome della coda. In questo modo verrà restituito un oggetto `CloudQueue` che è possibile usare per lavorare con la coda. Non importa se la coda non esiste ancora.

1. Chiamare `CreateIfNotExistsAsync()` sull'oggetto `CloudQueue` per verificare che la coda sia pronta per l'uso. Verrà creata la coda se necessario.
    - Poiché si tratta di un metodo asincrono, usare la parola chiave `await` di C# per verificarne il corretto funzionamento. Inoltre, sarà necessario decorare il _metodo_ con la parola chiave `async`. 
    - `CreateIfNotExistsAsync` restituisce il valore `bool` che diventerà `true` se la coda è stata creata, oppure `false` se esiste già. Inviare un messaggio alla console se la coda è stata creata.
    - L'esempio seguente fornisce supporto.

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        // Not Shown here - code from prior steps
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. Creare un'istanza di un `CloudQueueMessage`. 
    - Richiede un parametro `string`, quindi passare il parametro del metodo (`newsMessage`). Questo sarà il _corpo_ del messaggio. È inoltre disponibile un metodo statico denominato che può creare un payload del messaggio binario.
    

1. Chiamare `AddMessageAsync` sull'oggetto `CloudQueue` per aggiungere il messaggio alla coda. Questo è anche un metodo asincrono e sarà necessario usare la parola chiave `await` per verificarne la corretta interazione con l'utente.

1. Salvare il file e compilarlo digitando `dotnet build` nella finestra di comando. Risolvere eventuali errori. È possibile usare il codice seguente per controllare il lavoro.

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference(QueueName);
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a>Aggiungere codice per inviare un messaggio

Si modificherà il metodo `Main` per passare i parametri ricevuti nel nuovo metodo, in modo che sia possibile testare il nuovo metodo di invio.

1. Individuare il metodo `Main`.

1. Controllare il parametro passato `args` per verificare se alcuni dati sono stati passati alla riga di comando. In caso affermativo, usare `String.Join` per creare una singola stringa con tutte le parole usando uno spazio come separatore.

1. Passare al nuovo metodo `SendArticleAsync`. 

1. Una volta che viene restituito, usare `Console.WriteLine` per inviare il messaggio inviato.

1. Salvare il file e compilare il programma (`dotnet build`). È possibile usare il codice seguente per controllare il lavoro.

> [!NOTE]
> Poiché il metodo è tecnicamente asincrono, in genere è preferibile usare la parola chiave `await`, tuttavia questa funzionalità non è disponibile nel metodo `Main` a meno che non si usi C# 7.2 (si tratta di una nuova funzionalità). Per assicurarsi di poterla usare nelle versioni precedenti del linguaggio, è sufficiente chiamare `Wait()` sul metodo per evitare di attendere la restituzione del metodo.

    ```csharp
    static void Main(string[] args)
    {
        if (args.Length > 0)
        {
            string value = String.Join(" ", args);
            SendArticleAsync(value).Wait();
            Console.WriteLine($"Sent: {value}");
        }
    }
    ```

## <a name="execute-the-application"></a>Eseguire l'applicazione

L'applicazione può ora inviare messaggi. Per testarla, è possibile eseguirla dalla riga di comando con il comando `dotnet run`. Eventuali stringhe aggiuntive vengono passate all'applicazione come parametri. Ad esempio, è possibile digitare:

    ```azurecli
    dotnet run Send this message
    ```

> [!WARNING]
> Assicurarsi di aver salvato tutti i file nell'editor online prima di compilare ed eseguire il programma.

In questo modo, la stringa `"Send this message"` dovrebbe venire aggiunta alla coda.

## <a name="check-your-results"></a>Controllare i risultati

È possibile controllare le code nel portale di Azure usando **Storage Explorer**. Se si apre questo prodotto, sarà possibile esplorare i dati in tutti gli account di archiviazione di cui si è proprietari.

In alternativa, è possibile usare l'interfaccia della riga di comando di Azure o PowerShell. Provare a eseguire questo comando nella shell, sostituendo il valore `<connection-string>` con la stringa di connessione specifica:

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

Ciò dovrebbe avviare il dump delle informazioni per il messaggio, che avrà un aspetto simile al seguente:

```json
[
  {
    "content": "U2VuZCB0aGlzIG1lc3NhZ2U=",
    "dequeueCount": 0,
    "expirationTime": "2018-09-05T02:43:40+00:00",
    "id": "b47cbe9f-a246-4a81-86ae-fa02ea8d98bc",
    "insertionTime": "2018-09-24T02:43:40+00:00",
    "popReceipt": null,
    "timeNextVisible": null
  }
]
```

Sono disponibili numerosi altri comandi che è possibile provare con gli strumenti; eseguire il checkout di `az storage queue --help` e `az storage message --help` per esplorarli.

> [!TIP]
> Inserire il valore della stringa di connessione in una variabile di ambiente denominata `AZURE_STORAGE_CONNECTION_STRING` per non dover digitare ogni volta il parametro `--connection-string`.

Ora che il messaggio è stato inviato, l'ultimo passaggio consiste nell'aggiungere il supporto per la _ricezione_ del messaggio.