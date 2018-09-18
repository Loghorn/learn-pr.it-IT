<span data-ttu-id="e0b38-101">::: zone pivot="csharp" In questo esercizio si aggiunge all'applicazione .NET core il supporto per recuperare una stringa di connessione da un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e0b38-101">::: zone pivot="csharp" Let's add support to our .NET core application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="e0b38-102">Per iniziare si aggiungeranno i componenti di base necessari per gestire la configurazione in un file JSON.</span><span class="sxs-lookup"><span data-stu-id="e0b38-102">We'll start by adding the necessary plumbing to manage configuration in a JSON file.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="e0b38-103">Creare un file di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="e0b38-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="e0b38-104">Assicurarsi di essere nella directory di lavoro corretta per il progetto.</span><span class="sxs-lookup"><span data-stu-id="e0b38-104">Make sure you are in the correct working directory for your project.</span></span>

1. <span data-ttu-id="e0b38-105">Usare lo strumento `touch` sulla riga di comando per creare un file denominato **appsettings. JSON**.</span><span class="sxs-lookup"><span data-stu-id="e0b38-105">Use the `touch` tool on the command line to create a file named **appsettings.json**.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="e0b38-106">Aprire il progetto con l'editor interattivo, se si lavora in locale, usare il proprio editor preferito. È consigliabile usare **Visual Studio Code**, un ambiente di sviluppo integrato estendibile multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="e0b38-106">Open the project with the interactive editor, if you are working locally, use your editor of choice - we recommend **Visual Studio Code** which is an extensible cross-platform IDE.</span></span> <span data-ttu-id="e0b38-107">I comandi seguenti sono per l'editor di Cloud Shell, ma sono molto simili a quelli di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e0b38-107">The following commands are for the Cloud Shell editor, but are very similar to VS Code.</span></span>
    
    ```bash
    code .
    ```

1. <span data-ttu-id="e0b38-108">Selezionare il file **appsettings.json** file nell'editor e aggiungere il testo seguente.</span><span class="sxs-lookup"><span data-stu-id="e0b38-108">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="e0b38-109">Salvare il file. Nell'angolo superiore destro dell'editor online è disponibile un menu relativo alle operazioni comuni per i file.</span><span class="sxs-lookup"><span data-stu-id="e0b38-109">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```

1. <span data-ttu-id="e0b38-110">Selezionare quindi il file di progetto (**PhotoSharingApp.csproj**) per aprirlo nell'editor.</span><span class="sxs-lookup"><span data-stu-id="e0b38-110">Next, select the project file (**PhotoSharingApp.csproj**) to open it in the editor.</span></span>

1. <span data-ttu-id="e0b38-111">Aggiungere il blocco di configurazione seguente per includere il nuovo file nel progetto e copiarlo nella cartella di output.</span><span class="sxs-lookup"><span data-stu-id="e0b38-111">Add the following configuration block to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="e0b38-112">In questo modo, il file di configurazione dell'app viene inserito nella directory di output alla creazione/compilazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="e0b38-112">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. <span data-ttu-id="e0b38-113">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="e0b38-113">Save the file.</span></span> <span data-ttu-id="e0b38-114">Assicurarsi di eseguire questa operazione. In caso contrario le modifiche andranno perse quando si aggiunge il pacchetto più avanti.</span><span class="sxs-lookup"><span data-stu-id="e0b38-114">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="e0b38-115">Aggiungere il supporto per la lettura di un file di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="e0b38-115">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="e0b38-116">Un'applicazione .NET Core richiede pacchetti NuGet aggiuntivi per leggere un file di configurazione JSON.</span><span class="sxs-lookup"><span data-stu-id="e0b38-116">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="e0b38-117">Nella sezione della finestra relativa al prompt dei comandi aggiungere un riferimento al pacchetto NuGet **Microsoft.Extensions.Configuration.Json**.</span><span class="sxs-lookup"><span data-stu-id="e0b38-117">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="e0b38-118">Aggiungere il codice per leggere il file di configurazione</span><span class="sxs-lookup"><span data-stu-id="e0b38-118">Add code to read the configuration file</span></span>

<span data-ttu-id="e0b38-119">Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="e0b38-119">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="e0b38-120">Selezionare **Program.cs** nell'editor.</span><span class="sxs-lookup"><span data-stu-id="e0b38-120">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="e0b38-121">Nella parte superiore del file è presente la riga **using System;**.</span><span class="sxs-lookup"><span data-stu-id="e0b38-121">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="e0b38-122">Al di sotto di questa riga aggiungere le righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="e0b38-122">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="e0b38-123">Sostituire il contenuto del metodo **Main** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="e0b38-123">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="e0b38-124">Questo codice inizializza il sistema di configurazione per la lettura dal file **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="e0b38-124">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

