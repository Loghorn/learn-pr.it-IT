<span data-ttu-id="758d8-101">L'app e la funzione di Azure sono ora complete e in esecuzione in locale.</span><span class="sxs-lookup"><span data-stu-id="758d8-101">The app and Azure Function are now complete and running locally.</span></span> <span data-ttu-id="758d8-102">In questa unità la funzione viene pubblicata in Azure per l'esecuzione nel cloud.</span><span class="sxs-lookup"><span data-stu-id="758d8-102">In this unit, you publish the function to Azure to run in the cloud.</span></span>

> <span data-ttu-id="758d8-103">In questa unità la funzione verrà pubblicata da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="758d8-103">In this unit, you will publish your function from Visual Studio.</span></span> <span data-ttu-id="758d8-104">Si tratta di un modo efficace per iniziare a usare modelli di prova, prototipi e apprendimento, ma **non** per un'app di qualità idonea ad ambienti di produzione.</span><span class="sxs-lookup"><span data-stu-id="758d8-104">This is a great way to get started for proof-of-concepts, prototypes, and learning, but for a production-quality app you should **not** use this method.</span></span> <span data-ttu-id="758d8-105">È consigliabile usare un tipo di distribuzione basata sull'integrazione continua.</span><span class="sxs-lookup"><span data-stu-id="758d8-105">You should use some form of CI-based deployment.</span></span> <span data-ttu-id="758d8-106">Altre informazioni sono disponibili nei [documenti di distribuzione di Funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment).</span><span class="sxs-lookup"><span data-stu-id="758d8-106">You can read more about doing this in the [Azure Functions Deployment docs](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment).</span></span>
>
## <a name="publishing-your-app-to-azure"></a><span data-ttu-id="758d8-107">Pubblicazione di un'app in Azure</span><span class="sxs-lookup"><span data-stu-id="758d8-107">Publishing your app to Azure</span></span>

<span data-ttu-id="758d8-108">È possibile pubblicare le funzioni di Azure in Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="758d8-108">Azure functions can be published to Azure from inside Visual Studio.</span></span>

1. <span data-ttu-id="758d8-109">Arrestare il runtime di Funzioni di Azure locale se è ancora in esecuzione dall'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="758d8-109">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

