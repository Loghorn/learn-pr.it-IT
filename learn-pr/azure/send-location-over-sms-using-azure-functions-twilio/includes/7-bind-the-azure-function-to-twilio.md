A questo punto, l'app per dispositivi mobili è completa e può inviare la posizione e l'elenco dei numeri di telefono dell'utente a una funzione di Azure in grado di deserializzare i dati. In questa unità si associa la funzione di Azure a Twilio per inviare messaggi SMS.

Le funzioni di Azure possono essere connesso ad altri servizi, servizi di Azure o servizi di terze parti. Queste connessioni, definite associazioni, sono disponibili in due forme: associazioni di input e output. Le associazioni di input forniscono dati alla funzione e le associazioni di output prelevare i dati dalla funzione e li inviano a un altro servizio. Per ulteriori informazioni sulle associazioni vedere la [documentazione relativa all'associazione di funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).

Le associazioni per le funzioni di Azure create in Visual Studio vengono definite usando i parametri della funzione, decorate con attributi.

## <a name="bind-the-azure-function-to-twilio"></a>Associare la funzione di Azure a Twilio

L'invio di SMS messaggi tramite Twilio richiede un'associazione di output che viene configurata con l'ID sottoscrizione dell'account (SID) e il Token di autenticazione.

1. Arrestare il runtime di funzioni di Azure locale se è ancora in esecuzione dall'unità precedente.

1. Aggiungere il pacchetto "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet al progetto `ImHere.Functions`. Questo pacchetto NuGet contiene le classi pertinenti per l'associazione.

1. Aggiungere un nuovo parametro per il metodo `Run` statico sulla classe statica `SendLocation` denominata `messages`. Questo parametro sarà di tipo `ICollector<SMSMessage>`. È necessario aggiungere una direttiva utilizzo per lo spazio dei nomi `Twilio`.

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                   "get", "post",
                                                                   Route = null)]HttpRequestMessage req,
                                                      ICollector<SMSMessage> messages,
                                                      TraceWriter log)
    ```

1. Decorare il nuovo parametro `messages` con l'attributo `TwilioSms`. Questo attributo ha tre parametri che è necessario impostare.

    | Impostazione      |  Valore   | Descrizione                                        |
    | --- | --- | ---|
    | **AccountSidSetting** | "TwilioAccountSid" | SID dell'account Twilio. Anziché impostare direttamente il SID, questo parametro è il nome di un valore nelle impostazioni dell'app per le funzioni che verrà usato per recuperare il SID. |
    | **AuthTokenSetting** | "TwilioAuthToken" | Token di autenticazione per l'account Twilio. Anziché impostare direttamente il Token di autenticazione, questo parametro è il nome di un valore nelle impostazioni dell'app per le funzioni che verrà usato per recuperare il Token di autenticazione. |
    | **From** | Il numero di telefono di Twilio | Il numero di telefono Twilio da cui proverranno i messaggi SMS in formato internazionale (+\<prefisso internazionale\>\<numero di telefono\>, ad esempio "+1555123456"). |

    Quando si crea un account Twilio, viene assegnato un numero di telefono da cui è possibile inviare messaggi. È possibile trovare questo numero di telefono nel dashboard **Numeri di telefono** di Twilio. Dal sito di Twilio, selezionare i puntini di sospensione nella parte inferiore del menu a sinistra. Quindi, selezionare *SUPER NETWORK -> Numeri di telefono*. È possibile aggiungere questo dashboard al menu a sinistra mediante l'icona della puntina. Il numero Twilio si troverà in *Gestisci numeri -> Numeri attivi*. È necessario rimuovere gli spazi dal numero.

    ![Ricerca del numero Twilio](../media/7-twilio-find-number.png)

    ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",
               AuthTokenSetting = "TwilioAuthToken",
               From = "+1xxxxxxxxx")]ICollector<SMSMessage> messages,
    ```

1. Le impostazioni dell'app per le funzioni possono essere configurate in locale all'interno del file `local.settings.json`. Aggiungere il SID dell'account Twilio e il Token di autenticazione a questo file JSON usando i nomi delle impostazioni passate all'attributo `TwilioSMS`.

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

    Sostituire \<Your SID\> (SID) e \<Your Auth Token\> (Token di autenticazione) con i valori del dashboard di Twilio.

    > Queste impostazioni locali verranno usate solo per l'esecuzione in locale. In un'app di produzione questi valori sarebbero le credenziali dell'account di sviluppo o test locale. Dopo aver distribuito la funzione di Azure in Azure, sarà possibile configurare i valori di produzione.
    > Se si archivia il codice nel controllo codice sorgente verranno archiviati anche questi valori di impostazione dell'applicazione locale, quindi prestare attenzione a non archiviare nessun valore effettivo in questi file se il codice è open source o pubblico in qualsiasi forma.

## <a name="create-the-sms-messages"></a>Creare messaggi SMS

Il parametro `ICollector<SMSMessage>` è una raccolta di istanze `SMSMessage` e viene usato per raccogliere i messaggi SMS da inviare. Quando la funzione ha terminato l'esecuzione, tutte le istanze di `SMSMessage` aggiunte a questa raccolta vengono passate a Twilio e inviate ai relativi destinatari.

1. Nella funzione `SendLocation` aggiungere il codice per riprodurre a ciclo i numeri di telefono nel `PostData` e creare un messaggio SMS per ognuno di essi.

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

    Il messaggio richiede il numero di telefono per l'invio a e un corpo che contiene l'URL di Google Maps creato dalla posizione dell'utente.

1. Registrare ogni messaggio e quindi aggiungerlo alla raccolta `messages`.

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.Info($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

Il metodo `SendLocation` completo è riportato di seguito.

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

## <a name="test-it-out"></a>Eseguire il test

1. Impostare l'app `ImHere.Functions` come progetto di avvio e avviarla senza eseguire il debug.

1. Impostare l'app `ImHere.UWP` come progetto di avvio ed eseguirla.

1. Immettere il proprio numero di telefono in formato internazionale (+\<prefisso internazionale\>\<numero di telefono\>) nell'app Xamarin.Forms. Gli account in versione di valutazione di Twilio possono inviare messaggi solo a numeri di telefono verificati quindi, per il momento, sarà possibile solo inviare messaggi a se stessi a meno che non si esegua l'aggiornamento a un account a pagamento o non vengano verificati altri numeri.

1. Fare clic sul pulsante **Invia posizione**. Se il messaggio SMS è stato inviato correttamente, nell'app Xamarin.Forms verrà visualizzato il messaggio "Location sent successfully" (Posizione inviata correttamente).

    ![L'app Xamarin.Forms mostra la posizione come inviata](../media/7-ui-location-sent.png)

1. Nei log console per la funzione di Azure, verrà visualizzato il messaggio creato e inviato. Se si verificano errori (ad esempio, il numero ha un formato non corretto), si verrà disconnessi.

    ![La console di funzioni di Azure con il messaggio visualizzato è stata inviata](../media/7-function-message-sent.png)

1. Controllare se sul telefono è presente un messaggio. Fare clic sul collegamento nel messaggio per visualizzare la posizione.

    ![Il messaggio SMS ricevuto su un telefono cellulare](../media/7-message-received.png)

## <a name="summary"></a>Riepilogo

In questa unità si è appreso come creare un'associazione di Twilio per la funzione di Azure e inviare un messaggio SMS con la posizione dell'utente a una funzione in esecuzione in locale. Nell'unità di successiva si pubblicherà la funzione in Azure.