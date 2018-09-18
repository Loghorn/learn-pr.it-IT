<span data-ttu-id="702df-101">Ora si installerà l'interfaccia della riga di comando di Azure nel computer locale e poi si eseguirà eseguito un comando per verificare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="702df-101">Let's install the Azure CLI on your local machine, and then run a command to verify your installation.</span></span> <span data-ttu-id="702df-102">Il metodo usato per l'installazione dell'interfaccia della riga di comando di Azure dipende dal sistema operativo del computer in uso.</span><span class="sxs-lookup"><span data-stu-id="702df-102">The method you use for installing the Azure CLI depends on the operating system of your computer.</span></span> <span data-ttu-id="702df-103">Scegliere la procedura corretta per il sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="702df-103">Choose the steps for your operating system.</span></span>

> [!NOTE]
> <span data-ttu-id="702df-104">Questo esercizio illustra tutte le operazioni necessarie per installare lo strumento dell'interfaccia della riga di comando di Azure in locale.</span><span class="sxs-lookup"><span data-stu-id="702df-104">This exercise guides you through installing the Azure CLI tool locally.</span></span> <span data-ttu-id="702df-105">Nella parte restante del modulo verrà usato Azure Cloud Shell in modo da sfruttare il supporto gratuito per la sottoscrizione in Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="702df-105">The remainder of the module will use the Azure Cloud Shell so you can leverage the free subscription support in Microsoft Learn.</span></span> <span data-ttu-id="702df-106">Se si preferisce, è possibile considerare questo esercizio come un'attività facoltativa e limitarsi a leggere le istruzioni.</span><span class="sxs-lookup"><span data-stu-id="702df-106">You can consider this exercise as an optional activity and just review the instructions if you prefer.</span></span>

<span data-ttu-id="702df-107">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="702df-107">::: zone pivot="linux"</span></span>

## <a name="linux"></a><span data-ttu-id="702df-108">Linux</span><span class="sxs-lookup"><span data-stu-id="702df-108">Linux</span></span>

<span data-ttu-id="702df-109">L'interfaccia della riga di comando di Azure verrà installata in **Ubuntu Linux** usando Advanced Packaging Tool (**apt**) e la riga di comando di Bash.</span><span class="sxs-lookup"><span data-stu-id="702df-109">Here you will install the Azure CLI on **Ubuntu Linux** using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span>

