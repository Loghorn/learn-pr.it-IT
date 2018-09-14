<span data-ttu-id="9f834-101">Infine, è necessario ospitare il codice da qualche parte nel cloud.</span><span class="sxs-lookup"><span data-stu-id="9f834-101">Eventually, we need to host our code somewhere in the cloud.</span></span> <span data-ttu-id="9f834-102">La tecnologia Azure che si intende usare per eseguire questa operazione è funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="9f834-102">The Azure technology we are going to use to do this is Azure Functions.</span></span>

<span data-ttu-id="9f834-103">In questa lezione si apprenderà:</span><span class="sxs-lookup"><span data-stu-id="9f834-103">In this lecture you will learn:</span></span>

- <span data-ttu-id="9f834-104">Come creare un'App di funzione di Azure e `JavaScript` Http Trigger.</span><span class="sxs-lookup"><span data-stu-id="9f834-104">How to create an Azure Function App and `JavaScript` Http Trigger.</span></span>
- <span data-ttu-id="9f834-105">Modalità di esecuzione e il debug di una funzione di Azure in locale.</span><span class="sxs-lookup"><span data-stu-id="9f834-105">How to run and debug an Azure Function locally.</span></span>

## <a name="create-an-azure-function-project"></a><span data-ttu-id="9f834-106">Creare un progetto funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="9f834-106">Create an Azure Function Project</span></span>

<span data-ttu-id="9f834-107">Un progetto di funzioni di Azure è un contenitore per le funzioni multiple.</span><span class="sxs-lookup"><span data-stu-id="9f834-107">An Azure Function Project is a container for multiple Functions.</span></span> <span data-ttu-id="9f834-108">Le funzioni attivate in modi diversi, si saranno attiva funzioni eseguendo una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f834-108">Functions are triggered in different ways; we'll be triggering our functions by making a HTTP request.</span></span>

<span data-ttu-id="9f834-109">Esistono molti modi per creare funzioni di Azure; si intende usare Visual Studio Code con l'estensione di funzioni di Azure che è stato installato in precedenza in questo modulo.</span><span class="sxs-lookup"><span data-stu-id="9f834-109">There are many ways to create Azure Functions; we are going to use Visual Studio Code with the Azure Functions Extension which we installed earlier on in this module.</span></span>

<span data-ttu-id="9f834-110">È necessario prima creare il progetto di funzione.</span><span class="sxs-lookup"><span data-stu-id="9f834-110">Let's first create our Function Project.</span></span>

1. <span data-ttu-id="9f834-111">Tipo di <kbd>Ctrl</kbd>+<kbd>P</kbd> per aprire il riquadro comandi, quindi cercare e selezionare `Azure Functions: Create New Project...`</span><span class="sxs-lookup"><span data-stu-id="9f834-111">Type <kbd>Ctrl</kbd>+<kbd>P</kbd> to open up the command palette, then search for and select `Azure Functions: Create New Project...`</span></span>

   > <span data-ttu-id="9f834-112">**NOTA**</span><span class="sxs-lookup"><span data-stu-id="9f834-112">**NOTE**</span></span>
   >
   > <span data-ttu-id="9f834-113">Potrebbe chiedere se si desidera sovrascrivere un file, ad esempio `.gitignore`, risposte **alcun** a tutti!</span><span class="sxs-lookup"><span data-stu-id="9f834-113">It might ask if you want to overwrite some files such as `.gitignore`, answer **no** to all!</span></span>

   ![Creare un nuovo progetto](/media-drafts/7.create-new-project.png)

2. <span data-ttu-id="9f834-115">Selezionare la cartella in cui si desidera creare l'app per le funzioni (la prima opzione è la cartella corrente)</span><span class="sxs-lookup"><span data-stu-id="9f834-115">Select the folder where you want to create the function app (the first option is the current folder)</span></span>

   ![Seleziona cartella](/media-drafts/7.select-folder.png)

3. <span data-ttu-id="9f834-117">Scegliere `JavaScript` come il linguaggio desiderato.</span><span class="sxs-lookup"><span data-stu-id="9f834-117">Choose `JavaScript` as the desired language.</span></span>

   ![Seleziona lingua](/media-drafts/7.select-language.png)

4. <span data-ttu-id="9f834-119">Se il codice precedente completata correttamente verrà visualizzato il seguente i file creati nella cartella</span><span class="sxs-lookup"><span data-stu-id="9f834-119">If the above completed successfully you should see the below files created in the folder</span></span>

   ![App creata](/media-drafts/7.app-created.png)

## <a name="create-an-azure-function"></a><span data-ttu-id="9f834-121">Creare una funzione di Azure</span><span class="sxs-lookup"><span data-stu-id="9f834-121">Create an Azure Function</span></span>

