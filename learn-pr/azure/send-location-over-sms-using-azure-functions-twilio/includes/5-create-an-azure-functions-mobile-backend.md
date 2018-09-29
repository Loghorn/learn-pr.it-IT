<span data-ttu-id="2d6d3-101">A questo punto, l'app sta cercando di ottenere la posizione dell'utente ed è pronta per essere inviata a una funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-101">At this point, the app is working to get the user's location and is ready to be sent to an Azure function.</span></span> <span data-ttu-id="2d6d3-102">In questa unità, si compila la funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-102">In this unit, you build the Azure function.</span></span>

## <a name="create-an-azure-functions-project"></a><span data-ttu-id="2d6d3-103">Creare un progetto di Funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="2d6d3-103">Create an Azure Functions project</span></span>

1. <span data-ttu-id="2d6d3-104">Aggiungere un nuovo progetto nella soluzione `ImHere` facendo clic con il tasto destr sulla soluzione e selezionando *Aggiungi -> Nuovo progetto...*.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-104">Add a new project to the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

1. <span data-ttu-id="2d6d3-105">Dall'albero sul lato sinistro selezionare *Visual C#->Cloud* e quindi *Funzioni di Azure* dal pannello al centro.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-105">From the tree on the left-hand side, select *Visual C#->Cloud*, and then select *Azure Functions* from the panel in the center.</span></span>

1. <span data-ttu-id="2d6d3-106">Denominare il progetto "ImHere.Functions" e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-106">Name the project "ImHere.Functions", and then click **OK**.</span></span>

    ![Finestra di dialogo Aggiungi nuovo progetto](../media/5-add-new-functions-project.png)

1. <span data-ttu-id="2d6d3-108">Viene visualizzata la finestra di dialogo di configurazione **Nuovo progetto**, nella cui parte inferiore sinistra può essere presente una rotellina durante il caricamento dei modelli aggiornati.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-108">The **New Project** configuration dialog will appear, and it may show a spinner in the bottom-left whilst loading updated templates.</span></span> <span data-ttu-id="2d6d3-109">Se la rotellina è visualizzata, attendere fino al termine del caricamento e quindi, se i modelli aggiornati sono disponibili, fare clic sull'opzione **Aggiorna** visualizzata per assicurarsi di ottenere i modelli di Funzioni più recenti.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-109">If you see this, wait until this has finished loading, then if updated templates are available, click the **Refresh** option that will appear to ensure you get the latest Function templates.</span></span>

    ![Finestra di dialogo Nuovo progetto durante il caricamento dei modelli più recenti](../media/5-loading-templates.png)

1. <span data-ttu-id="2d6d3-111">Nella finestra di configurazione **Nuovo progetto** assicurarsi che la versione di Funzioni sia impostata su *Funzioni di Azure v2 (.NET Standard)*, **NON** su _Funzioni di Azure v1 (.NET Framework)_.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-111">In the **New Project** configuration dialog, ensure the Functions version is set to *Azure Functions v2 (.NET Standard)* (**NOT** _v1 (.NET Framework)_).</span></span> <span data-ttu-id="2d6d3-112">Selezionare *Trigger HTTP*, lasciare l'account di archiviazione impostato su *Emulatore di archiviazione* e impostare i diritti di accesso su *Anonimo*.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-112">Select *Http Trigger*, leave the storage account set to *Storage Emulator*, and set the access rights to *Anonymous*.</span></span> <span data-ttu-id="2d6d3-113">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-113">Then click **OK**.</span></span>

    ![Finestra di dialogo di configurazione del progetto Funzioni di Azure](../media/5-configure-trigger.png)

    <span data-ttu-id="2d6d3-115">Il nuovo progetto verrà creato e includerà una funzione predefinita denominata `Function1`.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-115">The new project will be created and have a default function called `Function1`.</span></span>

> [!NOTE]
> <span data-ttu-id="2d6d3-116">Questa funzione è stata creata con l'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-116">This function was created with anonymous access.</span></span> <span data-ttu-id="2d6d3-117">Dopo la pubblicazione in Azure, qualsiasi persona che disponga dell'URL potrà chiamare la funzione.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-117">Once published to Azure, anybody who knows the URL will be able to call this function.</span></span> <span data-ttu-id="2d6d3-118">In uno scenario reale, disporrebbe di protezione tramite un qualche tipo di autenticazione, ad esempio il [servizio app di Azure](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview?azure-portal=true) o [Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="2d6d3-118">In a real-world scenario, you would protect this with some form of authentication, such as [Azure App Service authentication](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview?azure-portal=true) or [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c?azure-portal=true).</span></span>

