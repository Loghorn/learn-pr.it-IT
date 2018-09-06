<span data-ttu-id="b8f9c-101">Ora che l'app Web è stata correttamente creata e pubblicata in Azure, cosa occorre fare se si vuole apportarvi modifiche?</span><span class="sxs-lookup"><span data-stu-id="b8f9c-101">You've successfully created your web app and published it to Azure, but what happens when you want to make changes?</span></span> <span data-ttu-id="b8f9c-102">Visual Studio tiene traccia della posizione di pubblicazione dell'app, quindi per aggiornarla e modificarla bastano due clic.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-102">Visual Studio will remember where the app is published, which makes updating and changing your app a two-click process.</span></span>

## <a name="explore-the-project-structure"></a><span data-ttu-id="b8f9c-103">Esplorare la struttura del progetto</span><span class="sxs-lookup"><span data-stu-id="b8f9c-103">Explore the project structure</span></span>

<span data-ttu-id="b8f9c-104">Abbiamo creato un'app Web ASP.NET in Visual Studio e ora è necessario modificare e personalizzare il sito Web.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-104">We've created an ASP.NET web app in Visual Studio, and now you will need to edit and customize your website.</span></span> <span data-ttu-id="b8f9c-105">Esploriamo la struttura del progetto per vedere quali elementi sono stati creati da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-105">Let's explore the project structure to see what Visual Studio has created for us.</span></span>

### <a name="dependencies"></a><span data-ttu-id="b8f9c-106">Dependencies</span><span class="sxs-lookup"><span data-stu-id="b8f9c-106">Dependencies</span></span>

<span data-ttu-id="b8f9c-107">Le dipendenze includono gli elementi interni ASP.NET che consentono il corretto funzionamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-107">Dependencies include the ASP.NET internals to get your app up and running.</span></span> <span data-ttu-id="b8f9c-108">A meno che non si vogliano aggiungere pacchetti di terze parti, questa cartella non richiede una particolare attenzione.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-108">Unless you are adding specific third-party packages, you won't need to spend much time in this folder.</span></span>

### <a name="properties"></a><span data-ttu-id="b8f9c-109">Properties</span><span class="sxs-lookup"><span data-stu-id="b8f9c-109">Properties</span></span>

<span data-ttu-id="b8f9c-110">La cartella Properties contiene i dati di configurazione per la posizione in cui è ospitata l'app Web.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-110">The properties folder contains configuration data for where you are hosting your web app.</span></span> <span data-ttu-id="b8f9c-111">Se si espande ora la cartella **PublishProfiles**, di dovrebbe vedere l'URL del sito Alpine Ski Hill.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-111">If you expand the **PublishProfiles** folder now, you should see the URL for the Alpine Ski Hill site.</span></span> <span data-ttu-id="b8f9c-112">Ogni profilo di pubblicazione è un file XML che contiene le informazioni di configurazione della pubblicazione, come l'indirizzo di Azure usato da Visual Studio per caricare i file.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-112">Each publishing profile is an .xml file containing publishing configuration information such as the Azure address that Visual Studio uses to upload your files.</span></span>

### <a name="wwwroot"></a><span data-ttu-id="b8f9c-113">wwwroot</span><span class="sxs-lookup"><span data-stu-id="b8f9c-113">wwwroot</span></span>

<span data-ttu-id="b8f9c-114">Il file wwwroot contiene tutti gli asset statici per il sito, come i file css, js, lib e i file di immagine.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-114">The wwwroot file contains all of your static assets for your site, such as the .css, .js, images, and .lib files.</span></span> <span data-ttu-id="b8f9c-115">Questo file viene usato per applicare uno stile e aggiungere altre funzionalità al sito.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-115">When you are ready to style and add more functionality to your site, you will work in here.</span></span>

### <a name="pages"></a><span data-ttu-id="b8f9c-116">Pages</span><span class="sxs-lookup"><span data-stu-id="b8f9c-116">Pages</span></span>

<span data-ttu-id="b8f9c-117">La cartella **Pages** include i modelli _**Razor**_ per le pagine del sito.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-117">The **Pages** folder includes _**Razor**_ templates for the pages of your site.</span></span>
<span data-ttu-id="b8f9c-118">Razor è una sintassi basata su HTML, che prevede convenzioni speciali per la visualizzazione dei dati e l'esecuzione della logica sul sito.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-118">Razor is a syntax that is built up around HTML, which has special conventions for displaying data and executing logic on your site.</span></span>

