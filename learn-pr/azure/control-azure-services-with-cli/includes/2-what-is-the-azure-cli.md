<span data-ttu-id="c0906-101">L'interfaccia della riga di comando di Azure è un programma della riga di comando per connettersi ad Azure ed eseguire comandi amministrativi sulle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="c0906-101">The Azure CLI is a command-line program to connect to Azure and execute administrative commands on Azure resources.</span></span> <span data-ttu-id="c0906-102">Viene eseguita in Linux, macOS e Windows e consente ad amministratori e sviluppatori di eseguire i comandi tramite un terminale o un prompt della riga di comando (o script) anziché tramite un Web browser.</span><span class="sxs-lookup"><span data-stu-id="c0906-102">It runs on Linux, macOS, and Windows and allows administrators and developers to execute their commands through a terminal or command line prompt (or script!) instead of a web browser.</span></span> <span data-ttu-id="c0906-103">Per riavviare una macchina virtuale, ad esempio, si userebbe un comando simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c0906-103">For example, to restart a virtual machine (VM), you would use a command like the following:</span></span>

 ```azurecli
 az vm restart -g MyResourceGroup -n MyVm
 ```

<span data-ttu-id="c0906-104">L'interfaccia della riga di comando di Azure offre strumenti da riga di comando multipiattaforma per la gestione delle risorse di Azure e può essere installata in locale in computer Linux, Mac o Windows.</span><span class="sxs-lookup"><span data-stu-id="c0906-104">The Azure CLI provides cross-platform command-line tools for managing Azure resources, and can be installed locally on Linux, Mac, or Windows computers.</span></span> <span data-ttu-id="c0906-105">L'interfaccia della riga di comando di Azure può anche essere usata da un browser tramite Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="c0906-105">The Azure CLI can also be used from a browser through the Azure Cloud Shell.</span></span> <span data-ttu-id="c0906-106">In entrambi i casi, può essere usata in modo interattivo o tramite script.</span><span class="sxs-lookup"><span data-stu-id="c0906-106">In both cases, it can be used interactively or scripted.</span></span> <span data-ttu-id="c0906-107">Per l'uso interattivo, si avvia prima di tutto una shell, ad esempio cmd.exe in Windows o Bash in Linux o macOS, quindi si esegue il comando al prompt della shell.</span><span class="sxs-lookup"><span data-stu-id="c0906-107">For interactive use, you first launch a shell such as cmd.exe on Windows or Bash on Linux or macOS and then issue the command at the shell prompt.</span></span> <span data-ttu-id="c0906-108">Per automatizzare attività ripetitive, i comandi dell'interfaccia della riga di comando vengono assemblati in uno script della shell tramite la sintassi per gli script della shell prescelta, quindi si esegue lo script.</span><span class="sxs-lookup"><span data-stu-id="c0906-108">To automate repetitive tasks, you assemble the CLI commands into a shell script using the script syntax of your chosen shell and then execute the script.</span></span>

## <a name="how-to-install-azure-cli"></a><span data-ttu-id="c0906-109">Come installare l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="c0906-109">How to install Azure CLI</span></span>

<span data-ttu-id="c0906-110">In Linux e macOS, si usa uno strumento di gestione pacchetti per installare PowerShell Core.</span><span class="sxs-lookup"><span data-stu-id="c0906-110">On both Linux and macOS, you use a package manager to install PowerShell Core.</span></span> <span data-ttu-id="c0906-111">Lo strumento di gestione pacchetti consigliato è diverso in base al sistema operativo e alla distribuzione:</span><span class="sxs-lookup"><span data-stu-id="c0906-111">The recommended package manager differs by OS and distribution:</span></span>
- <span data-ttu-id="c0906-112">Linux: **apt-get** su Ubuntu, **yum** su Red Hat e **zypper** su OpenSUSE</span><span class="sxs-lookup"><span data-stu-id="c0906-112">Linux: **apt-get** on Ubuntu, **yum** on Red Hat, and **zypper** on OpenSUSE</span></span>
- <span data-ttu-id="c0906-113">Mac: **Homebrew**</span><span class="sxs-lookup"><span data-stu-id="c0906-113">Mac: **Homebrew**</span></span>

<span data-ttu-id="c0906-114">L'interfaccia della riga di comando di Azure è disponibile nel repository Microsoft, quindi è prima necessario aggiungere tale repository allo strumento di gestione dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="c0906-114">The Azure CLI is available in the Microsoft repository, so you'll first need to add that repository to your package manager.</span></span>

<span data-ttu-id="c0906-115">Per installare l'interfaccia della riga di comando di Azure in Windows, occorre scaricare ed eseguire un file MSI.</span><span class="sxs-lookup"><span data-stu-id="c0906-115">On Windows, you install the Azure CLI by downloading and running an MSI file.</span></span>

## <a name="using-the-azure-cli-in-scripts"></a><span data-ttu-id="c0906-116">Uso dell'interfaccia della riga di comando di Azure negli script</span><span class="sxs-lookup"><span data-stu-id="c0906-116">Using the Azure CLI in scripts</span></span>

<span data-ttu-id="c0906-117">Se si vogliono usare i comandi dell'interfaccia della riga di comando di Azure negli script, è necessario essere a conoscenza di eventuali problemi correlati alla "shell" o all'ambiente usato per l'esecuzione dello script.</span><span class="sxs-lookup"><span data-stu-id="c0906-117">If you want to use the Azure CLI commands in scripts, you need to be aware of any issues around the "shell" or environment used for running the script.</span></span> <span data-ttu-id="c0906-118">In una shell bash, ad esempio, viene usata la sintassi seguente per l'impostazione delle variabili:</span><span class="sxs-lookup"><span data-stu-id="c0906-118">For example, in a bash shell, the following syntax is used when setting variables:</span></span>

 ```azurecli
 variable="value"
 variable=integer
 ```

<span data-ttu-id="c0906-119">Se si usa un ambiente di PowerShell per eseguire gli script dell'interfaccia della riga di comando di Azure, è necessario usare questa sintassi per le variabili:</span><span class="sxs-lookup"><span data-stu-id="c0906-119">If you use a PowerShell environment for running Azure CLI scripts, you'll need to use this syntax for variables:</span></span>

 ```powershell
 $variable="value"
 $variable=integer
 ```

## <a name="summary"></a><span data-ttu-id="c0906-120">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c0906-120">Summary</span></span>

<span data-ttu-id="c0906-121">L'interfaccia della riga di comando di Azure deve essere installata prima di poterla usare per gestire le risorse di Azure da un computer locale.</span><span class="sxs-lookup"><span data-stu-id="c0906-121">The Azure CLI must be installed before it can be used to manage Azure resources from a local computer.</span></span> <span data-ttu-id="c0906-122">La procedura di installazione varia per Windows, Linux e macOS, ma dopo l'installazione i comandi sono comuni tra le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="c0906-122">The installation steps vary for Windows, Linux, and macOS, but once installed, the commands are common across platforms.</span></span> 
