<span data-ttu-id="656c3-101">Azure offre servizi PaaS per semplificare la gestione di tutti i tipi di dati, da dati relazionali altamente strutturati a dati non strutturati.</span><span class="sxs-lookup"><span data-stu-id="656c3-101">Azure provides PaaS services to help you manage all kinds of data, from highly structured relational data to unstructured data.</span></span>

<span data-ttu-id="656c3-102">Di seguito verranno descritti i motivi per cui il database SQL di Azure è un sistema pratico, conveniente e sicuro per ospitare i database relazionali.</span><span class="sxs-lookup"><span data-stu-id="656c3-102">Here you'll learn why Azure SQL Database is a convenient, cost-effective, and secure way to host your relational databases.</span></span>

## <a name="why-choose-azure-sql-database"></a><span data-ttu-id="656c3-103">Perché scegliere il database SQL di Azure?</span><span class="sxs-lookup"><span data-stu-id="656c3-103">Why choose Azure SQL Database?</span></span>

<span data-ttu-id="656c3-104">L'applicazione di logistica per i trasporti richiede stored procedure che eseguano operazioni CRUD (**create**, **read**, **update** e **delete**) di base.</span><span class="sxs-lookup"><span data-stu-id="656c3-104">Your transportation logistics application requires stored procedures that run basic CRUD (**create**, **read**, **update**, and **delete**) operations.</span></span> <span data-ttu-id="656c3-105">Si ha familiarità con SQL Server e altri database relazionali.</span><span class="sxs-lookup"><span data-stu-id="656c3-105">You have experience working with SQL Server and other relational databases.</span></span>

<span data-ttu-id="656c3-106">Si prendono in considerazione due opzioni per il database:</span><span class="sxs-lookup"><span data-stu-id="656c3-106">You consider two choices for your database:</span></span>

1. <span data-ttu-id="656c3-107">Ospitare SQL Server in locale.</span><span class="sxs-lookup"><span data-stu-id="656c3-107">Host SQL Server on premises.</span></span> <span data-ttu-id="656c3-108">Il team IT esegue un data center interno di piccole dimensioni per supportare il reparto finanziario e alcuni altri team.</span><span class="sxs-lookup"><span data-stu-id="656c3-108">Your IT team runs a small in-house data center to support the finance department and a few other teams.</span></span> <span data-ttu-id="656c3-109">È possibile collaborare con il team IT per ospitare una distribuzione di SQL Server in questo data center.</span><span class="sxs-lookup"><span data-stu-id="656c3-109">You can work with IT to host a SQL Server deployment in their data center.</span></span>
1. <span data-ttu-id="656c3-110">Ospitare il database SQL di Azure nel cloud.</span><span class="sxs-lookup"><span data-stu-id="656c3-110">Host Azure SQL Database in the cloud.</span></span> <span data-ttu-id="656c3-111">Il database SQL di Azure si basa su SQL Server e fornisce le funzionalità per i database relazionale necessarie.</span><span class="sxs-lookup"><span data-stu-id="656c3-111">Azure SQL Database is based on SQL Server and provides the relational database functionality you need.</span></span>

<span data-ttu-id="656c3-112">Poiché si è deciso di sviluppare i livelli Web e applicazione per l'app di logistica su Azure,</span><span class="sxs-lookup"><span data-stu-id="656c3-112">You've decided to build the web and application tiers for your logistics app on Azure.</span></span> <span data-ttu-id="656c3-113">è opportuno ospitare anche il database in Azure.</span><span class="sxs-lookup"><span data-stu-id="656c3-113">So it makes sense to also host your database there.</span></span> <span data-ttu-id="656c3-114">Esistono tuttavia altri motivi per cui il database SQL di Azure rappresenta una scelta intelligente e per cui è ancora più semplice rispetto all'uso di macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="656c3-114">But there are some other reasons why Azure SQL Database is a smart choice, and why it's even easier than using virtual machines.</span></span>

* <span data-ttu-id="656c3-115">**Praticità**</span><span class="sxs-lookup"><span data-stu-id="656c3-115">**Convenience**</span></span>

    <span data-ttu-id="656c3-116">Per la configurazione di SQL Server in una macchina virtuale o su hardware fisico, è necessario conoscere i requisiti hardware e software.</span><span class="sxs-lookup"><span data-stu-id="656c3-116">Setting up SQL Server on a vm or on physical hardware requires you to know about hardware and software requirements.</span></span> <span data-ttu-id="656c3-117">È necessario comprendere le più recenti procedure consigliate di sicurezza e gestire periodicamente le patch per il sistema operativo e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="656c3-117">You'll need to understand the latest security best practices and manage operating system and SQL Server patches on a routine basis.</span></span> <span data-ttu-id="656c3-118">È infine necessario occuparsi autonomamente del backup e della conservazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="656c3-118">You also need to manage backup and data retention issues yourself.</span></span>

    <span data-ttu-id="656c3-119">Con il database SQL di Azure, l'hardware, gli aggiornamenti software e le patch del sistema operativo vengono gestiti automaticamente.</span><span class="sxs-lookup"><span data-stu-id="656c3-119">With Azure SQL Database, we manage the hardware, software updates, and OS patches for you.</span></span> <span data-ttu-id="656c3-120">Tutto quello che occorre specificare sono il nome del database e alcune opzioni.</span><span class="sxs-lookup"><span data-stu-id="656c3-120">All you specify is the name of your database and a few options.</span></span> <span data-ttu-id="656c3-121">Il database SQL sarà in esecuzione in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="656c3-121">You'll have a running SQL database in minutes.</span></span>

    <span data-ttu-id="656c3-122">È possibile aprire e chiudere le istanze del database SQL di Azure in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="656c3-122">You can bring up and tear down Azure SQL Database instances at your convenience.</span></span> <span data-ttu-id="656c3-123">Il database SQL di Azure è rapidamente accessibile e semplice da configurare.</span><span class="sxs-lookup"><span data-stu-id="656c3-123">Azure SQL Database comes up fast and is easy to configure.</span></span> <span data-ttu-id="656c3-124">È possibile dedicare meno tempo alla configurazione del software, per concentrarsi sulle funzionalità dell'app.</span><span class="sxs-lookup"><span data-stu-id="656c3-124">You can focus less on configuring software and more on making your app great.</span></span>
