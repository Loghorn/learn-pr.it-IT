<span data-ttu-id="eba9e-101">In questo esercizio si caricherà l'applicazione ASP.NET Core in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-101">In this exercise, you'll upload your ASP.NET Core application to Azure.</span></span>

## <a name="create-a-staging-deployment-slot"></a><span data-ttu-id="eba9e-102">Creare uno slot di distribuzione di staging</span><span class="sxs-lookup"><span data-stu-id="eba9e-102">Create a staging deployment slot</span></span>

1. <span data-ttu-id="eba9e-103">Individuare e selezionare la voce di menu **Slot di distribuzione** dal menu di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="eba9e-103">Locate and click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="eba9e-104">Azure aprirà il pannello **Slot di distribuzione** come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="eba9e-104">Azure opens the **Deployment slots** blade as shown below.</span></span>

    ![Slot di distribuzione](../media-draft/7-deployment-slot-blade.png)

1. <span data-ttu-id="eba9e-106">Individuare e selezionare il pulsante **+ Aggiungi slot** nella barra di spostamento superiore del pannello Slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-106">Locate and click the **+ Add Slot** button on the top navigation bar of the deployment slots blade.</span></span>

1. <span data-ttu-id="eba9e-107">Nel portale di Azure si aprirà il pannello **Aggiungi uno slot** come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="eba9e-107">The Azure portal opens the **Add a slot** blade as shown below.</span></span>

    ![Aggiungi uno slot](../media-draft/7-new-deployment-slot-blade.png)

    <span data-ttu-id="eba9e-109">Assegnare un nome allo slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-109">Give your deployment slot a name.</span></span> <span data-ttu-id="eba9e-110">In questo caso, usare **Staging**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-110">In this case, use **Staging**.</span></span>

    <span data-ttu-id="eba9e-111">Per scegliere un'**origine della configurazione**, sono disponibili 2 opzioni.</span><span class="sxs-lookup"><span data-stu-id="eba9e-111">To choose a **Configuration Source**, you have 2 options.</span></span>

    1. <span data-ttu-id="eba9e-112">È possibile scegliere di clonare gli elementi di configurazione di altri slot o un'app Web creata in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-112">Select to clone the configuration elements from any other slot or web app created on Azure.</span></span>

    1. <span data-ttu-id="eba9e-113">È possibile scegliere di non clonare alcun elemento di configurazione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-113">You can choose not to clone any configuration elements.</span></span> <span data-ttu-id="eba9e-114">In questo caso, è necessario selezionare l'opzione **Non clonare la configurazione da uno slot esistente**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-114">In this case, you select the option **Don't clone configuration from an existing slot**.</span></span>

    <span data-ttu-id="eba9e-115">Selezionare la seconda opzione **Non clonare la configurazione da uno slot esistente**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-115">Choose the second option, **Don't clone configuration from an existing slot**.</span></span>

1. <span data-ttu-id="eba9e-116">Individuare e fare clic sul pulsante **OK** nella parte inferiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="eba9e-116">Locate and click the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="eba9e-117">Dopo aver creato lo slot di distribuzione, il portale di Azure passa nuovamente al pannello **Slot di distribuzione** dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="eba9e-117">Once the deployment slot is successfully created, the Azure portal navigates you back to the **Deployment slots** blade of your web app.</span></span>

    <span data-ttu-id="eba9e-118">A questo punto, è possibile visualizzare il nuovo slot di distribuzione appena creato.</span><span class="sxs-lookup"><span data-stu-id="eba9e-118">Now, you can see the new deployment slot that you have just created.</span></span>

    ![Slot di distribuzione creato](../media-draft/7-deployment-slot-created.png)

1. <span data-ttu-id="eba9e-120">Individuare e selezionare il nuovo slot di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-120">Locate and click the new deployment slot.</span></span>

