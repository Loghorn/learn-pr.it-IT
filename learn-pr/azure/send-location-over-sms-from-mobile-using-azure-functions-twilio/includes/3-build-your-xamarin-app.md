A questo punto, l'app per dispositivi mobili è una semplice app "Hello World". In questa unità si aggiungerà l'interfaccia utente e una logica dell'applicazione di base.

L'interfaccia utente per l'app sarà costituita da:

* Un controllo voce di testo per immettere alcuni numeri di telefono.
* Un pulsante per inviare la posizione a questi numeri usando una funzione di Azure.
* Un'etichetta che visualizzerà un messaggio all'utente indicante lo stato corrente, ad esempio, il percorso inviato e l'invio corretto del percorso.

Xamarin.Forms supporta uno schema progettuale denominato MVVM (Model-View-ViewModel). Altre informazioni su MVVM vedere la [documentazione di Xamarin MVVM](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm), ma la logica essenziale è che ogni pagina (visualizzazione) contiene un elemento ViewModel che espone le proprietà e il comportamento.

Le proprietà ViewModel sono associate a componenti nell'interfaccia utente in base al nome e questa associazione consente di sincronizzare dati tra View e ViewModel. Ad esempio, una proprietà `string` in un ViewModel denominato `Name` può essere associata alla proprietà `Text` di un controllo voce di testo nell'interfaccia utente. Il controllo voce di testo Mostra il valore di `Name` proprietà e, quando l'utente modifica il testo nell'interfaccia utente, il `Name` proprietà viene aggiornata. Se il valore della proprietà `Name` viene modificato in ViewModel, viene generato un evento per indicare all'interfaccia utente da aggiornare.

Il comportamento di ViewModel viene esposto come proprietà dei comandi, dove un comando è un oggetto che esegue il wrapping di un'azione che viene eseguita quando il comando viene richiamato. Questi comandi sono associati in base al nome a controlli quali pulsanti e toccando un pulsante viene richiamato il comando.

## <a name="create-a-base-viewmodel"></a>Creare un'immagine di ViewModel

Tutti i ViewModel implementano l'interfaccia `INotifyPropertyChanged`. Questa interfaccia dispone di un singolo evento, `PropertyChanged`, che viene usato per notificare all'interfaccia utente eventuali aggiornamenti. Questo evento ha argomenti dell'evento che contengono il nome della proprietà che è stata modificata. È pratica comune creare una classe ViewModel di base che implementa questa interfaccia e fornisce alcuni metodi di supporto.

1. Creare una nuova classe nel progetto :NET `ImHere` standard denominato `BaseViewModel` facendo clic con il pulsante destro del mouse sul progetto e quindi selezionando *Aggiungi -> Classe...* . Assegnare un nome alla nuova classe "BaseViewModel" e fare clic su **Aggiungi**.

2. Rendere la classe `public` e derivarla da `INotifyPropertyChanged`. È necessario aggiungere una direttiva utilizzo per `System.ComponentModel`.

3. Implementare l'interfaccia `INotifyPropertyChanged` aggiungendo l'evento `PropertyChanged`:

    ```cs
    public event PropertyChangedEventHandler PropertyChanged;
    ```

4. Il modello comune per le proprietà ViewModel è disporre di una proprietà pubblica con un campo sottostante privato. Nel setter proprietà, il campo sottostante viene confrontato con il nuovo valore. Se il nuovo valore è diverso dal campo sottostante, il campo sottostante viene aggiornato e viene generato l'evento `PropertyChanged`. Questa logica è facile da prendere in considerazione in un metodo, quindi aggiungere il metodo `Set`. È necessario aggiungere una direttiva utilizzo per lo spazio dei nomi `System.Runtime.CompilerServices`.

    ```cs
    protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(field, value)) return;
        field = value;
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    ```

    Questo metodo fa riferimento al campo sottostante, a nuovo valore e al nome della proprietà. Se il campo non è stato modificato, il metodo restituisce, in caso contrario, il campo viene aggiornato e viene generato l'evento `PropertyChanged`. Il parametro `propertyName` nel metodo `Set` è un parametro predefinito ed è contrassegnato con l'attributo `CallerMemberName`. Quando questo metodo viene chiamato da un setter proprietà, in genere questo parametro viene lasciato come valore predefinito. Il compilatore quindi imposterà automaticamente il valore del parametro sul nome della proprietà chiamata.

Di seguito è riportato il codice completo per questa classe.

