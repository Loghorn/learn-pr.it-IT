<span data-ttu-id="b3e71-101">In questo esercizio si installerà **Azure PowerShell** nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="b3e71-101">In this exercise, you will install **Azure PowerShell** on your local machine.</span></span> <span data-ttu-id="b3e71-102">Scegliere la sezione appropriata per il sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="b3e71-102">Choose the appropriate section for your operating system.</span></span>

## <a name="linux-and-mac"></a><span data-ttu-id="b3e71-103">Linux e Mac</span><span class="sxs-lookup"><span data-stu-id="b3e71-103">Linux and Mac</span></span>
<span data-ttu-id="b3e71-104">In Linux e macOS, il primo passaggio consiste nell'installare **PowerShell Core**.</span><span class="sxs-lookup"><span data-stu-id="b3e71-104">On Linux and macOS, the first step is to install **PowerShell Core**.</span></span>

### <a name="linux"></a><span data-ttu-id="b3e71-105">Linux</span><span class="sxs-lookup"><span data-stu-id="b3e71-105">Linux</span></span>
<span data-ttu-id="b3e71-106">Come indicato nell'ultima unità, l'installazione di PowerShell per Linux comporterà l'uso di uno strumento di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="b3e71-106">As mentioned in the last unit, installing PowerShell for Linux will involve using a package manager.</span></span> <span data-ttu-id="b3e71-107">In questo esempio verrà usato **Ubuntu 18.04**, ma sono disponibili [istruzioni dettagliate per le altre versioni e distribuzioni nella documentazione](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span><span class="sxs-lookup"><span data-stu-id="b3e71-107">We will use **Ubuntu 18.04** for our example here, but we have [detailed instructions for other versions and distributions in our documentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span></span>

<span data-ttu-id="b3e71-108">PowerShell Core verrà installato in Ubuntu Linux tramite Advanced Packaging Tool (**apt**) e la riga di comando di Bash.</span><span class="sxs-lookup"><span data-stu-id="b3e71-108">You will install PowerShell Core on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span> 

