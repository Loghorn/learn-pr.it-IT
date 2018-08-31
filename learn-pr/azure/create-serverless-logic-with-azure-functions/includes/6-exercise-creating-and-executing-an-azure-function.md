<span data-ttu-id="be466-101">In questo esercizio si proseguirà con l'esempio della trasmissione a ingranaggi aggiungendo la logica per il servizio relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="be466-101">In this exercise, we'll continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="be466-102">In particolare, si riceveranno dati da una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="be466-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="be466-103">Requisiti della funzione</span><span class="sxs-lookup"><span data-stu-id="be466-103">Function requirements</span></span>
<span data-ttu-id="be466-104">I requisiti definiti per la logica sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="be466-104">Let's define some requirements for our logic:</span></span>
- <span data-ttu-id="be466-105">Le temperature tra 0 e 25 dovranno essere contrassegnate come **OK**</span><span class="sxs-lookup"><span data-stu-id="be466-105">temperatures between 0-25 should be flagged as **OK**</span></span>
- <span data-ttu-id="be466-106">Le temperature tra 26 e 50 dovranno essere contrassegnate come **CAUTION**</span><span class="sxs-lookup"><span data-stu-id="be466-106">temperatures between 26-50 should be flagged as **CAUTION**</span></span>
- <span data-ttu-id="be466-107">Le temperature oltre 50 dovranno essere contrassegnate come **DANGER**</span><span class="sxs-lookup"><span data-stu-id="be466-107">temperatures above 50 should be flagged as **DANGER**</span></span>

## <a name="adding-a-function-to-an-azure-function-app"></a><span data-ttu-id="be466-108">Aggiunta di una funzione a un'app per le funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="be466-108">Adding a function to an Azure function app</span></span>

<span data-ttu-id="be466-109">Come è già stato illustrato, Azure offre un valido aiuto per imparare a usare Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="be466-109">As we have already learned, Azure provides a helping hand when you're learning how to work with Azure Functions.</span></span> <span data-ttu-id="be466-110">Una funzionalità utile per acquisire familiarità con le funzioni è la possibilità di generarne una usando uno dei numerosi modelli.</span><span class="sxs-lookup"><span data-stu-id="be466-110">One great feature to get your feet wet with Functions is to generate one using one of many templates.</span></span> <span data-ttu-id="be466-111">In questo esercizio si userà il modello HttpTrigger per implementare il servizio relativo alla temperatura.</span><span class="sxs-lookup"><span data-stu-id="be466-111">In this exercise, you will be using the HttpTrigger template to implement the temperature service.</span></span>

