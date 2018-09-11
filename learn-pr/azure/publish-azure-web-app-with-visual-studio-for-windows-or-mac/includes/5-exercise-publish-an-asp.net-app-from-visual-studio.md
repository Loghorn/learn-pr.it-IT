<span data-ttu-id="ef79c-101">Ora si dispone di un'applicazione Web di ASP.NET Core in esecuzione localmente.</span><span class="sxs-lookup"><span data-stu-id="ef79c-101">You now have a local running ASP.NET Core web application.</span></span> <span data-ttu-id="ef79c-102">In questo esercizio si pubblicherà l'applicazione nel servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="ef79c-102">In this exercise, you'll publish your application to Azure App Service.</span></span>

### <a name="visual-studio-for-windows"></a><span data-ttu-id="ef79c-103">Visual Studio per Windows</span><span class="sxs-lookup"><span data-stu-id="ef79c-103">Visual Studio for Windows</span></span>

1. <span data-ttu-id="ef79c-104">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="ef79c-104">In Solution Explorer, right-click your project and select **Publish**.</span></span>

1. <span data-ttu-id="ef79c-105">Nella finestra di dialogo che viene visualizzata scegliere, a sinistra, **Servizio app** come destinazione della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="ef79c-105">In the dialog box that appears, on the left, choose **App Service** as your publish target.</span></span>  <span data-ttu-id="ef79c-106">A destra selezionare **Crea nuovo** per creare un nuovo servizio app.</span><span class="sxs-lookup"><span data-stu-id="ef79c-106">On the right, select **Create New** to create a new app service.</span></span>

1. <span data-ttu-id="ef79c-107">Scegliere il pulsante **Pubblica** per creare il nuovo servizio app.</span><span class="sxs-lookup"><span data-stu-id="ef79c-107">Click the **Publish** button to create your new app service.</span></span>

> [!NOTE]
> <span data-ttu-id="ef79c-108">Quando si distribuiscono le app, è possibile creare un nuovo piano di servizio app o continuare ad aggiungere le app a un piano esistente.</span><span class="sxs-lookup"><span data-stu-id="ef79c-108">When you deploy your apps, you can create a new App Service plan or you can continue to add apps to an existing plan.</span></span> <span data-ttu-id="ef79c-109">Per questo esercizio, si creerà un nuovo **servizio app di Azure**.</span><span class="sxs-lookup"><span data-stu-id="ef79c-109">For this exercise, you will create a new **Azure app service**.</span></span>

### <a name="visual-studio-mac"></a><span data-ttu-id="ef79c-110">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="ef79c-110">Visual Studio Mac</span></span>

1. <span data-ttu-id="ef79c-111">Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="ef79c-111">In  Solution Explorer, right-click your project.</span></span>

1. <span data-ttu-id="ef79c-112">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="ef79c-112">Select **Publish**.</span></span>

1. <span data-ttu-id="ef79c-113">Fare clic su **Pubblica in Azure**.</span><span class="sxs-lookup"><span data-stu-id="ef79c-113">Click **Publish to Azure**.</span></span> <span data-ttu-id="ef79c-114">Questo permette di connettersi all'account di Azure.</span><span class="sxs-lookup"><span data-stu-id="ef79c-114">This will connect to your Azure account.</span></span> <span data-ttu-id="ef79c-115">Potrebbero occorrere alcuni minuti per connettersi e visualizzare l'elenco dei servizi correnti.</span><span class="sxs-lookup"><span data-stu-id="ef79c-115">It may take a few minutes to connect and list your current services.</span></span>

1. <span data-ttu-id="ef79c-116">Fare clic su **Nuovo** per creare un nuovo servizio app.</span><span class="sxs-lookup"><span data-stu-id="ef79c-116">Click **New** to create a new app service.</span></span>

## <a name="configure-your-new-azure-app-service"></a><span data-ttu-id="ef79c-117">Configurare il nuovo servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="ef79c-117">Configure your new Azure App Service</span></span>

1. <span data-ttu-id="ef79c-118">Nella finestra di dialogo **Crea servizio app** fare clic su **Aggiungi un account** e accedere alla sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ef79c-118">In the **Create App Service dialog** box, click **Add an account**, and sign in to your Azure subscription.</span></span> <span data-ttu-id="ef79c-119">Se è già stato eseguito l'accesso, selezionare l'account contenente la sottoscrizione desiderata dal menu a discesa.</span><span class="sxs-lookup"><span data-stu-id="ef79c-119">If you're already signed in, select the account containing the desired subscription from the drop-down menu.</span></span>

