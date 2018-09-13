<span data-ttu-id="eb66e-101">In questa unità verrà recuperata la chiave di accesso e la si aggiungerà alla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="eb66e-101">In this unit you will retrieve the access key and add it to your app configuration.</span></span> <span data-ttu-id="eb66e-102">Inoltre, poiché è stata creata un'applicazione console .NET Core, è necessario aggiungere all'applicazione il supporto per la lettura da file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="eb66e-102">In addition, since we created a .NET Core console application, we need to add support for reading from configuration files to the application.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="eb66e-103">Creare un file di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="eb66e-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="eb66e-104">Nel progetto di Visual Studio creato nell'unità precedente fare clic con il pulsante destro del mouse sul progetto, scegliere **Aggiungi**, quindi **Nuovo elemento** (oppure premere CTRL+MAIUSC+A).</span><span class="sxs-lookup"><span data-stu-id="eb66e-104">In your Visual Studio project that we created in the previous unit, right click the project, select **Add**, and then select **New Item** (or hold down CTRL-SHIFT-A).</span></span>

1. <span data-ttu-id="eb66e-105">Verrà visualizzata una finestra di dialogo modale.</span><span class="sxs-lookup"><span data-stu-id="eb66e-105">A modal dialog is presented.</span></span> <span data-ttu-id="eb66e-106">Selezionare **File di configurazione JSON per JavaScript** e digitare **appsettings.json** nella casella di testo *Nome*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-106">Select **JavaScript JSON Configuration File** and type **appsettings.json** into the *Name* textbox, and then click **Add**.</span></span>

  ![Impostazioni app](..\media-draft\7-appsettings.png)

1. <span data-ttu-id="eb66e-108">Viene aggiunto un file al progetto con contenuto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="eb66e-108">A file is added to the project with contents similar to the following:</span></span>

    ```json
    {
      "exclude": [
        "**/bin",
        "**/bower_components",
        "**/jspm_packages",
        "**/node_modules",
        "**/obj",
        "**/platforms"
      ]
    }
    ```
