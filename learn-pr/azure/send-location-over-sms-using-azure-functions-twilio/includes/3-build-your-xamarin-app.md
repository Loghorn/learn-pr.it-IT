<span data-ttu-id="56110-101">A questo punto, l'app per dispositivi mobili è una semplice app "Hello World".</span><span class="sxs-lookup"><span data-stu-id="56110-101">At this point, the mobile app is a simple "Hello World" app.</span></span> <span data-ttu-id="56110-102">In questa unità si aggiungerà l'interfaccia utente e una logica dell'applicazione di base.</span><span class="sxs-lookup"><span data-stu-id="56110-102">In this unit, you add the UI and some basic application logic.</span></span>

<span data-ttu-id="56110-103">L'interfaccia utente per l'app sarà costituita da:</span><span class="sxs-lookup"><span data-stu-id="56110-103">The UI for the app will consist of:</span></span>

- <span data-ttu-id="56110-104">Un controllo voce di testo per immettere alcuni numeri di telefono.</span><span class="sxs-lookup"><span data-stu-id="56110-104">A text-entry control to enter some phone numbers.</span></span>
- <span data-ttu-id="56110-105">Un pulsante per inviare la posizione a questi numeri usando una funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="56110-105">A button to send your location to those numbers using an Azure function.</span></span>
- <span data-ttu-id="56110-106">Un'etichetta che visualizzerà un messaggio all'utente indicante lo stato corrente, ad esempio, il percorso inviato e l'invio corretto del percorso.</span><span class="sxs-lookup"><span data-stu-id="56110-106">A label that will show a message to the user of the current status, such as the location being sent and location sent successfully.</span></span>

<span data-ttu-id="56110-107">Xamarin.Forms supporta uno schema progettuale denominato MVVM (Model-View-ViewModel).</span><span class="sxs-lookup"><span data-stu-id="56110-107">Xamarin.Forms supports a design pattern called Model-View-ViewModel (MVVM).</span></span> <span data-ttu-id="56110-108">Altre informazioni su MVVM vedere la [documentazione di Xamarin MVVM](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm?azure-portal=true), ma la logica essenziale è che ogni pagina (visualizzazione) contiene un elemento ViewModel che espone le proprietà e il comportamento.</span><span class="sxs-lookup"><span data-stu-id="56110-108">You can read more about MVVM in the [Xamarin MVVM docs](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm?azure-portal=true), but the essence of it is, each page (View) has a ViewModel that exposes properties and behavior.</span></span>

<span data-ttu-id="56110-109">Le proprietà ViewModel sono associate a componenti nell'interfaccia utente in base al nome e questa associazione consente di sincronizzare dati tra View e ViewModel.</span><span class="sxs-lookup"><span data-stu-id="56110-109">ViewModel properties are 'bound' to components on the UI by name, and this binding synchronizes data between the View and ViewModel.</span></span> <span data-ttu-id="56110-110">Ad esempio, una proprietà `string` in un ViewModel denominato `Name` può essere associata alla proprietà `Text` di un controllo voce di testo nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="56110-110">For example, a `string` property on a ViewModel called `Name` could be bound to the `Text` property of a text-entry control on the UI.</span></span> <span data-ttu-id="56110-111">Il controllo voce di testo Mostra il valore di `Name` proprietà e, quando l'utente modifica il testo nell'interfaccia utente, il `Name` proprietà viene aggiornata.</span><span class="sxs-lookup"><span data-stu-id="56110-111">The text-entry control shows the value in the `Name` property and, when the user changes the text in the UI, the `Name` property is updated.</span></span> <span data-ttu-id="56110-112">Se il valore della proprietà `Name` viene modificato in ViewModel, viene generato un evento per indicare all'interfaccia utente da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="56110-112">If the value of the `Name` property is changed in the ViewModel, an event is raised to tell the UI to update.</span></span>

<span data-ttu-id="56110-113">Il comportamento di ViewModel viene esposto come proprietà dei comandi, dove un comando è un oggetto che esegue il wrapping di un'azione che viene eseguita quando il comando viene richiamato.</span><span class="sxs-lookup"><span data-stu-id="56110-113">ViewModel behavior is exposed as command properties, a command being an object that wraps an action that is executed when the command is invoked.</span></span> <span data-ttu-id="56110-114">Questi comandi sono associati in base al nome a controlli quali pulsanti e toccando un pulsante viene richiamato il comando.</span><span class="sxs-lookup"><span data-stu-id="56110-114">These commands are bound by name to controls like buttons, and tapping a button will invoke the command.</span></span>

