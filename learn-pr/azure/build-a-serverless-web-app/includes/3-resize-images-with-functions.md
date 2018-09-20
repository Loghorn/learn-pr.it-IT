<span data-ttu-id="4ac9f-101">Nell'unità precedente si è visto come una funzione serverless possa facilitare il caricamento sicuro di immagini nell'archiviazione BLOB da un'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-101">In the previous unit, you saw how a serverless function can facilitate the secure uploading of images to Blob storage from a web application.</span></span> <span data-ttu-id="4ac9f-102">In questo modulo si creerà un'altra funzione serverless che controllerà il caricamento di immagini e creerà le relative anteprime.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-102">In this module, you create another serverless function to watch for uploaded images and create thumbnails from them.</span></span>

## <a name="create-a-blob-storage-container-for-thumbnails"></a><span data-ttu-id="4ac9f-103">Creare un contenitore di archiviazione BLOB per le anteprime</span><span class="sxs-lookup"><span data-stu-id="4ac9f-103">Create a Blob storage container for thumbnails</span></span>

<span data-ttu-id="4ac9f-104">Le immagini con le dimensioni originali vengono archiviate in un contenitore denominato **images**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-104">The full-size images are stored in a container named **images**.</span></span> <span data-ttu-id="4ac9f-105">È necessario un altro contenitore per archiviare le anteprime di tali immagini.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-105">You need another container to store thumbnails of those images.</span></span>

<span data-ttu-id="4ac9f-106">Creare un nuovo contenitore denominato **thumbnails** nell'account di archiviazione con accesso pubblico a tutti i BLOB.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-106">Create a new container named **thumbnails** in your Storage account with public access to all blobs.</span></span>

```azurecli
az storage container create -n thumbnails --account-name <storage account name> --public-access blob
```

## <a name="create-a-blob-triggered-serverless-function"></a><span data-ttu-id="4ac9f-107">Creare una funzione serverless attivata dal BLOB</span><span class="sxs-lookup"><span data-stu-id="4ac9f-107">Create a blob-triggered serverless function</span></span>

<span data-ttu-id="4ac9f-108">Un trigger definisce come viene richiamata una funzione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-108">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="4ac9f-109">La funzione che verrà creata ora viene attivata dal BLOB.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-109">The function you create next uses a blob trigger.</span></span> <span data-ttu-id="4ac9f-110">La funzione viene richiamata automaticamente quando un BLOB (file immagine) viene caricato nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-110">The function is automatically invoked when a blob (image file) is uploaded to the **images** container.</span></span> <span data-ttu-id="4ac9f-111">Una funzione deve avere un trigger.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-111">A function must have one trigger.</span></span> <span data-ttu-id="4ac9f-112">I trigger hanno dati associati, in genere il payload che ha attivato la funzione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-112">Triggers have associated data, which are usually the payload that triggered the function.</span></span>

<span data-ttu-id="4ac9f-113">Le associazioni definiscono il modo in cui una funzione legge o scrive i dati in Azure o in servizi di terze parti.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-113">Bindings define how a function reads or writes data in Azure or third-party services.</span></span> <span data-ttu-id="4ac9f-114">Questa funzione crea una versione di anteprima dell'immagine che la attiva e salva l'anteprima in un contenitore denominato *thumbnails*.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-114">This function creates a thumbnail version of the image that triggers it and saves the thumbnail in a *thumbnails* container.</span></span>

1. <span data-ttu-id="4ac9f-115">Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui si è attivata la sandbox.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-115">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="4ac9f-116">Aprire l'app per le funzioni. È possibile usare la casella **Cerca** nella parte superiore del portale per trovarla in base al nome.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-116">Open your Function app, you can use the **Search** box at the top of the portal to find it by name.</span></span>

