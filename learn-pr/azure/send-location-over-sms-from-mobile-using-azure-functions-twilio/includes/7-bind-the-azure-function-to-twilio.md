<span data-ttu-id="d3b5f-101">A questo punto, l'app per dispositivi mobili è stata completata e invia la posizione e l'elenco dei numeri di telefono dell'utente a una funzione di Azure che può deserializzare i dati.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-101">At this point, the mobile app is complete and sends the user's location and list of phone numbers to an Azure function that can deserialize the data.</span></span> <span data-ttu-id="d3b5f-102">In questa unità, si associa la funzione di Azure a Twilio per inviare messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-102">In this unit, you bind the Azure function to Twilio to send SMS messages.</span></span>

<span data-ttu-id="d3b5f-103">Le funzioni di Azure possono essere connesso ad altri servizi, servizi di Azure o servizi di terze parti.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-103">Azure Functions can be connected to other services, either services in Azure or third-party services.</span></span> <span data-ttu-id="d3b5f-104">Queste connessioni, definite associazioni, sono disponibili in due forme: associazioni di input e output.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-104">These connections, called bindings, exist in two forms - input and output bindings.</span></span> <span data-ttu-id="d3b5f-105">Le associazioni di input forniscono dati alla funzione e le associazioni di output prelevare i dati dalla funzione e li inviano a un altro servizio.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-105">Input bindings provide data to your function and output bindings take data from your function and send it to another service.</span></span> <span data-ttu-id="d3b5f-106">Per ulteriori informazioni sulle associazioni vedere la [documentazione relativa all'associazione di funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).</span><span class="sxs-lookup"><span data-stu-id="d3b5f-106">You can read about bindings in the [Azure Functions Binding docs](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).</span></span>

<span data-ttu-id="d3b5f-107">Le associazioni per le funzioni di Azure create in Visual Studio vengono definite usando i parametri della funzione, decorate con attributi.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-107">Bindings for Azure Functions created in Visual Studio are defined using parameters to the function, decorated with attributes.</span></span>

## <a name="bind-the-azure-function-to-twilio"></a><span data-ttu-id="d3b5f-108">Associare la funzione di Azure a Twilio</span><span class="sxs-lookup"><span data-stu-id="d3b5f-108">Bind the Azure function to Twilio</span></span>

<span data-ttu-id="d3b5f-109">L'invio di SMS messaggi tramite Twilio richiede un'associazione di output che viene configurata con l'ID sottoscrizione dell'account (SID) e il Token di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-109">Sending SMS messages via Twilio requires an output binding that is configured with your account subscription ID (SID) and Auth Token.</span></span>

1. <span data-ttu-id="d3b5f-110">Arrestare il runtime di funzioni di Azure locale se è ancora in esecuzione dall'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-110">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

2. <span data-ttu-id="d3b5f-111">Aggiungere il pacchetto "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet al progetto `ImHere.Functions`.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-111">Add the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package to the `ImHere.Functions` project.</span></span> <span data-ttu-id="d3b5f-112">Questo pacchetto NuGet contiene le classi pertinenti per l'associazione.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-112">This NuGet package contains the relevant classes for the binding.</span></span>

