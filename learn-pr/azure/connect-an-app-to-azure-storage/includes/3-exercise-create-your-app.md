<span data-ttu-id="81a51-101">Stiamo lavorando su un'applicazione di condivisione foto che userà Archiviazione di Azure per gestire le immagini e gli altri dati archiviati per conto degli utenti.</span><span class="sxs-lookup"><span data-stu-id="81a51-101">Recall that we are working on a photo-sharing application that will use Azure Storage to manage pictures and other bits of data we store on behalf of our users.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="81a51-102">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="81a51-102">::: zone pivot="csharp"</span></span>

<span data-ttu-id="81a51-103">Per semplificare questo scenario, in modo da potersi concentrare sulle API di archiviazione, si creerà una nuova applicazione console .NET Core.</span><span class="sxs-lookup"><span data-stu-id="81a51-103">To simplify our scenario so that we can focus on the Storage APIs, we will create a new .NET Core Console application.</span></span> <span data-ttu-id="81a51-104">Si presupporrà anche che per l'applicazione sia sempre presente la connettività di rete.</span><span class="sxs-lookup"><span data-stu-id="81a51-104">We will also assume it always has network connectivity.</span></span> <span data-ttu-id="81a51-105">Tuttavia, è sempre necessario rafforzare l'app per assicurarsi che gli errori di rete non incidano sull'esperienza utente e non causino errori dell'applicazione stessa.</span><span class="sxs-lookup"><span data-stu-id="81a51-105">However, you should always harden your app to ensure network failures will not impact the user experience or result in a failure of the application itself.</span></span>

## <a name="create-a-net-core-application"></a><span data-ttu-id="81a51-106">Creare un'applicazione .NET Core</span><span class="sxs-lookup"><span data-stu-id="81a51-106">Create a .NET Core application</span></span>

<span data-ttu-id="81a51-107">.NET Core è una versione multipiattaforma di .NET che è possibile eseguire in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="81a51-107">.NET Core is a cross-platform version of .NET that runs on macOS, Windows, and Linux.</span></span> <span data-ttu-id="81a51-108">Si possono installare gli strumenti in locale o in alternativa si può usare Cloud Shell sul lato destro della finestra per eseguire i passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="81a51-108">You can install the tools locally or use the Cloud Shell on the right side of the window to execute the below steps below.</span></span>

1. <span data-ttu-id="81a51-109">Accedere a Cloud Shell o aprire una sessione della riga di comando e creare una nuova applicazione console .NET Core denominata "PhotoSharingApp".</span><span class="sxs-lookup"><span data-stu-id="81a51-109">Sign into the Cloud Shell or open a command-line session and create a new .NET Core Console application with the name "PhotoSharingApp".</span></span> <span data-ttu-id="81a51-110">È possibile aggiungere il flag `-o` o `--output` per creare l'app in una cartella specifica.</span><span class="sxs-lookup"><span data-stu-id="81a51-110">You can add the `-o` or `--output` flag to create the app in a specific folder.</span></span>

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. <span data-ttu-id="81a51-111">Eseguire l'app per assicurarsi che venga compilata e funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="81a51-111">Run the app to make sure it builds and executes correctly.</span></span> <span data-ttu-id="81a51-112">Deve visualizzare "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="81a51-112">It should display "Hello World!"</span></span> <span data-ttu-id="81a51-113">sulla console.</span><span class="sxs-lookup"><span data-stu-id="81a51-113">to the console.</span></span>

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
<span data-ttu-id="81a51-114">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="81a51-114">::: zone-end</span></span>

<span data-ttu-id="81a51-115">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="81a51-115">::: zone pivot="javascript"</span></span>

<span data-ttu-id="81a51-116">Per semplificare questo scenario, in modo da potersi concentrare sulle API di archiviazione, si creerà una nuova applicazione Node.js eseguibile dalla console.</span><span class="sxs-lookup"><span data-stu-id="81a51-116">To simplify our scenario so that we can focus on the Storage APIs, we will create a new Node.js application that can run from the console.</span></span> <span data-ttu-id="81a51-117">Si presupporrà anche che l'applicazione disponga sempre di connettività di rete.</span><span class="sxs-lookup"><span data-stu-id="81a51-117">We will also assume it always has network connectivity.</span></span> <span data-ttu-id="81a51-118">Tuttavia, è sempre necessario rafforzare l'app per assicurarsi che gli errori di rete non incidano sull'esperienza utente e non risultino in un errore dell'applicazione stessa.</span><span class="sxs-lookup"><span data-stu-id="81a51-118">However, you should always harden your app to ensure network failures will not impact the user experience, or result in a failure of the application itself.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="81a51-119">Creare un'applicazione Node.js</span><span class="sxs-lookup"><span data-stu-id="81a51-119">Create a Node.js application</span></span>

