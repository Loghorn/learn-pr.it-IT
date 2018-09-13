<span data-ttu-id="6be90-101">Qui di seguito si proseguirà con l'esempio della trasmissione a ingranaggi aggiungendo la logica per il servizio relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="6be90-101">Let's continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="6be90-102">In particolare, si riceveranno dati da una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6be90-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="6be90-103">Requisiti della funzione</span><span class="sxs-lookup"><span data-stu-id="6be90-103">Function requirements</span></span>
<span data-ttu-id="6be90-104">Innanzitutto, definire i requisiti per la logica:</span><span class="sxs-lookup"><span data-stu-id="6be90-104">First, we need to define some requirements for our logic:</span></span>

- <span data-ttu-id="6be90-105">Le temperature tra 0 e 25 dovranno essere contrassegnate come **OK**.</span><span class="sxs-lookup"><span data-stu-id="6be90-105">Temperatures between 0-25 should be flagged as **OK**.</span></span>
- <span data-ttu-id="6be90-106">Le temperature tra 26 e 50 dovranno essere contrassegnate come **CAUTION**.</span><span class="sxs-lookup"><span data-stu-id="6be90-106">Temperatures between 26-50 should be flagged as **CAUTION**.</span></span>
- <span data-ttu-id="6be90-107">Le temperature oltre 50 dovranno essere contrassegnate come **DANGER**.</span><span class="sxs-lookup"><span data-stu-id="6be90-107">Temperatures above 50 should be flagged as **DANGER**.</span></span>

## <a name="add-a-function-to-our-function-app"></a><span data-ttu-id="6be90-108">Aggiungere una funzione a un'app per le funzioni</span><span class="sxs-lookup"><span data-stu-id="6be90-108">Add a function to our function app</span></span>

<span data-ttu-id="6be90-109">Come descritto nell'unità precedente, Azure offre modelli che consentono di creare funzioni.</span><span class="sxs-lookup"><span data-stu-id="6be90-109">As we discussed in the preceding unit, Azure provides templates that help you get started building functions.</span></span> <span data-ttu-id="6be90-110">In questa unità si userà il modello `HttpTrigger` per implementare il servizio relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="6be90-110">In this unit, we'll use the `HttpTrigger` template to implement the temperature service.</span></span>

1. <span data-ttu-id="6be90-111">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true) con il proprio account Azure.</span><span class="sxs-lookup"><span data-stu-id="6be90-111">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true) using your Azure account.</span></span>

1. <span data-ttu-id="6be90-112">Selezionare il gruppo di risorse creato nel primo esercizio scegliendo **Tutte le risorse** dal menu a sinistra e selezionando **escalator-functions-group**.</span><span class="sxs-lookup"><span data-stu-id="6be90-112">Select the resource group you created in the first exercise by choosing **All resources** in the left-hand menu, and then selecting **escalator-functions-group**.</span></span>

