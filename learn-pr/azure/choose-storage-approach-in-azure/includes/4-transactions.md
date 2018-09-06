<span data-ttu-id="a7932-101">Spesso è necessario raggruppare una serie di aggiornamenti ai dati perché una modifica a un tipo di dati comporta una modifica a un altro tipo di dati.</span><span class="sxs-lookup"><span data-stu-id="a7932-101">There are many times when you need to group a series of data updates together because a change to one piece of data needs to result in a change to another piece of data.</span></span> <span data-ttu-id="a7932-102">Le transazioni consentono di raggruppare questi aggiornamenti in modo che, se un evento in una serie di aggiornamenti ha esito negativo, è possibile eseguire il rollback dell'intera serie o annullarla.</span><span class="sxs-lookup"><span data-stu-id="a7932-102">Transactions enable you to group these updates so that if one event in a series of updates fails, then entire series can be rolled back, or undone.</span></span> <span data-ttu-id="a7932-103">Un rivenditore online potrebbe ad esempio usare una transazione per l'invio di un ordine e la verifica del pagamento.</span><span class="sxs-lookup"><span data-stu-id="a7932-103">For example, an online retailer could use a transaction for the placement of an order along with payment verification.</span></span> <span data-ttu-id="a7932-104">Raggruppando gli eventi correlati, è possibile evitare di ridurre i livelli di inventario fino a quando non si riceve un metodo di pagamento approvato.</span><span class="sxs-lookup"><span data-stu-id="a7932-104">The grouping of the related events ensures that you don't reduce your inventory levels until an approved form of payment is received.</span></span>

<span data-ttu-id="a7932-105">In questo modulo si apprenderà che cos'è una transazione e quando è necessario usarla per i dati.</span><span class="sxs-lookup"><span data-stu-id="a7932-105">Here, you'll learn what a transaction is and when they're required for your data.</span></span>

## <a name="what-is-a-transaction"></a><span data-ttu-id="a7932-106">Che cos'è una transazione?</span><span class="sxs-lookup"><span data-stu-id="a7932-106">What is a transaction?</span></span>

<span data-ttu-id="a7932-107">Una transazione è un'unità logica che viene eseguita in modo indipendente per il recupero o gli aggiornamenti dei dati.</span><span class="sxs-lookup"><span data-stu-id="a7932-107">A transaction is a logical unit that is independently executed for data retrieval or updates.</span></span>

<span data-ttu-id="a7932-108">Ecco la domanda da porsi per determinare se è necessario un database transazionale: una modifica a un tipo di dati nel set di dati influirà su un altro tipo di dati?</span><span class="sxs-lookup"><span data-stu-id="a7932-108">Here's the question to ask yourself regarding whether you need a transactional database: Will a change to one piece of data in your dataset impact another?</span></span> <span data-ttu-id="a7932-109">Se la risposta è affermativa, nel servizio di database sarà necessario il supporto per le transazioni.</span><span class="sxs-lookup"><span data-stu-id="a7932-109">If the answer is yes, then you'll need transaction support in your database service.</span></span>

<span data-ttu-id="a7932-110">Le transazioni vengono spesso definite da un set di quattro requisiti, detti anche garanzie ACID.</span><span class="sxs-lookup"><span data-stu-id="a7932-110">Transactions are often defined by a set of four requirements, referred to as ACID guarantees.</span></span> <span data-ttu-id="a7932-111">ACID è l'acronimo di Atomicità, Coerenza, Isolamento e Durabilità:</span><span class="sxs-lookup"><span data-stu-id="a7932-111">ACID stands for Atomicity, Consistency, Isolation, and Durability:</span></span>

