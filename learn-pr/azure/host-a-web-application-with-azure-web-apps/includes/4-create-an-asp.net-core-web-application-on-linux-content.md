<span data-ttu-id="cb19e-101">Si è deciso di utilizzare una tecnologia open source per la compilazione dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="cb19e-101">You've decided to use an open-source technology for building your web application.</span></span> <span data-ttu-id="cb19e-102">Si sa che ASP.NET Core è un framework open source e multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="cb19e-102">You know that ASP.NET Core is a cross-platform and open-source framework.</span></span> <span data-ttu-id="cb19e-103">Si decide di sviluppare l'app Web in ambiente Linux tramite ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb19e-103">You decide to develop your web app in a Linux environment using ASP.NET Core!</span></span>

<span data-ttu-id="cb19e-104">App Web in Azure consente di usare la propria tecnologia Web preferita.</span><span class="sxs-lookup"><span data-stu-id="cb19e-104">Web Apps in Azure allows you to use your favorite web technology.</span></span> <span data-ttu-id="cb19e-105">Sia che si abbia maggiore familiarità con Node.js, PHP o NET Core, App Web è la soluzione adatta.</span><span class="sxs-lookup"><span data-stu-id="cb19e-105">Whether you're most comfortable with Node.js, PHP, or .NET Core, Web Apps will work for you.</span></span>

<span data-ttu-id="cb19e-106">In questo caso, si creerà un'applicazione ASP.NET Core usando l'interfaccia della riga di comando di .NET (CLI).</span><span class="sxs-lookup"><span data-stu-id="cb19e-106">Here, you will create an ASP.NET Core application using the .NET command-line interface (CLI).</span></span>

## <a name="what-is-aspnet-core"></a><span data-ttu-id="cb19e-107">Che cos'è ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cb19e-107">What is ASP.NET Core?</span></span>

<span data-ttu-id="cb19e-108">ASP.NET Core è l'evoluzione più recente del famoso framework Web ASP.NET di Microsoft, un framework multipiattaforma, open source per la compilazione di applicazioni moderne, basate su cloud e connesse a Internet.</span><span class="sxs-lookup"><span data-stu-id="cb19e-108">ASP.NET Core is the latest evolution of Microsoft's popular ASP.NET web framework, a cross-platform, open-source framework for building modern, cloud-based, and internet-connected applications.</span></span>

<span data-ttu-id="cb19e-109">Le applicazioni ASP.NET Core possono essere scritte per avere come destinazione .NET Core Framework o la versione esistente e completa .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="cb19e-109">ASP.NET Core applications can be written to target the .NET Core Framework or the existing, full .NET Framework.</span></span>

<span data-ttu-id="cb19e-110">Poiché si tratta di un framework open source e multipiattaforma, è possibile compilare le app ASP.NET Core su una vasta gamma di piattaforme, tra cui Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="cb19e-110">Being a cross-platform and open-source framework, you can build ASP.NET Core apps on a variety of platforms, including Windows, macOS, and Linux.</span></span> <span data-ttu-id="cb19e-111">Microsoft offre, fino a questo momento, l'ambiente di sviluppo integrato di Visual Studio per gli ambienti Windows e macOS.</span><span class="sxs-lookup"><span data-stu-id="cb19e-111">Thus far, Microsoft offers the Visual Studio IDE for both Windows and macOS environments.</span></span> <span data-ttu-id="cb19e-112">Inoltre, l'editor di Visual Studio Code è multipiattaforma e compatibile con essi.</span><span class="sxs-lookup"><span data-stu-id="cb19e-112">In addition, the Visual Studio Code editor is cross-platform and compatible with them.</span></span>

><span data-ttu-id="cb19e-113">Per supportare la creazione di applicazioni ASP.NET Core su diverse piattaforme, Microsoft ha introdotto gli strumenti della riga di comando .NET Core che consentono di compilare, testare e pubblicare le applicazioni usando un set completo, coerente e multipiattaforma di API.</span><span class="sxs-lookup"><span data-stu-id="cb19e-113">To support building ASP.NET Core applications on different platforms, Microsoft introduced the .NET Core CLI tools to help you build, test, and publish your applications using a rich, consistent, and cross-platform set of APIs.</span></span>

