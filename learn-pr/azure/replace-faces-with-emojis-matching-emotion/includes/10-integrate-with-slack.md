<span data-ttu-id="0a545-101">Abbiamo creato tutte le necessarie funzioni di Azure e sia possibile eseguirli in locale.</span><span class="sxs-lookup"><span data-stu-id="0a545-101">We have created all the required Azure functions, and we can run them locally.</span></span>

<span data-ttu-id="0a545-102">In questa lezione, è connettersi nostro funzioni di Azure con Slack e creare un Slack _barra_ comando che chiama la funzione di Azure e visualizza l'immagine risultante mojified nella finestra di slack.</span><span class="sxs-lookup"><span data-stu-id="0a545-102">In this lecture, we connect our Azure Functions with Slack and create a Slack _slash_ command which calls our Azure Function and displays the resulting mojified image in the slack window.</span></span>

<span data-ttu-id="0a545-103">Al termine di questa lezione si apprenderà:</span><span class="sxs-lookup"><span data-stu-id="0a545-103">By the end of this lecture you will learn:</span></span>

- <span data-ttu-id="0a545-104">Come creare un'app Slack e la connessione di un comando barra con un endpoint di funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="0a545-104">How to create a Slack app and connect a slash command with an Azure Function endpoint.</span></span>
- <span data-ttu-id="0a545-105">Come testare il comando barra in locale, senza la distribuzione nel cloud.</span><span class="sxs-lookup"><span data-stu-id="0a545-105">How to test your slash command locally, without deploying to the cloud.</span></span>

## <a name="using-ngrok-to-develop-with-slash-commands"></a><span data-ttu-id="0a545-106">Uso di ngrok per sviluppare comandi barra</span><span class="sxs-lookup"><span data-stu-id="0a545-106">Using ngrok to develop with slash commands</span></span>

<span data-ttu-id="0a545-107">Le funzioni di Azure sono attualmente in esecuzione in locale; solo Tuttavia, slack viene eseguito nel cloud, su internet.</span><span class="sxs-lookup"><span data-stu-id="0a545-107">Our Azure functions are currently only running locally; however, slack runs in the cloud, on the internet.</span></span>

<span data-ttu-id="0a545-108">Quando si configura slack _barra_ comando nella sezione successiva, sta per richiedere un URL pubblico che viene chiamato ogni volta che un utente esegue un comando della barra.</span><span class="sxs-lookup"><span data-stu-id="0a545-108">When we set up a slack _slash_ command in the next section, it's going to ask for a public URL which it calls whenever someone executes a slash command.</span></span>

<span data-ttu-id="0a545-109">Vogliamo fornire loro i `RespondToSlackCommand` URL, ma che esiste solo nel nostro computer locale.</span><span class="sxs-lookup"><span data-stu-id="0a545-109">We want to give them our `RespondToSlackCommand` URL, but that only exists on our local computer.</span></span>

<span data-ttu-id="0a545-110">Come posso avere servizi nell'accesso al cloud alle risorse nel computer locale per lo sviluppo?</span><span class="sxs-lookup"><span data-stu-id="0a545-110">How can you give services in the cloud access to resources on your local computer for development?</span></span>

<span data-ttu-id="0a545-111">È un'ottima soluzione chiave del rebus `ngrok` è uno strumento che consente di creare un tunnel sicuro nel computer locale da internet.</span><span class="sxs-lookup"><span data-stu-id="0a545-111">A great solution to this conundrum is `ngrok` this is a tool which creates secure tunnels to your local machine from the internet.</span></span>

1. <span data-ttu-id="0a545-112">Visitare https://ngrok.com/download</span><span class="sxs-lookup"><span data-stu-id="0a545-112">Visit https://ngrok.com/download</span></span>
2. <span data-ttu-id="0a545-113">Seguire le istruzioni per scaricare e installare il `ngrok` pacchetto nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="0a545-113">Follow the instructions to download and install the `ngrok` package to your local machine.</span></span>
3. <span data-ttu-id="0a545-114">Eseguire `ngrok http 7071` in un terminale.</span><span class="sxs-lookup"><span data-stu-id="0a545-114">Run `ngrok http 7071` in a terminal.</span></span>
   <span data-ttu-id="0a545-115">![ngrok](/media-drafts/9.ngrok.png)</span><span class="sxs-lookup"><span data-stu-id="0a545-115">![ngrok](/media-drafts/9.ngrok.png)</span></span>
