---
zone_pivot_groups: dev-lang-csharp-javascript
ms.openlocfilehash: 69bc512c02a30bc74ae82a3a43a083af635d89ba
ms.sourcegitcommit: bf091e3a389138b59573865ca54775e38a4ffa1f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43154576"
---
<span data-ttu-id="57ff7-101">L'applicazione che si sta creando è una raccolta di foto</span><span class="sxs-lookup"><span data-stu-id="57ff7-101">The application that you're building is a photo gallery.</span></span> <span data-ttu-id="57ff7-102">che usa JavaScript lato client per chiamare API per caricare e visualizzare immagini.</span><span class="sxs-lookup"><span data-stu-id="57ff7-102">It uses client-side JavaScript to call APIs to upload and display images.</span></span> <span data-ttu-id="57ff7-103">In questo modulo si creerà un'API usando una funzione serverless che genera un URL temporaneo per caricare un'immagine.</span><span class="sxs-lookup"><span data-stu-id="57ff7-103">In this module, you create an API using a serverless function that generates a time-limited URL to upload an image.</span></span> <span data-ttu-id="57ff7-104">L'applicazione Web usa l'URL generato per caricare un'immagine nell'archiviazione BLOB usando l'[API REST dell'archiviazione BLOB](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span><span class="sxs-lookup"><span data-stu-id="57ff7-104">The web application uses the generated URL to upload an image to Blob storage using the [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span></span>

## <a name="create-a-blob-storage-container-for-images"></a><span data-ttu-id="57ff7-105">Creare un contenitore di archiviazione BLOB per le immagini</span><span class="sxs-lookup"><span data-stu-id="57ff7-105">Create a Blob storage container for images</span></span>

<span data-ttu-id="57ff7-106">L'applicazione richiede un contenitore di archiviazione separato per caricare e ospitare immagini.</span><span class="sxs-lookup"><span data-stu-id="57ff7-106">The application requires a separate storage container to upload and host images.</span></span>

1. <span data-ttu-id="57ff7-107">Verificare di essere ancora connessi a Azure Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="57ff7-107">Ensure that you're still signed in to Azure Cloud Shell (Bash).</span></span> <span data-ttu-id="57ff7-108">In caso contrario, selezionare **Enter focus mode** (Accedi a modalità messa a fuoco) per aprire una finestra di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="57ff7-108">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span>

1.  <span data-ttu-id="57ff7-109">Creare un nuovo contenitore denominato **images** nel proprio account di archiviazione con accesso pubblico a tutti i BLOB.</span><span class="sxs-lookup"><span data-stu-id="57ff7-109">Create a new container named **images** in your Storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-functions-app"></a><span data-ttu-id="57ff7-110">Creare un'app Funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="57ff7-110">Create an Azure Functions app</span></span>

<span data-ttu-id="57ff7-111">Funzioni di Azure è un servizio per l'esecuzione di funzioni serverless.</span><span class="sxs-lookup"><span data-stu-id="57ff7-111">Azure Functions is a service for running serverless functions.</span></span> <span data-ttu-id="57ff7-112">Una funzione serverless può essere attivata (chiamata) da eventi come una richiesta HTTP o la creazione di un BLOB in un contenitore di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-112">A serverless function can be triggered (called) by events, such as an HTTP request, or when a blob is created in a storage container.</span></span>

<span data-ttu-id="57ff7-113">Un'app Funzioni di Azure è un contenitore per una o più funzioni serverless.</span><span class="sxs-lookup"><span data-stu-id="57ff7-113">An Azure Functions app is a container for one or more serverless functions.</span></span>

- <span data-ttu-id="57ff7-114">Creare una nuova app Funzioni con un nome univoco nel gruppo di risorse **first-serverless-app** creato precedentemente.</span><span class="sxs-lookup"><span data-stu-id="57ff7-114">Create a new Functions app with a unique name in the **first-serverless-app** resource group that you created earlier.</span></span> <span data-ttu-id="57ff7-115">Le app Funzioni richiedono un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-115">Functions apps require a Storage account.</span></span> <span data-ttu-id="57ff7-116">In questa esercitazione viene usato l'account di archiviazione esistente.</span><span class="sxs-lookup"><span data-stu-id="57ff7-116">In this tutorial, you use the existing storage account.</span></span>

    ```azurecli
    az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
    ```

## <a name="create-an-http-triggered-serverless-function"></a><span data-ttu-id="57ff7-117">Creare una funzione serverless attivata da HTTP</span><span class="sxs-lookup"><span data-stu-id="57ff7-117">Create an HTTP-triggered serverless function</span></span>

<span data-ttu-id="57ff7-118">Per caricare un'immagine in modo sicuro nell'archiviazione BLOB, l'applicazione Web della raccolta foto invia alla funzione serverless una richiesta HTTP di generazione di un URL temporaneo.</span><span class="sxs-lookup"><span data-stu-id="57ff7-118">To securely upload an image to Blob storage, the photo gallery web app makes an HTTP request to the serverless function to generate a time-limited URL.</span></span> <span data-ttu-id="57ff7-119">La funzione viene attivata da una richiesta HTTP e usa Azure Storage SDK per generare e restituire l'URL protetto.</span><span class="sxs-lookup"><span data-stu-id="57ff7-119">The function is triggered by an HTTP request and uses the Azure Storage SDK to generate and return the secure URL.</span></span>

1. <span data-ttu-id="57ff7-120">Dopo che l'app Funzioni di Azure è stata creata, cercarla nel portale di Azure usando la casella **Cerca**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-120">After the Functions app is created, search for it in the Azure portal using the **Search** box.</span></span> <span data-ttu-id="57ff7-121">Fare clic sull'app e aprirla.</span><span class="sxs-lookup"><span data-stu-id="57ff7-121">Click on the app to open it.</span></span>

    ![Aprire l'app Funzioni](../media/2-search-function-app.png)


1. <span data-ttu-id="57ff7-123">Nella finestra dell'app Funzioni nel riquadro di spostamento a sinistra scegliere **Funzioni** e fare clic sul segno più (+) per creare una nuova funzione serverless.</span><span class="sxs-lookup"><span data-stu-id="57ff7-123">In the Functions app window, in the left navigation, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span>

    ![Creare una nuova funzione](../media/2-new-function.png)

1. <span data-ttu-id="57ff7-125">Fare clic su **Funzione personalizzata** per visualizzare un elenco di modelli di funzione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-125">Click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="57ff7-126">Individuare il modello **HttpTrigger** e fare clic sul linguaggio da usare (C# o JavaScript).</span><span class="sxs-lookup"><span data-stu-id="57ff7-126">Find the **HttpTrigger** template and click the language to use (C# or JavaScript).</span></span>

1. <span data-ttu-id="57ff7-127">Usare i valori seguenti per creare una funzione che genera un URL di caricamento BLOB:</span><span class="sxs-lookup"><span data-stu-id="57ff7-127">Use the following values to create a function that generates a blob upload URL:</span></span>

    | <span data-ttu-id="57ff7-128">Impostazione</span><span class="sxs-lookup"><span data-stu-id="57ff7-128">Setting</span></span>      |  <span data-ttu-id="57ff7-129">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="57ff7-129">Suggested value</span></span>   | <span data-ttu-id="57ff7-130">Descrizione</span><span class="sxs-lookup"><span data-stu-id="57ff7-130">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="57ff7-131">**Linguaggio**</span><span class="sxs-lookup"><span data-stu-id="57ff7-131">**Language**</span></span> | <span data-ttu-id="57ff7-132">C# o JavaScript</span><span class="sxs-lookup"><span data-stu-id="57ff7-132">C# or JavaScript</span></span> | <span data-ttu-id="57ff7-133">Selezionare il linguaggio che si vuole usare.</span><span class="sxs-lookup"><span data-stu-id="57ff7-133">Select the language that you want to use.</span></span> |
    | <span data-ttu-id="57ff7-134">**Assegnare un nome alla funzione**</span><span class="sxs-lookup"><span data-stu-id="57ff7-134">**Name your function**</span></span> | <span data-ttu-id="57ff7-135">GetUploadUrl</span><span class="sxs-lookup"><span data-stu-id="57ff7-135">GetUploadUrl</span></span> | <span data-ttu-id="57ff7-136">Immettere questo nome esattamente come illustrato in modo che l'applicazione possa individuare la funzione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-136">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="57ff7-137">**Livello di autorizzazione**</span><span class="sxs-lookup"><span data-stu-id="57ff7-137">**Authorization level**</span></span> | <span data-ttu-id="57ff7-138">Anonimo</span><span class="sxs-lookup"><span data-stu-id="57ff7-138">Anonymous</span></span> | <span data-ttu-id="57ff7-139">Consente l'accesso pubblico alla funzione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-139">Allow the function to be accessed publicly.</span></span> |

    ![Inserire le impostazioni per una nuova funzione attivata da HTTP](../media/2-new-function-httptrigger.png)

1. <span data-ttu-id="57ff7-141">Fare clic su **Crea** per creare la funzione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-141">Click **Create** to create the function.</span></span>

<span data-ttu-id="57ff7-142">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="57ff7-142">::: zone pivot="csharp"</span></span>
1. <span data-ttu-id="57ff7-143">**C#**</span><span class="sxs-lookup"><span data-stu-id="57ff7-143">**C#**</span></span> 

    <span data-ttu-id="57ff7-144">Quando viene visualizzato il codice sorgente della funzione, sostituire l'intero contenuto del file **run.csx** con il contenuto del file [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span><span class="sxs-lookup"><span data-stu-id="57ff7-144">When the function's source code appears, replace all of the content in the **run.csx** file with the content in the [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) file.</span></span>

<span data-ttu-id="57ff7-145">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="57ff7-145">::: zone-end</span></span>

<span data-ttu-id="57ff7-146">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="57ff7-146">::: zone pivot="javascript"</span></span>
1. <span data-ttu-id="57ff7-147">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="57ff7-147">**JavaScript**</span></span> 

    1. <span data-ttu-id="57ff7-148">(JavaScript) Questa funzione richiede il pacchetto `azure-storage` da npm.</span><span class="sxs-lookup"><span data-stu-id="57ff7-148">(JavaScript) This function requires the `azure-storage` package from npm.</span></span> <span data-ttu-id="57ff7-149">Il pacchetto genera il token di firma di accesso condiviso (SAS) necessario per compilare l'URL protetto.</span><span class="sxs-lookup"><span data-stu-id="57ff7-149">The package generates the shared access signature (SAS) token that's required to build the secure URL.</span></span> <span data-ttu-id="57ff7-150">Per installare il pacchetto npm, fare clic sull'app Funzioni nel riquadro di spostamento di sinistra e fare clic su **Funzionalità della piattaforma**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-150">To install the npm package, click on the Functions app on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="57ff7-151">(JavaScript) Fare clic su **Console** per visualizzare una finestra della console.</span><span class="sxs-lookup"><span data-stu-id="57ff7-151">(JavaScript) Click **Console** to reveal a console window.</span></span>

        ![Aprire una finestra della console](../media/2-open-console.jpg)

    1. <span data-ttu-id="57ff7-153">(JavaScript) Assicurarsi che la directory corrente sia **d:\home\site\wwwroot** eseguendo il comando `cd d:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="57ff7-153">(JavaScript) Ensure the current directory is **d:\home\site\wwwroot** by running the command `cd d:\home\site\wwwroot`.</span></span>

    1. <span data-ttu-id="57ff7-154">(JavaScript) Eseguire il comando `npm init -y` per creare un file **package.json** vuoto.</span><span class="sxs-lookup"><span data-stu-id="57ff7-154">(JavaScript) Run the command `npm init -y` to create an empty **package.json** file.</span></span>

    1. <span data-ttu-id="57ff7-155">(JavaScript) Eseguire il comando `npm install --save azure-storage` nella console per installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="57ff7-155">(JavaScript) To install the package, run the command `npm install --save azure-storage` in the console.</span></span> <span data-ttu-id="57ff7-156">Salvare il pacchetto come **package.json**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-156">Save the package as **package.json**.</span></span> <span data-ttu-id="57ff7-157">Il completamento dell'operazione potrebbe richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="57ff7-157">It may take a few minutes to complete the operation.</span></span>

    1. <span data-ttu-id="57ff7-158">(JavaScript) Fare clic sulla funzione (**GetUploadUrl**) nel riquadro di spostamento a sinistra per visualizzare la funzione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-158">(JavaScript) Click on the function (**GetUploadUrl**) in the left navigation to reveal the function.</span></span> <span data-ttu-id="57ff7-159">Sostituire tutto il contenuto del file **index.js** con il contenuto del file [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span><span class="sxs-lookup"><span data-stu-id="57ff7-159">Replace all of the content in the **index.js** file with the content in the [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) file.</span></span>
    
        ![Contenuto di index.js dopo l'aggiornamento](../media/2-paste-js.jpg)

<span data-ttu-id="57ff7-161">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="57ff7-161">::: zone-end</span></span>

1. <span data-ttu-id="57ff7-162">Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.</span><span class="sxs-lookup"><span data-stu-id="57ff7-162">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="57ff7-163">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-163">Click **Save**.</span></span> <span data-ttu-id="57ff7-164">Verificare nel pannello dei log che la funzione sia stata compilata correttamente.</span><span class="sxs-lookup"><span data-stu-id="57ff7-164">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="57ff7-165">La funzione genera un cosiddetto URL di firma di accesso condiviso (SAS), che consente di caricare un file nell'archivio BLOB.</span><span class="sxs-lookup"><span data-stu-id="57ff7-165">The function generates what's called a shared access signature (SAS) URL that's used to upload a file to Blob storage.</span></span> <span data-ttu-id="57ff7-166">L'URL di firma di accesso condiviso è valido per un breve periodo di tempo e consente il caricamento di un solo file.</span><span class="sxs-lookup"><span data-stu-id="57ff7-166">The SAS URL is valid for a short period of time and only allows a single file to be uploaded.</span></span> <span data-ttu-id="57ff7-167">Vedere la documentazione dell'archiviazione BLOB per altre informazioni sull'[uso delle firme di accesso condiviso](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="57ff7-167">Consult the Blob storage documentation, for more information on [how to use shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span></span>


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a><span data-ttu-id="57ff7-168">Aggiungere una variabile di ambiente per la stringa di connessione di archiviazione</span><span class="sxs-lookup"><span data-stu-id="57ff7-168">Add an environment variable for the storage connection string</span></span>

<span data-ttu-id="57ff7-169">La funzione appena creata richiede una stringa di connessione per l'account di archiviazione in modo da poter generare l'URL SAS.</span><span class="sxs-lookup"><span data-stu-id="57ff7-169">The function that you created requires a connection string for the Storage account so that it can generate the SAS URL.</span></span> <span data-ttu-id="57ff7-170">Invece di codificare la stringa di connessione come hardcoded nel corpo della funzione, è possibile memorizzarla come impostazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-170">Instead of hardcoding the connection string in the function body, it can be stored as an application setting.</span></span> <span data-ttu-id="57ff7-171">Le impostazioni dell'applicazione sono accessibili come variabili di ambiente da tutte le funzioni nell'app Funzioni.</span><span class="sxs-lookup"><span data-stu-id="57ff7-171">Application settings are accessible as environment variables by all functions in the Functions app.</span></span>

1. <span data-ttu-id="57ff7-172">In Cloud Shell eseguire una query sulla stringa di connessione dell'account di archiviazione e salvarla in una variabile Bash denominata **STORAGE_CONNECTION_STRING**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-172">In Cloud Shell, query the Storage account connection string and save it to a Bash variable named **STORAGE_CONNECTION_STRING**.</span></span>

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    <span data-ttu-id="57ff7-173">Verificare che la variabile sia impostata correttamente.</span><span class="sxs-lookup"><span data-stu-id="57ff7-173">Confirm the variable is set successfully.</span></span>

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. <span data-ttu-id="57ff7-174">Creare una nuova impostazione dell'applicazione denominata **AZURE_STORAGE_CONNECTION_STRING** usando il valore salvato nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="57ff7-174">Create a new application setting named **AZURE_STORAGE_CONNECTION_STRING** using the value saved from the previous step.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    <span data-ttu-id="57ff7-175">Verificare che l'output del comando contenga la nuova impostazione dell'applicazione con il valore corretto.</span><span class="sxs-lookup"><span data-stu-id="57ff7-175">Confirm that the command's output contains the new application setting with the correct value.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="57ff7-176">Testare la funzione serverless</span><span class="sxs-lookup"><span data-stu-id="57ff7-176">Test the serverless function</span></span>

<span data-ttu-id="57ff7-177">Oltre alla creazione e alla modifica delle funzioni, il portale di Azure offre uno strumento predefinito anche per il test delle funzioni.</span><span class="sxs-lookup"><span data-stu-id="57ff7-177">In addition to creating and editing functions, the Azure portal also provides a built-in tool for testing functions.</span></span>

1. <span data-ttu-id="57ff7-178">Per testare la funzione HTTP serverless, fare clic sulla scheda **Test** sulla destra della finestra del codice per espandere il pannello di test.</span><span class="sxs-lookup"><span data-stu-id="57ff7-178">To test the HTTP serverless function, on the right of the code window, click on the **Test** tab to expand the test panel.</span></span>

1. <span data-ttu-id="57ff7-179">Modificare **Metodo HTTP** in **GET**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-179">Change the **Http method** to **GET**.</span></span>

1. <span data-ttu-id="57ff7-180">In **Query** fare clic su **Aggiungi parametro** e aggiungere il parametro seguente:</span><span class="sxs-lookup"><span data-stu-id="57ff7-180">Under **Query**, click **Add parameter** and add the following parameter:</span></span>

    | <span data-ttu-id="57ff7-181">Nome</span><span class="sxs-lookup"><span data-stu-id="57ff7-181">Name</span></span>      |  <span data-ttu-id="57ff7-182">Valore</span><span class="sxs-lookup"><span data-stu-id="57ff7-182">Value</span></span>   | 
    | --- | --- |
    | <span data-ttu-id="57ff7-183">**filename**</span><span class="sxs-lookup"><span data-stu-id="57ff7-183">**filename**</span></span> | <span data-ttu-id="57ff7-184">image1.jpg</span><span class="sxs-lookup"><span data-stu-id="57ff7-184">image1.jpg</span></span> |

1. <span data-ttu-id="57ff7-185">Fare clic su **Esegui** nel pannello di test per inviare una richiesta HTTP alla funzione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-185">In the test panel, click **Run** to send an HTTP request to the function.</span></span>

1. <span data-ttu-id="57ff7-186">La funzione restituisce un URL di caricamento nell'output.</span><span class="sxs-lookup"><span data-stu-id="57ff7-186">The function returns an upload URL in the output.</span></span> <span data-ttu-id="57ff7-187">L'esecuzione della funzione viene visualizzata nel pannello dei log.</span><span class="sxs-lookup"><span data-stu-id="57ff7-187">The function execution appears in the Logs panel.</span></span>

    ![Log che indicano l'esecuzione corretta della funzione](../media/2-test-function.png)


## <a name="configure-cors-in-the-functions-app"></a><span data-ttu-id="57ff7-189">Configurare CORS nell'app Funzioni</span><span class="sxs-lookup"><span data-stu-id="57ff7-189">Configure CORS in the Functions app</span></span>

<span data-ttu-id="57ff7-190">Poiché il front-end della funzione è ospitato nell'archiviazione BLOB, ha un nome di dominio diverso rispetto all'app Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="57ff7-190">Because the function front end is hosted in Blob storage, it has a different domain name than the Azure Functions app.</span></span> <span data-ttu-id="57ff7-191">Per consentire al codice JavaScript lato client di chiamare correttamente la funzione appena creata, l'app Funzioni deve essere configurata per la condivisione di risorse tra le origini (CORS).</span><span class="sxs-lookup"><span data-stu-id="57ff7-191">For the client-side JavaScript to successfully call the function that you created, the Functions app has to be configured for cross-origin resource sharing (CORS).</span></span>

1. <span data-ttu-id="57ff7-192">Nella barra di spostamento di sinistra della finestra dell'app Funzioni fare clic sul nome dell'app Funzioni.</span><span class="sxs-lookup"><span data-stu-id="57ff7-192">In the left navigation of the Functions app window, click on the name of your Functions app.</span></span>

1. <span data-ttu-id="57ff7-193">Fare clic su **Funzionalità della piattaforma** per visualizzare un elenco di funzionalità avanzate.</span><span class="sxs-lookup"><span data-stu-id="57ff7-193">Click on **Platform features** to view a list of advanced features.</span></span>

1. <span data-ttu-id="57ff7-194">Sotto **API** fare clic su **CORS**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-194">Under **API**, click **CORS**.</span></span>

    ![Selezionare CORS](../media/2-open-cors.jpg)

1. <span data-ttu-id="57ff7-196">Aggiungere un'origine consentita per l'URL dell'applicazione dal modulo precedente, omettendo il carattere finale (/).</span><span class="sxs-lookup"><span data-stu-id="57ff7-196">Add an allow origin for the application URL from the previous module and omit the trailing slash (/).</span></span> <span data-ttu-id="57ff7-197">Ad esempio, `https://firstserverlessweb.z4.web.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="57ff7-197">For example, `https://firstserverlessweb.z4.web.core.windows.net`.</span></span>

    ![Impostazioni CORS con l'aggiunta dell'URL dell'applicazione Web serverless](../media/2-add-cors.png)

1. <span data-ttu-id="57ff7-199">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-199">Click **Save**.</span></span>

1. <span data-ttu-id="57ff7-200">**C#**:</span><span class="sxs-lookup"><span data-stu-id="57ff7-200">**C#**:</span></span>

   1. <span data-ttu-id="57ff7-201">(C#) Tornare alla funzione `GetUploadUrl` e selezionare la scheda **Integrazione**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-201">(C#) Navigate back to the `GetUploadUrl` function and select the **Integrate** tab.</span></span>

   1. <span data-ttu-id="57ff7-202">(C#) In **Metodi HTTP selezionati** selezionare **OPTIONS**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-202">(C#) Under **Selected HTTP methods**, select **OPTIONS**.</span></span>

      <span data-ttu-id="57ff7-203">**GET**, **POST** e **OPTIONS** devono essere tutti selezionati.</span><span class="sxs-lookup"><span data-stu-id="57ff7-203">**GET**, **POST**, and **OPTIONS** should all be selected.</span></span> <span data-ttu-id="57ff7-204">CORS usa il metodo **OPTIONS**, che non è selezionato per impostazione predefinita per le funzioni C#.</span><span class="sxs-lookup"><span data-stu-id="57ff7-204">CORS uses the **OPTIONS** method, which isn't selected by default for C# functions.</span></span>  

   1. <span data-ttu-id="57ff7-205">(C#) Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-205">(C#) Click **Save**.</span></span>

1. <span data-ttu-id="57ff7-206">Sempre nel portale di Azure passare all'app Funzioni.</span><span class="sxs-lookup"><span data-stu-id="57ff7-206">Still in the Azure portal, navigate to the Functions app.</span></span> <span data-ttu-id="57ff7-207">Selezionare la scheda **Panoramica**. Fare clic su **Riavvia** per assicurarsi che le modifiche per CORS abbiano effetto.</span><span class="sxs-lookup"><span data-stu-id="57ff7-207">Select the **Overview** tab. Click **Restart** to make sure that the changes for CORS take effect.</span></span>

## <a name="configure-cors-in-the-storage-account"></a><span data-ttu-id="57ff7-208">Configurare CORS nell'account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="57ff7-208">Configure CORS in the Storage account</span></span>

<span data-ttu-id="57ff7-209">Poiché l'applicazione Funzioni esegue anche chiamate JavaScript lato client all'archiviazione BLOB per caricare i file, è necessario configurare anche l'account di archiviazione per CORS.</span><span class="sxs-lookup"><span data-stu-id="57ff7-209">Because the Functions app also makes client-side JavaScript calls to Blob storage to upload files, you have to configure the Storage account for CORS.</span></span>

- <span data-ttu-id="57ff7-210">Eseguire il comando seguente per consentire a tutte le origini di caricare file nell'account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="57ff7-210">Run the following command to allow all origins to upload files to the Storage account:</span></span>

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a><span data-ttu-id="57ff7-211">Modificare l'app Web per caricare immagini</span><span class="sxs-lookup"><span data-stu-id="57ff7-211">Modify the web app to upload images</span></span>

<span data-ttu-id="57ff7-212">L'app Web recupera le impostazioni da un file denominato **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-212">The web app retrieves settings from a file named **settings.js**.</span></span> <span data-ttu-id="57ff7-213">Nei passaggi seguenti viene creato il file usando Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="57ff7-213">In the following steps, you create the file using Cloud Shell.</span></span> <span data-ttu-id="57ff7-214">Impostare `window.apiBaseUrl` sull'URL dell'app Funzioni e `window.blobBaseUrl` sull'URL dell'endpoint di archiviazione BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="57ff7-214">You set `window.apiBaseUrl` to the URL of the Functions app, and `window.blobBaseUrl` to the URL of the Azure Blob storage endpoint.</span></span>

1. <span data-ttu-id="57ff7-215">In Cloud Shell assicurarsi che la directory corrente sia la cartella **www/dist**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-215">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="57ff7-216">Eseguire una query sull'URL dell'app Funzioni e memorizzarlo in una variabile Bash denominata **FUNCTION_APP_URL**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-216">Query the URL of the Functions app and store it in a Bash variable named **FUNCTION_APP_URL**.</span></span>

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    <span data-ttu-id="57ff7-217">Verificare che la variabile sia impostata correttamente.</span><span class="sxs-lookup"><span data-stu-id="57ff7-217">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. <span data-ttu-id="57ff7-218">Per impostare l'URI base delle chiamate dell'API alle app Funzioni, creare il file **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-218">To set the base URI of API calls to your Functions app, create the **settings.js** file.</span></span> <span data-ttu-id="57ff7-219">Aggiungere l'URL dell'app Funzioni, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="57ff7-219">Add the URL of the Functions app like the following example:</span></span>

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    <span data-ttu-id="57ff7-220">È possibile apportare la modifica eseguendo il comando seguente o usando un editor da riga di comando come VIM.</span><span class="sxs-lookup"><span data-stu-id="57ff7-220">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    <span data-ttu-id="57ff7-221">Verificare che il file sia stato scritto correttamente.</span><span class="sxs-lookup"><span data-stu-id="57ff7-221">Confirm the file was successfully written.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="57ff7-222">Eseguire una query sull'URL di base di archiviazione BLOB e memorizzarlo in una variabile Bash denominata **BLOB_BASE_URL**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-222">Query the base URL for the Blob storage and store it in a Bash variable named **BLOB_BASE_URL**.</span></span>

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    <span data-ttu-id="57ff7-223">Verificare che la variabile sia impostata correttamente.</span><span class="sxs-lookup"><span data-stu-id="57ff7-223">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. <span data-ttu-id="57ff7-224">Per impostare l'URI di base delle chiamate API all'app Funzioni, aggiungere l'URL di archiviazione BLOB al file **settings.js** come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="57ff7-224">To set the base URI of API calls to your Functions app, append the Blob storage URL to the **settings.js** file like the following example:</span></span>

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    <span data-ttu-id="57ff7-225">È possibile apportare la modifica eseguendo il comando seguente o usando un editor da riga di comando come VIM.</span><span class="sxs-lookup"><span data-stu-id="57ff7-225">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    <span data-ttu-id="57ff7-226">Verificare che il file sia stato scritto correttamente e che ora contenga due righe.</span><span class="sxs-lookup"><span data-stu-id="57ff7-226">Confirm the file was successfully written and now contains two lines.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="57ff7-227">Caricare il file nell'archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="57ff7-227">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a><span data-ttu-id="57ff7-228">Testare l'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="57ff7-228">Test the web application</span></span>

<span data-ttu-id="57ff7-229">A questo punto, l'applicazione della raccolta può caricare un'immagine nell'archiviazione BLOB, ma non può ancora visualizzare le immagini.</span><span class="sxs-lookup"><span data-stu-id="57ff7-229">At this point, the gallery application is able to upload an image to Blob storage, but it can't display images yet.</span></span> <span data-ttu-id="57ff7-230">Tenterà di chiamare una funzione `GetImages` che non esiste ancora perché verrà creata in un modulo successivo.</span><span class="sxs-lookup"><span data-stu-id="57ff7-230">It will try to call a `GetImages` function that doesn't exist yet because you create it in a later module.</span></span> <span data-ttu-id="57ff7-231">La chiamata avrà esito negativo e la pagina Web apparirà bloccata su "Analisi", ma l'immagine selezionata verrà caricata correttamente.</span><span class="sxs-lookup"><span data-stu-id="57ff7-231">The call will fail and the web page will appear to be stuck on "Analyzing...," but the image you select will be successfully uploaded.</span></span>

<span data-ttu-id="57ff7-232">È possibile verificare che un'immagine sia stata caricata correttamente controllando il contenuto del contenitore **images** nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="57ff7-232">You can verify that an image is successfully uploaded by checking the contents of the **images** container in the Azure portal.</span></span>

1. <span data-ttu-id="57ff7-233">In una finestra del browser passare all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="57ff7-233">In a browser window, browse to the application.</span></span> <span data-ttu-id="57ff7-234">Selezionare un file di immagine e caricarlo.</span><span class="sxs-lookup"><span data-stu-id="57ff7-234">Select an image file and upload it.</span></span> <span data-ttu-id="57ff7-235">Il file viene caricato ma, poiché non è stata ancora aggiunta la possibilità di visualizzare le immagini, la foto caricata non viene visualizzata nell'app.</span><span class="sxs-lookup"><span data-stu-id="57ff7-235">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span> <span data-ttu-id="57ff7-236">La pagina Web appare bloccata su "Analyzing image..." (Analisi immagine in corso...). Questo problema verrà risolto in seguito.</span><span class="sxs-lookup"><span data-stu-id="57ff7-236">(The web page appears to be stuck on "Analyzing image..." You'll fix that later.)</span></span>

1. <span data-ttu-id="57ff7-237">In Cloud Shell verificare che l'immagine sia stata caricata nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-237">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="57ff7-238">Prima di passare all'esercitazione successiva, eliminare tutti i file nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="57ff7-238">Before moving on to the next tutorial, delete all files in the **images** container.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="57ff7-239">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="57ff7-239">Summary</span></span>

<span data-ttu-id="57ff7-240">In questa unità è stata creata un'app Funzioni di Azure ed è stato illustrato come usare una funzione serverless per consentire a un'applicazione Web di caricare immagini nell'archivio BLOB.</span><span class="sxs-lookup"><span data-stu-id="57ff7-240">In this unit, you created an Azure Functions app and learned how to use a serverless function to allow a web application to upload images to Blob storage.</span></span> <span data-ttu-id="57ff7-241">Si vedrà ora come creare anteprime per le immagini caricate usando una funzione serverless attivata dal BLOB.</span><span class="sxs-lookup"><span data-stu-id="57ff7-241">Next, you learn how to create thumbnails for the uploaded images using a blob-triggered serverless function.</span></span>