<span data-ttu-id="b8f9c-119">Ogni pagina del sito è rappresentata con due file di codice:</span><span class="sxs-lookup"><span data-stu-id="b8f9c-119">Each page in your site is represented with two code files:</span></span>

- <span data-ttu-id="b8f9c-120">il primo è un file `.cshtml`, che è il file di markup Razor.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-120">The first is a `.cshtml` file, which is the Razor markup file.</span></span> <span data-ttu-id="b8f9c-121">Include l'HTML visualizzato e alcuni elementi della logica C#.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-121">This file includes your display HTML and some C# logic.</span></span>

- <span data-ttu-id="b8f9c-122">Il secondo file è un file `.cs`, ossia il code-behind C# che include la classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-122">The second file is is a `.cs` file, which is the C# code-behind that includes `PageModel` class.</span></span> <span data-ttu-id="b8f9c-123">Questo file consente di intercettare le richieste HTTP e di elaborarle in qualche modo prima di passare qualsiasi dato al file Razor.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-123">This file allows you to intercept HTTP requests and do some processing on that request before passing off any data to the Razor file.</span></span>

### <a name="appsettingjson"></a><span data-ttu-id="b8f9c-124">appsetting.json</span><span class="sxs-lookup"><span data-stu-id="b8f9c-124">appsetting.json</span></span>

<span data-ttu-id="b8f9c-125">Questo è un file di configurazione per ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-125">This is a configuration file for ASP.NET.</span></span>

### <a name="bundleconfigjson"></a><span data-ttu-id="b8f9c-126">bundleconfig.json</span><span class="sxs-lookup"><span data-stu-id="b8f9c-126">bundleconfig.json</span></span>

<span data-ttu-id="b8f9c-127">Il file bundleconfig.json gestisce la preelaborazione della configurazione.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-127">The bundleconfig.json is preprocessing configuration.</span></span> <span data-ttu-id="b8f9c-128">Riduce le dimensioni dei file css e js al momento della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-128">This file is making your .css and .js files smaller when they are published.</span></span>

### <a name="programcs-and-startupcs"></a><span data-ttu-id="b8f9c-129">Program.cs e Startup.cs</span><span class="sxs-lookup"><span data-stu-id="b8f9c-129">Program.cs and Startup.cs</span></span>

<span data-ttu-id="b8f9c-130">Program.cs e Startup.cs configurano e avviano l'host Web per il sito.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-130">Program.cs and Startup.cs configure and launch your web host for your site.</span></span>

## <a name="updating-your-website-using-razor"></a><span data-ttu-id="b8f9c-131">Aggiornamento del sito Web tramite Razor</span><span class="sxs-lookup"><span data-stu-id="b8f9c-131">Updating your website using Razor</span></span>

<span data-ttu-id="b8f9c-132">Per apportare alcune modifiche di base al sito Web,</span><span class="sxs-lookup"><span data-stu-id="b8f9c-132">We will want to make some basic changes to our website.</span></span> <span data-ttu-id="b8f9c-133">è importante avere una conoscenza di base dell'uso dei modelli Razor per personalizzare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-133">In order to do this, you will need to have a basic understanding of how to leverage the Razor templates to customize your web app.</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="b8f9c-134">Che cos'è Razor?</span><span class="sxs-lookup"><span data-stu-id="b8f9c-134">What is Razor?</span></span>

<span data-ttu-id="b8f9c-135">Razor è una sintassi ASP.NET che viene usata per creare pagine Web dinamiche con C#.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-135">Razor is an ASP.NET syntax used to create dynamic web pages with C#.</span></span> <span data-ttu-id="b8f9c-136">Quando un server legge una pagina Razor, il codice C# viene eseguito prima che il server esegua il rendering dell'HTML.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-136">When a server reads a Razor page, the C# code is run before it renders the HTML.</span></span> <span data-ttu-id="b8f9c-137">Questo permette di generare contenuto dinamico rapidamente.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-137">This allows you to generate dynamic content quickly.</span></span>

<span data-ttu-id="b8f9c-138">Razor usa direttive `@` per indicare ad ASP.NET come elaborare una pagina.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-138">Razor uses `@` directives to tell ASP.NET how to process a page.</span></span>