<span data-ttu-id="cb19e-114">Grazie ad ASP.NET Core, è possibile compilare servizi e app Web, app IoT e back-end mobili.</span><span class="sxs-lookup"><span data-stu-id="cb19e-114">With ASP.NET Core, you can build web apps and services, IoT apps, and mobile back ends.</span></span> <span data-ttu-id="cb19e-115">Le applicazioni ASP.NET Core possono essere ospitate nel cloud oppure in locale.</span><span class="sxs-lookup"><span data-stu-id="cb19e-115">ASP.NET Core applications can be hosted either in the cloud or on-premises.</span></span>

<span data-ttu-id="cb19e-116">Per impostazione predefinita, ASP.NET Core è costituito da un server Web incorporato e da un ambiente di runtime che esegue il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cb19e-116">By design, ASP.NET Core consists of an embedded web server and a runtime environment that runs the application code.</span></span> <span data-ttu-id="cb19e-117">Il codice dell'applicazione viene scritto usando un framework di ASP.NET MVC rielaborato che si basa su moduli e pacchetti più piccoli.</span><span class="sxs-lookup"><span data-stu-id="cb19e-117">The application code is written using a reworked ASP.NET MVC framework that relies on smaller modules and packages.</span></span> <span data-ttu-id="cb19e-118">Il risultato è un progetto di applicazione Web più piccolo che è facile gestire e ospitare in ambienti cloud.</span><span class="sxs-lookup"><span data-stu-id="cb19e-118">The result is a smaller web application blueprint that is easy to maintain and host over cloud environments.</span></span> <span data-ttu-id="cb19e-119">La figura seguente mostra un'applicazione ASP.NET Core ospitata in .NET Core e il server Web esterno che gestisce il traffico HTTP Internet.</span><span class="sxs-lookup"><span data-stu-id="cb19e-119">The following illustration shows an ASP.NET Core application hosted in .NET Core and the external web server that handles internet http traffic.</span></span>

![Un'illustrazione mostra un'applicazione ASP.NET Core e il relativo ambiente di esecuzione.](../media-draft/4-asp-net-core-architecture.png)

<span data-ttu-id="cb19e-121">Le applicazioni ASP.NET Core sono applicazioni **console** autonome richiamate tramite lo strumento **dotnet**.</span><span class="sxs-lookup"><span data-stu-id="cb19e-121">ASP.NET Core applications are standalone **console** applications invoked through the **dotnet** driver tool.</span></span> <span data-ttu-id="cb19e-122">Le applicazioni ASP.NET Core non vengono caricate nel processo di lavoro IIS, ma caricate tramite un modulo IIS nativo chiamato **AspNetCoreModule** che esegue l'applicazione console esterna.</span><span class="sxs-lookup"><span data-stu-id="cb19e-122">ASP.NET Core applications are not loaded into the IIS worker process, but rather, are loaded through a native IIS module called **AspNetCoreModule** that executes the external console application.</span></span>

## <a name="how-to-create-an-aspnet-core-web-project"></a><span data-ttu-id="cb19e-123">Come creare un progetto Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cb19e-123">How to create an ASP.NET Core web project</span></span>

<span data-ttu-id="cb19e-124">Sono disponibili diverse opzioni per la creazione di un nuovo progetto ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="cb19e-124">There are a few options for creating a new ASP.NET Core project:</span></span>

