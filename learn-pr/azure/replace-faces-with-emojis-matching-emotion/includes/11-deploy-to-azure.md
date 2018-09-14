<span data-ttu-id="07c7a-101">Finora in questo modulo è stato gestito ottenere un Slack _barra_ comando funziona ma solo mentre è in esecuzione su localhost e da tunnelling per il computer con ngrok.</span><span class="sxs-lookup"><span data-stu-id="07c7a-101">So far in this module, we've managed to get a Slack _slash_ command working but only while running on localhost and by tunnelling onto our computer using ngrok.</span></span> <span data-ttu-id="07c7a-102">L'operazione funziona per lo sviluppo ma per rilasciare si desidera che il codice sia in esecuzione nel cloud.</span><span class="sxs-lookup"><span data-stu-id="07c7a-102">This is fine for development but to release we want our code to be running in the cloud.</span></span>

<span data-ttu-id="07c7a-103">In questa lezione, si intende distribuire la funzione di Azure nel cloud e aggiornare la configurazione di slack per usare la nuova versione di produzione del codice.</span><span class="sxs-lookup"><span data-stu-id="07c7a-103">In this lecture, we are going to deploy our Azure Function to the cloud and update the slack configuration to use the new production version of our code.</span></span>

<span data-ttu-id="07c7a-104">Al termine di questa lezione si apprenderà:</span><span class="sxs-lookup"><span data-stu-id="07c7a-104">By the end of this lecture you will learn:</span></span>

- <span data-ttu-id="07c7a-105">Come creare un'App per le funzioni in Azure</span><span class="sxs-lookup"><span data-stu-id="07c7a-105">How to create an Azure Function App on Azure</span></span>
- <span data-ttu-id="07c7a-106">Come distribuire in Azure usando Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="07c7a-106">How to deploy to Azure using Visual Studio Code</span></span>
- <span data-ttu-id="07c7a-107">Come propagare le impostazioni locali nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="07c7a-107">How to propogate your local settings to production</span></span>

## <a name="create-an-azure-function-app-on-azure"></a><span data-ttu-id="07c7a-108">Creare un'App per le funzioni di Azure in Azure</span><span class="sxs-lookup"><span data-stu-id="07c7a-108">Create an Azure Function App on Azure</span></span>

<span data-ttu-id="07c7a-109">Verrà creata la funzione di azure tramite Visual Studio Code e l'estensione di funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="07c7a-109">We are going to be creating the azure function using VS Code and the Azure Function Extension.</span></span>

1. <span data-ttu-id="07c7a-110">Aprire il riquadro comandi con <kbd>Ctrl</kbd>+<kbd>P</kbd> e selezionare `Azure Function: Deploy to Function App`.</span><span class="sxs-lookup"><span data-stu-id="07c7a-110">Open the command palette with <kbd>Ctrl</kbd>+<kbd>P</kbd> and select `Azure Function: Deploy to Function App`.</span></span>

   ![Distribuire in App per le funzioni](/media-drafts/10.deploy-to-function-app.png)

2. <span data-ttu-id="07c7a-112">Viene richiesto all'utente di confermare che si vuole caricare il progetto corrente, andare avanti e selezionarla.</span><span class="sxs-lookup"><span data-stu-id="07c7a-112">It asks you to confirm you want to upload the current project, go ahead and select that.</span></span>

   ![Seleziona cartella da distribuire](/media-drafts/10.select-folder-to-deploy.png)

3. <span data-ttu-id="07c7a-114">Scegliere la sottoscrizione che si desidera associare all'app.</span><span class="sxs-lookup"><span data-stu-id="07c7a-114">Choose the subscription you want to associate with the app.</span></span>

   <span data-ttu-id="07c7a-115">Se si ha familiarità con Azure è necessario avere una sottoscrizione, questa selezione.</span><span class="sxs-lookup"><span data-stu-id="07c7a-115">If you are new to Azure you should have one subscription, pick that.</span></span>

4. <span data-ttu-id="07c7a-116">Scegliere Crea nuova app per le funzioni</span><span class="sxs-lookup"><span data-stu-id="07c7a-116">Choose create new function app</span></span>

   ![Creare nuove App per le funzioni](/media-drafts/10.create-new-function-app.png)

