<span data-ttu-id="cb0c1-101">In questo esercizio si creerà un'applicazione console .NET Core e un account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-101">In this exercise, you will create a .NET Core console application and an Azure Storage account.</span></span>

## <a name="create-a-net-core-console-application"></a><span data-ttu-id="cb0c1-102">Creare un'applicazione console .NET Core</span><span class="sxs-lookup"><span data-stu-id="cb0c1-102">Create a .NET Core console application</span></span>

<span data-ttu-id="cb0c1-103">Per creare l'app, si userà Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-103">We will be using Visual Studio 2017 to create our app.</span></span>

1. <span data-ttu-id="cb0c1-104">In Visual Studio 2017 selezionare l'opzione di menu **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-104">In Visual Studio 2017, select the **File** > **New** > **Project** menu option.</span></span>
1. <span data-ttu-id="cb0c1-105">Nella categoria **Visual C# - .NET Core** selezionare **App console (.NET Core)**, immettere un nome per l'app e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-105">Within the **Visual C# - .NET Core** category, select **Console App (.NET Core)**, enter a name for your app and select **OK**.</span></span>
  <span data-ttu-id="cb0c1-106">![Nuova app](..\media-draft\0-new-console-app.png)</span><span class="sxs-lookup"><span data-stu-id="cb0c1-106">![New App](..\media-draft\0-new-console-app.png)</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="cb0c1-107">Creare un account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="cb0c1-107">Create an Azure Storage account</span></span>

<span data-ttu-id="cb0c1-108">Per connettersi a un account di archiviazione di Azure, prima di tutto è necessario creare un account di archiviazione cui connettersi.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-108">In order to connect to an Azure Storage account, we must first create a storage account to connect to.</span></span>

1. <span data-ttu-id="cb0c1-109">In alto a sinistra nel portale di Azure selezionare "Crea una risorsa".</span><span class="sxs-lookup"><span data-stu-id="cb0c1-109">In the top left of the Azure Portal, select 'Create a resource'.</span></span>
1. <span data-ttu-id="cb0c1-110">Nel riquadro di selezione visualizzato selezionare "Archiviazione".</span><span class="sxs-lookup"><span data-stu-id="cb0c1-110">In the selection pane that appears, select 'Storage'.</span></span>
1. <span data-ttu-id="cb0c1-111">Sul lato destro del riquadro selezionare "Account di archiviazione: BLOB, File, Tabelle, Code".</span><span class="sxs-lookup"><span data-stu-id="cb0c1-111">On the right side of that pane, select 'Storage account - blob, file, table, queue'.</span></span>
  <span data-ttu-id="cb0c1-112">![Selezione nel portale](..\media-draft\1-portal-storage-select.png)</span><span class="sxs-lookup"><span data-stu-id="cb0c1-112">![portal select](..\media-draft\1-portal-storage-select.png)</span></span>
1. <span data-ttu-id="cb0c1-113">È necessario immettere i dettagli dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-113">The details of the storage account need to be entered.</span></span> <span data-ttu-id="cb0c1-114">Immettere un nome per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-114">Enter a name for the storage account.</span></span>
1. <span data-ttu-id="cb0c1-115">Un gruppo di risorse è un contenitore logico per le risorse.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-115">A resource group is a logical container for resources.</span></span> <span data-ttu-id="cb0c1-116">Per la selezione del gruppo di risorse, selezionare **Crea nuovo** e immettere un nome per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-116">For the resource group selection, select **Create New** and enter a name for your resource group.</span></span>
1. <span data-ttu-id="cb0c1-117">Lasciare tutte le altre opzioni impostate sui valori predefiniti e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-117">Leave all other options at their default values and click **Create**.</span></span>
  <span data-ttu-id="cb0c1-118">![Dettagli nel portale](..\media-draft\2-portal-storage-details.png).</span><span class="sxs-lookup"><span data-stu-id="cb0c1-118">![portal details](..\media-draft\2-portal-storage-details.png).</span></span>

<span data-ttu-id="cb0c1-119">Viene ora effettuato il provisioning del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-119">Your resource group is now being provisioned.</span></span> <span data-ttu-id="cb0c1-120">Al termine, l'account di archiviazione viene creato nel gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-120">After which, your storage account is then created within the resource group.</span></span>
<span data-ttu-id="cb0c1-121">È stata anche creata l'applicazione ed è ora possibile configurarla e connetterla all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="cb0c1-121">Additionally, you have created your application and are ready to configure and connect it to your storage account.</span></span>