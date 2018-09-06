<span data-ttu-id="3a57a-101">In questo esercizio si proseguirà con l'esempio della trasmissione a ingranaggi aggiungendo la logica per il servizio relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="3a57a-101">In this exercise, we'll continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="3a57a-102">In particolare, si riceveranno dati da una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a57a-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="3a57a-103">Requisiti della funzione</span><span class="sxs-lookup"><span data-stu-id="3a57a-103">Function requirements</span></span>
<span data-ttu-id="3a57a-104">I requisiti definiti per la logica sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="3a57a-104">Let's define some requirements for our logic:</span></span>
- <span data-ttu-id="3a57a-105">Le temperature tra 0 e 25 dovranno essere contrassegnate come **OK**</span><span class="sxs-lookup"><span data-stu-id="3a57a-105">temperatures between 0-25 should be flagged as **OK**</span></span>
- <span data-ttu-id="3a57a-106">Le temperature tra 26 e 50 dovranno essere contrassegnate come **CAUTION**</span><span class="sxs-lookup"><span data-stu-id="3a57a-106">temperatures between 26-50 should be flagged as **CAUTION**</span></span>
- <span data-ttu-id="3a57a-107">Le temperature oltre 50 dovranno essere contrassegnate come **DANGER**</span><span class="sxs-lookup"><span data-stu-id="3a57a-107">temperatures above 50 should be flagged as **DANGER**</span></span>

## <a name="add-a-function-to-an-azure-function-app"></a><span data-ttu-id="3a57a-108">Aggiungere una funzione a un'app per le funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="3a57a-108">Add a function to an Azure function app</span></span>

<span data-ttu-id="3a57a-109">Come descritto nell'unità precedente, Azure offre modelli che consentono di creare funzioni.</span><span class="sxs-lookup"><span data-stu-id="3a57a-109">As we discussed in the preceding unit, Azure provides templates that help you get started building functions.</span></span> <span data-ttu-id="3a57a-110">In questo esercizio si userà il modello HttpTrigger per implementare il servizio relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="3a57a-110">In this exercise, we'll use the HttpTrigger template to implement the temperature service.</span></span>