1. <span data-ttu-id="eba9e-121">Il portale di Azure passerà alla pagina **Panoramica** relativa allo slot di distribuzione creato.</span><span class="sxs-lookup"><span data-stu-id="eba9e-121">The Azure portal navigates to the **Overview** page of the newly created deployment slot.</span></span>

    ![Slot di distribuzione di staging](../media-draft/7-deployment-slot-staging.png)

    <span data-ttu-id="eba9e-123">Si noti l'**URL** dello slot di distribuzione di staging.</span><span class="sxs-lookup"><span data-stu-id="eba9e-123">Notice the **URL** of the staging deployment slot.</span></span> <span data-ttu-id="eba9e-124">È esattamente l'URL visto precedentemente all'inizio dell'unità.</span><span class="sxs-lookup"><span data-stu-id="eba9e-124">It is the exact same URL you saw previously at the beginning of this unit.</span></span>

    <span data-ttu-id="eba9e-125">In Azure uno slot di distribuzione viene considerato come se fosse un'app Web.</span><span class="sxs-lookup"><span data-stu-id="eba9e-125">A deployment slot is treated as a web app inside Azure.</span></span> <span data-ttu-id="eba9e-126">Si tratta tuttavia di un tipo speciale, un elemento figlio dell'app Web originale che può essere scambiato con l'app Web originale.</span><span class="sxs-lookup"><span data-stu-id="eba9e-126">However, it is a special type that is a child of the original web app and can be swapped with the original web app.</span></span>

    <span data-ttu-id="eba9e-127">Se si seleziona l'**URL**, sarà visualizzata la stessa pagina predefinita creata da Azure per l'app Web la prima volta in cui è stata creata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-127">If you click the **URL**, you will see the same default page that Azure created for the web app the first time we created it in the Azure portal.</span></span>

<span data-ttu-id="eba9e-128">Dopo aver creato la distribuzione di staging, è necessario configurare le **credenziali per la distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-128">Now that the staging deployment slot is created successfully, you need to configure **deployment credentials**.</span></span>

## <a name="create-deployment-credentials"></a><span data-ttu-id="eba9e-129">Creare le credenziali per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="eba9e-129">Create deployment credentials</span></span>

<span data-ttu-id="eba9e-130">In Azure è necessario che le credenziali di distribuzione siano configurate prima di iniziare l'effettivo processo di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-130">Azure requires deployment credentials to be set up before you can start the actual deployment process.</span></span> <span data-ttu-id="eba9e-131">Per questo motivo, si apprenderà come creare le credenziali di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-131">For that reason, you will learn how to create your own deployment credentials.</span></span>

1. <span data-ttu-id="eba9e-132">Individuare e selezionare la voce di menu **Credenziali per la distribuzione** dal menu di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="eba9e-132">Locate and click the **Deployment credentials** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="eba9e-133">Il portale di Azure passerà al pannello **Credenziali per la distribuzione** come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="eba9e-133">The Azure portal navigates to the **Deployment credentials** blade as shown below.</span></span>

    ![Credenziali per la distribuzione](../media-draft/7-deployment-credentials.png)

    <span data-ttu-id="eba9e-135">Immettere un **nome utente** e una **password** e riconfermare la password.</span><span class="sxs-lookup"><span data-stu-id="eba9e-135">Enter a **username** and **password** of your choice, and then confirm your password once again.</span></span>

    > <span data-ttu-id="eba9e-136">Ricordarsi di annotare nome utente e password in modo che non siano dimenticati.</span><span class="sxs-lookup"><span data-stu-id="eba9e-136">Make sure you write down your username and password so that you don't forget them!</span></span> <span data-ttu-id="eba9e-137">Queste credenziali saranno necessarie per avviare il caricamento e la distribuzione del codice in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-137">You will need them later when we start uploading and deploying our code to Azure.</span></span>

    <span data-ttu-id="eba9e-138">Dopo aver scelto nome utente e password, individuare e fare clic sul pulsante **Salva** nella parte superiore del pannello **Credenziali per la distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-138">Once you decide on the username and password, locate and click the **Save** button at the top of the **Deployment credentials** blade.</span></span>

<span data-ttu-id="eba9e-139">Le credenziali per la distribuzione sono state create. Ora è necessario configurare le **opzioni di distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-139">Now that the deployment credentials are created successfully, you need to configure **deployment options**.</span></span>

## <a name="use-a-local-git-repository-as-your-deployment-option"></a><span data-ttu-id="eba9e-140">Usare un repository GIT locale come opzione per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="eba9e-140">Use a local Git repository as your deployment option</span></span>

<span data-ttu-id="eba9e-141">A questo punto, perché sia possibile avviare il processo di caricamento del codice, è necessario creare un repository GIT locale in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-141">Now, it is time to create a local Git repository in Azure so that you can start the process of uploading your code.</span></span>

