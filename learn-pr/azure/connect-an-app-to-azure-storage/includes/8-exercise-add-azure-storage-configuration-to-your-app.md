<span data-ttu-id="7689e-101">::: zone pivot="csharp" In questo esercizio si aggiunge all'applicazione .NET core il supporto per recuperare una stringa di connessione da un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7689e-101">::: zone pivot="csharp" Let's add support to our .NET core application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="7689e-102">Per iniziare si aggiungeranno i componenti di base necessari per gestire la configurazione in un file JSON.</span><span class="sxs-lookup"><span data-stu-id="7689e-102">We'll start by adding the necessary plumbing to manage configuration in a JSON file.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="7689e-103">Creare un file di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="7689e-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="7689e-104">Passare con il comando `cd` alla directory PhotoSharingApp, se ci si trova in un'altra directory.</span><span class="sxs-lookup"><span data-stu-id="7689e-104">`cd` to the PhotoSharingApp directory if you aren't already there.</span></span>

1. <span data-ttu-id="7689e-105">Usare lo strumento `touch` nella riga di comando per creare un file denominato **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="7689e-105">Use the `touch` tool on the command line to create a file named **appsettings.json**.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="7689e-106">Aprire il progetto in un editor.</span><span class="sxs-lookup"><span data-stu-id="7689e-106">Open the project in an editor.</span></span> <span data-ttu-id="7689e-107">Se si lavora in locale, è possibile usare l'editor preferito.</span><span class="sxs-lookup"><span data-stu-id="7689e-107">If you are working locally, use your editor of choice.</span></span> <span data-ttu-id="7689e-108">È consigliabile usare **Visual Studio Code**, un IDE multipiattaforma estendibile.</span><span class="sxs-lookup"><span data-stu-id="7689e-108">We recommend **Visual Studio Code**, which is an extensible cross-platform IDE.</span></span> <span data-ttu-id="7689e-109">Se si lavora in Cloud Shell, è consigliabile usare l'editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="7689e-109">If you are working in the Cloud Shell, we recommend the Cloud Shell editor.</span></span> <span data-ttu-id="7689e-110">Il comando seguente funziona in entrambi gli editor.</span><span class="sxs-lookup"><span data-stu-id="7689e-110">The following command works for both.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="7689e-111">Selezionare il file **appsettings.json** nell'editor e aggiungere il testo seguente.</span><span class="sxs-lookup"><span data-stu-id="7689e-111">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="7689e-112">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7689e-112">Save the file.</span></span> <span data-ttu-id="7689e-113">Nell'angolo superiore destro dell'editor di Cloud Shell è disponibile un menu con le operazioni più comuni eseguite sui file.</span><span class="sxs-lookup"><span data-stu-id="7689e-113">In the Cloud Shell editor, there is a menu in the top right corner that has common file operations.</span></span>

    ```json
    {
      "StorageAccountConnectionString": "<value>"
    }
    ```

1. <span data-ttu-id="7689e-114">È ora necessario ottenere la stringa di connessione dell'account di archiviazione e inserirla nella configurazione per l'app.</span><span class="sxs-lookup"><span data-stu-id="7689e-114">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span> <span data-ttu-id="7689e-115">In Cloud Shell eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="7689e-115">In Cloud Shell run the following command.</span></span>

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. <span data-ttu-id="7689e-116">Copiare la stringa di connessione restituita dal comando e sostituire `"<value>"` nel file **appsettings.json** con questa stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="7689e-116">Copy the connection string that is returned from that command, and replace `"<value>"` in the **appsettings.json** file with this connection string.</span></span> <span data-ttu-id="7689e-117">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7689e-117">Save the file.</span></span>

1. <span data-ttu-id="7689e-118">Aprire quindi il file di progetto (**PhotoSharingApp.csproj**) nell'editor.</span><span class="sxs-lookup"><span data-stu-id="7689e-118">Next, open the project file (**PhotoSharingApp.csproj**) in the editor.</span></span>

1. <span data-ttu-id="7689e-119">Aggiungere il blocco di configurazione seguente per includere il nuovo file nel progetto e copiarlo nella cartella di output.</span><span class="sxs-lookup"><span data-stu-id="7689e-119">Add the following configuration block to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="7689e-120">In questo modo, il file di configurazione dell'app viene inserito nella directory di output alla creazione/compilazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="7689e-120">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

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

