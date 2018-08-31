In questo esercizio si proseguirà con l'esempio della trasmissione a ingranaggi aggiungendo la logica per il servizio relativo alla temperatura. In particolare, si riceveranno dati da una richiesta HTTP.

## <a name="function-requirements"></a>Requisiti della funzione
I requisiti definiti per la logica sono i seguenti:
- Le temperature tra 0 e 25 dovranno essere contrassegnate come **OK**
- Le temperature tra 26 e 50 dovranno essere contrassegnate come **CAUTION**
- Le temperature oltre 50 dovranno essere contrassegnate come **DANGER**

## <a name="adding-a-function-to-an-azure-function-app"></a>Aggiunta di una funzione a un'app per le funzioni di Azure

Come è già stato illustrato, Azure offre un valido aiuto per imparare a usare Funzioni di Azure. Una funzionalità utile per acquisire familiarità con le funzioni è la possibilità di generarne una usando uno dei numerosi modelli. In questo esercizio si userà il modello HttpTrigger per implementare il servizio relativo alla temperatura.

1. Accedere al [portale di Azure](https://portal.azure.com) con il proprio account Azure.
1. Accedere al gruppo di risorse creato nel primo esercizio scegliendo **Tutte le risorse** nel menu a sinistra e quindi selezionando **escalator-functions-group**.
1. Verranno quindi visualizzate le risorse del gruppo. Accedere all'app per le funzioni creata nell'esercizio precedente selezionando l'elemento **escalator-functions-xxxxxxx**, contrassegnato anche con l'icona a forma di fulmine.
  ![Accedere all'app per le funzioni](../images/6-access-function-app.png)
1. Nel pannello verrà visualizzata una panoramica dell'app per le funzioni. Sulla sinistra sarà anche presente un albero di spostamento contenente tutte le funzioni e tutti i proxy e gli slot definiti. Non si ha ancora una funzione, quindi questa sezione sarà vuota. Per creare una funzione, spostare il mouse su **Funzioni** nell'albero di spostamento e fare clic sul pulsante **+** visualizzato.
  ![Aggiungere una funzione nell'albero di spostamento](../images/5-function-add-button.png)
1. Nel modulo per iniziare rapidamente con una funzione preconfezionata selezionare il collegamento **Funzione personalizzata** nella sezione **Iniziare da zero**.
  ![Modulo per funzioni preconfezionate](../images/6-custom-function.png)
1. Verrà visualizzato un elenco di modelli. Selezionare l'implementazione JavaScript del modello di trigger HTTP.
  ![Modello di trigger HTTP](../images/6-httptrigger-template.png)
1. Verrà visualizzato un pannello che consente di definire ciò che verrà generato dal modello. In questo caso si è interessati a generare una funzione JavaScript denominata **DriveGearTemperatureService**. Dopo aver assegnato il nome alla funzione, fare clic sul pulsante **Crea**.
  ![Modulo per la creazione di un trigger HTTP](../images/6-create-httptrigger-form.png)
1. Dopo alcuni istanti verrà visualizzato il codice sorgente basato sul modello per la funzione. La funzione preconfezionata prevede che venga passato un nome e restituirà **Hello, {nome}**.

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

1. Sul lato destro della visualizzazione del codice sorgente saranno presenti due schede. Nella scheda **Visualizza file** vengono visualizzati tutti i file che supportano la funzione. È possibile selezionare **function.json** per visualizzare la configurazione della funzione. In questo caso vengono visualizzate la definizione di httpTrigger e l'associazione di output. La configurazione indica che la funzione viene avviata da una richiesta HTTP, mentre l'associazione di output indica che la risposta verrà inviata come risposta HTTP.

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

## <a name="running-the-premade-azure-function"></a>Esecuzione della funzione di Azure preconfezionata

Per eseguire la funzione, è possibile avviare una richiesta HTTP da cURL dal prompt dei comandi. Per trovare l'URL dell'endpoint, tornare al codice della funzione e selezionare il collegamento **Recupera URL della funzione**. Copiare il collegamento negli Appunti.  L'URL dovrebbe essere simile al seguente: https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg== ![Recuperare l'URL dell'endpoint](../images/6-get-function-url.png)

Con tale URL, eseguire un comando cURL per avviare la funzione sostituendo l'URL con il proprio:

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

![Risposta cURL per la funzione preconfezionata](../images/6-premadefunction-curl.png)

## <a name="adding-business-logic-to-the-function"></a>Aggiunta di logica di business alla funzione

La funzione prevede una matrice di letture della temperatura provenienti dal cliente. Questo è un esempio del corpo della richiesta:

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

Modificare il codice della funzione preconfezionata in modo da implementare la logica di business necessaria. Aprire il file index.js e sostituirlo con il listato seguente:

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

Si notino le istruzioni di log. Durante l'esecuzione della funzione aggiungeranno messaggi nella finestra di log.

## <a name="testing-your-business-logic"></a>Test della logica di business

Aprire la finestra di test dal menu a comparsa sul lato destro e incollare la richiesta di esempio riportata sopra nella casella di testo del corpo della richiesta. Fare clic sul pulsante **Esegui** e visualizzare l'output. È anche possibile aprire la finestra di log dal menu a comparsa in basso per visualizzare le istruzioni di registrazione nel log.
![Test della logica di business](../images/6-portal-testing.png) Nei dati JSON nella finestra di output è possibile osservare che il campo dello stato della temperatura è stato aggiunto correttamente a ogni lettura. È anche possibile accedere al dashboard di monitoraggio per verificare che la richiesta sia stata registrata in Application Insights.
![Richiesta in Application Insights](../images/6-app-insights.png)
