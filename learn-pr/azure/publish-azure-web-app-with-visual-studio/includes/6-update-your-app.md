Ora che l'app Web è stata correttamente creata e pubblicata in Azure, cosa occorre fare se si vuole apportarvi modifiche? Visual Studio tiene traccia della posizione di pubblicazione dell'app, quindi per aggiornarla e modificarla bastano due clic.

## <a name="explore-the-project-structure"></a>Esplorare la struttura del progetto

Abbiamo creato un'app Web ASP.NET in Visual Studio e ora è necessario modificare e personalizzare il sito Web. Esploriamo la struttura del progetto per vedere quali elementi sono stati creati da Visual Studio.

### <a name="dependencies"></a>Dependencies

Le dipendenze includono gli elementi interni ASP.NET che consentono il corretto funzionamento dell'app. A meno che non si vogliano aggiungere pacchetti di terze parti, questa cartella non richiede una particolare attenzione.

### <a name="properties"></a>Properties

La cartella Properties contiene i dati di configurazione per la posizione in cui è ospitata l'app Web. Se si espande ora la cartella **PublishProfiles**, di dovrebbe vedere l'URL del sito Alpine Ski Hill. Ogni profilo di pubblicazione è un file XML che contiene le informazioni di configurazione della pubblicazione, come l'indirizzo di Azure usato da Visual Studio per caricare i file.

### <a name="wwwroot"></a>wwwroot

Il file wwwroot contiene tutti gli asset statici per il sito, come i file css, js, lib e i file di immagine. Questo file viene usato per applicare uno stile e aggiungere altre funzionalità al sito.

### <a name="pages"></a>Pages

La cartella **Pages** include i modelli _**Razor**_ per le pagine del sito.
Razor è una sintassi basata su HTML, che prevede convenzioni speciali per la visualizzazione dei dati e l'esecuzione della logica sul sito.

Ogni pagina del sito è rappresentata con due file di codice:

- il primo è un file `.cshtml`, che è il file di markup Razor. Include l'HTML visualizzato e alcuni elementi della logica C#.

- Il secondo file è un file `.cs`, ossia il code-behind C# che include la classe `PageModel`. Questo file consente di intercettare le richieste HTTP e di elaborarle in qualche modo prima di passare qualsiasi dato al file Razor.

### <a name="appsettingjson"></a>appsetting.json

Questo è un file di configurazione per ASP.NET.

### <a name="bundleconfigjson"></a>bundleconfig.json

Il file bundleconfig.json gestisce la preelaborazione della configurazione. Riduce le dimensioni dei file css e js al momento della pubblicazione.

### <a name="programcs-and-startupcs"></a>Program.cs e Startup.cs

Program.cs e Startup.cs configurano e avviano l'host Web per il sito.

## <a name="updating-your-website-using-razor"></a>Aggiornamento del sito Web tramite Razor

Per apportare alcune modifiche di base al sito Web, è importante avere una conoscenza di base dell'uso dei modelli Razor per personalizzare l'app Web.

## <a name="what-is-razor"></a>Che cos'è Razor?

Razor è una sintassi ASP.NET che viene usata per creare pagine Web dinamiche con C#. Quando un server legge una pagina Razor, il codice C# viene eseguito prima che il server esegua il rendering dell'HTML. Questo permette di generare contenuto dinamico rapidamente.

Razor usa direttive `@` per indicare ad ASP.NET come elaborare una pagina.

Vediamo ad esempio il codice nella pagina `Contact.cshtml`:

```aspx-csharp
@page
@model ContactModel
@{
    ViewData["Title"] = "Contact";
}
<h2>@ViewData["Title"]</h2>
<h3>@Model.Message</h3>
...
```

- La direttiva `@page` indica ad ASP.NET di elaborare questo file come una pagina Razor.
- La direttiva `@model` indica ad ASP.NET di collegare questa pagina Razor a una classe C# denominata `ContactModel`.

Razor usa anche il simbolo `@` per eseguire la transizione tra il codice e HTML. Si noti la parte `@{ ... }` nel frammento di codice riportato sopra. È un **blocco di codice Razor**. È codice che viene _eseguito, ma senza rendering_.

Per eseguire il rendering dell'output di un'istruzione di codice, si usa `@` davanti a un'espressione C#. Ne abbiamo due esempi nel blocco di codice riportato sopra nei tag `<h2>` e `<h3>`.

## <a name="publish-your-updates"></a>Pubblicare gli aggiornamenti

Dopo aver apportato le modifiche al sito Web, è possibile pubblicarle in Azure. Questo processo è simile a quello di pubblicazione iniziale.

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.

1. Dovrebbe essere visualizzato il nome del sito Web seguito da Distribuzione Web. Se si è assegnato al sito Web il nome AlpineSkiHouse42, si dovrebbe vedere **AlpineSkiHouse42 - Distribuzione Web** nelle opzioni disponibili. Selezionare questo nome per aggiornare il sito in Azure.

## <a name="summary"></a>Riepilogo

La creazione e la pubblicazione di un sito Web sono solo le prime fasi del processo di creazione di un buon sito Web. Dopo aver iniziato ad aggiungere contenuti, sarà necessario aggiornare il sito. Dopo la pubblicazione iniziale del sito in Azure, è possibile aggiornarlo in qualsiasi momento da Esplora soluzioni.
