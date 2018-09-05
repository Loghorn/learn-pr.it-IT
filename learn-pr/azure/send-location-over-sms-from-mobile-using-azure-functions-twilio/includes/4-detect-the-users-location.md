L'app include un'interfaccia utente e un ViewModel. In questa unità si aggiunge la ricerca della posizione al ViewModel usando Xamarin.Essentials.

## <a name="enable-location-permissions"></a>Abilitare le autorizzazioni sulla posizione

Tutte le piattaforme mobili dispongono di funzionalità di sicurezza per proteggere le informazioni sull'utente e alcuni componenti hardware, ad esempio la fotocamera, la raccolta foto e la posizione dell'utente. Prima che un'app possa accedere alla posizione dell'utente, l'utente deve concedere l'autorizzazione apposita, concedendo le autorizzazioni implicitamente durante l'installazione o scegliendo un'autorizzazione in fase di esecuzione. Quando si visualizza un'app UWP nello Store, un elenco visualizza le autorizzazioni necessarie all'app. Quando si installa l'app, si concede implicitamente l'autorizzazione. Queste autorizzazioni vengono configurate in un file manifesto dell'app.

1. Nel progetto dell'app `ImHere.UWP` aprire il file `Package.appxmanifest`.

2. Accedere alla scheda **Funzionalità** e selezionare la funzionalità *Posizione*.

    ![Scheda delle funzionalità della piattaforma UWP](../media-drafts/4-uwp-location-capability.png)

> Se si vuole supportare Android o iOS, le autorizzazioni devono essere configurate in modo diverso. Queste informazioni sono descritte dettagliatamente nella [documentazione sulla geolocalizzazione di Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=android#getting-started).

## <a name="query-for-the-users-location"></a>Eseguire query sulla posizione dell'utente

Si possono rilevare due tipi di posizioni dell'utente, l'ultima posizione nota o quella corrente. Il rilevamento della posizione corrente può richiedere tempo perché il dispositivo potrebbe aver bisogno di stabilire un collegamento GPS e attendere che venga recuperata la posizione precisa. Il modo più rapido è ottenere l'ultima posizione nota rilevata dal dispositivo. L'ultima posizione nota è potenzialmente meno precisa, ma molto più veloce da rilevare. Le posizioni vengono visualizzate come latitudine e longitudine in [gradi decimali](https://en.wikipedia.org/wiki/Decimal_degrees) e altitudine del dispositivo in metri sul livello del mare.

1. Aprire la classe `MainViewModel` nel progetto standard .NET `ImHere`.

2. Nel metodo `SendLocation` effettuare una chiamata al metodo statico `GetLastKnownLocationAsync` sulla classe `Geolocation` nello spazio dei nomi `Xamarin.Essentials`.

    ```cs
    Location location = await Geolocation.GetLastKnownLocationAsync();
    ```

3. Aggiornare la proprietà `Message` con la posizione dell'utente eventualmente rilevata.

    ```cs
    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
    ```

Di seguito viene riportato l'intero codice del metodo.

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

Eseguire l'app e fare clic sul pulsante **Invia posizione** per vedere la posizione nell'interfaccia utente.

![App in esecuzione che mostra la posizione dell'utente](../media-drafts/4-running-app-showing-location.png)

> Questa app usa l'ultima posizione nota. In un'app per ambienti di produzione può essere utile ottenere la posizione precisa corrente con un periodo di scadenza per cui se una posizione non viene trovata entro il tempo specificato, viene restituita l'ultima posizione nota. Per altre informazioni su questa operazione, vedere la [documentazione sulla geolocalizzazione di Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation). Questa app non include funzionalità per la gestione degli errori. In un'app per ambienti di produzione è consigliabile gestire tutte le eccezioni che si verificano, ad esempio se la posizione non era disponibile e si è verificata un'eccezione.

## <a name="summary"></a>Riepilogo

In questa unità è stato descritto come usare Xamarin.Essentials per ottenere la posizione dell'utente. Nella prossima unità si creerà una funzione di Azure che agisca come back-end per l'app per dispositivi mobili.