<span data-ttu-id="9f834-122">Backup successivo è possibile creare la funzione di Azure, il frammento di codice che risponde a una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f834-122">Next up let's create the Azure Function itself, the piece of code that responds to a HTTP request.</span></span>

<span data-ttu-id="9f834-123">Anche in questo caso si intende usare l'estensione di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9f834-123">Again we are going to use the Visual Studio Code Extention.</span></span>

1.  <span data-ttu-id="9f834-124">Tipo di <kbd>Ctrl</kbd>+<kbd>P</kbd> per aprire il riquadro comandi, quindi cercare e selezionare `Azure Functions: Create Function...`</span><span class="sxs-lookup"><span data-stu-id="9f834-124">Type <kbd>Ctrl</kbd>+<kbd>P</kbd> to open up the command palette, then search for and select `Azure Functions: Create Function...`</span></span>

    ![Creare una nuova funzione](/media-drafts/7.create-function.png)

2.  <span data-ttu-id="9f834-126">Selezionare la cartella in cui la creazione del progetto di funzione in precedenza (la prima opzione è la cartella corrente)</span><span class="sxs-lookup"><span data-stu-id="9f834-126">Select the folder where you created the function project previously (the first option is the current folder)</span></span>

    ![Seleziona cartella](/media-drafts/7.select-current-project.png)

3.  <span data-ttu-id="9f834-128">Stiamo creando un _HTTP Trigger_, se si desidera selezionare tale opzione.</span><span class="sxs-lookup"><span data-stu-id="9f834-128">We are creating a _HTTP Trigger_, so select that option.</span></span>

    ![Seleziona cartella](/media-drafts/7.select-trigger.png)

4.  <span data-ttu-id="9f834-130">Digitare `MojifyImage` come il nome della funzione da creare</span><span class="sxs-lookup"><span data-stu-id="9f834-130">Type in `MojifyImage` as the name of the Function you want to create</span></span>

    ![Scegli nome](/media-drafts/7.choose-function-name.png)

5.  <span data-ttu-id="9f834-132">Scegliere `Anonymous` come il livello di autenticazione</span><span class="sxs-lookup"><span data-stu-id="9f834-132">Choose `Anonymous` as the authentication level</span></span>

    > <span data-ttu-id="9f834-133">Nota: scegliendo `Anonymous` la funzione è aperto per tutto il mondo e non sicuri.</span><span class="sxs-lookup"><span data-stu-id="9f834-133">NOTE: By choosing `Anonymous` the function is open to the world and insecure.</span></span>

    ![Scegli nome](/media-drafts/7.choose-auth-level.png)

## <a name="run-the-function-locally"></a><span data-ttu-id="9f834-135">Eseguire la funzione in locale</span><span class="sxs-lookup"><span data-stu-id="9f834-135">Run the function locally</span></span>

<span data-ttu-id="9f834-136">Una volta completato correttamente questi comandi aver convertito il progetto iniziale in un progetto di funzione con un _HTTP Trigger_ funzione chiamata `MojifyImage`.</span><span class="sxs-lookup"><span data-stu-id="9f834-136">Once these commands complete successfully you have converted the starter project to a function project with a _HTTP Trigger_ function called `MojifyImage`.</span></span>

<span data-ttu-id="9f834-137">Per verificare che tutto funzioni, è possibile eseguire l'app per le funzioni in locale, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9f834-137">To ensure everything is working you can run the function app locally, like so:</span></span>

```bash
func host start
```

<span data-ttu-id="9f834-138">Verrà avviata localmente; runtime della funzione Se tutto funziona correttamente alla fine si otterrà un risultato simile di seguito stampato nella console:</span><span class="sxs-lookup"><span data-stu-id="9f834-138">This starts the function's runtime locally; if it all works correctly eventually you should see something like the below printed to the console:</span></span>

