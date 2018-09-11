<span data-ttu-id="275f3-101">In questa unità l'applicazione ASP.NET Core verrà caricata in un'app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-101">In this unit, you'll upload your ASP.NET Core application to an Azure Web App.</span></span>

## <a name="create-a-staging-deployment-slot"></a><span data-ttu-id="275f3-102">Creare uno slot di distribuzione di staging</span><span class="sxs-lookup"><span data-stu-id="275f3-102">Create a staging deployment slot</span></span>

1. <span data-ttu-id="275f3-103">Aprire la risorsa di tipo app Web creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="275f3-103">Open the Web App resource you created previously.</span></span> <span data-ttu-id="275f3-104">Se il pannello della risorsa è stato chiuso, è possibile ritrovarlo cercando l'app in **Tutte le risorse** oppure cercando il gruppo di risorse che la contiene in **Gruppi di risorse**.</span><span class="sxs-lookup"><span data-stu-id="275f3-104">If you have closed its resource blade, you can find it again by searching for the app in **All resources** or the containing resource group in **Resource groups**.</span></span>

1. <span data-ttu-id="275f3-105">Fare clic sulla voce di menu **Slot di distribuzione** dal riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="275f3-105">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="275f3-106">Nel pannello **Slot di distribuzione** fare clic sul pulsante **Aggiungi slot** sulla barra odi spostamento superiore del pannello Slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-106">Inside the **Deployment slots** blade, click the **Add Slot** button on the top navigation bar of the deployment slots blade.</span></span>

1. <span data-ttu-id="275f3-107">Nel portale di Azure si aprirà il pannello **Aggiungi uno slot** come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="275f3-107">The Azure portal opens the **Add a slot** blade as shown below.</span></span>

    1. <span data-ttu-id="275f3-108">Assegnare un nome allo slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-108">Give your deployment slot a name.</span></span> <span data-ttu-id="275f3-109">In questo caso, usare `staging`.</span><span class="sxs-lookup"><span data-stu-id="275f3-109">In this case, use `staging`.</span></span>

    2. <span data-ttu-id="275f3-110">Per scegliere un'**Origine configurazione**, sono disponibili due opzioni.</span><span class="sxs-lookup"><span data-stu-id="275f3-110">To choose a **Configuration Source**, you have two options.</span></span>

        * <span data-ttu-id="275f3-111">È possibile scegliere di clonare gli elementi di configurazione da qualsiasi slot di distribuzione esistente o app Web creata in Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-111">You can choose to clone the configuration elements from any existing deployment slot or Web App created on Azure.</span></span>
        * <span data-ttu-id="275f3-112">In alternativa, è possibile scegliere di non clonare alcun elemento di configurazione.</span><span class="sxs-lookup"><span data-stu-id="275f3-112">Or you can choose not to clone any configuration elements.</span></span> <span data-ttu-id="275f3-113">Selezionare l'opzione **Non clonare la configurazione da uno slot esistente**.</span><span class="sxs-lookup"><span data-stu-id="275f3-113">Select the option **Don't clone configuration from an existing slot**.</span></span>

        <span data-ttu-id="275f3-114">Per questo slot di distribuzione scegliere la seconda opzione, **Non clonare la configurazione da uno slot esistente**.</span><span class="sxs-lookup"><span data-stu-id="275f3-114">For this deployment slot, choose the second option: **Don't clone configuration from an existing slot**.</span></span> <span data-ttu-id="275f3-115">Verrà eseguita la configurazione diretta.</span><span class="sxs-lookup"><span data-stu-id="275f3-115">You will configure it directly.</span></span>

    ![Screenshot del portale di Azure che mostra la configurazione per un nuovo slot di distribuzione di staging.](../media/7-new-deployment-slot-blade.png)

1. <span data-ttu-id="275f3-117">Fare clic sul pulsante **OK** nella parte inferiore del pannello per creare un nuovo slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-117">Click the **OK** button at the bottom of the blade to create your new deployment slot.</span></span>

