<span data-ttu-id="b4d14-101">In questa unità si creerà un'istanza di Cache Redis di Azure per archiviare e restituire valori usati comunemente.</span><span class="sxs-lookup"><span data-stu-id="b4d14-101">In this unit, you'll create an Azure Redis Cache instance to store and return commonly used values.</span></span>

## <a name="create-a-cache"></a><span data-ttu-id="b4d14-102">Creare una cache</span><span class="sxs-lookup"><span data-stu-id="b4d14-102">Create a cache</span></span>

1. <span data-ttu-id="b4d14-103">Accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="b4d14-103">Sign in to the Azure portal.</span></span> <span data-ttu-id="b4d14-104">Fare quindi clic su **Crea una risorsa**, su **Database** e quindi su **Cache Redis**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-104">Then click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    <span data-ttu-id="b4d14-105">Lo screenshot seguente mostra la posizione di Cache Redis all'interno delle varie opzioni per le risorse di database nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="b4d14-105">The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.</span></span>

    ![Screenshot che mostra le opzioni per i database del portale di Azure, con le opzioni Crea una risorsa, Database e Cache Redis evidenziate.](../media/4-create-a-cache-1.png)

## <a name="configure-your-cache"></a><span data-ttu-id="b4d14-107">Configurare la cache</span><span class="sxs-lookup"><span data-stu-id="b4d14-107">Configure your cache</span></span>

1. <span data-ttu-id="b4d14-108">**Nome DNS:** creare un nome globalmente univoco, ad esempio **ContosoSportsApp1028**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-108">**DNS Name:** Create a globally unique name such as **ContosoSportsApp1028**.</span></span>

1. <span data-ttu-id="b4d14-109">**Sottoscrizione:** selezionare la sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="b4d14-109">**Subscription:** Select your subscription.</span></span>

1. <span data-ttu-id="b4d14-110">**Gruppo di risorse:** creare un nuovo gruppo di risorse e assegnare al gruppo un nome.</span><span class="sxs-lookup"><span data-stu-id="b4d14-110">**Resource group:** Create a new resource group and give it a name.</span></span>

1. <span data-ttu-id="b4d14-111">**Area:** dato che la maggior parte dei clienti si trova nell'area di New York, è necessario selezionare **Stati Uniti orientali**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-111">**Location:** Because most of your customers are in the New York area, you should select **East US**.</span></span>

1. <span data-ttu-id="b4d14-112">**Piano tariffario:** selezionare **Basic C0**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-112">**Pricing tier:** Select **Basic C0**.</span></span>