## <a name="create-the-function"></a><span data-ttu-id="2d6d3-119">Creare la funzione</span><span class="sxs-lookup"><span data-stu-id="2d6d3-119">Create the function</span></span>

<span data-ttu-id="2d6d3-120">Il progetto di Funzioni di Azure viene creato con una singola funzione di trigger HTTP denominata `Function1`.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-120">The Azure Functions project is created with a single HTTP trigger function called `Function1`.</span></span> <span data-ttu-id="2d6d3-121">I trigger HTTP consentono di richiamare le funzioni tramite richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-121">HTTP triggers allow you to invoke your functions using HTTP requests.</span></span> <span data-ttu-id="2d6d3-122">La funzione stessa viene implementata come un metodo statico `Run` nella classe `Function1`.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-122">The function itself is implemented as a static `Run` method in the `Function1` class.</span></span>

1. <span data-ttu-id="2d6d3-123">Rinominare il file in Esplora soluzioni da "Function1.cs" a "SendLocation.cs".</span><span class="sxs-lookup"><span data-stu-id="2d6d3-123">Rename the file in Solution Explorer from "Function1.cs" to "SendLocation.cs".</span></span> <span data-ttu-id="2d6d3-124">Quando viene richiesto di rinominare tutti i riferimenti all'elemento di codice `Function1`, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-124">When prompted to rename all references to the code element `Function1`, click **Yes**.</span></span>

1. <span data-ttu-id="2d6d3-125">Rinominare il nome della funzione nell'attributo in "SendLocation".</span><span class="sxs-lookup"><span data-stu-id="2d6d3-125">Rename the function name in the attribute to "SendLocation".</span></span>

    ```cs
    [FunctionName("SendLocation")]
    ```

1. <span data-ttu-id="2d6d3-126">Eliminare il contenuto della funzione, eccetto la prima riga che scrive un messaggio informativo nel logger.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-126">Delete the contents of the function, except the first line that writes an information message to the logger.</span></span>

    ```cs
    public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                             "get", "post",
                                                             Route = null)]HttpRequestMessage req,
                                                ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
    }
    ```

## <a name="create-a-class-to-share-data-between-the-mobile-app-and-function"></a><span data-ttu-id="2d6d3-127">Creare una classe per condividere dati tra le app per dispositivi mobili e la funzione</span><span class="sxs-lookup"><span data-stu-id="2d6d3-127">Create a class to share data between the mobile app and function</span></span>

<span data-ttu-id="2d6d3-128">Quando vengono inviati a una funzione di Azure, i dati sono in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-128">When data is sent to the Azure function, it will be sent as JSON.</span></span> <span data-ttu-id="2d6d3-129">L'app per dispositivi mobili serializzerà i dati in JSON, mentre la funzione li deserializzerà.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-129">The mobile app will serialize data into JSON and the function will deserialize from JSON.</span></span> <span data-ttu-id="2d6d3-130">Per mantenere la coerenza dei dati tra l'app per dispositivi mobili e la funzione, creare un nuovo progetto che contenga una classe per mantenere i dati della posizione e del numero di telefono.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-130">To keep this data consistent between the mobile app and the function, create a new project that contains a class to hold the location and phone number data.</span></span> <span data-ttu-id="2d6d3-131">L'app e la funzione faranno quindi riferimento a questo progetto.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-131">This project will then be referenced by the app and function.</span></span>

1. <span data-ttu-id="2d6d3-132">Creare un nuovo progetto nella soluzione `ImHere` facendo clic con il tasto destr sulla soluzione e selezionando *Aggiungi -> Nuovo progetto...*.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-132">Create a new project under the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

1. <span data-ttu-id="2d6d3-133">Dall'albero sul lato sinistro selezionare *Visual C#->.NET Standard* e quindi *Libreria di classi (.NET Standard)* dal pannello al centro.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-133">From the tree on the left-hand side, select *Visual C#->.NET Standard*, and then select *Class Library (.NET Standard)* from the panel in the center.</span></span>

