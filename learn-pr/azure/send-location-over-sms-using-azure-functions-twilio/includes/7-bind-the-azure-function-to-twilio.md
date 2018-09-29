<span data-ttu-id="6bc2a-101">A questo punto, l'app per dispositivi mobili è completa e può inviare la posizione e l'elenco dei numeri di telefono dell'utente a una funzione di Azure in grado di deserializzare i dati.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-101">At this point, the mobile app is complete and it can send the user's location and list of phone numbers to an Azure function that can deserialize the data.</span></span> <span data-ttu-id="6bc2a-102">In questa unità si associa la funzione di Azure a Twilio per inviare messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-102">In this unit, you bind the Azure function to Twilio to send SMS messages.</span></span>

<span data-ttu-id="6bc2a-103">È possibile connettere Funzioni di Azure ad altri servizi, servizi di Azure o servizi di terze parti.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-103">Azure Functions can be connected to other services, either services in Azure or third-party services.</span></span> <span data-ttu-id="6bc2a-104">Queste connessioni, definite associazioni, sono disponibili in due forme: associazioni di input e output.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-104">These connections, called bindings, exist in two forms - input and output bindings.</span></span> <span data-ttu-id="6bc2a-105">Le associazioni di input forniscono dati alla funzione e le associazioni di output prelevare i dati dalla funzione e li inviano a un altro servizio.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-105">Input bindings provide data to your function and output bindings take data from your function and send it to another service.</span></span> <span data-ttu-id="6bc2a-106">Per ulteriori informazioni sulle associazioni vedere la [documentazione relativa all'associazione di funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="6bc2a-106">You can read about bindings in the [Azure Functions Binding docs](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings?azure-portal=true).</span></span>

<span data-ttu-id="6bc2a-107">Le associazioni per Funzioni di Azure create in Visual Studio vengono definite usando i parametri della funzione, decorate con attributi.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-107">Bindings for Azure Functions created in Visual Studio are defined using parameters to the function, decorated with attributes.</span></span>

## <a name="bind-the-azure-function-to-twilio"></a><span data-ttu-id="6bc2a-108">Associare la funzione di Azure a Twilio</span><span class="sxs-lookup"><span data-stu-id="6bc2a-108">Bind the Azure function to Twilio</span></span>

<span data-ttu-id="6bc2a-109">L'invio di SMS messaggi tramite Twilio richiede un'associazione di output che viene configurata con l'ID sottoscrizione dell'account (SID) e il Token di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-109">Sending SMS messages via Twilio requires an output binding that is configured with your account subscription ID (SID) and Auth Token.</span></span>

1. <span data-ttu-id="6bc2a-110">Arrestare il runtime locale di Funzioni di Azure se è ancora in esecuzione dall'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-110">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

2. <span data-ttu-id="6bc2a-111">Aggiungere il pacchetto "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet al progetto `ImHere.Functions`.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-111">Add the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package to the `ImHere.Functions` project.</span></span> <span data-ttu-id="6bc2a-112">Questo pacchetto NuGet contiene le classi pertinenti per l'associazione.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-112">This NuGet package contains the relevant classes for the binding.</span></span>

