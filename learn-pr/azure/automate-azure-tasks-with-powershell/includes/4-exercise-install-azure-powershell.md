<span data-ttu-id="fb9ee-101">In questa unità si installerà **PowerShell** nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-101">In this unit, you will install **PowerShell** on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="fb9ee-102">Questo esercizio illustra tutte le operazioni necessarie per installare gli strumenti di PowerShell in locale.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-102">This exercise guides you through installing the PowerShell tools locally.</span></span> <span data-ttu-id="fb9ee-103">Nella parte restante del modulo verrà usato Azure Cloud Shell in modo da sfruttare il supporto della sottoscrizione gratuita in Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-103">The remainder of the module will use the Azure Cloud Shell so you can leverage the free subscription support in Microsoft Learn.</span></span> <span data-ttu-id="fb9ee-104">Se si preferisce, è possibile considerare questo esercizio come un'attività facoltativa e limitarsi a leggere le istruzioni.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-104">You can consider this exercise as an optional activity and just review the instructions if you prefer.</span></span>

<span data-ttu-id="fb9ee-105">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="fb9ee-105">::: zone pivot="linux"</span></span>

## <a name="linux"></a><span data-ttu-id="fb9ee-106">Linux</span><span class="sxs-lookup"><span data-stu-id="fb9ee-106">Linux</span></span>

<span data-ttu-id="fb9ee-107">L'installazione di PowerShell per Linux comporterà l'uso di una gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-107">Installing PowerShell for Linux will involve using a package manager.</span></span> <span data-ttu-id="fb9ee-108">In questo esempio verrà usato **Ubuntu 18.04**, ma sono disponibili [istruzioni dettagliate per le altre versioni e distribuzioni nella documentazione](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span><span class="sxs-lookup"><span data-stu-id="fb9ee-108">We will use **Ubuntu 18.04** for our example here, but we have [detailed instructions for other versions and distributions in our documentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span></span>

<span data-ttu-id="fb9ee-109">PowerShell Core verrà installato in Ubuntu Linux tramite Advanced Packaging Tool (**apt**) e la riga di comando di Bash.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-109">You will install PowerShell Core on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span> 

1. <span data-ttu-id="fb9ee-110">Importare la chiave di crittografia per il repository Ubuntu di Microsoft.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-110">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="fb9ee-111">Questo consente allo strumento di gestione pacchetti di verificare che il pacchetto di PowerShell Core da installare sia fornito da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-111">This will allow the package manager to verify that the PowerShell Core package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="fb9ee-112">Registrare il **repository Ubuntu di Microsoft**, in modo da consentire allo strumento di gestione pacchetti di individuare il pacchetto di PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-112">Register the **Microsoft Ubuntu repository** so the package manager can locate the PowerShell Core package.</span></span>

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. <span data-ttu-id="fb9ee-113">Aggiornare l'elenco dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-113">Update the list of packages.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="fb9ee-114">Installare PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-114">Install PowerShell Core.</span></span>

    ```bash
    sudo apt-get install -y powershell
    ```

1. <span data-ttu-id="fb9ee-115">Avviare PowerShell per verificare che sia installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-115">Start PowerShell to verify that it installed successfully.</span></span>

    ```bash
    pwsh
    ```
<span data-ttu-id="fb9ee-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="fb9ee-116">::: zone-end</span></span>

<span data-ttu-id="fb9ee-117">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="fb9ee-117">::: zone pivot="macos"</span></span>

## <a name="macos"></a><span data-ttu-id="fb9ee-118">MacOS</span><span class="sxs-lookup"><span data-stu-id="fb9ee-118">MacOS</span></span>

<span data-ttu-id="fb9ee-119">In macOS, il primo passaggio consiste nell'installare **PowerShell Core**.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-119">On macOS, the first step is to install **PowerShell Core**.</span></span> <span data-ttu-id="fb9ee-120">Questa operazione viene eseguita tramite la gestione pacchetti Homebrew.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-120">This is done using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb9ee-121">Se il comando **brew** non è disponibile, potrebbe essere necessario installare la gestione pacchetti Homebrew.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-121">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="fb9ee-122">Per informazioni dettagliate, vedere il [sito Web di Homebrew](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="fb9ee-122">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="fb9ee-123">Installare Homebrew-Cask per ottenere altri pacchetti, incluso il pacchetto di PowerShell Core:</span><span class="sxs-lookup"><span data-stu-id="fb9ee-123">Install Homebrew-Cask to obtain more packages, including the PowerShell Core package:</span></span>

    ```bash
    brew tap caskroom/cask
    ```