1. <span data-ttu-id="3a57a-111">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true) con l'account di Azure.</span><span class="sxs-lookup"><span data-stu-id="3a57a-111">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true) using your Azure account.</span></span>
1. <span data-ttu-id="3a57a-112">Selezionare il gruppo di risorse creato nel primo esercizio scegliendo **Tutte le risorse** dal menu a sinistra e selezionando **escalator-functions-group**.</span><span class="sxs-lookup"><span data-stu-id="3a57a-112">Select the resource group you created in the first exercise by choosing **All resources** in the left-hand menu, and then selecting **escalator-functions-group**.</span></span>
1. <span data-ttu-id="3a57a-113">Verranno visualizzate le risorse del gruppo.</span><span class="sxs-lookup"><span data-stu-id="3a57a-113">The resources for the group will then be displayed.</span></span> <span data-ttu-id="3a57a-114">Accedere all'app per le funzioni creata nell'esercizio precedente selezionando l'elemento **escalator-functions-xxxxxxx**, contrassegnato anche con l'icona a forma di fulmine.</span><span class="sxs-lookup"><span data-stu-id="3a57a-114">Access the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt icon).</span></span>
  <span data-ttu-id="3a57a-115">![Screenshot di Tutte le risorse nel portale, in cui è evidenziato escalator-functions-group e l'app per le funzioni creata.](../images/6-access-function-app.png)</span><span class="sxs-lookup"><span data-stu-id="3a57a-115">![Screenshot of All Resources in the portal highlighting the escalator-functions-group and the function app we created.](../images/6-access-function-app.png)</span></span>
1. <span data-ttu-id="3a57a-116">Nel menu a sinistra vengono visualizzati il nome dell'app per le funzioni e un sottomenu contenente tre elementi: *Funzioni*, *Proxy* e *Slot*.</span><span class="sxs-lookup"><span data-stu-id="3a57a-116">The left-side menu displays your function app name and a submenu with three items - *Functions*, *Proxies*, and *Slots*.</span></span>  <span data-ttu-id="3a57a-117">Per creare la prima funzione, spostare il mouse su **Funzioni** nell'albero di spostamento e fare clic sul pulsante **+** visualizzato.</span><span class="sxs-lookup"><span data-stu-id="3a57a-117">To start creating our first function, move your mouse over **Functions** in the navigation tree and click  the **+** button that appears.</span></span>
  <span data-ttu-id="3a57a-118">![Screenshot del segno di addizione visualizzato al passaggio del mouse sulla voce di menu Funzioni che, se selezionata, avvia la procedura di creazione della funzione.](../images/5-function-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="3a57a-118">![Screenshot showing plus sign when you hover over the Functions menu item that, when clicked, starts the function creation procedure.](../images/5-function-add-button.png)</span></span>
1. <span data-ttu-id="3a57a-119">Nella schermata Avvio rapido selezionare il collegamento **Funzione personalizzata** nella sezione **Iniziare da zero**, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="3a57a-119">In the Quickstart screen, select the **Custom function** link in the **Get started on your own** section as shown in the following screenshot.</span></span>
  <span data-ttu-id="3a57a-120">![Screenshot della schermata Avvio rapido con il pulsante "Funzione personalizzata" evidenziato.](../images/6-custom-function.png)</span><span class="sxs-lookup"><span data-stu-id="3a57a-120">![Screenshot of Quickstart screen with the "Custom function" button highlighted.](../images/6-custom-function.png)</span></span>
1. <span data-ttu-id="3a57a-121">Dall'elenco dei modelli visualizzati nella schermata, selezionare l'implementazione JavaScript del modello di trigger HTTP, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="3a57a-121">From the list of templates displayed on the screen, select the JavaScript implementation of the HTTP trigger template as shown in the following screenshot.</span></span>
  <span data-ttu-id="3a57a-122">![Screenshot dell'elenco di modelli, con il trigger HTTP e l'opzione JavaScript evidenziati.](../images/6-httptrigger-template.png)</span><span class="sxs-lookup"><span data-stu-id="3a57a-122">![Screenshot of template list, with the HTTP trigger and JavaScript option highlighted.](../images/6-httptrigger-template.png)</span></span>
1.  <span data-ttu-id="3a57a-123">Immettere **DriveGearTemperatureService** nel campo relativo al nome all'interno della finestra di dialogo **Nuova funzione** visualizzata.</span><span class="sxs-lookup"><span data-stu-id="3a57a-123">Enter **DriveGearTemperatureService** in the name field of the **New Function** dialog that appears.</span></span> <span data-ttu-id="3a57a-124">Dopo avere assegnato un nome alla funzione, premere il pulsante **Crea**, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="3a57a-124">After you have named your function, press the **Create** button, as illustrated in te following screenshot.</span></span>
  <span data-ttu-id="3a57a-125">![Screenshot del modulo Nuova funzione, con il campo Nome evidenziato e il valore "DriveGearTemperatureService"](../images/6-create-httptrigger-form.png) impostato</span><span class="sxs-lookup"><span data-stu-id="3a57a-125">![Screenshot of New Function form, showing Name field highlighted and set with the value "DriveGearTemperatureService"](../images/6-create-httptrigger-form.png)</span></span>
1. <span data-ttu-id="3a57a-126">Quando la creazione della funzione viene completata, si aprirà l'editor di codice con il contenuto del file di codice *index. js*.</span><span class="sxs-lookup"><span data-stu-id="3a57a-126">When your function creation completes, the code editor opens with the contents of the *index.js* code file.</span></span> <span data-ttu-id="3a57a-127">Nel frammento di codice seguente viene elencato il codice predefinito che il modello ha generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="3a57a-127">The default code that the template generated for us is listed in the following snippet.</span></span>

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

<span data-ttu-id="3a57a-128">La funzione prevede il passaggio di un nome tramite la stringa della query di richiesta HTTP o come parte del corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="3a57a-128">Our function expects a name to be passed in either through the HTTP request query string or as part of the request body.</span></span> <span data-ttu-id="3a57a-129">La funzione risponde restituendo il messaggio **Salve, {nome}** e ripetendo il nome che è stato inviato nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="3a57a-129">The function responds by returning the message  **Hello, {name}**, echoing back the name that was sent in the request.</span></span>

1. <span data-ttu-id="3a57a-130">Sul lato destro della visualizzazione del codice sorgente saranno presenti due schede.</span><span class="sxs-lookup"><span data-stu-id="3a57a-130">On the right-hand side of the source view, you'll see two tabs.</span></span> <span data-ttu-id="3a57a-131">Nel file di visualizzazione \*\* vengono elencati il codice e il file di configurazione per la funzione.</span><span class="sxs-lookup"><span data-stu-id="3a57a-131">The \*\*View file lists the code and config file for your function.</span></span>  <span data-ttu-id="3a57a-132">Selezionare **function.json** per visualizzare la configurazione della funzione.</span><span class="sxs-lookup"><span data-stu-id="3a57a-132">Select **function.json** to view the configuration of the function.</span></span> <span data-ttu-id="3a57a-133">Il frammento di codice seguente illustra il contenuto del file **input.json**.</span><span class="sxs-lookup"><span data-stu-id="3a57a-133">The following snippet shows the contents of the **function.json** file.</span></span> <span data-ttu-id="3a57a-134">Con questa configurazione si dichiara che la funzione viene eseguita quando riceve una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a57a-134">This configuration declares that the function is runs when it receives an HTTP request.</span></span> <span data-ttu-id="3a57a-135">L'associazione di output dichiara che la risposta verrà inviata come risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a57a-135">The output binding declares that the response will be sent as an HTTP response.</span></span>

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
> [!TIP]
> <span data-ttu-id="3a57a-136">Nel file JSON precedente si noti che il trigger viene definito in una matrice di associazioni.</span><span class="sxs-lookup"><span data-stu-id="3a57a-136">Notice in the preceding JSON file that the trigger is defined in an array of bindings.</span></span> <span data-ttu-id="3a57a-137">Un trigger è quindi effettivamente un tipo speciale di associazione.</span><span class="sxs-lookup"><span data-stu-id="3a57a-137">So, a trigger is actually a special type of binding.</span></span>

## <a name="run-our-function"></a><span data-ttu-id="3a57a-138">Eseguire la funzione</span><span class="sxs-lookup"><span data-stu-id="3a57a-138">Run our function</span></span>

> [!TIP]
> <span data-ttu-id="3a57a-139">**cURL** è uno strumento da riga di comando che può essere usato per inviare o ricevere file.</span><span class="sxs-lookup"><span data-stu-id="3a57a-139">**cURL** is a command line tool that can be used to send or receive files.</span></span> <span data-ttu-id="3a57a-140">cURL.exe supporta vari protocolli, come HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3 e così via. Per altre informazioni, fare riferimento ai collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="3a57a-140">cURL.exe supports numerous protocols like HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3 etc. For more information please refer the below links:</span></span>
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

<span data-ttu-id="3a57a-141">Per testare la funzione, è possibile inviare una richiesta HTTP all'URL della funzione usando cURL dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="3a57a-141">To test our function, you can send an HTTP request to the function URL using cURL on the command line.</span></span> <span data-ttu-id="3a57a-142">Per trovare l'URL dell'endpoint della funzione, tornare al codice della funzione e selezionare il collegamento **Recupera URL della funzione**, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="3a57a-142">To find the endpoint URL of the function, return to your function code and select the **Get function URL** link, as shown in the following screenshot.</span></span> <span data-ttu-id="3a57a-143">Copiare il collegamento negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="3a57a-143">Copy this link to your clipboard.</span></span>  

 ![Schermata dell'editor di codice nella sezione App per le funzioni del portale.](../images/6-get-function-url.png)

<span data-ttu-id="3a57a-146">Usando questo URL della funzione, eseguire il comando cURL seguente dalla riga di comando e sostituire l'URL con quello personalizzato.</span><span class="sxs-lookup"><span data-stu-id="3a57a-146">Using this function URL, run the following cURL command from your command line, replacing the URL with your own.</span></span>

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

<span data-ttu-id="3a57a-147">Lo screenshot seguente illustra un esempio della risposta ricevuta dall'implementazione predefinita nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="3a57a-147">The following screenshot shows an example of the response you receive from the default implementation in the command line.</span></span>

![Screenshot di un esempio di comando cUrl e di una risposta dalla riga di comando.](../images/6-premadefunction-curl.png)

## <a name="add-business-logic-to-the-function"></a><span data-ttu-id="3a57a-149">Aggiungere la logica di business alla funzione</span><span class="sxs-lookup"><span data-stu-id="3a57a-149">Add business logic to the function</span></span>

<span data-ttu-id="3a57a-150">È ora possibile aggiungere la logica alla funzione che controlla le letture delle temperature ricevute e imposta ogni singolo stato.</span><span class="sxs-lookup"><span data-stu-id="3a57a-150">It's time to add the logic to the function that checks temperature readings that it receives and sets a status for each.</span></span>

<span data-ttu-id="3a57a-151">La funzione prevede una matrice di letture della temperatura.</span><span class="sxs-lookup"><span data-stu-id="3a57a-151">Our function is expecting an array of temperature readings.</span></span> <span data-ttu-id="3a57a-152">Il frammento JSON seguente costituisce un esempio di corpo della richiesta che sarà inviata alla funzione.</span><span class="sxs-lookup"><span data-stu-id="3a57a-152">The following JSON snippet is an example of the request body that we'll send to our function.</span></span> <span data-ttu-id="3a57a-153">Ogni elemento `reading` possiede un ID, un timestamp e una temperatura.</span><span class="sxs-lookup"><span data-stu-id="3a57a-153">Each `reading` entry has an ID, timestamp, and temperature.</span></span>

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

<span data-ttu-id="3a57a-154">Il codice predefinito della funzione sarà poi sostituito con il codice seguente, che implementa la logica di business.</span><span class="sxs-lookup"><span data-stu-id="3a57a-154">Next, we'll replace the default code in our function with the following code that implements our business logic.</span></span> 

1. <span data-ttu-id="3a57a-155">Aprire il file **index.js** e sostituirlo con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="3a57a-155">Open the **index.js** file, and replace it with the following code.</span></span>

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
<span data-ttu-id="3a57a-156">La logica che è stata aggiunta è semplice.</span><span class="sxs-lookup"><span data-stu-id="3a57a-156">The logic we added is straightforward.</span></span> <span data-ttu-id="3a57a-157">Vengono eseguiti l'iterazione della matrice delle letture e il controllo del campo relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="3a57a-157">We iterate over the array of readings and check the temperature field.</span></span> <span data-ttu-id="3a57a-158">A seconda del valore di tale campo, lo stato viene impostato su **OK**, **CAUTION** o **DANGER**.</span><span class="sxs-lookup"><span data-stu-id="3a57a-158">Depending on the value of that field, we set a status of **OK**, **CAUTION**, or **DANGER**.</span></span> <span data-ttu-id="3a57a-159">La matrice delle letture viene quindi restituita dopo aver aggiunto il campo dello stato per ogni voce.</span><span class="sxs-lookup"><span data-stu-id="3a57a-159">We then send back the array of readings with a status field added to each entry.</span></span>

<span data-ttu-id="3a57a-160">Si notino le istruzioni di log.</span><span class="sxs-lookup"><span data-stu-id="3a57a-160">Notice the log statements.</span></span> <span data-ttu-id="3a57a-161">Durante l'esecuzione della funzione, le istruzioni aggiungeranno messaggi nella finestra di log.</span><span class="sxs-lookup"><span data-stu-id="3a57a-161">When the function runs, these statements will add messages in the log window.</span></span>

## <a name="test-our-business-logic"></a><span data-ttu-id="3a57a-162">Testare la logica di business</span><span class="sxs-lookup"><span data-stu-id="3a57a-162">Test our business logic</span></span>

<span data-ttu-id="3a57a-163">In questo caso si userà il riquadro **Test** nel portale per testare la funzione.</span><span class="sxs-lookup"><span data-stu-id="3a57a-163">In this case, we're going to use the **Test** pane in the portal to test our function.</span></span>

1. <span data-ttu-id="3a57a-164">Aprire la finestra **Test** dal menu a comparsa a destra.</span><span class="sxs-lookup"><span data-stu-id="3a57a-164">Open the **Test** window from the right-hand side flyout menu.</span></span>
1. <span data-ttu-id="3a57a-165">Incollare la richiesta di esempio dall'esempio precedente nella casella di testo del corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="3a57a-165">Paste the sample request from above into the request body text box.</span></span> 
1. <span data-ttu-id="3a57a-166">Selezionare **Esegui** e visualizzare la risposta nel riquadro di output.</span><span class="sxs-lookup"><span data-stu-id="3a57a-166">Select **Run** and view the response in the output pane.</span></span> <span data-ttu-id="3a57a-167">Per visualizzare i messaggi di log, aprire la scheda **Log** nel riquadro a comparsa nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="3a57a-167">To see log messages, open the **Logs** tab in the bottom flyout of the page.</span></span> <span data-ttu-id="3a57a-168">Lo screenshot seguente illustra un esempio di risposta nel riquadro di output e i messaggi nel riquadro **Log**.</span><span class="sxs-lookup"><span data-stu-id="3a57a-168">The following screenshot shows an example response in the output pane and messages in the  **Logs** pane.</span></span>

![Screenshot delle schede "Test" e "Log" nell'interfaccia delle funzioni all'interno del portale.](../images/6-portal-testing.png)

<span data-ttu-id="3a57a-171">Nel riquadro di output è possibile osservare che il campo relativo allo stato è stato aggiunto correttamente per ogni lettura.</span><span class="sxs-lookup"><span data-stu-id="3a57a-171">You can see in the output pane that our status field has been correctly added to each of the readings.</span></span>

<span data-ttu-id="3a57a-172">Passare al dashboard **Monitoraggio** per verificare se la richiesta è stata registrata in Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3a57a-172">If you navigate to the **Monitor** dashboard, you'll see that the request has been logged to Application Insights.</span></span>

![Screenshot della parte del dashboard "Monitoraggio" che illustra un messaggio di conferma dalla funzione.](../images/6-app-insights.png)