3. <span data-ttu-id="d3b5f-113">Aggiungere un nuovo parametro per il metodo `Run` statico sulla classe statica `SendLocation` denominata `messages`.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-113">Add a new parameter to the static `Run` method on the `SendLocation` static class called `messages`.</span></span> <span data-ttu-id="d3b5f-114">Questo parametro sarà di tipo `ICollector<SMSMessage>`.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-114">This parameter will have a type of `ICollector<SMSMessage>`.</span></span> <span data-ttu-id="d3b5f-115">È necessario aggiungere una direttiva utilizzo per lo spazio dei nomi `Twilio`.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-115">You'll need to add a using directive for the `Twilio` namespace.</span></span>

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                   "get", "post",
                                                                   Route = null)]HttpRequestMessage req,
                                                      ICollector<SMSMessage> messages,
                                                      TraceWriter log)
    ```

4. <span data-ttu-id="d3b5f-116">Decorare il nuovo parametro `messages` con l'attributo `TwilioSms`.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-116">Decorate the new `messages` parameter with the `TwilioSms` attribute.</span></span> <span data-ttu-id="d3b5f-117">Questo attributo ha tre parametri che è necessario impostare.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-117">This attribute has three parameters you need to set.</span></span>

    | <span data-ttu-id="d3b5f-118">Impostazione</span><span class="sxs-lookup"><span data-stu-id="d3b5f-118">Setting</span></span>      |  <span data-ttu-id="d3b5f-119">Valore</span><span class="sxs-lookup"><span data-stu-id="d3b5f-119">Value</span></span>   | <span data-ttu-id="d3b5f-120">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d3b5f-120">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="d3b5f-121">**AccountSidSetting**</span><span class="sxs-lookup"><span data-stu-id="d3b5f-121">**AccountSidSetting**</span></span> | <span data-ttu-id="d3b5f-122">"TwilioAccountSid"</span><span class="sxs-lookup"><span data-stu-id="d3b5f-122">"TwilioAccountSid"</span></span> | <span data-ttu-id="d3b5f-123">SID dell'account Twilio.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-123">The SID for your Twilio account.</span></span> <span data-ttu-id="d3b5f-124">Anziché impostare direttamente il SID, questo parametro è il nome di un valore nelle impostazioni dell'app per le funzioni che verrà usato per recuperare il SID.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-124">Rather than set the SID directly, this parameter is the name of a value in the function app settings that will be used to retrieve the SID.</span></span> |
    | <span data-ttu-id="d3b5f-125">**AuthTokenSetting**</span><span class="sxs-lookup"><span data-stu-id="d3b5f-125">**AuthTokenSetting**</span></span> | <span data-ttu-id="d3b5f-126">"TwilioAuthToken"</span><span class="sxs-lookup"><span data-stu-id="d3b5f-126">"TwilioAuthToken"</span></span> | <span data-ttu-id="d3b5f-127">Token di autenticazione per l'account Twilio.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-127">The Auth Token for your Twilio account.</span></span> <span data-ttu-id="d3b5f-128">Anziché impostare direttamente il Token di autenticazione, questo parametro è il nome di un valore nelle impostazioni dell'app per le funzioni che verrà usato per recuperare il Token di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-128">Rather than set the Auth Token directly, this parameter is the name of a value in the function app settings that will be used to retrieve the Auth Token.</span></span> |
    | <span data-ttu-id="d3b5f-129">**From**</span><span class="sxs-lookup"><span data-stu-id="d3b5f-129">**From**</span></span> | <span data-ttu-id="d3b5f-130">Il numero di telefono di Twilio</span><span class="sxs-lookup"><span data-stu-id="d3b5f-130">Your Twilio phone number</span></span> | <span data-ttu-id="d3b5f-131">Il numero di telefono Twilio da cui proverranno i messaggi SMS in formato internazionale (+\<prefisso internazionale\>\<numero di telefono\>, ad esempio "+1555123456").</span><span class="sxs-lookup"><span data-stu-id="d3b5f-131">The Twilio phone number that the SMS messages will come from in international format (+\<country code\>\<phone number\>, for example "+1555123456").</span></span> |

    <span data-ttu-id="d3b5f-132">Quando si crea un account Twilio, viene assegnato un numero di telefono da cui è possibile inviare messaggi.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-132">When you create a Twilio account, you are assigned a phone number that you can send messages from.</span></span> <span data-ttu-id="d3b5f-133">È possibile trovare questo numero di telefono nel dashboard **Numeri di telefono** di Twilio.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-133">You can find this phone number on the Twilio **Phone Numbers** dashboard.</span></span> <span data-ttu-id="d3b5f-134">Dal sito di Twilio, selezionare i puntini di sospensione nella parte inferiore del menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-134">From the Twilio site, select the ellipses at the bottom of the left-hand menu.</span></span> <span data-ttu-id="d3b5f-135">Quindi, selezionare *SUPER NETWORK -> Numeri di telefono*.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-135">Then, select *SUPER NETWORK->Phone Numbers*.</span></span> <span data-ttu-id="d3b5f-136">È possibile aggiungere questo dashboard al menu a sinistra mediante l'icona della puntina.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-136">You can pin this dashboard to the left-hand menu using the pin icon.</span></span> <span data-ttu-id="d3b5f-137">Il numero Twilio si troverà in *Gestisci numeri -> Numeri attivi*.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-137">Your Twilio number will be under *Manage Numbers->Active Numbers*.</span></span> <span data-ttu-id="d3b5f-138">È necessario rimuovere gli spazi dal numero.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-138">You'll need to remove any spaces from the number.</span></span>

    ![Ricerca del numero Twilio](../media-drafts/7-twilio-find-number.png)

    ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",
               AuthTokenSetting = "TwilioAuthToken",
               From = "+1xxxxxxxxx")]ICollector<SMSMessage> messages,
    ```