```output
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

> <span data-ttu-id="9f834-139">**Nota**</span><span class="sxs-lookup"><span data-stu-id="9f834-139">**Note**</span></span>
>
> <span data-ttu-id="9f834-140">Questo è uno dei vantaggi dell'installazione del runtime di funzioni di Azure, consente di eseguire la funzione in locale usando la stessa tecnologia sottostante che potrebbe essere utilizzata per eseguire la funzione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="9f834-140">This is one of the advantages of installing the Azure Functions runtime, it allows us to run the function locally using the same underlying technology that would be used to run the function in production.</span></span>

<span data-ttu-id="9f834-141">Per garantire che la funzione funziona correttamente, visitare l'URL stampato nella console, questo viene stampato nella finestra del browser:</span><span class="sxs-lookup"><span data-stu-id="9f834-141">To ensure the function is working correctly, visit the URL printed to the console, this prints in the browser window:</span></span>

![App per le funzioni funzionante](/media-drafts/7.default-function-app-working.png)

## <a name="debug-the-function-locally"></a><span data-ttu-id="9f834-143">Eseguire il debug in locale la funzione</span><span class="sxs-lookup"><span data-stu-id="9f834-143">Debug the function locally</span></span>

> <span data-ttu-id="9f834-144">**Importante**</span><span class="sxs-lookup"><span data-stu-id="9f834-144">**Important**</span></span>
>
> <span data-ttu-id="9f834-145">Assicurarsi di avere chiuso la `func host start` comando sono stati eseguiti prima di provare a eseguirne il debug usando Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9f834-145">Make sure you exit the `func host start` command you just ran before trying to debug it using Visual Studio Code.</span></span>

<span data-ttu-id="9f834-146">Inoltre, possiamo eseguire _ed eseguire il debug_ l'applicazione all'interno di visual studio code.</span><span class="sxs-lookup"><span data-stu-id="9f834-146">Additionally, we can run _and debug_ the application inside visual studio code.</span></span>

<span data-ttu-id="9f834-147">Aggiungere un punto di interruzione il `index.js` file, nella parte superiore della funzione JavaScript, come segue:</span><span class="sxs-lookup"><span data-stu-id="9f834-147">Add a breakpoint to the `index.js` file, at the top of the JavaScript function, like so:</span></span>

![Aggiungi punto di interruzione](/media-drafts/7.add-breakpoint.png)

<span data-ttu-id="9f834-149">Per eseguire la funzione nella prima visita in modalità debug dal menu debug <kbd>Cmd<kbd> + <kbd>MAIUSC<kbd> + <kbd>1!d<kbd>.</span><span class="sxs-lookup"><span data-stu-id="9f834-149">To run the function in debug mode first visit the debug menu, <kbd>Cmd<kbd> + <kbd>Shift<kbd> + <kbd>D<kbd>.</span></span>

<span data-ttu-id="9f834-150">Selezionare quindi `Attach to JavaScript Functions` dalla configurazione di debug prima di premere infine il triangolo verde per avviare la sessione di debug.</span><span class="sxs-lookup"><span data-stu-id="9f834-150">Then pick `Attach to JavaScript Functions` from the debug configuration before finally pressing the green triangle to start the debug session.</span></span>

> <span data-ttu-id="9f834-151">**Nota**</span><span class="sxs-lookup"><span data-stu-id="9f834-151">**Note**</span></span>
>
> <span data-ttu-id="9f834-152">Il `Attach to JavaScript Functions` configurazione di debug è stato aggiunto automaticamente quando è stato creato il progetto di funzione.</span><span class="sxs-lookup"><span data-stu-id="9f834-152">The `Attach to JavaScript Functions` debug configuration was added automatically when you created the Function Project.</span></span>

<span data-ttu-id="9f834-153">In alternativa, è possibile premere <kbd>F5<kbd> da un punto qualsiasi nell'applicazione; viene eseguita l'ultima configurazione di debug.</span><span class="sxs-lookup"><span data-stu-id="9f834-153">Alternatively, you can press <kbd>F5<kbd> from anywhere in the application; this runs the last debug configuration.</span></span>

<span data-ttu-id="9f834-154">Il `func host start` comando verrà eseguito automaticamente; deve aprire un terminale alla fine con lo stesso output come indicato in precedenza:</span><span class="sxs-lookup"><span data-stu-id="9f834-154">The `func host start` command is run for you; a terminal should open with eventually the same output as above:</span></span>

```output
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

<span data-ttu-id="9f834-155">Perché si sta eseguendo il debug è anche vedere vengono visualizzati sulla barra di menu debug, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9f834-155">Since we are debugging you also see the debug menu bar appear, like so:</span></span>

![Barra dei Menu Debug](/media-drafts/7.debug-menu-bar.png)

<span data-ttu-id="9f834-157">Ora quando si visita l'URL riportato sopra si interrompe al punto di interruzione specificato ed è possibile scorrere la funzione.</span><span class="sxs-lookup"><span data-stu-id="9f834-157">Now when you visit the URL above it breaks at the breakpoint you specified, and you can step through the function.</span></span>

<!-- TODO Find Link -->

<span data-ttu-id="9f834-158">È possibile [altre informazioni](https://code.visualstudio.com/docs/editor/debugging) sul debug in Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f834-158">You can [learn more](https://code.visualstudio.com/docs/editor/debugging) about debugging in Visual Studio Code</span></span>
