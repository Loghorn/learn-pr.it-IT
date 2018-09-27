<span data-ttu-id="77307-101">Per usare Visual Studio Code per lo sviluppo in Azure, è necessario installare Visual Studio Code in locale e una o più estensioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="77307-101">To use Visual Studio Code for Azure development, you'll need to install Visual Studio Code locally and one or more Azure extensions.</span></span> <span data-ttu-id="77307-102">In questo esercizio si aggiungerà l'estensione **Servizio app di Azure**.</span><span class="sxs-lookup"><span data-stu-id="77307-102">In this exercise, we'll add the **Azure App Service** extension.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="77307-103">Installare Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77307-103">Install Visual Studio Code</span></span>

<span data-ttu-id="77307-104">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="77307-104">::: zone pivot="windows"</span></span>

### <a name="windows"></a><span data-ttu-id="77307-105">Windows</span><span class="sxs-lookup"><span data-stu-id="77307-105">Windows</span></span>

1. <span data-ttu-id="77307-106">[Scaricare il programma di installazione di Visual Studio Code per Windows.](https://code.visualstudio.com/)</span><span class="sxs-lookup"><span data-stu-id="77307-106">[Download the Visual Studio Code installer for Windows](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="77307-107">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="77307-107">Run the installer.</span></span>

1. <span data-ttu-id="77307-108">Aprire Visual Studio Code premendo il tasto Windows oppure facendo clic sull'icona di Windows nella barra delle applicazioni digitando "Visual Studio Code" e facendo clic sulla voce **Visual Studio Code** visualizzata.</span><span class="sxs-lookup"><span data-stu-id="77307-108">Open Visual Studio Code by pressing the Windows key or clicking the Windows icon on the task bar, typing "Visual Studio Code" and clicking on the **Visual Studio Code** result.</span></span>

<span data-ttu-id="77307-109">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="77307-109">::: zone-end</span></span>

<span data-ttu-id="77307-110">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="77307-110">::: zone pivot="macos"</span></span>

### <a name="macos"></a><span data-ttu-id="77307-111">macOS</span><span class="sxs-lookup"><span data-stu-id="77307-111">macOS</span></span>

1. <span data-ttu-id="77307-112">[Scaricare Visual Studio Code per macOS](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="77307-112">[Download Visual Studio Code for macOS](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="77307-113">Fare doppio clic sull'archivio scaricato per espandere il contenuto.</span><span class="sxs-lookup"><span data-stu-id="77307-113">Double-click on the downloaded archive to expand the contents.</span></span>

1. <span data-ttu-id="77307-114">Trascinare Code.app di Visual Studio nella cartella Applicazioni.</span><span class="sxs-lookup"><span data-stu-id="77307-114">Drag Visual Studio Code.app to the Applications folder.</span></span>

1. <span data-ttu-id="77307-115">Aprire Visual Studio Code facendo clic sull'icona nella sezione App o eseguendo una ricerca di Visual Studio Code in Spotlight.</span><span class="sxs-lookup"><span data-stu-id="77307-115">Open Visual Studio Code by clicking on the icon the Apps section or by searching for Visual Studio Code in Spotlight.</span></span>

<span data-ttu-id="77307-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="77307-116">::: zone-end</span></span>

<span data-ttu-id="77307-117">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="77307-117">::: zone pivot="linux"</span></span>

### <a name="linux"></a><span data-ttu-id="77307-118">Linux</span><span class="sxs-lookup"><span data-stu-id="77307-118">Linux</span></span> 

#### <a name="debian-and-ubuntu"></a><span data-ttu-id="77307-119">Debian e Ubuntu</span><span class="sxs-lookup"><span data-stu-id="77307-119">Debian and Ubuntu</span></span>

1. <span data-ttu-id="77307-120">Scaricare e installare il [pacchetto DEB (64 bit)](https://go.microsoft.com/fwlink/?LinkID=760868) tramite un centro software con interfaccia grafica, se disponibile, o tramite la riga di comando (sostituendo `<file>` con il nome del file DEB scaricato):</span><span class="sxs-lookup"><span data-stu-id="77307-120">Download and install the [.deb package (64-bit)](https://go.microsoft.com/fwlink/?LinkID=760868) through the graphical software center, if it's available, or through the command line (replacing `<file>` with the .deb filename you downloaded):</span></span>

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

#### <a name="rhel-fedora-and-centos"></a><span data-ttu-id="77307-121">RHEL, Fedora e CentOS</span><span class="sxs-lookup"><span data-stu-id="77307-121">RHEL, Fedora, and CentOS</span></span>

1. <span data-ttu-id="77307-122">Usare lo script seguente per installare la chiave e il repository:</span><span class="sxs-lookup"><span data-stu-id="77307-122">Use the following script to install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    ```

1. <span data-ttu-id="77307-123">Aggiornare la cache dei pacchetti e installare il pacchetto usando dnf (Fedora 22 e versioni successive):</span><span class="sxs-lookup"><span data-stu-id="77307-123">Update the package cache, and install the package by using dnf (Fedora 22 and above):</span></span>

    ```bash
    dnf check-update
    sudo dnf install code
    ```

#### <a name="opensuse-and-sle"></a><span data-ttu-id="77307-124">openSUSE e SLE</span><span class="sxs-lookup"><span data-stu-id="77307-124">openSUSE and SLE</span></span>

1. <span data-ttu-id="77307-125">Il repository yum funziona anche per sistemi basati su openSUSE e SLE.</span><span class="sxs-lookup"><span data-stu-id="77307-125">The yum repository also works for openSUSE and SLE based systems.</span></span> <span data-ttu-id="77307-126">Lo script seguente installerà la chiave e il repository:</span><span class="sxs-lookup"><span data-stu-id="77307-126">The following script will install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
    ```

1. <span data-ttu-id="77307-127">Aggiornare la cache dei pacchetti e installare il pacchetto con:</span><span class="sxs-lookup"><span data-stu-id="77307-127">Update the package cache and install the package by using:</span></span>

    ```bash
    sudo zypper refresh
    sudo zypper install code
    ```

> [!NOTE]
> <span data-ttu-id="77307-128">Per altri dettagli sull'installazione o l'aggiornamento di Visual Studio Code in varie distribuzioni Linux, vedere la [documentazione per l'esecuzione di Visual Studio Code in Linux](https://code.visualstudio.com/docs/setup/linux).</span><span class="sxs-lookup"><span data-stu-id="77307-128">For further details about installing or updating Visual Studio Code on various Linux distributions, please see the [Running Visual Studio Code on Linux documentation](https://code.visualstudio.com/docs/setup/linux).</span></span>

<span data-ttu-id="77307-129">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="77307-129">::: zone-end</span></span>

## <a name="install-azure-app-service-extension"></a><span data-ttu-id="77307-130">Installare l'estensione Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="77307-130">Install Azure App Service extension</span></span>

1. <span data-ttu-id="77307-131">Se non è già aperto, aprire Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="77307-131">If you haven't already, open Visual Studio Code.</span></span>

1. <span data-ttu-id="77307-132">Aprire il browser Estensioni. È accessibile tramite il menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="77307-132">Open the Extensions browser; it's accessed via the menu on the left.</span></span>

1. <span data-ttu-id="77307-133">Cercare **Servizio app di Azure**.</span><span class="sxs-lookup"><span data-stu-id="77307-133">Search for **Azure App Service**.</span></span>

1. <span data-ttu-id="77307-134">Selezionare **Servizio app di Azure** tra i risultati e fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="77307-134">Select the **Azure App Service** result and click **Install**.</span></span>

    <span data-ttu-id="77307-135">Lo screenshot seguente illustra l'estensione di Servizio app di Azure selezionata nei risultati di ricerca delle estensioni in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="77307-135">The following screenshot shows the Azure App Service extension selected from the Visual Studio Code extension search results.</span></span>

    ![Screenshot di Visual Studio Code che mostra la scheda Estensioni con l'estensione di Servizio app di Azure evidenziata nei risultati della ricerca.](../media/3-install-azure-extension.png)

<span data-ttu-id="77307-137">Verrà così installata l'estensione.</span><span class="sxs-lookup"><span data-stu-id="77307-137">This will install the extension.</span></span> <span data-ttu-id="77307-138">È ora possibile connettersi alla sottoscrizione di Azure e distribuire un'app Web, per dispositivi mobili o API in Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="77307-138">You're now ready to connect to your Azure subscription and deploy a web, mobile, or API app to an Azure App Service.</span></span>
