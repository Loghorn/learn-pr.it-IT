<span data-ttu-id="7c857-101">Dopo aver creato e configurato l'hub eventi, è necessario configurare le applicazioni per inviare e ricevere i flussi di dati di eventi.</span><span class="sxs-lookup"><span data-stu-id="7c857-101">After you have created and configured your event hub, you'll need to configure applications to send and receive event data streams.</span></span>

<span data-ttu-id="7c857-102">Ad esempio, una soluzione di elaborazione dei pagamenti userà un'applicazione mittente di qualche tipo per raccogliere i dati della carta di credito del cliente e un'applicazione ricevitore per verificare la validità della carta di credito.</span><span class="sxs-lookup"><span data-stu-id="7c857-102">For example, a payment processing solution will use some form of sender application to collect customer's credit card data and a receiver application to verify that the credit card is valid.</span></span>

<span data-ttu-id="7c857-103">Anche se la configurazione di un'applicazione Java presenta differenze rispetto a un'applicazione .NET, esistono alcuni principi generali per consentire alle applicazioni di connettersi a un hub eventi e inviare o ricevere correttamente i messaggi.</span><span class="sxs-lookup"><span data-stu-id="7c857-103">Although there are differences in how a Java application is configured, compared to a .NET application, there are general principles for enabling applications to connect to an event hub, and to successfully send or receive messages.</span></span> <span data-ttu-id="7c857-104">Pertanto, anche se il processo di modifica dei file di testo di configurazione Java è diverso dalla preparazione di un'applicazione .NET con Visual Studio, i principi sono gli stessi.</span><span class="sxs-lookup"><span data-stu-id="7c857-104">So, although the process of editing Java configuration text files is different to preparing a .NET application using Visual Studio, the principles are the same.</span></span>

## <a name="what-are-the-minimum-event-hub-application-requirements"></a><span data-ttu-id="7c857-105">Quali sono i requisiti minimi di un'applicazione per Hub eventi?</span><span class="sxs-lookup"><span data-stu-id="7c857-105">What are the minimum Event Hub application requirements?</span></span>

<span data-ttu-id="7c857-106">Per configurare un'applicazione per l'invio di messaggi a un hub eventi, è necessario fornire le informazioni seguenti, in modo da consentire all'applicazione di creare le credenziali di connessione:</span><span class="sxs-lookup"><span data-stu-id="7c857-106">To configure an application to send messages to an event hub, you must provide the following information, so that the application can create connection credentials:</span></span>

- <span data-ttu-id="7c857-107">Nome dello spazio dei nomi di Hub eventi</span><span class="sxs-lookup"><span data-stu-id="7c857-107">Event hub namespace name</span></span>
- <span data-ttu-id="7c857-108">Nome dell'hub eventi</span><span class="sxs-lookup"><span data-stu-id="7c857-108">Event hub name</span></span>
- <span data-ttu-id="7c857-109">Nome dei criteri di accesso condiviso</span><span class="sxs-lookup"><span data-stu-id="7c857-109">Shared access policy name</span></span>
- <span data-ttu-id="7c857-110">Chiave di accesso condiviso primaria</span><span class="sxs-lookup"><span data-stu-id="7c857-110">Primary shared access key</span></span>

<span data-ttu-id="7c857-111">Per configurare un'applicazione per la ricezione di messaggi da un hub eventi, fornire le informazioni seguenti, in modo da consentire all'applicazione di creare le credenziali di connessione:</span><span class="sxs-lookup"><span data-stu-id="7c857-111">To configure an application to receive messages from an event hub, provide the following information, so that the application can create connection credentials:</span></span>

- <span data-ttu-id="7c857-112">Nome dello spazio dei nomi di Hub eventi</span><span class="sxs-lookup"><span data-stu-id="7c857-112">Event hub namespace name</span></span>
- <span data-ttu-id="7c857-113">Nome dell'hub eventi</span><span class="sxs-lookup"><span data-stu-id="7c857-113">Event hub name</span></span>
- <span data-ttu-id="7c857-114">Nome dei criteri di accesso condiviso</span><span class="sxs-lookup"><span data-stu-id="7c857-114">Shared access policy name</span></span>
- <span data-ttu-id="7c857-115">Chiave di accesso condiviso primaria</span><span class="sxs-lookup"><span data-stu-id="7c857-115">Primary shared access key</span></span>
- <span data-ttu-id="7c857-116">Nome dell'account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="7c857-116">Storage account name</span></span>
- <span data-ttu-id="7c857-117">Stringa di connessione dell'account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="7c857-117">Storage account connection string</span></span>
- <span data-ttu-id="7c857-118">Nome del contenitore dell'account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="7c857-118">Storage account container name</span></span>

