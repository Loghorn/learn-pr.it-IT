<span data-ttu-id="1635e-101">In questa unità verrà creata un'app Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1635e-101">In this unit, you will create an ASP.NET Core web app.</span></span>

## <a name="create-a-new-web-project"></a><span data-ttu-id="1635e-102">Creare un nuovo progetto Web</span><span class="sxs-lookup"><span data-stu-id="1635e-102">Create a new web project</span></span>

<span data-ttu-id="1635e-103">Il nucleo degli strumenti dell'interfaccia della riga di comando .NET è rappresentato dallo strumento da riga di comando `dotnet`.</span><span class="sxs-lookup"><span data-stu-id="1635e-103">The heart of the .NET CLI tools is the `dotnet` command line tool.</span></span> <span data-ttu-id="1635e-104">Tramite questo comando si creerà un nuovo progetto Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1635e-104">Using this command, you will create a new ASP.NET Core web project.</span></span>

<span data-ttu-id="1635e-105">In Cloud Shell a destra, creare una nuova applicazione ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="1635e-105">In the Cloud Shell on the right, create a new ASP.NET Core MVC application.</span></span> <span data-ttu-id="1635e-106">Denominarla "BestBikeApp".</span><span class="sxs-lookup"><span data-stu-id="1635e-106">Name it "BestBikeApp".</span></span>

```bash
dotnet new mvc --name BestBikeApp
```

<span data-ttu-id="1635e-107">Il comando creerà una nuova cartella denominata "BestBikeApp" per contenere il progetto.</span><span class="sxs-lookup"><span data-stu-id="1635e-107">The command will create a new folder named "BestBikeApp" to hold your project.</span></span> <span data-ttu-id="1635e-108">`cd`, quindi compilare ed eseguire l'applicazione per verificare che sia completa.</span><span class="sxs-lookup"><span data-stu-id="1635e-108">`cd` there, then build and run the application to verify it is complete.</span></span>

```bash
cd BestBikeApp
dotnet run
```

<span data-ttu-id="1635e-109">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="1635e-109">You should get something like:</span></span>

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

<span data-ttu-id="1635e-110">L'output descrive la situazione dopo l'avvio dell'app: l'applicazione è in esecuzione e in ascolto sulla porta 5000.</span><span class="sxs-lookup"><span data-stu-id="1635e-110">The output describes the situation after starting your app: the application is running and listening at port 5000.</span></span>

<span data-ttu-id="1635e-111">Se l'app fosse in esecuzione nel computer locale, sarebbe possibile aprire una finestra del browser per http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="1635e-111">If were running the app on our own machine we'd be able to open a browser to http://localhost:5000.</span></span> <span data-ttu-id="1635e-112">Dato che si usa Azure Cloud Shell, però, sarà necessario distribuire l'app in una posizione con un endpoint pubblico.</span><span class="sxs-lookup"><span data-stu-id="1635e-112">Since we're on Azure Cloud Shell, we'll need to deploy the app to somewhere with a public endpoint.</span></span> <span data-ttu-id="1635e-113">L'istanza del servizio app creata in precedenza è perfetta a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="1635e-113">The App Service instance we created earlier is perfect for that.</span></span>