5. <span data-ttu-id="d3b5f-140">Le impostazioni dell'app per le funzioni possono essere configurate in locale all'interno del file `local.settings.json`.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-140">Function app settings can be configured locally inside the `local.settings.json` file.</span></span> <span data-ttu-id="d3b5f-141">Aggiungere il SID dell'account Twilio e il Token di autenticazione a questo file JSON usando i nomi delle impostazioni passate all'attributo `TwilioSMS`.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-141">Add your Twilio account SID and Auth Token to this JSON file using the setting names that were passed to the `TwilioSMS` attribute.</span></span>

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureWebJobsStorage": "UseDevelopmentStorage=true",
            "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
            "TwilioAccountSid": "<Your SID>",
            "TwilioAuthToken": "<Your Auth Token>"
        }
    }
    ```

    <span data-ttu-id="d3b5f-142">Sostituire \<Your SID\> (SID) e \<Your Auth Token\> (Token di autenticazione) con i valori del dashboard di Twilio.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-142">Replace \<Your SID\> and \<Your Auth Token\> with the values from your Twilio dashboard.</span></span>

    > <span data-ttu-id="d3b5f-143">Queste impostazioni locali potranno essere eseguite solo in locale.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-143">These local settings will be only for running locally.</span></span> <span data-ttu-id="d3b5f-144">In un'app di produzione questi valori sarebbero le credenziali dell'account di sviluppo o test locale.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-144">In a production app, these value would be your local development or test account credentials.</span></span> <span data-ttu-id="d3b5f-145">Dopo aver distribuito la funzione di Azure in Azure, sarà possibile configurare i valori di produzione.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-145">Once the Azure Function has been deployed to Azure, you'll be able to configure the production values.</span></span>
    > <span data-ttu-id="d3b5f-146">Se si archivia il codice nel controllo codice sorgente verranno archiviati anche questi valori di impostazione dell'applicazione locale, quindi prestare attenzione a non archiviare nessun valore effettivo in questi file se il codice è open source o pubblico in qualsiasi forma.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-146">If you check your code into source control, these local application setting values will be checked in, too, so be careful not to check in any actual values in these files if your code is open source or public in any form.</span></span>

## <a name="create-the-sms-messages"></a><span data-ttu-id="d3b5f-147">Creare messaggi SMS</span><span class="sxs-lookup"><span data-stu-id="d3b5f-147">Create the SMS messages</span></span>

<span data-ttu-id="d3b5f-148">Il parametro `ICollector<SMSMessage>` è una raccolta di istanze `SMSMessage` e viene usato per raccogliere i messaggi SMS da inviare.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-148">The `ICollector<SMSMessage>` parameter is a collection of `SMSMessage` instances and is used to collect the SMS messages you want to send.</span></span> <span data-ttu-id="d3b5f-149">Quando la funzione ha terminato l'esecuzione, tutte le istanze di `SMSMessage` aggiunte a questa raccolta vengono passate a Twilio e inviate ai relativi destinatari.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-149">After the function has finished running, any instances of `SMSMessage` added to this collection are passed to Twilio and sent to the relevant recipients.</span></span>

1. <span data-ttu-id="d3b5f-150">Nella funzione `SendLocation` aggiungere il codice per riprodurre a ciclo i numeri di telefono nel `PostData` e creare un messaggio SMS per ognuno di essi.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-150">In the `SendLocation` function, add code to loop through the phone numbers in the `PostData` and create an SMS message for each one.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        SMSMessage message = new SMSMessage
        {
            Body = $"I'm here! {url}",
            To = toNo
        };
    }
    ```

    <span data-ttu-id="d3b5f-151">Il messaggio richiede il numero di telefono per l'invio a e un corpo che contiene l'URL di Google Maps creato dalla posizione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-151">The message needs the phone number to send to and a body that contains the Google Maps URL created from the user's location.</span></span>

2. <span data-ttu-id="d3b5f-152">Registrare ogni messaggio e quindi aggiungerlo alla raccolta `messages`.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-152">Log each message, and then add it to the `messages` collection.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.Info($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

<span data-ttu-id="d3b5f-153">Il metodo `SendLocation` completo è riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-153">The complete `SendLocation` method is shown below.</span></span>