3. <span data-ttu-id="6bc2a-113">Aggiungere un nuovo parametro per il metodo `Run` statico sulla classe statica `SendLocation` denominata `messages`.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-113">Add a new parameter to the static `Run` method on the `SendLocation` static class called `messages`.</span></span> <span data-ttu-id="6bc2a-114">Questo parametro sarà di tipo `ICollector<CreateMessageOptions>`.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-114">This parameter will have a type of `ICollector<CreateMessageOptions>`.</span></span> <span data-ttu-id="6bc2a-115">È necessario aggiungere una direttiva `using` per lo spazio dei nomi `Twilio.Rest.Api.V2010.Account`.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-115">You'll need to add a `using` directive for the `Twilio.Rest.Api.V2010.Account` namespace.</span></span>

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,"get", "post", Route = null)]HttpRequestMessage req,
                                                ICollector<CreateMessageOptions> messages,
                                                ILogger log)
    ```

4. <span data-ttu-id="6bc2a-116">Decorare il nuovo parametro `messages` con l'attributo `TwilioSms` come segue:</span><span class="sxs-lookup"><span data-stu-id="6bc2a-116">Decorate the new `messages` parameter with the `TwilioSms` attribute as follows:</span></span> 

      ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",AuthTokenSetting = "TwilioAuthToken", From = "+1xxxxxxxxx")]ICollector<CreateMessageOptions> messages,
    ```
    <span data-ttu-id="6bc2a-117">Per questo attributo è necessario impostare tre parametri:</span><span class="sxs-lookup"><span data-stu-id="6bc2a-117">This attribute has three parameters you need to set:</span></span>

    * <span data-ttu-id="6bc2a-118">**AccountSidSetting**: da impostare su `"TwilioAccountSid"`</span><span class="sxs-lookup"><span data-stu-id="6bc2a-118">**AccountSidSetting** - set this to `"TwilioAccountSid"`</span></span>
  
        <span data-ttu-id="6bc2a-119">Si tratta del SID per l'account Twilio registrato in precedenza nel modulo.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-119">This is the SID for your Twilio account that you recorded earlier in the module.</span></span> <span data-ttu-id="6bc2a-120">Anziché impostare direttamente il SID, è possibile usare questo parametro, corrispondente al nome di un valore nelle impostazioni dell'app per le funzioni che verrà usato per recuperare il SID stesso.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-120">Rather than set the SID directly, this parameter is the name of a value in the function app settings that will be used to retrieve the SID.</span></span>

    * <span data-ttu-id="6bc2a-121">**AuthTokenSetting**: da impostare su `"TwilioAuthToken"`</span><span class="sxs-lookup"><span data-stu-id="6bc2a-121">**AuthTokenSetting** - set this to `"TwilioAuthToken"`</span></span>

       <span data-ttu-id="6bc2a-122">Si tratta del token di autenticazione per l'account Twilio registrato in precedenza nel modulo.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-122">This is the Auth Token for your Twilio account that you recorded earlier in the module.</span></span> <span data-ttu-id="6bc2a-123">Anziché impostare direttamente il token di autenticazione, è possibile usare questo parametro, corrispondente al nome di un valore nelle impostazioni dell'app per le funzioni che verrà usato per recuperare il token di autenticazione stesso.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-123">Rather than set the Auth Token directly, this parameter is the name of a value in the function app settings that will be used to retrieve the Auth Token.</span></span>

    * <span data-ttu-id="6bc2a-124">**From**: impostare questo parametro sul numero di telefono Twilio attivo registrato in precedenza nel modulo.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-124">**From** - set this to your Twilio active phone number that you recorded earlier in the module.</span></span>

        <span data-ttu-id="6bc2a-125">Il numero di telefono Twilio da cui proverranno i messaggi SMS in formato internazionale (+\<prefisso internazionale\>\<numero di telefono\>, ad esempio "+1555123456").</span><span class="sxs-lookup"><span data-stu-id="6bc2a-125">The Twilio phone number that the SMS messages will come from in international format (+\<country code\>\<phone number\>, for example "+1555123456").</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6bc2a-126">Assicurarsi di rimuovere tutti gli spazi dal numero di telefono.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-126">Make sure to remove all spaces from the phone number.</span></span>