## <a name="create-a-base-viewmodel"></a><span data-ttu-id="56110-115">Creare un'immagine di ViewModel</span><span class="sxs-lookup"><span data-stu-id="56110-115">Create a base ViewModel</span></span>

<span data-ttu-id="56110-116">Tutti i ViewModel implementano l'interfaccia `INotifyPropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="56110-116">ViewModels all implement the `INotifyPropertyChanged` interface.</span></span> <span data-ttu-id="56110-117">Questa interfaccia dispone di un singolo evento, `PropertyChanged`, che viene usato per notificare all'interfaccia utente eventuali aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="56110-117">This interface has a single event, `PropertyChanged`, which is used to notify the UI of any updates.</span></span> <span data-ttu-id="56110-118">Questo evento ha argomenti dell'evento che contengono il nome della proprietà che è stata modificata.</span><span class="sxs-lookup"><span data-stu-id="56110-118">This event has event args that contain the name of the property that has changed.</span></span> <span data-ttu-id="56110-119">È pratica comune creare una classe ViewModel di base che implementa questa interfaccia e fornisce alcuni metodi di supporto.</span><span class="sxs-lookup"><span data-stu-id="56110-119">It's common practice to create a base ViewModel class implementing this interface and providing some helper methods.</span></span>

1. <span data-ttu-id="56110-120">Creare una nuova classe nel progetto .NET `ImHere` Standard denominato `BaseViewModel` facendo clic con il pulsante destro del mouse sul progetto e quindi selezionando *Aggiungi -> Classe...* . Assegnare un nome alla nuova classe "BaseViewModel" e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="56110-120">Create a new class in the `ImHere` .NET Standard project called `BaseViewModel` by right-clicking on the project, and then selecting *Add->Class...*. Name the new class "BaseViewModel" and click **Add**.</span></span>

1. <span data-ttu-id="56110-121">Rendere la classe `public` e derivarla da `INotifyPropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="56110-121">Make the class `public` and derive from `INotifyPropertyChanged`.</span></span> <span data-ttu-id="56110-122">È necessario aggiungere una direttiva utilizzo per `System.ComponentModel`.</span><span class="sxs-lookup"><span data-stu-id="56110-122">You'll need to add a using directive for `System.ComponentModel`.</span></span>

1. <span data-ttu-id="56110-123">Implementare l'interfaccia `INotifyPropertyChanged` aggiungendo l'evento `PropertyChanged`:</span><span class="sxs-lookup"><span data-stu-id="56110-123">Implement the `INotifyPropertyChanged` interface by adding the `PropertyChanged` event:</span></span>

    ```cs
    public event PropertyChangedEventHandler PropertyChanged;
    ```

1. <span data-ttu-id="56110-124">Il modello comune per le proprietà ViewModel è disporre di una proprietà pubblica con un campo sottostante privato.</span><span class="sxs-lookup"><span data-stu-id="56110-124">The common pattern for ViewModel properties is to have a public property with a private backing field.</span></span> <span data-ttu-id="56110-125">Nel setter proprietà, il campo sottostante viene confrontato con il nuovo valore.</span><span class="sxs-lookup"><span data-stu-id="56110-125">In the property setter, the backing field is checked against the new value.</span></span> <span data-ttu-id="56110-126">Se il nuovo valore è diverso dal campo sottostante, il campo sottostante viene aggiornato e viene generato l'evento `PropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="56110-126">If the new value is different to the backing field, the backing field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="56110-127">Questa logica è facile da prendere in considerazione in un metodo, quindi aggiungere il metodo `Set`.</span><span class="sxs-lookup"><span data-stu-id="56110-127">This logic is easy to factor out into a method, so add the `Set` method.</span></span> <span data-ttu-id="56110-128">È necessario aggiungere una direttiva utilizzo per lo spazio dei nomi `System.Runtime.CompilerServices`.</span><span class="sxs-lookup"><span data-stu-id="56110-128">You'll need to add a using directive for the `System.Runtime.CompilerServices` namespace.</span></span>

    ```cs
    protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(field, value)) return;
        field = value;
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    ```

    <span data-ttu-id="56110-129">Questo metodo fa riferimento al campo sottostante, a nuovo valore e al nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="56110-129">This method takes a reference to the backing field, the new value, and the property name.</span></span> <span data-ttu-id="56110-130">Se il campo non è stato modificato, il metodo restituisce, in caso contrario, il campo viene aggiornato e viene generato l'evento `PropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="56110-130">If the field hasn't changed, the method returns, otherwise, the field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="56110-131">Il parametro `propertyName` nel metodo `Set` è un parametro predefinito ed è contrassegnato con l'attributo `CallerMemberName`.</span><span class="sxs-lookup"><span data-stu-id="56110-131">The `propertyName` parameter on the `Set` method is a default parameter and is marked with the `CallerMemberName` attribute.</span></span> <span data-ttu-id="56110-132">Quando questo metodo viene chiamato da un setter proprietà, in genere questo parametro viene lasciato come valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="56110-132">When this method is called from a property setter, this parameter is normally left as the default value.</span></span> <span data-ttu-id="56110-133">Il compilatore quindi imposterà automaticamente il valore del parametro sul nome della proprietà chiamata.</span><span class="sxs-lookup"><span data-stu-id="56110-133">The compiler will then automatically set the parameter value to be the name of the calling property.</span></span>

