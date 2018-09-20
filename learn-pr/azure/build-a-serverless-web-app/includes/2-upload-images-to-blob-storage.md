<span data-ttu-id="a2961-101">L'applicazione che si sta creando è una raccolta di foto</span><span class="sxs-lookup"><span data-stu-id="a2961-101">The application you're building is a photo gallery.</span></span> <span data-ttu-id="a2961-102">che usa JavaScript lato client per chiamare API per caricare e visualizzare immagini.</span><span class="sxs-lookup"><span data-stu-id="a2961-102">It uses client-side JavaScript to call APIs to upload and display images.</span></span> <span data-ttu-id="a2961-103">In questa unità si creerà un'API usando una funzione serverless che genera un URL temporaneo per caricare un'immagine.</span><span class="sxs-lookup"><span data-stu-id="a2961-103">In this unit, you will create an API using a serverless function that generates a time-limited URL to upload an image.</span></span> <span data-ttu-id="a2961-104">L'applicazione Web usa l'URL generato per caricare un'immagine nell'archiviazione BLOB usando l'[API REST dell'archiviazione BLOB](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span><span class="sxs-lookup"><span data-stu-id="a2961-104">The web application uses this URL to upload an image to Blob storage using the [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span></span>

## <a name="create-a-blob-storage-container-for-images"></a><span data-ttu-id="a2961-105">Creare un contenitore di archiviazione BLOB per le immagini</span><span class="sxs-lookup"><span data-stu-id="a2961-105">Create a Blob storage container for images</span></span>

<span data-ttu-id="a2961-106">L'applicazione richiede un contenitore di archiviazione separato per caricare e ospitare immagini.</span><span class="sxs-lookup"><span data-stu-id="a2961-106">The application requires a separate storage container to upload and host images.</span></span>

<span data-ttu-id="a2961-107">Creare un nuovo contenitore denominato **images** nell'account di archiviazione con accesso pubblico a tutti i BLOB.</span><span class="sxs-lookup"><span data-stu-id="a2961-107">Create a new container in your Storage account named **images** in your Storage account with public access to all blobs.</span></span>

```azurecli
az storage container create \
    -n images \
    --account-name <storage account name> \
    --public-access blob
```

## <a name="create-a-function-app"></a><span data-ttu-id="a2961-108">Creare un'app per le funzioni</span><span class="sxs-lookup"><span data-stu-id="a2961-108">Create a function app</span></span>

<span data-ttu-id="a2961-109">Funzioni di Azure è un servizio per l'esecuzione di funzioni serverless.</span><span class="sxs-lookup"><span data-stu-id="a2961-109">Azure Functions is a service for running serverless functions.</span></span> <span data-ttu-id="a2961-110">Una funzione serverless può essere attivata (chiamata) da eventi come una richiesta HTTP o la creazione di un BLOB in un contenitore di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="a2961-110">A serverless function can be triggered (called) by events, such as an HTTP request, or when a blob is created in a storage container.</span></span>

<span data-ttu-id="a2961-111">Un'app per le funzioni di Azure è un contenitore per una o più funzioni serverless.</span><span class="sxs-lookup"><span data-stu-id="a2961-111">A function app is a container for one or more serverless functions.</span></span>

<span data-ttu-id="a2961-112">Creare una nuova app per le funzioni con un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="a2961-112">Create a new function app with a unique name.</span></span> <span data-ttu-id="a2961-113">Le app per le funzioni richiedono un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="a2961-113">Function apps require a Storage account.</span></span> <span data-ttu-id="a2961-114">In questa unità si userà l'account di archiviazione esistente creato nell'ultima unità.</span><span class="sxs-lookup"><span data-stu-id="a2961-114">In this unit, you will use the existing storage account you created in the last unit.</span></span>

```azurecli
az functionapp create \
    -n <function app name> \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -s <storage account name> \
    --consumption-plan-location $(az group show -n <rgn>[Sandbox resource group name]</rgn> --query location --output tsv)
```

## <a name="create-an-http-triggered-serverless-function"></a><span data-ttu-id="a2961-115">Creare una funzione serverless attivata da HTTP</span><span class="sxs-lookup"><span data-stu-id="a2961-115">Create an HTTP-triggered serverless function</span></span>

<span data-ttu-id="a2961-116">Per caricare un'immagine in modo sicuro nell'archiviazione BLOB, l'applicazione Web della raccolta foto invia alla funzione serverless una richiesta HTTP di generazione di un URL temporaneo.</span><span class="sxs-lookup"><span data-stu-id="a2961-116">To securely upload an image to Blob storage, the photo gallery web app makes an HTTP request to the serverless function to generate a time-limited URL.</span></span> <span data-ttu-id="a2961-117">La funzione viene attivata da una richiesta HTTP e usa Azure Storage SDK per generare e restituire l'URL protetto.</span><span class="sxs-lookup"><span data-stu-id="a2961-117">The function is triggered by an HTTP request and uses the Azure Storage SDK to generate and return the secure URL.</span></span>

1. <span data-ttu-id="a2961-118">Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui si è attivata la sandbox.</span><span class="sxs-lookup"><span data-stu-id="a2961-118">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="a2961-119">Cercare l'app per le funzioni appena creata tramite la casella **Cerca**.</span><span class="sxs-lookup"><span data-stu-id="a2961-119">Search for the function app you just created using the **Search** box.</span></span> <span data-ttu-id="a2961-120">Fare clic sul risultato per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="a2961-120">Click on the result to open it.</span></span>

    ![Aprire l'app per le funzioni](../media/2-search-function-app.png)

1. <span data-ttu-id="a2961-122">Nella finestra dell'app per le funzioni nel riquadro di spostamento a sinistra scegliere **Funzioni** e fare clic sul segno più (+) per creare una nuova funzione serverless.</span><span class="sxs-lookup"><span data-stu-id="a2961-122">In the left navigation of the Function Apps window, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span>

    ![Creare una nuova funzione](../media/2-new-function.png)

1. <span data-ttu-id="a2961-124">Fare clic su **Funzione personalizzata** per visualizzare un elenco di modelli di funzione.</span><span class="sxs-lookup"><span data-stu-id="a2961-124">Click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="a2961-125">Individuare il modello **HttpTrigger** e fare clic su C# o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a2961-125">Find the **HttpTrigger** template and click C# or JavaScript.</span></span>

1. <span data-ttu-id="a2961-126">Usare i valori seguenti per creare una funzione che genera un URL di caricamento BLOB:</span><span class="sxs-lookup"><span data-stu-id="a2961-126">Use the following values to create a function that generates a blob upload URL:</span></span>

    | <span data-ttu-id="a2961-127">Impostazione</span><span class="sxs-lookup"><span data-stu-id="a2961-127">Setting</span></span>      |  <span data-ttu-id="a2961-128">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="a2961-128">Suggested value</span></span>   | <span data-ttu-id="a2961-129">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a2961-129">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="a2961-130">**Linguaggio**</span><span class="sxs-lookup"><span data-stu-id="a2961-130">**Language**</span></span> | <span data-ttu-id="a2961-131">C# o JavaScript</span><span class="sxs-lookup"><span data-stu-id="a2961-131">C# or JavaScript</span></span> | <span data-ttu-id="a2961-132">Selezionare il linguaggio che si vuole usare.</span><span class="sxs-lookup"><span data-stu-id="a2961-132">Select the language that you want to use.</span></span> |
    | <span data-ttu-id="a2961-133">**Assegnare un nome alla funzione**</span><span class="sxs-lookup"><span data-stu-id="a2961-133">**Name your function**</span></span> | <span data-ttu-id="a2961-134">GetUploadUrl</span><span class="sxs-lookup"><span data-stu-id="a2961-134">GetUploadUrl</span></span> | <span data-ttu-id="a2961-135">Immettere questo nome esattamente come illustrato in modo che l'applicazione possa individuare la funzione.</span><span class="sxs-lookup"><span data-stu-id="a2961-135">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="a2961-136">**Livello di autorizzazione**</span><span class="sxs-lookup"><span data-stu-id="a2961-136">**Authorization level**</span></span> | <span data-ttu-id="a2961-137">Anonimo</span><span class="sxs-lookup"><span data-stu-id="a2961-137">Anonymous</span></span> | <span data-ttu-id="a2961-138">Permette l'accesso pubblico alla funzione.</span><span class="sxs-lookup"><span data-stu-id="a2961-138">Allows the function to be accessed publicly.</span></span> |

    ![Immettere le impostazioni per una nuova funzione attivata da HTTP](../media/2-new-function-httptrigger.png)

1. <span data-ttu-id="a2961-140">Fare clic su **Crea** per creare la funzione.</span><span class="sxs-lookup"><span data-stu-id="a2961-140">Click **Create** to create the function.</span></span>

<span data-ttu-id="a2961-141">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="a2961-141">::: zone pivot="csharp"</span></span>

8. <span data-ttu-id="a2961-142">Quando viene visualizzato il codice sorgente della funzione, sostituire l'intero contenuto del file **run.csx** con il contenuto del file [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span><span class="sxs-lookup"><span data-stu-id="a2961-142">When the function's source code appears, replace all of the content in the **run.csx** file with the content in the [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) file.</span></span>

1. <span data-ttu-id="a2961-143">Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.</span><span class="sxs-lookup"><span data-stu-id="a2961-143">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="a2961-144">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="a2961-144">Click **Save**.</span></span> <span data-ttu-id="a2961-145">Verificare nel pannello dei log che la funzione sia stata compilata correttamente.</span><span class="sxs-lookup"><span data-stu-id="a2961-145">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="a2961-146">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="a2961-146">::: zone-end</span></span>

<span data-ttu-id="a2961-147">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="a2961-147">::: zone pivot="javascript"</span></span>

8. <span data-ttu-id="a2961-148">Questa funzione richiede il pacchetto `azure-storage` di npm.</span><span class="sxs-lookup"><span data-stu-id="a2961-148">This function requires the `azure-storage` package from npm.</span></span> <span data-ttu-id="a2961-149">Il pacchetto genera il token di firma di accesso condiviso (SAS) necessario per compilare l'URL protetto.</span><span class="sxs-lookup"><span data-stu-id="a2961-149">The package generates the shared access signature (SAS) token that's required to build the secure URL.</span></span> <span data-ttu-id="a2961-150">Per installare il pacchetto npm, fare clic sull'app per le funzioni nel riquadro di spostamento di sinistra e fare clic su **Funzionalità della piattaforma**.</span><span class="sxs-lookup"><span data-stu-id="a2961-150">To install the npm package, click on the Functions app on the left navigation and click **Platform features**.</span></span>

1. <span data-ttu-id="a2961-151">Fare clic su **Console** per visualizzare una finestra della console.</span><span class="sxs-lookup"><span data-stu-id="a2961-151">Click **Console** to reveal a console window.</span></span>

    ![Aprire una finestra della console](../media/2-open-console.jpg)

1. <span data-ttu-id="a2961-153">Assicurarsi che la directory corrente sia **d:\home\site\wwwroot** eseguendo il comando `cd d:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="a2961-153">Ensure the current directory is **d:\home\site\wwwroot** by running the command `cd d:\home\site\wwwroot`.</span></span>

1. <span data-ttu-id="a2961-154">Eseguire il comando `npm init -y` per creare un file **package.json** vuoto.</span><span class="sxs-lookup"><span data-stu-id="a2961-154">Run the command `npm init -y` to create an empty **package.json** file.</span></span>

1. <span data-ttu-id="a2961-155">Eseguire il comando `npm install --save azure-storage` nella console per installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="a2961-155">To install the package, run the command `npm install --save azure-storage` in the console.</span></span> <span data-ttu-id="a2961-156">Salvare il pacchetto come **package.json**.</span><span class="sxs-lookup"><span data-stu-id="a2961-156">Save the package as **package.json**.</span></span> <span data-ttu-id="a2961-157">Il completamento dell'operazione potrebbe richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="a2961-157">It may take a few minutes to complete the operation.</span></span>

1. <span data-ttu-id="a2961-158">Fare clic sulla funzione (**GetUploadUrl**) nel riquadro di spostamento a sinistra per visualizzare la funzione.</span><span class="sxs-lookup"><span data-stu-id="a2961-158">Click on the function (**GetUploadUrl**) in the left navigation to reveal the function.</span></span> <span data-ttu-id="a2961-159">Sostituire tutto il contenuto del file **index.js** con il contenuto del file [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span><span class="sxs-lookup"><span data-stu-id="a2961-159">Replace all of the content in the **index.js** file with the content in the [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) file.</span></span>

    ![Contenuto di index.js dopo l'aggiornamento](../media/2-paste-js.jpg)

1. <span data-ttu-id="a2961-161">Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.</span><span class="sxs-lookup"><span data-stu-id="a2961-161">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="a2961-162">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="a2961-162">Click **Save**.</span></span> <span data-ttu-id="a2961-163">Verificare nel pannello dei log che la funzione sia stata compilata correttamente.</span><span class="sxs-lookup"><span data-stu-id="a2961-163">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="a2961-164">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="a2961-164">::: zone-end</span></span>

<span data-ttu-id="a2961-165">La funzione genera un URL di firma di accesso condiviso, usato per caricare un file nell'archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="a2961-165">The function generates a shared access signature (SAS) URL that's used to upload a file to Blob storage.</span></span> <span data-ttu-id="a2961-166">L'URL di firma di accesso condiviso è valido per un breve periodo di tempo e permette il caricamento di un solo file.</span><span class="sxs-lookup"><span data-stu-id="a2961-166">The SAS URL is valid for a short time and only allows a single file to be uploaded.</span></span> <span data-ttu-id="a2961-167">Vedere la documentazione dell'archiviazione BLOB per altre informazioni sull'[uso di firme di accesso condiviso](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="a2961-167">Consult the Blob storage documentation for more information on [how to use shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span></span>


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a><span data-ttu-id="a2961-168">Aggiungere una variabile di ambiente per la stringa di connessione di archiviazione</span><span class="sxs-lookup"><span data-stu-id="a2961-168">Add an environment variable for the storage connection string</span></span>

<span data-ttu-id="a2961-169">La funzione appena creata richiede una stringa di connessione per l'account di archiviazione in modo da poter generare l'URL SAS.</span><span class="sxs-lookup"><span data-stu-id="a2961-169">The function that you created requires a connection string for the Storage account so that it can generate the SAS URL.</span></span> <span data-ttu-id="a2961-170">Invece di codificare la stringa di connessione come hardcoded nel corpo della funzione, è possibile archiviarla come impostazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a2961-170">Instead of hardcoding the connection string in the function body, it can be stored as an application setting.</span></span> <span data-ttu-id="a2961-171">Le impostazioni dell'applicazione sono accessibili come variabili di ambiente da tutte le funzioni nell'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="a2961-171">Application settings are accessible as environment variables by all functions in the function app.</span></span>

1. <span data-ttu-id="a2961-172">In Cloud Shell recuperare la stringa di connessione dell'account di archiviazione e salvarla in una variabile Bash denominata **STORAGE_CONNECTION_STRING**.</span><span class="sxs-lookup"><span data-stu-id="a2961-172">In Cloud Shell, query the Storage account connection string and save it to a Bash variable named **STORAGE_CONNECTION_STRING**.</span></span>

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn> --query "connectionString" --output tsv)
    ```

    <span data-ttu-id="a2961-173">Verificare che la variabile sia impostata correttamente.</span><span class="sxs-lookup"><span data-stu-id="a2961-173">Confirm the variable is set successfully.</span></span>

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. <span data-ttu-id="a2961-174">Creare una nuova impostazione dell'applicazione denominata **AZURE_STORAGE_CONNECTION_STRING** usando il valore salvato nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="a2961-174">Create a new application setting named **AZURE_STORAGE_CONNECTION_STRING** using the value saved from the previous step.</span></span>

    ```azurecli
    az functionapp config appsettings set \
        -n <function app name> \
        -g <rgn>[Sandbox resource group name]</rgn> \
        --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING \
        -o table
    ```

    <span data-ttu-id="a2961-175">Verificare che l'output del comando contenga la nuova impostazione dell'applicazione con il valore corretto.</span><span class="sxs-lookup"><span data-stu-id="a2961-175">Confirm that the command's output contains the new application setting with the correct value.</span></span>

## <a name="test-the-serverless-function"></a><span data-ttu-id="a2961-176">Testare la funzione serverless</span><span class="sxs-lookup"><span data-stu-id="a2961-176">Test the serverless function</span></span>

<span data-ttu-id="a2961-177">Oltre alla creazione e alla modifica delle funzioni, il portale di Azure offre uno strumento predefinito anche per il test delle funzioni.</span><span class="sxs-lookup"><span data-stu-id="a2961-177">In addition to creating and editing functions, the Azure portal also provides a built-in tool for testing functions.</span></span>

1. <span data-ttu-id="a2961-178">Per testare la funzione HTTP serverless, fare clic sulla scheda **Test** sulla destra della finestra del codice per espandere il pannello di test.</span><span class="sxs-lookup"><span data-stu-id="a2961-178">To test the HTTP serverless function, on the right of the code window, click on the **Test** tab to expand the test panel.</span></span>

1. <span data-ttu-id="a2961-179">Modificare **Metodo HTTP** in **GET**.</span><span class="sxs-lookup"><span data-stu-id="a2961-179">Change the **Http method** to **GET**.</span></span>

1. <span data-ttu-id="a2961-180">In **Query** fare clic su **Aggiungi parametro** e aggiungere il parametro seguente:</span><span class="sxs-lookup"><span data-stu-id="a2961-180">Under **Query**, click **Add parameter** and add the following parameter:</span></span>

    | <span data-ttu-id="a2961-181">Nome</span><span class="sxs-lookup"><span data-stu-id="a2961-181">Name</span></span>      |  <span data-ttu-id="a2961-182">Valore</span><span class="sxs-lookup"><span data-stu-id="a2961-182">Value</span></span>   | 
    | --- | --- |
    | <span data-ttu-id="a2961-183">**filename**</span><span class="sxs-lookup"><span data-stu-id="a2961-183">**filename**</span></span> | <span data-ttu-id="a2961-184">image1.jpg</span><span class="sxs-lookup"><span data-stu-id="a2961-184">image1.jpg</span></span> |

1. <span data-ttu-id="a2961-185">Fare clic su **Esegui** nel pannello di test per inviare una richiesta HTTP alla funzione.</span><span class="sxs-lookup"><span data-stu-id="a2961-185">In the test panel, click **Run** to send an HTTP request to the function.</span></span>

1. <span data-ttu-id="a2961-186">La funzione restituisce un URL di caricamento nell'output.</span><span class="sxs-lookup"><span data-stu-id="a2961-186">The function returns an upload URL in the output.</span></span> <span data-ttu-id="a2961-187">L'esecuzione della funzione viene visualizzata nel pannello dei log.</span><span class="sxs-lookup"><span data-stu-id="a2961-187">The function execution appears in the Logs panel.</span></span>

    ![Log che indicano l'esecuzione corretta della funzione](../media/2-test-function.png)

## <a name="configure-cors-in-the-function-app"></a><span data-ttu-id="a2961-189">Configurare CORS nell'app per le funzioni</span><span class="sxs-lookup"><span data-stu-id="a2961-189">Configure CORS in the function app</span></span>

<span data-ttu-id="a2961-190">Poiché il front-end della funzione è ospitato nell'archiviazione BLOB, ha un nome di dominio diverso rispetto all'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="a2961-190">Because the function front end is hosted in Blob storage, it has a different domain name than the function app.</span></span> <span data-ttu-id="a2961-191">Per consentire al codice JavaScript lato client di chiamare correttamente la funzione appena creata, l'app per le funzioni deve essere configurata per la condivisione di risorse tra le origini (CORS).</span><span class="sxs-lookup"><span data-stu-id="a2961-191">For the client-side JavaScript to successfully call the function that you created, the function app has to be configured for cross-origin resource sharing (CORS).</span></span>

1. <span data-ttu-id="a2961-192">Nella barra di spostamento di sinistra della finestra dell'app per le funzioni, fare clic sul nome dell'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="a2961-192">In the left navigation of the Function Apps window, click on the name of your function app.</span></span>

1. <span data-ttu-id="a2961-193">Fare clic su **Funzionalità della piattaforma** per visualizzare un elenco di funzionalità avanzate.</span><span class="sxs-lookup"><span data-stu-id="a2961-193">Click on **Platform features** to view a list of advanced features.</span></span>

1. <span data-ttu-id="a2961-194">Sotto **API** fare clic su **CORS**.</span><span class="sxs-lookup"><span data-stu-id="a2961-194">Under **API**, click **CORS**.</span></span>

    ![Selezionare CORS](../media/2-open-cors.jpg)

1. <span data-ttu-id="a2961-196">Aggiungere un'origine consentita per l'URL del sito Web creato nell'unità precedente, omettendo la barra finale (/).</span><span class="sxs-lookup"><span data-stu-id="a2961-196">Add an allow origin for the URL of your web site you created in the previous unit and omit the trailing slash (/).</span></span> <span data-ttu-id="a2961-197">Ad esempio: `https://firstserverlessweb.z4.web.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="a2961-197">For example: `https://firstserverlessweb.z4.web.core.windows.net`.</span></span>

    ![Impostazioni CORS che mostrano l'aggiunta dell'URL dell'app Web serverless](../media/2-add-cors.png)

1. <span data-ttu-id="a2961-199">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="a2961-199">Click **Save**.</span></span>

<span data-ttu-id="a2961-200">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="a2961-200">::: zone pivot="javascript"</span></span>

6. <span data-ttu-id="a2961-201">Sempre nel portale di Azure, passare alla finestra dell'app per le funzioni e fare clic sul nome dell'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="a2961-201">Still in the Azure portal, navigate to the Function Apps window and click on the name of your function app.</span></span> <span data-ttu-id="a2961-202">Selezionare la scheda **Panoramica**. Fare clic su **Riavvia** per assicurarsi che le modifiche per CORS diventino effettive.</span><span class="sxs-lookup"><span data-stu-id="a2961-202">Select the **Overview** tab. Click **Restart** to make sure that the changes for CORS take effect.</span></span>

<span data-ttu-id="a2961-203">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="a2961-203">::: zone-end</span></span>

<span data-ttu-id="a2961-204">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="a2961-204">::: zone pivot="csharp"</span></span>

6. <span data-ttu-id="a2961-205">Tornare alla funzione `GetUploadUrl` e selezionare la scheda **Integrazione**.</span><span class="sxs-lookup"><span data-stu-id="a2961-205">Navigate back to the `GetUploadUrl` function and select the **Integrate** tab.</span></span>

1. <span data-ttu-id="a2961-206">In **Metodi HTTP selezionati** selezionare **OPTIONS**.</span><span class="sxs-lookup"><span data-stu-id="a2961-206">Under **Selected HTTP methods**, select **OPTIONS**.</span></span>

    <span data-ttu-id="a2961-207">**GET**, **POST** e **OPTIONS** devono essere tutti selezionati.</span><span class="sxs-lookup"><span data-stu-id="a2961-207">**GET**, **POST**, and **OPTIONS** should all be selected.</span></span> <span data-ttu-id="a2961-208">CORS usa il metodo **OPTIONS**, che non è selezionato per impostazione predefinita per le funzioni C#.</span><span class="sxs-lookup"><span data-stu-id="a2961-208">CORS uses the **OPTIONS** method, which isn't selected by default for C# functions.</span></span>  

1. <span data-ttu-id="a2961-209">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="a2961-209">Click **Save**.</span></span>

1. <span data-ttu-id="a2961-210">Sempre nel portale di Azure, passare alla finestra dell'app per le funzioni e fare clic sul nome dell'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="a2961-210">Still in the Azure portal, navigate to the Function Apps window and click on the name of your function app.</span></span> <span data-ttu-id="a2961-211">Selezionare la scheda **Panoramica**. Fare clic su **Riavvia** per assicurarsi che le modifiche per CORS diventino effettive.</span><span class="sxs-lookup"><span data-stu-id="a2961-211">Select the **Overview** tab. Click **Restart** to make sure that the changes for CORS take effect.</span></span>

<span data-ttu-id="a2961-212">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="a2961-212">::: zone-end</span></span>

## <a name="configure-cors-in-the-storage-account"></a><span data-ttu-id="a2961-213">Configurare CORS nell'account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="a2961-213">Configure CORS in the Storage account</span></span>

<span data-ttu-id="a2961-214">Poiché l'app per le funzioni esegue anche chiamate JavaScript lato client all'archiviazione BLOB per caricare i file, è necessario configurare l'account di archiviazione per CORS.</span><span class="sxs-lookup"><span data-stu-id="a2961-214">Because the function app also makes client-side JavaScript calls to Blob storage to upload files, you have to configure the Storage account for CORS.</span></span>

- <span data-ttu-id="a2961-215">Eseguire il comando seguente per consentire a tutte le origini di caricare file nell'account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="a2961-215">Run the following command to allow all origins to upload files to the Storage account:</span></span>

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```

## <a name="modify-the-web-app-to-upload-images"></a><span data-ttu-id="a2961-216">Modificare l'app Web per caricare immagini</span><span class="sxs-lookup"><span data-stu-id="a2961-216">Modify the web app to upload images</span></span>

<span data-ttu-id="a2961-217">L'app Web recupera le impostazioni da un file denominato **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="a2961-217">The web app retrieves settings from a file named **settings.js**.</span></span> <span data-ttu-id="a2961-218">Nella procedura seguente viene creato il file usando Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="a2961-218">In the following steps, you create the file using Cloud Shell.</span></span> <span data-ttu-id="a2961-219">Impostare `window.apiBaseUrl` sull'URL dell'app per le funzioni e `window.blobBaseUrl` sull'URL dell'endpoint di archiviazione BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="a2961-219">You set `window.apiBaseUrl` to the URL of the function app, and `window.blobBaseUrl` to the URL of the Azure Blob storage endpoint.</span></span>

1. <span data-ttu-id="a2961-220">In Cloud Shell assicurarsi che la directory corrente sia la cartella **www/dist**.</span><span class="sxs-lookup"><span data-stu-id="a2961-220">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```bash
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="a2961-221">Aprire l'editor Cloud Shell nella directory corrente digitando il comando `code .`, incluso il punto.</span><span class="sxs-lookup"><span data-stu-id="a2961-221">Open the Cloud Shell Editor in the current directory by typing the command `code .`, including the dot.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="a2961-222">Nella finestra di Cloud Shell sotto l'editor recuperare l'URL dell'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="a2961-222">In the Cloud Shell window below the editor, query the function app's URL.</span></span>

    ```azurecli
    echo "https://"$(az functionapp show -n <function app name> -g <rgn>[Sandbox resource group name]</rgn> --query "defaultHostName" --output tsv)
    ```

1. <span data-ttu-id="a2961-223">Aggiungere la riga seguente nella finestra dell'editor, usando l'URL dell'app per le funzioni recuperato nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="a2961-223">Add the following line into the editor window, using the function app URL you retrieved in the previous step.</span></span>

    ```bash
    window.apiBaseUrl = '<function app url>'
    ```

1. <span data-ttu-id="a2961-224">Nella finestra di Cloud Shell sotto l'editor recuperare l'URL dell'endpoint di Archiviazione BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="a2961-224">In the Cloud Shell window below the editor, query the Azure Blob Storage endpoint URL.</span></span>

    ```azurecli
    echo $(az storage account show -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn> --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

1. <span data-ttu-id="a2961-225">Aggiungere una seconda riga nella finestra dell'editor, usando l'URL dell'endpoint di archiviazione recuperato nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="a2961-225">Append a second line into the editor window, using the Storage endpoint URL you retrieved in the previous step.</span></span>

    ```bash
    window.blobBaseUrl = '<blob storage endpoint url>'
    ```

1. <span data-ttu-id="a2961-226">Salvare il file come **settings.js** e chiudere l'editor.</span><span class="sxs-lookup"><span data-stu-id="a2961-226">Save the file as **settings.js** and close the editor.</span></span>

1. <span data-ttu-id="a2961-227">Verificare che il file sia stato scritto correttamente e che ora contenga 2 righe.</span><span class="sxs-lookup"><span data-stu-id="a2961-227">Confirm the file was successfully written and it now contains 2 lines.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="a2961-228">Caricare il file nell'archivio BLOB.</span><span class="sxs-lookup"><span data-stu-id="a2961-228">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload \
        -c \$web \
        --account-name <storage account name> \
        -f settings.js \
        -n settings.js
    ```

## <a name="test-the-web-application"></a><span data-ttu-id="a2961-229">Testare l'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="a2961-229">Test the web application</span></span>

<span data-ttu-id="a2961-230">A questo punto, l'applicazione della raccolta può caricare un'immagine nell'archiviazione BLOB, ma non può ancora visualizzare le immagini.</span><span class="sxs-lookup"><span data-stu-id="a2961-230">At this point, the gallery application is able to upload an image to Blob storage, but it can't display images yet.</span></span> <span data-ttu-id="a2961-231">Tenterà di chiamare una funzione `GetImages` che non esiste ancora perché verrà creata in un modulo successivo.</span><span class="sxs-lookup"><span data-stu-id="a2961-231">It will try to call a `GetImages` function that doesn't exist yet because you create it in a later module.</span></span> <span data-ttu-id="a2961-232">La chiamata avrà esito negativo e la pagina Web apparirà bloccata su "Analisi", ma l'immagine selezionata verrà caricata correttamente.</span><span class="sxs-lookup"><span data-stu-id="a2961-232">The call will fail and the web page will appear to be stuck on "Analyzing...," but the image you select will be successfully uploaded.</span></span>

<span data-ttu-id="a2961-233">È possibile verificare che un'immagine sia stata caricata correttamente controllando il contenuto del contenitore **images** nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="a2961-233">You can verify that an image is successfully uploaded by checking the contents of the **images** container in the Azure portal.</span></span>

1. <span data-ttu-id="a2961-234">In una finestra del browser passare all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a2961-234">In a browser window, browse to the application.</span></span> <span data-ttu-id="a2961-235">Selezionare un file di immagine e caricarlo.</span><span class="sxs-lookup"><span data-stu-id="a2961-235">Select an image file and upload it.</span></span> <span data-ttu-id="a2961-236">Il file viene caricato ma, poiché non è stata ancora aggiunta la possibilità di visualizzare le immagini, la foto caricata non viene visualizzata nell'app.</span><span class="sxs-lookup"><span data-stu-id="a2961-236">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span> <span data-ttu-id="a2961-237">La pagina Web appare bloccata su "Analyzing image..." (Analisi immagine in corso...). Questo problema verrà risolto in seguito.</span><span class="sxs-lookup"><span data-stu-id="a2961-237">(The web page appears to be stuck on "Analyzing image..." You'll fix that later.)</span></span>

1. <span data-ttu-id="a2961-238">In Cloud Shell verificare che l'immagine sia stata caricata nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="a2961-238">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list \
        --account-name <storage account name> \
        -c images \
        -o table
    ```

1. <span data-ttu-id="a2961-239">Prima di passare all'unità successiva, eliminare tutti i file nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="a2961-239">Before moving on to the next unit, delete all files in the **images** container.</span></span>

    ```azurecli
    az storage blob delete-batch \
        -s images \
        --account-name <storage account name>
    ```

## <a name="summary"></a><span data-ttu-id="a2961-240">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a2961-240">Summary</span></span>

<span data-ttu-id="a2961-241">In questa unità è stata creata un'app Funzioni di Azure ed è stato descritto come usare una funzione serverless per consentire a un'applicazione Web di caricare immagini nell'archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="a2961-241">In this unit, you created an Azure Functions app and learned how to use a serverless function to allow a web application to upload images to Blob storage.</span></span> <span data-ttu-id="a2961-242">Si osserverà ora come creare anteprime per le immagini caricate usando una funzione serverless attivata dal BLOB.</span><span class="sxs-lookup"><span data-stu-id="a2961-242">Next, you will learn how to create thumbnails for the uploaded images using a blob-triggered serverless function.</span></span>