A questo punto, l'app per dispositivi mobili è completa e può inviare la posizione e l'elenco dei numeri di telefono dell'utente a una funzione di Azure in grado di deserializzare i dati. In questa unità si associa la funzione di Azure a Twilio per inviare messaggi SMS.

È possibile connettere Funzioni di Azure ad altri servizi, servizi di Azure o servizi di terze parti. Queste connessioni, definite associazioni, sono disponibili in due forme: associazioni di input e output. Le associazioni di input forniscono dati alla funzione e le associazioni di output prelevare i dati dalla funzione e li inviano a un altro servizio. Per ulteriori informazioni sulle associazioni vedere la [documentazione relativa all'associazione di funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings?azure-portal=true).

Le associazioni per Funzioni di Azure create in Visual Studio vengono definite usando i parametri della funzione, decorate con attributi.

## <a name="bind-the-azure-function-to-twilio"></a>Associare la funzione di Azure a Twilio

L'invio di SMS messaggi tramite Twilio richiede un'associazione di output che viene configurata con l'ID sottoscrizione dell'account (SID) e il Token di autenticazione.

1. Se il runtime di Funzioni di Azure locale è ancora in esecuzione dall'unità precedente, arrestarlo.

1. Aggiungere il pacchetto NuGet "Microsoft.Azure.WebJobs.Extensions.Twilio" versione 3.0.0-rc1 al progetto `ImHere.Functions`. **Usare la versione 3.0.0-rc1, NON la versione 3.0.0, a causa di un bug nella versione stabile con le associazioni di Twilio**. Questo pacchetto NuGet contiene le classi pertinenti per l'associazione.

1. Aggiungere un nuovo parametro per il metodo `Run` statico sulla classe statica `SendLocation` denominata `messages`. Questo parametro sarà di tipo `ICollector<CreateMessageOptions>`. È necessario aggiungere una direttiva `using` per lo spazio dei nomi `Twilio.Rest.Api.V2010.Account`.

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,"get", "post", Route = null)]HttpRequestMessage req,
                                                ICollector<CreateMessageOptions> messages,
                                                ILogger log)
    ```

1. Decorare il nuovo parametro `messages` con l'attributo `TwilioSms` come segue: 

      ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",AuthTokenSetting = "TwilioAuthToken", From = "+1xxxxxxxxx")]ICollector<CreateMessageOptions> messages,
    ```
    Per questo attributo è necessario impostare tre parametri:

    * **AccountSidSetting**: da impostare su `"TwilioAccountSid"`
  
        Si tratta del SID per l'account Twilio registrato in precedenza nel modulo. Anziché impostare direttamente il SID, è possibile usare questo parametro, corrispondente al nome di un valore nelle impostazioni dell'app per le funzioni che verrà usato per recuperare il SID stesso.

    * **AuthTokenSetting**: da impostare su `"TwilioAuthToken"`

       Si tratta del token di autenticazione per l'account Twilio registrato in precedenza nel modulo. Anziché impostare direttamente il token di autenticazione, è possibile usare questo parametro, corrispondente al nome di un valore nelle impostazioni dell'app per le funzioni che verrà usato per recuperare il token di autenticazione stesso.

    * **From**: impostare questo parametro sul numero di telefono Twilio attivo registrato in precedenza nel modulo.

        Il numero di telefono Twilio da cui proverranno i messaggi SMS in formato internazionale (+\<prefisso internazionale\>\<numero di telefono\>, ad esempio "+1555123456").

    > [!IMPORTANT]
    > Assicurarsi di rimuovere tutti gli spazi dal numero di telefono.


