<span data-ttu-id="cdb46-101">Qui di seguito si proseguirà con l'esempio della trasmissione a ingranaggi aggiungendo la logica per il servizio relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="cdb46-101">Let's continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="cdb46-102">In particolare, si riceveranno dati da una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cdb46-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="cdb46-103">Requisiti della funzione</span><span class="sxs-lookup"><span data-stu-id="cdb46-103">Function requirements</span></span>

<span data-ttu-id="cdb46-104">Innanzitutto, definire i requisiti per la logica:</span><span class="sxs-lookup"><span data-stu-id="cdb46-104">First, we need to define some requirements for our logic:</span></span>

- <span data-ttu-id="cdb46-105">Le temperature tra 0 e 25 dovranno essere contrassegnate come **OK**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-105">Temperatures between 0-25 should be flagged as **OK**.</span></span>
- <span data-ttu-id="cdb46-106">Le temperature tra 26 e 50 dovranno essere contrassegnate come **CAUTION**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-106">Temperatures between 26-50 should be flagged as **CAUTION**.</span></span>
- <span data-ttu-id="cdb46-107">Le temperature oltre 50 dovranno essere contrassegnate come **DANGER**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-107">Temperatures above 50 should be flagged as **DANGER**.</span></span>

## <a name="add-a-function-to-our-function-app"></a><span data-ttu-id="cdb46-108">Aggiungere una funzione a un'app per le funzioni</span><span class="sxs-lookup"><span data-stu-id="cdb46-108">Add a function to our function app</span></span>

<span data-ttu-id="cdb46-109">Come descritto nell'unità precedente, Azure offre modelli che consentono di creare funzioni.</span><span class="sxs-lookup"><span data-stu-id="cdb46-109">As we discussed in the preceding unit, Azure provides templates that help you get started building functions.</span></span> <span data-ttu-id="cdb46-110">In questa unità si userà il modello `HttpTrigger` per implementare il servizio relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="cdb46-110">In this unit, we'll use the `HttpTrigger` template to implement the temperature service.</span></span>

1. <span data-ttu-id="cdb46-111">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="cdb46-111">Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).</span></span>

1. <span data-ttu-id="cdb46-112">Selezionare il gruppo di risorse creato nel primo esercizio scegliendo **Tutte le risorse** dal menu a sinistra e selezionando "**<rgn>[Nome gruppo di risorse sandbox]</rgn>**".</span><span class="sxs-lookup"><span data-stu-id="cdb46-112">Select the resource group from the first exercise by choosing **All resources** in the left-hand menu, and then selecting "**<rgn>[sandbox resource group name]</rgn>**".</span></span>