<span data-ttu-id="e0b38-125">Il file **Program.cs** avrà ora un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e0b38-125">Your **Program.cs** file should now look like the following:</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace PhotoSharingApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();            
        }
    }
}
```

<span data-ttu-id="e0b38-126">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="e0b38-126">::: zone-end</span></span>

<span data-ttu-id="e0b38-127">::: zone-pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="e0b38-127">::: zone-pivot="javascript"</span></span>

<span data-ttu-id="e0b38-128">Aggiungiamo all'applicazione Node.js il supporto per recuperare una stringa di connessione da un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e0b38-128">Let's add support to our Node.js application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="e0b38-129">Per iniziare si aggiungeranno i componenti di base necessari per gestire la configurazione dal file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0b38-129">We'll start by adding the necessary plumbing to manage configuration from our JavaScript file.</span></span>

## <a name="create-a-env-configuration-file"></a><span data-ttu-id="e0b38-130">Creare un file di configurazione con estensione env</span><span class="sxs-lookup"><span data-stu-id="e0b38-130">Create a .env configuration file</span></span>

1. <span data-ttu-id="e0b38-131">Assicurarsi di essere nella directory di lavoro corretta per il progetto.</span><span class="sxs-lookup"><span data-stu-id="e0b38-131">Make sure you are in the correct working directory for your project.</span></span>

1. <span data-ttu-id="e0b38-132">Usare lo strumento `touch` sulla riga di comando per creare un file denominato **.env**.</span><span class="sxs-lookup"><span data-stu-id="e0b38-132">Use the `touch` tool on the command line to create a file named **.env**.</span></span>

    ```bash
    touch .env
    ```

1. <span data-ttu-id="e0b38-133">Aprire il progetto con l'editor interattivo, se si lavora in locale, usare il proprio editor preferito. È consigliabile usare **Visual Studio Code**, un ambiente di sviluppo integrato estendibile multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="e0b38-133">Open the project with the interactive editor, if you are working locally, use your editor of choice - we recommend **Visual Studio Code** which is an extensible cross-platform IDE.</span></span> <span data-ttu-id="e0b38-134">I comandi seguenti sono per l'editor di Cloud Shell, ma sono molto simili a quelli di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e0b38-134">The following commands are for the Cloud Shell editor, but are very similar to VS Code.</span></span>
    
    ```bash
    code .
    ```

1. <span data-ttu-id="e0b38-135">Selezionare il file **.env** nell'editor e aggiungere il testo seguente.</span><span class="sxs-lookup"><span data-stu-id="e0b38-135">Select the **.env** file in the editor and add the following text.</span></span> <span data-ttu-id="e0b38-136">Salvare il file. Nell'angolo superiore destro dell'editor online è disponibile un menu relativo alle operazioni comuni per i file.</span><span class="sxs-lookup"><span data-stu-id="e0b38-136">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > <span data-ttu-id="e0b38-137">Il valore **AZURE_STORAGE_CONNECTION_STRING** è una variabile di ambiente hardcoded usata dalle API di archiviazione per cercare le chiavi di accesso.</span><span class="sxs-lookup"><span data-stu-id="e0b38-137">The **AZURE_STORAGE_CONNECTION_STRING** value is a hard-coded environment variable used for Storage APIs to look up access keys.</span></span> <span data-ttu-id="e0b38-138">Se si preferisce si può usare un nome personalizzato, ma è necessario fornire il nome quando si crea l'oggetto `BlobService`.</span><span class="sxs-lookup"><span data-stu-id="e0b38-138">You can use your own name if you prefer - but you must supply the name to the when you create the `BlobService` object.</span></span>

1. <span data-ttu-id="e0b38-139">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="e0b38-139">Save the file.</span></span>

## <a name="add-support-to-read-an-environment-configuration-file"></a><span data-ttu-id="e0b38-140">Aggiungere il supporto per la lettura di un file di configurazione dell'ambiente</span><span class="sxs-lookup"><span data-stu-id="e0b38-140">Add support to read an environment configuration file</span></span>

<span data-ttu-id="e0b38-141">Si può includere nelle app Node.js il supporto per la lettura dal file **.env** aggiungendo il pacchetto **dotenv**.</span><span class="sxs-lookup"><span data-stu-id="e0b38-141">Node.js apps can include support to read from the **.env** file by adding the **dotenv** package.</span></span>

1. <span data-ttu-id="e0b38-142">Nella della finestra relativa al prompt dei comandi aggiungere una dipendenza dal pacchetto \*dotenv\*\*.</span><span class="sxs-lookup"><span data-stu-id="e0b38-142">In the command prompt section of the window, add a dependency to the  *dotenv*\* package.</span></span>

    ```bash
    node install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="e0b38-143">Aggiungere il codice per leggere il file di configurazione</span><span class="sxs-lookup"><span data-stu-id="e0b38-143">Add code to read the configuration file</span></span>