2. <span data-ttu-id="758d8-110">Fare clic con il pulsante destro del mouse sull'app `ImHere.Functions` in Esplora soluzioni e scegliere *Pubblica..*.</span><span class="sxs-lookup"><span data-stu-id="758d8-110">Right-click on the `ImHere.Functions` app in the solution explorer and select *Publish...*.</span></span>

    ![Pubblicazione diretta nell'app per le funzioni](../media-drafts/8-right-click-publish.png)

3. <span data-ttu-id="758d8-112">Nella finestra di dialogo **Selezionare una destinazione di pubblicazione** selezionare *App per le funzioni di Azure* e per **Servizio app di Azure** selezionare *Crea nuovo*.</span><span class="sxs-lookup"><span data-stu-id="758d8-112">From the **Pick a publish target** dialog, select *Azure Function App*, and for **Azure App Service**, select *Create New*.</span></span> <span data-ttu-id="758d8-113">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="758d8-113">Click **Publish**.</span></span>

    ![Creazione di un nuovo servizio app di Azure per la pubblicazione](../media-drafts/8-pick-publish-target.png)

4. <span data-ttu-id="758d8-115">Selezionare l'account di Azure dall'elenco a discesa nell'angolo superiore destro se si hanno più account di Azure e non è selezionato quello giusto.</span><span class="sxs-lookup"><span data-stu-id="758d8-115">Select your Azure account from the drop-down in the top-right corner if you have more than one Azure accounts and the right one isn't selected.</span></span>

5. <span data-ttu-id="758d8-116">Fornire un nome all'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="758d8-116">Give your Functions app a name.</span></span> <span data-ttu-id="758d8-117">Questo nome deve essere globalmente univoco tra tutte le app per le funzioni nell'intero ambiente di Azure. Usare quindi un nome come "ImHere-\<Nome\>".</span><span class="sxs-lookup"><span data-stu-id="758d8-117">This name needs to be globally unique across all the Functions apps in the whole of Azure, so use something like "ImHere-\<YourName\>".</span></span>

6. <span data-ttu-id="758d8-118">Selezionare la sottoscrizione in cui si intende creare questa app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="758d8-118">Select the subscription you want to create this Functions app under.</span></span>

7. <span data-ttu-id="758d8-119">Creare un nuovo gruppo di risorse per questa app per le funzioni scegliendo il pulsante **Nuovo...** accanto all'elenco a discesa **Gruppo di risorse** e assegnargli un nome, ad esempio "ImHere".</span><span class="sxs-lookup"><span data-stu-id="758d8-119">Create a new resource group for this Functions app by clicking the **New...** button next to the **Resource Group** drop-down and giving it a name such as "ImHere".</span></span> <span data-ttu-id="758d8-120">I nomi dei gruppi di risorse devono essere univoci per la sottoscrizione, non globalmente univoci in Azure.</span><span class="sxs-lookup"><span data-stu-id="758d8-120">Resource group names need to be unique to your subscription, not globally unique across Azure.</span></span> <span data-ttu-id="758d8-121">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="758d8-121">Then, click **OK**.</span></span>

    ![Creare un nuovo gruppo di risorse](../media-drafts/8-create-new-resource-group.png)

   <span data-ttu-id="758d8-123">La creazione di un nuovo gruppo di risorse ne semplifica la pulizia in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="758d8-123">Creating a new resource group makes it easier to clean up later.</span></span> <span data-ttu-id="758d8-124">È possibile eliminare il gruppo di risorse con la certezza che tutti gli elementi creati per questa app per le funzioni verranno eliminati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="758d8-124">You can delete the resource group and know that everything you've created for this Functions app will all be deleted at the same time.</span></span>

8. <span data-ttu-id="758d8-125">Creare un nuovo piano di hosting facendo clic sul pulsante **Nuovo...**  accanto all'elenco a discesa **Piano di hosting**.</span><span class="sxs-lookup"><span data-stu-id="758d8-125">Create a new hosting plan by clicking the **New...** button next to the **Hosting Plan** drop-down.</span></span> <span data-ttu-id="758d8-126">Il nome del piano del servizio app predefinito sarà il nome dell'app con "Plan" alla fine.</span><span class="sxs-lookup"><span data-stu-id="758d8-126">The App Service plan name will default to your app name with "Plan" on the end.</span></span> <span data-ttu-id="758d8-127">Impostare la **posizione** sulla posizione più vicina e assicurarsi che **Dimensioni** sia impostata su Consumo.</span><span class="sxs-lookup"><span data-stu-id="758d8-127">Set the **Location** to the closest location to you and make sure **Size** is set to consumption.</span></span> <span data-ttu-id="758d8-128">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="758d8-128">Then, click **OK**.</span></span>

    ![Configurare il piano di hosting](../media-drafts/8-configure-hosting-plan.png)

9. <span data-ttu-id="758d8-130">Creare un nuovo account di archiviazione facendo clic sul pulsante **Nuovo...** accanto all'elenco a discesa **Account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="758d8-130">Create a new storage account by clicking the **New...** button next to the **Storage Account** drop-down.</span></span> <span data-ttu-id="758d8-131">Verrà fornito un nome predefinito. Mantenere tutti i valori predefiniti e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="758d8-131">A default name will be provided, so keep all the default values and click **OK**.</span></span>

    ![Creare un account di archiviazione](../media-drafts/8-create-storage-account.png)

10. <span data-ttu-id="758d8-133">Fare clic su **Crea** per effettuare il provisioning di tutte le risorse in Azure e pubblicare l'app per le funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="758d8-133">Click **Create** to provision all the resources on Azure and publish your Azure Functions app.</span></span>

    ![Creare il servizio app](../media-drafts/8-create-app-service.png)

<span data-ttu-id="758d8-135">L'esecuzione del provisioning richiede pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="758d8-135">Provisioning will take a couple of minutes or so to run.</span></span> <span data-ttu-id="758d8-136">Verrà effettuato il provisioning delle risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="758d8-136">The following resources will be provisioned:</span></span>

* <span data-ttu-id="758d8-137">Un account di archiviazione per archiviare i file necessari per l'app per le funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="758d8-137">A storage account to store the files needed for the Azure Functions app</span></span>
* <span data-ttu-id="758d8-138">Un'app del piano di servizio per gestire le risorse di calcolo necessarie per l'app per le funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="758d8-138">An App Service plan to manage the compute resources needed by the Azure Functions app</span></span>
* <span data-ttu-id="758d8-139">Il servizio app che esegue la funzione di Azure</span><span class="sxs-lookup"><span data-stu-id="758d8-139">The App Service that runs the Azure function</span></span>

<span data-ttu-id="758d8-140">La funzione verrà ora pubblicata e sarà disponibile per la chiamata all'indirizzo https://<nome-app>.azurewebsites.net/api/SendLocation.</span><span class="sxs-lookup"><span data-stu-id="758d8-140">The function will now be published and available to call at https://<your-app-name>.azurewebsites.net/api/SendLocation.</span></span>

## <a name="configuring-your-app"></a><span data-ttu-id="758d8-141">Configurazione dell'app</span><span class="sxs-lookup"><span data-stu-id="758d8-141">Configuring your app</span></span>

<span data-ttu-id="758d8-142">Quando la funzione di Azure era in esecuzione in locale, usava le credenziali di Twilio archiviate in un file `local.settings.json`.</span><span class="sxs-lookup"><span data-stu-id="758d8-142">When the Azure function was running locally, it was using Twilio credentials that were stored in a `local.settings.json` file.</span></span> <span data-ttu-id="758d8-143">Come suggerisce il nome, questo file è per le impostazioni locali, non per le impostazioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="758d8-143">As the name suggests, this file is for local settings, not Azure settings.</span></span> <span data-ttu-id="758d8-144">Prima che la funzione di Azure possa essere chiamata all'interno di Azure, è necessario configurare le impostazioni `TwilioAccountSid` e `TwilioAuthToken`.</span><span class="sxs-lookup"><span data-stu-id="758d8-144">Before the Azure function can be called inside Azure, the `TwilioAccountSid` and `TwilioAuthToken` settings need to be configured.</span></span>

1. <span data-ttu-id="758d8-145">Nella scheda Pubblica fare clic sull'opzione **Gestisci impostazioni applicazione**.</span><span class="sxs-lookup"><span data-stu-id="758d8-145">From the Publish tab, click the **Manage Application Settings** option.</span></span>

    ![Opzione Gestisci impostazioni applicazione](../media-drafts/8-application-settings-option.png)

2. <span data-ttu-id="758d8-147">Fare clic sul pulsante **Aggiungi** per aggiungere una nuova impostazione.</span><span class="sxs-lookup"><span data-stu-id="758d8-147">Click the **Add** button to add a new setting.</span></span> <span data-ttu-id="758d8-148">Denominarla "TwilioAccountSid" e impostare il valore sul SID dell'account Twilio.</span><span class="sxs-lookup"><span data-stu-id="758d8-148">Name it "TwilioAccountSid" and set the value to your Twilio account SID.</span></span> <span data-ttu-id="758d8-149">Ripetere questo passaggio per il token di autenticazione usando il nome "TwilioAuthToken".</span><span class="sxs-lookup"><span data-stu-id="758d8-149">Repeat this step for your Auth Token using the name "TwilioAuthToken".</span></span>

    ![Impostare le credenziali Twilio nelle impostazioni dell'applicazione](../media-drafts/8-set-creds-in-app-settings.png)

3. <span data-ttu-id="758d8-151">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="758d8-151">Click **OK**.</span></span>

4. <span data-ttu-id="758d8-152">Fare clic su **pubblica** per ripubblicare l'app per le funzioni di Azure con le nuove impostazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="758d8-152">Click **Publish** to republish the Azure Functions app with the new application settings.</span></span>

    ![Pulsante Pubblica](../media-drafts/8-publish-application-button.png)

## <a name="pointing-the-mobile-app-to-azure"></a><span data-ttu-id="758d8-154">Come puntare l'app per dispositivi mobili ad Azure</span><span class="sxs-lookup"><span data-stu-id="758d8-154">Pointing the mobile app to Azure</span></span>

1. <span data-ttu-id="758d8-155">Dalla scheda Pubblica copiare l'**URL del sito** usando il pulsante Copia accanto al valore.</span><span class="sxs-lookup"><span data-stu-id="758d8-155">From the Publish tab, copy the **Site URL** using the copy button next to the value.</span></span>

    ![Copiare l'URL del sito dalla scheda Pubblica](../media-drafts/8-copy-site-url.png)

2. <span data-ttu-id="758d8-157">Aprire il file `MainViewModel` dal progetto `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="758d8-157">Open the `MainViewModel` from the `ImHere` project.</span></span>

3. <span data-ttu-id="758d8-158">Aggiornare il valore del campo `baseUrl` in modo che indichi l'URL del sito copiato dalla scheda Pubblica.</span><span class="sxs-lookup"><span data-stu-id="758d8-158">Update the value of the `baseUrl` field to be the site URL copied from the Publish tab.</span></span>

4. <span data-ttu-id="758d8-159">Modificare il protocollo per questo valore da `http` in `https`.</span><span class="sxs-lookup"><span data-stu-id="758d8-159">Change the protocol for this value from `http` to `https`.</span></span> <span data-ttu-id="758d8-160">L'URL del sito viene sempre fornito con HTTP, ma è necessario usare HTTPS per chiamare una funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="758d8-160">The site URL is always given using HTTP, but you have to use HTTPS to call an Azure function.</span></span>

## <a name="test-it-out"></a><span data-ttu-id="758d8-161">Eseguire il test</span><span class="sxs-lookup"><span data-stu-id="758d8-161">Test it out</span></span>

1. <span data-ttu-id="758d8-162">Impostare l'app `ImHere.UWP` come app di avvio ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="758d8-162">Set the `ImHere.UWP` app as the startup app and run it.</span></span>

2. <span data-ttu-id="758d8-163">Immettere un numero di telefono, quindi fare clic sul pulsante **Invia posizione**.</span><span class="sxs-lookup"><span data-stu-id="758d8-163">Enter a phone number and click the **Send Location** button.</span></span>

3. <span data-ttu-id="758d8-164">La posizione dovrebbe essere ricevuta con un SMS.</span><span class="sxs-lookup"><span data-stu-id="758d8-164">You should receive the location as an SMS message.</span></span>

## <a name="summary"></a><span data-ttu-id="758d8-165">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="758d8-165">Summary</span></span>

<span data-ttu-id="758d8-166">In questa unità si è appreso come pubblicare un progetto di funzioni di Azure in Azure da Visual Studio e configurare le impostazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="758d8-166">In this unit, you learned how to publish an Azure Functions project to Azure from inside Visual Studio and configure application settings.</span></span>