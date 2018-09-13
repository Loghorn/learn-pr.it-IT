<span data-ttu-id="e3e5a-101">In questa unità si creerà un'applicazione console .NET Core e un account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-101">In this unit, you will create a .NET Core console application and an Azure storage account.</span></span>

## <a name="create-a-net-core-console-application"></a><span data-ttu-id="e3e5a-102">Creare un'applicazione console .NET Core</span><span class="sxs-lookup"><span data-stu-id="e3e5a-102">Create a .NET Core console application</span></span>

<span data-ttu-id="e3e5a-103">Per creare l'app, si userà Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-103">We will be using Visual Studio 2017 to create our app.</span></span>

1. <span data-ttu-id="e3e5a-104">In Visual Studio 2017 selezionare l'opzione di menu **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-104">In Visual Studio 2017, select the **File** > **New** > **Project** menu option.</span></span>

1. <span data-ttu-id="e3e5a-105">Nella categoria **Visual C# - .NET Core** selezionare **App console (.NET Core)**, immettere un nome per l'app e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-105">Within the **Visual C# - .NET Core** category, select **Console App (.NET Core)**, enter a name for your app and select **OK**.</span></span>
  <span data-ttu-id="e3e5a-106">![Nuova app](..\media-draft\3-new-console-app.png)</span><span class="sxs-lookup"><span data-stu-id="e3e5a-106">![New App](..\media-draft\3-new-console-app.png)</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="e3e5a-107">Creare un account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="e3e5a-107">Create an Azure storage account</span></span>

<span data-ttu-id="e3e5a-108">Per connettersi a un account di archiviazione di Azure, prima di tutto è necessario creare un account di archiviazione a cui connettersi.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-108">In order to connect to an Azure storage account, we must first create a storage account to connect to.</span></span>

1. <span data-ttu-id="e3e5a-109">In alto a sinistra nel portale di Azure selezionare 'Crea una risorsa'.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-109">In the top left of the Azure Portal, select 'Create a resource'.</span></span>

1. <span data-ttu-id="e3e5a-110">Nel riquadro di selezione visualizzato selezionare "Archiviazione".</span><span class="sxs-lookup"><span data-stu-id="e3e5a-110">In the selection pane that appears, select 'Storage'.</span></span>

1. <span data-ttu-id="e3e5a-111">Sul lato destro del riquadro selezionare "Account di archiviazione: BLOB, File, Tabelle, Code".</span><span class="sxs-lookup"><span data-stu-id="e3e5a-111">On the right side of that pane, select 'Storage account - blob, file, table, queue'.</span></span>
  <span data-ttu-id="e3e5a-112">![Selezione nel portale](..\media-draft\3-portal-storage-select.png)</span><span class="sxs-lookup"><span data-stu-id="e3e5a-112">![portal select](..\media-draft\3-portal-storage-select.png)</span></span>

1. <span data-ttu-id="e3e5a-113">È necessario immettere i dettagli dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-113">The details of the storage account need to be entered.</span></span> <span data-ttu-id="e3e5a-114">Immettere un nome per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-114">Enter a name for the storage account.</span></span>

1. <span data-ttu-id="e3e5a-115">Un gruppo di risorse è un contenitore logico per le risorse.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-115">A resource group is a logical container for resources.</span></span> <span data-ttu-id="e3e5a-116">Per la selezione del gruppo di risorse, selezionare **Crea nuovo** e immettere un nome per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-116">For the resource group selection, select **Create New** and enter a name for your resource group.</span></span>

1. <span data-ttu-id="e3e5a-117">Lasciare tutte le altre opzioni impostate sui valori predefiniti e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-117">Leave all other options at their default values and click **Create**.</span></span>
  <span data-ttu-id="e3e5a-118">![Dettagli nel portale](..\media-draft\3-portal-storage-details.png).</span><span class="sxs-lookup"><span data-stu-id="e3e5a-118">![Portal details](..\media-draft\3-portal-storage-details.png).</span></span>

<span data-ttu-id="e3e5a-119">Viene ora effettuato il provisioning del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-119">Your resource group is now being provisioned.</span></span> <span data-ttu-id="e3e5a-120">Al termine, l'account di archiviazione viene creato nel gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-120">After that, your storage account is created within the resource group.</span></span>
<span data-ttu-id="e3e5a-121">È stata anche creata l'applicazione ed è ora possibile configurarla e connetterla all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="e3e5a-121">Additionally, you have created your application and are ready to configure and connect it to your storage account.</span></span>