* <span data-ttu-id="a7932-112">L'atomicità indica che tutti i dati vengono aggiornati oppure viene eseguito il rollback di tutti i dati allo stato originale.</span><span class="sxs-lookup"><span data-stu-id="a7932-112">Atomicity means all the data is updated, or all the data is rolled back to its original state.</span></span>
* <span data-ttu-id="a7932-113">La coerenza garantisce che se accade qualcosa nel corso della transazione, non venga aggiornata solo una parte dei dati, mentre l'altra no.</span><span class="sxs-lookup"><span data-stu-id="a7932-113">Consistency ensures that if something happens partway through the transaction, that part of the data isn't updated and part of it isn't.</span></span> <span data-ttu-id="a7932-114">La transazione viene applicata in modo coerente a tutti i dati oppure non viene applicata.</span><span class="sxs-lookup"><span data-stu-id="a7932-114">The data is consistent in applying the transaction or not.</span></span>
* <span data-ttu-id="a7932-115">L'isolamento garantisce che una transazione non influisca su un'altra transazione.</span><span class="sxs-lookup"><span data-stu-id="a7932-115">Isolation ensures that one transaction is not impacted by another transaction.</span></span>
* <span data-ttu-id="a7932-116">La durabilità significa che le modifiche apportate nell'ambito della transazione vengono salvate in modo permanente nel sistema.</span><span class="sxs-lookup"><span data-stu-id="a7932-116">Durability means that the changes made due to the transaction are permanently saved in the system.</span></span> <span data-ttu-id="a7932-117">I dati di cui viene eseguito il commit vengono salvati dal sistema in modo che, anche in caso di un errore con conseguente riavvio del sistema, i dati saranno disponibili nello stato corretto.</span><span class="sxs-lookup"><span data-stu-id="a7932-117">Committed data is saved by the system such that, even in the event of a failure and system restart, the data is available in its correct state.</span></span>

<span data-ttu-id="a7932-118">Quando un database ha le garanzie ACID, questi principi vengono applicati alle transazioni e si può avere la certezza che le transazioni verranno applicate in modo coerente.</span><span class="sxs-lookup"><span data-stu-id="a7932-118">When a database has ACID guarantees, it applies these principles to its transactions, and you can be assured that your transactions will be applied in a consistent manner.</span></span>

## <a name="oltp-vs-olap"></a><span data-ttu-id="a7932-119">OLTP e OLAP</span><span class="sxs-lookup"><span data-stu-id="a7932-119">OLTP vs OLAP</span></span>

<span data-ttu-id="a7932-120">I database transazionali vengono spesso denominati sistemi OLTP (Online Transaction Processing, elaborazione di transazioni online).</span><span class="sxs-lookup"><span data-stu-id="a7932-120">Transactional databases are often called OLTP (On-line Transaction Processing) systems.</span></span> <span data-ttu-id="a7932-121">I sistemi OLTP supportano in genere numerosi utenti, hanno tempi di risposta rapidi, gestiscono volumi elevati di dati, offrono disponibilità elevata, ovvero hanno tempi di inattività minimi, e in genere gestiscono le transazioni di piccole dimensioni o relativamente semplici.</span><span class="sxs-lookup"><span data-stu-id="a7932-121">OLTP systems commonly support lots of users, have quick response times, handle large volumes of data, are highly available (meaning they have very minimal downtime), and typically handle small or relatively simple transactions.</span></span>

<span data-ttu-id="a7932-122">Al contrario, i sistemi OLAP (Online Analytical Processing, elaborazione analitica online) supportano in genere un numero inferiore di utenti, hanno tempi di risposta più lunghi, possono offrire una minore disponibilità e in genere gestiscono le transazioni di grandi dimensioni e complesse.</span><span class="sxs-lookup"><span data-stu-id="a7932-122">On the contrary, OLAP (On-line Analytical Processing) systems commonly support fewer users, have longer response times, can be less available, and typically handle large and complex transactions.</span></span>

<span data-ttu-id="a7932-123">I sistemi OLTP e OLAP non vengono usati tanto frequentemente quanto in passato, ma il confronto semplifica la classificazione delle esigenze dell'applicazione, pertanto si tratta di concetti importanti da conoscere.</span><span class="sxs-lookup"><span data-stu-id="a7932-123">OLTP and OLAP aren't used as frequently as they used to be, but the comparison does make it easier to categorize the needs of your application, so it's an important concept to be aware of.</span></span> 

<span data-ttu-id="a7932-124">Ora che è stata acquisita familiarità con le transazioni e con i concetti di OLTP e OLAP, verranno esaminati i set di dati dello scenario di vendita online per determinare la necessità di transazioni.</span><span class="sxs-lookup"><span data-stu-id="a7932-124">Now that we're familiar with transactions, OLTP, and OLAP, let's walk through each of the data sets in the online retail scenario and determine the need for transactions.</span></span>

