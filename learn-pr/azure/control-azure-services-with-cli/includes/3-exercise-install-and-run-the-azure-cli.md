---
zone_pivot_groups: platform
ms.openlocfilehash: 5e0a236b9cf0c3c0b23beb1324f35a34dade2e92
ms.sourcegitcommit: 926510a198d738c5726081f6d7994fe9b6fc6edb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43179829"
---
<span data-ttu-id="67e7d-101">Verrà ora installata l'interfaccia della riga di comando di Azure nel computer locale e dopo verrà eseguito un semplice comando per verificare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="67e7d-101">Let's install the Azure CLI on your local machine, and then run a simple command to verify your installation.</span></span> <span data-ttu-id="67e7d-102">Il metodo usato per l'installazione dell'interfaccia della riga di comando di Azure dipende dal sistema operativo del computer in uso.</span><span class="sxs-lookup"><span data-stu-id="67e7d-102">The method you use for installing the Azure CLI depends on the operating system of your computer.</span></span> <span data-ttu-id="67e7d-103">Scegliere la procedura corretta per il sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="67e7d-103">Please choose the steps for your operating system.</span></span>

<span data-ttu-id="67e7d-104">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="67e7d-104">::: zone pivot="linux"</span></span>

### <a name="linux"></a><span data-ttu-id="67e7d-105">Linux</span><span class="sxs-lookup"><span data-stu-id="67e7d-105">Linux</span></span>
<span data-ttu-id="67e7d-106">L'interfaccia della riga di comando di Azure verrà installata in **Ubuntu Linux** usando Advanced Packaging Tool (**apt**) e la riga di comando di Bash.</span><span class="sxs-lookup"><span data-stu-id="67e7d-106">Here you will install the Azure CLI on **Ubuntu Linux** using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span>

