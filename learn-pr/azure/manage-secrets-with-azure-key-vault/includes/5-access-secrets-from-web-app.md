Dopo aver appreso come l'abilitazione dell'identità del servizio gestita crea un'identità che l'app può usare per l'autenticazione, verrà creata un'app che usa tale identità per accedere ai segreti nell'insieme di credenziali.

## <a name="reading-secrets-in-an-aspnet-core-app"></a>Lettura di segreti in un'app ASP.NET Core

L'API Azure Key Vault è un'API REST che gestisce tutte le attività di gestione e utilizzo delle chiavi e degli insiemi di credenziali. Ogni segreto contenuto in un insieme di credenziali ha un URL univoco; i valori dei segreti vengono recuperati con richieste HTTP GET.

La libreria client ufficiale dell'insieme di credenziali delle chiavi per .NET Core è la classe `KeyVaultClient` nel pacchetto Microsoft.Azure.KeyVault NuGet. Non è necessario usarla direttamente &mdash; con il metodo `AddAzureKeyVault` di ASP.NET Core è possibile caricare tutti i segreti all'avvio da un insieme di credenziali nell'API di configurazione. Questa tecnica consente di accedere a tutti i segreti con il nome, usando la stessa interfaccia `IConfiguration` usata per il resto della configurazione. Le app che usano `AddAzureKeyVault` richiedono entrambe le autorizzazioni **Get** e **List** per l'insieme delle credenziali.

> [!TIP]
> Indipendentemente dal framework o dal linguaggio usato per compilare l'app, è consigliabile caricare i segreti in memoria una sola volta all'avvio dell'app, a meno che non si abbia un motivo valido per non farlo. Leggere i segreti direttamente dall'insieme di credenziali ogni volta che sono necessari è un'attività lenta e costosa.

Come input, il metodo `AddAzureKeyVault` richiede solo il nome dell'insieme di credenziali, che viene ottenuto dalla configurazione dell'app locale. Gestisce automaticamente anche l'autenticazione dell'identità del servizio gestita. Quando usato in un'app distribuita nel servizio app di Azure con l'identità del servizio gestita abilitata, rileva il servizio token dell'identità del servizio gestita e lo usa per eseguire l'autenticazione. È una scelta ideale per la maggior parte degli scenari e consente di adottare tutte le procedure consigliate. Verrà usato nell'esercizio di questa unità.

## <a name="handling-secrets-in-an-app"></a>Gestione di segreti in un'app

Dopo aver caricato un segreto nell'app, sarà l'app a gestirlo in modo sicuro. Nell'app che viene compilata in questo modulo, il valore del segreto viene scritto nella risposta del client e visualizzato in un Web browser per dimostrare che è stato caricato correttamente. **La restituzione di un valore del segreto al client *non è*  un'operazione usuale.** I segreti sono usati, ad esempio, per inizializzare le librerie client di database o di API remote.

> [!IMPORTANT]
> Esaminare sempre con attenzione il codice per verificare che l'app non scriva i segreti in output di qualunque tipo, inclusi log, archiviazione e risposte.

## <a name="exercise"></a>Esercizio

Viene creata una nuova API Web di ASP.NET Core e usato il metodo `AddAzureKeyVault` per caricare il segreto dall'insieme di credenziali.

### <a name="create-the-app"></a>Creare l'app

Nel terminale Azure Cloud Shell, eseguire il comando seguente per creare una nuova applicazione API Web di ASP.NET Core e aprirla nell'editor.

```console
dotnet new webapi -o KeyVaultDemoApp
cd KeyVaultDemoApp
code .
```

Dopo il caricamento dell'editor, eseguire i comandi seguenti nella shell per aggiungere il pacchetto NuGet che contiene il metodo `AddAzureKeyVault` e ripristinare tutte le dipendenze dell'app.

```console
dotnet add package Microsoft.Extensions.Configuration.AzureKeyVault
dotnet restore
```

### <a name="add-code-to-load-and-use-secrets"></a>Aggiungere il codice per caricare e usare i segreti

Per mostrare l'utilizzo ottimale dell'insieme di credenziali delle chiavi, l'app viene modificata in modo che carichi i segreti dall'insieme di credenziali all'avvio. Viene anche aggiunto un nuovo controller con un endpoint che ottiene il segreto **SecretPassword** dall'insieme di credenziali.

Avviare l'app: aprire `Program.cs`, eliminare il contenuto e sostituirlo con il codice seguente:

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
> Verificare di aver salvato i file con `Ctrl+S` dopo aver apportato le modifiche.

L'unica modifica rispetto al codice di avvio è l'aggiunta di `ConfigureAppConfiguration`. È qui che viene caricato il nome dell'insieme di credenziali dalla configurazione e chiamato il metodo `AddAzureKeyVault`.

Avviare il controller: creare un nuovo file nella cartella `Controllers` denominato `SecretTestController.cs` e incollarvi il codice seguente.

> [!NOTE]
> Per creare un nuovo file, usare il comando `touch` nella shell. In questo caso, usare `touch Controllers/SecretTestController.cs`. Fare clic sul pulsante di aggiornamento nel riquadro File dell'editor per visualizzarlo.

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

Eseguire `dotnet build` nella shell per verificare che tutto venga compilato. L'app è pronta per l'esecuzione in Azure.