1. <span data-ttu-id="eba9e-142">Individuare e selezionare la voce di menu **Opzioni di distribuzione** dal menu di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="eba9e-142">Locate and click the **Deployment options** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="eba9e-143">Il portale di Azure passerà al pannello **Opzioni di distribuzione** come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="eba9e-143">The Azure portal navigates to the **Deployment options** blade as shown below.</span></span>

    ![Opzioni di distribuzione](../media-draft/7-deployment-options.png)

1. <span data-ttu-id="eba9e-145">Fare clic su **Scegliere l'origine / Configurare le impostazioni necessarie**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-145">Click on **Choose source / Configure required settings**.</span></span>

    ![Credenziali per la distribuzione](../media-draft/7-deployment-sources.png)

1. <span data-ttu-id="eba9e-147">Il portale di Azure visualizza le opzioni disponibili che è possibile configurare e usare.</span><span class="sxs-lookup"><span data-stu-id="eba9e-147">The Azure portal displays the available options that you can configure and use.</span></span> <span data-ttu-id="eba9e-148">In questo caso scegliere l'opzione **Repository GIT locale**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-148">In our case, choose the **Local Git Repository** option.</span></span>

    ![Repository GIT locale](../media-draft/7-local-git-repo.png)

1. <span data-ttu-id="eba9e-150">Individuare e fare clic sul pulsante **OK** nella parte inferiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="eba9e-150">Locate and click the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="eba9e-151">Individuare e selezionare la voce di menu **Panoramica** dal menu di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="eba9e-151">Locate and click the **Overview** menu item on the left-side navigation.</span></span>

    ![Slot di distribuzione con GIT configurato](../media-draft/7-staging-after-setting-git.png)

    <span data-ttu-id="eba9e-153">Si notino due informazioni importanti:</span><span class="sxs-lookup"><span data-stu-id="eba9e-153">Notice two important pieces of information:</span></span>
    - <span data-ttu-id="eba9e-154">**Nome utente GIT/distribuzione**: sono le credenziali che si useranno per la connessione al repository GIT locale in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-154">**Git/Deployment username**: These are the credentials that you will use later on to connect to the local Git repository on Azure.</span></span>

    - <span data-ttu-id="eba9e-155">**URL clone GIT**: è l'URL del repository GIT locale che si userà come **remoto** per il repository dell'applicazione Web locale.</span><span class="sxs-lookup"><span data-stu-id="eba9e-155">**Git clone url**: This is the local Git repository URL that you will use as a **remote** for your local web application repository.</span></span>

<span data-ttu-id="eba9e-156">A questo punto è possibile iniziare a caricare il codice nello slot di distribuzione di staging.</span><span class="sxs-lookup"><span data-stu-id="eba9e-156">It is time to start uploading your code to the staging deployment slot.</span></span>

## <a name="install-git-on-your-machine"></a><span data-ttu-id="eba9e-157">Installare GIT nel computer</span><span class="sxs-lookup"><span data-stu-id="eba9e-157">Install Git on your machine</span></span>

<span data-ttu-id="eba9e-158">Si installerà GIT nel computer Ubuntu 18.04.</span><span class="sxs-lookup"><span data-stu-id="eba9e-158">I will be installing Git on my Ubuntu 18.04 machine.</span></span>

1. <span data-ttu-id="eba9e-159">Aprire una nuova finestra **Terminale**</span><span class="sxs-lookup"><span data-stu-id="eba9e-159">Open a new **Terminal** window</span></span>

1. <span data-ttu-id="eba9e-160">Digitare il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="eba9e-160">Type the following command.</span></span> <span data-ttu-id="eba9e-161">Verrà richiesto di immettere la password utente Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="eba9e-161">You will be prompted to enter your Ubuntu user password.</span></span>

    ```console
    sudo apt-get update
    ```

1. <span data-ttu-id="eba9e-162">Dopo aver eseguito l'aggiornamento, digitare il comando seguente per installare GIT in locale.</span><span class="sxs-lookup"><span data-stu-id="eba9e-162">Once the update is successful, type the following command to install Git locally.</span></span> <span data-ttu-id="eba9e-163">Verrà richiesto di accettare l'installazione di GIT nel computer.</span><span class="sxs-lookup"><span data-stu-id="eba9e-163">You will be prompted to accept the installation of Git on your machine.</span></span>

    ```console
    sudo apt-get install git-core
    ```

