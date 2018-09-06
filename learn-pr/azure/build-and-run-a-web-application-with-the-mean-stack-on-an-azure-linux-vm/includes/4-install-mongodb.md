<span data-ttu-id="d5607-101">Un archivio dati è necessario nella maggior parte delle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="d5607-101">Most applications need a data store.</span></span> <span data-ttu-id="d5607-102">MongoDB, che rappresenta la lettera "M" nello stack MEAN, è una delle soluzioni di archivio dati NoSQL più note ed è gratuita e open source.</span><span class="sxs-lookup"><span data-stu-id="d5607-102">MongoDB, the "M" in the MEAN stack, is one of the most popular NoSQL data store solutions, and it is free and open source.</span></span> <span data-ttu-id="d5607-103">Un archivio dati NoSQL non richiede che i dati siano strutturati in un modo predefinito, come accade invece con un database relazionale come SQL Server o MySQL.</span><span class="sxs-lookup"><span data-stu-id="d5607-103">A NoSQL data store does not require data be structured in a pre-defined way as it would with a relational database like SQL Server or MySQL.</span></span>

<span data-ttu-id="d5607-104">MongoDB archivia i dati in documenti in un formato simile a JSON che non richiedono strutture dei dati rigide.</span><span class="sxs-lookup"><span data-stu-id="d5607-104">MongoDB stores its data in JSON-like documents which don't require rigid data structures.</span></span> <span data-ttu-id="d5607-105">Per interagire con MongoDB si usano quindi query e comandi inviati in formato JSON (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="d5607-105">We then interact with MongoDB using queries and commands sent as JavaScript Object Notation (JSON).</span></span>

## <a name="what-must-be-installed"></a><span data-ttu-id="d5607-106">Che cosa è necessario installare?</span><span class="sxs-lookup"><span data-stu-id="d5607-106">What must be Installed?</span></span>

<span data-ttu-id="d5607-107">Sono disponibili due versioni di MongoDB:</span><span class="sxs-lookup"><span data-stu-id="d5607-107">There are two versions of MongoDB available:</span></span>

- <span data-ttu-id="d5607-108">MongoDB Community Server</span><span class="sxs-lookup"><span data-stu-id="d5607-108">MongoDB Community Server</span></span>
- <span data-ttu-id="d5607-109">MongoDB Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="d5607-109">MongoDB Enterprise Server.</span></span>

<span data-ttu-id="d5607-110">In questo modulo useremo MongoDB Community Server, disponibile nella versione 3.6 al momento della stesura di questo articolo, per l'archivio dati della nostra applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="d5607-110">In this module we are going to use MongoDB Community Server, version 3.6 at the time of this writing, for our web application's data store.</span></span>

## <a name="how-to-install-mongodb"></a><span data-ttu-id="d5607-111">Come installare MongoDB</span><span class="sxs-lookup"><span data-stu-id="d5607-111">How to install MongoDB</span></span>

<span data-ttu-id="d5607-112">MongoDB può essere installato su Linux, macOS e Windows.</span><span class="sxs-lookup"><span data-stu-id="d5607-112">MongoDB can be installed on Linux, macOS, and Windows.</span></span> <span data-ttu-id="d5607-113">Per questo modulo verrà installato nella macchina virtuale Ubuntu Linux, ma lo strumento di gestione pacchetti consigliato varia in base al sistema operativo e alla distribuzione.</span><span class="sxs-lookup"><span data-stu-id="d5607-113">We will be installing it on our Ubuntu Linux VM for this module, but the recommended package manager differs by OS and distribution.</span></span> <span data-ttu-id="d5607-114">Il processo di installazione per tutti i sistemi operativi è ben documentato nella [documentazione sull'installazione di MongoDB](https://docs.mongodb.com/manual/administration/install-community/).</span><span class="sxs-lookup"><span data-stu-id="d5607-114">The installation process for all operating systems is well documented in the [MongoDB install documentation](https://docs.mongodb.com/manual/administration/install-community/).</span></span>

<span data-ttu-id="d5607-115">Per eseguire l'installazione nella macchina virtuale Ubuntu, useremo lo strumento di gestione pacchetti **apt-get**.</span><span class="sxs-lookup"><span data-stu-id="d5607-115">To install on our Ubuntu VM, we will use the **apt-get** package manager.</span></span>