- <span data-ttu-id="cb19e-125">È possibile usare i modelli di Visual Studio, versioni Windows e macOS, per generare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="cb19e-125">You can use Visual Studio (Windows and macOS versions) templates to generate a new project.</span></span> <span data-ttu-id="cb19e-126">Visual Studio offre un'ampia gamma di modelli che è possibile usare per creare progetti Web.</span><span class="sxs-lookup"><span data-stu-id="cb19e-126">Visual Studio offers a variety of templates that you can use to create web projects.</span></span> <span data-ttu-id="cb19e-127">Ad esempio, è possibile usare il modello **vuoto** per creare un progetto ASP.NET Core essenziale con la configurazione di base.</span><span class="sxs-lookup"><span data-stu-id="cb19e-127">For instance, you can use the **Empty** template to create a bare-bones ASP.NET Core project with the basic setup.</span></span> <span data-ttu-id="cb19e-128">Inoltre, è possibile usare il modello **Applicazione Web (finestra modale-View-Controller)** per generare un'applicazione ASP.NET Core MVC completa, con **controller** e **viste** di esempio che consentono di iniziare a scrivere il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cb19e-128">In addition, you can use the **Web Application (Modal-View-Controller)** template to generate a full-fledged ASP.NET Core MVC application, with sample **controllers** and **views** that can help you start coding your application.</span></span> <span data-ttu-id="cb19e-129">L'arrivo più recente è il modello di progetto **Applicazione Web** da usare per creare un progetto ASP.NET Core basato su Razor Pages anziché sulla struttura di progetto MVC tradizionale.</span><span class="sxs-lookup"><span data-stu-id="cb19e-129">The latest arrival is the **Web Application** project template that is used to create an ASP.NET Core project based on Razor pages and not on the traditional MVC project structure.</span></span>

