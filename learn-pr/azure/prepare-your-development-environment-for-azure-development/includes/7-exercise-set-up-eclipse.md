<span data-ttu-id="e42db-101">In questo esercizio verrà installato Eclipse nel computer locale e quindi si installerà Azure Toolkit, in preparazione per lo sviluppo di applicazioni Java con l'integrazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="e42db-101">In this exercise, you will install Eclipse on your local machine and then install the Azure Toolkit, preparing you for developing Java applications with Azure integration.</span></span> <span data-ttu-id="e42db-102">L'installazione è rapida e semplice.</span><span class="sxs-lookup"><span data-stu-id="e42db-102">The installation is quick and simple.</span></span> <span data-ttu-id="e42db-103">Al termine dell'esercizio, sarà pronto tutto quello che serve per avviare la prima applicazione Java, sfruttando le funzionalità e i servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="e42db-103">At the end of the exercise, you will have everything set up that you need to start your first Java application, taking advantage of the features and services of Azure.</span></span>

## <a name="install-eclipse-ide"></a><span data-ttu-id="e42db-104">Installare l'IDE di Eclipse</span><span class="sxs-lookup"><span data-stu-id="e42db-104">Install Eclipse IDE</span></span>

1. <span data-ttu-id="e42db-105">Scaricare la versione di Eclipse appropriata per il sistema operativo da http://www.eclipse.org/downloads/packages/installer.</span><span class="sxs-lookup"><span data-stu-id="e42db-105">Download the Eclipse version that suits your operating system from http://www.eclipse.org/downloads/packages/installer.</span></span>

1. <span data-ttu-id="e42db-106">Avviare il programma di installazione di Eclipse dopo averlo scaricato.</span><span class="sxs-lookup"><span data-stu-id="e42db-106">Start the Eclipse installer once downloaded.</span></span>

    1. <span data-ttu-id="e42db-107">In Windows, fare doppio clic sul file scaricato.</span><span class="sxs-lookup"><span data-stu-id="e42db-107">On Windows, double-click the downloaded file.</span></span>

    1. <span data-ttu-id="e42db-108">In macOS e Linux, decomprimere il programma di installazione dal file scaricato.</span><span class="sxs-lookup"><span data-stu-id="e42db-108">On macOS and Linux, unzip the installer from the downloaded file.</span></span> <span data-ttu-id="e42db-109">Avviare quindi il programma di installazione dopo averlo decompresso.</span><span class="sxs-lookup"><span data-stu-id="e42db-109">Then start the installer once unzipped.</span></span>

        > [!NOTE]
        > <span data-ttu-id="e42db-110">Il programma di installazione potrebbe richiedere di installare Java Development Kit, se risulta mancante.</span><span class="sxs-lookup"><span data-stu-id="e42db-110">The installer may prompt you to install the Java Development Kit, if it is missing.</span></span>

1. <span data-ttu-id="e42db-111">Selezionare i pacchetti da installare.</span><span class="sxs-lookup"><span data-stu-id="e42db-111">Select the packages to install.</span></span> <span data-ttu-id="e42db-112">Per gli sviluppatori Java, scegliere l'opzione per l'IDE di Eclipse Java o Jave EE.</span><span class="sxs-lookup"><span data-stu-id="e42db-112">For Java developers, choose either the Java or Java EE Eclipse IDE option.</span></span>

1. <span data-ttu-id="e42db-113">Selezionare la destinazione di installazione nel computer.</span><span class="sxs-lookup"><span data-stu-id="e42db-113">Select the installation destination on your machine.</span></span>

1. <span data-ttu-id="e42db-114">Avviare Eclipse per verificare che sia stato installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="e42db-114">Launch Eclipse to validate that it installed correctly.</span></span>

## <a name="install-azure-toolkit-for-eclipse"></a><span data-ttu-id="e42db-115">Installare Azure Toolkit for Eclipse</span><span class="sxs-lookup"><span data-stu-id="e42db-115">Install Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="e42db-116">La procedura per l'installazione di Azure Toolkit è identica in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="e42db-116">Installing the Azure Toolkit is the same across Windows, macOS, and Linux.</span></span>