1. <span data-ttu-id="eb66e-109">Rimuovere la voce **exclude** e il contenuto associato e sostituirla con una singola voce **StorageAccountConnectionString** con un valore di stringa vuoto.</span><span class="sxs-lookup"><span data-stu-id="eb66e-109">Remove the **exclude** entry and its associated contents and replace it with the single entry **StorageAccountConnectionString**, with an empty string value.</span></span> <span data-ttu-id="eb66e-110">La voce deve essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="eb66e-110">It should look similar to the following:</span></span>

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```
1. <span data-ttu-id="eb66e-111">Selezionare il file **appsettings.json**, fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** (in alternativa, premere ALT+INVIO o F4).</span><span class="sxs-lookup"><span data-stu-id="eb66e-111">Select the **appsettings.json** file, right click it and select **Properties** (alternatively, press ALT+ENTER or F4).</span></span>

1. <span data-ttu-id="eb66e-112">Modificare il valore di **Copia nella directory di output** in **Copia se più recente**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-112">Change **Copy to Output Directory** to **Copy if newer**.</span></span>

  ![Azione di compilazione](..\media-draft\7-build-action.png)

  > <span data-ttu-id="eb66e-114">In questo modo, il file di configurazione dell'app viene inserito nella directory di output alla creazione/compilazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="eb66e-114">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="eb66e-115">Aggiungere il supporto per la lettura di un file di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="eb66e-115">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="eb66e-116">Un'applicazione console .NET Core richiede l'aggiunta di determinate librerie per permettere la lettura da un file di configurazione JSON in tutta semplicità.</span><span class="sxs-lookup"><span data-stu-id="eb66e-116">A .NET Core console application requires certain libraries to be added to allow it to easily read from a JSON configuration file.</span></span>

1. <span data-ttu-id="eb66e-117">Fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-117">Right click on the project and select **Manage Nuget Packages…**.</span></span>

![Gestisci pacchetti NuGet](..\media-draft\5-manage-nuget-packages.png)

1. <span data-ttu-id="eb66e-119">Verrà visualizzata una pagina che mostra i pacchetti NuGet attualmente installati.</span><span class="sxs-lookup"><span data-stu-id="eb66e-119">A page is displayed, showing currently installed Nuget packages.</span></span> <span data-ttu-id="eb66e-120">Fare clic sull'opzione **Sfoglia** e digitare **Microsoft.Extensions.Configuration.Json** nel campo di ricerca.</span><span class="sxs-lookup"><span data-stu-id="eb66e-120">Click on the **Browse** option and type **Microsoft.Extensions.Configuration.Json** in the search field.</span></span> <span data-ttu-id="eb66e-121">Nei risultati della ricerca viene visualizzato il pacchetto **Microsoft.Extensions.Configuration.Json**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-121">The **Microsoft.Extensions.Configuration.Json** package is displayed in the search results.</span></span>

1. <span data-ttu-id="eb66e-122">Selezionare il pacchetto **Microsoft.Extensions.Configuration.Json** e nel riquadro destro selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-122">Select the **Microsoft.Extensions.Configuration.Json** package, and in the right pane select **Install**.</span></span>

1. <span data-ttu-id="eb66e-123">Verrà visualizzata la finestra di dialogo **Anteprima modifiche**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-123">A **Preview Changes** dialog is displayed.</span></span> <span data-ttu-id="eb66e-124">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-124">Click **OK**.</span></span>

1. <span data-ttu-id="eb66e-125">Verrà visualizzata una finestra di dialogo per l'accettazione della licenza.</span><span class="sxs-lookup"><span data-stu-id="eb66e-125">A license acceptance dialog is displayed.</span></span> <span data-ttu-id="eb66e-126">Fare clic su **Accetto**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-126">Click **I Accept**.</span></span>


## <a name="read-from-the-configuration-file"></a><span data-ttu-id="eb66e-127">Lettura dal file di configurazione</span><span class="sxs-lookup"><span data-stu-id="eb66e-127">Read from the configuration file</span></span>

<span data-ttu-id="eb66e-128">Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="eb66e-128">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="eb66e-129">Individuare il file **Program.cs** nel progetto e aprirlo in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb66e-129">Locate the **Program.cs** file in your project and open it in Visual Studio.</span></span>

1. <span data-ttu-id="eb66e-130">Nella parte superiore del file è presente la riga **using System;**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-130">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="eb66e-131">Al di sotto di questa riga aggiungere le righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="eb66e-131">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="eb66e-132">Sostituire il contenuto del metodo **Main** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="eb66e-132">Replace the contents of the **Main** method with the following code:</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

1. <span data-ttu-id="eb66e-133">Il file **Program.cs** avrà ora un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="eb66e-133">Your **Program.cs** file should now look like the following:</span></span>

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;

    namespace DemoConsoleApp
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

> <span data-ttu-id="eb66e-134">Questo codice inizializza il sistema di configurazione per la lettura dal file **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-134">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

## <a name="add-your-connection-string-to-the-configuration-file"></a><span data-ttu-id="eb66e-135">Aggiungere la stringa di connessione al file di configurazione</span><span class="sxs-lookup"><span data-stu-id="eb66e-135">Add your connection string to the configuration file</span></span>

<span data-ttu-id="eb66e-136">È ora necessario ottenere la stringa di connessione dell'account di archiviazione e aggiungerla nella configurazione per l'app.</span><span class="sxs-lookup"><span data-stu-id="eb66e-136">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span>

1. <span data-ttu-id="eb66e-137">Nel portale di Azure passare alla sottoscrizione che contiene l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="eb66e-137">Navigate to the Azure portal for your subscription that contains your storage account.</span></span>

1. <span data-ttu-id="eb66e-138">Passare all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="eb66e-138">Navigate to your storage account.</span></span>

1. <span data-ttu-id="eb66e-139">Passare al pannello Chiavi dell'account dell'account di archiviazione nel portale.</span><span class="sxs-lookup"><span data-stu-id="eb66e-139">Navigate to the Account Keys blade of the storage account in the portal.</span></span>

1. <span data-ttu-id="eb66e-140">Copiare la stringa di connessione **key1**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-140">Copy the **key1** connection string.</span></span>

1. <span data-ttu-id="eb66e-141">Incollare il contenuto della chiave di accesso copiata dal portale come valore per **StorageAccountConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="eb66e-141">Paste in the contents of the access key you copied from the portal as the value for **StorageAccountConnectionString**.</span></span> <span data-ttu-id="eb66e-142">La configurazione sarà ora simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="eb66e-142">Your configuration should now look similar to the following:</span></span>

    ```json
    {
      "StorageAccountConnectionString": "your-access-key-connection-string-goes-here"
    }
    ```
