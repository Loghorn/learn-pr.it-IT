<span data-ttu-id="6dd9b-101">Qui sotto si vedrà come creare un'applicazione client per lavorare con una coda.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-101">Let's create a client application to work with a queue.</span></span> <span data-ttu-id="6dd9b-102">Si aggiungerà quindi la stringa di connessione al codice.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-102">Then we'll add our connection string to the code.</span></span>

> [!NOTE]
> <span data-ttu-id="6dd9b-103">È possibile creare l'applicazione client nel computer locale, se è installato .NET Core, oppure direttamente nell'ambiente Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-103">You can create the client application on your local computer if you have .NET Core installed, or directly in the Cloud Shell environment.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="6dd9b-104">Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="6dd9b-104">Create the application</span></span>

<span data-ttu-id="6dd9b-105">Si creerà un'applicazione .NET Core che è possibile eseguire su Linux, macOS o Windows.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-105">We'll create a .NET Core application that you can run on Linux, macOS, or Windows.</span></span> <span data-ttu-id="6dd9b-106">L'applicazione verrà chiamata **QueueApp**.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-106">Let's name it **QueueApp**.</span></span> <span data-ttu-id="6dd9b-107">Per semplicità, verrà usata una sola app che invierà e riceverà messaggi tramite la coda.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-107">For simplicity, we'll use a single app that will both send and receive messages through our queue.</span></span>

1. <span data-ttu-id="6dd9b-108">Usare il comando `dotnet new` per creare una nuova app console con il nome **QueueApp**.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-108">Use the `dotnet new` command to create a new console app with the name **QueueApp**.</span></span> <span data-ttu-id="6dd9b-109">È possibile digitare i comandi in Cloud Shell a destra oppure, se si lavora in locale, in una finestra del terminale o della console.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-109">You can type commands into the Cloud Shell on the right, or if you are working locally, in a terminal/console window.</span></span> <span data-ttu-id="6dd9b-110">Questo comando crea un'app semplice con un singolo file di origine: `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-110">This command creates a simple app with a single source file: `Program.cs`.</span></span>

    ```azurecli
    dotnet new console -n QueueApp
    ```

1. <span data-ttu-id="6dd9b-111">Passare alla cartella `QueueApp` appena creata e compilare l'app per verificare che tutto funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-111">Switch to the newly created `QueueApp` folder and build the app to verify that all is well.</span></span>

    ```azurecli
    cd QueueApp
    ```

    ```azurecli
    dotnet build
    ```

## <a name="get-your-connection-string"></a><span data-ttu-id="6dd9b-112">Ottenere una stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="6dd9b-112">Get your connection string</span></span>

<span data-ttu-id="6dd9b-113">È importante ricordare che la stringa di connessione è disponibile nella sezione **Impostazioni > Chiavi di accesso** dell'account di archiviazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-113">Recall that your connection string is available in the Azure portal **Settings > Access keys** section of your storage account.</span></span>

<span data-ttu-id="6dd9b-114">È anche possibile recuperarla tramite gli strumenti dell'interfaccia della riga di comando di Azure o PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-114">You can also retrieve it through the Azure CLI or PowerShell tools.</span></span> <span data-ttu-id="6dd9b-115">Qui viene usata l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-115">Let's use the Azure CLI command.</span></span> <span data-ttu-id="6dd9b-116">Ricordare di sostituire `<name>` con il nome specifico dell'account di archiviazione creato.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-116">Remember to replace the `<name>` with the specific name of the storage account you created.</span></span> <span data-ttu-id="6dd9b-117">È possibile usare `az storage account list` se è necessario un promemoria.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-117">You can use `az storage account list` if you need a reminder.</span></span>

```azurecli
az storage account show-connection-string --name <name> --resource-group <rgn>[sandbox resource group name]</rgn>
```