1. <span data-ttu-id="4ac9f-117">Nella finestra dell'app Funzioni nel riquadro di spostamento a sinistra scegliere **Funzioni** e fare clic sul segno più (+) per creare una nuova funzione serverless.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-117">In the Functions app window's left navigation, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span> <span data-ttu-id="4ac9f-118">Se viene visualizzata una pagina della guida introduttiva, fare clic su **Funzione personalizzata** per visualizzare un elenco di modelli di funzione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-118">If a quickstart page appears, click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="4ac9f-119">Individuare il modello **BlobTrigger** e selezionarlo.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-119">Find the **BlobTrigger** template and select it.</span></span>

1. <span data-ttu-id="4ac9f-120">Usare questi valori per creare una funzione che crea anteprime quando vengono caricate immagini:</span><span class="sxs-lookup"><span data-stu-id="4ac9f-120">Use these values to create a function that creates thumbnails as images are uploaded:</span></span>

    | <span data-ttu-id="4ac9f-121">Impostazione</span><span class="sxs-lookup"><span data-stu-id="4ac9f-121">Setting</span></span>      |  <span data-ttu-id="4ac9f-122">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="4ac9f-122">Suggested value</span></span>   | <span data-ttu-id="4ac9f-123">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4ac9f-123">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="4ac9f-124">**Linguaggio**</span><span class="sxs-lookup"><span data-stu-id="4ac9f-124">**Language**</span></span> | <span data-ttu-id="4ac9f-125">C# o JavaScript</span><span class="sxs-lookup"><span data-stu-id="4ac9f-125">C# or JavaScript</span></span> | <span data-ttu-id="4ac9f-126">Scegliere il linguaggio preferito.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-126">Choose your preferred language.</span></span> |
    | <span data-ttu-id="4ac9f-127">**Assegnare un nome alla funzione**</span><span class="sxs-lookup"><span data-stu-id="4ac9f-127">**Name your function**</span></span> | <span data-ttu-id="4ac9f-128">ResizeImage</span><span class="sxs-lookup"><span data-stu-id="4ac9f-128">ResizeImage</span></span> | <span data-ttu-id="4ac9f-129">Immettere questo nome esattamente come illustrato in modo che l'applicazione possa individuare la funzione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-129">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="4ac9f-130">**Percorso**</span><span class="sxs-lookup"><span data-stu-id="4ac9f-130">**Path**</span></span> | <span data-ttu-id="4ac9f-131">images/{name}</span><span class="sxs-lookup"><span data-stu-id="4ac9f-131">images/{name}</span></span> | <span data-ttu-id="4ac9f-132">Esegue la funzione quando viene visualizzato un file nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-132">Execute the function when a file appears in the **images** container.</span></span> |
    | <span data-ttu-id="4ac9f-133">**Informazioni account di archiviazione**</span><span class="sxs-lookup"><span data-stu-id="4ac9f-133">**Storage account information**</span></span> | <span data-ttu-id="4ac9f-134">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="4ac9f-134">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="4ac9f-135">Usare la variabile di ambiente creata in precedenza con la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-135">Use the environment variable previously created with the connection string.</span></span> |

    ![Immettere le impostazioni per la nuova funzione](../media/3-new-function.png)

1. <span data-ttu-id="4ac9f-137">Fare clic su **Crea** per creare la funzione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-137">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="4ac9f-138">Quando la funzione è stata creata, fare clic su **Integrazione** per visualizzarne le associazioni di trigger, input e output.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-138">When the function is created, click **Integrate** to view its trigger, input, and output bindings.</span></span>

1. <span data-ttu-id="4ac9f-139">Fare clic su **Nuovo output** per creare una nuova associazione di output.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-139">Click **New output** to create a new output binding.</span></span>

    ![Selezionare Nuovo output nella scheda Integrazione](../media/3-new-output.jpg)

1. <span data-ttu-id="4ac9f-141">Selezionare **Archiviazione BLOB di Azure** e fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-141">Select **Azure Blob Storage** and click **Select**.</span></span> <span data-ttu-id="4ac9f-142">Può essere necessario scorrere verso il basso per vedere il pulsante **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-142">You may have to scroll down to see the **Select** button.</span></span>

    ![Selezionare Archiviazione BLOB di Azure](../media/3-storage-output.jpg)