5. <span data-ttu-id="6bc2a-127">Le impostazioni dell'app per le funzioni possono essere configurate in locale all'interno del file `local.settings.json`.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-127">Function app settings can be configured locally inside the `local.settings.json` file.</span></span> <span data-ttu-id="6bc2a-128">Aggiungere il SID dell'account Twilio e il Token di autenticazione a questo file JSON usando i nomi delle impostazioni passate all'attributo `TwilioSMS`.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-128">Add your Twilio account SID and Auth Token to this JSON file using the setting names that were passed to the `TwilioSMS` attribute.</span></span>

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureWebJobsStorage": "UseDevelopmentStorage=true",
            "FUNCTIONS_WORKER_RUNTIME": "dotnet",
            "TwilioAccountSid": "<Your SID>",
            "TwilioAuthToken": "<Your Auth Token>"
        }
    }
    ```

    <span data-ttu-id="6bc2a-129">Sostituire \<Your SID\> (SID) e \<Your Auth Token\> (Token di autenticazione) con i valori del dashboard di Twilio.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-129">Replace \<Your SID\> and \<Your Auth Token\> with the values from your Twilio dashboard.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6bc2a-130">Queste impostazioni locali verranno usate solo per l'esecuzione in locale.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-130">These local settings will be only for running locally.</span></span> <span data-ttu-id="6bc2a-131">In un'app di produzione questi valori sarebbero le credenziali dell'account di sviluppo o di test locale.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-131">In a production app, these values would be your local development or test account credentials.</span></span> <span data-ttu-id="6bc2a-132">Dopo aver distribuito la funzione di Azure in Azure, sarà possibile configurare i valori di produzione.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-132">Once the Azure Function has been deployed to Azure, you'll be able to configure the production values.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6bc2a-133">Se si archivia il codice nel controllo codice sorgente verranno archiviati anche questi valori di impostazione dell'applicazione locale, quindi prestare attenzione a non archiviare nessun valore effettivo in questi file se il codice è open source o pubblico in qualsiasi forma.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-133">If you check your code into source control, these local application setting values will be checked in, too, so be careful not to check in any actual values in these files if your code is open source or public in any form.</span></span>

## <a name="create-the-sms-messages"></a><span data-ttu-id="6bc2a-134">Creare messaggi SMS</span><span class="sxs-lookup"><span data-stu-id="6bc2a-134">Create the SMS messages</span></span>

<span data-ttu-id="6bc2a-135">Il parametro `ICollector<CreateMessageOptions>` è una raccolta di istanze `CreateMessageOptions` e viene usato per raccogliere i messaggi SMS da inviare.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-135">The `ICollector<CreateMessageOptions>` parameter is a collection of `CreateMessageOptions` instances and is used to collect the SMS messages you want to send.</span></span> <span data-ttu-id="6bc2a-136">Al termine dell'esecuzione della funzione tutte le istanze di `CreateMessageOptions` aggiunte a questa raccolta vengono passate a Twilio e usate per creare i messaggi da inviare ai destinatari di pertinenza.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-136">After the function has finished running, any instances of `CreateMessageOptions` added to this collection are passed to Twilio and used to create messages to be sent to the relevant recipients.</span></span>

1. <span data-ttu-id="6bc2a-137">Nella funzione `SendLocation` aggiungere il codice per riprodurre a ciclo i numeri di telefono nel `PostData` e creare un messaggio SMS per ognuno di essi.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-137">In the `SendLocation` function, add code to loop through the phone numbers in the `PostData` and create an SMS message for each one.</span></span> <span data-ttu-id="6bc2a-138">Sarà necessario aggiungere una direttiva using per `Twilio.Types`.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-138">You will need to add a using directive for `Twilio.Types`.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        PhoneNumber number = new PhoneNumber(toNo);
        CreateMessageOptions message = new CreateMessageOptions(number)
        {
            Body = $"I'm here! {url}"
        };
    }
    ```

    <span data-ttu-id="6bc2a-139">Nel messaggio è necessario includere il numero di telefono di destinazione e un corpo che contiene l'URL di Google Maps creato dalla posizione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-139">The message needs the phone number to send to and a body that contains the Google Maps URL created from the user's location.</span></span>

1. <span data-ttu-id="6bc2a-140">Registrare ogni messaggio e quindi aggiungerlo alla raccolta `messages`.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-140">Log each message, and then add it to the `messages` collection.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.LogInformation($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

<span data-ttu-id="6bc2a-141">Il metodo `SendLocation` completo è riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-141">The complete `SendLocation` method is shown below.</span></span> <span data-ttu-id="6bc2a-142">Il numero di telefono attivo sostituirà il segnaposto nel parametro `From`.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-142">Your active phone number would replace the placeholder in the `From` parameter.</span></span>

```cs
[FunctionName("SendLocation")]
public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                         "get", "post",
                                                         Route = null)]HttpRequest req,
                                            [TwilioSms(AccountSidSetting = "TwilioAccountSid",
                                                       AuthTokenSetting = "TwilioAuthToken",
                                                       From = "+1xxxxxxxxx")] ICollector<CreateMessageOptions> messages,
                                            ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.LogInformation($"URL created - {url}");

    foreach (string toNo in data.ToNumbers)
    {
        PhoneNumber number = new PhoneNumber(toNo);
        CreateMessageOptions message = new CreateMessageOptions(number)
        {
            Body = $"I'm here! {url}"
        };
        log.LogInformation($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }

    return new OkResult();
}
```