```cs
using System.ComponentModel;
using System.Runtime.CompilerServices;

namespace ImHere
{
    public class BaseViewModel : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;

        protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
        {
            if (Equals(field, value)) return;
            field = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

## <a name="create-a-viewmodel-for-the-page"></a>Creare un ViewModel per la pagina

Il `MainPage` disporrà di un controllo di immissione del testo per i numeri di telefono e di un'etichetta per visualizzare un messaggio. Questi controlli verranno associati alle proprietà in un ViewModel.

1. Creare una classe denominata `MainViewModel` nel progetto standard .NET `ImHere`.

2. Rendere pubblica la classe e derivarla da `BaseViewModel`.

3. Aggiungere due proprietà `string`, `PhoneNumbers` e `Message`, ciascuna con un campo sottostante. Nel setter proprietà, usare il metodo `Set` della classe di base per aggiornare il valore e generare l'evento `PropertyChanged`.

   ```cs
    string message = "";
    public string Message
    {
        get => message;
        set => Set(ref message, value);
    }

    string phoneNumbers = "";
    public string PhoneNumbers
    {
        get => phoneNumbers;
        set => Set(ref phoneNumbers, value);
    }
   ```

4. Aggiungere una proprietà comando di sola lettura denominata `SendLocationCommand`. Questo comando avrà un tipo di `ICommand` dallo spazio dei nomi `System.Windows.Input`.

    ```cs
    public ICommand SendLocationCommand { get; }
    ```

5. Aggiungere un costruttore alla classe e in questo costruttore inizializzare `SendLocationCommand` come nuovo `Command` dallo spazio dei nomi `Xamarin.Forms`. Il costruttore per questo comando impiega `Action` per l'esecuzione quando il comando viene richiamato, quindi creare un metodo `async` denominato `SendLocation` e passare una funzione lambda che `await` questa chiamata al costruttore. Il corpo del metodo `SendLocation` verrà implementato nelle unità successive in questo modulo. È necessario aggiungere una direttiva utilizzo per lo spazio dei nomi `System.Threading.Tasks` perché venga restituito un `Task`.

    ```cs
    public MainViewModel()
    {
        SendLocationCommand = new Command(async () => await SendLocation());
    }

    async Task SendLocation()
    {
    }
    ```

Di seguito è riportato il codice per questa classe.

```cs
using System.Threading.Tasks;
using System.Windows.Input;
using Xamarin.Forms;

namespace ImHere
{
    public class MainViewModel : BaseViewModel
    {
        string message = "";
        public string Message
        {
            get => message;
            set => Set(ref message, value);
        }

        string phoneNumbers = "";
        public string PhoneNumbers
        {
            get => phoneNumbers;
            set => Set(ref phoneNumbers, value);
        }

        public MainViewModel()
        {
            SendLocationCommand = new Command(async () => await SendLocation());
        }

        public ICommand SendLocationCommand { get; }

        async Task SendLocation()
        {
        }
    }
}
```

## <a name="create-the-user-interface"></a>Creare l'interfaccia utente

Le interfacce utente Xamarin.Forms possono essere create tramite XAML.

1. Aprire il file `MainPage.xaml` dal progetto `ImHere`. La pagina verrà aperta nell'editor XAML.

    NOTA: il progetto `ImHere.UWP` contiene anche un file denominato `MainPage.xaml`. Assicurarsi di stare modificando quello nella libreria standard .NET.

2. Prima di poter associare controlli alle proprietà di un ViewModel, è necessario impostare un'istanza del ViewModel come contesto di associazione della pagina. Aggiungere il codice XAML seguente all'interno del `ContentPage` di primo livello.

    ```xml
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    ```

3. Eliminare il contenuto del `StackLayout` e aggiungere una spaziatura interna per migliorare l'aspetto dell'interfaccia utente.

    ```xml
    <StackLayout Padding="20">
    </StackLayout>
    ```

4. Aggiungere un controllo `Editor` che l'utente può usare per aggiungere i numeri di telefono al `StackLayout`, con un `Label` precedente per descrivere la funzione del controllo di immissione. `StackLayout` distribuisce i controlli figlio in senso orizzontale o verticale nell'ordine in cui vengono aggiunti i controlli, pertanto aggiungendo prima `Label`, questo verrà inserito prima di `Editor`. I controlli `Editor` sono controlli voce su più righe che consentono all'utente di immettere più numeri di telefono, uno per riga.

    ```xml
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Center"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
    </StackLayout>
    ```

    La proprietà `Text` su `Editor` è associata alla proprietà `PhoneNumbers` su `MainViewModel`. La sintassi per l'associazione consiste nell'impostare il valore della proprietà su `"{Binding <property name>}"`. Le parentesi graffe indicheranno al compilatore XAML che questo valore è speciale e deve essere trattato in modo diverso da un semplice `string`.

5. Aggiungere un `Button` per inviare la posizione dell'utente sotto `Editor`.

    ```xml
    <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
            Command="{Binding SendLocationCommand}" />
    ```

    La proprietà `Command` è associata al comando `SendLocationCommand` nel ViewModel. Quando viene toccato il pulsante, verrà eseguito il comando.

6. Aggiungere un `Label` per visualizzare il messaggio di stato sotto il `Button`.

    ```xml
    <Label Text="{Binding Message}"
           HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    ```

Di seguito è riportato il codice completo per questa pagina.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ImHere"
             x:Class="ImHere.MainPage">
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Center"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
        <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
                Command="{Binding SendLocationCommand}" />
        <Label Text="{Binding Message}"
               HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Eseguire l'app per visualizzare la nuova interfaccia utente. Se, a questo punto, si intende convalidare l'associazione, è possibile farlo mediante l'aggiunta di punti di interruzione alle proprietà o al metodo `SendLocation`.

![La nuova interfaccia utente dell'app](../media/3-new-ui.png)

## <a name="summary"></a>Summary

In questa unità è stato descritto come creare l'interfaccia utente per le tramite XAML, insieme a un ViewModel per gestire la logica delle applicazioni. Inoltre si è appreso come associare un ViewModel all'interfaccia utente. Nell'unità successiva verrà descritto come aggiungere la ricerca posizioni al ViewModel.