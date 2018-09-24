Qui sotto si vedrà come creare un'applicazione client per lavorare con una coda. Si aggiungerà quindi la stringa di connessione al codice.

> [!NOTE]
> È possibile creare l'applicazione client nel computer locale, se è installato .NET Core, oppure direttamente nell'ambiente Cloud Shell.

## <a name="create-the-application"></a>Creare l'applicazione

Si creerà un'applicazione .NET Core che è possibile eseguire su Linux, macOS o Windows. L'applicazione verrà chiamata **QueueApp**. Per semplicità, verrà usata una sola app che invierà e riceverà messaggi tramite la coda.

1. Usare il comando `dotnet new` per creare una nuova app console con il nome **QueueApp**. È possibile digitare i comandi in Cloud Shell a destra oppure, se si lavora in locale, in una finestra del terminale o della console. Questo comando crea un'app semplice con un singolo file di origine: `Program.cs`.

    ```azurecli
    dotnet new console -n QueueApp
    ```

1. Passare alla cartella `QueueApp` appena creata e compilare l'app per verificare che tutto funzioni correttamente.

    ```azurecli
    cd QueueApp
    ```

    ```azurecli
    dotnet build
    ```

## <a name="get-your-connection-string"></a>Ottenere una stringa di connessione

È importante ricordare che la stringa di connessione è disponibile nella sezione **Impostazioni > Chiavi di accesso** dell'account di archiviazione nel portale di Azure.

È anche possibile recuperarla tramite gli strumenti dell'interfaccia della riga di comando di Azure o PowerShell. Qui viene usata l'interfaccia della riga di comando di Azure. Ricordare di sostituire `<name>` con il nome specifico dell'account di archiviazione creato. È possibile usare `az storage account list` se è necessario un promemoria.

```azurecli
az storage account show-connection-string --name <name> --resource-group <rgn>[Sandbox resource group name]</rgn>
```

Questo comando restituirà un blocco JSON con la stringa di connessione. Includerà il nome e la chiave dell'account di archiviazione:

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=<name>;AccountKey=vyw6aKz2PtSAgQ4ljJQgJFgxbCETdXt39ZyYQ5fLqoBJj/gT+43TbrhoVco7Rqj/AAJVlvFORRfnYqGHiX9QcQ=="
}
```

## <a name="add-the-connection-string-to-the-application"></a>Aggiungere la stringa di connessione all'applicazione

Infine, aggiungere la stringa di connessione all'app in modo che possa accedere all'account di archiviazione.

> [!WARNING]
> Per semplicità, la stringa di connessione verrà inserita nel file **Program.cs**. In un'applicazione di produzione è necessario archiviarla in una posizione sicura. Per l'uso sul lato server, è consigliabile usare Azure Key Vault.

1. Digitare `code .` nel terminale per aprire l'editor di codice online. In alternativa, se si lavora in modo autonomo è possibile usare l'IDE preferito. Si consiglia Visual Studio Code, che è un eccellente IDE multipiattaforma.

1. Aprire il file di origine `Program.cs` nel progetto.

1. Nella classe `Program`, aggiungere un valore `const string` per memorizzare la stringa di connessione. È necessario solo il valore (inizia con il testo **DefaultEndpointsProtocol**).

1. Salvare il file. È possibile fare clic sui puntini di sospensione "..." nell'angolo a destra dell'editor cloud o usare i tasti di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux o <kbd>CMD+S</kbd> in macOS).

Il codice dovrebbe essere simile al seguente (il valore della stringa sarà univoco per l'account).

```csharp
...
namespace QueueApp
{
    class Program
    {
        private const string ConnectionString = "DefaultEndpointsProtocol=https; ...";
        
        ...
    }
}
```

Con questa configurazione iniziale del progetto, sarà possibile esaminare come lavorare con una coda nel codice. Tutto inizia con i _messaggi_.