1. <span data-ttu-id="cdb46-113">Verranno quindi visualizzate le risorse del gruppo.</span><span class="sxs-lookup"><span data-stu-id="cdb46-113">The resources for the group will then be displayed.</span></span> <span data-ttu-id="cdb46-114">Fare clic sul nome dell'app per le funzioni creata nell'esercizio precedente selezionando l'elemento **escalator-functions-xxxxxxx**, contrassegnato anche con l'icona Funzione a forma di fulmine.</span><span class="sxs-lookup"><span data-stu-id="cdb46-114">Click the name of the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt Function icon).</span></span>

    ![Screenshot del portale di Azure che visualizza il pannello Tutte le risorse evidenziato e l'app per le funzioni escalator che è stata creata.](../media/5-access-function-app.png)

<!-- Start temporary fix for issue #2498. -->
> [!IMPORTANT]
> <span data-ttu-id="cdb46-116">Gli esercizi in questo modulo attualmente funzionano con Funzioni di Azure V1.</span><span class="sxs-lookup"><span data-stu-id="cdb46-116">The exercises in this module currently work with Azure Functions V1.</span></span> <span data-ttu-id="cdb46-117">Seguire questa procedura con attenzione per assicurarsi che l'app per le funzioni usi la versione del runtime V1.</span><span class="sxs-lookup"><span data-stu-id="cdb46-117">Please follow these steps carefully to make sure your function app uses the V1 runtime version.</span></span> 

1. <span data-ttu-id="cdb46-118">Selezionare l'app per le funzioni nell'elenco **App per le funzioni**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-118">Select your function app in the **Function Apps** list.</span></span>
1. <span data-ttu-id="cdb46-119">Selezionare **Funzionalità della piattaforma**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-119">Select **Platform features**.</span></span>
1. <span data-ttu-id="cdb46-120">Nella schermata **Funzionalità della piattaforma** selezionare **Impostazioni dell'app per le funzioni** in **Impostazioni generali**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-120">In the **Platform features** screen, select **Function app settings** under **General Settings**.</span></span>
1. <span data-ttu-id="cdb46-121">Selezionare *~1* in **Versione runtime**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-121">Select *~1* in the **Runtime version** .</span></span>
1. <span data-ttu-id="cdb46-122">Chiudere **Impostazioni dell'app per le funzioni**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-122">Close **Function app settings**.</span></span>

<span data-ttu-id="cdb46-123">L'app per le funzioni è ora configurata per usare il runtime V1 di Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="cdb46-123">Our function app is now configured to use the Azure Functions V1 runtime.</span></span> <span data-ttu-id="cdb46-124">È ora possibile continuare a creare la prima funzione.</span><span class="sxs-lookup"><span data-stu-id="cdb46-124">We can now continue to create our first function.</span></span>
<!-- End temporary fix for issue #2498. --> 

1. <span data-ttu-id="cdb46-125">Nel menu a sinistra vengono visualizzati il nome dell'app per le funzioni e un sottomenu contenente tre elementi: *Funzioni*, *Proxy* e *Slot*.</span><span class="sxs-lookup"><span data-stu-id="cdb46-125">The left-side menu displays your function app name and a submenu with three items: *Functions*, *Proxies*, and *Slots*.</span></span>  

1. <span data-ttu-id="cdb46-126">Per iniziare a creare la prima funzione, selezionare **Funzioni** e fare clic su **Nuova funzione** nella parte superiore della pagina visualizzata.</span><span class="sxs-lookup"><span data-stu-id="cdb46-126">To start creating our first function, select **Functions** and click  the **New function** button at the top of the resulting page.</span></span>

    ![Screenshot del portale di Azure che visualizza l'elenco Funzioni dell'app per le funzioni, con la voce di menu Funzioni e il pulsante Nuova funzione evidenziato.](../media/5-function-add-button.png)

1. <span data-ttu-id="cdb46-128">Nella schermata Avvio rapido selezionare il collegamento **Funzione personalizzata** nella sezione **Iniziare da zero**, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="cdb46-128">In the Quickstart screen, select the **Custom function** link in the **Get started on your own** section as shown in the following screenshot.</span></span> <span data-ttu-id="cdb46-129">Se non viene visualizzata la schermata di avvio rapido, fare clic sul collegamento **passare all'Avvio rapido** nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="cdb46-129">If you don't see the Quickstart screen, click on the **go to the quickstart** link at the top of the page.</span></span>

    ![Screenshot del portale di Azure che visualizza il pannello Avvio rapido con il pulsante Funzione personalizzata evidenziato nella sezione Iniziare da zero.](../media/5-custom-function.png)

1. <span data-ttu-id="cdb46-131">Dall'elenco dei modelli visualizzati nella schermata, selezionare il modello **Trigger HTTP**, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="cdb46-131">From the list of templates displayed on the screen, select the **HTTP trigger** template as shown in the following screenshot.</span></span>

1. <span data-ttu-id="cdb46-132">Immettere **DriveGearTemperatureService** nel campo relativo al nome all'interno della finestra di dialogo **Nuova funzione** visualizzata.</span><span class="sxs-lookup"><span data-stu-id="cdb46-132">Enter **DriveGearTemperatureService** in the name field of the **New Function** dialog that appears.</span></span> <span data-ttu-id="cdb46-133">Lasciare il livello Autorizzazione impostato su "Funzione" e premere il pulsante **Crea** per creare la funzione.</span><span class="sxs-lookup"><span data-stu-id="cdb46-133">Leave the Authorization level as "Function" and press the **Create** button to create the function.</span></span>

1. <span data-ttu-id="cdb46-134">Quando la creazione della funzione viene completata si apre l'editor di codice con il contenuto del file di codice *index.js*.</span><span class="sxs-lookup"><span data-stu-id="cdb46-134">When your function creation completes, the code editor opens with the contents of the *index.js* code file.</span></span> <span data-ttu-id="cdb46-135">Nel frammento di codice seguente viene elencato il codice predefinito che il modello ha generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="cdb46-135">The default code that the template generated for us is listed in the following snippet.</span></span>

    ```javascript
    module.exports = function (context, req) {
        context.log('JavaScript HTTP trigger function processed a request.');
    
        if (req.query.name || (req.body && req.body.name)) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "Hello " + (req.query.name || req.body.name)
            };
        }
        else {
            context.res = {
                status: 400,
                body: "Please pass a name on the query string or in the request body"
            };
        }
        context.done();
    };
    ```

    <span data-ttu-id="cdb46-136">La funzione prevede il passaggio di un nome tramite la stringa della query di richiesta HTTP o come parte del corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="cdb46-136">Our function expects a name to be passed in either through the HTTP request query string or as part of the request body.</span></span> <span data-ttu-id="cdb46-137">La funzione risponde restituendo il messaggio **Salve, {nome}** e ripetendo il nome che è stato inviato nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="cdb46-137">The function responds by returning the message  **Hello, {name}**, echoing back the name that was sent in the request.</span></span>

    <span data-ttu-id="cdb46-138">Sul lato destro della visualizzazione del codice sorgente sono presenti due schede.</span><span class="sxs-lookup"><span data-stu-id="cdb46-138">On the right-hand side of the source view, you'll find two tabs.</span></span> <span data-ttu-id="cdb46-139">La scheda **Visualizza file** elenca il codice e il file di configurazione per la funzione.</span><span class="sxs-lookup"><span data-stu-id="cdb46-139">The **View files** tab lists the code and config file for your function.</span></span>  <span data-ttu-id="cdb46-140">Selezionare **function.json** per visualizzare la configurazione della funzione, che è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="cdb46-140">Select **function.json** to view the configuration of the function, which should look like the following:</span></span>

    ```javascript
    {
        "disabled": false,
        "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
        ]
    }
    ```

    <span data-ttu-id="cdb46-141">Con questa configurazione si dichiara che la funzione viene eseguita quando riceve una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cdb46-141">This configuration declares that the function runs when it receives an HTTP request.</span></span> <span data-ttu-id="cdb46-142">L'associazione di output dichiara che la risposta verrà inviata come risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cdb46-142">The output binding declares that the response will be sent as an HTTP response.</span></span>    

## <a name="test-the-function"></a><span data-ttu-id="cdb46-143">Testare la funzione</span><span class="sxs-lookup"><span data-stu-id="cdb46-143">Test the function</span></span>

> [!TIP]
> <span data-ttu-id="cdb46-144">**cURL** è uno strumento da riga di comando che può essere usato per inviare o ricevere file.</span><span class="sxs-lookup"><span data-stu-id="cdb46-144">**cURL** is a command line tool that can be used to send or receive files.</span></span> <span data-ttu-id="cdb46-145">È incluso in Linux, macOS e Windows 10 e può essere scaricato per la maggior parte degli altri sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="cdb46-145">It's included with Linux, macOS, and Windows 10, and can be downloaded for most other operating systems.</span></span> <span data-ttu-id="cdb46-146">cURL supporta vari protocolli come HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3 e così via. Per altre informazioni, consultare i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="cdb46-146">cURL supports numerous protocols like HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3, etc. For more information, refer to the links below:</span></span>
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

<span data-ttu-id="cdb46-147">Per testare la funzione, è possibile inviare una richiesta HTTP all'URL della funzione usando cURL dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="cdb46-147">To test the function, you can send an HTTP request to the function URL using cURL on the command line.</span></span> <span data-ttu-id="cdb46-148">Per trovare l'URL dell'endpoint della funzione, tornare al codice della funzione e selezionare il collegamento **Recupera URL della funzione** come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="cdb46-148">To find the endpoint URL of the function, return to your function code and select the **Get function URL** link, as shown in the following screenshot.</span></span> <span data-ttu-id="cdb46-149">Salvare temporaneamente questo collegamento.</span><span class="sxs-lookup"><span data-stu-id="cdb46-149">Save this link temporarily.</span></span>

![Screenshot del portale di Azure che visualizza l'editor delle funzioni con il pulsante Recupera URL della funzione evidenziato.](../media/5-get-function-url.png)

### <a name="securing-http-triggers"></a><span data-ttu-id="cdb46-151">Protezione dei trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="cdb46-151">Securing HTTP triggers</span></span>

<span data-ttu-id="cdb46-152">I trigger HTTP consentono di usare le chiavi API per bloccare i chiamanti sconosciuti, richiedendo che la chiave sia presente in ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="cdb46-152">HTTP triggers let you use API keys to block unknown callers by requiring the key to be present on each request.</span></span> <span data-ttu-id="cdb46-153">Quando si crea una funzione si seleziona il _livello di autorizzazione_.</span><span class="sxs-lookup"><span data-stu-id="cdb46-153">When you create a function, you select the _authorization level_.</span></span> <span data-ttu-id="cdb46-154">Per impostazione predefinita il livello è impostato su "Funzione", che richiede una chiave API specifica della funzione, ma è possibile impostarlo anche su "Amministratore" per usare una chiave "master" globale o su "Anonimo" per indicare che non è richiesta una chiave.</span><span class="sxs-lookup"><span data-stu-id="cdb46-154">By default, it's set to "Function", which requires a function-specific API key, but it can also be set to "Admin" to use a global "master" key, or "Anonymous" to indicate that no key is required.</span></span> <span data-ttu-id="cdb46-155">Il livello di autorizzazione può essere modificato anche tramite le proprietà di funzione dopo la creazione.</span><span class="sxs-lookup"><span data-stu-id="cdb46-155">You can also change the authorization level through the function properties after creation.</span></span>

<span data-ttu-id="cdb46-156">Poiché è stato specificato "Funzione" durante la creazione di questa funzione, è necessario specificare la chiave quando si invia la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cdb46-156">Since we specified "Function" when we created this function, we will need to supply the key when we send the HTTP request.</span></span> <span data-ttu-id="cdb46-157">È possibile inviarla come parametro della stringa di query denominato `code`, oppure come un'intestazione HTTP (scelta consigliata) denominata `x-functions-key`.</span><span class="sxs-lookup"><span data-stu-id="cdb46-157">You can send it as a query string parameter named `code`, or as an HTTP header (preferred) named `x-functions-key`.</span></span>

<span data-ttu-id="cdb46-158">Le chiavi di funzione e master sono disponibili nella sezione **Gestisci** quando si espande la funzione.</span><span class="sxs-lookup"><span data-stu-id="cdb46-158">The function and master keys are found in the **Manage** section when the function is expanded.</span></span> <span data-ttu-id="cdb46-159">Per impostazione predefinita sono nascoste ed è necessario visualizzarle.</span><span class="sxs-lookup"><span data-stu-id="cdb46-159">By default, they are hidden, and you need to display them.</span></span>

1. <span data-ttu-id="cdb46-160">Espandere la funzione e selezionare la sezione **Gestisci**, visualizzare la chiave di funzione predefinita e copiarla negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="cdb46-160">Expand your function and select the **Manage** section, show the default Function Key, and copy it to the clipboard.</span></span>

    ![Screenshot del portale di Azure che visualizza il pannello Gestisci con la chiave di funzione evidenziata.](../media/5-get-function-key.png)

1. <span data-ttu-id="cdb46-162">Nella riga di comando in cui è stato installato lo strumento **cURL**, formattare un comando cURL con l'URL per la funzione e la chiave di funzione.</span><span class="sxs-lookup"><span data-stu-id="cdb46-162">Next, from the command line where you installed the **cURL** tool, format a cURL command with the URL for your function, and the Function key.</span></span>

    - <span data-ttu-id="cdb46-163">Usare una richiesta `POST`.</span><span class="sxs-lookup"><span data-stu-id="cdb46-163">Use a `POST` request.</span></span>
    - <span data-ttu-id="cdb46-164">Aggiungere un valore dell'intestazione `Content-Type` del tipo `application/json`.</span><span class="sxs-lookup"><span data-stu-id="cdb46-164">Add a `Content-Type` header value of type `application/json`.</span></span>
    - <span data-ttu-id="cdb46-165">Assicurarsi di sostituire l'URL qui sotto con il proprio.</span><span class="sxs-lookup"><span data-stu-id="cdb46-165">Make sure to replace the URL below with your own.</span></span>
    - <span data-ttu-id="cdb46-166">Passare la chiave di funzione come valore dell'intestazione `x-functions-key`.</span><span class="sxs-lookup"><span data-stu-id="cdb46-166">Pass the Function Key as the header value `x-functions-key`.</span></span>

    ```bash
    curl --header "Content-Type: application/json" --header "x-functions-key: <your-function-key>" --request POST --data "{\"name\": \"Azure Function\"}" https://<your-url-here>/api/DriveGearTemperatureService
    ```

<span data-ttu-id="cdb46-167">La funzione risponderà con il testo `"Hello Azure Function"`.</span><span class="sxs-lookup"><span data-stu-id="cdb46-167">The function will respond back with the text `"Hello Azure Function"`.</span></span>

> [!CAUTION]
> <span data-ttu-id="cdb46-168">Se si usa Windows, eseguire il comando `cURL` dal prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="cdb46-168">If you are on Windows, please run  `cURL` from the command prompt.</span></span> <span data-ttu-id="cdb46-169">In PowerShell è presente un comando *curl*, ma è in realtà un alias di Invoke-WebRequest e pertanto non è identico a `cURL`.</span><span class="sxs-lookup"><span data-stu-id="cdb46-169">PowerShell has a *curl* command, but it's an alias for Invoke-WebRequest and is not the same as `cURL`.</span></span>

> [!NOTE]
> <span data-ttu-id="cdb46-170">È anche possibile eseguire il test dalla sezione di una singola funzione con la scheda **Test** a lato di una funzione selezionata. In questo caso non sarà però possibile verificare il funzionamento del sistema della chiave di funzione poiché non è richiesto in questa sede.</span><span class="sxs-lookup"><span data-stu-id="cdb46-170">You can also test from an individual function's section with the **Test** tab on the side of a selected function, though you won't be able to verify the function key system is working, as it is not required here.</span></span> <span data-ttu-id="cdb46-171">Aggiungere l'intestazione appropriata e i valori dei parametri nell'interfaccia Test e fare clic sul pulsante **Esegui** per visualizzare l'output del test.</span><span class="sxs-lookup"><span data-stu-id="cdb46-171">Add the appropriate header and parameter values in the Test interface and click the **Run** button to see the test output.</span></span>

## <a name="add-business-logic-to-the-function"></a><span data-ttu-id="cdb46-172">Aggiungere la logica di business alla funzione</span><span class="sxs-lookup"><span data-stu-id="cdb46-172">Add business logic to the function</span></span>

<span data-ttu-id="cdb46-173">È ora possibile aggiungere la logica alla funzione che controlla le letture delle temperature ricevute e imposta ogni singolo stato.</span><span class="sxs-lookup"><span data-stu-id="cdb46-173">Next, let's add the logic to the function that checks temperature readings that it receives and sets a status for each.</span></span>

<span data-ttu-id="cdb46-174">La funzione prevede una matrice di letture della temperatura.</span><span class="sxs-lookup"><span data-stu-id="cdb46-174">Our function is expecting an array of temperature readings.</span></span> <span data-ttu-id="cdb46-175">Il frammento JSON seguente costituisce un esempio di corpo della richiesta che sarà inviata alla funzione.</span><span class="sxs-lookup"><span data-stu-id="cdb46-175">The following JSON snippet is an example of the request body that we'll send to our function.</span></span> <span data-ttu-id="cdb46-176">Ogni elemento `reading` possiede un ID, un timestamp e una temperatura.</span><span class="sxs-lookup"><span data-stu-id="cdb46-176">Each `reading` entry has an ID, timestamp, and temperature.</span></span>

```json
{
    "readings": [
        {
            "driveGearId": 1,
            "timestamp": 1534263995,
            "temperature": 23
        },
        {
            "driveGearId": 3,
            "timestamp": 1534264048,
            "temperature": 45
        },
        {
            "driveGearId": 18,
            "timestamp": 1534264050,
            "temperature": 55
        }
    ]
}
```

<span data-ttu-id="cdb46-177">Il codice predefinito della funzione sarà poi sostituito con il codice seguente, che implementa la logica di business.</span><span class="sxs-lookup"><span data-stu-id="cdb46-177">Next, we'll replace the default code in our function with the following code that implements our business logic.</span></span>

1. <span data-ttu-id="cdb46-178">Aprire il file **index.js** e sostituirlo con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="cdb46-178">Open the **index.js** file and replace it with the following code.</span></span>

```javascript
module.exports = function (context, req) {
    context.log('Drive Gear Temperature Service triggered');
    if (req.body && req.body.readings) {
        req.body.readings.forEach(function(reading) {

            if(reading.temperature<=25) {
                reading.status = 'OK';
            } else if (reading.temperature<=50) {
                reading.status = 'CAUTION';
            } else {
                reading.status = 'DANGER'
            }
            context.log('Reading is ' + reading.status);
        });

        context.res = {
            // status: 200, /* Defaults to 200 */
            body: {
                "readings": req.body.readings
            }
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please send an array of readings in the request body"
        };
    }
    context.done();
};
```

<span data-ttu-id="cdb46-179">La logica che è stata aggiunta è semplice.</span><span class="sxs-lookup"><span data-stu-id="cdb46-179">The logic we added is straightforward.</span></span> <span data-ttu-id="cdb46-180">Vengono eseguiti l'iterazione della matrice delle letture e il controllo del campo relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="cdb46-180">We iterate over the array of readings and check the temperature field.</span></span> <span data-ttu-id="cdb46-181">A seconda del valore di tale campo, lo stato viene impostato su **OK**, **CAUTION** o **DANGER**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-181">Depending on the value of that field, we set a status of **OK**, **CAUTION**, or **DANGER**.</span></span> <span data-ttu-id="cdb46-182">La matrice delle letture viene quindi restituita dopo aver aggiunto il campo dello stato per ogni voce.</span><span class="sxs-lookup"><span data-stu-id="cdb46-182">We then send back the array of readings with a status field added to each entry.</span></span>

<span data-ttu-id="cdb46-183">Si notino le istruzioni `log`.</span><span class="sxs-lookup"><span data-stu-id="cdb46-183">Notice the `log` statements.</span></span> <span data-ttu-id="cdb46-184">Durante l'esecuzione della funzione, le istruzioni aggiungeranno messaggi nella finestra di log.</span><span class="sxs-lookup"><span data-stu-id="cdb46-184">When the function runs, these statements will add messages in the log window.</span></span>

## <a name="test-our-business-logic"></a><span data-ttu-id="cdb46-185">Testare la logica di business</span><span class="sxs-lookup"><span data-stu-id="cdb46-185">Test our business logic</span></span>

<span data-ttu-id="cdb46-186">In questo caso si userà il riquadro **Test** nel portale per testare la funzione.</span><span class="sxs-lookup"><span data-stu-id="cdb46-186">In this case, we're going to use the **Test** pane in the portal to test our function.</span></span>

1. <span data-ttu-id="cdb46-187">Aprire la finestra **Test** dal menu a comparsa a destra.</span><span class="sxs-lookup"><span data-stu-id="cdb46-187">Open the **Test** window from the right-hand side flyout menu.</span></span>

1. <span data-ttu-id="cdb46-188">Incollare la richiesta di esempio nella casella di testo del corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="cdb46-188">Paste the sample request into the request body text box.</span></span>

    ```json
    {
        "readings": [
            {
                "driveGearId": 1,
                "timestamp": 1534263995,
                "temperature": 23
            },
            {
                "driveGearId": 3,
                "timestamp": 1534264048,
                "temperature": 45
            },
            {
                "driveGearId": 18,
                "timestamp": 1534264050,
                "temperature": 55
            }
        ]
    }
    ```

1. <span data-ttu-id="cdb46-189">Selezionare **Esegui** e visualizzare la risposta nel riquadro di output.</span><span class="sxs-lookup"><span data-stu-id="cdb46-189">Select **Run** and view the response in the output pane.</span></span> <span data-ttu-id="cdb46-190">Per visualizzare i messaggi di log, aprire la scheda **Log** nel riquadro a comparsa nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="cdb46-190">To see log messages, open the **Logs** tab in the bottom flyout of the page.</span></span> <span data-ttu-id="cdb46-191">Lo screenshot seguente illustra un esempio di risposta nel riquadro di output e i messaggi nel riquadro **Log**.</span><span class="sxs-lookup"><span data-stu-id="cdb46-191">The following screenshot shows an example response in the output pane and messages in the  **Logs** pane.</span></span>

    ![Screenshot del portale di Azure che visualizza il pannello dell'editor di funzioni con le schede Test e Log visualizzate.](../media/5-portal-testing.png)

    <span data-ttu-id="cdb46-194">Nel riquadro di output è possibile osservare che il campo relativo allo stato è stato aggiunto correttamente per ogni lettura.</span><span class="sxs-lookup"><span data-stu-id="cdb46-194">You can see in the output pane that our status field has been correctly added to each of the readings.</span></span>

    <span data-ttu-id="cdb46-195">Passare al dashboard **Monitoraggio** per verificare se la richiesta è stata registrata in Application Insights.</span><span class="sxs-lookup"><span data-stu-id="cdb46-195">If you navigate to the **Monitor** dashboard, you'll see that the request has been logged to Application Insights.</span></span>

    ![Screenshot del portale di Azure che visualizza l'esito positivo del test precedente nel dashboard Monitoraggio della funzione.](../media/5-app-insights.png)