1. <span data-ttu-id="fb9ee-124">Installare PowerShell Core:</span><span class="sxs-lookup"><span data-stu-id="fb9ee-124">Install PowerShell Core:</span></span>

    ```bash
    brew cask install powershell
    ```

1. <span data-ttu-id="fb9ee-125">Avviare PowerShell Core per verificare che sia installato correttamente:</span><span class="sxs-lookup"><span data-stu-id="fb9ee-125">Start PowerShell Core to verify that it installed successfully:</span></span>

    ```bash
    pwsh
    ```

<span data-ttu-id="fb9ee-126">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="fb9ee-126">::: zone-end</span></span>

<span data-ttu-id="fb9ee-127">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="fb9ee-127">::: zone pivot="windows"</span></span>

## <a name="windows"></a><span data-ttu-id="fb9ee-128">Windows</span><span class="sxs-lookup"><span data-stu-id="fb9ee-128">Windows</span></span>
<span data-ttu-id="fb9ee-129">PowerShell è incluso in Windows, ma potrebbe essere disponibile un aggiornamento per il computer in uso.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-129">PowerShell is included with Windows, however there may be an update available for your machine.</span></span> <span data-ttu-id="fb9ee-130">Per il supporto di Azure che verrà usato è richiesto PowerShell versione 5.0 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-130">The Azure support we are going to use requires PowerShell version 5.0 or higher.</span></span> <span data-ttu-id="fb9ee-131">È possibile controllare la versione installata seguendo questa procedura:</span><span class="sxs-lookup"><span data-stu-id="fb9ee-131">You can check the version you have installed through the following steps:</span></span>

1. <span data-ttu-id="fb9ee-132">Aprire il menu **Start** e digitare **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-132">Open the **Start** menu and type **Windows PowerShell**.</span></span> <span data-ttu-id="fb9ee-133">Potrebbero essere disponibili più collegamenti:</span><span class="sxs-lookup"><span data-stu-id="fb9ee-133">There may be multiple shortcut links available:</span></span>
    - <span data-ttu-id="fb9ee-134">Windows PowerShell - questa è la versione a 64 bit e a livello generale quella da scegliere.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-134">Windows PowerShell - this is the 64-bit version and generally what you should choose.</span></span>
    - <span data-ttu-id="fb9ee-135">Windows PowerShell (x86) - questa è una versione a 32 bit installata in Windows a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-135">Windows PowerShell (x86) - this is a 32-bit version installed on 64-bit Windows.</span></span>
    - <span data-ttu-id="fb9ee-136">Windows PowerShell ISE - Integrated Scripting Environment (ISE) viene usato per la scrittura di script di PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-136">Windows PowerShell ISE - The Integrated Scripting Environment (ISE) is used for writing scripts in PowerShell.</span></span> 
    - <span data-ttu-id="fb9ee-137">Windows PowerShell ISE (x86) - Versione a 32 bit di ISE.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-137">Windows PowerShell ISE (x86) - A 32-bit version of the ISE.</span></span>

1. <span data-ttu-id="fb9ee-138">Selezionare l'icona di Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-138">Select the Windows PowerShell icon.</span></span>

1. <span data-ttu-id="fb9ee-139">Digitare il comando seguente per determinare la versione di PowerShell installata.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-139">Type the following command to determine the version of PowerShell installed.</span></span>

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
<span data-ttu-id="fb9ee-140">Se il numero di versione è inferiore a 5.0, seguire queste istruzioni per l'[aggiornamento della versione esistente di Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="fb9ee-140">If the version number is lower than 5.0, follow these instructions for [upgrading existing Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span></span>

<span data-ttu-id="fb9ee-141">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="fb9ee-141">::: zone-end</span></span>

<span data-ttu-id="fb9ee-142">Il computer locale è stato configurato per il supporto di PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-142">You have setup your local machine(s) to support PowerShell.</span></span> <span data-ttu-id="fb9ee-143">Verranno poi presentati altri comandi che è possibile aggiungere includendo il modulo di Azure.</span><span class="sxs-lookup"><span data-stu-id="fb9ee-143">Next, we will talk about additional commands you can add including the Azure module.</span></span>