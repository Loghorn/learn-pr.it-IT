Qui di seguito si proseguirà con l'esempio della trasmissione a ingranaggi aggiungendo la logica per il servizio relativo alla temperatura. In particolare, si riceveranno dati da una richiesta HTTP.

## <a name="function-requirements"></a>Requisiti della funzione

Innanzitutto, definire i requisiti per la logica:

- Le temperature tra 0 e 25 dovranno essere contrassegnate come **OK**.
- Le temperature tra 26 e 50 dovranno essere contrassegnate come **CAUTION**.
- Le temperature oltre 50 dovranno essere contrassegnate come **DANGER**.

## <a name="add-a-function-to-our-function-app"></a>Aggiungere una funzione a un'app per le funzioni

Come descritto nell'unità precedente, Azure offre modelli che consentono di creare funzioni. In questa unità si userà il modello `HttpTrigger` per implementare il servizio relativo alla temperatura.

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).

1. Selezionare il gruppo di risorse creato nel primo esercizio scegliendo **Tutte le risorse** dal menu a sinistra e selezionando "**<rgn>[Nome gruppo di risorse sandbox]</rgn>**".

1. Verranno quindi visualizzate le risorse del gruppo. Fare clic sul nome dell'app per le funzioni creata nell'esercizio precedente selezionando l'elemento **escalator-functions-xxxxxxx**, contrassegnato anche con l'icona Funzione a forma di fulmine.

    ![Screenshot del portale di Azure che visualizza il pannello Tutte le risorse evidenziato e l'app per le funzioni escalator che è stata creata.](../media/5-access-function-app.png)

<!-- Start temporary fix for issue #2498. -->
> [!IMPORTANT]
> Gli esercizi in questo modulo attualmente funzionano con Funzioni di Azure V1. Seguire questa procedura con attenzione per assicurarsi che l'app per le funzioni usi la versione del runtime V1. 

1. Selezionare l'app per le funzioni nell'elenco **App per le funzioni**.
1. Selezionare **Funzionalità della piattaforma**.
1. Nella schermata **Funzionalità della piattaforma** selezionare **Impostazioni dell'app per le funzioni** in **Impostazioni generali**.
1. Selezionare *~1* in **Versione runtime**.
1. Chiudere **Impostazioni dell'app per le funzioni**.

L'app per le funzioni è ora configurata per usare il runtime V1 di Funzioni di Azure. È ora possibile continuare a creare la prima funzione.
<!-- End temporary fix for issue #2498. --> 

1. Nel menu a sinistra vengono visualizzati il nome dell'app per le funzioni e un sottomenu contenente tre elementi: *Funzioni*, *Proxy* e *Slot*.  

1. Per iniziare a creare la prima funzione, selezionare **Funzioni** e fare clic su **Nuova funzione** nella parte superiore della pagina visualizzata.

    ![Screenshot del portale di Azure che visualizza l'elenco Funzioni dell'app per le funzioni, con la voce di menu Funzioni e il pulsante Nuova funzione evidenziato.](../media/5-function-add-button.png)

1. Nella schermata Avvio rapido selezionare il collegamento **Funzione personalizzata** nella sezione **Iniziare da zero**, come illustrato nello screenshot seguente. Se non viene visualizzata la schermata di avvio rapido, fare clic sul collegamento **passare all'Avvio rapido** nella parte superiore della pagina.

    ![Screenshot del portale di Azure che visualizza il pannello Avvio rapido con il pulsante Funzione personalizzata evidenziato nella sezione Iniziare da zero.](../media/5-custom-function.png)

1. Dall'elenco dei modelli visualizzati nella schermata, selezionare il modello **Trigger HTTP**, come illustrato nello screenshot seguente.

1. Immettere **DriveGearTemperatureService** nel campo relativo al nome all'interno della finestra di dialogo **Nuova funzione** visualizzata. Lasciare il livello Autorizzazione impostato su "Funzione" e premere il pulsante **Crea** per creare la funzione.

1. Quando la creazione della funzione viene completata si apre l'editor di codice con il contenuto del file di codice *index.js*. Nel frammento di codice seguente viene elencato il codice predefinito che il modello ha generato automaticamente.

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

    La funzione prevede il passaggio di un nome tramite la stringa della query di richiesta HTTP o come parte del corpo della richiesta. La funzione risponde restituendo il messaggio **Salve, {nome}** e ripetendo il nome che è stato inviato nella richiesta.

    Sul lato destro della visualizzazione del codice sorgente sono presenti due schede. La scheda **Visualizza file** elenca il codice e il file di configurazione per la funzione.  Selezionare **function.json** per visualizzare la configurazione della funzione, che è simile alla seguente:

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

    Con questa configurazione si dichiara che la funzione viene eseguita quando riceve una richiesta HTTP. L'associazione di output dichiara che la risposta verrà inviata come risposta HTTP.    

## <a name="test-the-function"></a>Testare la funzione

> [!TIP]
> **cURL** è uno strumento da riga di comando che può essere usato per inviare o ricevere file. È incluso in Linux, macOS e Windows 10 e può essere scaricato per la maggior parte degli altri sistemi operativi. cURL supporta vari protocolli come HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3 e così via. Per altre informazioni, consultare i collegamenti seguenti:
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

Per testare la funzione, è possibile inviare una richiesta HTTP all'URL della funzione usando cURL dalla riga di comando. Per trovare l'URL dell'endpoint della funzione, tornare al codice della funzione e selezionare il collegamento **Recupera URL della funzione** come illustrato nello screenshot seguente. Salvare temporaneamente questo collegamento.

![Screenshot del portale di Azure che visualizza l'editor delle funzioni con il pulsante Recupera URL della funzione evidenziato.](../media/5-get-function-url.png)

### <a name="securing-http-triggers"></a>Protezione dei trigger HTTP

I trigger HTTP consentono di usare le chiavi API per bloccare i chiamanti sconosciuti, richiedendo che la chiave sia presente in ogni richiesta. Quando si crea una funzione si seleziona il _livello di autorizzazione_. Per impostazione predefinita il livello è impostato su "Funzione", che richiede una chiave API specifica della funzione, ma è possibile impostarlo anche su "Amministratore" per usare una chiave "master" globale o su "Anonimo" per indicare che non è richiesta una chiave. Il livello di autorizzazione può essere modificato anche tramite le proprietà di funzione dopo la creazione.

Poiché è stato specificato "Funzione" durante la creazione di questa funzione, è necessario specificare la chiave quando si invia la richiesta HTTP. È possibile inviarla come parametro della stringa di query denominato `code`, oppure come un'intestazione HTTP (scelta consigliata) denominata `x-functions-key`.

Le chiavi di funzione e master sono disponibili nella sezione **Gestisci** quando si espande la funzione. Per impostazione predefinita sono nascoste ed è necessario visualizzarle.

1. Espandere la funzione e selezionare la sezione **Gestisci**, visualizzare la chiave di funzione predefinita e copiarla negli Appunti.

    ![Screenshot del portale di Azure che visualizza il pannello Gestisci con la chiave di funzione evidenziata.](../media/5-get-function-key.png)

1. Nella riga di comando in cui è stato installato lo strumento **cURL**, formattare un comando cURL con l'URL per la funzione e la chiave di funzione.

    - Usare una richiesta `POST`.
    - Aggiungere un valore dell'intestazione `Content-Type` del tipo `application/json`.
    - Assicurarsi di sostituire l'URL qui sotto con il proprio.
    - Passare la chiave di funzione come valore dell'intestazione `x-functions-key`.

    ```bash
    curl --header "Content-Type: application/json" --header "x-functions-key: <your-function-key>" --request POST --data "{\"name\": \"Azure Function\"}" https://<your-url-here>/api/DriveGearTemperatureService
    ```

La funzione risponderà con il testo `"Hello Azure Function"`.

> [!CAUTION]
> Se si usa Windows, eseguire il comando `cURL` dal prompt dei comandi. In PowerShell è presente un comando *curl*, ma è in realtà un alias di Invoke-WebRequest e pertanto non è identico a `cURL`.

> [!NOTE]
> È anche possibile eseguire il test dalla sezione di una singola funzione con la scheda **Test** a lato di una funzione selezionata. In questo caso non sarà però possibile verificare il funzionamento del sistema della chiave di funzione poiché non è richiesto in questa sede. Aggiungere l'intestazione appropriata e i valori dei parametri nell'interfaccia Test e fare clic sul pulsante **Esegui** per visualizzare l'output del test.

## <a name="add-business-logic-to-the-function"></a>Aggiungere la logica di business alla funzione

È ora possibile aggiungere la logica alla funzione che controlla le letture delle temperature ricevute e imposta ogni singolo stato.

La funzione prevede una matrice di letture della temperatura. Il frammento JSON seguente costituisce un esempio di corpo della richiesta che sarà inviata alla funzione. Ogni elemento `reading` possiede un ID, un timestamp e una temperatura.

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

Il codice predefinito della funzione sarà poi sostituito con il codice seguente, che implementa la logica di business.

1. Aprire il file **index.js** e sostituirlo con il codice seguente.

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

La logica che è stata aggiunta è semplice. Vengono eseguiti l'iterazione della matrice delle letture e il controllo del campo relativo alla temperatura. A seconda del valore di tale campo, lo stato viene impostato su **OK**, **CAUTION** o **DANGER**. La matrice delle letture viene quindi restituita dopo aver aggiunto il campo dello stato per ogni voce.

Si notino le istruzioni `log`. Durante l'esecuzione della funzione, le istruzioni aggiungeranno messaggi nella finestra di log.

## <a name="test-our-business-logic"></a>Testare la logica di business

In questo caso si userà il riquadro **Test** nel portale per testare la funzione.

1. Aprire la finestra **Test** dal menu a comparsa a destra.

1. Incollare la richiesta di esempio nella casella di testo del corpo della richiesta.

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

1. Selezionare **Esegui** e visualizzare la risposta nel riquadro di output. Per visualizzare i messaggi di log, aprire la scheda **Log** nel riquadro a comparsa nella parte inferiore della pagina. Lo screenshot seguente illustra un esempio di risposta nel riquadro di output e i messaggi nel riquadro **Log**.

    ![Screenshot del portale di Azure che visualizza il pannello dell'editor di funzioni con le schede Test e Log visualizzate. Nel riquadro di output è visualizzato un esempio di risposta inviata dalla funzione.](../media/5-portal-testing.png)

    Nel riquadro di output è possibile osservare che il campo relativo allo stato è stato aggiunto correttamente per ogni lettura.

    Passare al dashboard **Monitoraggio** per verificare se la richiesta è stata registrata in Application Insights.

    ![Screenshot del portale di Azure che visualizza l'esito positivo del test precedente nel dashboard Monitoraggio della funzione.](../media/5-app-insights.png)