* <span data-ttu-id="656c3-125">**Costi**</span><span class="sxs-lookup"><span data-stu-id="656c3-125">**Cost**</span></span>

    <span data-ttu-id="656c3-126">Poiché le operazioni vengono gestite automaticamente, non è necessario acquistare sistemi, alimentarli o provvedere alla relativa manutenzione.</span><span class="sxs-lookup"><span data-stu-id="656c3-126">Because we manage things for you, there are no systems for you to buy, provide power for, or otherwise maintain.</span></span>

    <span data-ttu-id="656c3-127">Il database SQL di Azure offre diverse opzioni di prezzo,</span><span class="sxs-lookup"><span data-stu-id="656c3-127">Azure SQL Database has several pricing options.</span></span> <span data-ttu-id="656c3-128">che consentono di bilanciare prestazioni e costi.</span><span class="sxs-lookup"><span data-stu-id="656c3-128">These pricing options enable you to balance performance versus cost.</span></span> <span data-ttu-id="656c3-129">È possibile iniziare con una spesa mensile molto contenuta.</span><span class="sxs-lookup"><span data-stu-id="656c3-129">You can start for just a few dollars a month.</span></span>
* <span data-ttu-id="656c3-130">**Scalabilità**</span><span class="sxs-lookup"><span data-stu-id="656c3-130">**Scale**</span></span>
 
    <span data-ttu-id="656c3-131">Si scopre che la quantità di dati logistici sui trasporti che è necessario archiviare raddoppia ogni anno.</span><span class="sxs-lookup"><span data-stu-id="656c3-131">You find that the amount of transportation logistics data you must store doubles every year.</span></span> <span data-ttu-id="656c3-132">In caso di esecuzione in locale, quanta capacità in eccesso è necessario pianificare?</span><span class="sxs-lookup"><span data-stu-id="656c3-132">When running on-prem, how much excess capacity should you plan for?</span></span>

    <span data-ttu-id="656c3-133">Con il database SQL di Azure è possibile regolare velocemente le prestazioni e le dimensioni del database quando cambiano le esigenze.</span><span class="sxs-lookup"><span data-stu-id="656c3-133">With Azure SQL Database, you can adjust the performance and size of your database on the fly when your needs change.</span></span>

* <span data-ttu-id="656c3-134">**Sicurezza**</span><span class="sxs-lookup"><span data-stu-id="656c3-134">**Security**</span></span>

    <span data-ttu-id="656c3-135">Il database SQL di Azure viene fornito con un firewall configurato automaticamente per limitare le connessioni da Internet.</span><span class="sxs-lookup"><span data-stu-id="656c3-135">Azure SQL Database comes with a firewall that's automatically configured to restrict connections from the Internet.</span></span>

    <span data-ttu-id="656c3-136">È possibile consentire gli indirizzi che si ritengono attendibili.</span><span class="sxs-lookup"><span data-stu-id="656c3-136">You can "white list" IP addresses you trust.</span></span> <span data-ttu-id="656c3-137">L'elenco degli indirizzi consentiti permette di usare Visual Studio, SQL Server Management Studio o altri strumenti per gestire il database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="656c3-137">Whitelisting lets you use Visual Studio, SQL Server Management Studio, or other tools to manage your Azure SQL Database.</span></span>

## <a name="summary"></a><span data-ttu-id="656c3-138">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="656c3-138">Summary</span></span>

<span data-ttu-id="656c3-139">Con il database SQL di Azure, l'hardware, gli aggiornamenti software e le patch del sistema operativo vengono gestiti automaticamente.</span><span class="sxs-lookup"><span data-stu-id="656c3-139">With Azure SQL Database, we manage the hardware, software updates, and OS patches for you.</span></span> <span data-ttu-id="656c3-140">Sono disponibili varie opzioni di acquisto per ottenere le prestazioni necessarie a costi prevedibili.</span><span class="sxs-lookup"><span data-stu-id="656c3-140">We provide buying options to help you get the performance you need at a predictable cost.</span></span> <span data-ttu-id="656c3-141">Il database SQL di Azure include anche un firewall, pertanto è possibile controllare l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="656c3-141">Azure SQL Database also comes with a firewall so you can control access to your data.</span></span>

<span data-ttu-id="656c3-142">Sebbene non sia necessario essere un amministratore di database per usare il database SQL di Azure, vi sono alcuni concetti che è necessario comprendere prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="656c3-142">Although you don't need to be a DBA to use Azure SQL Database, there are a few concepts you should understand before you start.</span></span> <span data-ttu-id="656c3-143">Questi concetti verranno illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="656c3-143">We'll cover these concepts next.</span></span>