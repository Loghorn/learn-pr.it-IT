<span data-ttu-id="d09d7-101">L'app e la funzione di Azure sono ora complete e in esecuzione in locale.</span><span class="sxs-lookup"><span data-stu-id="d09d7-101">The app and Azure Function are now complete and running locally.</span></span> <span data-ttu-id="d09d7-102">In questa unità la funzione viene pubblicata in Azure per l'esecuzione nel cloud.</span><span class="sxs-lookup"><span data-stu-id="d09d7-102">In this unit, you publish the function to Azure to run in the cloud.</span></span>

> [!Note]
> <span data-ttu-id="d09d7-103">Si pubblicherà la funzione da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d09d7-103">You will publish your function from Visual Studio.</span></span> <span data-ttu-id="d09d7-104">Si tratta di un modo efficace per iniziare a usare modelli di prova, prototipi e apprendimento, ma **non** per un'app di qualità idonea ad ambienti di produzione.</span><span class="sxs-lookup"><span data-stu-id="d09d7-104">This is a great way to get started for proof-of-concepts, prototypes, and learning, but for a production-quality app you should **not** use this method.</span></span> <span data-ttu-id="d09d7-105">È consigliabile usare un tipo di distribuzione basata sull'integrazione continua.</span><span class="sxs-lookup"><span data-stu-id="d09d7-105">You should use some form of CI-based deployment.</span></span> <span data-ttu-id="d09d7-106">Altre informazioni sono disponibili nei [documenti sulla distribuzione di Funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="d09d7-106">You can read more about doing this in the [Azure Functions Deployment docs](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment?azure-portal=true).</span></span>

## <a name="publishing-your-app-to-azure"></a><span data-ttu-id="d09d7-107">Pubblicazione di un'app in Azure</span><span class="sxs-lookup"><span data-stu-id="d09d7-107">Publishing your app to Azure</span></span>

<span data-ttu-id="d09d7-108">È possibile pubblicare le funzioni di Azure in Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d09d7-108">Azure functions can be published to Azure from inside Visual Studio.</span></span>

1. <span data-ttu-id="d09d7-109">Arrestare il runtime di Funzioni di Azure locale se è ancora in esecuzione dall'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="d09d7-109">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

