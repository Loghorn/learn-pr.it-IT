<span data-ttu-id="6eb56-101">Nell'unità precedente si è visto come una funzione serverless possa facilitare il caricamento sicuro di immagini nell'archiviazione BLOB da un'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="6eb56-101">In the previous unit, you saw how a serverless function can facilitate the secure uploading of images to Blob storage from a web application.</span></span> <span data-ttu-id="6eb56-102">In questo modulo si creerà un'altra funzione serverless che controllerà il caricamento di immagini e creerà le relative anteprime.</span><span class="sxs-lookup"><span data-stu-id="6eb56-102">In this module, you create another serverless function to watch for uploaded images and create thumbnails from them.</span></span>

## <a name="create-a-blob-storage-container-for-thumbnails"></a><span data-ttu-id="6eb56-103">Creare un contenitore di archiviazione BLOB per le anteprime</span><span class="sxs-lookup"><span data-stu-id="6eb56-103">Create a Blob storage container for thumbnails</span></span>

<span data-ttu-id="6eb56-104">Le immagini con le dimensioni originali vengono archiviate in un contenitore denominato **images**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-104">The full-size images are stored in a container named **images**.</span></span> <span data-ttu-id="6eb56-105">È necessario un altro contenitore per memorizzare le anteprime di tali immagini.</span><span class="sxs-lookup"><span data-stu-id="6eb56-105">You need another container to store thumbnails of those images.</span></span>