1. <span data-ttu-id="275f3-118">Dopo aver creato lo slot di distribuzione, il portale di Azure torna al pannello **Slot di distribuzione** dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="275f3-118">Once the deployment slot is successfully created, the Azure portal navigates you back to the **Deployment slots** blade of your web app.</span></span>

    <span data-ttu-id="275f3-119">A questo punto, è possibile visualizzare il nuovo slot di distribuzione appena creato.</span><span class="sxs-lookup"><span data-stu-id="275f3-119">Now, you can see the new deployment slot that you have just created.</span></span>

    ![Screenshot del portale di Azure che mostra il pannello Slot di distribuzione con il nuovo slot creato.](../media/7-deployment-slot-created.png)

1. <span data-ttu-id="275f3-121">Selezionare il nuovo slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-121">Select the new deployment slot.</span></span>

1. <span data-ttu-id="275f3-122">Il portale di Azure passerà alla pagina **Panoramica** relativa allo slot di distribuzione creato.</span><span class="sxs-lookup"><span data-stu-id="275f3-122">The Azure portal navigates to the **Overview** page of the newly created deployment slot.</span></span>

    ![Slot di distribuzione di staging](../media/7-deployment-slot-staging.png)

    <span data-ttu-id="275f3-124">Si noti l'**URL** dello slot di distribuzione di staging.</span><span class="sxs-lookup"><span data-stu-id="275f3-124">Notice the **URL** of the staging deployment slot.</span></span> <span data-ttu-id="275f3-125">Si tratta di un URL diverso rispetto a quello mostrato in precedenza, con il nome dello slot aggiunto come suffisso.</span><span class="sxs-lookup"><span data-stu-id="275f3-125">It is a different URL from what you saw previously, with the slot name appended.</span></span>

    <span data-ttu-id="275f3-126">Uno slot di distribuzione viene considerato come un'app Web completa in Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-126">A deployment slot is treated as a full Web App inside Azure.</span></span> <span data-ttu-id="275f3-127">Si tratta tuttavia di un tipo speciale, un elemento figlio dell'app Web originale che può essere scambiato con l'app Web originale.</span><span class="sxs-lookup"><span data-stu-id="275f3-127">However, it is a special type that is a child of the original web app and can be swapped with the original web app.</span></span>

    <span data-ttu-id="275f3-128">Se si seleziona l'**URL**, sarà visualizzata la stessa pagina predefinita creata da Azure per l'app Web la prima volta in cui è stata creata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-128">If you click the **URL**, you will see the same default page that Azure created for the web app the first time we created it in the Azure portal.</span></span>

<span data-ttu-id="275f3-129">Dopo aver creato la distribuzione di staging, è necessario configurare le **credenziali per la distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="275f3-129">Now that the staging deployment slot is created successfully, you need to configure **deployment credentials**.</span></span>

## <a name="create-deployment-credentials"></a><span data-ttu-id="275f3-130">Creare le credenziali per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="275f3-130">Create deployment credentials</span></span>

<span data-ttu-id="275f3-131">In Azure è necessario che le credenziali di distribuzione siano configurate prima di iniziare l'effettivo processo di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-131">Azure requires deployment credentials to be set up before you can start the actual deployment process.</span></span> <span data-ttu-id="275f3-132">Per questo motivo, si apprenderà come creare le credenziali di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-132">For that reason, you will learn how to create your own deployment credentials.</span></span>

1. <span data-ttu-id="275f3-133">Selezionare la voce di menu **Credenziali per la distribuzione** nel riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="275f3-133">Click the **Deployment credentials** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="275f3-134">Il portale di Azure passerà al pannello **Credenziali per la distribuzione** come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="275f3-134">The Azure portal navigates to the **Deployment credentials** blade as shown below.</span></span>

    <span data-ttu-id="275f3-135">Immettere un **nome utente** e una **password** e riconfermare la password.</span><span class="sxs-lookup"><span data-stu-id="275f3-135">Enter a **username** and **password** of your choice, and then confirm your password once again.</span></span>

    > [!NOTE]
    > <span data-ttu-id="275f3-136">Ricordarsi di annotare nome utente e password in modo che non siano dimenticati.</span><span class="sxs-lookup"><span data-stu-id="275f3-136">Make sure you write down your username and password so that you don't forget them!</span></span> <span data-ttu-id="275f3-137">Queste credenziali saranno necessarie per avviare il caricamento e la distribuzione del codice in Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-137">You will need them later when we start uploading and deploying our code to Azure.</span></span>

    ![Screenshot del portale di Azure che mostra il pannello Credenziali per la distribuzione dello slot di staging con credenziali di esempio nei campi obbligatori.](../media/7-deployment-credentials.png)