1. <span data-ttu-id="e42db-117">Avviare Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e42db-117">Start Eclipse.</span></span>

1. <span data-ttu-id="e42db-118">Passare a **Help** > **Install New Software** (Guida, Installa nuovo software).</span><span class="sxs-lookup"><span data-stu-id="e42db-118">Go to **Help** > **Install New Software...**.</span></span>

    <span data-ttu-id="e42db-119">Lo screenshot seguente mostra la posizione nel menu della voce **Install New Software** (Installa nuovo software).</span><span class="sxs-lookup"><span data-stu-id="e42db-119">The following screenshot shows the menu location of the **Install New Software...** item.</span></span>

    ![Screenshot dell'opzione Install New Software (Installa nuovo software) evidenziata all'interno del menu Help (Guida) di Eclipse.](../media/7-eclipse-install-new-software.png)

1. <span data-ttu-id="e42db-121">Verrà aperta la finestra di dialogo **Available Software** (Software disponibile).</span><span class="sxs-lookup"><span data-stu-id="e42db-121">The **Available Software** dialog will open.</span></span> <span data-ttu-id="e42db-122">Nella casella di testo **Work with** (Usa) immettere `http://dl.microsoft.com/eclipse/` e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="e42db-122">In the **Work with:** text box, type `http://dl.microsoft.com/eclipse/` and press Enter.</span></span>

1. <span data-ttu-id="e42db-123">Nei risultati selezionare l'opzione **Azure Toolkit for Java**.</span><span class="sxs-lookup"><span data-stu-id="e42db-123">In the results, check the **Azure Toolkit for Java** option.</span></span> <span data-ttu-id="e42db-124">Assicurarsi di deselezionare l'opzione **Contact all update sites during install to find required software** (Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto).</span><span class="sxs-lookup"><span data-stu-id="e42db-124">Make sure you uncheck the **Contact all update sites during install to find required software** option, if it isn't already.</span></span>

    <span data-ttu-id="e42db-125">La schermata seguente mostra la configurazione di installazione **Available Software** (Software disponibile) sopra descritta.</span><span class="sxs-lookup"><span data-stu-id="e42db-125">The following screenshot shows the **Available Software** install configuration as described above.</span></span>

    ![Screenshot della finestra Available Software (Software disponibile) in Eclipse, con riquadri che evidenziano la configurazione necessaria per trovare e installare Azure Toolkit for Java.](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. <span data-ttu-id="e42db-127">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e42db-127">Click **Next**.</span></span>

1. <span data-ttu-id="e42db-128">Rivedere e accettare i contratti di licenza quando richiesto e fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="e42db-128">Review and accept the license agreements when prompted, and click **Finish**.</span></span>

1. <span data-ttu-id="e42db-129">Eclipse scarica e installa Azure Toolkit.</span><span class="sxs-lookup"><span data-stu-id="e42db-129">Eclipse will download and install the Azure Toolkit.</span></span>

1. <span data-ttu-id="e42db-130">Se richiesto, riavviare Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e42db-130">Restart Eclipse if required.</span></span>

1. <span data-ttu-id="e42db-131">Convalidare l'installazione di Azure Toolkit verificando che sia disponibile la voce di menu **Tools** > **Azure** (Strumenti, Azure) in Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e42db-131">Validate installation of Azure Toolkit by verifying that you can find a **Tools** > **Azure** menu option in Eclipse.</span></span>

## <a name="summary"></a><span data-ttu-id="e42db-132">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e42db-132">Summary</span></span>

<span data-ttu-id="e42db-133">In questa unità è stato installato Eclipse per Java e il programma è stato preparato per sfruttare i vantaggi dell'integrazione con i prodotti e i servizi Azure.</span><span class="sxs-lookup"><span data-stu-id="e42db-133">In this unit, you installed Eclipse for Java, and prepared it to take advantage of the integration with Azure services and products.</span></span> <span data-ttu-id="e42db-134">L'installazione è rapida e semplice e ciò rende Eclipse uno strumento ideale per l'attività di sviluppo Java con l'integrazione di servizi cloud.</span><span class="sxs-lookup"><span data-stu-id="e42db-134">The installation is quick and straightforward, making Eclipse ideal for the task of Java development with cloud services integration.</span></span>