<span data-ttu-id="56110-134">Di seguito è riportato il codice completo per questa classe.</span><span class="sxs-lookup"><span data-stu-id="56110-134">The full code for this class is below.</span></span>

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

## <a name="create-a-viewmodel-for-the-page"></a><span data-ttu-id="56110-135">Creare un ViewModel per la pagina</span><span class="sxs-lookup"><span data-stu-id="56110-135">Create a ViewModel for the page</span></span>

<span data-ttu-id="56110-136">Il `MainPage` disporrà di un controllo di immissione del testo per i numeri di telefono e di un'etichetta per visualizzare un messaggio.</span><span class="sxs-lookup"><span data-stu-id="56110-136">The `MainPage` will have a text-entry control for phone numbers and a label to display a message.</span></span> <span data-ttu-id="56110-137">Questi controlli verranno associati alle proprietà in un ViewModel.</span><span class="sxs-lookup"><span data-stu-id="56110-137">These controls will be bound to properties on a ViewModel.</span></span>

1. <span data-ttu-id="56110-138">Creare una classe denominata `MainViewModel` nel progetto .NET Standard `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="56110-138">Create a class called `MainViewModel` in the `ImHere` .NET Standard project.</span></span>

1. <span data-ttu-id="56110-139">Rendere pubblica la classe e derivarla da `BaseViewModel`.</span><span class="sxs-lookup"><span data-stu-id="56110-139">Make this class public and derive from `BaseViewModel`.</span></span>

1. <span data-ttu-id="56110-140">Aggiungere due proprietà `string`, `PhoneNumbers` e `Message`, ciascuna con un campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="56110-140">Add two `string` properties, `PhoneNumbers` and `Message`, each with a backing field.</span></span> <span data-ttu-id="56110-141">Nel setter proprietà, usare il metodo `Set` della classe di base per aggiornare il valore e generare l'evento `PropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="56110-141">In the property setter, use the base class `Set` method to update the value and raise the `PropertyChanged` event.</span></span>

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

1. <span data-ttu-id="56110-142">Aggiungere una proprietà comando di sola lettura denominata `SendLocationCommand`.</span><span class="sxs-lookup"><span data-stu-id="56110-142">Add a read-only command property called `SendLocationCommand`.</span></span> <span data-ttu-id="56110-143">Questo comando avrà un tipo di `ICommand` dallo spazio dei nomi `System.Windows.Input`.</span><span class="sxs-lookup"><span data-stu-id="56110-143">This command will have a type of `ICommand` from the `System.Windows.Input` namespace.</span></span>

    ```cs
    public ICommand SendLocationCommand { get; }
    ```

1. <span data-ttu-id="56110-144">Aggiungere un costruttore alla classe e in questo costruttore inizializzare `SendLocationCommand` come nuovo Xamarin.Forms `Command`.</span><span class="sxs-lookup"><span data-stu-id="56110-144">Add a constructor to the class, and in this constructor, initialize the `SendLocationCommand` as a new Xamarin.Forms `Command`.</span></span> <span data-ttu-id="56110-145">È necessario aggiungere una direttiva utilizzo per lo spazio dei nomi `Xamarin.Forms`.</span><span class="sxs-lookup"><span data-stu-id="56110-145">You will need to add a using directive for the `Xamarin.Forms` namespace.</span></span> <span data-ttu-id="56110-146">Il costruttore per questo comando impiega `Action` per l'esecuzione quando il comando viene richiamato; quindi creare un metodo `async` denominato `SendLocation` e passare una funzione lambda che `await` questa chiamata al costruttore.</span><span class="sxs-lookup"><span data-stu-id="56110-146">The constructor for this command takes an `Action` to run when the command is invoked, so create an `async` method called `SendLocation` and pass a lambda function that `await`s this call to the constructor.</span></span> <span data-ttu-id="56110-147">Il corpo del metodo `SendLocation` verrà implementato nelle unità successive in questo modulo.</span><span class="sxs-lookup"><span data-stu-id="56110-147">The body of the `SendLocation` method will be implemented in later units in this module.</span></span> <span data-ttu-id="56110-148">È necessario aggiungere una direttiva utilizzo per lo spazio dei nomi `System.Threading.Tasks` perché venga restituito un `Task`.</span><span class="sxs-lookup"><span data-stu-id="56110-148">You'll need to add a using directive for the `System.Threading.Tasks` namespace to be able to return a `Task`.</span></span>

    ```cs
    public MainViewModel()
    {
        SendLocationCommand = new Command(async () => await SendLocation());
    }

    async Task SendLocation()
    {
    }
    ```