4. <span data-ttu-id="0a545-116">Questo visualizza un URL pubblico che terminano con `ngrok.io` ad esempio `http://bbade778.ngrok.io`, prendere nota di questo è necessario nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="0a545-116">This prints out a public URL ending in `ngrok.io` e.g. `http://bbade778.ngrok.io`, make a note of this you need it in the next section.</span></span>

> <span data-ttu-id="0a545-117">**Nota**</span><span class="sxs-lookup"><span data-stu-id="0a545-117">**Note**</span></span>
>
> <span data-ttu-id="0a545-118">Ciò `http://*.ngrock.io` URL è possibile usare esattamente come è stato usato `http://localhost:7071`, provare con un'immagine come illustrato di seguito `http://[ngrok-url]/api/MojifyImage?imageUrl=[url-to-an-image]`</span><span class="sxs-lookup"><span data-stu-id="0a545-118">This `http://*.ngrock.io` URL you can use just as you used `http://localhost:7071`, try it out with an image like so `http://[ngrok-url]/api/MojifyImage?imageUrl=[url-to-an-image]`</span></span>

## <a name="create-a-slack-app"></a><span data-ttu-id="0a545-119">Creare un'app slack</span><span class="sxs-lookup"><span data-stu-id="0a545-119">Create a slack app</span></span>

<span data-ttu-id="0a545-120">Esiste un comando barra come parte di un'app slack, slack _bot_.</span><span class="sxs-lookup"><span data-stu-id="0a545-120">A slash command exists as part of a slack app, a slack _bot_.</span></span>

