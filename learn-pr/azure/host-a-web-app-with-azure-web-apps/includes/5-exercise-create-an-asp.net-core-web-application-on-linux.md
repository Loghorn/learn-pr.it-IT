<span data-ttu-id="d879f-101">In questa unità verrà creata un'app web ASP.NET Core tramite l'interfaccia della riga di comando di .NET in un computer Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="d879f-101">In this unit, you will create an ASP.NET Core web app using the .NET CLI on an Ubuntu machine.</span></span>

## <a name="aspnet-core-installation-on-linux-environment"></a><span data-ttu-id="d879f-102">Installazione di ASP.NET Core in ambiente Linux</span><span class="sxs-lookup"><span data-stu-id="d879f-102">ASP.NET Core installation on Linux environment</span></span>

<span data-ttu-id="d879f-103">Visitare la [pagina dei download di .NET](https://www.microsoft.com/net/download) di Microsoft e seguire gli stessi passaggi indicati nella pagina di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="d879f-103">Visit the Microsoft [.NET Downloads page](https://www.microsoft.com/net/download) and follow the same steps mentioned on the .NET Core SDK page.</span></span> <span data-ttu-id="d879f-104">Sono riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="d879f-104">They are below.</span></span> <span data-ttu-id="d879f-105">Per eseguire i comandi, è necessario aprire una nuova istanza della riga di comando del **terminale**.</span><span class="sxs-lookup"><span data-stu-id="d879f-105">In order to execute the commands, you need to open a new **Terminal** command-line instance.</span></span>

### <a name="register-microsoft-key-and-feed"></a><span data-ttu-id="d879f-106">Registrare la chiave Microsoft e il feed</span><span class="sxs-lookup"><span data-stu-id="d879f-106">Register Microsoft key and feed</span></span>

<span data-ttu-id="d879f-107">Prima di installare .NET è necessario registrare la chiave Microsoft, registrare il repository dei prodotti e installare le dipendenze necessarie.</span><span class="sxs-lookup"><span data-stu-id="d879f-107">Before installing .NET, you'll need to register the Microsoft key, register the product repository, and install the required dependencies.</span></span> <span data-ttu-id="d879f-108">Questa operazione deve essere eseguita una volta sola per ogni computer.</span><span class="sxs-lookup"><span data-stu-id="d879f-108">This only needs to be done once per machine.</span></span>

```console
~$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
~$ sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-the-net-sdk"></a><span data-ttu-id="d879f-109">Installare .NET SDK</span><span class="sxs-lookup"><span data-stu-id="d879f-109">Install the .NET SDK</span></span>

<span data-ttu-id="d879f-110">Aggiornare i prodotti disponibili per l'installazione, quindi installare .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="d879f-110">Update the products available for installation, then install the .NET SDK.</span></span>

<span data-ttu-id="d879f-111">Al prompt dei comandi eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="d879f-111">At your command prompt, run the following commands:</span></span>

```console
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-2.1
```

<span data-ttu-id="d879f-112">Durante il processo di installazione potrebbe essere richiesto di immettere la password dell'account.</span><span class="sxs-lookup"><span data-stu-id="d879f-112">During the installation process, you might be asked to enter your account password.</span></span> <span data-ttu-id="d879f-113">Specificare la password e premere INVIO per continuare.</span><span class="sxs-lookup"><span data-stu-id="d879f-113">Provide your password and hit Enter to continue.</span></span>

<span data-ttu-id="d879f-114">Digitare quanto segue per verificare l'installazione:</span><span class="sxs-lookup"><span data-stu-id="d879f-114">To verify the installation, type the following:</span></span>

```console
dotnet --version
```

<span data-ttu-id="d879f-115">L'output restituito è:</span><span class="sxs-lookup"><span data-stu-id="d879f-115">And the output displayed is:</span></span>

```console
2.1.302
```

<span data-ttu-id="d879f-116">Ora che .NET Core SDK è installato, è stato installato anche ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d879f-116">Now that you have the .NET Core SDK installed, you also have the ASP.NET Core installed.</span></span> <span data-ttu-id="d879f-117">A questo punto verrà illustrato come usare l'interfaccia della riga di comando di .NET per creare un nuovo progetto ASP.NET Core usando i comandi appena appresi.</span><span class="sxs-lookup"><span data-stu-id="d879f-117">Let's see together how you can make use of the .NET CLI to create a new ASP.NET Core project using the commands we just learned.</span></span>

## <a name="open-a-terminal-window"></a><span data-ttu-id="d879f-118">Aprire una finestra terminale</span><span class="sxs-lookup"><span data-stu-id="d879f-118">Open a Terminal window</span></span>

<span data-ttu-id="d879f-119">In primo luogo, è necessario aprire la finestra Terminale.</span><span class="sxs-lookup"><span data-stu-id="d879f-119">First, you need to open the Terminal window.</span></span> <span data-ttu-id="d879f-120">La finestra terminale consente di eseguire comandi e ha un ruolo simile alla finestra del prompt dei comandi di Windows.</span><span class="sxs-lookup"><span data-stu-id="d879f-120">The Terminal window allows you to execute commands, and has a similar role to the Windows Command Prompt window.</span></span>

## <a name="create-a-new-web-project"></a><span data-ttu-id="d879f-121">Creare un nuovo progetto Web</span><span class="sxs-lookup"><span data-stu-id="d879f-121">Create a new web project</span></span>

<span data-ttu-id="d879f-122">Il fulcro degli strumenti dell'interfaccia della riga di comando di .NET è lo strumento driver *dotnet*.</span><span class="sxs-lookup"><span data-stu-id="d879f-122">The heart of the .NET CLI tools is the *dotnet* driver tool.</span></span> <span data-ttu-id="d879f-123">Tramite questo comando, si creerà un nuovo progetto Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d879f-123">Using this command, you will create a new ASP.NET Core web project.</span></span>

<span data-ttu-id="d879f-124">Per creare una nuova applicazione ASP.NET Core MVC, è sufficiente digitare i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="d879f-124">To create a new ASP.NET Core MVC application, you only need to type the following commands:</span></span>

```console
cd ~/Documents
mkdir BestBikeApp   # Hit Enter
cd BestBikeApp      # Hit Enter
dotnet new mvc      # Hit Enter
```

<span data-ttu-id="d879f-125">Con i comandi precedenti si passa alla cartella radice *Documenti* e quindi si crea una nuova cartella.</span><span class="sxs-lookup"><span data-stu-id="d879f-125">The above commands navigate to the *Documents* root folder, and then create a new folder.</span></span> <span data-ttu-id="d879f-126">Si passa quindi all'interno di questa cartella</span><span class="sxs-lookup"><span data-stu-id="d879f-126">Then, you go inside that folder.</span></span> <span data-ttu-id="d879f-127">e infine si invia il comando dell'interfaccia della riga di comando di .NET per generare una nuova applicazione ASP.NET MVC con tutti i pacchetti ripristinati:</span><span class="sxs-lookup"><span data-stu-id="d879f-127">Finally, you issue the .NET CLI command to generate a new ASP.NET MVC application with all the packages restored:</span></span>

```console
The template "ASP.NET Core Web App (Model-View-Controller)" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore-template-3pn-210 for details.

Processing post-creation actions...
Running 'dotnet restore' on /home/your-user/Documents/BestBikeApp/BestBikeApp.csproj...
...
```

<span data-ttu-id="d879f-128">Ora è sufficiente eseguire l'applicazione inviando questo comando:</span><span class="sxs-lookup"><span data-stu-id="d879f-128">All you have to do now is run the application by issuing this command:</span></span>

```console
dotnet run
```

<span data-ttu-id="d879f-129">Nel *terminale* il comando *dotnet* consente di visualizzare alcune informazioni utili per l'app in esecuzione:</span><span class="sxs-lookup"><span data-stu-id="d879f-129">In the *terminal*, the *dotnet* command displays some useful information for the running app:</span></span>

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Development
Content root path: /home/your-user/Documents/BestBikeApp
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="d879f-130">L'output descrive la situazione dopo l'avvio dell'app: l'applicazione è in esecuzione e in ascolto sulla porta 5001 e 5002 (tramite HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d879f-130">The output describes the situation after starting your app: the application is running and listening at port 5001 and 5002 (via HTTPS).</span></span>

<span data-ttu-id="d879f-131">Per eseguire localmente HTTPS nel computer in modalità di sviluppo, è necessario creare e considerare attendibile un **certificato autofirmato**.</span><span class="sxs-lookup"><span data-stu-id="d879f-131">In order to run HTTPS locally on the machine in development mode, you need to create and trust a **self-signed certificate**.</span></span>

## <a name="create-a-self-signed-certificate"></a><span data-ttu-id="d879f-132">Creare un certificato autofirmato</span><span class="sxs-lookup"><span data-stu-id="d879f-132">Create a self-signed certificate</span></span>

<span data-ttu-id="d879f-133">Eseguire il comando seguente per generare un certificato autofirmato di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="d879f-133">Run the command below to generate a development self-signed certificate:</span></span>

```console
dotnet dev-certs https -v
```

<span data-ttu-id="d879f-134">L'esecuzione del comando restituisce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="d879f-134">Running the command returns the following:</span></span>

```console
The HTTPS developer certificate was generated successfully.
```

## <a name="run-the-application"></a><span data-ttu-id="d879f-135">Eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="d879f-135">Run the application</span></span>

<span data-ttu-id="d879f-136">Per questa dimostrazione verrà usato il browser Firefox.</span><span class="sxs-lookup"><span data-stu-id="d879f-136">For this demonstration, we are using the Firefox browser.</span></span>

<span data-ttu-id="d879f-137">Aprire il browser e digitare l'indirizzo `http://localhost:5001` per verificare che l'applicazione funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="d879f-137">Open the browser and type in the address `http://localhost:5001` to verify the application is running successfully.</span></span>

![Screenshot che mostra la pagina Web predefinita del modello ASP.NET Core MVC nel Web browser.](../media/5-asp-core-mvc-default-template.PNG)

> [!NOTE]
> <span data-ttu-id="d879f-139">È necessario **aggiungere un'eccezione** per l'URL dell'applicazione perché Firefox non è stato in grado di verificare il certificato autofirmato per lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d879f-139">You need to **add an exception** for the application's URL because the development self-signed certificate couldn't be verified by Firefox.</span></span>
