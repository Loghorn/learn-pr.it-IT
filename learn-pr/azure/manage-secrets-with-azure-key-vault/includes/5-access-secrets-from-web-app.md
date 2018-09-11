<span data-ttu-id="623c8-101">Dopo aver appreso come l'abilitazione dell'identità del servizio gestita crea un'identità che l'app può usare per l'autenticazione, verrà creata un'app che usa tale identità per accedere ai segreti nell'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="623c8-101">Now that you know how enabling MSI creates an identity for our app to use for authentication, we'll create an app that uses that identity to access secrets in the vault.</span></span>

## <a name="reading-secrets-in-an-aspnet-core-app"></a><span data-ttu-id="623c8-102">Lettura di segreti in un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="623c8-102">Reading secrets in an ASP.NET Core app</span></span>

<span data-ttu-id="623c8-103">L'API Azure Key Vault è un'API REST che gestisce tutte le attività di gestione e utilizzo delle chiavi e degli insiemi di credenziali.</span><span class="sxs-lookup"><span data-stu-id="623c8-103">The Azure Key Vault API is a REST API that handles all management and usage of keys and vaults.</span></span> <span data-ttu-id="623c8-104">Ogni segreto contenuto in un insieme di credenziali ha un URL univoco; i valori dei segreti vengono recuperati con richieste HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="623c8-104">Each secret in a vault has a unique URL, and secret values are retrieved with HTTP GET requests.</span></span>

<span data-ttu-id="623c8-105">La libreria client ufficiale dell'insieme di credenziali delle chiavi per .NET Core è la classe `KeyVaultClient` nel pacchetto Microsoft.Azure.KeyVault NuGet.</span><span class="sxs-lookup"><span data-stu-id="623c8-105">The official Key Vault client library for .NET Core is the `KeyVaultClient` class in the Microsoft.Azure.KeyVault NuGet package.</span></span> <span data-ttu-id="623c8-106">Non è necessario usarla direttamente &mdash; con il metodo `AddAzureKeyVault` di ASP.NET Core è possibile caricare tutti i segreti all'avvio da un insieme di credenziali nell'API di configurazione.</span><span class="sxs-lookup"><span data-stu-id="623c8-106">You don't need to use it directly, though &mdash; with ASP.NET Core's `AddAzureKeyVault` method, you can load all the secrets from a vault into the Configuration API at startup.</span></span> <span data-ttu-id="623c8-107">Questa tecnica consente di accedere a tutti i segreti con il nome, usando la stessa interfaccia `IConfiguration` usata per il resto della configurazione.</span><span class="sxs-lookup"><span data-stu-id="623c8-107">This technique enables you to access all of your secrets by name using the same `IConfiguration` interface you use for the rest of your configuration.</span></span> <span data-ttu-id="623c8-108">Le app che usano `AddAzureKeyVault` richiedono entrambe le autorizzazioni **Get** e **List** per l'insieme delle credenziali.</span><span class="sxs-lookup"><span data-stu-id="623c8-108">Apps that use `AddAzureKeyVault` require both **Get** and **List** permissions to the vault.</span></span>

> [!TIP]
> <span data-ttu-id="623c8-109">Indipendentemente dal framework o dal linguaggio usato per compilare l'app, è consigliabile caricare i segreti in memoria una sola volta all'avvio dell'app, a meno che non si abbia un motivo valido per non farlo.</span><span class="sxs-lookup"><span data-stu-id="623c8-109">Regardless of the framework or language you use to build your app, load secrets into memory once at app startup unless you have a specific reason not to.</span></span> <span data-ttu-id="623c8-110">Leggere i segreti direttamente dall'insieme di credenziali ogni volta che sono necessari è un'attività lenta e costosa.</span><span class="sxs-lookup"><span data-stu-id="623c8-110">Reading them directly from the vault every time you need them is unnecessarily slow and expensive.</span></span>

<span data-ttu-id="623c8-111">Come input, il metodo `AddAzureKeyVault` richiede solo il nome dell'insieme di credenziali, che viene ottenuto dalla configurazione dell'app locale.</span><span class="sxs-lookup"><span data-stu-id="623c8-111">`AddAzureKeyVault` only requires the vault name as an input, which we'll get from our local app configuration.</span></span> <span data-ttu-id="623c8-112">Gestisce automaticamente anche l'autenticazione dell'identità del servizio gestita. Quando usato in un'app distribuita nel servizio app di Azure con l'identità del servizio gestita abilitata, rileva il servizio token dell'identità del servizio gestita e lo usa per eseguire l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="623c8-112">It also automatically handles MSI authentication &mdash; when used in an app deployed to Azure App Service with MSI enabled, it will detect the MSI token service and use it to authenticate.</span></span> <span data-ttu-id="623c8-113">È una scelta ideale per la maggior parte degli scenari e consente di adottare tutte le procedure consigliate. Verrà usato nell'esercizio di questa unità.</span><span class="sxs-lookup"><span data-stu-id="623c8-113">It's a good fit for most scenarios and implements all best practices, and we'll use it in this unit's exercise.</span></span>