<span data-ttu-id="0a545-121">Visitare [creare App Slack](https://api.slack.com/apps/new)</span><span class="sxs-lookup"><span data-stu-id="0a545-121">Visit [Create Slack App](https://api.slack.com/apps/new)</span></span>

<span data-ttu-id="0a545-122">Scegliere un' `App Name` e associarlo con il `Workspace` creato all'inizio di questo esercizio e quindi premere `Create App`, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0a545-122">Choose an `App Name` and associate it with the `Workspace` you created at the start of this exercise and then press `Create App`, like so:</span></span>

![Creare App Slack](/media-drafts/9.create-slack-app.png)

<span data-ttu-id="0a545-124">Una volta che viene creato quindi dobbiamo andare in app slack e creare un comando della barra.</span><span class="sxs-lookup"><span data-stu-id="0a545-124">Once that's created we then need to go into our slack app and create a slash command.</span></span>

## <a name="create-a-slash-command"></a><span data-ttu-id="0a545-125">Creare un comando della barra</span><span class="sxs-lookup"><span data-stu-id="0a545-125">Create a slash command</span></span>

<span data-ttu-id="0a545-126">Fare clic su di `Slash Commands` voce di menu.</span><span class="sxs-lookup"><span data-stu-id="0a545-126">Click on the `Slash Commands` menu item.</span></span>

![Comandi barra](/media-drafts/9.slash-commands.png)

<span data-ttu-id="0a545-128">Fare clic su`Create New Command`.</span><span class="sxs-lookup"><span data-stu-id="0a545-128">Click `Create New Command`</span></span>

- <span data-ttu-id="0a545-129">Immettere qualsiasi nome desiderato per il comando barra nel `command` campo.</span><span class="sxs-lookup"><span data-stu-id="0a545-129">Enter whatever name you want for your slash command in the `command` field.</span></span>
- <span data-ttu-id="0a545-130">Immettere l'URL pubblico ngrok nel `Request URL` campo</span><span class="sxs-lookup"><span data-stu-id="0a545-130">Enter the ngrok public URL into the `Request URL` field</span></span>

  > <span data-ttu-id="0a545-131">**IMPORTANTE**</span><span class="sxs-lookup"><span data-stu-id="0a545-131">**IMPORTANT**</span></span>
  >
  > <span data-ttu-id="0a545-132">Ricordarsi di aggiungere `/api/RespondToSlackCommand` all'URL</span><span class="sxs-lookup"><span data-stu-id="0a545-132">Remember to append `/api/RespondToSlackCommand` to the URL</span></span>

- <span data-ttu-id="0a545-133">Aggiungere un hint per la breve descrizione e l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="0a545-133">Add a short description and usage hint.</span></span>
- <span data-ttu-id="0a545-134">Premere `Save`</span><span class="sxs-lookup"><span data-stu-id="0a545-134">Press `Save`</span></span>

![Comandi barra](/media-drafts/9.create-slash-command.png)

<span data-ttu-id="0a545-136">Dopo il salvataggio verrà visualizzata una schermata come segue:</span><span class="sxs-lookup"><span data-stu-id="0a545-136">Once saved you should see a screen like so:</span></span>

![Barra i comandi riuscita](/media-drafts/9.create-slash-commands-success.png)

## <a name="install-the-slack-app-to-the-workspace"></a><span data-ttu-id="0a545-138">Installare l'app slack all'area di lavoro</span><span class="sxs-lookup"><span data-stu-id="0a545-138">Install the slack app to the workspace</span></span>

<span data-ttu-id="0a545-139">Per poter usare l'App nell'app slack nell'area di lavoro, è necessario installarlo.</span><span class="sxs-lookup"><span data-stu-id="0a545-139">Before we can use our slack app in our workspace, we need to install it.</span></span>

1. <span data-ttu-id="0a545-140">Selezionare il `Basic Information` voce di menu</span><span class="sxs-lookup"><span data-stu-id="0a545-140">Select the `Basic Information` menu item</span></span>

2. <span data-ttu-id="0a545-141">Espandere la `Install your app to your workspace` opzione, quindi premere il `Install app to workspace` pulsante.</span><span class="sxs-lookup"><span data-stu-id="0a545-141">Expand out the `Install your app to your workspace` option and press the `Install app to workspace` button.</span></span>

   ![Installare l'App all'area di lavoro](/media-drafts/9.install-app-to-workspace.png)

3. <span data-ttu-id="0a545-143">Potrebbe chiedere di autenticare l'applicazione come segue:</span><span class="sxs-lookup"><span data-stu-id="0a545-143">It may ask you to authenticate your application like so:</span></span>

   ![Eseguire l'autenticazione dell'App dell'area di lavoro](/media-drafts/9.authenticate-slack-app.png)

## <a name="try-it-out"></a><span data-ttu-id="0a545-145">Provare questa operazione</span><span class="sxs-lookup"><span data-stu-id="0a545-145">Try it out</span></span>

<span data-ttu-id="0a545-146">Ora che il comando della barra è stato creato e connesso alla nostra istanza in esecuzione in locale di funzioni di Azure tramite il collegamento ngrok è possibile eseguirne il test.</span><span class="sxs-lookup"><span data-stu-id="0a545-146">Now that your slash command has been created and connected to our locally running instance of Azure Functions via the ngrok link let's test it.</span></span>

1. <span data-ttu-id="0a545-147">Passare all'area di lavoro slack che è associato all'app slack.</span><span class="sxs-lookup"><span data-stu-id="0a545-147">Go to the slack workspace you associated with your slack app.</span></span>
2. <span data-ttu-id="0a545-148">Passare a qualsiasi finestra di chat e tipo `/mojify` (o nome assegnato all'app)</span><span class="sxs-lookup"><span data-stu-id="0a545-148">Go to any chat window and type `/mojify` (or whatever name you gave the app)</span></span>
3. <span data-ttu-id="0a545-149">Se tutto ha funzionato correttamente, dovrebbe essere `mojify` come un'opzione</span><span class="sxs-lookup"><span data-stu-id="0a545-149">If everything has worked correctly, you should see `mojify` as an option</span></span>

   ![Controllo Mojify](/media-drafts/9.slack-check-mojify.png)

4. <span data-ttu-id="0a545-151">A questo punto digitare `/mojify` e aggiungere un URL immagine, quindi premere INVIO, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0a545-151">Now type `/mojify` and add an image URL then press enter, like so:</span></span>

   ![Tipo Mojify](/media-drafts/9.slack-type-mojify.png)
