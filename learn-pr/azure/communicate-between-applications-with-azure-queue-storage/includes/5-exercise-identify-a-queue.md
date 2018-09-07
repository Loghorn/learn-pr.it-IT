<span data-ttu-id="8b845-101">Qui sotto si vedrà come creare un'applicazione client per lavorare con una coda.</span><span class="sxs-lookup"><span data-stu-id="8b845-101">Let's create a client application to work with a queue.</span></span> <span data-ttu-id="8b845-102">Quindi, la stringa di connessione verrà aggiunta al codice.</span><span class="sxs-lookup"><span data-stu-id="8b845-102">Then we'll add our connection string to the code.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="8b845-103">Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="8b845-103">Create the application</span></span>

<span data-ttu-id="8b845-104">Si creerà un'applicazione .NET Core che è possibile eseguire su Linux, macOS o Windows.</span><span class="sxs-lookup"><span data-stu-id="8b845-104">We'll create a .NET Core application that you can run on Linux, macOS, or Windows.</span></span> <span data-ttu-id="8b845-105">L'applicazione verrà chiamata **QueueApp**.</span><span class="sxs-lookup"><span data-stu-id="8b845-105">Let's name it **QueueApp**.</span></span> <span data-ttu-id="8b845-106">Per semplicità, verrà usata una sola app che invierà e riceverà messaggi tramite la coda.</span><span class="sxs-lookup"><span data-stu-id="8b845-106">For simplicity, we'll use a single app that will both send and receive messages through our queue.</span></span>

1. <span data-ttu-id="8b845-107">Usare il comando `dotnet new` per creare una nuova app console con il nome **QueueApp**.</span><span class="sxs-lookup"><span data-stu-id="8b845-107">Use the `dotnet new` command to create a new console app with the name **QueueApp**.</span></span> <span data-ttu-id="8b845-108">È possibile digitare i comandi in Cloud Shell a destra, oppure se si lavora in locale, in una finestra del terminale o della console.</span><span class="sxs-lookup"><span data-stu-id="8b845-108">You can type commands into the Cloud Shell on the right, or if you are working locally, in a terminal/console window.</span></span> <span data-ttu-id="8b845-109">Verrà creata un'app semplice con un singolo file di origine: `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="8b845-109">This creates a simple app with a single source file: `Program.cs`.</span></span>

    ```azurecli
    dotnet new console -n QueueApp
    ```

2. <span data-ttu-id="8b845-110">Passare alla cartella `QueueApp` appena creata e compilare l'app per verificare che tutto funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="8b845-110">Switch to the newly created `QueueApp` folder and build the app to verify that all is well.</span></span>

    ```azurecli
    cd QueueApp
    ```

    ```azurecli
    dotnet build
    ```

## <a name="get-your-connection-string"></a><span data-ttu-id="8b845-111">Ottenere una stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="8b845-111">Get your connection string</span></span>

<span data-ttu-id="8b845-112">È importante ricordare che la stringa di connessione è disponibile nella sezione **Impostazioni > Chiavi di accesso** dell'account di archiviazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="8b845-112">Recall that your connection string is available in the Azure portal **Settings > Access keys** section of your storage account.</span></span>

<span data-ttu-id="8b845-113">È anche possibile recuperarla tramite gli strumenti interfaccia della riga di comando di Azure o Powershell.</span><span class="sxs-lookup"><span data-stu-id="8b845-113">You can also retrieve it through the Azure CLI or Powershell tools.</span></span> <span data-ttu-id="8b845-114">Verrà usata l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="8b845-114">Let's use the Azure CLI command.</span></span> <span data-ttu-id="8b845-115">Ricordare di sostituire `<name>` con il nome specifico dell'account di archiviazione creato.</span><span class="sxs-lookup"><span data-stu-id="8b845-115">Remember to replace the `<name>` with the specific name of the storage account you created.</span></span> <span data-ttu-id="8b845-116">È possibile usare `az storage account list` se è necessario un promemoria.</span><span class="sxs-lookup"><span data-stu-id="8b845-116">You can use `az storage account list` if you need a reminder.</span></span>

```azurecli
az storage account show-connection-string --name <name> --resource-group ExerciseResources
```