<span data-ttu-id="7c857-119">Se si dispone di un'applicazione ricevitore che archivia i messaggi in Archiviazione BLOB di Azure, è anche necessario configurare prima un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7c857-119">If you have a receiver application that stores messages in Azure Blob Storage, you'll also need to first configure a storage account.</span></span>

## <a name="the-azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a><span data-ttu-id="7c857-120">Comandi dell'interfaccia della riga di comando di Azure per la creazione di un account di archiviazione standard per utilizzo generico</span><span class="sxs-lookup"><span data-stu-id="7c857-120">The Azure CLI commands for creating a general-purpose standard storage account</span></span>

1. <span data-ttu-id="7c857-121">Creare l'account di archiviazione (per utilizzo generico V2) nel gruppo di risorse e nella posizione del data center di Azure usati per la creazione del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="7c857-121">Create the Storage account (general-purpose V2) in your resource group, and the same Azure datacenter location that you used when creating the resource group.</span></span>

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name> --location <location> --sku Standard_RAGRS --encryption blob
    ```

1. <span data-ttu-id="7c857-122">Per accedere a questa risorsa di archiviazione, è necessaria una chiave di accesso dell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7c857-122">To access this storage, you'll need a storage account access key.</span></span> <span data-ttu-id="7c857-123">Visualizzare le chiavi di accesso associate all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7c857-123">View the access keys that are associated with the storage account.</span></span>

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```

1. <span data-ttu-id="7c857-124">Salvare il valore associato a **key1**.</span><span class="sxs-lookup"><span data-stu-id="7c857-124">Save the value associated with **key1**.</span></span>