1. <span data-ttu-id="be466-112">Accedere al [portale di Azure](https://portal.azure.com) con il proprio account Azure.</span><span class="sxs-lookup"><span data-stu-id="be466-112">Sign in to the [Azure portal](https://portal.azure.com) using your Azure account.</span></span>
1. <span data-ttu-id="be466-113">Accedere al gruppo di risorse creato nel primo esercizio scegliendo **Tutte le risorse** nel menu a sinistra e quindi selezionando **escalator-functions-group**.</span><span class="sxs-lookup"><span data-stu-id="be466-113">Access the resource group you created in the first exercise by choosing **All resources** in the left-hand menu, and then selecting **escalator-functions-group**.</span></span>
1. <span data-ttu-id="be466-114">Verranno quindi visualizzate le risorse del gruppo.</span><span class="sxs-lookup"><span data-stu-id="be466-114">The resources for the group will then be displayed.</span></span> <span data-ttu-id="be466-115">Accedere all'app per le funzioni creata nell'esercizio precedente selezionando l'elemento **escalator-functions-xxxxxxx**, contrassegnato anche con l'icona a forma di fulmine.</span><span class="sxs-lookup"><span data-stu-id="be466-115">Access the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt icon).</span></span>
  <span data-ttu-id="be466-116">![Accedere all'app per le funzioni](../images/6-access-function-app.png)</span><span class="sxs-lookup"><span data-stu-id="be466-116">![Access the function app](../images/6-access-function-app.png)</span></span>
1. <span data-ttu-id="be466-117">Nel pannello verrà visualizzata una panoramica dell'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="be466-117">The blade will show an overview of your function app.</span></span> <span data-ttu-id="be466-118">Sulla sinistra sarà anche presente un albero di spostamento contenente tutte le funzioni e tutti i proxy e gli slot definiti.</span><span class="sxs-lookup"><span data-stu-id="be466-118">There is also a navigation tree on the left that shows any functions, proxies or slots that are defined.</span></span> <span data-ttu-id="be466-119">Non si ha ancora una funzione, quindi questa sezione sarà vuota.</span><span class="sxs-lookup"><span data-stu-id="be466-119">We don't have a function yet, so this  will be empty.</span></span> <span data-ttu-id="be466-120">Per creare una funzione, spostare il mouse su **Funzioni** nell'albero di spostamento e fare clic sul pulsante **+** visualizzato.</span><span class="sxs-lookup"><span data-stu-id="be466-120">To create a function, move your mouse over **Function** in the navigation tree and click  the **+** button that appears.</span></span>
  <span data-ttu-id="be466-121">![Aggiungere una funzione nell'albero di spostamento](../images/5-function-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="be466-121">![Add function navigation](../images/5-function-add-button.png)</span></span>
1. <span data-ttu-id="be466-122">Nel modulo per iniziare rapidamente con una funzione preconfezionata selezionare il collegamento **Funzione personalizzata** nella sezione **Iniziare da zero**.</span><span class="sxs-lookup"><span data-stu-id="be466-122">In the quickstart premade function form, select the **Custom function** link in the **Get started on your own** section.</span></span>
  <span data-ttu-id="be466-123">![Modulo per funzioni preconfezionate](../images/6-custom-function.png)</span><span class="sxs-lookup"><span data-stu-id="be466-123">![Premade function form](../images/6-custom-function.png)</span></span>
1. <span data-ttu-id="be466-124">Verrà visualizzato un elenco di modelli.</span><span class="sxs-lookup"><span data-stu-id="be466-124">You are now presented with a list of templates.</span></span> <span data-ttu-id="be466-125">Selezionare l'implementazione JavaScript del modello di trigger HTTP.</span><span class="sxs-lookup"><span data-stu-id="be466-125">Select the JavaScript implementation of the HTTP trigger template.</span></span>
  <span data-ttu-id="be466-126">![Modello di trigger HTTP](../images/6-httptrigger-template.png)</span><span class="sxs-lookup"><span data-stu-id="be466-126">![HTTP trigger template](../images/6-httptrigger-template.png)</span></span>
1. <span data-ttu-id="be466-127">Verrà visualizzato un pannello che consente di definire ciò che verrà generato dal modello.</span><span class="sxs-lookup"><span data-stu-id="be466-127">A blade will appear, allowing you to define what the template will generate.</span></span> <span data-ttu-id="be466-128">In questo caso si è interessati a generare una funzione JavaScript denominata **DriveGearTemperatureService**.</span><span class="sxs-lookup"><span data-stu-id="be466-128">In this case, you are interested in generating a JavaScript function named **DriveGearTemperatureService**.</span></span> <span data-ttu-id="be466-129">Dopo aver assegnato il nome alla funzione, fare clic sul pulsante **Crea**.</span><span class="sxs-lookup"><span data-stu-id="be466-129">After you have named your function, press the **Create** button.</span></span>
  <span data-ttu-id="be466-130">![Modulo per la creazione di un trigger HTTP](../images/6-create-httptrigger-form.png)</span><span class="sxs-lookup"><span data-stu-id="be466-130">![Create HTTP trigger form](../images/6-create-httptrigger-form.png)</span></span>
1. <span data-ttu-id="be466-131">Dopo alcuni istanti verrà visualizzato il codice sorgente basato sul modello per la funzione.</span><span class="sxs-lookup"><span data-stu-id="be466-131">After a few moments, you will be presented with the templated source code for your function.</span></span> <span data-ttu-id="be466-132">La funzione preconfezionata prevede che venga passato un nome e restituirà **Hello, {nome}**.</span><span class="sxs-lookup"><span data-stu-id="be466-132">The premade function expects a name to be passed in, and it will return **Hello, {name}**.</span></span>

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

1. <span data-ttu-id="be466-133">Sul lato destro della visualizzazione del codice sorgente saranno presenti due schede.</span><span class="sxs-lookup"><span data-stu-id="be466-133">On the right-hand side of the source view, you will see two tabs.</span></span> <span data-ttu-id="be466-134">Nella scheda **Visualizza file** vengono visualizzati tutti i file che supportano la funzione. È possibile selezionare **function.json** per visualizzare la configurazione della funzione.</span><span class="sxs-lookup"><span data-stu-id="be466-134">You are able to view all the files supporting the function in the **View files** tab. You can select **function.json** to view the configuration of the function.</span></span> <span data-ttu-id="be466-135">In questo caso vengono visualizzate la definizione di httpTrigger e l'associazione di output.</span><span class="sxs-lookup"><span data-stu-id="be466-135">Here you can see the httpTrigger defined, as well as the output binding.</span></span> <span data-ttu-id="be466-136">La configurazione indica che la funzione viene avviata da una richiesta HTTP, mentre l'associazione di output indica che la risposta verrà inviata come risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="be466-136">This configuration describes that the function is initiated by an HTTP request, and the output binding describes that the response will be sent as an HTTP response.</span></span>

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

## <a name="running-the-premade-azure-function"></a><span data-ttu-id="be466-137">Esecuzione della funzione di Azure preconfezionata</span><span class="sxs-lookup"><span data-stu-id="be466-137">Running the premade Azure function</span></span>

<span data-ttu-id="be466-138">Per eseguire la funzione, è possibile avviare una richiesta HTTP da cURL dal prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="be466-138">To run the function, you can initiate an HTTP request from cURL from a command prompt.</span></span> <span data-ttu-id="be466-139">Per trovare l'URL dell'endpoint, tornare al codice della funzione e selezionare il collegamento **Recupera URL della funzione**.</span><span class="sxs-lookup"><span data-stu-id="be466-139">To find the endpoint URL, return to your function code and select the **Get function URL** link.</span></span> <span data-ttu-id="be466-140">Copiare il collegamento negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="be466-140">Copy this link to your clipboard.</span></span>  <span data-ttu-id="be466-141">L'URL dovrebbe essere simile al seguente: https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg== ![Recuperare l'URL dell'endpoint](../images/6-get-function-url.png)</span><span class="sxs-lookup"><span data-stu-id="be466-141">Your URL should look something similar to the following: https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg== ![Get endpoint URL](../images/6-get-function-url.png)</span></span>

<span data-ttu-id="be466-142">Con tale URL, eseguire un comando cURL per avviare la funzione sostituendo l'URL con il proprio:</span><span class="sxs-lookup"><span data-stu-id="be466-142">With that URL, run a cURL command to initiate your function (replacing the URL with your own):</span></span>

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

![Risposta cURL per la funzione preconfezionata](../images/6-premadefunction-curl.png)

## <a name="adding-business-logic-to-the-function"></a><span data-ttu-id="be466-144">Aggiunta di logica di business alla funzione</span><span class="sxs-lookup"><span data-stu-id="be466-144">Adding business logic to the function</span></span>

<span data-ttu-id="be466-145">La funzione prevede una matrice di letture della temperatura provenienti dal cliente.</span><span class="sxs-lookup"><span data-stu-id="be466-145">Our function is expecting an array of temperature readings from the customer.</span></span> <span data-ttu-id="be466-146">Questo è un esempio del corpo della richiesta:</span><span class="sxs-lookup"><span data-stu-id="be466-146">This is an example of the request body:</span></span>

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

<span data-ttu-id="be466-147">Modificare il codice della funzione preconfezionata in modo da implementare la logica di business necessaria.</span><span class="sxs-lookup"><span data-stu-id="be466-147">Modify the premade function code to implement the required business logic.</span></span> <span data-ttu-id="be466-148">Aprire il file index.js e sostituirlo con il listato seguente:</span><span class="sxs-lookup"><span data-stu-id="be466-148">Open the index.js file, and replace it with the following listing:</span></span>

```javascript
module.exports = function (context, req) {
    context.log('Drive Gear Temperature Service triggered');
    if (req.body && req.body.readings) {
        for(var i=0; i<req.body.readings.length; i++){
            var reading = req.body.readings[i];
            if(reading.temperature<=25){
                context.log('Reading is OK');
                reading.status = 'OK';
                continue;
            }
            if(reading.temperature<=50){
                context.log('Reading is CAUTION');
                reading.status = 'CAUTION';
                continue;
            }
            context.log('Reading is DANGER');
            reading.status = 'DANGER'
        }
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

<span data-ttu-id="be466-149">Si notino le istruzioni di log.</span><span class="sxs-lookup"><span data-stu-id="be466-149">Notice the log statements.</span></span> <span data-ttu-id="be466-150">Durante l'esecuzione della funzione aggiungeranno messaggi nella finestra di log.</span><span class="sxs-lookup"><span data-stu-id="be466-150">When the function runs, they will add messages in the log window.</span></span>

## <a name="testing-your-business-logic"></a><span data-ttu-id="be466-151">Test della logica di business</span><span class="sxs-lookup"><span data-stu-id="be466-151">Testing your business logic</span></span>

<span data-ttu-id="be466-152">Aprire la finestra di test dal menu a comparsa sul lato destro e incollare la richiesta di esempio riportata sopra nella casella di testo del corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="be466-152">Open the test window from the right-hand side flyout menu, and paste the sample request from above into the request body text box.</span></span> <span data-ttu-id="be466-153">Fare clic sul pulsante **Esegui** e visualizzare l'output.</span><span class="sxs-lookup"><span data-stu-id="be466-153">Press the **Run** button and view the output.</span></span> <span data-ttu-id="be466-154">È anche possibile aprire la finestra di log dal menu a comparsa in basso per visualizzare le istruzioni di registrazione nel log.</span><span class="sxs-lookup"><span data-stu-id="be466-154">You can also open the log window from the bottom flyout menu to see the logging statements in the log.</span></span>
<span data-ttu-id="be466-155">![Test della logica di business](../images/6-portal-testing.png) Nei dati JSON nella finestra di output è possibile osservare che il campo dello stato della temperatura è stato aggiunto correttamente a ogni lettura.</span><span class="sxs-lookup"><span data-stu-id="be466-155">![Testing the business Logic](../images/6-portal-testing.png) You can see from the JSON data in the output window that our temperature status field has been added to each of the readings correctly.</span></span> <span data-ttu-id="be466-156">È anche possibile accedere al dashboard di monitoraggio per verificare che la richiesta sia stata registrata in Application Insights.</span><span class="sxs-lookup"><span data-stu-id="be466-156">You may also visit the Monitor dashboard to see that the request has been logged to Application Insights.</span></span>
<span data-ttu-id="be466-157">![Richiesta in Application Insights](../images/6-app-insights.png)</span><span class="sxs-lookup"><span data-stu-id="be466-157">![Request in Application Insights](../images/6-app-insights.png)</span></span>