### <a name="product-catalog-data"></a><span data-ttu-id="a7932-125">Dati del catalogo prodotti</span><span class="sxs-lookup"><span data-stu-id="a7932-125">Product catalog data</span></span>

<span data-ttu-id="a7932-126">I dati del catalogo prodotti devono essere archiviati in un database transazionale.</span><span class="sxs-lookup"><span data-stu-id="a7932-126">Product catalog data should be stored in a transactional database.</span></span> <span data-ttu-id="a7932-127">Quando gli utenti effettuano un ordine e il pagamento viene verificato, l'inventario per l'articolo deve venire aggiornato.</span><span class="sxs-lookup"><span data-stu-id="a7932-127">When users place an order and the payment is verified, the inventory for the item should be updated.</span></span> <span data-ttu-id="a7932-128">Analogamente, se la carta di credito del cliente viene rifiutata, deve essere eseguito il rollback dell'ordine e l'inventario non deve venire aggiornato.</span><span class="sxs-lookup"><span data-stu-id="a7932-128">Likewise, if the customer's credit card is declined, the order should be rolled back, and the inventory should not be updated.</span></span> <span data-ttu-id="a7932-129">Queste relazioni richiedono tutte le transazioni.</span><span class="sxs-lookup"><span data-stu-id="a7932-129">These relationships all require transactions.</span></span>

### <a name="photos-and-videos"></a><span data-ttu-id="a7932-130">Foto e video</span><span class="sxs-lookup"><span data-stu-id="a7932-130">Photos and videos</span></span>

<span data-ttu-id="a7932-131">Le foto e i video in un catalogo prodotti non richiedono il supporto delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="a7932-131">Photos and videos in a product catalog don't require transaction support.</span></span> <span data-ttu-id="a7932-132">L'unico motivo per cui può venire apportata una modifica a una foto o a un video è la presenza di un aggiornamento o l'aggiunta di nuovi file.</span><span class="sxs-lookup"><span data-stu-id="a7932-132">The only reason a change would be made to a photo or video is if an update was made or new files were added.</span></span> <span data-ttu-id="a7932-133">Anche se c'è una relazione tra l'immagine e i dati effettivi del prodotto, la natura non è transazionale.</span><span class="sxs-lookup"><span data-stu-id="a7932-133">Even though there is a relationship between the image and the actual product data, it's not transactional in nature.</span></span>

### <a name="business-data"></a><span data-ttu-id="a7932-134">Dati di business</span><span class="sxs-lookup"><span data-stu-id="a7932-134">Business data</span></span>

<span data-ttu-id="a7932-135">Per i dati di business, poiché tutti i dati sono cronologici e non cambiano, il supporto delle transazioni non è richiesto.</span><span class="sxs-lookup"><span data-stu-id="a7932-135">For the business data, because all of the data is historical and unchanging, transaction support is not required.</span></span> <span data-ttu-id="a7932-136">I business analyst che usano i dati hanno esigenze specifiche, in quanto spesso richiedono l'uso delle aggregazioni nelle query, in modo da poter lavorare con i totali di gruppi di dati più piccoli.</span><span class="sxs-lookup"><span data-stu-id="a7932-136">The business analysts working with the data also have unique needs in that they often require working with aggregates in their queries, so that they can work with the totals of other smaller data points.</span></span>

## <a name="summary"></a><span data-ttu-id="a7932-137">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a7932-137">Summary</span></span>

<span data-ttu-id="a7932-138">Garantire che i dati siano nello stato corretto non è sempre facile.</span><span class="sxs-lookup"><span data-stu-id="a7932-138">Ensuring that your data is in the correct state is not always an easy task.</span></span> <span data-ttu-id="a7932-139">Le transazioni possono risultare utili in quanto permettono di applicare requisiti di integrità dei dati.</span><span class="sxs-lookup"><span data-stu-id="a7932-139">Transactions can help by enforcing data integrity requirements on your data.</span></span> <span data-ttu-id="a7932-140">Se i principi ACID possono essere utili per i dati, è consigliabile scegliere una soluzione di archiviazione che supporti le transazioni.</span><span class="sxs-lookup"><span data-stu-id="a7932-140">If your data would benefit from the principles of ACID, you should choose a storage solution that supports transactions.</span></span>