1. <span data-ttu-id="7c857-125">Saranno anche necessari i dettagli della connessione per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7c857-125">You will also need the connection details for the storage account.</span></span> <span data-ttu-id="7c857-126">Visualizzare la stringa di connessione per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7c857-126">View the connection string for the storage account.</span></span>

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```

1. <span data-ttu-id="7c857-127">Salvare il valore associato a **connectionString**.</span><span class="sxs-lookup"><span data-stu-id="7c857-127">Save the value associated with the **connectionString**.</span></span>

1. <span data-ttu-id="7c857-128">I messaggi verranno archiviati in un contenitore nell'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7c857-128">Messages will be stored in a container within your storage account.</span></span> <span data-ttu-id="7c857-129">Creare un contenitore nell'account di archiviazione usando `<connection string>` con la stringa di connessione ottenuta nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="7c857-129">Create a container in your storage account, using `<connection string>` with the connection string from the previous step.</span></span>

    ```azurecli
    az storage container create -n <container> --connection-string "<connection string>"
    ```

## <a name="shell-command-for-cloning-an-application-github-repository"></a><span data-ttu-id="7c857-130">Comando della shell per clonare un repository GitHub dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="7c857-130">Shell command for cloning an application GitHub repository</span></span>

<span data-ttu-id="7c857-131">Git è uno strumento di collaborazione che usa un modello di controllo della versione distribuito ed è progettato per la collaborazione su progetti software e di documentazione.</span><span class="sxs-lookup"><span data-stu-id="7c857-131">Git is a collaboration tool that uses a distributed version control model, and is designed for collaborative working on software and documentation projects.</span></span> <span data-ttu-id="7c857-132">I client Git sono disponibili per più piattaforme, tra cui Windows, e la riga di comando di Git è inclusa nella cloud shell Bash di Azure.</span><span class="sxs-lookup"><span data-stu-id="7c857-132">Git clients are available for multiple platforms, including Windows, and the Git command line is included in the Azure Bash cloud shell.</span></span> <span data-ttu-id="7c857-133">GitHub è un servizio di hosting basato sul Web per i repository Git.</span><span class="sxs-lookup"><span data-stu-id="7c857-133">GitHub is a web-based hosting service for Git repositories.</span></span> 

<span data-ttu-id="7c857-134">Se si dispone di un'applicazione ospitata come progetto in GitHub, è possibile creare una copia locale del progetto clonandone il repository tramite il comando **git clone**.</span><span class="sxs-lookup"><span data-stu-id="7c857-134">If you have an application that is hosted as a project in GitHub, you can make a local copy of the project, by cloning its repository using the **git clone** command.</span></span>

1. <span data-ttu-id="7c857-135">Clonare un repository nella home directory.</span><span class="sxs-lookup"><span data-stu-id="7c857-135">Clone a repository to your home directory.</span></span>

    ```azurecli
      git clone https://github.com/<project repository>
    ```

## <a name="shell-commands-for-preparing-an-application"></a><span data-ttu-id="7c857-136">Comandi della shell per la preparazione di un'applicazione</span><span class="sxs-lookup"><span data-stu-id="7c857-136">Shell commands for preparing an application</span></span>

1. <span data-ttu-id="7c857-137">A questo punto, è possibile usare un editor come **nano** per modificare l'applicazione e aggiungere lo spazio dei nomi di Hub eventi, il nome dell'hub eventi, il nome dei criteri di accesso condiviso e la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="7c857-137">You can now use an editor, such as **nano** to edit the application and add your event hub namespace, event hub name, shared access policy name, and primary key.</span></span> <span data-ttu-id="7c857-138">Nano è un semplice editor di testo disponibile per Linux e altri sistemi operativi di tipo Unix. È anche disponibile nella cloud shell Bash di Azure.</span><span class="sxs-lookup"><span data-stu-id="7c857-138">Nano is a simple text editor available for Linux and other Unix-like operating systems; it is also available in the Azure Bash cloud shell.</span></span> <span data-ttu-id="7c857-139">Ad esempio, usare questo comando per aprire un file di applicazione Java nell'editor **nano**.</span><span class="sxs-lookup"><span data-stu-id="7c857-139">For example, use this command to open a Java application file in the **nano** editor.</span></span>

    ```azurecli
    nano <application>.java
    ```

1. <span data-ttu-id="7c857-140">A seconda del modo in cui sono sviluppate le applicazioni, potrebbe essere necessario compilare l'applicazione dopo aver aggiunto le informazioni di configurazione di Hub eventi.</span><span class="sxs-lookup"><span data-stu-id="7c857-140">Depending on how your applications are developed, you may need to compile or build the application after you've added the event hub configuration information.</span></span> <span data-ttu-id="7c857-141">Ad esempio, le applicazioni Java usate nell'unità successiva devono essere compilate tramite uno strumento come Apache **Maven**.</span><span class="sxs-lookup"><span data-stu-id="7c857-141">For example, the Java applications used in the next unit need to be built using a tool such as Apache **Maven**.</span></span> <span data-ttu-id="7c857-142">Maven compila i file Java in file con estensione class o li comprime in file con estensione jar.</span><span class="sxs-lookup"><span data-stu-id="7c857-142">Maven compiles .java files into .class files or packages them into .jar files.</span></span> <span data-ttu-id="7c857-143">Maven è disponibile nella cloud shell Bash. Per compilare l'applicazione Java, usare i comandi **mvn**.</span><span class="sxs-lookup"><span data-stu-id="7c857-143">Maven is available from the Bash cloud shell; to build the Java application, you use **mvn** commands.</span></span> <span data-ttu-id="7c857-144">Usare questo comando mvn per compilare un'applicazione Java.</span><span class="sxs-lookup"><span data-stu-id="7c857-144">Use this mvn command to build a Java  application.</span></span>

    ```azurecli
    mvn clean package -DskipTests
    ```

## <a name="summary"></a><span data-ttu-id="7c857-145">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="7c857-145">Summary</span></span>

<span data-ttu-id="7c857-146">Le applicazioni mittente e ricevitore devono essere configurate con informazioni specifiche sull'ambiente di Hub eventi.</span><span class="sxs-lookup"><span data-stu-id="7c857-146">Sender and receiver applications must be configured with specific information about the event hub environment.</span></span> <span data-ttu-id="7c857-147">Creare un account di archiviazione se l'applicazione ricevitore archivia i messaggi in Archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="7c857-147">You create a storage account if your receiver application stores messages in Blob Storage.</span></span> <span data-ttu-id="7c857-148">Se l'applicazione è ospitata in GitHub, è necessario clonarla nella directory locale.</span><span class="sxs-lookup"><span data-stu-id="7c857-148">If your application is hosted on GitHub, you have to clone it to your local directory.</span></span> <span data-ttu-id="7c857-149">Per aggiungere lo spazio dei nomi all'applicazione, è possibile usare un editor di testo come **nano**.</span><span class="sxs-lookup"><span data-stu-id="7c857-149">Text editors, such as **nano** is used to  add your namespace to the application.</span></span>