<span data-ttu-id="81a51-120">Node.js è un framework diffuso per l'esecuzione di app JavaScript.</span><span class="sxs-lookup"><span data-stu-id="81a51-120">Node.js is a popular framework for running JavaScript apps.</span></span> <span data-ttu-id="81a51-121">Viene in genere usato per le app Web, ma si può anche usare per eseguire logica dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="81a51-121">It is most commonly used for web apps, but you can use it to run logic from the command line as well.</span></span> <span data-ttu-id="81a51-122">Con gli strumenti installati in locale, è possibile eseguire i passaggi seguenti da una riga di comando.</span><span class="sxs-lookup"><span data-stu-id="81a51-122">If you have the tools installed locally, you can run the following steps from a command line.</span></span> <span data-ttu-id="81a51-123">In alternativa, è possibile usare Cloud Shell sul lato destro della finestra.</span><span class="sxs-lookup"><span data-stu-id="81a51-123">Alternatively, you can use the Cloud Shell on the right side of the window to execute the below steps.</span></span>

1. <span data-ttu-id="81a51-124">Accedere a Cloud Shell o aprire una sessione della riga di comando e creare una nuova cartella denominata "PhotoSharingApp".</span><span class="sxs-lookup"><span data-stu-id="81a51-124">Sign into the Cloud Shell or open a command-line session and create a new folder named "PhotoSharingApp".</span></span>

    ```bash
    mkdir PhotoSharingApp
    ```

1. <span data-ttu-id="81a51-125">Passare alla nuova cartella e usare `npm` per inizializzare una nuova app Node.js.</span><span class="sxs-lookup"><span data-stu-id="81a51-125">Change into the new folder and use `npm` to initialize a new Node.js app.</span></span> <span data-ttu-id="81a51-126">In questo modo viene creato un file **package.json** contenente i metadati che descrivono l'app.</span><span class="sxs-lookup"><span data-stu-id="81a51-126">This will create a **package.json** file containing metadata that describes the app.</span></span>

    ```bash
    cd PhotoSharingApp
    npm init -y
    ```

1. <span data-ttu-id="81a51-127">Creare un nuovo file di origine **index.js** in cui salvare il codice.</span><span class="sxs-lookup"><span data-stu-id="81a51-127">Create a new source file **index.js**, which is where our code will go.</span></span>

    ```bash
    touch index.js
    ```

1. <span data-ttu-id="81a51-128">Aprire il file **index.js** con un editor.</span><span class="sxs-lookup"><span data-stu-id="81a51-128">Open the **index.js** file with an editor.</span></span> <span data-ttu-id="81a51-129">Se si usa Cloud Shell, è possibile digitare `code .` per aprire un editor.</span><span class="sxs-lookup"><span data-stu-id="81a51-129">If you are using the Cloud Shell, you can type `code .` to open an editor.</span></span>

1. <span data-ttu-id="81a51-130">Incollare il programma seguente nel file **index.js**.</span><span class="sxs-lookup"><span data-stu-id="81a51-130">Paste the following program into the **index.js** file.</span></span> <span data-ttu-id="81a51-131">È possibile usare i tasti di scelta rapida **Ctrl+V** o il pulsante destro del mouse per incollare.</span><span class="sxs-lookup"><span data-stu-id="81a51-131">You can use the **Ctrl+V** keyboard shortcut or right-click to paste.</span></span>

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. <span data-ttu-id="81a51-132">Salvare il file&mdash; Si può usare il menu "..." nell'angolo superiore destro dell'editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="81a51-132">Save the file&mdash;you can use the "..." menu on the top right corner of the Cloud Shell editor.</span></span>

1. <span data-ttu-id="81a51-133">Eseguire l'app per assicurarsi che funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="81a51-133">Run the app to make sure it executes correctly.</span></span> <span data-ttu-id="81a51-134">Dovrebbe visualizzare "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="81a51-134">It should display "Hello, World!"</span></span> <span data-ttu-id="81a51-135">sulla console.</span><span class="sxs-lookup"><span data-stu-id="81a51-135">to the console.</span></span>

    ```bash
    node index.js
    ```

<span data-ttu-id="81a51-136">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="81a51-136">::: zone-end</span></span>