1. <span data-ttu-id="6eb56-106">Verificare di essere ancora connessi a Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="6eb56-106">Ensure you're still signed in to Cloud Shell (Bash).</span></span> <span data-ttu-id="6eb56-107">In caso contrario, selezionare **Enter focus mode** (Accedi a modalità messa a fuoco) per aprire una finestra di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6eb56-107">If you aren't, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="6eb56-108">Creare un nuovo contenitore denominato **thumbnails** nell'account di archiviazione con accesso pubblico a tutti i BLOB.</span><span class="sxs-lookup"><span data-stu-id="6eb56-108">Create a new container named **thumbnails** in your Storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n thumbnails --account-name <storage account name> --public-access blob
    ```


## <a name="create-a-blob-triggered-serverless-function"></a><span data-ttu-id="6eb56-109">Creare una funzione serverless attivata dal BLOB</span><span class="sxs-lookup"><span data-stu-id="6eb56-109">Create a blob-triggered serverless function</span></span>

<span data-ttu-id="6eb56-110">Un trigger definisce come viene richiamata una funzione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-110">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="6eb56-111">La funzione che verrà creata ora viene attivata dal BLOB.</span><span class="sxs-lookup"><span data-stu-id="6eb56-111">The function you create next uses a blob trigger.</span></span> <span data-ttu-id="6eb56-112">La funzione viene richiamata automaticamente quando un BLOB (file immagine) viene caricato nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-112">The function is automatically invoked when a blob (image file) is uploaded to the **images** container.</span></span> <span data-ttu-id="6eb56-113">Una funzione deve avere un trigger.</span><span class="sxs-lookup"><span data-stu-id="6eb56-113">A function must have one trigger.</span></span> <span data-ttu-id="6eb56-114">I trigger hanno dei dati associati, in genere il payload che ha attivato la funzione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-114">Triggers have associated data, which is usually the payload that triggered the function.</span></span>

<span data-ttu-id="6eb56-115">Le associazioni definiscono il modo in cui una funzione legge o scrive i dati in Azure o in servizi di terze parti.</span><span class="sxs-lookup"><span data-stu-id="6eb56-115">Bindings define how a function reads or writes data in Azure or third-party services.</span></span> <span data-ttu-id="6eb56-116">Questa funzione crea una versione di anteprima dell'immagine che la attiva e salva l'anteprima in un contenitore denominato *thumbnails*.</span><span class="sxs-lookup"><span data-stu-id="6eb56-116">This function creates a thumbnail version of the image that triggers it and saves the thumbnail in a *thumbnails* container.</span></span>

1. <span data-ttu-id="6eb56-117">Aprire l'app Funzioni nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="6eb56-117">Open your Functions app in the Azure portal.</span></span>

1. <span data-ttu-id="6eb56-118">Nella finestra dell'app Funzioni nel riquadro di spostamento a sinistra scegliere **Funzioni** e fare clic sul segno più (+) per creare una nuova funzione serverless.</span><span class="sxs-lookup"><span data-stu-id="6eb56-118">In the Functions app window's left navigation, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span> <span data-ttu-id="6eb56-119">Se viene visualizzata una pagina della guida introduttiva, fare clic su **Funzione personalizzata** per visualizzare un elenco di modelli di funzione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-119">If a quickstart page appears, click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="6eb56-120">Individuare il modello **BlobTrigger** e selezionarlo.</span><span class="sxs-lookup"><span data-stu-id="6eb56-120">Find the **BlobTrigger** template and select it.</span></span>

1. <span data-ttu-id="6eb56-121">Usare questi valori per creare una funzione che crea anteprime quando vengono caricate immagini:</span><span class="sxs-lookup"><span data-stu-id="6eb56-121">Use these values to create a function that creates thumbnails as images are uploaded:</span></span>

    | <span data-ttu-id="6eb56-122">Impostazione</span><span class="sxs-lookup"><span data-stu-id="6eb56-122">Setting</span></span>      |  <span data-ttu-id="6eb56-123">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="6eb56-123">Suggested value</span></span>   | <span data-ttu-id="6eb56-124">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6eb56-124">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6eb56-125">**Linguaggio**</span><span class="sxs-lookup"><span data-stu-id="6eb56-125">**Language**</span></span> | <span data-ttu-id="6eb56-126">C# o JavaScript</span><span class="sxs-lookup"><span data-stu-id="6eb56-126">C# or JavaScript</span></span> | <span data-ttu-id="6eb56-127">Scegliere il linguaggio preferito.</span><span class="sxs-lookup"><span data-stu-id="6eb56-127">Choose your preferred language.</span></span> |
    | <span data-ttu-id="6eb56-128">**Assegnare un nome alla funzione**</span><span class="sxs-lookup"><span data-stu-id="6eb56-128">**Name your function**</span></span> | <span data-ttu-id="6eb56-129">ResizeImage</span><span class="sxs-lookup"><span data-stu-id="6eb56-129">ResizeImage</span></span> | <span data-ttu-id="6eb56-130">Immettere questo nome esattamente come illustrato in modo che l'applicazione possa individuare la funzione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-130">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="6eb56-131">**Percorso**</span><span class="sxs-lookup"><span data-stu-id="6eb56-131">**Path**</span></span> | <span data-ttu-id="6eb56-132">images/{name}</span><span class="sxs-lookup"><span data-stu-id="6eb56-132">images/{name}</span></span> | <span data-ttu-id="6eb56-133">Esegue la funzione quando viene visualizzato un file nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-133">Execute the function when a file appears in the **images** container.</span></span> |
    | <span data-ttu-id="6eb56-134">**Informazioni account di archiviazione**</span><span class="sxs-lookup"><span data-stu-id="6eb56-134">**Storage account information**</span></span> | <span data-ttu-id="6eb56-135">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="6eb56-135">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="6eb56-136">Usare la variabile di ambiente creata in precedenza con la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-136">Use the environment variable previously created with the connection string.</span></span> |

    ![Immettere le impostazioni per la nuova funzione](../media/3-new-function.png)

1. <span data-ttu-id="6eb56-138">Fare clic su **Crea** per creare la funzione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-138">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="6eb56-139">Quando la funzione è stata creata, fare clic su **Integrazione** per visualizzarne le associazioni di trigger, input e output.</span><span class="sxs-lookup"><span data-stu-id="6eb56-139">When the function is created, click **Integrate** to view its trigger, input, and output bindings.</span></span>

1. <span data-ttu-id="6eb56-140">Fare clic su **Nuovo output** per creare una nuova associazione di output.</span><span class="sxs-lookup"><span data-stu-id="6eb56-140">Click **New output** to create a new output binding.</span></span>

    ![Selezionare Nuovo output nella scheda Integrazione](../media/3-new-output.jpg)

1. <span data-ttu-id="6eb56-142">Selezionare **Archiviazione BLOB di Azure** e fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-142">Select **Azure Blob Storage** and click **Select**.</span></span> <span data-ttu-id="6eb56-143">Può essere necessario scorrere verso il basso per vedere il pulsante **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-143">You may have to scroll down to see the **Select** button.</span></span>

    ![Selezionare Archiviazione BLOB di Azure](../media/3-storage-output.jpg)

1. <span data-ttu-id="6eb56-145">Immettere i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="6eb56-145">Enter the following values:</span></span>

    | <span data-ttu-id="6eb56-146">Impostazione</span><span class="sxs-lookup"><span data-stu-id="6eb56-146">Setting</span></span>      |  <span data-ttu-id="6eb56-147">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="6eb56-147">Suggested value</span></span>   | <span data-ttu-id="6eb56-148">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6eb56-148">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6eb56-149">**Nome del parametro del BLOB**</span><span class="sxs-lookup"><span data-stu-id="6eb56-149">**Blob parameter name**</span></span> | <span data-ttu-id="6eb56-150">thumbnail</span><span class="sxs-lookup"><span data-stu-id="6eb56-150">thumbnail</span></span> | <span data-ttu-id="6eb56-151">La funzione usa il parametro con questo nome per scrivere l'anteprima.</span><span class="sxs-lookup"><span data-stu-id="6eb56-151">The function uses the parameter with this name to write the thumbnail.</span></span> |
    | <span data-ttu-id="6eb56-152">**Usa valore restituito della funzione**</span><span class="sxs-lookup"><span data-stu-id="6eb56-152">**Use function return value**</span></span> | <span data-ttu-id="6eb56-153">No</span><span class="sxs-lookup"><span data-stu-id="6eb56-153">No</span></span> |  |
    | <span data-ttu-id="6eb56-154">**Percorso**</span><span class="sxs-lookup"><span data-stu-id="6eb56-154">**Path**</span></span> | <span data-ttu-id="6eb56-155">thumbnails/{name}</span><span class="sxs-lookup"><span data-stu-id="6eb56-155">thumbnails/{name}</span></span> | <span data-ttu-id="6eb56-156">Le anteprime vengono scritte in un contenitore denominato **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-156">The thumbnails are written to a container named **thumbnails**.</span></span> |
    | <span data-ttu-id="6eb56-157">**Connessione dell'account di archiviazione**</span><span class="sxs-lookup"><span data-stu-id="6eb56-157">**Storage account connection**</span></span> | <span data-ttu-id="6eb56-158">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="6eb56-158">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="6eb56-159">Usare la variabile di ambiente creata in precedenza con la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-159">Use the environment variable previously created with the connection string.</span></span> |

    ![Immettere le impostazioni per l'associazione di output del BLOB](../media/3-blob-output.png)


1. <span data-ttu-id="6eb56-161">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6eb56-161">**JavaScript**</span></span>

    1. <span data-ttu-id="6eb56-162">(JavaScript) Fare clic su **Editor avanzato** nell'angolo superiore destro della finestra per visualizzare il codice JSON che rappresenta le associazioni.</span><span class="sxs-lookup"><span data-stu-id="6eb56-162">(JavaScript) Click on **Advanced editor** in the top right corner of the window to reveal the JSON that represents the bindings.</span></span>

    1. <span data-ttu-id="6eb56-163">(JavaScript) Nell'associazione `blobTrigger` aggiungere una proprietà denominata `dataType` con valore `binary`.</span><span class="sxs-lookup"><span data-stu-id="6eb56-163">(JavaScript) In the `blobTrigger` binding, add a property named `dataType` with a value of `binary`.</span></span> <span data-ttu-id="6eb56-164">In questo modo si configura l'associazione per il passaggio dei contenuti del BLOB alla funzione come dati binari.</span><span class="sxs-lookup"><span data-stu-id="6eb56-164">This configures the binding to pass the blob contents to the function as binary data.</span></span>

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

1. <span data-ttu-id="6eb56-165">Fare clic su **Salva** per creare la nuova associazione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-165">Click **Save** to create the new binding.</span></span>

1. <span data-ttu-id="6eb56-166">**C#**</span><span class="sxs-lookup"><span data-stu-id="6eb56-166">**C#**</span></span>

    1. <span data-ttu-id="6eb56-167">(C#) Selezionare il nome della funzione **ResizeImage** nel riquadro di spostamento di sinistra per aprire il codice sorgente della funzione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-167">(C#) Select the **ResizeImage** function name in the left navigation to open the function's source code.</span></span>

    1. <span data-ttu-id="6eb56-168">(C#) La funzione richiede un pacchetto NuGet chiamato **ImageResizer** per generare le anteprime.</span><span class="sxs-lookup"><span data-stu-id="6eb56-168">(C#) The function requires a NuGet package called **ImageResizer** to generate the thumbnails.</span></span> <span data-ttu-id="6eb56-169">I pacchetti NuGet vengono aggiunti alle funzioni C# tramite un file **project.json**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-169">NuGet packages are added to C# functions using a **project.json** file.</span></span> <span data-ttu-id="6eb56-170">Per creare il file, fare clic su **Visualizza file** a destra per visualizzare i file che costituiscono la funzione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-170">To create the file, click **View Files** on the right to reveal the files that make up the function.</span></span>
    
    1. <span data-ttu-id="6eb56-171">(C#) Fare clic su **Aggiungi** per aggiungere un nuovo file denominato **project.json**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-171">(C#) Click **Add** to add a new file named **project.json**.</span></span>
    
    1. <span data-ttu-id="6eb56-172">(C#) Copiare il contenuto del file [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) nel file appena creato.</span><span class="sxs-lookup"><span data-stu-id="6eb56-172">(C#) Copy the contents of the [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) file into the newly created file.</span></span> <span data-ttu-id="6eb56-173">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="6eb56-173">Save the file.</span></span> <span data-ttu-id="6eb56-174">I pacchetti vengono ripristinati automaticamente quando il file viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="6eb56-174">Packages are automatically restored when the file is updated.</span></span>
    
        ![file project.json con ImageResizer](../media/3-project-json.png)
    
    1. <span data-ttu-id="6eb56-176">(C#) Sotto **Visualizza file** selezionare **run.csx**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-176">(C#) Under **View Files**, select **run.csx**.</span></span> <span data-ttu-id="6eb56-177">Sostituirne il contenuto con quello presente nel file [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span><span class="sxs-lookup"><span data-stu-id="6eb56-177">Replace its content with the content in the [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx) file.</span></span>

1. <span data-ttu-id="6eb56-178">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6eb56-178">**JavaScript**</span></span> 

    1. <span data-ttu-id="6eb56-179">(JavaScript) Questa funzione richiede il pacchetto `jimp` da npm per ridimensionare la foto.</span><span class="sxs-lookup"><span data-stu-id="6eb56-179">(JavaScript) This function requires the `jimp` package from npm to resize the photo.</span></span> <span data-ttu-id="6eb56-180">Per installare il pacchetto npm, fare clic sul nome dell'app Funzioni nel riquadro di spostamento di sinistra e fare clic su **Funzionalità della piattaforma**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-180">To install the npm package, click on the Functions app name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="6eb56-181">(JavaScript) Fare clic su **Console** per visualizzare una finestra della console.</span><span class="sxs-lookup"><span data-stu-id="6eb56-181">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="6eb56-182">(JavaScript) Eseguire il comando `npm install jimp` nella console.</span><span class="sxs-lookup"><span data-stu-id="6eb56-182">(JavaScript) Run the command `npm install jimp` in the console.</span></span> <span data-ttu-id="6eb56-183">Il completamento dell'operazione potrebbe richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="6eb56-183">It may take a few minutes to complete the operation.</span></span>

    1. <span data-ttu-id="6eb56-184">(JavaScript) Fare clic sulla funzione **ResizeImage** nel riquadro di spostamento a sinistra per visualizzare la funzione.</span><span class="sxs-lookup"><span data-stu-id="6eb56-184">(JavaScript) Click on the **ResizeImage** function name in the left navigation to reveal the function.</span></span> <span data-ttu-id="6eb56-185">Sostituire tutto il contenuto del file **index.js** con il contenuto del file [**javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span><span class="sxs-lookup"><span data-stu-id="6eb56-185">Replace all the content in the **index.js** file with the content of the [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js) file.</span></span>

1. <span data-ttu-id="6eb56-186">Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.</span><span class="sxs-lookup"><span data-stu-id="6eb56-186">To expand the logs panel, click **Logs** below the code window.</span></span>

1. <span data-ttu-id="6eb56-187">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-187">Click **Save**.</span></span> <span data-ttu-id="6eb56-188">Verificare nel pannello dei log che la funzione sia stata salvata correttamente e che non vi siano errori.</span><span class="sxs-lookup"><span data-stu-id="6eb56-188">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="6eb56-189">Testare la funzione serverless</span><span class="sxs-lookup"><span data-stu-id="6eb56-189">Test the serverless function</span></span>

1. <span data-ttu-id="6eb56-190">Aprire l'applicazione in un browser.</span><span class="sxs-lookup"><span data-stu-id="6eb56-190">Open the application in a browser.</span></span> <span data-ttu-id="6eb56-191">Selezionare un file di immagine e caricarlo.</span><span class="sxs-lookup"><span data-stu-id="6eb56-191">Select an image file and upload it.</span></span> <span data-ttu-id="6eb56-192">Il file viene caricato ma, poiché non è stata ancora aggiunta la possibilità di visualizzare le immagini, la foto caricata non viene visualizzata nell'app.</span><span class="sxs-lookup"><span data-stu-id="6eb56-192">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span>

1. <span data-ttu-id="6eb56-193">In Cloud Shell verificare che l'immagine sia stata caricata nel contenitore **images**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-193">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="6eb56-194">Verificare che sia stata creata l'anteprima in un contenitore denominato **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-194">Confirm the thumbnail was created in a container named **thumbnails**.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c thumbnails -o table
    ```