```cs
[FunctionName("SendLocation")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                                Route = null)]HttpRequestMessage req,
                                                    [TwilioSms(AccountSidSetting = "TwilioAccountSid",
                                                                AuthTokenSetting = "TwilioAuthToken",
                                                                From = "<your Twilio phone number>")]ICollector<SMSMessage> messages,
                                                    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    PostData data = await req.Content.ReadAsAsync<PostData>();
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.Info($"URL created - {url}");
    foreach (string toNo in data.ToNumbers)
    {
        SMSMessage message = new SMSMessage
        {
            Body = $"I'm here! {url}",
            To = toNo
        };
        log.Info($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

## <a name="test-it-out"></a><span data-ttu-id="d3b5f-154">Eseguire il test</span><span class="sxs-lookup"><span data-stu-id="d3b5f-154">Test it out</span></span>

1. <span data-ttu-id="d3b5f-155">Impostare l'app `ImHere.Functions` come progetto di avvio e avviarla senza eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-155">Set the `ImHere.Functions` app as the startup project and start it without debugging.</span></span>

2. <span data-ttu-id="d3b5f-156">Impostare l'app `ImHere.UWP` come progetto di avvio ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-156">Set the `ImHere.UWP` app as the startup project and run it.</span></span>

3. <span data-ttu-id="d3b5f-157">Immettere il proprio numero di telefono in formato internazionale (+\<prefisso internazionale\>\<numero di telefono\>) nell'app Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-157">Enter your own phone number in international format (+\<country code\>\<phone number\>) into the Xamarin.Forms app.</span></span> <span data-ttu-id="d3b5f-158">Gli account in versione di valutazione di Twilio possono inviare messaggi solo a numeri di telefono verificati quindi, per il momento, sarà possibile solo inviare messaggi a se stessi a meno che non si esegua l'aggiornamento a un account a pagamento o non vengano verificati altri numeri.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-158">Twilio trial accounts can send  messages only to verified phone numbers, so for now, you'll only be able to message yourself unless you upgrade to a paid account or verify other numbers.</span></span>

4. <span data-ttu-id="d3b5f-159">Fare clic sul pulsante **Invia posizione**.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-159">Click the **Send Location** button.</span></span> <span data-ttu-id="d3b5f-160">Se il messaggio SMS è stato inviato correttamente, nell'app Xamarin.Forms verrà visualizzato il messaggio "Posizione inviata correttamente".</span><span class="sxs-lookup"><span data-stu-id="d3b5f-160">If the SMS message was sent successfully, you'll see a message in the Xamarin.Forms app saying "Location sent successfully".</span></span>

    ![L'app Xamarin.Forms mostra il percorso come inviato](../media-drafts/7-ui-location-sent.png)

5. <span data-ttu-id="d3b5f-162">Nei log console per la funzione di Azure, verrà visualizzato il messaggio creato e inviato.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-162">In the console logs for the Azure function, you'll see the message being created and sent.</span></span> <span data-ttu-id="d3b5f-163">Se si verificano errori (ad esempio, il numero ha un formato non corretto), si verrà disconnessi.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-163">If any errors occur (such as, the number is in the wrong format), they will be logged out here.</span></span>

    ![La console di funzioni di Azure con il messaggio visualizzato è stata inviata](../media-drafts/7-function-message-sent.png)

6. <span data-ttu-id="d3b5f-165">Controllare se sul telefono è presente un messaggio.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-165">Check your phone for a message.</span></span> <span data-ttu-id="d3b5f-166">Fare clic sul collegamento nel messaggio per visualizzare la posizione.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-166">Follow the link in the message to see your location.</span></span>

    ![Il messaggio SMS ricevuto su un telefono cellulare](../media-drafts/7-message-received.png)

## <a name="summary"></a><span data-ttu-id="d3b5f-168">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="d3b5f-168">Summary</span></span>

<span data-ttu-id="d3b5f-169">In questa unità si è appreso come creare un'associazione di Twilio per la funzione di Azure e inviare un messaggio SMS con la posizione dell'utente a una funzione in esecuzione in locale.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-169">In this unit, you learned how to create a Twilio binding for the Azure function and send an SMS message with the user's location to a function that was running locally.</span></span> <span data-ttu-id="d3b5f-170">Nell'unità di successiva si pubblicherà la funzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="d3b5f-170">In the next unit, you publish the function to Azure.</span></span>