1. <span data-ttu-id="eba9e-164">Per verificare che GIT sia ora installato, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-164">To verify that Git is now installed, type the following command:</span></span>

    ```console
    git --version
    ```

   <span data-ttu-id="eba9e-165">Se l'installazione è stata eseguita correttamente, verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-165">If the installation is successful, you will see the following output:</span></span>

    ```console
    git version 2.17.1
    ```

1. <span data-ttu-id="eba9e-166">È sempre consigliabile configurare le impostazioni di GIT specificando nome e indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="eba9e-166">It is always a good practice to configure Git settings by providing your name and email.</span></span> <span data-ttu-id="eba9e-167">A tal scopo, eseguire i comandi seguenti, sostituendo i segnaposto per nome e indirizzo di posta elettronica, senza le parentesi graffe (`{}`):</span><span class="sxs-lookup"><span data-stu-id="eba9e-167">For that, you need to issue the following commands, replacing the placeholders for name and email, without the curly braces (`{}`):</span></span>

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. <span data-ttu-id="eba9e-168">Per verificare che le informazioni siano state registrate da GIT, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-168">To verify that your information has been recorded by Git, type the following command:</span></span>

    ```console
    cat ~/.gitconfig
    ```

   <span data-ttu-id="eba9e-169">Sarà visualizzato quanto riportato di seguito insieme a nome e indirizzo di posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="eba9e-169">You should be seeing the following, with your name and email shown:</span></span>

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a><span data-ttu-id="eba9e-170">Inizializzare un repository GIT locale per l'app Web</span><span class="sxs-lookup"><span data-stu-id="eba9e-170">Initialize a local Git repository for your web app</span></span>

<span data-ttu-id="eba9e-171">Per iniziare a usare GIT, è necessario inizializzare un repository GIT locale per l'app Web.</span><span class="sxs-lookup"><span data-stu-id="eba9e-171">To start using Git, you need to initialize a local Git repository for the web app.</span></span>

1. <span data-ttu-id="eba9e-172">Aprire una nuova finestra **Terminale**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-172">Open a new **Terminal** window.</span></span>