- <span data-ttu-id="cb19e-130">È possibile usare gli strumenti dell'interfaccia della riga di comando di .NET Core per generare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb19e-130">You can use the .NET Core CLI tools to generate a new ASP.NET Core project.</span></span> <span data-ttu-id="cb19e-131">Microsoft gestisce un set di modelli di progetto quasi comuni di ASP.NET Core per Visual Studio e gli strumenti dell'interfaccia della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="cb19e-131">Microsoft maintains an almost common set of ASP.NET Core project templates for both Visual Studio and the CLI tools.</span></span> <span data-ttu-id="cb19e-132">L'unica differenza è che, per gli strumenti dell'interfaccia della riga di comando, è necessario digitare i comandi per creare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb19e-132">The only difference with the CLI tools is that you need to type commands to create a new ASP.NET Core project.</span></span>
> <span data-ttu-id="cb19e-133">Gli strumenti dell'interfaccia della riga di comando di .NET utilizzano il **motore di creazione modello** per supportare diversi modelli di progetto.</span><span class="sxs-lookup"><span data-stu-id="cb19e-133">The .NET CLI tools make use of the **templating engine** to support different project templates.</span></span>  <span data-ttu-id="cb19e-134">Per altre informazioni, visitare il repository GitHub per il [motore di creazione modello](https://github.com/dotnet/templating) usato internamente dagli strumenti dell'interfaccia della riga di comando di .NET.</span><span class="sxs-lookup"><span data-stu-id="cb19e-134">To learn more, visit the GitHub repository for the [templating engine](https://github.com/dotnet/templating) used internally by the .NET CLI tools.</span></span>

<span data-ttu-id="cb19e-135">I precedenti sono considerati gli strumenti principali per la generazione di progetti ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb19e-135">The above are considered the top tools for generating ASP.NET Core projects.</span></span> <span data-ttu-id="cb19e-136">Tuttavia, esistono altri strumenti che è possibile cercare ed esplorare.</span><span class="sxs-lookup"><span data-stu-id="cb19e-136">However, there are more tools out there that you can search for and explore.</span></span>

<span data-ttu-id="cb19e-137">Vale la pena sottolineare che i progetti generati con altri strumenti possono essere leggermente diversi, tuttavia, tutti generano progetti ASP.NET Core validi e ottimizzati.</span><span class="sxs-lookup"><span data-stu-id="cb19e-137">It's worth mentioning that the projects generated by the different tools can be slightly different, however, they all generate valid and optimized ASP.NET Core projects.</span></span>

## <a name="net-cli-tools"></a><span data-ttu-id="cb19e-138">Strumenti dell'interfaccia della riga di comando di .NET</span><span class="sxs-lookup"><span data-stu-id="cb19e-138">.NET CLI tools</span></span>

<span data-ttu-id="cb19e-139">Gli strumenti dell'interfaccia della riga di comando di .NET o .NET Core sono strumenti multipiattaforma che comprendono comandi per la creazione e il ripristino dei pacchetti e per la compilazione, l'esecuzione</span><span class="sxs-lookup"><span data-stu-id="cb19e-139">The .NET CLI tools, also known as .NET Core CLI, is a cross-platform tool that provides commands for creating and restoring packages, and for building, running.</span></span> <span data-ttu-id="cb19e-140">e la pubblicazione di applicazioni .NET dalla riga di comando senza la necessità di ricorrere a un ambiente di sviluppo integrato completo.</span><span class="sxs-lookup"><span data-stu-id="cb19e-140">and publishing .NET applications from the command line without the need for a full-featured IDE.</span></span>

<span data-ttu-id="cb19e-141">L'interfaccia della riga di comando di .NET viene installata come parte di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="cb19e-141">The .NET CLI is installed as part of the .NET Core SDK.</span></span> <span data-ttu-id="cb19e-142">Nello stesso computer possono coesistere più versioni dell'interfaccia della riga di comando che possono essere eseguire affiancate.</span><span class="sxs-lookup"><span data-stu-id="cb19e-142">Multiple versions of the CLI can coexist on the same machine and run side by side.</span></span>

<span data-ttu-id="cb19e-143">Per iniziare a usare l'interfaccia della riga di comando di .NET, è necessario installare .NET Core SDK rilevante per l'ambiente che si utilizza per sviluppare l'app: Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="cb19e-143">To start using the .NET CLI, you need to install the relevant .NET Core SDK with respect to the environment that you are using to develop your app: Windows, macOS, or Linux.</span></span> <span data-ttu-id="cb19e-144">Passare a [.NET Downloads](https://www.microsoft.com/net/download?initial-os=linux) e scaricare .NET Core SDK per Linux.</span><span class="sxs-lookup"><span data-stu-id="cb19e-144">Go to [.NET Downloads](https://www.microsoft.com/net/download?initial-os=linux) and grab the .NET Core SDK for Linux.</span></span> <span data-ttu-id="cb19e-145">Nell'esempio si utilizza un sistema operativo Ubuntu 18.04 in esecuzione su Windows 10 con VMware Workspace Player 14 per illustrare i diversi comandi offerti dall'interfaccia della riga di comando di .NET.</span><span class="sxs-lookup"><span data-stu-id="cb19e-145">I am using an Ubuntu 18.04 OS running on top Windows 10 using VMware Workspace Player 14 to demonstrate the different commands offered by .NET CLI.</span></span>

<span data-ttu-id="cb19e-146">Aprire la riga di comando e immettere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="cb19e-146">Open the command line and type the following:</span></span>

```console
dotnet --version
```

<span data-ttu-id="cb19e-147">Questo comando visualizza la versione dell'interfaccia della riga di comando di .NET installata.</span><span class="sxs-lookup"><span data-stu-id="cb19e-147">This command displays the version of the .NET CLI installed.</span></span>

<span data-ttu-id="cb19e-148">L'esecuzione del comando precedente sul computer visualizza quanto segue: `2.1.302`.</span><span class="sxs-lookup"><span data-stu-id="cb19e-148">Running the above command on my machine displays this: `2.1.302`.</span></span>

<span data-ttu-id="cb19e-149">Possiamo iniziare a esplorare alcuni dei comandi comuni dell'interfaccia della riga di comando di .NET.</span><span class="sxs-lookup"><span data-stu-id="cb19e-149">Let us start exploring some of the popular commands of the .NET CLI.</span></span>

<span data-ttu-id="cb19e-150">Il comando *dotnet* ha la sintassi generale seguente:</span><span class="sxs-lookup"><span data-stu-id="cb19e-150">The *dotnet* command has the following general syntax:</span></span>

```console
dotnet [verb] [arguments]
```

<span data-ttu-id="cb19e-151">Il verbo rappresenta l'azione da eseguire.</span><span class="sxs-lookup"><span data-stu-id="cb19e-151">The verb represents the action to execute.</span></span> <span data-ttu-id="cb19e-152">Gli argomenti rappresentano l'elenco di argomenti di input richiesti per eseguire il verbo.</span><span class="sxs-lookup"><span data-stu-id="cb19e-152">The arguments represent the list of input arguments that the verb requires in order to execute.</span></span>

<span data-ttu-id="cb19e-153">Per ottenere informazioni sull'utilizzo di *dotnet* e per elencare tutti i *verbi* disponibili e altre informazioni correlate, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="cb19e-153">To get help on the *dotnet* usage and list all of the *verbs* available, and other related information, type the following command:</span></span>

```console
dotnet --help
```

<span data-ttu-id="cb19e-154">Questo comando visualizza quanto segue:</span><span class="sxs-lookup"><span data-stu-id="cb19e-154">This command displays the following:</span></span>

```console
.NET Command Line Tools (2.1.302)
Usage: dotnet [runtime-options] [path-to-application]
Usage: dotnet [sdk-options] [command] [arguments] [command-options]

path-to-application:
  The path to an application .dll file to execute.

SDK commands:
  new              Initialize .NET projects.
  restore          Restore dependencies specified in the .NET project.
  run              Compiles and immediately executes a .NET project.
  build            Builds a .NET project.
  publish          Publishes a .NET project for deployment (including the runtime).
  test             Runs unit tests using the test runner specified in the project.

...
```

<span data-ttu-id="cb19e-155">Sotto **SDK commands** è possibile visualizzare l'elenco completo dei comandi che possono essere eseguiti in .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="cb19e-155">Under the **SDK commands**, you can see the entire list of commands that you can execute against the .NET Core SDK.</span></span>

<span data-ttu-id="cb19e-156">I comandi più utili in assoluto sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="cb19e-156">The most useful commands at all times are the following:</span></span>

- <span data-ttu-id="cb19e-157">**dotnet new**: questo comando viene usato per lo scaffolding o la generazione di una nuova applicazione .NET.</span><span class="sxs-lookup"><span data-stu-id="cb19e-157">**dotnet new**: This command is used to scaffold/generate a new .NET application.</span></span>

- <span data-ttu-id="cb19e-158">**dotnet restore**: questo comando viene usato per il ripristino o il download di tutti i pacchetti a cui l'applicazione fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="cb19e-158">**dotnet restore**: This command is used to restore/download all packages that are referenced by the application.</span></span>

- <span data-ttu-id="cb19e-159">**dotnet run**: questo comando viene utilizzato per eseguire l'applicazione .NET.</span><span class="sxs-lookup"><span data-stu-id="cb19e-159">**dotnet run**: This command is used to run your .NET application.</span></span>

<span data-ttu-id="cb19e-160">Per ottenere assistenza sull'uso di un comando specifico, digitare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="cb19e-160">Now, to get assistance on how to use a specific command, you can type the following:</span></span>

```console
dotnet run --help
```

<span data-ttu-id="cb19e-161">Questo comando restituisce:</span><span class="sxs-lookup"><span data-stu-id="cb19e-161">This command results in:</span></span>

```console
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.
  -n, --name          The name for the output being created. If no name is specified, the name of the current directory is used.
  -o, --output        Location to place the generated output.
  -i, --install       Installs a source or a template pack.
  -u, --uninstall     Uninstalls a source or a template pack.
  --nuget-source      Specifies a NuGet source to use during install.
  --type              Filters templates based on available types. Predefined values are "project", "item" or "other".
  --force             Forces content to be generated even if it would change existing files.
  -lang, --language   Filters templates based on language and specifies the language of the template to create.


Templates                                         Short Name         Language          Tags
----------------------------------------------------------------------------------------------------------------------------
Console Application                               console            [C#], F#, VB      Common/Console
Class library                                     classlib           [C#], F#, VB      Common/Library
...

Razor Page                                        page               [C#]              Web/ASP.NET
MVC ViewImports                                   viewimports        [C#]              Web/ASP.NET
MVC ViewStart                                     viewstart          [C#]              Web/ASP.NET
ASP.NET Core Empty                                web                [C#], F#          Web/Empty
ASP.NET Core Web App (Model-View-Controller)      mvc                [C#], F#          Web/MVC
ASP.NET Core Web App                              razor              [C#]              Web/MVC/Razor Pages
ASP.NET Core with Angular                         angular            [C#]              Web/MVC/SPA
...

Solution File                                     sln                                  Solution

Examples:
    dotnet new mvc --auth Individual
    dotnet new webapi
    dotnet new --help
```

<span data-ttu-id="cb19e-162">Questo comando elenca tutte le opzioni disponibili che è possibile usare con il comando `dotnet new`.</span><span class="sxs-lookup"><span data-stu-id="cb19e-162">This command lists all the available options that you can use with the `dotnet new` command.</span></span> <span data-ttu-id="cb19e-163">Inoltre, elenca tutti i modelli di progetto disponibili che è possibile usare per generare la successiva applicazione .NET.</span><span class="sxs-lookup"><span data-stu-id="cb19e-163">Also, it lists all the available project templates that you can use to generate your next .NET application.</span></span> <span data-ttu-id="cb19e-164">Infine, una sezione contiene esempi su come usare il comando per generare una nuova applicazione .NET.</span><span class="sxs-lookup"><span data-stu-id="cb19e-164">Finally, a section shows examples on how to use the command to generate a new .NET application.</span></span>

<span data-ttu-id="cb19e-165">È possibile imparare i comandi restanti usando l'argomento `--help` per qualsiasi comando disponibile nell'interfaccia della riga di comando di .NET.</span><span class="sxs-lookup"><span data-stu-id="cb19e-165">You can learn the rest of the commands by using the `--help` argument for any command available in the .NET CLI.</span></span>

## <a name="aspnet-core-installation-on-linux-environment"></a><span data-ttu-id="cb19e-166">Installazione di ASP.NET Core in ambiente Linux</span><span class="sxs-lookup"><span data-stu-id="cb19e-166">ASP.NET Core installation on Linux environment</span></span>

<span data-ttu-id="cb19e-167">La versione più recente e avanzata di ASP.NET Core è v2.1.</span><span class="sxs-lookup"><span data-stu-id="cb19e-167">The latest and greatest version of ASP.NET Core is v2.1.</span></span> <span data-ttu-id="cb19e-168">In generale, ASP.NET Core viene fornito come parte di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="cb19e-168">In general, ASP.NET Core ships as part of the .NET Core SDK.</span></span> <span data-ttu-id="cb19e-169">Pertanto, dopo l'installazione di .NET Core SDK nel computer, è possibile iniziare a sviluppare qualsiasi applicazione .NET Core, comprese le app Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb19e-169">Therefore, by installing the .NET Core SDK on your machine, you can start developing with any .NET Core application, including ASP.NET Core web apps.</span></span>

<span data-ttu-id="cb19e-170">Per questa dimostrazione, viene utilizzato un sistema operativo Ubuntu 18.04 in esecuzione su Windows 10 con l'aiuto di VMware Workstation Player.</span><span class="sxs-lookup"><span data-stu-id="cb19e-170">For this demonstration, I am using an Ubuntu 18.04 OS running on Windows 10, with the help of VMware Workstation Player.</span></span>

<span data-ttu-id="cb19e-171">Per scaricare .NET Core SDK, è necessario visitare la home page di [.NET Downloads](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="cb19e-171">To download the .NET Core SDK, you need to visit the [.NET Downloads](https://www.microsoft.com/net/download) home page.</span></span> <span data-ttu-id="cb19e-172">La pagina contiene collegamenti per scaricare .NET Core SDK su diverse piattaforme: Windows, Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="cb19e-172">The page contains links to download the .NET Core SDK on different platforms: Windows, Linux, and macOS.</span></span>

<span data-ttu-id="cb19e-173">Individuare la scheda **Linux** e farci clic sopra. Sono disponibili due opzioni:</span><span class="sxs-lookup"><span data-stu-id="cb19e-173">Locate and click on the **Linux** tab. You have two options:</span></span>

- <span data-ttu-id="cb19e-174">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="cb19e-174">.NET Core SDK</span></span>
- <span data-ttu-id="cb19e-175">.NET Core Runtime</span><span class="sxs-lookup"><span data-stu-id="cb19e-175">.NET Core Runtime</span></span>

<span data-ttu-id="cb19e-176">.NET Core Runtime è incluso all'interno di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="cb19e-176">The .NET Core Runtime is already contained inside the .NET Core SDK.</span></span> <span data-ttu-id="cb19e-177">Il runtime viene usato per eseguire un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cb19e-177">The runtime is used to run an application.</span></span> <span data-ttu-id="cb19e-178">Tutti gli strumenti necessari per sviluppare, testare, eseguire e pubblicare sono inclusi all'interno di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="cb19e-178">All the tools to develop, test, run, and publish are inside the .NET Core SDK.</span></span> <span data-ttu-id="cb19e-179">Pertanto, fare clic su **.NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="cb19e-179">Therefore, you will click on **.NET Core SDK**.</span></span>

<span data-ttu-id="cb19e-180">Dal sito Web Microsoft si viene reindirizzati alla pagina di download.</span><span class="sxs-lookup"><span data-stu-id="cb19e-180">The Microsoft website redirects you to the download page.</span></span> <span data-ttu-id="cb19e-181">Qui, selezionare la **Distribuzione di Linux**.</span><span class="sxs-lookup"><span data-stu-id="cb19e-181">There, you need to select the **Linux Distribution**.</span></span> <span data-ttu-id="cb19e-182">Per l'esempio, si selezionerà **Ubuntu 18.04**.</span><span class="sxs-lookup"><span data-stu-id="cb19e-182">In my case, I will select **Ubuntu 18.04**.</span></span> <span data-ttu-id="cb19e-183">Dopo la selezione, vengono visualizzate automaticamente le istruzioni per installare .NET Core SDK in Ubuntu 18.04.</span><span class="sxs-lookup"><span data-stu-id="cb19e-183">Automatically, upon selection, the instructions on how to install .NET Core SDK on Ubuntu 18.04 is displayed.</span></span>

## <a name="summary"></a><span data-ttu-id="cb19e-184">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="cb19e-184">Summary</span></span>

<span data-ttu-id="cb19e-185">Quando si decide di creare un'applicazione Web, è possibile scegliere tra molte lingue e framework.</span><span class="sxs-lookup"><span data-stu-id="cb19e-185">When deciding to build a web application, you have a choice of many languages and frameworks.</span></span> <span data-ttu-id="cb19e-186">Azure rende questa scelta più semplice in quanto consente di ospitare tipi diversi di applicazioni, ad esempio Node.js, PHP o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb19e-186">Azure tries to make this choice easier by allowing you to host different types of applications, like Node.js, PHP, or .NET Core.</span></span> <span data-ttu-id="cb19e-187">In questo modo è possibile utilizzare le lingue e i framework con cui si ha più dimestichezza anziché cambiarli per soddisfare le esigenze dell'host Web.</span><span class="sxs-lookup"><span data-stu-id="cb19e-187">This allows you to use the languages and frameworks that you're most comfortable with instead of changing to meet the needs of your web host.</span></span>