1. Le impostazioni dell'app per le funzioni possono essere configurate in locale all'interno del file `local.settings.json`. Aggiungere il SID dell'account Twilio e il Token di autenticazione a questo file JSON usando i nomi delle impostazioni passate all'attributo `TwilioSMS`.

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

    Sostituire \<Your SID\> (SID) e \<Your Auth Token\> (Token di autenticazione) con i valori del dashboard di Twilio.

    > [!NOTE]
    > Queste impostazioni locali verranno usate solo per l'esecuzione in locale. In un'app di produzione questi valori sarebbero le credenziali dell'account di sviluppo o di test locale. Dopo aver distribuito la funzione di Azure in Azure, sarà possibile configurare i valori di produzione.

     > [!NOTE]
    > Se si archivia il codice nel controllo codice sorgente verranno archiviati anche questi valori di impostazione dell'applicazione locale, quindi prestare attenzione a non archiviare nessun valore effettivo in questi file se il codice è open source o pubblico in qualsiasi forma.
    

## <a name="create-the-sms-messages"></a>Creare messaggi SMS

Il parametro `ICollector<CreateMessageOptions>` è una raccolta di istanze `CreateMessageOptions` e viene usato per raccogliere i messaggi SMS da inviare. Al termine dell'esecuzione della funzione tutte le istanze di `CreateMessageOptions` aggiunte a questa raccolta vengono passate a Twilio e usate per creare i messaggi da inviare ai destinatari di pertinenza.

1. Nella funzione `SendLocation` aggiungere il codice per riprodurre a ciclo i numeri di telefono nel `PostData` e creare un messaggio SMS per ognuno di essi. Sarà necessario aggiungere una direttiva using per `Twilio.Types`.

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

    Nel messaggio è necessario includere il numero di telefono di destinazione e un corpo che contiene l'URL di Google Maps creato dalla posizione dell'utente.

1. Registrare ogni messaggio e quindi aggiungerlo alla raccolta `messages`.

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.LogInformation($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

Il metodo `SendLocation` completo è riportato di seguito. Il numero di telefono attivo sostituirà il segnaposto nel parametro `From`.

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

## <a name="test-it-out"></a>Eseguire il test

1. Impostare l'app `ImHere.Functions` come progetto di avvio e avviarla senza eseguire il debug.

1. Impostare l'app `ImHere.UWP` come progetto di avvio ed eseguirla.

1. Immettere il proprio numero di telefono in formato internazionale (+\<prefisso internazionale\>\<numero di telefono\>) nell'app Xamarin.Forms. Gli account in versione di valutazione di Twilio possono inviare messaggi solo a numeri di telefono verificati quindi, per il momento, sarà possibile solo inviare messaggi a se stessi a meno che non si esegua l'aggiornamento a un account a pagamento o non vengano verificati altri numeri.

1. Fare clic sul pulsante **Invia posizione**. Se il messaggio SMS è stato inviato correttamente, nell'app Xamarin.Forms verrà visualizzato un messaggio simile a "Posizione inviata correttamente".

    ![App Xamarin.Forms in cui la posizione risulta inviata](../media/7-ui-location-sent.png)

1. Nei log console per la funzione di Azure, verrà visualizzato il messaggio creato e inviato. Se si verificano errori (ad esempio, il numero ha un formato non corretto), si verrà disconnessi.

    ![La console di funzioni di Azure con il messaggio visualizzato è stata inviata](../media/7-function-message-sent.png)

1. Controllare se sul telefono è presente un messaggio. Fare clic sul collegamento nel messaggio per visualizzare la posizione.

    ![Il messaggio SMS ricevuto su un telefono cellulare](../media/7-message-received.png)

    > [!TIP]
    > La località visualizzata corrisponde a quella in cui l'app è in esecuzione, nelle vicinanze, quindi, del data center in cui la macchina virtuale è in esecuzione. Se l'app è in esecuzione nel dispositivo locale, verrà visualizzata la località dell'utente.

## <a name="summary"></a>Riepilogo

In questa unità si è appreso come creare un'associazione di Twilio per la funzione di Azure e inviare un messaggio SMS con la posizione dell'utente a una funzione in esecuzione in locale. Nell'unità di successiva si pubblicherà la funzione in Azure.