1. <span data-ttu-id="7689e-121">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7689e-121">Save the file.</span></span> <span data-ttu-id="7689e-122">Assicurarsi di eseguire questa operazione. In caso contrario le modifiche andranno perse quando si aggiunge il pacchetto più avanti.</span><span class="sxs-lookup"><span data-stu-id="7689e-122">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="7689e-123">Aggiungere il supporto per la lettura di un file di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="7689e-123">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="7689e-124">Un'applicazione .NET Core richiede pacchetti NuGet aggiuntivi per leggere un file di configurazione JSON.</span><span class="sxs-lookup"><span data-stu-id="7689e-124">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="7689e-125">Nella sezione della finestra relativa al prompt dei comandi aggiungere un riferimento al pacchetto NuGet **Microsoft.Extensions.Configuration.Json**.</span><span class="sxs-lookup"><span data-stu-id="7689e-125">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="7689e-126">Aggiungere codice per leggere il file di configurazione</span><span class="sxs-lookup"><span data-stu-id="7689e-126">Add code to read the configuration file</span></span>

<span data-ttu-id="7689e-127">Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="7689e-127">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="7689e-128">Selezionare **Program.cs** nell'editor.</span><span class="sxs-lookup"><span data-stu-id="7689e-128">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="7689e-129">Nella parte superiore del file è presente la riga **using System;**.</span><span class="sxs-lookup"><span data-stu-id="7689e-129">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="7689e-130">Al di sotto di questa riga aggiungere le righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="7689e-130">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="7689e-131">Sostituire il contenuto del metodo **Main** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="7689e-131">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="7689e-132">Questo codice inizializza il sistema di configurazione per la lettura dal file **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="7689e-132">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

<span data-ttu-id="7689e-133">Il file **Program.cs** avrà ora un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7689e-133">Your **Program.cs** file should now look like the following:</span></span>

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

<span data-ttu-id="7689e-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="7689e-134">::: zone-end</span></span>

<span data-ttu-id="7689e-135">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="7689e-135">::: zone pivot="javascript"</span></span>

<span data-ttu-id="7689e-136">Si procederà ad aggiungere all'applicazione Node.js il supporto per recuperare una stringa di connessione da un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7689e-136">Let's add support to our Node.js application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="7689e-137">Per iniziare si aggiungeranno i componenti di base necessari per gestire la configurazione dal file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7689e-137">We'll start by adding the necessary plumbing to manage configuration from our JavaScript file.</span></span>

## <a name="create-a-env-configuration-file"></a><span data-ttu-id="7689e-138">Creare un file di configurazione con estensione env</span><span class="sxs-lookup"><span data-stu-id="7689e-138">Create a .env configuration file</span></span>

1. <span data-ttu-id="7689e-139">Assicurarsi di essere nella directory di lavoro corretta per il progetto.</span><span class="sxs-lookup"><span data-stu-id="7689e-139">Make sure you are in the correct working directory for your project.</span></span>

1. <span data-ttu-id="7689e-140">Usare lo strumento `touch` nella riga di comando per creare un file con estensione **env**.</span><span class="sxs-lookup"><span data-stu-id="7689e-140">Use the `touch` tool on the command line to create a file named **.env**.</span></span>

    ```bash
    touch .env
    ```

1. <span data-ttu-id="7689e-141">Aprire il progetto nell'editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="7689e-141">Open the project in the Cloud Shell editor.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="7689e-142">Selezionare il file con estensione **env** nell'editor e aggiungere il testo seguente.</span><span class="sxs-lookup"><span data-stu-id="7689e-142">Select the **.env** file in the editor and add the following text.</span></span> <span data-ttu-id="7689e-143">Può essere necessario fare clic sul pulsante Aggiorna nel codice per visualizzare i nuovi file.</span><span class="sxs-lookup"><span data-stu-id="7689e-143">You may need to click the refresh button in code to see the new files.</span></span> <span data-ttu-id="7689e-144">Salvare il file. Nell'angolo superiore destro dell'editor online è disponibile un menu con le operazioni più comuni eseguite sui file.</span><span class="sxs-lookup"><span data-stu-id="7689e-144">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > <span data-ttu-id="7689e-145">Il valore **AZURE_STORAGE_CONNECTION_STRING** è una variabile di ambiente hardcoded usata dalle API di archiviazione per cercare le chiavi di accesso.</span><span class="sxs-lookup"><span data-stu-id="7689e-145">The **AZURE_STORAGE_CONNECTION_STRING** value is a hard-coded environment variable used for Storage APIs to look up access keys.</span></span> <span data-ttu-id="7689e-146">Se si preferisce, è possibile usare un nome personalizzato, ma è necessario specificare il nome quando si crea l'oggetto `BlobService` nell'app Node.js.</span><span class="sxs-lookup"><span data-stu-id="7689e-146">You can use your own name if you prefer, but you must supply the name to the when you create the `BlobService` object in your Node.js app.</span></span>