## <a name="handling-secrets-in-an-app"></a><span data-ttu-id="623c8-114">Gestione di segreti in un'app</span><span class="sxs-lookup"><span data-stu-id="623c8-114">Handling secrets in an app</span></span>

<span data-ttu-id="623c8-115">Dopo aver caricato un segreto nell'app, sarà l'app a gestirlo in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="623c8-115">Once a secret is loaded into your app, it's up to your app to handle it securely.</span></span> <span data-ttu-id="623c8-116">Nell'app che viene compilata in questo modulo, il valore del segreto viene scritto nella risposta del client e visualizzato in un Web browser per dimostrare che è stato caricato correttamente.</span><span class="sxs-lookup"><span data-stu-id="623c8-116">In the app we build in this module, we write our secret value out to the client response and view it in a web browser to demonstrate that it has been loaded successfully.</span></span> <span data-ttu-id="623c8-117">**La restituzione di un valore del segreto al client *non è*  un'operazione usuale.**</span><span class="sxs-lookup"><span data-stu-id="623c8-117">**Returning a secret value to the client is *not* something you'd normally do!**</span></span> <span data-ttu-id="623c8-118">I segreti sono usati, ad esempio, per inizializzare le librerie client di database o di API remote.</span><span class="sxs-lookup"><span data-stu-id="623c8-118">Usually, you'll use secrets to do things like initialize client libraries for databases or remote APIs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="623c8-119">Esaminare sempre con attenzione il codice per verificare che l'app non scriva i segreti in output di qualunque tipo, inclusi log, archiviazione e risposte.</span><span class="sxs-lookup"><span data-stu-id="623c8-119">Always carefully review your code to ensure that your app never writes secrets to any kind of output, including logs, storage, and responses.</span></span>

## <a name="exercise"></a><span data-ttu-id="623c8-120">Esercizio</span><span class="sxs-lookup"><span data-stu-id="623c8-120">Exercise</span></span>

<span data-ttu-id="623c8-121">Viene creata una nuova API Web di ASP.NET Core e usato il metodo `AddAzureKeyVault` per caricare il segreto dall'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="623c8-121">We'll create a new ASP.NET Core web API and use `AddAzureKeyVault` to load the secret from our vault.</span></span>

### <a name="create-the-app"></a><span data-ttu-id="623c8-122">Creare l'app</span><span class="sxs-lookup"><span data-stu-id="623c8-122">Create the app</span></span>

<span data-ttu-id="623c8-123">Nel terminale Azure Cloud Shell, eseguire il comando seguente per creare una nuova applicazione API Web di ASP.NET Core e aprirla nell'editor.</span><span class="sxs-lookup"><span data-stu-id="623c8-123">In the Azure Cloud Shell terminal, run the following to create a new ASP.NET Core web API application and open it in the editor.</span></span>

```console
dotnet new webapi -o KeyVaultDemoApp
cd KeyVaultDemoApp
code .
```

<span data-ttu-id="623c8-124">Dopo il caricamento dell'editor, eseguire i comandi seguenti nella shell per aggiungere il pacchetto NuGet che contiene il metodo `AddAzureKeyVault` e ripristinare tutte le dipendenze dell'app.</span><span class="sxs-lookup"><span data-stu-id="623c8-124">After the editor loads, run the following commands in the shell to add the NuGet package containing `AddAzureKeyVault` and restore all of the app's dependencies.</span></span>

```console
dotnet add package Microsoft.Extensions.Configuration.AzureKeyVault
dotnet restore
```

### <a name="add-code-to-load-and-use-secrets"></a><span data-ttu-id="623c8-125">Aggiungere il codice per caricare e usare i segreti</span><span class="sxs-lookup"><span data-stu-id="623c8-125">Add code to load and use secrets</span></span>