5. <span data-ttu-id="07c7a-118">Scegliere un nome per l'app</span><span class="sxs-lookup"><span data-stu-id="07c7a-118">Pick a name for your app</span></span>

   <span data-ttu-id="07c7a-119">Viene richiesto di scegliere un nome, chiamarlo di ciò che si vuole.</span><span class="sxs-lookup"><span data-stu-id="07c7a-119">It asks you to choose a name, call it whatever you want.</span></span> <span data-ttu-id="07c7a-120">Si chiama il mio `mojifier-slack-function-app`.</span><span class="sxs-lookup"><span data-stu-id="07c7a-120">I'm calling mine `mojifier-slack-function-app`.</span></span>

   ![Scegliere nome dell'App](/media-drafts/10.choose-app-name.png)

6. <span data-ttu-id="07c7a-122">Creare un nuovo gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="07c7a-122">Create a new resource group</span></span>

   <span data-ttu-id="07c7a-123">Gruppi di risorse sono come verranno raggruppate più servizi di Azure in un unico _app_.</span><span class="sxs-lookup"><span data-stu-id="07c7a-123">Resource groups are how we group multiple services in Azure into one _app_.</span></span> <span data-ttu-id="07c7a-124">È possibile creare una nuova oppure usare un oggetto esistente è già stato creato.</span><span class="sxs-lookup"><span data-stu-id="07c7a-124">You can create a new one or use an existing one you've already created.</span></span>

   <span data-ttu-id="07c7a-125">Se si crea un nuovo uno, verrà automaticamente-consente di popolare un nome per l'utente, è possibile selezionarla o digitare un'altra.</span><span class="sxs-lookup"><span data-stu-id="07c7a-125">If you create a new one, then It auto-populates a name for you, you can either select that or type another.</span></span>

7. <span data-ttu-id="07c7a-126">Scegli _Crea nuovo account di archiviazione_.</span><span class="sxs-lookup"><span data-stu-id="07c7a-126">Choose _create new storage account_.</span></span>

   <span data-ttu-id="07c7a-127">Un'app di funzione deve essere un account di archiviazione, un luogo più permanente per archiviare i dati e file.</span><span class="sxs-lookup"><span data-stu-id="07c7a-127">A function app needs a storage account, a more permanent place to store data and files.</span></span> <span data-ttu-id="07c7a-128">È possibile creare una nuova oppure usare un oggetto esistente è già stato creato.</span><span class="sxs-lookup"><span data-stu-id="07c7a-128">You can create a new one or use an existing one you've already created.</span></span>

   <span data-ttu-id="07c7a-129">Popolato automaticamente un nome per l'utente; è possibile selezionarla o digitare un'altra.</span><span class="sxs-lookup"><span data-stu-id="07c7a-129">It auto-populates a name for you; you can either select that or type another.</span></span>

8. <span data-ttu-id="07c7a-130">Selezionare un'area</span><span class="sxs-lookup"><span data-stu-id="07c7a-130">Pick a region</span></span>

   <span data-ttu-id="07c7a-131">App per le funzioni ha durata (TTL) in un punto.</span><span class="sxs-lookup"><span data-stu-id="07c7a-131">Your function app has to live somewhere.</span></span> <span data-ttu-id="07c7a-132">Consigliabile scegliere una località più vicina all'utente o gli utenti.</span><span class="sxs-lookup"><span data-stu-id="07c7a-132">I recommend choosing a location that is closest to you or your users.</span></span> <span data-ttu-id="07c7a-133">Sta selezionando `West Europe`</span><span class="sxs-lookup"><span data-stu-id="07c7a-133">I'm picking `West Europe`</span></span>

   ![Scegliere nome dell'App](/media-drafts/10.select-region.png)

9. <span data-ttu-id="07c7a-135">Attendere quindi il caricamento completo, una volta completata che la finestra di output viene visualizzato un URL simile a:</span><span class="sxs-lookup"><span data-stu-id="07c7a-135">Wait for the upload to complete, once complete the output window shows a URL like so:</span></span>

```output
Deployment to "mojifier-slack-function-app" completed.
HTTP Trigger Urls:
  MojifyImage: https://mojifier-slack-function-app.azurewebsites.net/api/MojifyImage
```

## <a name="try-it-out"></a><span data-ttu-id="07c7a-136">Provare questa operazione</span><span class="sxs-lookup"><span data-stu-id="07c7a-136">Try it out</span></span>

<span data-ttu-id="07c7a-137">A questo punto, dovrebbe essere almeno prevediamo di `RespondToSlackCommand` funzione funzionare poiché non dipende dall'eventuali altre dipendenze.</span><span class="sxs-lookup"><span data-stu-id="07c7a-137">At this point, we should at least expect the `RespondToSlackCommand` function to be working since it doesn't rely on any other dependencies.</span></span>

<span data-ttu-id="07c7a-138">Visitare `[deployed-app-name].azurefunctions.com/api/RespondToSlashCommand?text=[image-url]`</span><span class="sxs-lookup"><span data-stu-id="07c7a-138">Visit `[deployed-app-name].azurefunctions.com/api/RespondToSlashCommand?text=[image-url]`</span></span>

<span data-ttu-id="07c7a-139">Se tutte le funzioni, allora è necessario un codice JSON restituito nella finestra del browser nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="07c7a-139">If all is working well then you should some JSON returned in the browser window like so:</span></span>

```json
{
  "response_type": "in_channel",
  "text": "You must provide an image to mojify",
  "attachments": [
    {
      "image_url": "https://mojifier-slack.azurewebsites.net/api/MojifyImage?imageUrl=undefined"
    }
  ]
}
```

## <a name="upload-settings"></a><span data-ttu-id="07c7a-140">Impostazioni di caricamento</span><span class="sxs-lookup"><span data-stu-id="07c7a-140">Upload Settings</span></span>

<span data-ttu-id="07c7a-141">Ricordare sono disponibili alcune impostazioni inseriamo nel `local.settings.json` queste sono le chiavi e l'URL per i servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="07c7a-141">Remember we have some settings we put in `local.settings.json` these were the keys and URL for cognitive services.</span></span>

<span data-ttu-id="07c7a-142">`local.settings.json` è esattamente, locale, non si è mai copiato nell'ambiente di produzione quando si distribuisce.</span><span class="sxs-lookup"><span data-stu-id="07c7a-142">`local.settings.json` is exactly that, local, it's never copied over to production when you deploy.</span></span>

<span data-ttu-id="07c7a-143">In genere, è necessario passare al portale e forse aggiungere manualmente le impostazioni tramite l'interfaccia utente o l'utilizzo di `func` CLI per eseguire la stessa.</span><span class="sxs-lookup"><span data-stu-id="07c7a-143">Usually, you would need to head over to the portal and perhaps manually add in the settings via the UI or use the `func` CLI to do the same.</span></span>

<span data-ttu-id="07c7a-144">Tuttavia, poiché si sta usando Visual Studio Code, è possibile usare l'estensione di Func Azure e il riquadro comandi per caricare le impostazioni locali per noi.</span><span class="sxs-lookup"><span data-stu-id="07c7a-144">However, since we are using VS Code, we can use the Azure Func extension and the command palette to upload the local settings for us.</span></span>

1.  <span data-ttu-id="07c7a-145">Aprire il riquadro comandi con <kbd>Ctrl</kbd>+<kbd>P</kbd> e selezionare `Azure Functions: Upload Local Settings`.</span><span class="sxs-lookup"><span data-stu-id="07c7a-145">Open the command palette with <kbd>Ctrl</kbd>+<kbd>P</kbd> and select `Azure Functions: Upload Local Settings`.</span></span>

    ![Caricare le impostazioni locali](/media-drafts/10.upload-local-settings.png)

2.  <span data-ttu-id="07c7a-147">Scegliere `local.settings.json`</span><span class="sxs-lookup"><span data-stu-id="07c7a-147">Choose `local.settings.json`</span></span>

    ![Scegliere le impostazioni locali](/media-drafts/10.choose-localsettings.png)

3.  <span data-ttu-id="07c7a-149">Scegliere la sottoscrizione che è associata l'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="07c7a-149">Choose the subscription your function app is associated with.</span></span>

4.  <span data-ttu-id="07c7a-150">Selezionare l'app per le funzioni che si desidera caricare, data mining viene chiamato `mojifier-slack-function-app`</span><span class="sxs-lookup"><span data-stu-id="07c7a-150">Select the function app you want to upload to, mine is called `mojifier-slack-function-app`</span></span>

5.  <span data-ttu-id="07c7a-151">Se viene richiesto di sovrascrivere tutte le impostazioni, scegliere `No To All`</span><span class="sxs-lookup"><span data-stu-id="07c7a-151">If it asks you to overwrite any settings, choose `No To All`</span></span>

    <span data-ttu-id="07c7a-152">Si carica solo le nuove impostazioni che è ciò che vogliamo, in caso contrario, sussiste il rischio si esegue l'override delle impostazioni di ambiente di produzione con le impostazioni dall'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="07c7a-152">This only uploads new settings which is what we want, otherwise there is a risk you override production settings with settings from development.</span></span>

    ![Scegliere le impostazioni locali](/media-drafts/10.choose-no-to-all.png)

## <a name="try-it-out"></a><span data-ttu-id="07c7a-154">Provare questa operazione</span><span class="sxs-lookup"><span data-stu-id="07c7a-154">Try it out</span></span>

<span data-ttu-id="07c7a-155">Ora se tutto funziona come previsto quindi a questo punto l'endpoint MojifyImage dovrebbe funzionare.</span><span class="sxs-lookup"><span data-stu-id="07c7a-155">Now if everything is working as expected then now the MojifyImage endpoint should be working.</span></span>

<span data-ttu-id="07c7a-156">Visitare `[deployed-app-name].azurefunctions.com/api/MojifyImage?imageUrl=[image-url]`</span><span class="sxs-lookup"><span data-stu-id="07c7a-156">Visit `[deployed-app-name].azurefunctions.com/api/MojifyImage?imageUrl=[image-url]`</span></span>

<span data-ttu-id="07c7a-157">Se tutto funziona correttamente, si dovrebbe vedere l'immagine mojified nella finestra.</span><span class="sxs-lookup"><span data-stu-id="07c7a-157">If all is working well, then you should see the mojified image in the window.</span></span>

<span data-ttu-id="07c7a-158">Se non funziona, quindi provare a risolvere i problemi di seguito.</span><span class="sxs-lookup"><span data-stu-id="07c7a-158">If it's not working, then try troubleshooting the issues here.</span></span>

## <a name="update-slack"></a><span data-ttu-id="07c7a-159">Aggiornare Slack</span><span class="sxs-lookup"><span data-stu-id="07c7a-159">Update Slack</span></span>

<span data-ttu-id="07c7a-160">Ora che abbiamo installato funzioni nel cloud è possibile aggiornare le impostazioni in Slack in modo che punti al nuovo _pubblica_ URL.</span><span class="sxs-lookup"><span data-stu-id="07c7a-160">Now we have deployed our Functions to the cloud we can update the settings in Slack to point to the new _public_ URL.</span></span>

1. <span data-ttu-id="07c7a-161">Aggiornare Slack, in modo che usa l'URL pubblico del `RespondToSlackCommand`.</span><span class="sxs-lookup"><span data-stu-id="07c7a-161">Update Slack, so it uses the public URL of your `RespondToSlackCommand`.</span></span>

![Aggiornare Slack con l'URL di produzione](/media-drafts/10.deploy-update-url.png)

> <span data-ttu-id="07c7a-163">**NOTA**</span><span class="sxs-lookup"><span data-stu-id="07c7a-163">**NOTE**</span></span>
>
> <span data-ttu-id="07c7a-164">Si aggiorna il comando per l'ambiente di produzione, usare _pubblici_, versione del `RespondToSlackCommand` ospitato in `*.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="07c7a-164">I'm updating the command to use the production, _public_, version of the `RespondToSlackCommand` hosted on `*.azurewebsites.net`</span></span>

## <a name="try-it-out"></a><span data-ttu-id="07c7a-165">Provare questa operazione</span><span class="sxs-lookup"><span data-stu-id="07c7a-165">Try it out</span></span>

<span data-ttu-id="07c7a-166">Per verificare che tutto sia configurato correttamente e che Slack richiede dati da app distribuita, piuttosto che l'app locale, consentire a disattivare `ngrok` per il momento.</span><span class="sxs-lookup"><span data-stu-id="07c7a-166">To check that everything is configured correctly and that Slack requests data from the deployed app rather than the local app, let switch off `ngrok` for now.</span></span>

<span data-ttu-id="07c7a-167">Quindi head per slack finestra e digitare il comando della barra di slack che preferisci.</span><span class="sxs-lookup"><span data-stu-id="07c7a-167">Then head to slack window and type your favourite slack slash command.</span></span>

`/mojify <image>`

<span data-ttu-id="07c7a-168">Se è tutto funzioni come previsto, dovremmo vedere un'immagine visualizzata nella finestra di slack come segue:</span><span class="sxs-lookup"><span data-stu-id="07c7a-168">If it's all working as expected then we should see an image appear in the slack window like so:</span></span>

![Win](/media-drafts/10.publish-success.png)
