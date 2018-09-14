<span data-ttu-id="43873-101">Quando è stato creato un bot app Web di Azure nell'[Esercizio 1](#Exercise1), è stata distribuita un'app Web di Azure per ospitarlo.</span><span class="sxs-lookup"><span data-stu-id="43873-101">When you created an Azure web app bot in [Exercise 1](#Exercise1), an Azure web app was deployed to host it.</span></span> <span data-ttu-id="43873-102">Il bot tuttavia richiede del codice e deve ancora essere distribuito all'app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="43873-102">But the bot does require some code, and it still needs to be deployed to the Azure web app.</span></span> <span data-ttu-id="43873-103">Fortunatamente, il codice è stato generato automaticamente dal servizio Azure Bot.</span><span class="sxs-lookup"><span data-stu-id="43873-103">Fortunately, the code was generated for you by the Azure Bot Service.</span></span> <span data-ttu-id="43873-104">In questo esercizio si userà Visual Studio Code per inserire il codice in un repository Git locale e pubblicare il bot in Azure eseguendo il push delle modifiche dal repository locale in un repository remoto connesso all'app Web di Azure che ospita il bot, un processo noto come [integrazione continua](https://en.wikipedia.org/wiki/Continuous_integration).</span><span class="sxs-lookup"><span data-stu-id="43873-104">In this exercise, you will use Visual Studio Code to place the code in a local Git repository and publish the bot to Azure by pushing changes from the local repository to a remote repository connected to the Azure web app that hosts the bot — a process known as [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration).</span></span>

1. <span data-ttu-id="43873-105">Se [Git](https://git-scm.com/) non è installato nel PC, passare a https://git-scm.com/downloads e installare il client Git per il sistema operativo in uso.</span><span class="sxs-lookup"><span data-stu-id="43873-105">If [Git](https://git-scm.com/) isn't installed on your PC, go to https://git-scm.com/downloads and install the Git client for your operating system.</span></span> <span data-ttu-id="43873-106">Git è un sistema di controllo della versione open source e distribuito che si integra facilmente in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="43873-106">Git is a free and open-source distributed version control system, and it integrates seamlessly into Visual Studio Code.</span></span> <span data-ttu-id="43873-107">Se non si è certi che Git sia installato, aprire un prompt dei comandi o una finestra del terminale ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="43873-107">If you aren't sure whether Git is installed, open a Command Prompt or terminal window and execute the following command:</span></span>

    ``` 
    git --version
    ```

    <span data-ttu-id="43873-108">Se viene visualizzato un numero di versione, il client Git è installato.</span><span class="sxs-lookup"><span data-stu-id="43873-108">If a version number is displayed, then the Git client is installed.</span></span>

1. <span data-ttu-id="43873-109">Se Node.js non è installato nel PC, passare a https://nodejs.org/ e installare l'ultima versione LTS.</span><span class="sxs-lookup"><span data-stu-id="43873-109">If Node.js isn't installed on your PC, go to https://nodejs.org/ and install the latest LTS version.</span></span> <span data-ttu-id="43873-110">È possibile determinare se Node.js è installato aprendo un prompt dei comandi o una finestra del terminale e digitando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="43873-110">You can determine whether Node.js is installed by opening a Command Prompt or terminal window and typing the following command:</span></span>

    ```
    node --version
    ```

    <span data-ttu-id="43873-111">Se Node è installato, verrà visualizzato il numero di versione.</span><span class="sxs-lookup"><span data-stu-id="43873-111">If Node is installed, the version number will be displayed.</span></span>

1. <span data-ttu-id="43873-112">Se Visual Studio Code non è installato nel PC, passare a https://code.visualstudio.com/ e installarlo ora.</span><span class="sxs-lookup"><span data-stu-id="43873-112">If Visual Studio Code isn't installed on your PC, go to https://code.visualstudio.com/ and install it now.</span></span>

1. <span data-ttu-id="43873-113">Creare una cartella denominata "Factbot" per il codice sorgente del bot nella posizione scelta del disco rigido.</span><span class="sxs-lookup"><span data-stu-id="43873-113">Create a folder named "Factbot" in the location of your choice on your hard disk to hold the bot's source code.</span></span>

1. <span data-ttu-id="43873-114">Tornare al portale di Azure e aprire il gruppo di risorse "factbot-rg".</span><span class="sxs-lookup"><span data-stu-id="43873-114">Return to the Azure portal and open the "factbot-rg" resource group.</span></span> <span data-ttu-id="43873-115">Quindi fare clic sul bot app Web creato nell'[Esercizio 1](#Exercise1).</span><span class="sxs-lookup"><span data-stu-id="43873-115">Then, click the web app bot you created in [Exercise 1](#Exercise1).</span></span>

    ![Apertura del bot app Web](../images/open-web-app-bot.png)

    <span data-ttu-id="43873-117">_Apertura del bot app Web_</span><span class="sxs-lookup"><span data-stu-id="43873-117">_Opening the web app bot_</span></span>

1. <span data-ttu-id="43873-118">Fare clic su **Build** nel menu a sinistra, quindi fare clic su **Scarica il file zip** per preparare un file ZIP contenente il codice sorgente del bot.</span><span class="sxs-lookup"><span data-stu-id="43873-118">Click **Build** in the menu on the left, and then click **Download zip file** to prepare a zip file containing the bot's source code.</span></span> <span data-ttu-id="43873-119">Dopo aver preparato il file ZIP, fare clic sul pulsante **Scarica il file zip** per scaricarlo.</span><span class="sxs-lookup"><span data-stu-id="43873-119">Once the zip file is prepared, click the **Download zip file** button to download it.</span></span> <span data-ttu-id="43873-120">Una volta completato il download, copiare il contenuto del file ZIP nella cartella "Factbot" creata al passaggio 4.</span><span class="sxs-lookup"><span data-stu-id="43873-120">When the download is complete, copy the contents of the zip file to the "Factbot" folder that you created in Step 4.</span></span>

    ![Download del codice sorgente](../images/download-source.png)

    <span data-ttu-id="43873-122">_Download del codice sorgente_</span><span class="sxs-lookup"><span data-stu-id="43873-122">_Downloading the source code_</span></span>
  
1. <span data-ttu-id="43873-123">Sempre nel pannello "Build" fare clic su **Configure continuous deployment** (Configura distribuzione continua).</span><span class="sxs-lookup"><span data-stu-id="43873-123">Still on the "Build" blade, click **Configure continuous deployment**.</span></span> <span data-ttu-id="43873-124">Fare clic su **Configura** nella parte superiore del pannello che segue, seguito da **Scegliere l'origine**.</span><span class="sxs-lookup"><span data-stu-id="43873-124">Click **Setup** at the top of the ensuing blade, followed by **Choose Source**.</span></span> <span data-ttu-id="43873-125">Selezionare **Repository Git locale** come origine della distribuzione e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="43873-125">Then, select **Local Git Repository** as the deployment source and click **OK**.</span></span> 
 
    ![Specifica di un repository Git locale come origine della distribuzione](../images/portal-set-local-git.png)

    <span data-ttu-id="43873-127">_Specifica di un repository Git locale come origine della distribuzione_</span><span class="sxs-lookup"><span data-stu-id="43873-127">_Specifying a local Git repository as the deployment source_</span></span>  

1. <span data-ttu-id="43873-128">Chiudere il pannello "Distribuzioni" e fare clic su **All App service settings** (Tutte le impostazioni del servizio app) nel menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="43873-128">Close the "Deployments" blade and click **All App service settings** in the menu on the left.</span></span>

1. <span data-ttu-id="43873-129">Fare clic su **Credenziali distribuzione** e immettere un nome utente e una password.</span><span class="sxs-lookup"><span data-stu-id="43873-129">Click **Deployment credentials**, and then enter a user name and password.</span></span> <span data-ttu-id="43873-130">Probabilmente si dovrà immettere un nome utente diverso da "FactbotAdministrator", perché il nome deve essere univoco all'interno di Azure.</span><span class="sxs-lookup"><span data-stu-id="43873-130">You will probably have to enter a user name other than "FactbotAdministrator" because the name must be unique within Azure.</span></span> <span data-ttu-id="43873-131">Fare clic su **Salva** e chiudere il pannello.</span><span class="sxs-lookup"><span data-stu-id="43873-131">Then, click **Save** and close the blade.</span></span>

    ![Immissione delle credenziali di distribuzione](../images/portal-enter-ci-creds.png)

    <span data-ttu-id="43873-133">_Immissione delle credenziali di distribuzione_</span><span class="sxs-lookup"><span data-stu-id="43873-133">_Entering deployment credentials_</span></span>  

1. <span data-ttu-id="43873-134">Avviare Visual Studio Code e usare il comando **File** > **Apri cartella...** per aprire la cartella "Factbot" in cui è stato copiato il codice sorgente del bot nel passaggio 6.</span><span class="sxs-lookup"><span data-stu-id="43873-134">Start Visual Studio Code and use the **File** > **Open Folder...** command to open the "Factbot" folder that you copied the bot's source code to in Step 6.</span></span>

1. <span data-ttu-id="43873-135">Fare clic sul pulsante **Controllo del codice sorgente** sulla barra delle attività sul lato sinistro di Visual Studio Code e scegliere l'icona **Inizializza repository** nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="43873-135">Click the **Source Control** button in the activity bar on the left side of Visual Studio Code, and click the **Initialize Repository** icon at the top.</span></span> <span data-ttu-id="43873-136">Quindi, fare clic sul pulsante **Inizializza repository** nella finestra di dialogo che segue.</span><span class="sxs-lookup"><span data-stu-id="43873-136">Then, click the **Initialize Repository** button in the ensuing dialog.</span></span>

    ![Inizializzazione di un repository Git locale](../images/vs-init-git-repo.png)

    <span data-ttu-id="43873-138">_Inizializzazione di un repository Git locale_</span><span class="sxs-lookup"><span data-stu-id="43873-138">_Initializing a local Git repository_</span></span>  

1. <span data-ttu-id="43873-139">Digitare "First commit."</span><span class="sxs-lookup"><span data-stu-id="43873-139">Type "First commit."</span></span> <span data-ttu-id="43873-140">nella finestra di messaggio e quindi fare clic sul segno di spunta per eseguire il commit delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="43873-140">into the message box, and then click the check mark to commit your changes.</span></span>

    ![Commit delle modifiche nel repository locale](../images/vs-first-git-commit.png)

    <span data-ttu-id="43873-142">_Commit delle modifiche nel repository locale_</span><span class="sxs-lookup"><span data-stu-id="43873-142">_Committing changes to the local repository_</span></span>  

1. <span data-ttu-id="43873-143">Selezionare **Terminale integrato** dal menu **Visualizza** di Visual Studio Code per aprire un terminale integrato.</span><span class="sxs-lookup"><span data-stu-id="43873-143">Select **Integrated Terminal** from Visual Studio Code's **View** menu to open an integrated terminal.</span></span> <span data-ttu-id="43873-144">Eseguire quindi il comando seguente nel terminale integrato, sostituendo BOT_NAME in due punti con il nome del bot immesso al passaggio 3 dell'Esercizio 1.</span><span class="sxs-lookup"><span data-stu-id="43873-144">Then, execute the following command in the integrated terminal, replacing BOT_NAME in two places with the bot name you entered in Exercise 1, Step 3.</span></span>

    ```
    git remote add qna-factbot https://BOT_NAME.scm.azurewebsites.net:443/BOT_NAME.git
    ```

1. <span data-ttu-id="43873-145">Fare clic sui puntini di sospensione nella parte superiore del pannello CONTROLLO DEL CODICE SORGENTE e selezionare **Pubblica ramo** dal menu per eseguire il push del codice del bot dal repository locale ad Azure.</span><span class="sxs-lookup"><span data-stu-id="43873-145">Click the ellipsis (the three dots) at the top of the SOURCE CONTROL panel and select **Publish Branch** from the menu to push the bot code from the local repository to Azure.</span></span> <span data-ttu-id="43873-146">Se vengono richieste le credenziali, immettere il nome utente e la password specificati al passaggio 9 di questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="43873-146">If prompted for credentials, enter the user name and password you specified in Step 9 of this exercise.</span></span>

<span data-ttu-id="43873-147">Il bot è stato pubblicato in Azure.</span><span class="sxs-lookup"><span data-stu-id="43873-147">Your bot has been published to Azure.</span></span> <span data-ttu-id="43873-148">Prima di testarlo in Azure, tuttavia, verrà eseguito in locale e si apprenderà come eseguirne il debug in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="43873-148">But before you test it there, let's run it locally and learn how to debug it in Visual Studio Code.</span></span>