1. <span data-ttu-id="275f3-139">Fare clic sul pulsante **Salva** nella parte superiore del pannello **Credenziali per la distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="275f3-139">Click the **Save** button at the top of the **Deployment credentials** blade.</span></span>

<span data-ttu-id="275f3-140">Le credenziali per la distribuzione sono state create. Ora è necessario configurare altre opzioni di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-140">Now that the deployment credentials are created successfully, you need to configure other deployment options.</span></span>

## <a name="use-a-local-git-repository-as-your-deployment-option"></a><span data-ttu-id="275f3-141">Usare un repository GIT locale come opzione per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="275f3-141">Use a local Git repository as your deployment option</span></span>

<span data-ttu-id="275f3-142">Verrà creato un repository Git locale in Azure per consentire di avviare il caricamento del codice.</span><span class="sxs-lookup"><span data-stu-id="275f3-142">Next, we'll create a local Git repository in Azure so you can start uploading your code.</span></span>

1. <span data-ttu-id="275f3-143">Nell'app Web dello slot di distribuzione di **staging** selezionare la voce di menu **Opzioni di distribuzione** dal riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="275f3-143">Within the **staging** deployment slot Web App, click the **Deployment options** menu item on the left-hand navigation.</span></span>

1. <span data-ttu-id="275f3-144">Il portale di Azure passerà al pannello **Opzioni di distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="275f3-144">The Azure portal navigates to the **Deployment options** blade.</span></span>

1. <span data-ttu-id="275f3-145">Fare clic su **Scegliere l'origine** per configurare le impostazioni necessarie.</span><span class="sxs-lookup"><span data-stu-id="275f3-145">Click on the **Choose Source** to configure the required settings.</span></span>

1. <span data-ttu-id="275f3-146">Il portale di Azure visualizza le opzioni disponibili che è possibile configurare e usare.</span><span class="sxs-lookup"><span data-stu-id="275f3-146">The Azure portal displays the available options that you can configure and use.</span></span> <span data-ttu-id="275f3-147">In questo caso scegliere l'opzione **Repository Git locale**.</span><span class="sxs-lookup"><span data-stu-id="275f3-147">In our case, choose the **Local Git Repository** option.</span></span>

1. <span data-ttu-id="275f3-148">Verrà visualizzato nuovamente il pannello **Opzioni di distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="275f3-148">You will be returned to the **Deployment option** blade.</span></span> <span data-ttu-id="275f3-149">Fare clic sul pulsante **OK** nella parte inferiore del pannello per configurare l'ordine della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-149">Click the **OK** button at the bottom of the blade to set up the deployment source.</span></span>