<span data-ttu-id="623c8-126">Per mostrare l'utilizzo ottimale dell'insieme di credenziali delle chiavi, l'app viene modificata in modo che carichi i segreti dall'insieme di credenziali all'avvio.</span><span class="sxs-lookup"><span data-stu-id="623c8-126">To demonstrate good usage of Key Vault, we will modify our app to load secrets from the vault at startup.</span></span> <span data-ttu-id="623c8-127">Viene anche aggiunto un nuovo controller con un endpoint che ottiene il segreto **SecretPassword** dall'insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="623c8-127">We'll also add a new controller with an endpoint that gets our **SecretPassword** secret from the vault.</span></span>

<span data-ttu-id="623c8-128">Avviare l'app: aprire `Program.cs`, eliminare il contenuto e sostituirlo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="623c8-128">First, the app startup: Open `Program.cs`, delete the contents and replace them with the following code:</span></span>

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;

namespace KeyVaultDemoApp
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((context, config) =>
                {
                    // Build the current set of configuration to load values from
                    // JSON files and environment variables, including VaultName.
                    var builtConfig = config.Build();

                    // Use VaultName from the configuration to create the full vault URL.
                    var vaultUrl = $"https://{builtConfig["VaultName"]}.vault.azure.net/";

                    // Load all secrets from the vault into configuration. This will automatically
                    // authenticate to the vault using MSI. If MSI is not available, it will
                    // check if Visual Studio and/or the Azure CLI are installed locally and
                    // see if they are configured with credentials that can access the vault.
                    config.AddAzureKeyVault(vaultUrl);
                })
                .UseStartup<Startup>();
    }
}
```

> [!NOTE]
> <span data-ttu-id="623c8-129">Verificare di aver salvato i file con `Ctrl+S` dopo aver apportato le modifiche.</span><span class="sxs-lookup"><span data-stu-id="623c8-129">Make sure to save files with `Ctrl+S` when you're done editing them.</span></span>

<span data-ttu-id="623c8-130">L'unica modifica rispetto al codice di avvio è l'aggiunta di `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="623c8-130">The only change from the starter code is the addition of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="623c8-131">È qui che viene caricato il nome dell'insieme di credenziali dalla configurazione e chiamato il metodo `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="623c8-131">This is where we load the vault name from configuration and call `AddAzureKeyVault` with it.</span></span>

<span data-ttu-id="623c8-132">Avviare il controller: creare un nuovo file nella cartella `Controllers` denominato `SecretTestController.cs` e incollarvi il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="623c8-132">Next, the controller: Create a new file in the `Controllers` folder called `SecretTestController.cs` and paste the following code into it.</span></span>

> [!NOTE]
> <span data-ttu-id="623c8-133">Per creare un nuovo file, usare il comando `touch` nella shell.</span><span class="sxs-lookup"><span data-stu-id="623c8-133">To create a new file, use the `touch` command in the shell.</span></span> <span data-ttu-id="623c8-134">In questo caso, usare `touch Controllers/SecretTestController.cs`.</span><span class="sxs-lookup"><span data-stu-id="623c8-134">In this case, use `touch Controllers/SecretTestController.cs`.</span></span> <span data-ttu-id="623c8-135">Fare clic sul pulsante di aggiornamento nel riquadro File dell'editor per visualizzarlo.</span><span class="sxs-lookup"><span data-stu-id="623c8-135">You'll need to click the refresh button in the Files pane of the editor to see it there.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;

namespace KeyVaultDemoApp.Controllers
{
    [Route("api/[controller]")]
    public class SecretTestController : ControllerBase
    {
        private readonly IConfiguration _configuration;

        public SecretTestController(IConfiguration configuration)
        {
            _configuration = configuration;
        }

        [HttpGet]
        public IActionResult Get()
        {
            // Get the secret value from configuration. This can be done anywhere
            // we have access to IConfiguration. This does not call the Key Vault
            // API, because the secrets were loaded at startup.
            var secretName = "SecretPassword";
            var secretValue = _configuration[secretName];

            if (secretValue == null)
            {
                return StatusCode(
                    StatusCodes.Status500InternalServerError,
                    $"Error: No secret named {secretName} was found...");
            }
            else {
                return Content($"Secret value: {secretValue}" +
                    Environment.NewLine + Environment.NewLine +
                    "This is for testing only! Never output a secret " +
                    "to a response or anywhere else in a real app!");
            }
        }
    }
}
```

<span data-ttu-id="623c8-136">Eseguire `dotnet build` nella shell per verificare che tutto venga compilato.</span><span class="sxs-lookup"><span data-stu-id="623c8-136">Run `dotnet build` in the shell to make sure everything compiles.</span></span> <span data-ttu-id="623c8-137">L'app è pronta per l'esecuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="623c8-137">The app is ready to run &mdash; now let's get it into Azure!</span></span>