<span data-ttu-id="56110-149">Di seguito è riportato il codice per questa classe.</span><span class="sxs-lookup"><span data-stu-id="56110-149">The code for this class is shown below.</span></span>

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

## <a name="create-the-user-interface"></a><span data-ttu-id="56110-150">Creare l'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="56110-150">Create the user interface</span></span>

<span data-ttu-id="56110-151">Le interfacce utente Xamarin.Forms possono essere create tramite XAML.</span><span class="sxs-lookup"><span data-stu-id="56110-151">Xamarin.Forms UIs can be built using XAML.</span></span>

1. <span data-ttu-id="56110-152">Aprire il file `MainPage.xaml` dal progetto `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="56110-152">Open the `MainPage.xaml` file from the `ImHere` project.</span></span> <span data-ttu-id="56110-153">La pagina verrà aperta nell'editor XAML.</span><span class="sxs-lookup"><span data-stu-id="56110-153">The page will open in the XAML editor.</span></span>

    > [!NOTE]
    >  <span data-ttu-id="56110-154">Il progetto `ImHere.UWP` contiene anche un file denominato `MainPage.xaml`.</span><span class="sxs-lookup"><span data-stu-id="56110-154">The `ImHere.UWP` project also contains a file called `MainPage.xaml`.</span></span> <span data-ttu-id="56110-155">Assicurarsi di modificare tale file nella libreria .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="56110-155">Make sure you're editing the one in the .NET Standard library.</span></span>

1. <span data-ttu-id="56110-156">Prima di poter associare controlli alle proprietà di un ViewModel, è necessario impostare un'istanza del ViewModel come contesto di associazione della pagina.</span><span class="sxs-lookup"><span data-stu-id="56110-156">Before you can bind controls to properties on a ViewModel, you have to set an instance of the ViewModel as the binding context of the page.</span></span> <span data-ttu-id="56110-157">Aggiungere il codice XAML seguente all'interno del `ContentPage` di primo livello.</span><span class="sxs-lookup"><span data-stu-id="56110-157">Add the following XAML inside the top-level `ContentPage`.</span></span>

    ```xml
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    ```

1. <span data-ttu-id="56110-158">Sovrascrivere `StackLayout` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="56110-158">Overwrite the `StackLayout` with the following code:</span></span>

     ```xml
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Start"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
    </StackLayout>
    ```
    - <span data-ttu-id="56110-159">Il controllo `Editor` verrà usato per aggiungere i numeri di telefono e `Label` riportato sopra descrive lo scopo di questo campo per l'utente.</span><span class="sxs-lookup"><span data-stu-id="56110-159">The `Editor` control will be used to add phone numbers, and the `Label` above describes the purpose of this field to the user.</span></span> 
    - <span data-ttu-id="56110-160">`StackLayout` distribuisce i controlli figlio in senso orizzontale o verticale nell'ordine in cui vengono aggiunti i controlli, pertanto aggiungendo prima `Label`, questo verrà inserito prima di `Editor`.</span><span class="sxs-lookup"><span data-stu-id="56110-160">`StackLayout`'s stack child controls either horizontally or vertically in the order in which the controls are added, so adding the `Label` first will put it above the `Editor`.</span></span>
    - <span data-ttu-id="56110-161">I controlli `Editor` sono controlli voce su più righe che consentono all'utente di immettere più numeri di telefono, uno per riga.</span><span class="sxs-lookup"><span data-stu-id="56110-161">`Editor` controls are multi-line entry controls, allowing the user to enter multiple phone numbers, one per line.</span></span>

   

    <span data-ttu-id="56110-162">La proprietà `Text` su `Editor` è associata alla proprietà `PhoneNumbers` su `MainViewModel`.</span><span class="sxs-lookup"><span data-stu-id="56110-162">The `Text` property on the `Editor` is bound to the `PhoneNumbers` property on the `MainViewModel`.</span></span> <span data-ttu-id="56110-163">La sintassi per l'associazione consiste nell'impostare il valore della proprietà su `"{Binding <property name>}"`.</span><span class="sxs-lookup"><span data-stu-id="56110-163">The syntax for binding is to set the property value to `"{Binding <property name>}"`.</span></span> <span data-ttu-id="56110-164">Le parentesi graffe indicheranno al compilatore XAML che questo valore è speciale e deve essere trattato in modo diverso da un semplice `string`.</span><span class="sxs-lookup"><span data-stu-id="56110-164">The curly braces will tell the XAML compiler that this value is special and should be treated differently from a simple `string`.</span></span>

1. <span data-ttu-id="56110-165">Aggiungere un `Button` sotto al controllo `Editor`.</span><span class="sxs-lookup"><span data-stu-id="56110-165">Add a `Button` below the `Editor` control.</span></span> <span data-ttu-id="56110-166">Questo pulsante verrà usato per inviare la posizione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="56110-166">We'll use this button to send the user's location.</span></span>

    ```xml
    <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
            Command="{Binding SendLocationCommand}" />
    ```

    <span data-ttu-id="56110-167">La proprietà `Command` è associata al comando `SendLocationCommand` nel ViewModel.</span><span class="sxs-lookup"><span data-stu-id="56110-167">The `Command` property is bound to the `SendLocationCommand` command on the ViewModel.</span></span> <span data-ttu-id="56110-168">Quando viene toccato il pulsante, verrà eseguito il comando.</span><span class="sxs-lookup"><span data-stu-id="56110-168">When the button is tapped, the command will be executed.</span></span>

1. <span data-ttu-id="56110-169">Aggiungere un `Label` sotto al controllo `Button`.</span><span class="sxs-lookup"><span data-stu-id="56110-169">Add a `Label` below the `Button` control.</span></span> <span data-ttu-id="56110-170">I messaggi di stato verranno visualizzati in questa etichetta.</span><span class="sxs-lookup"><span data-stu-id="56110-170">We'll display status messages in this label.</span></span>

    ```xml
    <Label Text="{Binding Message}"
           HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    ```

    <span data-ttu-id="56110-171">Di seguito è riportato il codice completo per questa pagina.</span><span class="sxs-lookup"><span data-stu-id="56110-171">The full code for this page is below.</span></span>
    
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
            <Label Text="Phone numbers to send to:" HorizontalOptions="Start"/>
            <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
            <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
                    Command="{Binding SendLocationCommand}" />
            <Label Text="{Binding Message}"
                   HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    ```

