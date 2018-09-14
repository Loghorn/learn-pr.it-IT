<span data-ttu-id="59369-101">In questa unità, si creerà un'app web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59369-101">In this unit, you will create an ASP.NET Core web app.</span></span>

## <a name="create-a-new-web-project"></a><span data-ttu-id="59369-102">Creare un nuovo progetto Web</span><span class="sxs-lookup"><span data-stu-id="59369-102">Create a new web project</span></span>

<span data-ttu-id="59369-103">La funzionalità degli strumenti della CLI di .NET si il `dotnet` strumento da riga di comando.</span><span class="sxs-lookup"><span data-stu-id="59369-103">The heart of the .NET CLI tools is the `dotnet` command line tool.</span></span> <span data-ttu-id="59369-104">Tramite questo comando, si creerà un nuovo progetto Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59369-104">Using this command, you will create a new ASP.NET Core web project.</span></span>

1. <span data-ttu-id="59369-105">In Cloud Shell a destra, creare una nuova applicazione ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="59369-105">In the Cloud Shell on the right, create a new ASP.NET Core MVC application.</span></span> <span data-ttu-id="59369-106">Denominarla "BestBikeApp".</span><span class="sxs-lookup"><span data-stu-id="59369-106">Name it "BestBikeApp".</span></span>

```bash
dotnet new mvc -name BestBikeApp
```

1. <span data-ttu-id="59369-107">Il comando creerà una nuova cartella denominata "BestBikeApp" per contenere il progetto.</span><span class="sxs-lookup"><span data-stu-id="59369-107">The command will create a new folder named "BestBikeApp" to hold your project.</span></span> <span data-ttu-id="59369-108">Passare alla cartella.</span><span class="sxs-lookup"><span data-stu-id="59369-108">Change into that folder.</span></span>

1. <span data-ttu-id="59369-109">Compilare ed eseguire l'applicazione per verificare che sia completata.</span><span class="sxs-lookup"><span data-stu-id="59369-109">Build and run the application to verify it is complete.</span></span>

```bash
dotnet run
```

<span data-ttu-id="59369-110">Dovrebbe essere simile a:</span><span class="sxs-lookup"><span data-stu-id="59369-110">You should get something like:</span></span>

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

<span data-ttu-id="59369-111">L'output descrive la situazione dopo avere avviato l'app: l'applicazione è in esecuzione e in ascolto sulla porta 5000.</span><span class="sxs-lookup"><span data-stu-id="59369-111">The output describes the situation after starting your app: the application is running and listening at port 5000.</span></span>

<span data-ttu-id="59369-112">Se fosse in esecuzione l'app nel nostro computer saremo in grado di aprire un browser per http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="59369-112">If were running the app on our own machine we'd be able to open a browser to http://localhost:5000.</span></span> <span data-ttu-id="59369-113">Poiché si usa Azure Cloud Shell, è necessario distribuire l'app a una destinazione con un endpoint pubblico.</span><span class="sxs-lookup"><span data-stu-id="59369-113">Since we're on Azure Cloud Shell, we'll need to deploy the app to somewhere with a public endpoint.</span></span> <span data-ttu-id="59369-114">È ideale per tale istanza del servizio App creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="59369-114">The App Service instance we created earlier is perfect for that.</span></span>