<span data-ttu-id="07a23-101">L'app include un'interfaccia utente e un ViewModel.</span><span class="sxs-lookup"><span data-stu-id="07a23-101">The app has a UI and a ViewModel.</span></span> <span data-ttu-id="07a23-102">In questa unità si aggiunge la ricerca della posizione al ViewModel usando Xamarin.Essentials.</span><span class="sxs-lookup"><span data-stu-id="07a23-102">In this unit, you add location lookup to the ViewModel using Xamarin.Essentials.</span></span>

## <a name="enable-location-permissions"></a><span data-ttu-id="07a23-103">Abilitare le autorizzazioni sulla posizione</span><span class="sxs-lookup"><span data-stu-id="07a23-103">Enable location permissions</span></span>

<span data-ttu-id="07a23-104">Tutte le piattaforme mobili dispongono di funzionalità di sicurezza per proteggere le informazioni sull'utente e alcuni componenti hardware, ad esempio la fotocamera, la raccolta foto e la posizione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="07a23-104">All mobile platforms have security around user information and certain hardware, such as the camera, photo library, and the user's location.</span></span> <span data-ttu-id="07a23-105">Prima che un'app possa accedere alla posizione dell'utente, l'utente deve concedere l'autorizzazione apposita, concedendo le autorizzazioni implicitamente durante l'installazione o scegliendo un'autorizzazione in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="07a23-105">Before an app can access the user's location, the user has to grant permission - either by implicitly granting these permissions at install time or by choosing to grant a permission at runtime.</span></span> <span data-ttu-id="07a23-106">Quando si visualizza un'app UWP nello Store, un elenco visualizza le autorizzazioni necessarie all'app.</span><span class="sxs-lookup"><span data-stu-id="07a23-106">When you view a UWP app on the store, the listing will show the permissions that the app needs.</span></span> <span data-ttu-id="07a23-107">Quando si installa l'app, si concede implicitamente l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="07a23-107">By installing the app, you implicitly grant permission.</span></span> <span data-ttu-id="07a23-108">Queste autorizzazioni vengono configurate in un file manifesto dell'app.</span><span class="sxs-lookup"><span data-stu-id="07a23-108">These permissions are configured in an app manifest file.</span></span>

1. <span data-ttu-id="07a23-109">Nel progetto dell'app `ImHere.UWP` aprire il file `Package.appxmanifest`.</span><span class="sxs-lookup"><span data-stu-id="07a23-109">In the `ImHere.UWP` app project, open the `Package.appxmanifest` file.</span></span>

2. <span data-ttu-id="07a23-110">Accedere alla scheda **Funzionalità** e selezionare la funzionalità *Posizione*.</span><span class="sxs-lookup"><span data-stu-id="07a23-110">Head to the **Capabilities** tab and check the *Location* capability.</span></span>

    ![Scheda delle funzionalità della piattaforma UWP](../media/4-uwp-location-capability.png)