1. <span data-ttu-id="7689e-147">È ora necessario ottenere la stringa di connessione dell'account di archiviazione e inserirla nella configurazione per l'app.</span><span class="sxs-lookup"><span data-stu-id="7689e-147">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span> <span data-ttu-id="7689e-148">In Cloud Shell eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="7689e-148">In Cloud Shell run the following command.</span></span>

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. <span data-ttu-id="7689e-149">Copiare la stringa di connessione restituita dal comando, senza le virgolette, e sostituire `<value>` nel file con estensione **env** con questa stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="7689e-149">Copy the connection string that is returned from that command, minus the quotes, and replace `<value>` in the **.env** file with this connection string.</span></span>

1. <span data-ttu-id="7689e-150">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7689e-150">Save the file.</span></span>

## <a name="add-support-to-read-an-environment-configuration-file"></a><span data-ttu-id="7689e-151">Aggiungere il supporto per la lettura di un file di configurazione dell'ambiente</span><span class="sxs-lookup"><span data-stu-id="7689e-151">Add support to read an environment configuration file</span></span>

<span data-ttu-id="7689e-152">Per includere nelle app Node.js il supporto per la lettura del file con estensione **env**, è possibile aggiungere il pacchetto **dotenv**.</span><span class="sxs-lookup"><span data-stu-id="7689e-152">Node.js apps can include support to read from the **.env** file by adding the **dotenv** package.</span></span>

1. <span data-ttu-id="7689e-153">Nella sezione relativa al prompt dei comandi della finestra aggiungere una dipendenza al pacchetto *dotenv*\* tramite `npm`.</span><span class="sxs-lookup"><span data-stu-id="7689e-153">In the command prompt section of the window, add a dependency to the  *dotenv*\* package using `npm`.</span></span>

    ```bash
    npm install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="7689e-154">Aggiungere codice per la lettura del file di configurazione</span><span class="sxs-lookup"><span data-stu-id="7689e-154">Add code to read the configuration file</span></span>

<span data-ttu-id="7689e-155">Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7689e-155">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our application.</span></span>

1. <span data-ttu-id="7689e-156">Selezionare *index.js*\* nell'editor.</span><span class="sxs-lookup"><span data-stu-id="7689e-156">Select *index.js*\* in the editor.</span></span>

1. <span data-ttu-id="7689e-157">Nella parte superiore del file è presente la riga **#!/usr/bin/env node**.</span><span class="sxs-lookup"><span data-stu-id="7689e-157">At the top of the file, a **#!/usr/bin/env node** line is present.</span></span> <span data-ttu-id="7689e-158">Al di sotto di questa riga aggiungere un'istruzione `require` per caricare il pacchetto **dotenv**.</span><span class="sxs-lookup"><span data-stu-id="7689e-158">Underneath that line, add a `require` statement to load the **dotenv** package.</span></span> <span data-ttu-id="7689e-159">In questo modo si rendono le variabili di ambiente definite nel file **.env** disponibili per il programma.</span><span class="sxs-lookup"><span data-stu-id="7689e-159">This will make environment variables defined in our **.env** file available to the program.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
<span data-ttu-id="7689e-160">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="7689e-160">::: zone-end</span></span>

<span data-ttu-id="7689e-161">Ora che tutto è pronto, si può iniziare ad aggiungere codice per usare l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7689e-161">Now that we have that all wired up, we can start adding code to use our storage account.</span></span>