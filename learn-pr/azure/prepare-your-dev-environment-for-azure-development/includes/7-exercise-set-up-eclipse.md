<span data-ttu-id="e8659-101">Qui si installerà Eclipse e Azure Toolkit nel proprio computer di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="e8659-101">Here you'll install Eclipse and the Azure Toolkit on your development machine.</span></span> <span data-ttu-id="e8659-102">Al termine dell'esercizio si avranno a disposizione tutti gli strumenti necessari per creare un'applicazione Java collegata ad Azure.</span><span class="sxs-lookup"><span data-stu-id="e8659-102">By the end of the exercise, you'll have everything you need to create a Java application connected to Azure.</span></span>

## <a name="install-eclipse-ide"></a><span data-ttu-id="e8659-103">Installare l'IDE di Eclipse</span><span class="sxs-lookup"><span data-stu-id="e8659-103">Install Eclipse IDE</span></span>

1. <span data-ttu-id="e8659-104">Scaricare [l'IDE Eclipse appropriato per il proprio sistema operativo](https://www.eclipse.org/downloads/packages/installer).</span><span class="sxs-lookup"><span data-stu-id="e8659-104">Download the appropriate [Eclipse IDE for your operating system](https://www.eclipse.org/downloads/packages/installer).</span></span>

1. <span data-ttu-id="e8659-105">Avviare il programma di installazione di Eclipse dopo averlo scaricato.</span><span class="sxs-lookup"><span data-stu-id="e8659-105">Start the Eclipse installer once downloaded.</span></span>

    1. <span data-ttu-id="e8659-106">In Windows fare doppio clic sul file scaricato.</span><span class="sxs-lookup"><span data-stu-id="e8659-106">On Windows, double-click the downloaded file.</span></span>

    1. <span data-ttu-id="e8659-107">In macOS e Linux decomprimere il programma di installazione dal file scaricato ed eseguirlo.</span><span class="sxs-lookup"><span data-stu-id="e8659-107">On macOS and Linux, unzip the installer from the downloaded file and run it.</span></span>

        > [!NOTE]
        > <span data-ttu-id="e8659-108">Il programma di installazione potrebbe richiedere di installare Java Development Kit, se risulta mancante.</span><span class="sxs-lookup"><span data-stu-id="e8659-108">The installer may prompt you to install the Java Development Kit, if it is missing.</span></span>

1. <span data-ttu-id="e8659-109">Selezionare i pacchetti da installare.</span><span class="sxs-lookup"><span data-stu-id="e8659-109">Select the packages to install.</span></span> <span data-ttu-id="e8659-110">Per gli sviluppatori Java, scegliere l'opzione per l'IDE di Eclipse Java o Jave EE.</span><span class="sxs-lookup"><span data-stu-id="e8659-110">For Java developers, choose either the Java or Java EE Eclipse IDE option.</span></span>

1. <span data-ttu-id="e8659-111">Selezionare la destinazione di installazione nel computer.</span><span class="sxs-lookup"><span data-stu-id="e8659-111">Select the installation destination on your machine.</span></span>

1. <span data-ttu-id="e8659-112">Avviare Eclipse per verificare che sia stato installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="e8659-112">Launch Eclipse to validate that it installed correctly.</span></span>

## <a name="install-azure-toolkit-for-eclipse"></a><span data-ttu-id="e8659-113">Installare Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="e8659-113">Install Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="e8659-114">La procedura per l'installazione di Azure Toolkit è identica in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="e8659-114">Installing the Azure Toolkit is the same across Windows, macOS, and Linux.</span></span>

1. <span data-ttu-id="e8659-115">Avviare Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e8659-115">Start Eclipse.</span></span>

1. <span data-ttu-id="e8659-116">Passare a **Help** > **Install New Software** (Guida, Installa nuovo software).</span><span class="sxs-lookup"><span data-stu-id="e8659-116">Go to **Help** > **Install New Software...**.</span></span>

    <span data-ttu-id="e8659-117">Lo screenshot seguente mostra la posizione nel menu della voce **Install New Software** (Installa nuovo software).</span><span class="sxs-lookup"><span data-stu-id="e8659-117">The following screenshot shows the menu location of the **Install New Software...** item.</span></span>

    ![Screenshot dell'opzione Install New Software (Installa nuovo software) evidenziata all'interno del menu Help (Guida) di Eclipse.](../media/7-eclipse-install-new-software.png)

1. <span data-ttu-id="e8659-119">Verrà aperta la finestra di dialogo **Available Software** (Software disponibile).</span><span class="sxs-lookup"><span data-stu-id="e8659-119">The **Available Software** dialog will open.</span></span> <span data-ttu-id="e8659-120">Nella casella di testo **Work with** (Usa) immettere `http://dl.microsoft.com/eclipse/` e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="e8659-120">In the **Work with:** text box, type `http://dl.microsoft.com/eclipse/` and press Enter.</span></span>

1. <span data-ttu-id="e8659-121">Nei risultati selezionare l'opzione **Azure Toolkit for Java**.</span><span class="sxs-lookup"><span data-stu-id="e8659-121">In the results, check the **Azure Toolkit for Java** option.</span></span> <span data-ttu-id="e8659-122">Assicurarsi di deselezionare l'opzione **Contact all update sites during install to find required software** (Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto).</span><span class="sxs-lookup"><span data-stu-id="e8659-122">Make sure you uncheck the **Contact all update sites during install to find required software** option, if it isn't already.</span></span>

    <span data-ttu-id="e8659-123">La schermata seguente mostra la configurazione di installazione **Available Software** (Software disponibile) sopra descritta.</span><span class="sxs-lookup"><span data-stu-id="e8659-123">The following screenshot shows the **Available Software** install configuration as described above.</span></span>

    ![Screenshot della finestra Available Software (Software disponibile) in Eclipse, con riquadri che evidenziano la configurazione necessaria per trovare e installare Azure Toolkit for Java.](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. <span data-ttu-id="e8659-125">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e8659-125">Click **Next**.</span></span>

1. <span data-ttu-id="e8659-126">Rivedere e accettare i contratti di licenza quando richiesto e fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="e8659-126">Review and accept the license agreements when prompted, and click **Finish**.</span></span>

1. <span data-ttu-id="e8659-127">Eclipse scarica e installa Azure Toolkit.</span><span class="sxs-lookup"><span data-stu-id="e8659-127">Eclipse will download and install the Azure Toolkit.</span></span>

1. <span data-ttu-id="e8659-128">Se richiesto, riavviare Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e8659-128">Restart Eclipse if required.</span></span>

1. <span data-ttu-id="e8659-129">Convalidare l'installazione di Azure Toolkit verificando che sia disponibile la voce di menu **Tools** > **Azure** (Strumenti, Azure) in Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e8659-129">Validate the Azure Toolkit installation by verifying that you can find a **Tools** > **Azure** menu option in Eclipse.</span></span>
