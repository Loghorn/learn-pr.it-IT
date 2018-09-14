<span data-ttu-id="71235-101">In questa unità, verrà installato **PowerShell** nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="71235-101">In this unit, you will install **PowerShell** on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="71235-102">Questa esercitazione descrive l'installazione gli strumenti di PowerShell in locale.</span><span class="sxs-lookup"><span data-stu-id="71235-102">This exercise guides you through installing the PowerShell tools locally.</span></span> <span data-ttu-id="71235-103">Nella parte restante del modulo verrà usato Azure Cloud Shell in modo da sfruttare il supporto gratuito per la sottoscrizione in Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="71235-103">The remainder of the module will use the Azure Cloud Shell so you can leverage the free subscription support in Microsoft Learn.</span></span> <span data-ttu-id="71235-104">Se si preferisce, è possibile considerare questo esercizio come un'attività facoltativa e limitarsi a leggere le istruzioni.</span><span class="sxs-lookup"><span data-stu-id="71235-104">You can consider this exercise as an optional activity and just review the instructions if you prefer.</span></span>

<span data-ttu-id="71235-105">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="71235-105">::: zone pivot="linux"</span></span>

## <a name="linux"></a><span data-ttu-id="71235-106">Linux</span><span class="sxs-lookup"><span data-stu-id="71235-106">Linux</span></span>

<span data-ttu-id="71235-107">Installazione di PowerShell per Linux comporterà l'uso di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="71235-107">Installing PowerShell for Linux will involve using a package manager.</span></span> <span data-ttu-id="71235-108">In questo esempio verrà usato **Ubuntu 18.04**, ma sono disponibili [istruzioni dettagliate per le altre versioni e distribuzioni nella documentazione](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span><span class="sxs-lookup"><span data-stu-id="71235-108">We will use **Ubuntu 18.04** for our example here, but we have [detailed instructions for other versions and distributions in our documentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span></span>

<span data-ttu-id="71235-109">PowerShell Core verrà installato in Ubuntu Linux tramite Advanced Packaging Tool (**apt**) e la riga di comando di Bash.</span><span class="sxs-lookup"><span data-stu-id="71235-109">You will install PowerShell Core on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span> 

1. <span data-ttu-id="71235-110">Importare la chiave di crittografia per il repository Ubuntu di Microsoft.</span><span class="sxs-lookup"><span data-stu-id="71235-110">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="71235-111">Questo consente allo strumento di gestione pacchetti di verificare che il pacchetto di PowerShell Core da installare sia fornito da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="71235-111">This will allow the package manager to verify that the PowerShell Core package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="71235-112">Registrare il **repository Ubuntu di Microsoft**, in modo da consentire allo strumento di gestione pacchetti di individuare il pacchetto di PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="71235-112">Register the **Microsoft Ubuntu repository** so the package manager can locate the PowerShell Core package.</span></span>

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. <span data-ttu-id="71235-113">Aggiornare l'elenco dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="71235-113">Update the list of packages.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="71235-114">Installare PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="71235-114">Install PowerShell Core.</span></span>

    ```bash
    sudo apt-get install -y powershell
    ```

1. <span data-ttu-id="71235-115">Avviare PowerShell per verificare che sia installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="71235-115">Start PowerShell to verify that it installed successfully.</span></span>

    ```bash
    pwsh
    ```
<span data-ttu-id="71235-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="71235-116">::: zone-end</span></span>

<span data-ttu-id="71235-117">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="71235-117">::: zone pivot="macos"</span></span>

## <a name="macos"></a><span data-ttu-id="71235-118">MacOS</span><span class="sxs-lookup"><span data-stu-id="71235-118">MacOS</span></span>

<span data-ttu-id="71235-119">In macOS, il primo passaggio consiste nell'installare **PowerShell Core**.</span><span class="sxs-lookup"><span data-stu-id="71235-119">On macOS, the first step is to install **PowerShell Core**.</span></span> <span data-ttu-id="71235-120">Questa operazione viene eseguita tramite Gestione pacchetti Homebrew.</span><span class="sxs-lookup"><span data-stu-id="71235-120">This is done using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="71235-121">Se il comando **brew** non è disponibile, potrebbe essere necessario installare la gestione pacchetti Homebrew.</span><span class="sxs-lookup"><span data-stu-id="71235-121">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="71235-122">Per informazioni dettagliate, vedere il [sito Web di Homebrew](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="71235-122">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="71235-123">Installare Homebrew-Cask per ottenere altri pacchetti, incluso il pacchetto di PowerShell Core:</span><span class="sxs-lookup"><span data-stu-id="71235-123">Install Homebrew-Cask to obtain more packages, including the PowerShell Core package:</span></span>

    ```bash
    brew tap caskroom/cask
    ```