<span data-ttu-id="b8f9c-139">Vediamo ad esempio il codice nella pagina `Contact.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-139">For example, take a look at the code in the `Contact.cshtml` page.</span></span>

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

<span data-ttu-id="b8f9c-140">Ad esempio, la direttiva `@page` indica ad ASP.NET di elaborare questo file come una pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-140">For example: The `@page` directive is telling ASP.NET to process this file as a Razor page.</span></span>
<span data-ttu-id="b8f9c-141">La direttiva `@model` indica ad ASP.NET di collegare questa pagina Razor a una classe C# denominata `ContactModel`.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-141">The `@model` directive is telling ASP.NET to tie this Razor page with a C# class called `ContactModel`.</span></span>

<span data-ttu-id="b8f9c-142">Razor usa anche il simbolo `@` per eseguire la transizione tra il codice e HTML.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-142">Razor also uses the `@` symbol to transition between code and HTML.</span></span>
<span data-ttu-id="b8f9c-143">Si noti ad esempio la parte `@{ ... }` nel frammento di codice riportato sopra.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-143">For example, if you look at the snippet above, you'll notice `@{ ... }`.</span></span> <span data-ttu-id="b8f9c-144">È un **blocco di codice Razor**.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-144">This is a **Razor code block**.</span></span> <span data-ttu-id="b8f9c-145">È codice che viene _eseguito, ma senza rendering_.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-145">It's code which is _executed but not rendered_.</span></span>

<span data-ttu-id="b8f9c-146">Per eseguire il rendering dell'output di un'istruzione di codice, si usa `@` davanti a un'espressione C#.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-146">To render the output of a code statement, we use the `@` in front of a C# expression.</span></span> <span data-ttu-id="b8f9c-147">Ne abbiamo due esempi nel blocco di codice riportato sopra nei tag `<h2>` e `<h3>`.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-147">We have two examples of that in the code block above in the `<h2>` and `<h3>` tags.</span></span>

## <a name="publish-your-updates"></a><span data-ttu-id="b8f9c-148">Pubblicare gli aggiornamenti</span><span class="sxs-lookup"><span data-stu-id="b8f9c-148">Publish your updates</span></span>

<span data-ttu-id="b8f9c-149">Dopo aver apportato le modifiche al sito Web, è possibile pubblicarle in Azure.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-149">Once you've made the changes to your website, you will want to publish them to Azure.</span></span> <span data-ttu-id="b8f9c-150">Questo processo è simile a quello di pubblicazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-150">This process is similar to how we initially published.</span></span>

1. <span data-ttu-id="b8f9c-151">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-151">Right-click the project in Solution Explorer.</span></span>

1. <span data-ttu-id="b8f9c-152">Dovrebbe essere visualizzato il nome del sito Web seguito da Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-152">Now you should see the name of your website followed by Web Deploy.</span></span> <span data-ttu-id="b8f9c-153">Ad esempio, se si è assegnato al sito Web il nome AlpineSkiHouse42, si dovrebbe vedere **AlpineSkiHouse42 - Distribuzione Web** nelle opzioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-153">For example if you named your website AlpineSkiHouse42, you would see **AlpineSkiHouse42 - Web Deploy** in the available options.</span></span> <span data-ttu-id="b8f9c-154">Selezionare questo nome per aggiornare il sito in Azure.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-154">Select that and your site will update in Azure.</span></span>

## <a name="summary"></a><span data-ttu-id="b8f9c-155">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="b8f9c-155">Summary</span></span>

<span data-ttu-id="b8f9c-156">La creazione e la pubblicazione di un sito Web sono solo le prime fasi del processo di creazione di un buon sito Web.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-156">Creating and publishing a website are just the first steps in creating a good website.</span></span> <span data-ttu-id="b8f9c-157">Dopo aver iniziato ad aggiungere contenuti, sarà necessario aggiornare il sito.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-157">Once you start to add content, you'll need to update your site.</span></span> <span data-ttu-id="b8f9c-158">Dopo la pubblicazione iniziale del sito in Azure, è possibile aggiornarlo in qualsiasi momento da Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="b8f9c-158">Once you've initially published your site to Azure, you can update at any time from the Solution Explorer.</span></span>