1. <span data-ttu-id="d09d7-110">Fare clic con il pulsante destro del mouse sull'app `ImHere.Functions` in Esplora soluzioni e scegliere *Pubblica*.</span><span class="sxs-lookup"><span data-stu-id="d09d7-110">Right-click on the `ImHere.Functions` app in the solution explorer and select *Publish...*.</span></span>

    ![Pubblicazione diretta nell'app per le funzioni](../media/8-right-click-publish.png)

1. <span data-ttu-id="d09d7-112">Nella finestra di dialogo **Selezionare una destinazione di pubblicazione** selezionare *App per le funzioni di Azure* e per **Servizio app di Azure** selezionare *Crea nuovo*.</span><span class="sxs-lookup"><span data-stu-id="d09d7-112">From the **Pick a publish target** dialog, select *Azure Function App*, and for **Azure App Service**, select *Create New*.</span></span> <span data-ttu-id="d09d7-113">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="d09d7-113">Click **Publish**.</span></span>

    ![Creazione di una nuova istanza di Servizio app di Azure in cui eseguire la pubblicazione](../media/8-pick-publish-target.png)

1. <span data-ttu-id="d09d7-115">Accedere ad Azure usando il nome utente e la password nella scheda **Risorse** di queste istruzioni.</span><span class="sxs-lookup"><span data-stu-id="d09d7-115">Sign in to Azure using the username and password in the **Resources** tab of these instructions.</span></span> <span data-ttu-id="d09d7-116">Se si sta compilando l'app in locale, anziché tramite la macchina virtuale, accedere con l'account di Azure, creandone uno nuovo, se necessario, attraverso i collegamenti nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="d09d7-116">If you are building this app locally instead of using the VM, log in with your Azure account, creating a new one if necessary using the links on the dialog.</span></span>

1. <span data-ttu-id="d09d7-117">Lasciare le impostazioni predefinite per tutti i valori. In questo modo verrà creata l'infrastruttura necessaria per l'esecuzione dell'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="d09d7-117">Leave all the values as the defaults, as this will create all the necessary infrastructure to run your Functions app.</span></span>

1. <span data-ttu-id="d09d7-118">Fare clic su **Crea** per effettuare il provisioning di tutte le risorse in Azure e pubblicare l'app di Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="d09d7-118">Click **Create** to provision all the resources on Azure and publish your Azure Functions app.</span></span>

    ![Creare il servizio app](../media/8-create-app-service.png)

1. <span data-ttu-id="d09d7-120">Può essere visualizzata la richiesta di specificare se si vuole aggiornare la versione di Funzioni in Azure.</span><span class="sxs-lookup"><span data-stu-id="d09d7-120">You may be asked if you want to update the functions version on Azure.</span></span> <span data-ttu-id="d09d7-121">Se viene visualizzata la finestra di dialogo con tale richiesta, selezionare **Sì** per assicurarsi che l'app per le funzioni venga pubblicata con la versione più recente del runtime di Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="d09d7-121">If this dialog appears, select **Yes** to ensure your function app is published with the latest Azure Functions runtime version.</span></span>
    <span data-ttu-id="d09d7-122">![Finestra di dialogo di aggiornamento di Funzioni di Azure](../media/8-update-functions-on-azure.png)</span><span class="sxs-lookup"><span data-stu-id="d09d7-122">![The update Azure Functions dialog](../media/8-update-functions-on-azure.png)</span></span>

<span data-ttu-id="d09d7-123">Il provisioning richiede pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="d09d7-123">Provisioning will take a couple of minutes to complete.</span></span> <span data-ttu-id="d09d7-124">Verrà effettuato il provisioning delle risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="d09d7-124">The following resources will be provisioned:</span></span>

- <span data-ttu-id="d09d7-125">Un account di archiviazione per archiviare i file necessari per l'app per le funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="d09d7-125">A storage account to store the files needed for the Azure Functions app</span></span>
- <span data-ttu-id="d09d7-126">Un'app del piano di servizio per gestire le risorse di calcolo necessarie per l'app per le funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="d09d7-126">An App Service plan to manage the compute resources needed by the Azure Functions app</span></span>
- <span data-ttu-id="d09d7-127">Il servizio app che esegue la funzione di Azure</span><span class="sxs-lookup"><span data-stu-id="d09d7-127">The App Service that runs the Azure function</span></span>

<span data-ttu-id="d09d7-128">La funzione verrà ora pubblicata e sarà disponibile per la chiamata all'indirizzo **https://\<nome-app\>.azurewebsites.net/api/SendLocation**.</span><span class="sxs-lookup"><span data-stu-id="d09d7-128">The function will now be published and available to call at **https://\<your-app-name\>.azurewebsites.net/api/SendLocation**.</span></span>

## <a name="configuring-your-app"></a><span data-ttu-id="d09d7-129">Configurazione dell'app</span><span class="sxs-lookup"><span data-stu-id="d09d7-129">Configuring your app</span></span>

<span data-ttu-id="d09d7-130">Quando la funzione di Azure era in esecuzione in locale, usava le credenziali di Twilio archiviate in un file `local.settings.json`.</span><span class="sxs-lookup"><span data-stu-id="d09d7-130">When the Azure function was running locally, it was using Twilio credentials that were stored in a `local.settings.json` file.</span></span> <span data-ttu-id="d09d7-131">Come suggerisce il nome, questo file è per le impostazioni locali, non per le impostazioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="d09d7-131">As the name suggests, this file is for local settings, not Azure settings.</span></span> <span data-ttu-id="d09d7-132">Prima che la funzione di Azure possa essere chiamata all'interno di Azure, è necessario configurare le impostazioni `TwilioAccountSid` e `TwilioAuthToken`.</span><span class="sxs-lookup"><span data-stu-id="d09d7-132">Before the Azure function can be called inside Azure, the `TwilioAccountSid` and `TwilioAuthToken` settings need to be configured.</span></span>

1. <span data-ttu-id="d09d7-133">Nella scheda Pubblica fare clic sull'opzione **Gestisci impostazioni applicazione**.</span><span class="sxs-lookup"><span data-stu-id="d09d7-133">From the Publish tab, click the **Manage Application Settings** option.</span></span>

    ![Opzione Gestisci impostazioni applicazione](../media/8-application-settings-option.png)

1. <span data-ttu-id="d09d7-135">La finestra di dialogo **Impostazioni applicazione** illustra le impostazioni dell'applicazione, sia quelle con un valore locale che quelle con un valore remoto. Il valore locale deriva dal file `local.settings.json` e il valore remoto è quello usato dalla funzione quando è ospitata in Azure.</span><span class="sxs-lookup"><span data-stu-id="d09d7-135">The **Application Settings** dialog will show application settings with both a local and remote value - the local coming from your `local.settings.json` file, and the remote value is the one your function will use when it is hosted in Azure.</span></span> <span data-ttu-id="d09d7-136">Copiare i valori dalla casella *Locale* alla casella *Remoto* per i valori **TwilioAccountSid** e **TwilioAuthToken**.</span><span class="sxs-lookup"><span data-stu-id="d09d7-136">Copy the values from the *Local* to the *Remote* boxes for the **TwilioAccountSid** and **TwilioAuthToken** values.</span></span>

    ![Impostare le credenziali Twilio nelle impostazioni dell'applicazione](../media/8-set-creds-in-app-settings.png)

1. <span data-ttu-id="d09d7-138">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d09d7-138">Click **OK**.</span></span> <span data-ttu-id="d09d7-139">I valori verranno pubblicati nell'app per le funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="d09d7-139">This will publish the values to the Azure functions app.</span></span>

## <a name="pointing-the-mobile-app-to-azure"></a><span data-ttu-id="d09d7-140">Come puntare l'app per dispositivi mobili ad Azure</span><span class="sxs-lookup"><span data-stu-id="d09d7-140">Pointing the mobile app to Azure</span></span>

1. <span data-ttu-id="d09d7-141">Dalla scheda Pubblica copiare l'**URL del sito** usando il pulsante **Copia negli Appunti** accanto al valore.</span><span class="sxs-lookup"><span data-stu-id="d09d7-141">From the Publish tab, copy the **Site URL** using the **Copy to clipboard** button next to the value.</span></span>

    ![Copiare l'URL del sito dalla scheda Pubblica](../media/8-copy-site-url.png)

1. <span data-ttu-id="d09d7-143">Aprire il file `MainViewModel` dal progetto `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="d09d7-143">Open the `MainViewModel` from the `ImHere` project.</span></span>

1. <span data-ttu-id="d09d7-144">Aggiornare il valore del campo `baseUrl` in modo che indichi l'URL del sito copiato dalla scheda Pubblica.</span><span class="sxs-lookup"><span data-stu-id="d09d7-144">Update the value of the `baseUrl` field to be the site URL copied from the Publish tab.</span></span>

## <a name="test-it-out"></a><span data-ttu-id="d09d7-145">Eseguire il test</span><span class="sxs-lookup"><span data-stu-id="d09d7-145">Test it out</span></span>

1. <span data-ttu-id="d09d7-146">Impostare l'app `ImHere.UWP` come app di avvio ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="d09d7-146">Set the `ImHere.UWP` app as the startup app and run it.</span></span>

1. <span data-ttu-id="d09d7-147">Immettere un numero di telefono, quindi fare clic sul pulsante **Invia posizione**.</span><span class="sxs-lookup"><span data-stu-id="d09d7-147">Enter a phone number and click the **Send Location** button.</span></span>

1. <span data-ttu-id="d09d7-148">Si dovrebbe ricevere la posizione tramite SMS.</span><span class="sxs-lookup"><span data-stu-id="d09d7-148">You should receive the location as an SMS message.</span></span>

1. <span data-ttu-id="d09d7-149">Se viene visualizzato un errore *Il servizio non è disponibile*, verificare la versione del pacchetto NuGet "Microsoft.Azure.WebJobs.Extensions.Twilio" usato dall'app per le funzioni. Deve corrispondere alla versione 3.0.0-rc1, NON alla versione 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="d09d7-149">If you get back a *Service Unavailable* error, check what version of the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package your functions app is using, it should be 3.0.0-rc1, NOT 3.0.0.</span></span>
1. <span data-ttu-id="d09d7-150">Se viene visualizzato un errore *Il servizio non è disponibile*, verificare la versione del pacchetto NuGet "Microsoft.Azure.WebJobs.Extensions.Twilio" usato dall'app per le funzioni. Deve corrispondere alla versione 3.0.0-rc1, **NON** alla versione 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="d09d7-150">If you get back a *Service Unavailable* error, check what version of the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package your functions app is using, it should be 3.0.0-rc1, **NOT** 3.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="d09d7-151">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="d09d7-151">Summary</span></span>

<span data-ttu-id="d09d7-152">In questa unità si è appreso come pubblicare un progetto di funzioni di Azure in Azure da Visual Studio e configurare le impostazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d09d7-152">In this unit, you learned how to publish an Azure Functions project to Azure from inside Visual Studio and configure application settings.</span></span>