1. <span data-ttu-id="ef79c-120">Immettere le informazioni necessarie relative al piano di servizio app.</span><span class="sxs-lookup"><span data-stu-id="ef79c-120">Enter the required information about your App Service plan.</span></span>

    ![Creare un servizio app](../media-draft/5-CreateAppService.png)

    <span data-ttu-id="ef79c-122">Nella finestra di dialogo **Crea servizio app** specificare le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ef79c-122">In the **Create App Service** dialog box, you will provide the following information:</span></span>

    - <span data-ttu-id="ef79c-123">**Nome app**: il nome dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ef79c-123">**App Name**: This is the name of your application.</span></span>  <span data-ttu-id="ef79c-124">Ciò determinerà l'URL dell'applicazione pubblicata, che sarà https://_NomeApp_. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="ef79c-124">This will determine the URL of the published application, which will be https://_AppName_.azurewebsites.net.</span></span>  <span data-ttu-id="ef79c-125">Deve essere un valore univoco.</span><span class="sxs-lookup"><span data-stu-id="ef79c-125">It has to be a unique value.</span></span> <span data-ttu-id="ef79c-126">Pertanto, sarà necessario provare alcuni nomi diversi per trovarne uno non riservato.</span><span class="sxs-lookup"><span data-stu-id="ef79c-126">Therefore, you will have to try out some different names to find one that is not reserved.</span></span>

    - <span data-ttu-id="ef79c-127">**Sottoscrizione**: sottoscrizione di Azure a cui si desidera distribuire il servizio app.</span><span class="sxs-lookup"><span data-stu-id="ef79c-127">**Subscription**: The Azure subscription you wish to deploy the app service to.</span></span>

    - <span data-ttu-id="ef79c-128">**Gruppo di risorse:** fare clic sul pulsante **Nuovo...** accanto al gruppo di risorse e immettere un nome univoco per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="ef79c-128">**Resource Group:** Click the **New...** button next to the resource group, and enter a unique name for the resource group.</span></span>

        ![Nuovo gruppo di risorse](../media-draft/5-NewResourceGroup.png)

    - <span data-ttu-id="ef79c-130">**Piano di hosting:** specifica la località, le dimensioni e le funzionalità della server farm Web che ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="ef79c-130">**Hosting Plan:** The hosting plan specifies the location, size, and features of the web server farm that hosts your app.</span></span> <span data-ttu-id="ef79c-131">È possibile selezionare un piano di hosting esistente o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="ef79c-131">You can select an existing hosting plan or create a new one.</span></span> <span data-ttu-id="ef79c-132">Per questo esercizio, se ne creerà uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="ef79c-132">For this exercise, you will create a new one.</span></span>

        <span data-ttu-id="ef79c-133">Fare clic sul pulsante **Nuovo...** accanto al piano di hosting.</span><span class="sxs-lookup"><span data-stu-id="ef79c-133">Click the **New...** button next to the hosting plan.</span></span>

        ![Nuovo piano di hosting](../media-draft/5-NewHostingPlan.png)

        <span data-ttu-id="ef79c-135">Assegnare un nome al piano di hosting e quindi specificare la località e le dimensioni.</span><span class="sxs-lookup"><span data-stu-id="ef79c-135">Give your hosting plan a name, and then specify the location and the size.</span></span>  
        
        > [!NOTE]
        > <span data-ttu-id="ef79c-136">La specifica di dimensioni e località diverse influirà sul costo del servizio.</span><span class="sxs-lookup"><span data-stu-id="ef79c-136">Specifying different locations and sizes will impact the cost of the service.</span></span> <span data-ttu-id="ef79c-137">Per questo esercizio, si consiglia di specificare la dimensione **gratuita**, affinché non venga addebitato alcun costo.</span><span class="sxs-lookup"><span data-stu-id="ef79c-137">For the exercise, it is recommended that you specify the **Free** size, which will ensure you won't incur any costs.</span></span>

    - <span data-ttu-id="ef79c-138">**Application Insights:** consente di specificare se usare Application Insights per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ef79c-138">**Application Insights:** Specifies if you want to use Application Insights for your application.</span></span> <span data-ttu-id="ef79c-139">Per questo esercizio, è consigliabile scegliere **Nessuna**.</span><span class="sxs-lookup"><span data-stu-id="ef79c-139">For this exercise, we recommend that you choose **None.**</span></span>

1. <span data-ttu-id="ef79c-140">Scegliere il pulsante **Crea** per avviare il provisioning del servizio app.</span><span class="sxs-lookup"><span data-stu-id="ef79c-140">Click the **Create** button to start provisioning your app service.</span></span> <span data-ttu-id="ef79c-141">Viene visualizzato lo stato di avanzamento della distribuzione:</span><span class="sxs-lookup"><span data-stu-id="ef79c-141">You will see progress as it deploys:</span></span>

    ![Stato di avanzamento della distribuzione](../media-draft/5-DeployProgress.png)

1. <span data-ttu-id="ef79c-143">Al termine del provisioning del servizio app, Visual Studio compila e distribuisce l'app Web.</span><span class="sxs-lookup"><span data-stu-id="ef79c-143">Once the app service is provisioned, Visual Studio will build and deploy your web app.</span></span>  <span data-ttu-id="ef79c-144">Nell'output di compilazione in Visual Studio, è possibile vedere l'output della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="ef79c-144">In your Visual Studio build output, you can see the output of the deployment.</span></span>

    ![Risultato della pubblicazione](../media-draft/5-PublishResult.png)

1. <span data-ttu-id="ef79c-146">Complimenti, l'applicazione Web di ASP.NET Core è ora pubblicata e live.</span><span class="sxs-lookup"><span data-stu-id="ef79c-146">Congratulations, your ASP.NET Core web application is now published and live.</span></span> <span data-ttu-id="ef79c-147">Sarà possibile vedere l'URL finale del sito nell'output di compilazione e anche nella pagina di pubblicazione in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef79c-147">You will be able to see the final URL for your site in the build output and also the on publishing page in Visual Studio.</span></span>

    ![Risultato della pubblicazione](../media-draft/5-PublishPage.png)

1. <span data-ttu-id="ef79c-149">Per testare il sito Web, passare all'URL indicato.</span><span class="sxs-lookup"><span data-stu-id="ef79c-149">To test your website, go to the URL indicated.</span></span>

    ![Sito Live](../media-draft/5-WebPageLive.png)

## <a name="summary"></a><span data-ttu-id="ef79c-151">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="ef79c-151">Summary</span></span>

<span data-ttu-id="ef79c-152">In questo esercizio è stato illustrato il processo di provisioning di un servizio app di Azure e la distribuzione di un'applicazione Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef79c-152">This exercise demonstrated the process of provisioning an Azure app service and deploying an ASP.NET Core web application.</span></span> <span data-ttu-id="ef79c-153">Ora si dispone di un'app Web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="ef79c-153">You now have a live web app.</span></span>