## <a name="test-it-out"></a><span data-ttu-id="6bc2a-143">Eseguire il test</span><span class="sxs-lookup"><span data-stu-id="6bc2a-143">Test it out</span></span>

1. <span data-ttu-id="6bc2a-144">Impostare l'app `ImHere.Functions` come progetto di avvio e avviarla senza eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-144">Set the `ImHere.Functions` app as the startup project and start it without debugging.</span></span>

1. <span data-ttu-id="6bc2a-145">Impostare l'app `ImHere.UWP` come progetto di avvio ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-145">Set the `ImHere.UWP` app as the startup project and run it.</span></span>

1. <span data-ttu-id="6bc2a-146">Immettere il proprio numero di telefono in formato internazionale (+\<prefisso internazionale\>\<numero di telefono\>) nell'app Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-146">Enter your own phone number in international format (+\<country code\>\<phone number\>) into the Xamarin.Forms app.</span></span> <span data-ttu-id="6bc2a-147">Gli account in versione di valutazione di Twilio possono inviare messaggi solo a numeri di telefono verificati quindi, per il momento, sarà possibile solo inviare messaggi a se stessi a meno che non si esegua l'aggiornamento a un account a pagamento o non vengano verificati altri numeri.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-147">Twilio trial accounts can send  messages only to verified phone numbers, so for now, you'll only be able to message yourself unless you upgrade to a paid account or verify other numbers.</span></span>

1. <span data-ttu-id="6bc2a-148">Fare clic sul pulsante **Invia posizione**.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-148">Click the **Send Location** button.</span></span> <span data-ttu-id="6bc2a-149">Se il messaggio SMS è stato inviato correttamente, nell'app Xamarin.Forms verrà visualizzato un messaggio simile a "Posizione inviata correttamente".</span><span class="sxs-lookup"><span data-stu-id="6bc2a-149">If the SMS message was sent successfully, you'll see a message in the Xamarin.Forms app saying, "Location sent successfully".</span></span>

    ![App Xamarin.Forms in cui la posizione risulta inviata](../media/7-ui-location-sent.png)

1. <span data-ttu-id="6bc2a-151">Nei log console per la funzione di Azure, verrà visualizzato il messaggio creato e inviato.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-151">In the console logs for the Azure function, you'll see the message being created and sent.</span></span> <span data-ttu-id="6bc2a-152">Se si verificano errori (ad esempio, il numero ha un formato non corretto), si verrà disconnessi.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-152">If any errors occur (such as, the number is in the wrong format), they will be logged out here.</span></span>

    ![La console di funzioni di Azure con il messaggio visualizzato è stata inviata](../media/7-function-message-sent.png)

1. <span data-ttu-id="6bc2a-154">Controllare se sul telefono è presente un messaggio.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-154">Check your phone for a message.</span></span> <span data-ttu-id="6bc2a-155">Fare clic sul collegamento nel messaggio per visualizzare la posizione.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-155">Follow the link in the message to see your location.</span></span>

    ![Il messaggio SMS ricevuto su un telefono cellulare](../media/7-message-received.png)

    > [!TIP]
    > <span data-ttu-id="6bc2a-157">La località visualizzata corrisponde a quella in cui l'app è in esecuzione, nelle vicinanze, quindi, del data center in cui la macchina virtuale è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-157">The location you'll see is the location where the app is running, so will be near to the data center that the VM is running from.</span></span> <span data-ttu-id="6bc2a-158">Se l'app è in esecuzione nel dispositivo locale, verrà visualizzata la località dell'utente.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-158">If this app was running on your local device then it would show your location.</span></span>

## <a name="summary"></a><span data-ttu-id="6bc2a-159">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="6bc2a-159">Summary</span></span>

<span data-ttu-id="6bc2a-160">In questa unità si è appreso come creare un'associazione di Twilio per la funzione di Azure e inviare un messaggio SMS con la posizione dell'utente a una funzione in esecuzione in locale.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-160">In this unit, you learned how to create a Twilio binding for the Azure function and send an SMS message with the user's location to a function that was running locally.</span></span> <span data-ttu-id="6bc2a-161">Nell'unità di successiva si pubblicherà la funzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="6bc2a-161">In the next unit, you publish the function to Azure.</span></span>