<span data-ttu-id="e0b38-144">Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e0b38-144">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our application.</span></span>

1. <span data-ttu-id="e0b38-145">Selezionare *index.js*\* nell'editor.</span><span class="sxs-lookup"><span data-stu-id="e0b38-145">Select *index.js*\* in the editor.</span></span>

1. <span data-ttu-id="e0b38-146">Nella parte superiore del file è presente la riga **#!/usr/bin/env node**.</span><span class="sxs-lookup"><span data-stu-id="e0b38-146">At the top of the file, a **#!/usr/bin/env node** line is present.</span></span> <span data-ttu-id="e0b38-147">Al di sotto di questa riga aggiungere un'istruzione `require` per caricare il pacchetto **dotenv**.</span><span class="sxs-lookup"><span data-stu-id="e0b38-147">Underneath that line, add a `require` statement to load the **dotenv** package.</span></span> <span data-ttu-id="e0b38-148">In questo modo si rendono le variabili di ambiente definite nel file **.env** disponibili per il programma.</span><span class="sxs-lookup"><span data-stu-id="e0b38-148">This will make environment variables defined in our **.env** file available to the program.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
<span data-ttu-id="e0b38-149">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="e0b38-149">::: zone-end</span></span>

## <a name="add-the-connection-string-to-the-configuration-file"></a><span data-ttu-id="e0b38-150">Aggiungere la stringa di connessione al file di configurazione</span><span class="sxs-lookup"><span data-stu-id="e0b38-150">Add the connection string to the configuration file</span></span>

<span data-ttu-id="e0b38-151">È ora necessario ottenere la stringa di connessione dell'account di archiviazione e aggiungerla nella configurazione per l'app.</span><span class="sxs-lookup"><span data-stu-id="e0b38-151">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span>

1. <span data-ttu-id="e0b38-152">Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="e0b38-152">Sign in to the [Azure Portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="e0b38-153">Passare all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="e0b38-153">Navigate to your storage account.</span></span> <span data-ttu-id="e0b38-154">È possibile usare la sezione **Tutte le risorse** per trovare l'account di archiviazione oppure eseguire una ricerca per nome nella _casella di ricerca_ nella parte superiore della finestra del portale.</span><span class="sxs-lookup"><span data-stu-id="e0b38-154">You can use the **All Resources** section to find the storage account, or search by name from the _search box_ at the top of the portal window.</span></span> 

1. <span data-ttu-id="e0b38-155">Selezionare il pannello **Chiavi di accesso** dell'account di archiviazione nel portale.</span><span class="sxs-lookup"><span data-stu-id="e0b38-155">Select the **Access Keys** blade of the storage account in the portal.</span></span>

1. <span data-ttu-id="e0b38-156">Copiare la stringa di connessione **key1**.</span><span class="sxs-lookup"><span data-stu-id="e0b38-156">Copy the **key1** Connection string.</span></span>

1. <span data-ttu-id="e0b38-157">Incollare il contenuto della chiave di accesso copiata dal portale come valore per la variabile di configurazione della stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="e0b38-157">Paste in the contents of the access key you copied from the portal as the value for the connection string configuration variable.</span></span>

<span data-ttu-id="e0b38-158">La configurazione sarà ora simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="e0b38-158">Your configuration should now look similar to the following:</span></span>

<span data-ttu-id="e0b38-159">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="e0b38-159">::: zone pivot="csharp"</span></span>
    ```json
    {
        "StorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net"
    }
    ```
<span data-ttu-id="e0b38-160">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="e0b38-160">::: zone-end</span></span>

<span data-ttu-id="e0b38-161">::: zone-pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="e0b38-161">::: zone-pivot="javascript"</span></span>
    ```
    AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net
    ```
<span data-ttu-id="e0b38-162">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="e0b38-162">::: zone-end</span></span>

<span data-ttu-id="e0b38-163">Ora che tutto è pronto, si può iniziare ad aggiungere codice per usare l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="e0b38-163">Now that we have that all wired up, we can start adding code to use our storage account.</span></span>