> [!TIP]
> <span data-ttu-id="702df-110">I comandi elencati di seguito si riferiscono a Ubuntu versione 18.04.</span><span class="sxs-lookup"><span data-stu-id="702df-110">The commands listed below are for Ubuntu version 18.04.</span></span> <span data-ttu-id="702df-111">Le istruzioni sono diverse per altre versioni e distribuzioni di Linux.</span><span class="sxs-lookup"><span data-stu-id="702df-111">Other versions and distributions of Linux have different instructions.</span></span> <span data-ttu-id="702df-112">Se si usa una versione diversa di Linux, consultare la [documentazione ufficiale](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="702df-112">Check the [official documentation](https://docs.microsoft.com/cli/azure/install-azure-cli) if you are using a different Linux version.</span></span>

1. <span data-ttu-id="702df-113">Modificare l'elenco di origini in modo che il repository di Microsoft venga registrato e la gestione dei pacchetti possa individuare il pacchetto dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="702df-113">Modify your sources list so that the Microsoft repository is registered, and the package manager can locate the Azure CLI package.</span></span>

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

1. <span data-ttu-id="702df-114">Importare la chiave di crittografia per il repository Microsoft Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="702df-114">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="702df-115">Questo consente allo strumento di gestione pacchetti di verificare che il pacchetto dell'interfaccia della riga di comando di Azure da installare sia fornito da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="702df-115">This will allow the package manager to verify that the Azure CLI package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="702df-116">Installare l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="702df-116">Install the Azure CLI.</span></span>

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

<span data-ttu-id="702df-117">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="702df-117">::: zone-end</span></span>

<span data-ttu-id="702df-118">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="702df-118">::: zone pivot="macos"</span></span>

## <a name="macos"></a><span data-ttu-id="702df-119">macOS</span><span class="sxs-lookup"><span data-stu-id="702df-119">macOS</span></span>

<span data-ttu-id="702df-120">Di seguito si installerà l'interfaccia della riga di comando di Azure in macOS tramite la gestione pacchetti Homebrew.</span><span class="sxs-lookup"><span data-stu-id="702df-120">Here you will install the Azure CLI on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="702df-121">Se il comando **brew** non è disponibile, potrebbe essere necessario installare la gestione pacchetti Homebrew.</span><span class="sxs-lookup"><span data-stu-id="702df-121">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="702df-122">Per informazioni dettagliate, vedere il [sito Web di Homebrew](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="702df-122">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="702df-123">Aggiornare il repository brew per assicurarsi di ottenere il pacchetto più recente dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="702df-123">Update your brew repository to make sure you get the latest Azure CLI package.</span></span>

    ```bash
    brew update
    ```

1. <span data-ttu-id="702df-124">Installare l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="702df-124">Install the Azure CLI.</span></span>

    ```bash
    brew install azure-cli
    ```

<span data-ttu-id="702df-125">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="702df-125">::: zone-end</span></span>

<span data-ttu-id="702df-126">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="702df-126">::: zone pivot="windows"</span></span>

## <a name="windows"></a><span data-ttu-id="702df-127">Windows</span><span class="sxs-lookup"><span data-stu-id="702df-127">Windows</span></span>

<span data-ttu-id="702df-128">Di seguito si installerà l'interfaccia della riga di comando di Azure in Windows tramite il programma di installazione MSI.</span><span class="sxs-lookup"><span data-stu-id="702df-128">Here you will install the Azure CLI on Windows using the MSI installer.</span></span>

1. <span data-ttu-id="702df-129">Passare a [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows) e nella finestra di dialogo della sicurezza del browser fare clic su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="702df-129">Go to [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), and in the browser security dialog box, click **Run**.</span></span>
1. <span data-ttu-id="702df-130">Nel programma di installazione accettare le condizioni di licenza e quindi fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="702df-130">In the installer, accept the license terms, and then click **Install**.</span></span>
1. <span data-ttu-id="702df-131">Nella finestra di dialogo **Controllo account utente** selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="702df-131">In the **User Account Control** dialog, select **Yes**.</span></span>

<span data-ttu-id="702df-132">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="702df-132">::: zone-end</span></span>

## <a name="running-the-azure-cli"></a><span data-ttu-id="702df-133">Esecuzione dell'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="702df-133">Running the Azure CLI</span></span>

<span data-ttu-id="702df-134">Per eseguire l'interfaccia della riga di comando di Azure, aprire una shell bash (Linux e macOS) o usare il prompt dei comandi o PowerShell (Windows).</span><span class="sxs-lookup"><span data-stu-id="702df-134">You run the Azure CLI by opening a bash shell (Linux and macOS), or from the command prompt or PowerShell (Windows).</span></span>

1. <span data-ttu-id="702df-135">Avviare l'interfaccia della riga di comando di Azure e verificare l'installazione eseguendo il controllo della versione.</span><span class="sxs-lookup"><span data-stu-id="702df-135">Start the Azure CLI and verify your installation by running the version check.</span></span>

    ```azurecli
    az --version
    ```

<span data-ttu-id="702df-136">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="702df-136">::: zone pivot="windows"</span></span>

> [!TIP]
> <span data-ttu-id="702df-137">L'esecuzione dell'interfaccia della riga di comando di Azure da PowerShell presenta alcuni vantaggi rispetto all'esecuzione dal prompt dei comandi di Windows.</span><span class="sxs-lookup"><span data-stu-id="702df-137">Running the Azure CLI from PowerShell has some advantages over running the Azure CLI from the Windows command prompt.</span></span> <span data-ttu-id="702df-138">PowerShell offre altre funzionalità di completamento con il tasto TAB oltre a quelle disponibili dal prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="702df-138">PowerShell provides additional tab completion features over those available from the command prompt.</span></span>

<span data-ttu-id="702df-139">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="702df-139">::: zone-end</span></span>

<span data-ttu-id="702df-140">I computer locali sono stati configurati per amministrare le risorse di Azure con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="702df-140">You set up your local machines to administer Azure resources with the Azure CLI.</span></span> <span data-ttu-id="702df-141">È ora possibile usare l'interfaccia della riga di comando di Azure in locale per immettere comandi o eseguire script.</span><span class="sxs-lookup"><span data-stu-id="702df-141">You can now use the Azure CLI locally to enter commands or execute scripts.</span></span> <span data-ttu-id="702df-142">L'interfaccia della riga di comando di Azure inoltrerà i comandi ai data center di Azure dove verranno eseguiti all'interno della sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="702df-142">The Azure CLI will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>