> [!WARNING]
> <span data-ttu-id="67e7d-107">I comandi elencati di seguito sono per Ubuntu versione 18.04.</span><span class="sxs-lookup"><span data-stu-id="67e7d-107">The commands listed below are for Ubuntu version 18.04.</span></span> <span data-ttu-id="67e7d-108">Se si usa una versione diversa di Ubuntu, è necessario aggiungere un altro repository.</span><span class="sxs-lookup"><span data-stu-id="67e7d-108">If you are using a different version of Ubuntu, you must add a different repository.</span></span> <span data-ttu-id="67e7d-109">Per informazioni dettagliate, vedere [Installare l'interfaccia della riga di comando di Azure 2.0 con APT](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).</span><span class="sxs-lookup"><span data-stu-id="67e7d-109">For details see [Install the Azure CLI 2.0 with apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).</span></span>

1. <span data-ttu-id="67e7d-110">Modificare l'elenco di origini in modo che il repository di Microsoft venga registrato e la gestione dei pacchetti possa individuare il pacchetto dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="67e7d-110">Modify your sources list so that the Microsoft repository is registered, and the package manager can locate the Azure CLI package.</span></span>

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

1. <span data-ttu-id="67e7d-111">Importare la chiave di crittografia per il repository Microsoft Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="67e7d-111">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="67e7d-112">Questo consente allo strumento di gestione pacchetti di verificare che il pacchetto dell'interfaccia della riga di comando di Azure da installare sia fornito da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="67e7d-112">This will allow the package manager to verify that the Azure CLI package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="67e7d-113">Installare l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="67e7d-113">Install the Azure CLI.</span></span>

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

<span data-ttu-id="67e7d-114">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="67e7d-114">::: zone-end</span></span>

<span data-ttu-id="67e7d-115">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="67e7d-115">::: zone pivot="macos"</span></span>

### <a name="macos"></a><span data-ttu-id="67e7d-116">macOS</span><span class="sxs-lookup"><span data-stu-id="67e7d-116">macOS</span></span>
<span data-ttu-id="67e7d-117">Di seguito si installerà l'interfaccia della riga di comando di Azure in macOS tramite la gestione pacchetti Homebrew.</span><span class="sxs-lookup"><span data-stu-id="67e7d-117">Here you will install the Azure CLI on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67e7d-118">Se il comando **brew** non è disponibile, potrebbe essere necessario installare la gestione pacchetti HomeBrew.</span><span class="sxs-lookup"><span data-stu-id="67e7d-118">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="67e7d-119">Per informazioni dettagliate, vedere il [sito Web di Homebrew](https://brew.sh/).</span><span class="sxs-lookup"><span data-stu-id="67e7d-119">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="67e7d-120">Aggiornare il repository brew per assicurarsi di ottenere il pacchetto più recente dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="67e7d-120">Update your brew repository to make sure you get the latest Azure CLI package.</span></span>

    ```bash
    brew update
    ```

1. <span data-ttu-id="67e7d-121">Installare l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="67e7d-121">Install the Azure CLI.</span></span>

    ```bash
    brew install azure-cli
    ```

<span data-ttu-id="67e7d-122">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="67e7d-122">::: zone-end</span></span>

<span data-ttu-id="67e7d-123">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="67e7d-123">::: zone pivot="windows"</span></span>

### <a name="windows"></a><span data-ttu-id="67e7d-124">Windows</span><span class="sxs-lookup"><span data-stu-id="67e7d-124">Windows</span></span>
<span data-ttu-id="67e7d-125">Di seguito si installerà l'interfaccia della riga di comando di Azure in Windows tramite il programma di installazione MSI.</span><span class="sxs-lookup"><span data-stu-id="67e7d-125">Here you will install the Azure CLI on Windows using the MSI installer.</span></span>

1. <span data-ttu-id="67e7d-126">Passare a [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows) e nella finestra di dialogo della sicurezza del browser fare clic su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="67e7d-126">Go to [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), and in the browser security dialog box, click **Run**.</span></span>
1. <span data-ttu-id="67e7d-127">Nel programma di installazione accettare le condizioni di licenza e quindi fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="67e7d-127">In the installer, accept the license terms, and then click **Install**.</span></span>
1. <span data-ttu-id="67e7d-128">Nella finestra di dialogo **Controllo account utente** selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="67e7d-128">In the **User Account Control** dialog, select **Yes**.</span></span>

<span data-ttu-id="67e7d-129">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="67e7d-129">::: zone-end</span></span>

## <a name="running-the-azure-cli"></a><span data-ttu-id="67e7d-130">Esecuzione dell'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="67e7d-130">Running the Azure CLI</span></span>
<span data-ttu-id="67e7d-131">Per eseguire l'interfaccia della riga di comando di Azure, aprire una shell bash (Linux e macOS) o usare il prompt dei comandi o PowerShell (Windows).</span><span class="sxs-lookup"><span data-stu-id="67e7d-131">You run the Azure CLI by opening a bash shell (Linux and macOS), or from the command prompt or PowerShell (Windows).</span></span>

1. <span data-ttu-id="67e7d-132">Avviare l'interfaccia della riga di comando di Azure e verificare l'installazione eseguendo il controllo della versione.</span><span class="sxs-lookup"><span data-stu-id="67e7d-132">Start the Azure CLI and verify your installation by running the version check.</span></span>

    ```bash
    az --version
    ```

<span data-ttu-id="67e7d-133">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="67e7d-133">::: zone pivot="windows"</span></span>

> [!NOTE]
> <span data-ttu-id="67e7d-134">L'esecuzione dell'interfaccia della riga di comando di Azure da PowerShell presenta alcuni vantaggi rispetto all'esecuzione dal prompt dei comandi di Windows.</span><span class="sxs-lookup"><span data-stu-id="67e7d-134">Running the Azure CLI from PowerShell has some advantages over running the Azure CLI from the Windows command prompt.</span></span> <span data-ttu-id="67e7d-135">PowerShell offre altre funzionalità di completamento con il tasto TAB oltre a quelle disponibili dal prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="67e7d-135">PowerShell provides additional tab completion features over those available from the command prompt.</span></span> 

<span data-ttu-id="67e7d-136">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="67e7d-136">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="67e7d-137">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="67e7d-137">Summary</span></span>
<span data-ttu-id="67e7d-138">I computer locali sono stati configurati per amministrare le risorse di Azure con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="67e7d-138">You set up your local machines to administer Azure resources with the Azure CLI.</span></span> <span data-ttu-id="67e7d-139">È ora possibile usare l'interfaccia della riga di comando di Azure in locale per immettere comandi o eseguire script.</span><span class="sxs-lookup"><span data-stu-id="67e7d-139">You can now use the Azure CLI locally to enter commands or execute scripts.</span></span> <span data-ttu-id="67e7d-140">L'interfaccia della riga di comando di Azure inoltrerà i comandi ai data center di Azure dove verranno eseguiti all'interno della sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="67e7d-140">The Azure CLI will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>