1. <span data-ttu-id="b4d14-113">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-113">Click **Create**.</span></span>

    <span data-ttu-id="b4d14-114">Lo screenshot seguente mostra una configurazione rappresentativa usata per creare una nuova risorsa di Cache Redis.</span><span class="sxs-lookup"><span data-stu-id="b4d14-114">The following screenshot shows a representative configuration used to create a new Redis Cache resource.</span></span>

    ![Screenshot che mostra il pannello del portale di Azure quando si crea una nuova risorsa di Cache Redis, popolato con un nome DNS della configurazione di esempio, sottoscrizione, nuovo gruppo di risorse, area e piano tariffario.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> <span data-ttu-id="b4d14-116">Si dovrà attendere che la cache venga distribuita prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="b4d14-116">You will have to wait until the cache is deployed before continuing.</span></span> <span data-ttu-id="b4d14-117">Questo processo può richiedere del tempo.</span><span class="sxs-lookup"><span data-stu-id="b4d14-117">This process might take some time.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="b4d14-118">Recuperare le chiavi di accesso e il nome host</span><span class="sxs-lookup"><span data-stu-id="b4d14-118">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="b4d14-119">Nel portale di Azure passare alla cache e selezionare **Impostazioni** > **Chiavi di accesso**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-119">In the Azure portal, browse to your cache and select **Settings** > **Access keys**.</span></span> <span data-ttu-id="b4d14-120">Prendere nota della **Stringa di connessione primaria (StackExchange.Redis)**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-120">Write down the **Primary connection string (StackExchange.Redis)**.</span></span>

    <span data-ttu-id="b4d14-121">Questa chiave include la chiave primaria e il nome host in una stringa di connessione completa per l'uso all'interno delle impostazioni dell'applicazione per il pacchetto StackExchange.Redis usato.</span><span class="sxs-lookup"><span data-stu-id="b4d14-121">This key includes your primary key and host name in a complete connection string for use within your application settings for the StackExchange.Redis package we are using.</span></span>

1. <span data-ttu-id="b4d14-122">Avviare il Blocco note o un altro editor di testo nel computer.</span><span class="sxs-lookup"><span data-stu-id="b4d14-122">Start Notepad or another text editor on your computer.</span></span>

1. <span data-ttu-id="b4d14-123">Aggiungere il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="b4d14-123">Add the following contents:</span></span>

    <span data-ttu-id="b4d14-124">Sostituire `<connection-string>` con la stringa di connessione primaria della cache di cui si è preso nota in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b4d14-124">Replace `<connection-string>` with your cache's primary connection string from above.</span></span>

    ```xml
    <appSettings>
        <add key="CacheConnection" value="<connection-string>"/>
    </appSettings>
    ```

1. <span data-ttu-id="b4d14-125">Salvare il file in una posizione in cui non verrà incluso nel codice sorgente dell'applicazione di esempio.</span><span class="sxs-lookup"><span data-stu-id="b4d14-125">Save the file in a location where it won't be included within the source code of your sample application.</span></span> <span data-ttu-id="b4d14-126">Per questa esercitazione, il file verrà salvato come `C:\AppSecrets\CacheSecrets.config`.</span><span class="sxs-lookup"><span data-stu-id="b4d14-126">For this tutorial, we will save the file as `C:\AppSecrets\CacheSecrets.config`.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="b4d14-127">Creare un'app console</span><span class="sxs-lookup"><span data-stu-id="b4d14-127">Create a console app</span></span>

1. <span data-ttu-id="b4d14-128">Avviare Visual Studio e fare clic sulla voce di menu **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-128">Start Visual Studio and click the **File** > **New** > **Project...** menu item.</span></span>

1. <span data-ttu-id="b4d14-129">Selezionare il modello **Visual C#** > **Windows Desktop** > **App console**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-129">Select the **Visual C#** > **Windows Desktop** > **Console App** template.</span></span>

1. <span data-ttu-id="b4d14-130">Assegnare un nome all'app e fare clic su **OK** per creare una nuova applicazione console.</span><span class="sxs-lookup"><span data-stu-id="b4d14-130">Give your app a name and click **OK** to create a new console application.</span></span>

## <a name="configure-the-cache-client"></a><span data-ttu-id="b4d14-131">Configurare il client della cache</span><span class="sxs-lookup"><span data-stu-id="b4d14-131">Configure the cache client</span></span>

<span data-ttu-id="b4d14-132">In questa sezione, si configurerà l'applicazione console per usare il client **StackExchange.Redis** per .NET.</span><span class="sxs-lookup"><span data-stu-id="b4d14-132">In this section, you will configure the console application to use the **StackExchange.Redis** client for .NET.</span></span>

1. <span data-ttu-id="b4d14-133">In Visual Studio fare clic su **Strumenti**, su **Gestione pacchetti NuGet**, **Console di Gestione pacchetti** ed eseguire il comando seguente dalla finestra Console di Gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="b4d14-133">In Visual Studio, click **Tools**, point to **NuGet Package Manager**, click **Package Manager Console**, and run the following command from the Package Manager Console window:</span></span>

    ```powershell
    Install-Package StackExchange.Redis
    ```

<span data-ttu-id="b4d14-134">Una volta completata l'installazione, il client della cache StackExchange.Redis è disponibile per l'uso con il progetto.</span><span class="sxs-lookup"><span data-stu-id="b4d14-134">Once the installation is completed, the StackExchange.Redis cache client is available to use with your project.</span></span>

## <a name="connect-to-the-cache"></a><span data-ttu-id="b4d14-135">Connettersi alla cache</span><span class="sxs-lookup"><span data-stu-id="b4d14-135">Connect to the cache</span></span>

1. <span data-ttu-id="b4d14-136">In Visual Studio aprire il file **App.config** in **Esplora soluzioni** e aggiornarlo per includere un attributo di file **appSettings** che fa riferimento al file **CacheSecrets.config**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-136">In Visual Studio, open the **App.config** file within the **Solution Explorer** and update it to include an **appSettings** file attribute that references the **CacheSecrets.config** file.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.1" />
        </startup>

        <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>

    </configuration>
    ```

1. <span data-ttu-id="b4d14-137">In Esplora soluzioni fare clic con il pulsante destro del mouse su **Riferimenti** e scegliere **Aggiungi riferimento**. Selezionare **System.Configuration** nell'elenco **Assembly** > **Framework** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4d14-137">In Solution Explorer, right-click **References** and click **Add Reference...**. Select **System.Configuration** from the **Assemblies** > **Framework** list and click **OK**.</span></span>

1. <span data-ttu-id="b4d14-138">Aggiungere le istruzioni `using` seguenti a **Program.cs**:</span><span class="sxs-lookup"><span data-stu-id="b4d14-138">Add the following `using` statements to **Program.cs**:</span></span>

    ```csharp
    using StackExchange.Redis;
    using System.Configuration;
    ```

<span data-ttu-id="b4d14-139">La connessione a Cache Redis di Azure è gestita dalla classe ConnectionMultiplexer.</span><span class="sxs-lookup"><span data-stu-id="b4d14-139">The connection to Azure Redis Cache is managed by the ConnectionMultiplexer class.</span></span> <span data-ttu-id="b4d14-140">Questa classe deve essere condivisa e riutilizzata in tutta l'applicazione client.</span><span class="sxs-lookup"><span data-stu-id="b4d14-140">This class should be shared and reused throughout your client application.</span></span> <span data-ttu-id="b4d14-141">Non creare una nuova connessione per ogni operazione.</span><span class="sxs-lookup"><span data-stu-id="b4d14-141">Do not create a new connection for each operation.</span></span>

<span data-ttu-id="b4d14-142">Non archiviare mai le credenziali nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b4d14-142">Never store credentials in source code.</span></span> <span data-ttu-id="b4d14-143">Per semplificare questo esempio, viene usato solo un file di configurazione dei segreti esterno.</span><span class="sxs-lookup"><span data-stu-id="b4d14-143">To keep this sample simple, we're only using an external secrets config file.</span></span> <span data-ttu-id="b4d14-144">Un approccio migliore potrebbe consistere nell'usare Azure Key Vault con certificati.</span><span class="sxs-lookup"><span data-stu-id="b4d14-144">A better approach would be to use Azure Key Vault with certificates.</span></span>

1. <span data-ttu-id="b4d14-145">In **Program.cs** aggiungere i membri seguenti alla classe Program dell'applicazione console:</span><span class="sxs-lookup"><span data-stu-id="b4d14-145">In **Program.cs**, add the following members to the Program class of your console application:</span></span>

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

<span data-ttu-id="b4d14-146">Questo approccio per la condivisione di un'istanza di ConnectionMultiplexer nell'applicazione usa una proprietà statica che restituisce un'istanza connessa.</span><span class="sxs-lookup"><span data-stu-id="b4d14-146">This approach to sharing a ConnectionMultiplexer instance in your application uses a static property that returns a connected instance.</span></span> <span data-ttu-id="b4d14-147">Il codice offre un modo thread-safe per inizializzare solo una singola istanza di ConnectionMultiplexer connessa.</span><span class="sxs-lookup"><span data-stu-id="b4d14-147">The code provides a thread-safe way to initialize only a single connected ConnectionMultiplexer instance.</span></span> <span data-ttu-id="b4d14-148">La proprietà **abortConnect** è impostata su false, a indicare che la chiamata riuscirà anche se non viene stabilita una connessione a Cache Redis di Azure.</span><span class="sxs-lookup"><span data-stu-id="b4d14-148">**abortConnect** is set to false, which means that the call succeeds even if a connection to Azure Redis Cache is not established.</span></span> <span data-ttu-id="b4d14-149">Una delle funzionalità principali di ConnectionMultiplexer è il ripristino automatico della connettività alla cache non appena l'errore di rete o eventuali altri problemi vengono risolti.</span><span class="sxs-lookup"><span data-stu-id="b4d14-149">One key feature of ConnectionMultiplexer is that it automatically restores connectivity to the cache once the network issue or other causes are resolved.</span></span>

<span data-ttu-id="b4d14-150">Il valore dell'appSetting **CacheConnection** viene usato per fare riferimento alla stringa di connessione della cache dal portale di Azure come parametro password.</span><span class="sxs-lookup"><span data-stu-id="b4d14-150">The value of the **CacheConnection** appSetting value is used to reference the cache connection string from the Azure portal as the password parameter.</span></span>