1. <span data-ttu-id="71235-124">Installare PowerShell Core:</span><span class="sxs-lookup"><span data-stu-id="71235-124">Install PowerShell Core:</span></span>

    ```bash
    brew cask installs powershell
    ```

1. <span data-ttu-id="71235-125">Avviare PowerShell Core per verificare che sia installato correttamente:</span><span class="sxs-lookup"><span data-stu-id="71235-125">Start PowerShell Core to verify that it installed successfully:</span></span>

    ```bash
    pwsh
    ```

<span data-ttu-id="71235-126">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="71235-126">::: zone-end</span></span>

<span data-ttu-id="71235-127">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="71235-127">::: zone pivot="windows"</span></span>

## <a name="windows"></a><span data-ttu-id="71235-128">Windows</span><span class="sxs-lookup"><span data-stu-id="71235-128">Windows</span></span>
<span data-ttu-id="71235-129">PowerShell è incluso in Windows, potrebbe essere tuttavia presenti un aggiornamento disponibile per la macchina.</span><span class="sxs-lookup"><span data-stu-id="71235-129">PowerShell is included with Windows, however there may be an update available for your machine.</span></span> <span data-ttu-id="71235-130">Il supporto tecnico di Azure che si intende usare richiede PowerShell versione 5.0 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="71235-130">The Azure support we are going to use requires PowerShell version 5.0 or higher.</span></span> <span data-ttu-id="71235-131">È possibile controllare la versione installata seguendo questa procedura:</span><span class="sxs-lookup"><span data-stu-id="71235-131">You can check the version you have installed through the following steps:</span></span>

1. <span data-ttu-id="71235-132">Aprire il menu **Start** e digitare **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="71235-132">Open the **Start** menu and type **Windows PowerShell**.</span></span> <span data-ttu-id="71235-133">Potrebbero essere presenti più collegamenti di scelta rapida disponibili:</span><span class="sxs-lookup"><span data-stu-id="71235-133">There may be multiple shortcut links available:</span></span>
    - <span data-ttu-id="71235-134">Windows PowerShell - questa è la versione a 64 bit e a livello generale ciò che è consigliabile scegliere.</span><span class="sxs-lookup"><span data-stu-id="71235-134">Windows PowerShell - this is the 64-bit version and generally what you should choose.</span></span>
    - <span data-ttu-id="71235-135">Windows PowerShell (x86)-Questa è una versione a 32 bit installata in Windows a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="71235-135">Windows PowerShell (x86) - this is a 32-bit version installed on 64-bit Windows.</span></span>
    - <span data-ttu-id="71235-136">Windows PowerShell ISE - Integrated Scripting Environment (ISE) viene usato per la scrittura di script di PowerShell.</span><span class="sxs-lookup"><span data-stu-id="71235-136">Windows PowerShell ISE - The Integrated Scripting Environment (ISE) is used for writing scripts in PowerShell.</span></span> 
    - <span data-ttu-id="71235-137">Windows PowerShell ISE (x86)-versione A 32 bit di ISE.</span><span class="sxs-lookup"><span data-stu-id="71235-137">Windows PowerShell ISE (x86) - A 32-bit version of the ISE.</span></span>

1. <span data-ttu-id="71235-138">Selezionare l'icona di Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="71235-138">Select the Windows PowerShell icon.</span></span>

1. <span data-ttu-id="71235-139">Digitare il comando seguente per determinare la versione di PowerShell installata.</span><span class="sxs-lookup"><span data-stu-id="71235-139">Type the following command to determine the version of PowerShell installed.</span></span>

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
<span data-ttu-id="71235-140">Se il numero di versione è inferiore alla versione 5.0, seguire queste istruzioni per la [aggiornamento di PowerShell di Windows esistente](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="71235-140">If the version number is lower than 5.0, follow these instructions for [upgrading existing Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span></span>

<span data-ttu-id="71235-141">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="71235-141">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="71235-142">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="71235-142">Summary</span></span>
<span data-ttu-id="71235-143">Aver configurato il computer locale per il supporto di PowerShell.</span><span class="sxs-lookup"><span data-stu-id="71235-143">You have setup your local machine(s) to support PowerShell.</span></span> <span data-ttu-id="71235-144">Successivamente, si discuterà comandi aggiuntivi, che è possibile aggiungere tra cui il modulo di Azure.</span><span class="sxs-lookup"><span data-stu-id="71235-144">Next, we will talk about additional commands you can add including the Azure module.</span></span>