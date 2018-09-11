<span data-ttu-id="817ee-101">L'app per dispositivi mobili è in esecuzione ed è stata creata la versione iniziale della funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="817ee-101">The mobile app runs and the initial version of the Azure function has been created.</span></span> <span data-ttu-id="817ee-102">In questa unità si chiama la funzione di Azure all'app per dispositivi mobili, passando la posizione dell'utente e l'elenco dei numeri di telefono a cui l'utente intende inviare i messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="817ee-102">In this unit, you call the Azure function from the mobile app, passing in the user's location and the list of phone numbers the user wants to send SMS messages to.</span></span>

## <a name="calling-the-azure-function-from-the-mobile-app"></a><span data-ttu-id="817ee-103">Chiamata della funzione di Azure dall'app per dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="817ee-103">Calling the Azure function from the mobile app</span></span>

1. <span data-ttu-id="817ee-104">Aprire il file `MainViewModel`.</span><span class="sxs-lookup"><span data-stu-id="817ee-104">Open the `MainViewModel`.</span></span>

1. <span data-ttu-id="817ee-105">In questa classe aggiungere un campo `HttpClient` privato denominato `client`.</span><span class="sxs-lookup"><span data-stu-id="817ee-105">In this class, add a private `HttpClient` field called `client`.</span></span> <span data-ttu-id="817ee-106">Occorrerà aggiungere un riferimento allo spazio dei nomi `System.Net.Http`.</span><span class="sxs-lookup"><span data-stu-id="817ee-106">You'll need to add a reference to the `System.Net.Http` namespace.</span></span>

    ```cs
    HttpClient client = new HttpClient();
    ```

1. <span data-ttu-id="817ee-107">Aggiungere un campo costante per l'URL di base per la funzione.</span><span class="sxs-lookup"><span data-stu-id="817ee-107">Add a constant field for the base URL for the function.</span></span> <span data-ttu-id="817ee-108">Impostare il campo sull'indirizzo su cui è in ascolto il runtime locale di Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="817ee-108">Set this to the address that the local Azure Functions runtime is listening on.</span></span> <span data-ttu-id="817ee-109">Dopo che la funzione è stata distribuita in Azure, questa costante può essere modificata nell'URL di Azure.</span><span class="sxs-lookup"><span data-stu-id="817ee-109">Once the function is deployed to Azure, this constant can be changed to be the Azure URL.</span></span>

    ```cs
    const string baseUrl = "http://localhost:7071";
    ```

1. <span data-ttu-id="817ee-110">Nel metodo `SendLocation`, dopo che la posizione è stata rilevata, creare una nuova istanza di `PostData` usando la posizione e l'elenco dei numero di telefono immessi dall'utente.</span><span class="sxs-lookup"><span data-stu-id="817ee-110">Inside the `SendLocation` method, after the location has been found, create a new instance of `PostData` using the location and the list of phone numbers entered by the user.</span></span> <span data-ttu-id="817ee-111">È necessario aggiungere una direttiva using per lo spazio dei nomi `ImHere.Data`.</span><span class="sxs-lookup"><span data-stu-id="817ee-111">You'll need to add a using directive for the `ImHere.Data` namespace.</span></span>

    ```cs
    PostData postData = new PostData
    {
        Latitude = location.Latitude,
        Longitude = location.Longitude,
        ToNumbers = PhoneNumbers.Split('\n')
    };
    ```

    > <span data-ttu-id="817ee-112">Si presuppone che i numeri di telefono siano stati inseriti nel formato corretto, uno per riga nel controllo `Editor`.</span><span class="sxs-lookup"><span data-stu-id="817ee-112">This assumes that the phone numbers have been entered in the correct format, one per line in the `Editor` control.</span></span> <span data-ttu-id="817ee-113">In un'app per ambienti di produzione si dovrebbe prevedere una fase di convalida per assicurarsi che sia stato immesso almeno un numero di telefono e che abbia il formato corretto.</span><span class="sxs-lookup"><span data-stu-id="817ee-113">In a production-quality app, there would be validation around this to ensure one or more phone numbers were entered and were in the correct format.</span></span>

1. <span data-ttu-id="817ee-114">Per serializzare `PostData` come JSON, il modo più facile consiste nell'usare il pacchetto NuGet Newtonsoft.JSON.</span><span class="sxs-lookup"><span data-stu-id="817ee-114">To serialize the `PostData` as JSON, the easiest way is to use the Newtonsoft.JSON NuGet package.</span></span> <span data-ttu-id="817ee-115">Aggiungere il pacchetto NuGet al progetto `ImHere` nello stesso modo in cui si è aggiunto Xamarin.Essentials in un'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="817ee-115">Add this NuGet package to the `ImHere` project in the same way that you added Xamarin.Essentials in an earlier unit.</span></span>