1. <span data-ttu-id="56110-172">Eseguire l'app per visualizzare la nuova interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="56110-172">Run the app to see the new UI.</span></span> <span data-ttu-id="56110-173">Se, a questo punto, si intende convalidare l'associazione, è possibile farlo mediante l'aggiunta di punti di interruzione alle proprietà o al metodo `SendLocation`.</span><span class="sxs-lookup"><span data-stu-id="56110-173">If you want to validate the bindings at this point, you can do so by adding breakpoints to the properties or the `SendLocation` method.</span></span>

    > [!NOTE]
    > <span data-ttu-id="56110-174">Durante la compilazione dell'app, verrà visualizzato un avviso relativo alla `SendLocation` mancanza di modificatori `await`.</span><span class="sxs-lookup"><span data-stu-id="56110-174">When you compile this app, you will see a warning about `SendLocation` lacking `await` modifiers.</span></span> <span data-ttu-id="56110-175">È possibile ignorare questo avviso in quanto verrà risolto non appena verrà aggiunto altro codice a questo metodo nell'unità successiva.</span><span class="sxs-lookup"><span data-stu-id="56110-175">You can ignore this warning as this will be resolved once more code is added to this method in the next unit.</span></span>
    
    
    ![La nuova interfaccia utente dell'app](../media/3-new-ui.png)

## <a name="summary"></a><span data-ttu-id="56110-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="56110-177">Summary</span></span>

<span data-ttu-id="56110-178">In questa unità è stato descritto come creare l'interfaccia utente per le tramite XAML, insieme a un ViewModel per gestire la logica delle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="56110-178">In this unit, you learned how to create the UI for the app using XAML, along with a ViewModel to handle the applications logic.</span></span> <span data-ttu-id="56110-179">Inoltre si è appreso come associare un ViewModel all'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="56110-179">You also learned how to bind the ViewModel to the UI.</span></span> <span data-ttu-id="56110-180">Nell'unità successiva verrà descritto come aggiungere la ricerca posizioni al ViewModel.</span><span class="sxs-lookup"><span data-stu-id="56110-180">In the next unit, you add location lookup to the ViewModel.</span></span>