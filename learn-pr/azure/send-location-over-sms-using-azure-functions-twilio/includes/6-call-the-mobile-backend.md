L'app per dispositivi mobili è in esecuzione ed è stata creata la versione iniziale della funzione di Azure. In questa unità si chiama la funzione di Azure all'app per dispositivi mobili, passando la posizione dell'utente e l'elenco dei numeri di telefono a cui l'utente intende inviare i messaggi SMS.

## <a name="calling-the-azure-function-from-the-mobile-app"></a>Chiamata della funzione di Azure dall'app per dispositivi mobili

1. Aprire il file `MainViewModel`.

1. In questa classe aggiungere un campo `HttpClient` privato denominato `client`. Occorrerà aggiungere un riferimento allo spazio dei nomi `System.Net.Http`.

    ```cs
    HttpClient client = new HttpClient();
    ```

1. Aggiungere un campo costante per l'URL di base per la funzione. Impostare il campo sull'indirizzo su cui è in ascolto il runtime locale di Funzioni di Azure. Dopo che la funzione è stata distribuita in Azure, questa costante può essere modificata nell'URL di Azure.

    ```cs
    const string baseUrl = "http://localhost:7071";
    ```

1. Nel metodo `SendLocation`, dopo che la posizione è stata rilevata, creare una nuova istanza di `PostData` usando la posizione e l'elenco dei numero di telefono immessi dall'utente. È necessario aggiungere una direttiva using per lo spazio dei nomi `ImHere.Data`.

    ```cs
    PostData postData = new PostData
    {
        Latitude = location.Latitude,
        Longitude = location.Longitude,
        ToNumbers = PhoneNumbers.Split('\n')
    };
    ```

    > [!NOTE]
    > Si presuppone che i numeri di telefono siano stati inseriti nel formato corretto, uno per riga nel controllo `Editor`. In un'app per ambienti di produzione si dovrebbe prevedere una fase di convalida per assicurarsi che sia stato immesso almeno un numero di telefono e che abbia il formato corretto.    
 

1. Per serializzare `PostData` come JSON, il modo più facile consiste nell'usare il pacchetto NuGet Newtonsoft.Json. Aggiungere il pacchetto NuGet al progetto `ImHere` nello stesso modo in cui si è aggiunto Xamarin.Essentials in un'unità precedente.

1. Serializzare `PostData` in una `string` usando la classe statica `JsonConvert`. È necessario aggiungere una direttiva using per lo spazio dei nomi `Newtonsoft.Json`. Codificare questa stringa in una classe `StringContent` in modo che possa essere passata alla funzione di Azure come JSON.

    ```cs
    string data = JsonConvert.SerializeObject(postData);
    StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
    ```

1. Pubblicare questi dati alla funzione e ottenere il risultato.

   ```cs
    HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                        content);
   ```

   È possibile accedere alle funzioni di Azure usando `/api/<function name>`, quindi supponendo che la porta scelta dal runtime locale di Funzioni di Azure sia la porta 7071, sarà possibile accedere alla funzione `SendLocation` all'indirizzo `http://localhost:7071/api/SendLocation`.

1. A seconda del risultato visualizzare un messaggio nell'interfaccia utente.

    ```cs
    if (result.IsSuccessStatusCode)
        Message = "Location sent successfully";
    else
        Message = $"Error - {result.ReasonPhrase}";
    ```

Di seguito sono riportati il codice completo per i nuovi campi e il metodo `SendLocation`.

```cs
HttpClient client = new HttpClient();
const string baseUrl = "http://localhost:7071";

async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";

        PostData postData = new PostData
        {
            Latitude = location.Latitude,
            Longitude = location.Longitude,
            ToNumbers = PhoneNumbers.Split('\n')
        };

        string data = JsonConvert.SerializeObject(postData);
        StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
        HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                            content);

        if (result.IsSuccessStatusCode)
            Message = "Location sent successfully";
        else
            Message = $"Error - {result.ReasonPhrase}";
    }
}
```

## <a name="testing-it-out"></a>Esecuzione dei test

1. Assicurarsi che la funzione di Azure sia ancora in esecuzione in locale e che la porta corrisponda a quella indicata nel metodo `SendLocation`.

1. Impostare l'app UWP come app di avvio ed eseguirla. Fare clic sul pulsante **Invia posizione**. L'output verrà visualizzato nella finestra della console del runtime di Funzioni di Azure con la funzione chiamata e la registrazione che mostra l'URL generato.

    ![Output della funzione chiamata](../media/6-function-called.png)

1. Per testare la generazione dell'URL, incollarlo dalla console in un browser. Dovrebbe mostrare la posizione corrente.

    > [!TIP]
    > La località visualizzata corrisponde a quella in cui l'app è in esecuzione, nelle vicinanze, quindi, del data center in cui la macchina virtuale è in esecuzione. Se l'app è in esecuzione nel dispositivo locale, verrà visualizzata la località dell'utente.

## <a name="summary"></a>Riepilogo

In questa unità è stato descritto come chiamare una funzione di Azure dall'app per dispositivi mobili. Questa chiamata ha passato la posizione dell'utente e i numeri di telefono immessi come JSON. Nella prossima unità si eseguirà l'associazione tra la funzione di Azure e Twilio per inviare la posizione in un messaggio SMS.