1. <span data-ttu-id="817ee-116">Serializzare `PostData` in una `string` usando la classe statica `JsonConvert`.</span><span class="sxs-lookup"><span data-stu-id="817ee-116">Serialize the `PostData` to a `string` using the `JsonConvert` static class.</span></span> <span data-ttu-id="817ee-117">È necessario aggiungere una direttiva using per lo spazio dei nomi `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="817ee-117">You'll need to add a using directive for the `Newtonsoft.Json` namespace.</span></span> <span data-ttu-id="817ee-118">Codificare questa stringa in una classe `StringContent` in modo che possa essere passata alla funzione di Azure come JSON.</span><span class="sxs-lookup"><span data-stu-id="817ee-118">Encode this string into a `StringContent` class so that it can be passed to the Azure function as JSON.</span></span>

    ```cs
    string data = JsonConvert.SerializeObject(postData);
    StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
    ```

1. <span data-ttu-id="817ee-119">Pubblicare questi dati alla funzione e ottenere il risultato.</span><span class="sxs-lookup"><span data-stu-id="817ee-119">Post this data to the function and get the result back.</span></span>

   ```cs
    HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                        content);
   ```

   <span data-ttu-id="817ee-120">È possibile accedere alle funzioni di Azure usando `/api/<function name>`, quindi supponendo che la porta scelta dal runtime locale di Funzioni di Azure sia la porta 7071, sarà possibile accedere alla funzione `SendLocation` all'indirizzo `http://localhost:7071/api/SendLocation`.</span><span class="sxs-lookup"><span data-stu-id="817ee-120">Azure functions are accessed using `/api/<function name>`, so assuming the port chosen by the local Functions runtime is 7071, the `SendLocation` function will be accessible at `http://localhost:7071/api/SendLocation`.</span></span>

1. <span data-ttu-id="817ee-121">A seconda del risultato visualizzare un messaggio nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="817ee-121">Depending on the result, show a message on the UI.</span></span>

    ```cs
    if (result.IsSuccessStatusCode)
        Message = "Location sent successfully";
    else
        Message = $"Error - {result.ReasonPhrase}";
    ```

<span data-ttu-id="817ee-122">Di seguito sono riportati il codice completo per i nuovi campi e il metodo `SendLocation`.</span><span class="sxs-lookup"><span data-stu-id="817ee-122">The full code for the new fields and the `SendLocation` method is below.</span></span>

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

## <a name="testing-it-out"></a><span data-ttu-id="817ee-123">Esecuzione dei test</span><span class="sxs-lookup"><span data-stu-id="817ee-123">Testing it out</span></span>

1. <span data-ttu-id="817ee-124">Assicurarsi che la funzione di Azure sia ancora in esecuzione in locale e che la porta corrisponda a quella indicata nel metodo `SendLocation`.</span><span class="sxs-lookup"><span data-stu-id="817ee-124">Make sure the Azure function is still running locally and the port matches the `SendLocation` method.</span></span>

1. <span data-ttu-id="817ee-125">Impostare l'app UWP come app di avvio ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="817ee-125">Set the UWP app as the startup app and run it.</span></span> <span data-ttu-id="817ee-126">Fare clic sul pulsante **Invia posizione**.</span><span class="sxs-lookup"><span data-stu-id="817ee-126">Click the **Send Location** button.</span></span> <span data-ttu-id="817ee-127">L'output verrà visualizzato nella finestra della console del runtime di Funzioni di Azure con la funzione chiamata e la registrazione che mostra l'URL generato.</span><span class="sxs-lookup"><span data-stu-id="817ee-127">You'll see output in the Functions runtime console window showing the function being called, and the logging showing the generated URL.</span></span>

    ![Output della funzione chiamata](../media-drafts/6-function-called.png)

1. <span data-ttu-id="817ee-129">Per testare la generazione dell'URL, incollarlo dalla console in un browser.</span><span class="sxs-lookup"><span data-stu-id="817ee-129">To test the URL generation, paste it from the console into a browser.</span></span> <span data-ttu-id="817ee-130">Dovrebbe mostrare la posizione corrente.</span><span class="sxs-lookup"><span data-stu-id="817ee-130">It should show your current location.</span></span>

## <a name="summary"></a><span data-ttu-id="817ee-131">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="817ee-131">Summary</span></span>

<span data-ttu-id="817ee-132">In questa unità è stato descritto come chiamare una funzione di Azure dall'app per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="817ee-132">In this unit, you learned how to call an Azure function from the mobile app.</span></span> <span data-ttu-id="817ee-133">Questa chiamata ha passato la posizione dell'utente e i numeri di telefono immessi come JSON.</span><span class="sxs-lookup"><span data-stu-id="817ee-133">This call passed the user's location and the phone numbers they entered as JSON.</span></span> <span data-ttu-id="817ee-134">Nella prossima unità si eseguirà l'associazione tra la funzione di Azure e Twilio per inviare la posizione in un messaggio SMS.</span><span class="sxs-lookup"><span data-stu-id="817ee-134">In the next unit, you'll bind the Azure function to Twilio to send this location as an SMS message.</span></span>