1. <span data-ttu-id="2d6d3-134">Denominare il progetto "ImHere.Data" e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-134">Name the project "ImHere.Data", and then click **OK**.</span></span>

    ![Finestra di dialogo Aggiungi nuovo progetto](../media/5-add-new-net-standard-project.png)

1. <span data-ttu-id="2d6d3-136">Eliminare il file "Class1.cs" generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-136">Delete the auto-generated "Class1.cs" file.</span></span>

1. <span data-ttu-id="2d6d3-137">Creare una nuova classe nel progetto `ImHere.Data` denominata `PostData` facendo clic con il pulsante destro del mouse sul progetto e quindi selezionando *Aggiungi -> Classe...*. Assegnare il nome "PostData" alla nuova classe e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-137">Create a new class in the `ImHere.Data` project called `PostData` by right-clicking on the project and then selecting *Add->Class...*. Name the new class "PostData" and click **OK**.</span></span> <span data-ttu-id="2d6d3-138">Contrassegnare la nuova classe come `public`.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-138">Mark this new class as `public`.</span></span>

1. <span data-ttu-id="2d6d3-139">Aggiungere le proprietà `double` per la latitudine e la longitudine e una proprietà `string[]` per indicare i numeri di telefono a cui inviare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-139">Add `double` properties for the latitude and longitude, as well as a `string[]` property for the phone numbers to send to.</span></span>

    ```cs
    public class PostData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string[] ToNumbers { get; set; }
    }
    ```

1. <span data-ttu-id="2d6d3-140">Aggiungere un riferimento a questo progetto in entrambi i progetti `ImHere.Functions` e `ImHere` facendo clic sul progetto e selezionando *Aggiungi -> Riferimento...*. Selezionare *Progetti* nell'albero a sinistra, quindi selezionare la casella accanto a *ImHere.Data*.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-140">Add a reference to this project to both the `ImHere.Functions` and `ImHere` projects by right-clicking on the project and then selecting *Add->Reference...*. Select *Projects* from the tree on the left-hand side, and then check the box next to *ImHere.Data*.</span></span>

    ![Configurazione dei riferimenti del progetto](../media/5-configure-project-references.png)

## <a name="read-the-data-sent-to-the-function"></a><span data-ttu-id="2d6d3-142">Leggere i dati inviati alla funzione</span><span class="sxs-lookup"><span data-stu-id="2d6d3-142">Read the data sent to the function</span></span>

<span data-ttu-id="2d6d3-143">Nella funzione di Azure, il parametro `req` contiene la richiesta HTTP eseguita. I dati all'interno della richiesta saranno un oggetto `PostData` JSON serializzato.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-143">In the Azure function, the `req` parameter contains the HTTP request that was made and the data inside this request will be a JSON serialized `PostData` object.</span></span>

1. <span data-ttu-id="2d6d3-144">Aprire la classe `SendLocation` nel progetto `ImHere.Functions`.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-144">Open the `SendLocation` class in the `ImHere.Functions` project.</span></span>

1. <span data-ttu-id="2d6d3-145">Leggere il contenuto della richiesta HTTP in una stringa, quindi deserializzarlo in un oggetto `PostData`, aggiungendo una direttiva d'uso per lo spazio dei nomi `ImHere.Data`.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-145">Read the contents of the HTTP request into a string, then deserialize it into a `PostData` object, adding a using directive for the `ImHere.Data` namespace.</span></span>

    ```cs
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    ```

1. <span data-ttu-id="2d6d3-146">Costruire un URL di Google Maps usando la latitudine e la longitudine da `PostData`.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-146">Construct a Google Maps URL using the latitude and longitude from the `PostData`.</span></span>

   ```cs
   string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
   ```

1. <span data-ttu-id="2d6d3-147">Registrare l'URL.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-147">Log the URL.</span></span>

    ```cs
    log.LogInformation($"URL created - {url}");
    ```

1. <span data-ttu-id="2d6d3-148">Viene restituito un codice di stato 200 per mostrare il completamento della funzione senza errori.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-148">Return a 200 status code to show the function completed without error.</span></span>

    ```cs
    return new OkResult();
    ```

