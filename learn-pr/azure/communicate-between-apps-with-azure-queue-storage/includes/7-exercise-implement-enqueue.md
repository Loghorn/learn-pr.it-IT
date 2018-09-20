Ora che tutti i requisiti sono soddisfatti, è possibile scrivere codice per creare una nuova coda di archiviazione e aggiungere un messaggio. In genere, questo codice viene inserito nelle app front-end che generano i dati.

## <a name="add-the-client-library-for-azure-storage"></a>Aggiungere la libreria client di Archiviazione di Azure

Per iniziare, aggiungere la **libreria client Archiviazione di Azure per .NET** all'app. È possibile installarla con NuGet (uno strumento di gestione pacchetti .NET). 

1. Installare il pacchetto `WindowsAzure.Storage` nel progetto con il comando `dotnet add package`. È possibile eseguire questa operazione nella finestra del terminale _sotto_ Cloud Shell oppure, se si sta usando il computer locale, in una finestra del terminale o della console nella stessa cartella del progetto.

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a>Aggiungere un metodo per inviare un avviso di notizia

È possibile creare un nuovo metodo per l'invio di una notizia in una coda.

1. Aprire il file `Program.cs` nell'editor di codice.

1. Aggiungere gli spazi dei nomi seguenti all'inizio del file. Verranno usati tipi di entrambi gli spazi per connettersi ad Archiviazione di Azure e quindi usare le code.

    - `System.Threading.Tasks`
    - `Microsoft.WindowsAzure.Storage`
    - `Microsoft.WindowsAzure.Storage.Queue`

1. Creare un metodo statico nella classe `Program` denominato `SendArticleAsync` che accetta `string` e restituisce `Task`. Questo metodo verrà usato per inviare un articolo al servizio. Assegnare un nome al parametro di input `newsMessage` come illustrato di seguito.

    ```csharp
    ...
    using System.Threading.Tasks;
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

1. Successivamente, chiamare il metodo `GetQueueReference` sull'oggetto client e passare la stringa "newsqueue" per il nome della coda. In questo modo verrà restituito un oggetto `CloudQueue` che è possibile usare per lavorare con la coda. Non importa se la coda non esiste ancora.

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
    
        CloudQueue queue = queueClient.GetQueueReference("newsqueue");
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
    > Poiché il metodo è tecnicamente asincrono, è preferibile usare la parola chiave `await`, tuttavia questa funzionalità non è disponibile nel metodo `Main` a meno che non si usi C# 7.1 o versione successiva. Chiamare `Wait()` sul metodo per bloccare l'attesa della restituzione del metodo. Questo problema verrà risolto tra poco.

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

L'applicazione può ora inviare messaggi. Per testarla, è possibile eseguirla dalla riga di comando con il comando `dotnet run`. Eventuali stringhe aggiuntive vengono passate all'applicazione come parametri. 

> [!WARNING]
> Assicurarsi di aver salvato tutti i file nell'editor online prima di compilare ed eseguire il programma.

Ad esempio, è possibile digitare:

```azurecli
dotnet run Send this message
```

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

## <a name="optional---use-the-async-versions-of-the-methods"></a>Facoltativo: usare le versioni asincrone dei metodi

Sul metodo send precedente è stato usato `Wait()`, che non corrisponde tuttavia a un uso efficiente delle risorse di calcolo. È preferibile usare invece i metodi `async` e `await` di C#. Sarà tuttavia necessario usare _almeno_ C# 7.1 per poter applicare queste parole chiave al metodo **Main**.

### <a name="switch-to-c-71"></a>Passare a C# 7.1

Le parole chiave `async` e `await` di C# non sono parole chiave valide nei metodi **Main** nelle versioni precedenti a C# 7.1. È possibile passare con facilità a questo compilatore tramite un flag nel file con estensione **csproj**.

1. Aprire il file **QueueApp.csproj** nell'editor.

1. Aggiungere `<LangVersion>7.1</LangVersion>` nel primo elemento `PropertyGroup` del file di compilazione. Al termine il risultato dovrebbe essere simile al seguente.
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a>Applicare la parola chiave async

Applicare quindi la parola chiave `async` al metodo **Main**. Sarà necessario eseguire tre operazioni.

1. Aggiungere la parola chiave `async` alla firma del metodo **Main**.
1. Cambiare il tipo restituito da `void` a `Task`.
1. Rimuovere la chiamata a `Wait()` nella chiamata a `SendArticleAsync` e sostituirla con `await`.

Provare a eseguire di nuovo l'app. Dovrebbe funzionare esattamente come prima, ma ora il codice è in grado di rilasciare il thread al pool di thread .NET durante l'attesa dell'inserimento del messaggio nella coda.

Ora che il messaggio è stato inviato, l'ultimo passaggio consiste nell'aggiungere il supporto per la _ricezione_ del messaggio.