> <span data-ttu-id="07a23-112">Se si vuole supportare Android o iOS, le autorizzazioni devono essere configurate in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="07a23-112">If you want to support Android or iOS, the permissions need to be configured differently.</span></span> <span data-ttu-id="07a23-113">Queste informazioni sono descritte dettagliatamente nella [documentazione sulla geolocalizzazione di Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=android#getting-started).</span><span class="sxs-lookup"><span data-stu-id="07a23-113">This is detailed in the [Xamarin.Essentials Geolocation docs](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=android#getting-started).</span></span>

## <a name="query-for-the-users-location"></a><span data-ttu-id="07a23-114">Eseguire query sulla posizione dell'utente</span><span class="sxs-lookup"><span data-stu-id="07a23-114">Query for the user's location</span></span>

<span data-ttu-id="07a23-115">Si possono rilevare due tipi di posizioni dell'utente, l'ultima posizione nota o quella corrente.</span><span class="sxs-lookup"><span data-stu-id="07a23-115">There are two ways to get the user's location - the last known or the current.</span></span> <span data-ttu-id="07a23-116">Il rilevamento della posizione corrente può richiedere tempo perché il dispositivo potrebbe aver bisogno di stabilire un collegamento GPS e attendere che venga recuperata la posizione precisa.</span><span class="sxs-lookup"><span data-stu-id="07a23-116">The current location can take some time to get because the device may need to establish a GPS link and wait for the accurate location to be retrieved.</span></span> <span data-ttu-id="07a23-117">Il modo più rapido è ottenere l'ultima posizione nota rilevata dal dispositivo.</span><span class="sxs-lookup"><span data-stu-id="07a23-117">The fastest way is to get the last known location detected by the device.</span></span> <span data-ttu-id="07a23-118">L'ultima posizione nota è potenzialmente meno precisa, ma molto più veloce da rilevare.</span><span class="sxs-lookup"><span data-stu-id="07a23-118">The last known location is potentially less accurate but is a much faster call.</span></span> <span data-ttu-id="07a23-119">Le posizioni vengono visualizzate come latitudine e longitudine in [gradi decimali](https://en.wikipedia.org/wiki/Decimal_degrees) e altitudine del dispositivo in metri sul livello del mare.</span><span class="sxs-lookup"><span data-stu-id="07a23-119">Locations come as the latitude and longitude in [decimal degrees](https://en.wikipedia.org/wiki/Decimal_degrees) and the altitude of the device in meters above sea level.</span></span>

1. <span data-ttu-id="07a23-120">Aprire la classe `MainViewModel` nel progetto standard .NET `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="07a23-120">Open the `MainViewModel` class in the `ImHere` .NET standard project.</span></span>

2. <span data-ttu-id="07a23-121">Nel metodo `SendLocation` effettuare una chiamata al metodo statico `GetLastKnownLocationAsync` sulla classe `Geolocation` nello spazio dei nomi `Xamarin.Essentials`.</span><span class="sxs-lookup"><span data-stu-id="07a23-121">In the `SendLocation` method, make a call to the `GetLastKnownLocationAsync` static method on the `Geolocation` class in the `Xamarin.Essentials` namespace.</span></span>

    ```cs
    Location location = await Geolocation.GetLastKnownLocationAsync();
    ```

3. <span data-ttu-id="07a23-122">Aggiornare la proprietà `Message` con la posizione dell'utente eventualmente rilevata.</span><span class="sxs-lookup"><span data-stu-id="07a23-122">Update the `Message` property with the user's location if one is found.</span></span>

    ```cs
    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
    ```

<span data-ttu-id="07a23-123">Di seguito viene riportato l'intero codice del metodo.</span><span class="sxs-lookup"><span data-stu-id="07a23-123">The full code for this method is below.</span></span>

```cs
async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
}
```

<span data-ttu-id="07a23-124">Eseguire l'app e fare clic sul pulsante **Invia posizione** per vedere la posizione nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="07a23-124">Run the app and click the **Send Location** button to see the location on the UI.</span></span>

![App in esecuzione che mostra la posizione dell'utente](../media/4-running-app-showing-location.png)

> <span data-ttu-id="07a23-126">Questa app usa l'ultima posizione nota.</span><span class="sxs-lookup"><span data-stu-id="07a23-126">This app uses the last known location.</span></span> <span data-ttu-id="07a23-127">In un'app per ambienti di produzione può essere utile ottenere la posizione precisa corrente con un periodo di scadenza per cui se una posizione non viene trovata entro il tempo specificato, viene restituita l'ultima posizione nota.</span><span class="sxs-lookup"><span data-stu-id="07a23-127">In a production-quality app, you would want to get the current accurate location with a time-out, and if one is not found in time, fall back to the last known.</span></span> <span data-ttu-id="07a23-128">Per altre informazioni su questa operazione, vedere la [documentazione sulla geolocalizzazione di Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation). Questa app non include funzionalità per la gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="07a23-128">You can read more on how to do this in the [Xamarin.Essentials Geolocation docs](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation). This app does not have error handling.</span></span> <span data-ttu-id="07a23-129">In un'app per ambienti di produzione è consigliabile gestire tutte le eccezioni che si verificano, ad esempio se la posizione non era disponibile e si è verificata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="07a23-129">In a production-quality app, you should handle any exceptions that occur, for example, if the location was not available and an exception was thrown.</span></span>

## <a name="summary"></a><span data-ttu-id="07a23-130">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="07a23-130">Summary</span></span>

<span data-ttu-id="07a23-131">In questa unità è stato descritto come usare Xamarin.Essentials per ottenere la posizione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="07a23-131">In this unit, you learned how to use Xamarin.Essentials to get the user's location.</span></span> <span data-ttu-id="07a23-132">Nella prossima unità si creerà una funzione di Azure che agisca come back-end per l'app per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="07a23-132">In the next unit, you'll create an Azure function to act as a back end for the mobile app.</span></span>