<span data-ttu-id="6dd9b-118">Questo comando restituirà un blocco JSON con la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-118">This command will return a JSON block with your connection string.</span></span> <span data-ttu-id="6dd9b-119">Includerà il nome e la chiave dell'account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="6dd9b-119">It will include the storage account name and the account key:</span></span>

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=<name>;AccountKey=vyw6aKz2PtSAgQ4ljJQgJFgxbCETdXt39ZyYQ5fLqoBJj/gT+43TbrhoVco7Rqj/AAJVlvFORRfnYqGHiX9QcQ=="
}
```

## <a name="add-the-connection-string-to-the-application"></a><span data-ttu-id="6dd9b-120">Aggiungere la stringa di connessione all'applicazione</span><span class="sxs-lookup"><span data-stu-id="6dd9b-120">Add the connection string to the application</span></span>

<span data-ttu-id="6dd9b-121">Infine, aggiungere la stringa di connessione all'app in modo che possa accedere all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-121">Finally, let's add the connection string into our app so it can access the storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="6dd9b-122">Per semplicità, la stringa di connessione verrà inserita nel file **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-122">For simplicity, you will place the connection string in the **Program.cs** file.</span></span> <span data-ttu-id="6dd9b-123">In un'applicazione di produzione è necessario archiviarla in una posizione sicura.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-123">In a production application, you should store it in a secure location.</span></span> <span data-ttu-id="6dd9b-124">Per l'uso sul lato server, è consigliabile usare Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-124">For server side use, we recommend using Azure Key Vault.</span></span>

1. <span data-ttu-id="6dd9b-125">Digitare `code .` nel terminale per aprire l'editor di codice online.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-125">Type `code .` in the terminal to open the online code editor.</span></span> <span data-ttu-id="6dd9b-126">In alternativa, se si lavora in modo autonomo è possibile usare l'IDE preferito.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-126">Alternatively, if you are working on your own you can use the IDE of your choice.</span></span> <span data-ttu-id="6dd9b-127">Si consiglia Visual Studio Code, che è un eccellente IDE multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-127">We recommend Visual Studio Code, which is an excellent cross-platform IDE.</span></span>

1. <span data-ttu-id="6dd9b-128">Aprire il file di origine `Program.cs` nel progetto.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-128">Open the `Program.cs` source file in the project.</span></span>

1. <span data-ttu-id="6dd9b-129">Nella classe `Program`, aggiungere un valore `const string` per memorizzare la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-129">In the `Program` class, add a `const string` value to hold the connection string.</span></span> <span data-ttu-id="6dd9b-130">È necessario solo il valore (inizia con il testo **DefaultEndpointsProtocol**).</span><span class="sxs-lookup"><span data-stu-id="6dd9b-130">You only need the value (it starts with the text **DefaultEndpointsProtocol**).</span></span>

1. <span data-ttu-id="6dd9b-131">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-131">Save the file.</span></span> <span data-ttu-id="6dd9b-132">È possibile fare clic sui puntini di sospensione "..." nell'angolo a destra dell'editor cloud o usare i tasti di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux o <kbd>CMD+S</kbd> in macOS).</span><span class="sxs-lookup"><span data-stu-id="6dd9b-132">You can click the ellipse "..." in the right corner of the cloud editor, or use the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

<span data-ttu-id="6dd9b-133">Il codice dovrebbe essere simile al seguente (il valore della stringa sarà univoco per l'account).</span><span class="sxs-lookup"><span data-stu-id="6dd9b-133">Your code should look something like this (the string value will be unique to your account).</span></span>

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

<span data-ttu-id="6dd9b-134">Con questa configurazione iniziale del progetto, sarà possibile esaminare come lavorare con una coda nel codice.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-134">Now that we have this starter project setup, let's look at how to work with a queue in code.</span></span> <span data-ttu-id="6dd9b-135">Tutto inizia con i _messaggi_.</span><span class="sxs-lookup"><span data-stu-id="6dd9b-135">It all starts with _messages_.</span></span>