1. <span data-ttu-id="6be90-113">Verranno quindi visualizzate le risorse del gruppo.</span><span class="sxs-lookup"><span data-stu-id="6be90-113">The resources for the group will then be displayed.</span></span> <span data-ttu-id="6be90-114">Accedere all'app per le funzioni creata nell'esercizio precedente selezionando l'elemento **escalator-functions-xxxxxxx**, contrassegnato anche con l'icona a forma di fulmine.</span><span class="sxs-lookup"><span data-stu-id="6be90-114">Access the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt icon).</span></span>

  ![Screenshot di Tutte le risorse nel portale, in cui è evidenziato escalator-functions-group e l'app per le funzioni creata.](../media-draft/5-access-function-app.png)

1. <span data-ttu-id="6be90-116">Nel menu a sinistra vengono visualizzati il nome dell'app per le funzioni e un sottomenu contenente tre elementi: *Funzioni*, *Proxy* e *Slot*.</span><span class="sxs-lookup"><span data-stu-id="6be90-116">The left-side menu displays your function app name and a submenu with three items: *Functions*, *Proxies*, and *Slots*.</span></span>  <span data-ttu-id="6be90-117">Per creare la prima funzione, spostare il mouse su **Funzioni** nell'albero di spostamento e fare clic sul pulsante **+** visualizzato.</span><span class="sxs-lookup"><span data-stu-id="6be90-117">To start creating our first function, move your mouse over **Functions** in the navigation tree and click  the **+** button that appears.</span></span>

  ![Screenshot del segno di addizione visualizzato al passaggio del mouse sulla voce di menu Funzioni che, se selezionata, avvia la procedura di creazione della funzione.](../media-draft/5-function-add-button.png)

1. <span data-ttu-id="6be90-119">Nella schermata Avvio rapido selezionare il collegamento **Funzione personalizzata** nella sezione **Iniziare da zero**, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="6be90-119">In the Quickstart screen, select the **Custom function** link in the **Get started on your own** section as shown in the following screenshot.</span></span>
 
  ![Screenshot della schermata Avvio rapido con il pulsante "Funzione personalizzata" evidenziato.](../media-draft/5-custom-function.png)

1. <span data-ttu-id="6be90-121">Dall'elenco dei modelli visualizzati nella schermata, selezionare l'implementazione JavaScript del modello di trigger HTTP, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="6be90-121">From the list of templates displayed on the screen, select the JavaScript implementation of the HTTP trigger template as shown in the following screenshot.</span></span>

  ![Screenshot dell'elenco di modelli, con il trigger HTTP e l'opzione JavaScript evidenziati.](../media-draft/5-httptrigger-template.png)

1.  <span data-ttu-id="6be90-123">Immettere **DriveGearTemperatureService** nel campo relativo al nome all'interno della finestra di dialogo **Nuova funzione** visualizzata.</span><span class="sxs-lookup"><span data-stu-id="6be90-123">Enter **DriveGearTemperatureService** in the name field of the **New Function** dialog that appears.</span></span> <span data-ttu-id="6be90-124">Lasciare il livello Autorizzazione impostato su "Funzione" e premere il pulsante **Crea** per creare la funzione.</span><span class="sxs-lookup"><span data-stu-id="6be90-124">Leave the Authorization level as "Function" and press the **Create** button to create the function.</span></span>

  ![Screenshot del modulo Nuova funzione, con il campo Nome evidenziato e il valore "DriveGearTemperatureService" impostato](../media-draft/5-create-httptrigger-form.png)

1. <span data-ttu-id="6be90-126">Quando la creazione della funzione viene completata, si aprirà l'editor di codice con il contenuto del file di codice *index. js*.</span><span class="sxs-lookup"><span data-stu-id="6be90-126">When your function creation completes, the code editor opens with the contents of the *index.js* code file.</span></span> <span data-ttu-id="6be90-127">Nel frammento di codice seguente viene elencato il codice predefinito che il modello ha generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6be90-127">The default code that the template generated for us is listed in the following snippet.</span></span>

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

<span data-ttu-id="6be90-128">La funzione prevede il passaggio di un nome tramite la stringa della query di richiesta HTTP o come parte del corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="6be90-128">Our function expects a name to be passed in either through the HTTP request query string or as part of the request body.</span></span> <span data-ttu-id="6be90-129">La funzione risponde restituendo il messaggio **Salve, {nome}** e ripetendo il nome che è stato inviato nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="6be90-129">The function responds by returning the message  **Hello, {name}**, echoing back the name that was sent in the request.</span></span>

<span data-ttu-id="6be90-130">Sul lato destro della visualizzazione del codice sorgente saranno presenti due schede.</span><span class="sxs-lookup"><span data-stu-id="6be90-130">On the right-hand side of the source view, you'll find two tabs.</span></span> <span data-ttu-id="6be90-131">Nel **file di visualizzazione** vengono elencati il codice e il file di configurazione per la funzione.</span><span class="sxs-lookup"><span data-stu-id="6be90-131">The **View file** lists the code and config file for your function.</span></span>  <span data-ttu-id="6be90-132">Selezionare **function.json** per visualizzare la configurazione della funzione, che dovrebbe essere simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="6be90-132">Select **function.json** to view the configuration of the function which should look like the following:</span></span> 

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

<span data-ttu-id="6be90-133">Con questa configurazione si dichiara che la funzione viene eseguita quando riceve una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6be90-133">This configuration declares that the function runs when it receives an HTTP request.</span></span> <span data-ttu-id="6be90-134">L'associazione di output dichiara che la risposta verrà inviata come risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6be90-134">The output binding declares that the response will be sent as an HTTP response.</span></span>

## <a name="test-the-function-using-curl"></a><span data-ttu-id="6be90-135">Testare la funzione usando cURL</span><span class="sxs-lookup"><span data-stu-id="6be90-135">Test the function using cURL</span></span>

> [!TIP]
> <span data-ttu-id="6be90-136">**cURL** è uno strumento da riga di comando che può essere usato per inviare o ricevere file.</span><span class="sxs-lookup"><span data-stu-id="6be90-136">**cURL** is a command line tool that can be used to send or receive files.</span></span> <span data-ttu-id="6be90-137">È incluso in Linux, macOS e Windows 10 e può essere scaricato per la maggior parte degli altri sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="6be90-137">It's included with Linux, macOS, and Windows 10, and can be downloaded for most other operating systems.</span></span> <span data-ttu-id="6be90-138">cURL supporta vari protocolli, come HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3 e così via. Per altre informazioni, consultare i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="6be90-138">cURL supports numerous protocols like HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3, etc. For more information, please refer to the links below:</span></span>
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

<span data-ttu-id="6be90-139">Per testare la funzione, è possibile inviare una richiesta HTTP all'URL della funzione usando cURL dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6be90-139">To test the function, you can send an HTTP request to the function URL using cURL on the command line.</span></span> <span data-ttu-id="6be90-140">Per trovare l'URL dell'endpoint della funzione, tornare al codice della funzione e selezionare il collegamento **Recupera URL della funzione**, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="6be90-140">To find the endpoint URL of the function, return to your function code and select the **Get function URL** link, as shown in the following screenshot.</span></span> <span data-ttu-id="6be90-141">Salvare temporaneamente questo collegamento.</span><span class="sxs-lookup"><span data-stu-id="6be90-141">Save this link off temporarily.</span></span>  

 ![Screenshot dell'editor di codice nella sezione App per le funzioni del portale.](../media-draft/5-get-function-url.png)

### <a name="securing-http-triggers"></a><span data-ttu-id="6be90-144">Protezione dei trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="6be90-144">Securing HTTP triggers</span></span>
<span data-ttu-id="6be90-145">I trigger HTTP consentono di usare le chiavi API per bloccare i chiamanti sconosciuti, richiedendo che la chiave sia presente in ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6be90-145">HTTP triggers let you use API keys to block unknown callers by requiring the key to be present on each request.</span></span> <span data-ttu-id="6be90-146">Quando si crea una funzione, si seleziona il _livello di autorizzazione_.</span><span class="sxs-lookup"><span data-stu-id="6be90-146">When you create a function, you select the _authorization level_.</span></span> <span data-ttu-id="6be90-147">Per impostazione predefinita è impostato su "Funzione", che richiede una chiave API specifica della funzione, ma è possibile impostarlo anche su "Amministratore" per usare una chiave "master" globale o "Anonimo" per indicare che non sono richieste chiavi.</span><span class="sxs-lookup"><span data-stu-id="6be90-147">By default, it's set to "Function" which requires a function-specific API key, but it can also be set to "Admin" to use a global "master" key, or "Anonymous" to indicate that no key is required.</span></span> <span data-ttu-id="6be90-148">Il livello di autorizzazione può essere modificato anche tramite le proprietà di funzione dopo la creazione.</span><span class="sxs-lookup"><span data-stu-id="6be90-148">You can also change the authorization level through the function properties after creation.</span></span>

<span data-ttu-id="6be90-149">Poiché è stato specificato "Funzione" durante la creazione di questa funzione, è necessario specificare la chiave quando si invia la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6be90-149">Since we specified "Function" when we created this function, we will need to supply the key when we send the HTTP request.</span></span> <span data-ttu-id="6be90-150">È possibile inviarla come parametro della stringa di query denominato `code`, oppure come un'intestazione HTTP (scelta consigliata) denominata `x-functions-key`.</span><span class="sxs-lookup"><span data-stu-id="6be90-150">You can send it as a query string parameter named `code`, or as an HTTP header (preferred) named `x-functions-key`.</span></span>

<span data-ttu-id="6be90-151">Le chiavi di funzione e master sono disponibili nella sezione **Gestisci** quando si espande la funzione.</span><span class="sxs-lookup"><span data-stu-id="6be90-151">The function and master keys are found in the **Manage** section when the function is expanded.</span></span> <span data-ttu-id="6be90-152">Per impostazione predefinita sono nascoste ed è necessario mostrarle.</span><span class="sxs-lookup"><span data-stu-id="6be90-152">By default, they are hidden, and you need to display them.</span></span>

1. <span data-ttu-id="6be90-153">Espandere la funzione e scegliere la sezione **Gestisci**, mostrare la Chiave di funzione predefinita e copiarla negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="6be90-153">Expand your function and select the **Manage** section, show the default Function Key and copy it to the clipboard.</span></span>

![Pannello per ottenere la chiave di funzione dal portale di Azure](../media-draft/5-get-function-key.png)

1. <span data-ttu-id="6be90-155">Formattare quindi un comando cURL con l'URL per la funzione e la chiave di funzione.</span><span class="sxs-lookup"><span data-stu-id="6be90-155">Next, format a cURL command with the URL for your function, and the Function key.</span></span>

    - <span data-ttu-id="6be90-156">Usare una richiesta `POST`.</span><span class="sxs-lookup"><span data-stu-id="6be90-156">Use a `POST` request.</span></span>
    - <span data-ttu-id="6be90-157">Aggiungere un valore dell'intestazione `Content-Type` del tipo `application/json`.</span><span class="sxs-lookup"><span data-stu-id="6be90-157">Add a `Content-Type` header value of type `application/json`.</span></span>
    - <span data-ttu-id="6be90-158">Assicurarsi di sostituire l'URL qui sotto con il proprio.</span><span class="sxs-lookup"><span data-stu-id="6be90-158">Make sure to replace the URL below with your own.</span></span>
    - <span data-ttu-id="6be90-159">Passare la chiave di funzione come valore dell'intestazione `x-functions-key`.</span><span class="sxs-lookup"><span data-stu-id="6be90-159">Pass the Function Key as the header value `x-functions-key`.</span></span>

```bash
curl --header "Content-Type: application/json" --header "x-functions-key: VCWjWkBTvWBsnvw0TlbIbtsav3P3J80m/PKe8WclH0C3RSmvG4Sy8w==" --request POST --data "{\"name\": \"Azure Function\"}" https://<your-url-here>/api/DriveGearTemperatureService
```

<span data-ttu-id="6be90-160">La funzione risponderà con il testo `"Hello Azure Function"`.</span><span class="sxs-lookup"><span data-stu-id="6be90-160">The function will respond back with the text `"Hello Azure Function"`.</span></span>

## <a name="add-business-logic-to-the-function"></a><span data-ttu-id="6be90-161">Aggiungere la logica di business alla funzione</span><span class="sxs-lookup"><span data-stu-id="6be90-161">Add business logic to the function</span></span>

<span data-ttu-id="6be90-162">È ora possibile aggiungere la logica alla funzione che controlla le letture delle temperature ricevute e imposta ogni singolo stato.</span><span class="sxs-lookup"><span data-stu-id="6be90-162">Next, let's add the logic to the function that checks temperature readings that it receives and sets a status for each.</span></span>

<span data-ttu-id="6be90-163">La funzione prevede una matrice di letture della temperatura.</span><span class="sxs-lookup"><span data-stu-id="6be90-163">Our function is expecting an array of temperature readings.</span></span> <span data-ttu-id="6be90-164">Il frammento JSON seguente costituisce un esempio di corpo della richiesta che sarà inviata alla funzione.</span><span class="sxs-lookup"><span data-stu-id="6be90-164">The following JSON snippet is an example of the request body that we'll send to our function.</span></span> <span data-ttu-id="6be90-165">Ogni elemento `reading` possiede un ID, un timestamp e una temperatura.</span><span class="sxs-lookup"><span data-stu-id="6be90-165">Each `reading` entry has an ID, timestamp, and temperature.</span></span>

```javascript
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

<span data-ttu-id="6be90-166">Il codice predefinito della funzione sarà poi sostituito con il codice seguente, che implementa la logica di business.</span><span class="sxs-lookup"><span data-stu-id="6be90-166">Next, we'll replace the default code in our function with the following code that implements our business logic.</span></span> 

1. <span data-ttu-id="6be90-167">Aprire il file **index.js** e sostituirlo con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="6be90-167">Open the **index.js** file and replace it with the following code.</span></span>

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

<span data-ttu-id="6be90-168">La logica che è stata aggiunta è semplice.</span><span class="sxs-lookup"><span data-stu-id="6be90-168">The logic we added is straightforward.</span></span> <span data-ttu-id="6be90-169">Vengono eseguiti l'iterazione della matrice delle letture e il controllo del campo relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="6be90-169">We iterate over the array of readings and check the temperature field.</span></span> <span data-ttu-id="6be90-170">A seconda del valore di tale campo, lo stato viene impostato su **OK**, **CAUTION** o **DANGER**.</span><span class="sxs-lookup"><span data-stu-id="6be90-170">Depending on the value of that field, we set a status of **OK**, **CAUTION**, or **DANGER**.</span></span> <span data-ttu-id="6be90-171">La matrice delle letture viene quindi restituita dopo aver aggiunto il campo dello stato per ogni voce.</span><span class="sxs-lookup"><span data-stu-id="6be90-171">We then send back the array of readings with a status field added to each entry.</span></span>

<span data-ttu-id="6be90-172">Si notino le istruzioni `log`.</span><span class="sxs-lookup"><span data-stu-id="6be90-172">Notice the `log` statements.</span></span> <span data-ttu-id="6be90-173">Durante l'esecuzione della funzione, le istruzioni aggiungeranno messaggi nella finestra di log.</span><span class="sxs-lookup"><span data-stu-id="6be90-173">When the function runs, these statements will add messages in the log window.</span></span>

## <a name="test-our-business-logic"></a><span data-ttu-id="6be90-174">Testare la logica di business</span><span class="sxs-lookup"><span data-stu-id="6be90-174">Test our business logic</span></span>

<span data-ttu-id="6be90-175">In questo caso si userà il riquadro **Test** nel portale per testare la funzione.</span><span class="sxs-lookup"><span data-stu-id="6be90-175">In this case, we're going to use the **Test** pane in the portal to test our function.</span></span>

1. <span data-ttu-id="6be90-176">Aprire la finestra **Test** dal menu a comparsa a destra.</span><span class="sxs-lookup"><span data-stu-id="6be90-176">Open the **Test** window from the right-hand side flyout menu.</span></span>

1. <span data-ttu-id="6be90-177">Incollare la richiesta di esempio dall'esempio precedente nella casella di testo del corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="6be90-177">Paste the sample request from above into the request body text box.</span></span> 

1. <span data-ttu-id="6be90-178">Selezionare **Esegui** e visualizzare la risposta nel riquadro di output.</span><span class="sxs-lookup"><span data-stu-id="6be90-178">Select **Run** and view the response in the output pane.</span></span> <span data-ttu-id="6be90-179">Per visualizzare i messaggi di log, aprire la scheda **Log** nel riquadro a comparsa nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="6be90-179">To see log messages, open the **Logs** tab in the bottom flyout of the page.</span></span> <span data-ttu-id="6be90-180">Lo screenshot seguente illustra un esempio di risposta nel riquadro di output e i messaggi nel riquadro **Log**.</span><span class="sxs-lookup"><span data-stu-id="6be90-180">The following screenshot shows an example response in the output pane and messages in the  **Logs** pane.</span></span>

![Screenshot delle schede "Test" e "Log" nell'interfaccia delle funzioni all'interno del portale.](../media-draft/5-portal-testing.png)

<span data-ttu-id="6be90-183">Nel riquadro di output è possibile osservare che il campo relativo allo stato è stato aggiunto correttamente per ogni lettura.</span><span class="sxs-lookup"><span data-stu-id="6be90-183">You can see in the output pane that our status field has been correctly added to each of the readings.</span></span>

<span data-ttu-id="6be90-184">Passare al dashboard **Monitoraggio** per verificare se la richiesta è stata registrata in Application Insights.</span><span class="sxs-lookup"><span data-stu-id="6be90-184">If you navigate to the **Monitor** dashboard, you'll see that the request has been logged to Application Insights.</span></span>

![Screenshot della parte del dashboard "Monitoraggio" che illustra un messaggio di conferma dalla funzione.](../media-draft/5-app-insights.png)