1. <span data-ttu-id="b3e71-109">Importare la chiave di crittografia per il repository Ubuntu di Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b3e71-109">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="b3e71-110">Questo consente allo strumento di gestione pacchetti di verificare che il pacchetto di PowerShell Core da installare sia fornito da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b3e71-110">This will allow the package manager to verify that the PowerShell Core package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```
1. <span data-ttu-id="b3e71-111">Registrare il **repository Ubuntu di Microsoft**, in modo da consentire allo strumento di gestione pacchetti di individuare il pacchetto di PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="b3e71-111">Register the **Microsoft Ubuntu repository** so the package manager can locate the PowerShell Core package.</span></span>

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. <span data-ttu-id="b3e71-112">Aggiornare l'elenco dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="b3e71-112">Update the list of packages.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="b3e71-113">Installare PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="b3e71-113">Install PowerShell Core.</span></span>

    ```bash
    sudo apt-get install -y powershell
    ```

1. <span data-ttu-id="b3e71-114">Avviare PowerShell per verificare che sia installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="b3e71-114">Start PowerShell to verify that it installed successfully.</span></span>

    ```bash
    pwsh
    ```

### <a name="macos"></a><span data-ttu-id="b3e71-115">macOS</span><span class="sxs-lookup"><span data-stu-id="b3e71-115">macOS</span></span>
<span data-ttu-id="b3e71-116">Installare quindi **PowerShell Core** su macOS tramite la gestione pacchetti Homebrew.</span><span class="sxs-lookup"><span data-stu-id="b3e71-116">Next, install **PowerShell Core** on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b3e71-117">Se il comando **brew** non è disponibile, potrebbe essere necessario installare la gestione pacchetti Homebrew.</span><span class="sxs-lookup"><span data-stu-id="b3e71-117">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="b3e71-118">Per informazioni dettagliate, vedere il [sito Web di Homebrew](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="b3e71-118">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="b3e71-119">Installare Homebrew-Cask per ottenere altri pacchetti, incluso il pacchetto di PowerShell Core:</span><span class="sxs-lookup"><span data-stu-id="b3e71-119">Install Homebrew-Cask to obtain more packages, including the PowerShell Core package:</span></span>

    ```bash
    brew tap caskroom/cask
    ```
1. <span data-ttu-id="b3e71-120">Installare PowerShell Core:</span><span class="sxs-lookup"><span data-stu-id="b3e71-120">Install PowerShell Core:</span></span>

    ```bash
    brew cask installs powershell
    ```

1. <span data-ttu-id="b3e71-121">Avviare PowerShell Core per verificare che sia installato correttamente:</span><span class="sxs-lookup"><span data-stu-id="b3e71-121">Start PowerShell Core to verify that it installed successfully:</span></span>

    ```bash
    pwsh
    ```

## <a name="install-azure-powershell"></a><span data-ttu-id="b3e71-122">Installare Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3e71-122">Install Azure PowerShell</span></span>
<span data-ttu-id="b3e71-123">Dopo l'installazione del prodotto **PowerShell** di base, installare **Azure PowerShell** per aggiungere i comandi specifici di Azure.</span><span class="sxs-lookup"><span data-stu-id="b3e71-123">After installing the base **PowerShell** product, install **Azure PowerShell** to add the Azure-specific commands.</span></span>

### <a name="windows"></a><span data-ttu-id="b3e71-124">Windows</span><span class="sxs-lookup"><span data-stu-id="b3e71-124">Windows</span></span>
<span data-ttu-id="b3e71-125">Installare Azure PowerShell in Windows usando il comando `Install-Module` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b3e71-125">Install Azure PowerShell on Windows using the `Install-Module` PowerShell command.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b3e71-126">Per installare Azure PowerShell, è necessario PowerShell versione 5.0 o superiore.</span><span class="sxs-lookup"><span data-stu-id="b3e71-126">You must have PowerShell version 5.0 or higher to install Azure PowerShell.</span></span> <span data-ttu-id="b3e71-127">Per controllare la versione di PowerShell, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b3e71-127">To check your version of PowerShell, use the following command:</span></span> 
>
> `$PSVersionTable.PSVersion` 
>
><span data-ttu-id="b3e71-128">Se il numero di versione è inferiore alla versione 5.0, seguire le istruzioni per l'[aggiornamento di Windows PowerShell esistente](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="b3e71-128">If the version number is lower than 5.0, follow the instructions for [upgrading existing Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span></span>

1. <span data-ttu-id="b3e71-129">Aprire il menu **Start** e digitare **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="b3e71-129">Open the **Start** menu and type **Windows PowerShell**.</span></span>
2. <span data-ttu-id="b3e71-130">Fare clic con il pulsante destro del mouse sull'icona di **Windows PowerShell** e scegliere **Esegui come amministratore**.</span><span class="sxs-lookup"><span data-stu-id="b3e71-130">Right-click the **Windows PowerShell** icon and select **Run as administrator**.</span></span>
3. <span data-ttu-id="b3e71-131">Nella finestra di dialogo **Controllo account utente** selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="b3e71-131">In the **User Account Control** dialog, select **Yes**.</span></span>
4. <span data-ttu-id="b3e71-132">Digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="b3e71-132">Type the following command, and then press Enter:</span></span>

    ```powershell
    Install-Module -Name AzureRM
    ```
5. <span data-ttu-id="b3e71-133">Se viene richiesto se considerare attendibili i moduli di PSGallery, rispondere **Sì** oppure **Sì a tutti**.</span><span class="sxs-lookup"><span data-stu-id="b3e71-133">If you are asked whether you trust modules from PSGallery, answer **Yes** or **Yes to All**.</span></span>

> [!NOTE]
> <span data-ttu-id="b3e71-134">Se viene visualizzato un messaggio di errore che indica che è già installata una versione del modulo Azure PowerShell, è possibile eseguire l'aggiornamento alla versione _più recente_ eseguendo il comando:</span><span class="sxs-lookup"><span data-stu-id="b3e71-134">If you get an error message indicating that a version of the Azure PowerShell module is already installed, you can update to the _latest_ version by issuing the command:</span></span>
> 
> `Update-Module -Name AzureRM`
> 
> <span data-ttu-id="b3e71-135">Come nel caso del comando `Install-Module`, rispondere **Sì** o **Sì a tutti** quando viene richiesto se considerare attendibile il modulo.</span><span class="sxs-lookup"><span data-stu-id="b3e71-135">As with the `Install-Module` command, answer **Yes** or **Yes to All** when prompted to trust the module.</span></span>

### <a name="linux-or-macos"></a><span data-ttu-id="b3e71-136">Linux o macOS</span><span class="sxs-lookup"><span data-stu-id="b3e71-136">Linux or macOS</span></span>
<span data-ttu-id="b3e71-137">Per installare Azure PowerShell in Linux o macOS, viene usato lo stesso processo di base.</span><span class="sxs-lookup"><span data-stu-id="b3e71-137">We use the same basic process to install the Azure PowerShell on either Linux or macOS.</span></span> <span data-ttu-id="b3e71-138">La procedura è la stessa per entrambi i sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="b3e71-138">The procedure is the same for both operating systems.</span></span> <span data-ttu-id="b3e71-139">La differenza consiste nel metodo per l'avvio di una sessione di PowerShell Core con privilegi elevati.</span><span class="sxs-lookup"><span data-stu-id="b3e71-139">The difference is in getting an elevated PowerShell Core session.</span></span>

1. <span data-ttu-id="b3e71-140">In un terminale, digitare il comando seguente per avviare PowerShell Core con privilegi elevati.</span><span class="sxs-lookup"><span data-stu-id="b3e71-140">In a terminal, type the following command to launch PowerShell Core with elevated privileges.</span></span>

    ```bash
    sudo pwsh
    ```

1. <span data-ttu-id="b3e71-141">Per installare Azure PowerShell, eseguire il comando seguente nel prompt di PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="b3e71-141">Run the following command at the PowerShell Core prompt to install Azure PowerShell.</span></span>

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. <span data-ttu-id="b3e71-142">Se viene richiesto se considerare attendibili i moduli di **PSGallery**, rispondere **Sì** oppure **Sì a tutti**.</span><span class="sxs-lookup"><span data-stu-id="b3e71-142">If you are asked whether you trust modules from **PSGallery**, answer **Yes** or **Yes to All**.</span></span>

## <a name="summary"></a><span data-ttu-id="b3e71-143">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="b3e71-143">Summary</span></span>
<span data-ttu-id="b3e71-144">È stato configurato il computer locale per amministrare le risorse di Azure con Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b3e71-144">You have setup your local machine(s) to administer Azure resources with Azure PowerShell.</span></span> <span data-ttu-id="b3e71-145">Ora è possibile usare Azure PowerShell in locale per immettere comandi o eseguire script.</span><span class="sxs-lookup"><span data-stu-id="b3e71-145">You can now use Azure PowerShell locally to enter commands or execute scripts.</span></span> <span data-ttu-id="b3e71-146">Azure PowerShell inoltrerà i comandi ai data center di Azure, in cui verranno eseguiti all'interno della sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="b3e71-146">Azure PowerShell will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>