<span data-ttu-id="8b845-117">Questo comando restituirà un blocco JSON con la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="8b845-117">This command will return a JSON block with your connection string.</span></span> <span data-ttu-id="8b845-118">Includerà il nome e la chiave dell'account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="8b845-118">It will include the storage account name and the account key:</span></span>

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=<name>;AccountKey=vyw6aKz2PtSAgQ4ljJQgJFgxbCETdXt39ZyYQ5fLqoBJj/gT+43TbrhoVco7Rqj/AAJVlvFORRfnYqGHiX9QcQ=="
}
```

## <a name="add-the-connection-string-to-the-application"></a><span data-ttu-id="8b845-119">Aggiungere la stringa di connessione all'applicazione</span><span class="sxs-lookup"><span data-stu-id="8b845-119">Add the connection string to the application</span></span>

<span data-ttu-id="8b845-120">Infine, aggiungere la stringa di connessione all'app in modo che possa accedere all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="8b845-120">Finally, let's add the connection string into our app so it can access the storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="8b845-121">Per semplicità, la stringa di connessione verrà inserita nel file **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="8b845-121">For simplicity, you will place the connection string in the **Program.cs** file.</span></span> <span data-ttu-id="8b845-122">In un'applicazione di produzione è necessario archiviarla in una posizione sicura.</span><span class="sxs-lookup"><span data-stu-id="8b845-122">In a production application, you should store it in a secure location.</span></span> <span data-ttu-id="8b845-123">Per l'uso sul lato server, è consigliabile usare Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="8b845-123">For server side use, we recommend using Azure Key Vault.</span></span>

1. <span data-ttu-id="8b845-124">Digitare `code .` nel terminale per aprire l'editor di codice online.</span><span class="sxs-lookup"><span data-stu-id="8b845-124">Type `code .` in the terminal to open the online code editor.</span></span> <span data-ttu-id="8b845-125">In alternativa, se si lavora in modo autonomo è possibile usare l'IDE preferito.</span><span class="sxs-lookup"><span data-stu-id="8b845-125">Alternatively, if you are working on your own you can use the IDE of your choice.</span></span> <span data-ttu-id="8b845-126">Si consiglia Visual Studio Code, che è un eccellente IDE multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="8b845-126">We recommend Visual Studio Code, which is an excellent cross-platform IDE.</span></span>

2. <span data-ttu-id="8b845-127">Aprire il file di origine `Program.cs` nel progetto.</span><span class="sxs-lookup"><span data-stu-id="8b845-127">Open the `Program.cs` source file in the project.</span></span>

3. <span data-ttu-id="8b845-128">Nella classe `Program`, aggiungere un valore `const string` per memorizzare la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="8b845-128">In the `Program` class, add a `const string` value to hold the connection string.</span></span> <span data-ttu-id="8b845-129">È necessario solo il valore (inizia con il testo **DefaultEndpointsProtocol**).</span><span class="sxs-lookup"><span data-stu-id="8b845-129">You only need the value (it starts with the text **DefaultEndpointsProtocol**).</span></span>

4. <span data-ttu-id="8b845-130">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="8b845-130">Save the file.</span></span> <span data-ttu-id="8b845-131">È possibile fare clic sui puntini di sospensione "..." nell'angolo a destra dell'editor del cloud. Verranno visualizzati anche i tasti di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="8b845-131">You can click the ellipse "..." in the right corner of the cloud editor, it will also show you the keyboard shortcut.</span></span>

<span data-ttu-id="8b845-132">Il codice dovrebbe essere simile al seguente (il valore della stringa sarà univoco per l'account).</span><span class="sxs-lookup"><span data-stu-id="8b845-132">Your code should look something like this (the string value will be unique to your account).</span></span>

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

<span data-ttu-id="8b845-133">Con questa configurazione iniziale del progetto, sarà possibile esaminare come lavorare con una coda nel codice.</span><span class="sxs-lookup"><span data-stu-id="8b845-133">Now that we have this starter project setup, let's look at how to work with a queue in code.</span></span> <span data-ttu-id="8b845-134">Tutto inizia con i _messaggi_.</span><span class="sxs-lookup"><span data-stu-id="8b845-134">It all starts with _messages_.</span></span>