<span data-ttu-id="2d6d3-149">La funzione completa viene mostrata di seguito.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-149">The complete function is shown below.</span></span>

```cs
[FunctionName("SendLocation")]
public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                         Route = null)]HttpRequest req,
                                                    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.LogInformation($"URL created - {url}");
    return new OkResult();
}
```

## <a name="run-the-azure-function-locally"></a><span data-ttu-id="2d6d3-150">Eseguire localmente la funzione di Azure</span><span class="sxs-lookup"><span data-stu-id="2d6d3-150">Run the Azure function locally</span></span>

<span data-ttu-id="2d6d3-151">È possibile eseguire in locale le funzioni con un account di archiviazione locale e un runtime Funzioni di Azure locale.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-151">Functions can be run locally using a local storage account and local Azure Functions runtime.</span></span> <span data-ttu-id="2d6d3-152">Questo runtime locale consente di testare la funzione prima di distribuirla in Azure.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-152">This local runtime allows you to test out your function before deploying it to Azure.</span></span>

1. <span data-ttu-id="2d6d3-153">Fare clic con il pulsante destro del mouse sul progetto `ImHere.Functions` in Esplora soluzioni, quindi scegliere *Imposta come progetto di avvio*.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-153">Right-click on the `ImHere.Functions` project in the solution explorer, and then select *Set as StartUp project*.</span></span>

1. <span data-ttu-id="2d6d3-154">Dal menu *Debug*, selezionare *Avvia senza eseguire debug*.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-154">From the *Debug* menu, select *Start Without Debugging*.</span></span> <span data-ttu-id="2d6d3-155">Il runtime di Funzioni di Azure locale verrà avviato all'interno di una finestra della console e avvierà la funzione, in ascolto su una porta disponibile in `localhost`.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-155">The local Azure Functions runtime will launch inside a console window and start your function, listening on an available port on `localhost`.</span></span> <span data-ttu-id="2d6d3-156">Se viene visualizzata una finestra di dialogo che richiede l'accesso al firewall, consentire l'accesso alle reti private (opzione predefinita).</span><span class="sxs-lookup"><span data-stu-id="2d6d3-156">If you see a dialog asking for firewall access, allow access to private networks (the default option).</span></span>

    ![Funzione di Azure eseguita localmente](../media/5-function-running-locally.png)

1. <span data-ttu-id="2d6d3-158">Prendere nota della porta su cui è in ascolto la funzione.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-158">Take a note of the port that the function is listening on.</span></span> <span data-ttu-id="2d6d3-159">Sarà necessaria nell'unità successiva per testare l'app per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-159">You'll need this in the next unit to test out the mobile app.</span></span> <span data-ttu-id="2d6d3-160">Nell'immagine precedente, la funzione è in ascolto sulla porta **7071**.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-160">In the image above, the function is listening on port **7071**.</span></span>

    ```sh
    Listening on http://localhost:7071/
    ```

1. <span data-ttu-id="2d6d3-161">Lasciare la funzione in esecuzione in modo da poter testare l'app per dispositivi mobili nell'unità successiva.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-161">Leave the function running so that you can test the mobile app in the next unit.</span></span>

## <a name="summary"></a><span data-ttu-id="2d6d3-162">Summary</span><span class="sxs-lookup"><span data-stu-id="2d6d3-162">Summary</span></span>

<span data-ttu-id="2d6d3-163">In questa unità si è visto come creare un progetto Funzioni di Azure in Visual Studio, si è aggiunto un progetto condiviso con un oggetto dati da condividere tra l'app per dispositivi mobili e la funzione e si è appreso come creare un'implementazione di base della funzione per deserializzare i dati al suo interno.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-163">In this unit, you learned how to create an Azure Functions project in Visual Studio, added a shared project with a data object to be shared between the mobile app and the function, and learned how to create a basic implementation of the function to deserialize the data passed in.</span></span> <span data-ttu-id="2d6d3-164">Si è anche appreso come eseguire in locale una funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-164">You also learned how to run an Azure function locally.</span></span> <span data-ttu-id="2d6d3-165">Nell'unità successiva si chiamerà la funzione di Azure dall'app per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="2d6d3-165">In the next unit, you'll call the Azure function from the mobile app.</span></span>