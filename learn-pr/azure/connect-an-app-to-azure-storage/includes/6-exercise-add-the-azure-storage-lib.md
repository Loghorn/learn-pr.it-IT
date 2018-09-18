<span data-ttu-id="bb676-101">::: zone pivot="csharp" In questa unità si integrerà la libreria client di Archiviazione di Azure nell'applicazione console .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bb676-101">::: zone pivot="csharp" Let's integrate the Azure Storage client library into your .NET Core console application.</span></span>

<span data-ttu-id="bb676-102">La libreria client di Archiviazione di Azure per .NET viene distribuita con NuGet.</span><span class="sxs-lookup"><span data-stu-id="bb676-102">The Azure storage client library for .NET is distributed with NuGet.</span></span> <span data-ttu-id="bb676-103">Occorrerà aggiungere il pacchetto **WindowsAzure.Storage** alle applicazioni .NET o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bb676-103">You will want to add the **WindowsAzure.Storage** package to your .NET or .NET Core applications.</span></span>

## <a name="add-the-azure-storage-nuget-package"></a><span data-ttu-id="bb676-104">Aggiungere il pacchetto NuGet per Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="bb676-104">Add the Azure Storage NuGet package</span></span>

1. <span data-ttu-id="bb676-105">Se si usa Cloud Shell, passare alla cartella corretta.</span><span class="sxs-lookup"><span data-stu-id="bb676-105">Switch to the correct folder if you are using the Cloud Shell.</span></span>

1. <span data-ttu-id="bb676-106">Aggiungere il pacchetto **WindowsAzure.Storage** all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bb676-106">Add the **WindowsAzure.Storage** package to the application.</span></span>

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. <span data-ttu-id="bb676-107">Questo dovrebbe tradursi in alcune attività della console durante il download della libreria client e di tutte le dipendenze necessarie.</span><span class="sxs-lookup"><span data-stu-id="bb676-107">This should result in some console activity while the client library and all the required dependencies are downloaded.</span></span> <span data-ttu-id="bb676-108">Al termine, compilare ed eseguire di nuovo l'applicazione per assicurarsi che sia tutto pronto.</span><span class="sxs-lookup"><span data-stu-id="bb676-108">Once it's done, go ahead and build and run the app again to make sure everything is ready to go.</span></span>

    ```bash
    dotnet run
    ```

1. <span data-ttu-id="bb676-109">Come prima, dovrebbe restituire l'output "Hello, World!".</span><span class="sxs-lookup"><span data-stu-id="bb676-109">As before, it should output "Hello, World!".</span></span>

<span data-ttu-id="bb676-110">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="bb676-110">::: zone-end</span></span>

<span data-ttu-id="bb676-111">::: zone-pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="bb676-111">::: zone-pivot="javascript"</span></span>

<span data-ttu-id="bb676-112">Ora si integrerà la **libreria client di Archiviazione di Microsoft Azure per Node.js e JavaScript** nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bb676-112">Let's integrate the **Microsoft Azure Storage Client Library for Node.js and JavaScript** into your application.</span></span>

<span data-ttu-id="bb676-113">La libreria client per Node.js è disponibile tramite Node Package manager (NPM).</span><span class="sxs-lookup"><span data-stu-id="bb676-113">The client library for Node.js is available through the Node Package manager (NPM).</span></span> <span data-ttu-id="bb676-114">Occorrerà aggiungere il pacchetto **azure-storage** al file **packages.json**.</span><span class="sxs-lookup"><span data-stu-id="bb676-114">You will want to add the **azure-storage** package to your **packages.json** file.</span></span>

> [!NOTE]
> <span data-ttu-id="bb676-115">La **libreria client di Archiviazione di Microsoft Azure per Node.js e JavaScript** è destinata alle applicazioni server.</span><span class="sxs-lookup"><span data-stu-id="bb676-115">The **Microsoft Azure Storage Client Library for Node.js and JavaScript** is intended for server applications.</span></span> <span data-ttu-id="bb676-116">Se si sta creando codice JavaScript lato client, è consigliabile usare la **libreria client di Archiviazione di Azure per JavaScript**, che offre le stesse funzionalità ma è stata sviluppata specificamente per l'esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="bb676-116">If you are doing client-side JavaScript, you will want to use the **Azure Storage Client Library for JavaScript** which provides the same functionality but is tailored to running in a browser.</span></span>

## <a name="add-the-azure-storage-package"></a><span data-ttu-id="bb676-117">Aggiungere il pacchetto di Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="bb676-117">Add the Azure Storage package</span></span>

1. <span data-ttu-id="bb676-118">Se si usa Cloud Shell, passare alla cartella corretta.</span><span class="sxs-lookup"><span data-stu-id="bb676-118">Switch to the correct folder if you are using the Cloud Shell.</span></span>

1. <span data-ttu-id="bb676-119">Aggiungere il pacchetto **azure-storage** all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bb676-119">Add the **azure-storage** package to the application.</span></span> <span data-ttu-id="bb676-120">Assicurarsi di specificare l'opzione `--save`, per salvarlo in modo permanente in **packages.json**.</span><span class="sxs-lookup"><span data-stu-id="bb676-120">Make sure to supply the `--save` option so it persists to **packages.json**.</span></span>

    ```bash
    npm install azure-storage --save
    ```

1. <span data-ttu-id="bb676-121">Questo dovrebbe tradursi in alcune attività della console durante il download della libreria client e di tutte le dipendenze necessarie.</span><span class="sxs-lookup"><span data-stu-id="bb676-121">This should result in some console activity while the client library and all the required dependencies are downloaded.</span></span> <span data-ttu-id="bb676-122">Al termine, compilare ed eseguire di nuovo l'applicazione per assicurarsi che sia tutto pronto.</span><span class="sxs-lookup"><span data-stu-id="bb676-122">Once it's done, go ahead and build and run the app again to make sure everything is ready to go.</span></span>

    ```bash
    node index.js
    ```

1. <span data-ttu-id="bb676-123">Come prima, dovrebbe restituire l'output "Hello, World!".</span><span class="sxs-lookup"><span data-stu-id="bb676-123">As before, it should output "Hello, World!".</span></span>

<span data-ttu-id="bb676-124">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="bb676-124">::: zone-end</span></span>

<span data-ttu-id="bb676-125">Ora che sono disponibili le librerie necessarie, è possibile esaminare le attività comuni da eseguire nel codice per usare Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="bb676-125">Now that we have the necessary libraries in place, let's look at the common tasks you'll do in your code to work with Azure storage.</span></span>