1. <span data-ttu-id="4ac9f-144">Immettere i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="4ac9f-144">Enter the following values:</span></span>

    | <span data-ttu-id="4ac9f-145">Impostazione</span><span class="sxs-lookup"><span data-stu-id="4ac9f-145">Setting</span></span>      |  <span data-ttu-id="4ac9f-146">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="4ac9f-146">Suggested value</span></span>   | <span data-ttu-id="4ac9f-147">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4ac9f-147">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="4ac9f-148">**Nome del parametro del BLOB**</span><span class="sxs-lookup"><span data-stu-id="4ac9f-148">**Blob parameter name**</span></span> | <span data-ttu-id="4ac9f-149">thumbnail</span><span class="sxs-lookup"><span data-stu-id="4ac9f-149">thumbnail</span></span> | <span data-ttu-id="4ac9f-150">La funzione usa il parametro con questo nome per scrivere l'anteprima.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-150">The function uses the parameter with this name to write the thumbnail.</span></span> |
    | <span data-ttu-id="4ac9f-151">**Usa valore restituito della funzione**</span><span class="sxs-lookup"><span data-stu-id="4ac9f-151">**Use function return value**</span></span> | <span data-ttu-id="4ac9f-152">No</span><span class="sxs-lookup"><span data-stu-id="4ac9f-152">No</span></span> |  |
    | <span data-ttu-id="4ac9f-153">**Percorso**</span><span class="sxs-lookup"><span data-stu-id="4ac9f-153">**Path**</span></span> | <span data-ttu-id="4ac9f-154">thumbnails/{name}</span><span class="sxs-lookup"><span data-stu-id="4ac9f-154">thumbnails/{name}</span></span> | <span data-ttu-id="4ac9f-155">Le anteprime vengono scritte in un contenitore denominato **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-155">The thumbnails are written to a container named **thumbnails**.</span></span> |
    | <span data-ttu-id="4ac9f-156">**Connessione dell'account di archiviazione**</span><span class="sxs-lookup"><span data-stu-id="4ac9f-156">**Storage account connection**</span></span> | <span data-ttu-id="4ac9f-157">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="4ac9f-157">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="4ac9f-158">Usare la variabile di ambiente creata in precedenza con la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-158">Use the environment variable previously created with the connection string.</span></span> |

    ![Immettere le impostazioni per l'associazione di output del BLOB](../media/3-blob-output.png)

1. <span data-ttu-id="4ac9f-160">Fare clic su **Salva** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-160">Click **Save** to save your changes.</span></span>

<span data-ttu-id="4ac9f-161">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="4ac9f-161">::: zone pivot="javascript"</span></span>

11. <span data-ttu-id="4ac9f-162">Fare clic su **Editor avanzato** nell'angolo superiore destro della finestra per visualizzare il codice JSON che rappresenta le associazioni.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-162">Click on **Advanced editor** in the top right corner of the window to reveal the JSON that represents the bindings.</span></span>

1. <span data-ttu-id="4ac9f-163">Nell'associazione `blobTrigger` aggiungere una proprietà denominata `dataType` con valore `binary`.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-163">In the `blobTrigger` binding, add a property named `dataType` with a value of `binary`.</span></span> <span data-ttu-id="4ac9f-164">In questo modo, viene configurata l'associazione per il passaggio dei contenuti del BLOB alla funzione come dati binari.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-164">This configures the binding to pass the blob contents to the function as binary data.</span></span>

    ```json
    {
        "name": "myBlob",
        "type": "blobTrigger",
        "direction": "in",
        "path": "images/{name}",
        "connection": "AZURE_STORAGE_CONNECTION_STRING",
        "dataType": "binary"
    }
    ```

1. <span data-ttu-id="4ac9f-165">Fare clic su **Salva** per creare la nuova associazione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-165">Click **Save** to create the new binding.</span></span>

<span data-ttu-id="4ac9f-166">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="4ac9f-166">::: zone-end</span></span>

<span data-ttu-id="4ac9f-167">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="4ac9f-167">::: zone pivot="csharp"</span></span>

11. <span data-ttu-id="4ac9f-168">Selezionare il nome della funzione **ResizeImage** nel riquadro di spostamento a sinistra per aprire il codice sorgente della funzione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-168">Select the **ResizeImage** function name in the left navigation to open the function's source code.</span></span>

1. <span data-ttu-id="4ac9f-169">La funzione richiede un pacchetto NuGet chiamato **ImageResizer** per generare le anteprime.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-169">The function requires a NuGet package called **ImageResizer** to generate the thumbnails.</span></span> <span data-ttu-id="4ac9f-170">I pacchetti NuGet vengono aggiunti alle funzioni C# tramite un file **project.json**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-170">NuGet packages are added to C# functions using a **project.json** file.</span></span> <span data-ttu-id="4ac9f-171">Per creare il file, fare clic su **Visualizza file** a destra per visualizzare i file che costituiscono la funzione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-171">To create the file, click **View Files** on the right to reveal the files that make up the function.</span></span>

1. <span data-ttu-id="4ac9f-172">Fare clic su **Aggiungi** per aggiungere un nuovo file denominato **project.json**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-172">Click **Add** to add a new file named **project.json**.</span></span> <span data-ttu-id="4ac9f-173">Premere **INVIO** al termine per aggiungere il file.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-173">Press **Enter** when done to add the file.</span></span>

1. <span data-ttu-id="4ac9f-174">Copiare il contenuto del file [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) nel file appena creato.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-174">Copy the contents of the [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) file into the newly created file.</span></span> <span data-ttu-id="4ac9f-175">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-175">Save the file.</span></span> <span data-ttu-id="4ac9f-176">I pacchetti vengono ripristinati automaticamente quando il file viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-176">Packages are automatically restored when the file is updated.</span></span>

    ![File project.json con ImageResizer](../media/3-project-json.png)

1. <span data-ttu-id="4ac9f-178">In **Visualizza file** selezionare **run.csx**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-178">Under **View Files**, select **run.csx**.</span></span> <span data-ttu-id="4ac9f-179">Sostituirne il contenuto con quello del file [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span><span class="sxs-lookup"><span data-stu-id="4ac9f-179">Replace its content with the content in the [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx) file.</span></span>

1. <span data-ttu-id="4ac9f-180">Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-180">To expand the logs panel, click **Logs** below the code window.</span></span>

1. <span data-ttu-id="4ac9f-181">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-181">Click **Save**.</span></span> <span data-ttu-id="4ac9f-182">Verificare nel pannello dei log che la funzione sia stata salvata correttamente e che non vi siano errori.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-182">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>

<span data-ttu-id="4ac9f-183">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="4ac9f-183">::: zone-end</span></span>

<span data-ttu-id="4ac9f-184">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="4ac9f-184">::: zone pivot="javascript"</span></span>

14. <span data-ttu-id="4ac9f-185">Questa funzione richiede il pacchetto `jimp` di npm per ridimensionare la foto.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-185">This function requires the `jimp` package from npm to resize the photo.</span></span> <span data-ttu-id="4ac9f-186">Per installare il pacchetto npm, fare clic sul nome dell'app Funzioni nel riquadro di spostamento di sinistra e fare clic su **Funzionalità della piattaforma**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-186">To install the npm package, click on the Functions app name on the left navigation and click **Platform features**.</span></span>

1. <span data-ttu-id="4ac9f-187">Fare clic su **Console** per visualizzare una finestra della console.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-187">Click **Console** to reveal a console window.</span></span>

1. <span data-ttu-id="4ac9f-188">Eseguire il comando `npm install jimp` nella console.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-188">Run the command `npm install jimp` in the console.</span></span> <span data-ttu-id="4ac9f-189">Il completamento dell'operazione potrebbe richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-189">It may take a few minutes to complete the operation.</span></span>

1. <span data-ttu-id="4ac9f-190">Fare clic sulla funzione **ResizeImage** nel riquadro di spostamento a sinistra per visualizzare la funzione.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-190">Click on the **ResizeImage** function name in the left navigation to reveal the function.</span></span> <span data-ttu-id="4ac9f-191">Sostituire tutto il contenuto del file **index.js** con il contenuto del file [**javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span><span class="sxs-lookup"><span data-stu-id="4ac9f-191">Replace all the content in the **index.js** file with the content of the [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js) file.</span></span>

1. <span data-ttu-id="4ac9f-192">Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-192">To expand the logs panel, click **Logs** below the code window.</span></span>

1. <span data-ttu-id="4ac9f-193">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-193">Click **Save**.</span></span> <span data-ttu-id="4ac9f-194">Verificare nel pannello dei log che la funzione sia stata salvata correttamente e che non vi siano errori.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-194">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>

<span data-ttu-id="4ac9f-195">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="4ac9f-195">::: zone-end</span></span>

## <a name="test-the-serverless-function"></a><span data-ttu-id="4ac9f-196">Testare la funzione serverless</span><span class="sxs-lookup"><span data-stu-id="4ac9f-196">Test the serverless function</span></span>

1. <span data-ttu-id="4ac9f-197">Aprire l'applicazione in un browser.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-197">Open the application in a browser.</span></span> <span data-ttu-id="4ac9f-198">Selezionare un file di immagine e caricarlo.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-198">Select an image file and upload it.</span></span> <span data-ttu-id="4ac9f-199">Il file viene caricato ma, poiché non è stata ancora aggiunta la possibilità di visualizzare le immagini, la foto caricata non viene visualizzata nell'app.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-199">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span>

1. <span data-ttu-id="4ac9f-200">In Cloud Shell verificare che l'immagine sia stata caricata nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-200">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list \
        --account-name <storage account name> \
        -c images \
        -o table
    ```

1. <span data-ttu-id="4ac9f-201">Verificare che sia stata creata l'anteprima in un contenitore denominato **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-201">Confirm the thumbnail was created in a container named **thumbnails**.</span></span>

    ```azurecli
    az storage blob list \
        --account-name <storage account name> \
        -c thumbnails \
        -o table
    ```

1. <span data-ttu-id="4ac9f-202">Ottenere l'URL per l'anteprima.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-202">Get the URL for the thumbnail.</span></span>

    ```azurecli
    az storage blob url \
        --account-name <storage account name> \
        -c thumbnails \
        -n <filename> \
        --output tsv
    ```

    <span data-ttu-id="4ac9f-203">Aprire l'URL in un browser e verificare che l'anteprima sia stata creata correttamente.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-203">Open the URL in a browser and confirm the thumbnail was properly created.</span></span>

1. <span data-ttu-id="4ac9f-204">Prima di passare alla prossima unità, eliminare tutti i file nei contenitori **images** e **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-204">Before continuing to the next unit, delete all files in the **images** and **thumbnails** containers.</span></span>

    ```azurecli
    az storage blob delete-batch \
        -s images \
        --account-name <storage account name>
    ```

    ```azurecli
    az storage blob delete-batch \
        -s thumbnails \
        --account-name <storage account name>
    ```

## <a name="summary"></a><span data-ttu-id="4ac9f-205">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="4ac9f-205">Summary</span></span>

<span data-ttu-id="4ac9f-206">In questa unità è stata creata una funzione serverless in modo da creare un'anteprima ogni volta che viene caricata un'immagine in un contenitore di archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-206">In this unit, you created a serverless function to create a thumbnail when an image is uploaded to a Blob storage container.</span></span> <span data-ttu-id="4ac9f-207">Si osserverà ora come usare Azure Cosmos DB per archiviare ed elencare i metadati delle immagini.</span><span class="sxs-lookup"><span data-stu-id="4ac9f-207">Next, you will learn how to use Azure Cosmos DB to store and list image metadata.</span></span>