1. <span data-ttu-id="275f3-150">Passare quindi alla sezione **Centro distribuzione (anteprima)** nel riquadro di spostamento a sinistra per visualizzare i dettagli della nuova distribuzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-150">Now, navigate to the **Deployment Center (Preview)** section on the left-side navigation to view the new deployment details.</span></span>

    ![Screenshot del portale di Azure che mostra il pannello Centro distribuzione dello slot di distribuzione con l'URI di git clone per lo slot evidenziato.](../media/7-staging-after-setting-git.png)

    <span data-ttu-id="275f3-152">L'informazione rilevante in questo contesto è l'**URI di git clone**, ovvero l'URL del repository Git locale che verrà usato come **remoto** per il repository dell'applicazione Web locale.</span><span class="sxs-lookup"><span data-stu-id="275f3-152">The important information to note here is the **Git Clone Uri**, which is the local Git repository URL that you will use as a **remote** for your local web application repository.</span></span>

<span data-ttu-id="275f3-153">A questo punto è possibile iniziare a caricare il codice nello slot di distribuzione di staging.</span><span class="sxs-lookup"><span data-stu-id="275f3-153">It is time to start uploading your code to the staging deployment slot.</span></span>

## <a name="install-git-on-your-machine"></a><span data-ttu-id="275f3-154">Installare Git nel computer</span><span class="sxs-lookup"><span data-stu-id="275f3-154">Install Git on your machine</span></span>

<span data-ttu-id="275f3-155">Se non è già disponibile, installare Git nel computer Linux.</span><span class="sxs-lookup"><span data-stu-id="275f3-155">If you don't already have it, install Git on your Linux machine.</span></span>

> [!NOTE]
> <span data-ttu-id="275f3-156">Queste istruzioni sono relative a Ubuntu 18.04, quindi la procedura per installare Git potrebbe risultare diversa per la distribuzione e la versione specifiche in uso.</span><span class="sxs-lookup"><span data-stu-id="275f3-156">These instructions are for Ubuntu 18.04, so the steps to install Git may differ for your distribution and version.</span></span> <span data-ttu-id="275f3-157">Per informazioni sui passaggi specifici, vedere le [istruzioni di installazione per Git in Linux](https://git-scm.com/download/linux).</span><span class="sxs-lookup"><span data-stu-id="275f3-157">Check out the [Linux Git installation instructions](https://git-scm.com/download/linux) for the appropriate steps.</span></span>

1. <span data-ttu-id="275f3-158">Aprire una nuova finestra **Terminale**</span><span class="sxs-lookup"><span data-stu-id="275f3-158">Open a new **Terminal** window</span></span>

1. <span data-ttu-id="275f3-159">Digitare il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="275f3-159">Type the following command.</span></span> <span data-ttu-id="275f3-160">Verrà richiesto di immettere la password utente Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="275f3-160">You will be prompted to enter your Ubuntu user password.</span></span>

    ```console
    sudo apt-get update
    ```

1. <span data-ttu-id="275f3-161">Dopo aver eseguito l'aggiornamento, digitare il comando seguente per installare GIT in locale.</span><span class="sxs-lookup"><span data-stu-id="275f3-161">Once the update is successful, type the following command to install Git locally.</span></span> <span data-ttu-id="275f3-162">Verrà richiesto di accettare l'installazione di GIT nel computer.</span><span class="sxs-lookup"><span data-stu-id="275f3-162">You will be prompted to accept the installation of Git on your machine.</span></span>

    ```console
    sudo apt-get install git-core
    ```

1. <span data-ttu-id="275f3-163">Per verificare che GIT sia ora installato, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="275f3-163">To verify that Git is now installed, type the following command:</span></span>

    ```console
    git --version
    ```

   <span data-ttu-id="275f3-164">Se l'installazione è stata eseguita correttamente, verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="275f3-164">If the installation is successful, you will see the following output:</span></span>

    ```console
    git version 2.17.1
    ```

1. <span data-ttu-id="275f3-165">È sempre consigliabile configurare le impostazioni di Git specificando nome e indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="275f3-165">It is always a good practice to configure Git settings by providing your name and email.</span></span> <span data-ttu-id="275f3-166">A tale scopo, eseguire i comandi seguenti, sostituendo i segnaposto `{your name}` e `{your email}` con il proprio nome e il proprio indirizzo di posta elettronica, senza le parentesi graffe `{}`:</span><span class="sxs-lookup"><span data-stu-id="275f3-166">For that, you need to issue the following commands, replacing the `{your name}` and `{your email}` placeholders with your own name and email (without the `{}` curly braces):</span></span>

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. <span data-ttu-id="275f3-167">Per verificare che le informazioni siano state registrate da Git, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="275f3-167">To verify that your information has been recorded by Git, type the following command:</span></span>

    ```console
    cat ~/.gitconfig
    ```

   <span data-ttu-id="275f3-168">Sarà visualizzato quanto riportato di seguito insieme a nome e indirizzo di posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="275f3-168">You should be seeing the following, with your name and email shown:</span></span>

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a><span data-ttu-id="275f3-169">Inizializzare un repository GIT locale per l'app Web</span><span class="sxs-lookup"><span data-stu-id="275f3-169">Initialize a local Git repository for your web app</span></span>

<span data-ttu-id="275f3-170">Per iniziare a usare GIT, è necessario inizializzare un repository GIT locale per l'app Web.</span><span class="sxs-lookup"><span data-stu-id="275f3-170">To start using Git, you need to initialize a local Git repository for the web app.</span></span>

1. <span data-ttu-id="275f3-171">Aprire una nuova finestra **Terminale**.</span><span class="sxs-lookup"><span data-stu-id="275f3-171">Open a new **Terminal** window.</span></span>

1. <span data-ttu-id="275f3-172">Passare alla cartella radice del contenuto dell'app Web creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="275f3-172">Navigate to the content root folder of the web app you created previously.</span></span> <span data-ttu-id="275f3-173">È possibile digitare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="275f3-173">You can type the following:</span></span>

    ```console
    cd ~/Documents/BestBikeApp/
    ```

1. <span data-ttu-id="275f3-174">Eseguire il comando seguente per inizializzare un nuovo repository Git:</span><span class="sxs-lookup"><span data-stu-id="275f3-174">Initialize a new Git repository by issuing the following command:</span></span>

    ```console
    git init
    ```

    <span data-ttu-id="275f3-175">Se il comando ha esito positivo, viene visualizzato un messaggio simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="275f3-175">If the command is successful, you receive a message like the following:</span></span>

    ```console
    Initialized empty Git repository in /home/{your-user}/Documents/BestBikeApp/.git/
    ```

1. <span data-ttu-id="275f3-176">Eseguire lo staging di tutti i file dell'app Web in GIT.</span><span class="sxs-lookup"><span data-stu-id="275f3-176">Stage all the web app files to Git.</span></span>

   <span data-ttu-id="275f3-177">Nel passaggio successivo GIT riconosce i file dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="275f3-177">The next step is to let Git know about your web app files.</span></span> <span data-ttu-id="275f3-178">Aggiungere quindi tutti i file della directory di lavoro in modo che siano **gestiti temporaneamente** da GIT.</span><span class="sxs-lookup"><span data-stu-id="275f3-178">Do that by adding all the files of the working directory so that they get **staged** by Git.</span></span> <span data-ttu-id="275f3-179">Digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="275f3-179">Type the following command:</span></span>

    ```console
    git add .
    ```

    <span data-ttu-id="275f3-180">Il comando precedente aggiunge tutti i file, rappresentati da ".", allo stato di staging di GIT.</span><span class="sxs-lookup"><span data-stu-id="275f3-180">The command above adds all files, represented by the ".", to the staging state of Git.</span></span>

1. <span data-ttu-id="275f3-181">A questo punto, eseguire il commit delle modifiche in GIT.</span><span class="sxs-lookup"><span data-stu-id="275f3-181">Now, you need to commit your changes to Git.</span></span>

   <span data-ttu-id="275f3-182">Dopo aver completato lo staging dei file in GIT, è necessario eseguire il commit dei file nella **cronologia del commit GIT** nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="275f3-182">Once you stage the files with Git, you need to commit your files to the **Git commit history** on your local machine.</span></span> <span data-ttu-id="275f3-183">A tal scopo, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="275f3-183">You do that by typing the following command:</span></span>

    ```console
   git commit -m "Initial create"
    ```

   <span data-ttu-id="275f3-184">Il comando `commit` accetta l'argomento `-m` per includere un messaggio con il commit che si sta creando.</span><span class="sxs-lookup"><span data-stu-id="275f3-184">The `commit` command accepts  `-m` argument to include a message with the commit you are creating.</span></span> <span data-ttu-id="275f3-185">Quando si effettuerà il push del codice in Azure, sarà possibile visualizzare lo stesso messaggio archiviato con questo commit specifico.</span><span class="sxs-lookup"><span data-stu-id="275f3-185">Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.</span></span>

## <a name="add-a-remote-for-the-local-git-repository"></a><span data-ttu-id="275f3-186">Aggiungere un repository remoto per il repository GIT locale</span><span class="sxs-lookup"><span data-stu-id="275f3-186">Add a remote for the local Git repository</span></span>

<span data-ttu-id="275f3-187">A questo punto è stato inizializzato un nuovo repository GIT locale.</span><span class="sxs-lookup"><span data-stu-id="275f3-187">At this point, you have successfully initialized a new local Git repository.</span></span> <span data-ttu-id="275f3-188">È stato anche eseguito il commit di tutti i file dell'app Web in GIT.</span><span class="sxs-lookup"><span data-stu-id="275f3-188">In addition, you've committed all of your web app files to Git.</span></span> <span data-ttu-id="275f3-189">Rimane da aggiungere un repository **remoto** al quale connettere il repository GIT locale che è stato ospitato in Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-189">What remains is to add a **remote** to connect your local Git repository to that hosted on Azure.</span></span>

<span data-ttu-id="275f3-190">A tale scopo, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="275f3-190">To do so, you need to:</span></span>

1. <span data-ttu-id="275f3-191">Copiare l'**URL clone GIT** visto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="275f3-191">Copy the **Git clone url** that you saw above.</span></span>

1. <span data-ttu-id="275f3-192">Dopo averlo copiato, tornare alla finestra **Terminale** ed eseguire il comando GIT seguente:</span><span class="sxs-lookup"><span data-stu-id="275f3-192">Once copied, you go back to the **Terminal** window and issue the following Git command:</span></span>

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    <span data-ttu-id="275f3-193">Il comando Git precedente consente al repository GIT locale di collegarsi a quello ospitato in Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-193">The above Git command hooks your local Git repository to the one hosted on Azure.</span></span> <span data-ttu-id="275f3-194">È ora possibile effettuare il push e il pull tra il repository GIT locale e quello remoto.</span><span class="sxs-lookup"><span data-stu-id="275f3-194">Now, you can start pushing and pulling between the local and remote Git repositories!</span></span>

1. <span data-ttu-id="275f3-195">Per verificare il comando precedente, digitare il comando GIT seguente:</span><span class="sxs-lookup"><span data-stu-id="275f3-195">To verify the above command, type the following Git command:</span></span>

    ```console
    git remote -v
    ```

    <span data-ttu-id="275f3-196">Il comando precedente genera l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="275f3-196">The command above generates the following output:</span></span>

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a><span data-ttu-id="275f3-197">Effettuare il push del codice in Azure</span><span class="sxs-lookup"><span data-stu-id="275f3-197">Push your code to Azure</span></span>

<span data-ttu-id="275f3-198">Ora che in Azure il repository GIT locale è collegato al repository GIT remoto, sarà possibile sviluppare e compilare l'app ed eseguire il push dell'app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-198">Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app, and then push your web app to Azure.</span></span>

1. <span data-ttu-id="275f3-199">Digitare il comando GIT seguente per effettuare il push del ramo **principale** nel repository GIT remoto in Azure:</span><span class="sxs-lookup"><span data-stu-id="275f3-199">Type the following Git command to push your **master** branch to the remote Git repository on Azure:</span></span>

    ```console
    git push origin master
    ```

1. <span data-ttu-id="275f3-200">Verrà richiesto di immettere la password configurata nella sezione precedente **Credenziali per la distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="275f3-200">You will be prompted to enter the password that you have configured in the **Deployment credentials** section above.</span></span> <span data-ttu-id="275f3-201">Immetter la password e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="275f3-201">Enter your password and hit Enter.</span></span> <span data-ttu-id="275f3-202">GIT inizia a caricare i file di cui è stato eseguito il commit nel repository GIT remoto di Azure configurato, all'interno dello slot di distribuzione di staging.</span><span class="sxs-lookup"><span data-stu-id="275f3-202">Git starts uploading your committed files to the Azure remote Git repository configured under the staging deployment slot.</span></span>

## <a name="verify-the-web-app-is-uploaded-to-azure"></a><span data-ttu-id="275f3-203">Verificare che l'app Web sia caricata in Azure</span><span class="sxs-lookup"><span data-stu-id="275f3-203">Verify the web app is uploaded to Azure</span></span>

1. <span data-ttu-id="275f3-204">Accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-204">Sign in to the Azure portal.</span></span>

1. <span data-ttu-id="275f3-205">Selezionare la voce di menu **Tutte le risorse** dal menu di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="275f3-205">Click on the **All Resources** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="275f3-206">Il portale di Azure passerà all'elenco di tutte le risorse create in Azure fino a questo momento.</span><span class="sxs-lookup"><span data-stu-id="275f3-206">The Azure portal navigates you to the list of all resources created on Azure so far.</span></span>

1. <span data-ttu-id="275f3-207">Fare clic sullo slot di staging creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="275f3-207">Click on the staging slot created above.</span></span> <span data-ttu-id="275f3-208">Tenere presente che uno slot di distribuzione viene considerato come se fosse un'app Web e pertanto sarà visualizzato come risorsa dell'app Web in **Tutte le risorse**.</span><span class="sxs-lookup"><span data-stu-id="275f3-208">Remember, a deployment slot is considered a web app, and hence, it will appear as a web app resource under **All Resources**.</span></span>

1. <span data-ttu-id="275f3-209">Dopo essere passati al pannello dello slot di distribuzione di staging, scegliere **Opzioni di distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="275f3-209">Once you arrive to the staging deployment slot blade, go to **Deployment options**.</span></span>

    <span data-ttu-id="275f3-210">Si noti che il primo commit disponibile in locale nel computer è ora caricato nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-210">You will see that your first commit that you have locally on your machine is now uploaded to the Azure portal.</span></span>

    <span data-ttu-id="275f3-211">Effettuando il push del codice in locale nel repository Git remoto ospitato in Azure, Azure ha registrato l'operazione.</span><span class="sxs-lookup"><span data-stu-id="275f3-211">By pushing your code locally to the remote Git repository hosted on Azure, Azure has recorded this operation.</span></span>

    <span data-ttu-id="275f3-212">Ogni volta che si effettua il push del codice in Azure, verrà visualizzato un nuovo record insieme al messaggio digitato durante il commit delle modifiche in locale nel computer.</span><span class="sxs-lookup"><span data-stu-id="275f3-212">Every time you push your code to Azure, you will see a new record, together with the message that you type when committing your changes locally on your machine.</span></span>

    ![Screenshot del portale di Azure che mostra un repository Git recente nel pannello Opzioni di distribuzione.](../media/7-staging-deployment-slot-after-uploading-files.png)

1. <span data-ttu-id="275f3-214">Osserviamo ora l'URL dello **slot di staging**.</span><span class="sxs-lookup"><span data-stu-id="275f3-214">Let's visit the **staging slot** URL.</span></span> <span data-ttu-id="275f3-215">L'URL è stato specificato in precedenza. Se però è stato dimenticato, è sempre possibile accedere alla pagina **Panoramica** dello slot di distribuzione di staging per recuperarlo.</span><span class="sxs-lookup"><span data-stu-id="275f3-215">The URL was mentioned above, however, if you forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.</span></span>

1. <span data-ttu-id="275f3-216">Digitare l'URL seguente nella barra degli indirizzi del browser: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="275f3-216">Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span></span>

    ![Screenshot che mostra una visualizzazione del Web browser del sito Web dello slot di distribuzione di staging.](../media/7-staging-slot-hosted-online.png)

<span data-ttu-id="275f3-218">I file dell'app Web locale sono stati caricati nello slot di distribuzione di staging in Azure.</span><span class="sxs-lookup"><span data-stu-id="275f3-218">You have successfully uploaded your local web app files to the staging deployment slot on Azure.</span></span>

## <a name="swapping-the-staging-and-production-deployment-slots"></a><span data-ttu-id="275f3-219">Scambio di slot di distribuzione di staging e di produzione</span><span class="sxs-lookup"><span data-stu-id="275f3-219">Swapping the staging and production deployment slots</span></span>

<span data-ttu-id="275f3-220">Ora che l'applicazione è attiva e in esecuzione nello slot di distribuzione di staging ospitato in Azure, è possibile scambiare questo slot con quello di produzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-220">Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one.</span></span> <span data-ttu-id="275f3-221">A questo scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="275f3-221">To do so, follow these steps:</span></span>

1. <span data-ttu-id="275f3-222">Passare al pannello dell'app Web originale creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="275f3-222">Navigate to the original Web App blade created early.</span></span> <span data-ttu-id="275f3-223">L'app Web originale è disponibile nel pannello **Tutte le risorse**.</span><span class="sxs-lookup"><span data-stu-id="275f3-223">You can find the original Web App from the **All resources** blade.</span></span>

1. <span data-ttu-id="275f3-224">Fare clic sulla voce di menu **Slot di distribuzione** dal riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="275f3-224">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="275f3-225">Fare clic sul pulsante **Scambia** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="275f3-225">Click on the **Swap** button at the top of the blade.</span></span>

1. <span data-ttu-id="275f3-226">Nel portale di Azure viene visualizzato il pannello **Scambia**.</span><span class="sxs-lookup"><span data-stu-id="275f3-226">The Azure portal navigates you to the **Swap** blade.</span></span>

1. <span data-ttu-id="275f3-227">Per il campo **Scambia**, selezionare **Scambia**.</span><span class="sxs-lookup"><span data-stu-id="275f3-227">For the **Swap** field, select **Swap**.</span></span>

1. <span data-ttu-id="275f3-228">Per il campo **Origine**, selezionare **Staging**.</span><span class="sxs-lookup"><span data-stu-id="275f3-228">For the **Source** field, select **Staging**.</span></span>

1. <span data-ttu-id="275f3-229">Per il campo **Destinazione**, selezionare **Produzione**.</span><span class="sxs-lookup"><span data-stu-id="275f3-229">For the **Destination** field, select **Production**.</span></span>

    ![Screenshot del portale di Azure che mostra il pannello Scambia dello slot di distribuzione.](../media/7-swap-blade.png)

1. <span data-ttu-id="275f3-231">Fare clic sul pulsante **OK** nella parte inferiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="275f3-231">Click on the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="275f3-232">Azure avvierà il processo di scambio.</span><span class="sxs-lookup"><span data-stu-id="275f3-232">Azure starts the swapping process.</span></span> <span data-ttu-id="275f3-233">Questa operazione richiede generalmente pochi secondi, a seconda delle dimensioni dell'app Web sottoposta a scambio.</span><span class="sxs-lookup"><span data-stu-id="275f3-233">Usually, this operation takes a few seconds, depending on the size of the web app being swapped.</span></span>

1. <span data-ttu-id="275f3-234">Al termine dell'operazione, visitare l'URL dell'app Web: [ https://bestbike.azurewebsites.net/ ](https://bestbike.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="275f3-234">Once the operation ends, visit the web app URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span></span>

    ![Screenshot che mostra una visualizzazione del Web browser dello slot di distribuzione di staging precedente ospitato ora come app Web primaria.](../media/7-web-app-page.png)

    <span data-ttu-id="275f3-236">L'operazione di scambio è stata completata.</span><span class="sxs-lookup"><span data-stu-id="275f3-236">The swapping operation has been successful!</span></span> <span data-ttu-id="275f3-237">È ora possibile osservare che il codice che è stato caricato nello slot di distribuzione di staging è anche ospitato nello slot di produzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-237">You can now see the code that you uploaded to the staging deployment slot also being hosted on the production slot.</span></span>

1. <span data-ttu-id="275f3-238">A questo punto, visitare l'URL dello slot di staging [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="275f3-238">Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span></span>

    ![Screenshot che mostra una visualizzazione del Web browser dello slot di distribuzione primario precedente ospitato ora come app Web dello slot di distribuzione di staging.](../media/7-staging-after-swapping.png)

    <span data-ttu-id="275f3-240">Lo slot di distribuzione di staging ora contiene i file dell'applicazione Web predefiniti e originali che erano in precedenza ospitati nello slot di produzione.</span><span class="sxs-lookup"><span data-stu-id="275f3-240">The staging deployment slot now contains the original, default web application files that were previously hosted under the production slot.</span></span>

<span data-ttu-id="275f3-241">Congratulazioni.</span><span class="sxs-lookup"><span data-stu-id="275f3-241">Congratulations!</span></span> <span data-ttu-id="275f3-242">L'app Web è stata caricata in Azure e gli slot di distribuzione sono stati scambiati.</span><span class="sxs-lookup"><span data-stu-id="275f3-242">You have successfully uploaded your web app to Azure and swapped deployment slots.</span></span>