1. <span data-ttu-id="eba9e-173">Passare alla cartella radice del contenuto dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="eba9e-173">Navigate to the content root folder of your web app.</span></span> <span data-ttu-id="eba9e-174">È possibile digitare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="eba9e-174">You can type the following:</span></span>

    ```console
    cd ~/Documents/BestBikeApp/
    ```

    ![Cartella radice del contenuto dell'app Web](../media-draft/7-web-app-content-root-folder.png)

1. <span data-ttu-id="eba9e-176">Eseguire il comando seguente per inizializzare un nuovo repository GIT:</span><span class="sxs-lookup"><span data-stu-id="eba9e-176">Initialize a new Git repository by issuing the following command:</span></span>

    ```console
    git init
    ```

    <span data-ttu-id="eba9e-177">Se il comando ha esito positivo, viene visualizzato un messaggio simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-177">If the command is successful, you receive a message like the following:</span></span>

    ```console
    Initialized empty Git repository in /home/your-user/Documents/BestBikeApp/.git/
    ```

1. <span data-ttu-id="eba9e-178">Eseguire lo staging di tutti i file dell'app Web in GIT.</span><span class="sxs-lookup"><span data-stu-id="eba9e-178">Stage all the web app files to Git.</span></span>

   <span data-ttu-id="eba9e-179">Nel passaggio successivo GIT riconosce i file dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="eba9e-179">The next step is to let Git know about your web app files.</span></span> <span data-ttu-id="eba9e-180">Aggiungere quindi tutti i file della directory di lavoro in modo che siano **gestiti temporaneamente** da GIT.</span><span class="sxs-lookup"><span data-stu-id="eba9e-180">Do that by adding all the files of the working directory so that they get **staged** by Git.</span></span> <span data-ttu-id="eba9e-181">Digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-181">Type the following command:</span></span>

    ```console
    git add .
    ```

    <span data-ttu-id="eba9e-182">Il comando precedente aggiunge tutti i file, rappresentati da ".", allo stato di staging di GIT.</span><span class="sxs-lookup"><span data-stu-id="eba9e-182">The command above adds all files, represented by the ".", to the staging state of Git.</span></span>

1. <span data-ttu-id="eba9e-183">A questo punto, eseguire il commit delle modifiche in GIT.</span><span class="sxs-lookup"><span data-stu-id="eba9e-183">Now, you need to commit your changes to Git.</span></span>

   <span data-ttu-id="eba9e-184">Dopo aver completato lo staging dei file in GIT, è necessario eseguire il commit dei file nella **cronologia del commit GIT** nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="eba9e-184">Once you stage the files with Git, you need to commit your files to the **Git commit history** on your local machine.</span></span> <span data-ttu-id="eba9e-185">A tal scopo, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-185">You do that by typing the following command:</span></span>

   `git commit -m "Initial create"`

   <span data-ttu-id="eba9e-186">Il comando `commit` accetta l'argomento `-m` per includere un messaggio con il commit che si sta creando.</span><span class="sxs-lookup"><span data-stu-id="eba9e-186">The `commit` command accepts  `-m` argument to include a message with the commit you are creating.</span></span> <span data-ttu-id="eba9e-187">Quando si eseguirà il push del codice in Azure, sarà possibile visualizzare lo stesso messaggio archiviato con questo commit specifico.</span><span class="sxs-lookup"><span data-stu-id="eba9e-187">Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.</span></span>

## <a name="add-a-remote-for-the-local-git-repository"></a><span data-ttu-id="eba9e-188">Aggiungere un repository remoto per il repository GIT locale</span><span class="sxs-lookup"><span data-stu-id="eba9e-188">Add a remote for the local Git repository</span></span>

<span data-ttu-id="eba9e-189">A questo punto è stato inizializzato un nuovo repository GIT locale.</span><span class="sxs-lookup"><span data-stu-id="eba9e-189">At this point, you have successfully initialized a new local Git repository.</span></span> <span data-ttu-id="eba9e-190">È stato anche eseguito il commit di tutti i file dell'app Web in GIT.</span><span class="sxs-lookup"><span data-stu-id="eba9e-190">In addition, you've committed all of your web app files to Git.</span></span> <span data-ttu-id="eba9e-191">Rimane da aggiungere un repository **remoto** al quale connettere il repository GIT locale che è stato ospitato in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-191">What remains is to add a **remote** to connect your local Git repository to that hosted on Azure.</span></span>

<span data-ttu-id="eba9e-192">A tale scopo, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="eba9e-192">To do so, you need to:</span></span>

1. <span data-ttu-id="eba9e-193">Copiare l'**URL clone GIT** visto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="eba9e-193">Copy the **Git clone url** that you saw above.</span></span>

1. <span data-ttu-id="eba9e-194">Dopo averlo copiato, tornare alla finestra **Terminale** ed eseguire il comando GIT seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-194">Once copied, you go back to the **Terminal** window and issue the following Git command:</span></span>

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    <span data-ttu-id="eba9e-195">Il comando GIT precedente consente al repository GIT locale di collegarsi a quello ospitato in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-195">The above Git command hooks your local Git repository to the one hosted on Azure.</span></span> <span data-ttu-id="eba9e-196">È ora possibile eseguire il push e il pull tra il repository GIT locale e quello remoto.</span><span class="sxs-lookup"><span data-stu-id="eba9e-196">Now, you can start pushing and pulling between the local and remote Git repositories!</span></span>

1. <span data-ttu-id="eba9e-197">Per verificare il comando precedente, digitare il comando GIT seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-197">To verify the above command, type the following Git command:</span></span>

    ```
    git remote -v
    ```

    <span data-ttu-id="eba9e-198">Il comando precedente genera l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-198">The command above generates the following output:</span></span>

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a><span data-ttu-id="eba9e-199">Eseguire il push del codice in Azure</span><span class="sxs-lookup"><span data-stu-id="eba9e-199">Push your code to Azure</span></span>

<span data-ttu-id="eba9e-200">Ora che in Azure il repository GIT locale è collegato al repository GIT remoto, sarà possibile sviluppare e compilare l'app ed eseguire il push dell'app Web in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-200">Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app, and then push your web app to Azure.</span></span>

1. <span data-ttu-id="eba9e-201">Digitare il comando GIT seguente per eseguire il push del ramo **principale** nel repository GIT remoto in Azure:</span><span class="sxs-lookup"><span data-stu-id="eba9e-201">Type the following Git command to push your **master** branch to the remote Git repository on Azure:</span></span>

    ```console
    git push origin master
    ```

1. <span data-ttu-id="eba9e-202">Verrà richiesto di immettere la password configurata nella sezione precedente **Credenziali per la distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-202">You will be prompted to enter the password that you have configured in the **Deployment credentials** section above.</span></span> <span data-ttu-id="eba9e-203">Immettere la password e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="eba9e-203">Enter your password and hit Enter.</span></span> <span data-ttu-id="eba9e-204">GIT inizia a caricare i file di cui è stato eseguito il commit nel repository GIT remoto di Azure configurato, all'interno dello slot di distribuzione di staging.</span><span class="sxs-lookup"><span data-stu-id="eba9e-204">Git starts uploading your committed files to the Azure remote Git repository configured under the staging deployment slot.</span></span>

## <a name="verify-the-web-app-is-uploaded-to-azure"></a><span data-ttu-id="eba9e-205">Verificare che l'app Web sia caricata in Azure</span><span class="sxs-lookup"><span data-stu-id="eba9e-205">Verify the web app is uploaded to Azure</span></span>

1. <span data-ttu-id="eba9e-206">Accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-206">Log in to the Azure portal.</span></span>

1. <span data-ttu-id="eba9e-207">Individuare e selezionare la voce di menu **Tutte le risorse** dal menu di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="eba9e-207">Locate and click on the **All Resources** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="eba9e-208">Il portale di Azure passerà all'elenco di tutte le risorse create in Azure fino a questo momento.</span><span class="sxs-lookup"><span data-stu-id="eba9e-208">The Azure portal navigates you to the list of all resources created on Azure so far.</span></span>

1. <span data-ttu-id="eba9e-209">Individuare e fare clic sullo slot di staging creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="eba9e-209">Locate and click on the staging slot created above.</span></span> <span data-ttu-id="eba9e-210">Tenere presente che uno slot di distribuzione viene considerato come se fosse un'app Web e pertanto sarà visualizzato come risorsa dell'app Web in **Tutte le risorse**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-210">Remember, a deployment slot is considered a web app, and hence, it will appear as a web app resource under **All Resources**.</span></span>

1. <span data-ttu-id="eba9e-211">Dopo essere passati al pannello dello slot di distribuzione di staging, scegliere **Opzioni di distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-211">Once you arrive to the staging deployment slot blade, go to **Deployment options**.</span></span>

    ![Slot di distribuzione di staging dopo aver caricato l'app Web](../media-draft/7-staging-deployment-slot-after-uploading-files.png)

    <span data-ttu-id="eba9e-213">Si noti che il primo commit disponibile in locale nel computer è ora caricato nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-213">You will see that your first commit that you have locally on your machine is now uploaded to the Azure portal!</span></span>

    <span data-ttu-id="eba9e-214">Eseguendo il push del codice in locale nel repository GIT remoto ospitato in Azure, Azure ha registrato l'operazione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-214">By pushing your code locally to the remote Git repository hosted on Azure, Azure has recorded this operation.</span></span>

    <span data-ttu-id="eba9e-215">Ogni volta che si esegue il push del codice in Azure, verrà visualizzato un nuovo record insieme al messaggio digitato durante il commit delle modifiche in locale nel computer.</span><span class="sxs-lookup"><span data-stu-id="eba9e-215">Every time you push your code to Azure, you will see a new record, together with the message that you type when committing your changes locally on your machine.</span></span>

1. <span data-ttu-id="eba9e-216">Osserviamo ora l'URL dello **slot di staging**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-216">Let's visit the **staging slot** URL.</span></span> <span data-ttu-id="eba9e-217">L'URL è stato specificato in precedenza. Se però è stato dimenticato, è sempre possibile accedere alla pagina **Panoramica** dello slot di distribuzione di staging per recuperarlo.</span><span class="sxs-lookup"><span data-stu-id="eba9e-217">The URL was mentioned above, however, if your forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.</span></span>

1. <span data-ttu-id="eba9e-218">Digitare l'URL seguente nella barra degli indirizzi del browser: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="eba9e-218">Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span></span>

    ![Slot di staging ospitato online](../media-draft/7-staging-slot-hosted-online.png)

<span data-ttu-id="eba9e-220">I file dell'app Web locale sono stati caricati nello slot di distribuzione di staging in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-220">You have successfully uploaded your local web app files to the staging deployment slot on Azure.</span></span>

## <a name="swapping-the-staging-and-production-deployment-slots"></a><span data-ttu-id="eba9e-221">Scambio di slot di distribuzione di staging e di produzione</span><span class="sxs-lookup"><span data-stu-id="eba9e-221">Swapping the staging and production deployment slots</span></span>

<span data-ttu-id="eba9e-222">Ora che l'applicazione è attiva e in esecuzione nello slot di distribuzione di staging ospitato in Azure, è possibile scambiare questo slot con quello di produzione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-222">Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one.</span></span> <span data-ttu-id="eba9e-223">A questo scopo, attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="eba9e-223">To do so, follow these steps:</span></span>

1. <span data-ttu-id="eba9e-224">Individuare e passare al pannello dell'app Web originale creata nell'unità 2.</span><span class="sxs-lookup"><span data-stu-id="eba9e-224">Locate and navigate to the original web app blade, created in Unit #2.</span></span>

1. <span data-ttu-id="eba9e-225">Individuare e selezionare la voce di menu **Slot di distribuzione** dal menu di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="eba9e-225">Locate and click the **Deployment slots** menu item on the left-side navigation.</span></span>

    ![Pannello Slot di distribuzione dell'app Web](../media-draft/7-web-app-slots.png)

1. <span data-ttu-id="eba9e-227">Individuare e fare clic sul pulsante **Scambia** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="eba9e-227">Locate and click on the **Swap** button at the top of the blade.</span></span>

1. <span data-ttu-id="eba9e-228">Nel portale di Azure viene visualizzato il pannello **Scambia**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-228">The Azure portal navigates you to the **Swap** blade.</span></span>

    ![Pannello Scambia](../media-draft/7-swap-blade.png)

1. <span data-ttu-id="eba9e-230">Per il campo **Scambia**, selezionare **Scambia**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-230">For the **Swap** field, select **Swap**.</span></span>

1. <span data-ttu-id="eba9e-231">Per il campo **Origine**, selezionare **Staging**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-231">For the **Source** field, select **Staging**.</span></span>

1. <span data-ttu-id="eba9e-232">Per il campo **Destinazione**, selezionare **Produzione**.</span><span class="sxs-lookup"><span data-stu-id="eba9e-232">For the **Destination** field, select **Production**.</span></span>

1. <span data-ttu-id="eba9e-233">Individuare e fare clic sul pulsante **OK** nella parte inferiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="eba9e-233">Locate and click on the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="eba9e-234">Azure avvierà il processo di scambio.</span><span class="sxs-lookup"><span data-stu-id="eba9e-234">Azure starts the swapping process.</span></span> <span data-ttu-id="eba9e-235">Questa operazione richiede generalmente pochi secondi, a seconda delle dimensioni dell'app Web sottoposta a scambio.</span><span class="sxs-lookup"><span data-stu-id="eba9e-235">Usually, this operation takes a few seconds, depending on the size of the web app being swapped.</span></span>

1. <span data-ttu-id="eba9e-236">Al termine dell'operazione, visitare l'URL dell'app Web: [ https://bestbike.azurewebsites.net/ ](https://bestbike.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="eba9e-236">Once the operation ends, visit the web app URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span></span>

    ![Pagina App Web](../media-draft/7-web-app-page.png)

    <span data-ttu-id="eba9e-238">L'operazione di scambio è stata completata.</span><span class="sxs-lookup"><span data-stu-id="eba9e-238">The swapping operation has been successful!</span></span> <span data-ttu-id="eba9e-239">È ora possibile osservare che il codice che è stato caricato nello slot di distribuzione di staging è anche ospitato nello slot di produzione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-239">You can now see the code that you uploaded to the staging deployment slot also being hosted on the production slot!</span></span>

1. <span data-ttu-id="eba9e-240">A questo punto, visitare l'URL dello slot di staging [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="eba9e-240">Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span></span>

    ![Staging dell'app Web](../media-draft/7-staging-after-swapping.png)

    <span data-ttu-id="eba9e-242">Lo slot di distribuzione di staging ora contiene i file dell'applicazione Web predefiniti e originali che erano in precedenza ospitati nello slot di produzione.</span><span class="sxs-lookup"><span data-stu-id="eba9e-242">The staging deployment slot now contains the original, default web application files that were previously hosted under the production slot.</span></span>

<span data-ttu-id="eba9e-243">Congratulazioni.</span><span class="sxs-lookup"><span data-stu-id="eba9e-243">Congratulations!</span></span> <span data-ttu-id="eba9e-244">L'app Web è stata caricata in Azure.</span><span class="sxs-lookup"><span data-stu-id="eba9e-244">You have successfully uploaded your web app to Azure!</span></span>