1. <span data-ttu-id="6eb56-195">Ottenere l'URL per l'anteprima.</span><span class="sxs-lookup"><span data-stu-id="6eb56-195">Get the URL for the thumbnail.</span></span>

    ```azurecli
    az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
    ```

    <span data-ttu-id="6eb56-196">Aprire l'URL in un browser e verificare che l'anteprima sia stata creata correttamente.</span><span class="sxs-lookup"><span data-stu-id="6eb56-196">Open the URL in a browser and confirm the thumbnail was properly created.</span></span>

1. <span data-ttu-id="6eb56-197">Prima di passare all'esercitazione successiva, eliminare tutti i file nei contenitori **images** e **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="6eb56-197">Before continuing to the next tutorial, delete all files in the **images** and **thumbnails** containers.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```
    ```azurecli
    az storage blob delete-batch -s thumbnails --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="6eb56-198">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="6eb56-198">Summary</span></span>

<span data-ttu-id="6eb56-199">In questa unità è stata creata una funzione serverless per creare un'anteprima ogni volta che viene caricata un'immagine in un contenitore di archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="6eb56-199">In this unit, you created a serverless function to create a thumbnail when an image is uploaded to a Blob storage container.</span></span> <span data-ttu-id="6eb56-200">Si vedrà ora come usare Azure Cosmos DB per archiviare ed elencare i metadati delle immagini.</span><span class="sxs-lookup"><span data-stu-id="6eb56-200">Next, you learn how to use Azure Cosmos DB to store and list image metadata.</span></span>