<span data-ttu-id="7bab5-101">In questo esercizio verrà installato Visual Studio nel computer Windows o nel macOS in uso.</span><span class="sxs-lookup"><span data-stu-id="7bab5-101">In this exercise, you will install Visual Studio on either your Windows or your macOS computer.</span></span> <span data-ttu-id="7bab5-102">In Windows sarà necessario installare il carico di lavoro Sviluppo di Azure.</span><span class="sxs-lookup"><span data-stu-id="7bab5-102">On Windows, the Azure development workload will need to be installed.</span></span> <span data-ttu-id="7bab5-103">In Visual Studio per Mac, il flusso di lavoro di Servizi connessi predefinito consentirà di creare app per Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="7bab5-103">And on Visual Studio for Mac, the built-in Connected Services workflow will enable you to build apps for Azure App Service.</span></span> <span data-ttu-id="7bab5-104">Al termine, sarà tutto pronto per iniziare a creare le applicazioni e pubblicarle in Azure.</span><span class="sxs-lookup"><span data-stu-id="7bab5-104">At the end, you will be ready to start creating applications and publishing them to Azure.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="7bab5-105">Passaggi dell'esercizio</span><span class="sxs-lookup"><span data-stu-id="7bab5-105">Exercise steps</span></span>

<span data-ttu-id="7bab5-106">Esistono piccole differenze per l'installazione di Visual Studio tra Windows e macOS.</span><span class="sxs-lookup"><span data-stu-id="7bab5-106">There are slight differences in installing Visual Studio between Windows and macOS.</span></span> <span data-ttu-id="7bab5-107">Le sezioni seguenti descrivono tali differenze.</span><span class="sxs-lookup"><span data-stu-id="7bab5-107">The following sections outline these differences.</span></span>

### <a name="windows"></a><span data-ttu-id="7bab5-108">Windows</span><span class="sxs-lookup"><span data-stu-id="7bab5-108">Windows</span></span>

1. <span data-ttu-id="7bab5-109">Scaricare il programma di installazione di Visual Studio da https://visualstudio.microsoft.com/downloads/.</span><span class="sxs-lookup"><span data-stu-id="7bab5-109">Download the Visual Studio installer from https://visualstudio.microsoft.com/downloads/.</span></span>

1. <span data-ttu-id="7bab5-110">Eseguire il programma di installazione. Si aprirà la finestra Carichi di lavoro.</span><span class="sxs-lookup"><span data-stu-id="7bab5-110">Run the installer and it will open the Workloads window.</span></span>

1. <span data-ttu-id="7bab5-111">Scegliere il carico di lavoro **Sviluppo di Azure**.</span><span class="sxs-lookup"><span data-stu-id="7bab5-111">Choose the **Azure development** workload.</span></span>

    <span data-ttu-id="7bab5-112">Lo screenshot seguente mostra il carico di lavoro per il programma di installazione di Visual Studio selezionato per consentire lo sviluppo di Azure all'interno di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bab5-112">The following screenshot shows the Visual Studio Installer workload selected to allow Azure development within Visual Studio.</span></span>

    ![Screenshot del programma di installazione di Visual Studio con il carico di lavoro di sviluppo di Azure evidenziato.](../media/5-select-azure-workload.png)

1. <span data-ttu-id="7bab5-114">(Facoltativo) Installare il carico di lavoro Sviluppo ASP.NET e Web per prepararsi per la creazione di applicazioni Web per Azure.</span><span class="sxs-lookup"><span data-stu-id="7bab5-114">(Optional) Install the ASP.NET and web development workload to be ready to create web applications for Azure.</span></span>

1. <span data-ttu-id="7bab5-115">Fare clic su **Installa** e attendere il completamento dell'installazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bab5-115">Click **Install**, and wait for Visual Studio to install.</span></span>

1. <span data-ttu-id="7bab5-116">Al termine dell'installazione aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bab5-116">When the installation is complete, open Visual Studio.</span></span>

1. <span data-ttu-id="7bab5-117">Passare al menu Visualizza in Visual Studio e assicurarsi di avere l'opzione **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="7bab5-117">Go to the View menu in Visual Studio and make sure you have the **Cloud Explorer** option.</span></span>

    <span data-ttu-id="7bab5-118">Lo screenshot seguente mostra l'opzione di menu Cloud Explorer che sarà presente se è installato il carico di lavoro Sviluppo di Azure.</span><span class="sxs-lookup"><span data-stu-id="7bab5-118">The following screenshot shows the Cloud Explorer menu option that will be present if you have the Azure development workload installed.</span></span>

    ![Screenshot del menu Visualizza di Visual Studio con l'opzione di menu Cloud Explorer evidenziata.](../media/5-verify-cloud-explorer.png)

### <a name="macos"></a><span data-ttu-id="7bab5-120">macOS</span><span class="sxs-lookup"><span data-stu-id="7bab5-120">macOS</span></span>

1. <span data-ttu-id="7bab5-121">Passare a https://visualstudio.microsoft.com/ e scaricare il programma di installazione di Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="7bab5-121">Go to https://visualstudio.microsoft.com/ and download the Visual Studio for Mac installer.</span></span>

1. <span data-ttu-id="7bab5-122">Fare clic sul file VisualStudioInstaller.dmg per montare il programma di installazione e quindi eseguirlo facendo doppio clic sul logo.</span><span class="sxs-lookup"><span data-stu-id="7bab5-122">Click the VisualStudioInstaller.dmg file to mount the installer and then run it by double-clicking the logo.</span></span>

1. <span data-ttu-id="7bab5-123">Accettare le condizioni di licenza e privacy quando vengono visualizzate.</span><span class="sxs-lookup"><span data-stu-id="7bab5-123">Acknowledge the Privacy and License terms when presented.</span></span>

1. <span data-ttu-id="7bab5-124">Il programma di installazione richiederà i componenti da installare.</span><span class="sxs-lookup"><span data-stu-id="7bab5-124">The installer will ask which components you wish to install.</span></span> <span data-ttu-id="7bab5-125">I componenti di Azure fanno già parte di Visual Studio per Mac, ma è consigliabile installare la piattaforma **.NET Core** per sviluppare esperienze Web per Azure.</span><span class="sxs-lookup"><span data-stu-id="7bab5-125">Azure components are already part of Visual Studio for Mac, but it is recommended to install the **.NET Core** platform to develop web experiences for Azure.</span></span>

    <span data-ttu-id="7bab5-126">Lo screenshot seguente mostra la piattaforma .NET Core necessaria per aggiungere funzionalità di sviluppo di Azure a Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="7bab5-126">The following screenshot shows the .NET Core platform required to add Azure development capabilities to Visual Studio for Mac.</span></span>

    ![Screenshot del programma di installazione di Visual Studio per Mac con l'opzione per la piattaforma .NET Core evidenziata.](../media/5-vsmac-install-net-core.png)

1. <span data-ttu-id="7bab5-128">Fare clic su **Installa e aggiorna** quando si è soddisfatti delle selezioni e attendere il completamento del programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="7bab5-128">Click **Install and Update** once you are happy with the selections, and wait for the installer to complete.</span></span>

1. <span data-ttu-id="7bab5-129">Se viene chiesto di elevare le autorizzazioni necessarie, usare le credenziali di amministratore per eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="7bab5-129">If you are prompted to elevate the permissions needed, use your administrator credentials to do so.</span></span>

1. <span data-ttu-id="7bab5-130">Una volta completato il programma di installazione, avviare Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="7bab5-130">Once the installer is complete, start Visual Studio for Mac.</span></span>
