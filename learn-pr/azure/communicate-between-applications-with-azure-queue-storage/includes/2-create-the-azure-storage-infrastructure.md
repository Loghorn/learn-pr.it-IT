<span data-ttu-id="7d933-101">La comunicazione diretta tra i componenti di un'applicazione distribuita può essere problematica, in quanto potrebbe subire interruzioni in caso di larghezza di banda ridotta o numero elevato di richieste.</span><span class="sxs-lookup"><span data-stu-id="7d933-101">Direct communication between the components of a distributed application can be problematic because it might be disrupted when network bandwidth is low or when demand is high.</span></span>

<span data-ttu-id="7d933-102">Abbiamo riscontrato questo problema nel nostro sistema: il portale Web chiama un servizio Web, che funziona perfettamente finché il servizio risponde in maniera tempestiva.</span><span class="sxs-lookup"><span data-stu-id="7d933-102">We've seen this in our system: the web portal calls a web service, which works great if the service responds in a timely manner.</span></span> <span data-ttu-id="7d933-103">Dato che il traffico elevato causa problemi, la soluzione è usare una coda per eliminare il collegamento diretto tra le app front-end e il servizio Web del livello intermedio.</span><span class="sxs-lookup"><span data-stu-id="7d933-103">High traffic causes problems and so the plan is to use a queue to eliminate the direct link between the front-end apps and your middle-tier web service.</span></span>

## <a name="what-is-azure-queue-storage"></a><span data-ttu-id="7d933-104">Informazioni su Archiviazione code di Azure</span><span class="sxs-lookup"><span data-stu-id="7d933-104">What is Azure Queue storage?</span></span>

<span data-ttu-id="7d933-105">Archiviazione code di Azure è un servizio di Azure che implementa code basate sul cloud.</span><span class="sxs-lookup"><span data-stu-id="7d933-105">Azure Queue storage is an Azure service that implements cloud-based queues.</span></span> <span data-ttu-id="7d933-106">Ogni coda gestisce un elenco di messaggi.</span><span class="sxs-lookup"><span data-stu-id="7d933-106">Each queue maintains a list of messages.</span></span> <span data-ttu-id="7d933-107">I componenti delle applicazioni accedono a una coda usando un'API REST o una libreria client fornita da Azure.</span><span class="sxs-lookup"><span data-stu-id="7d933-107">Application components access a queue using a REST API or an Azure-supplied client library.</span></span> <span data-ttu-id="7d933-108">Generalmente sono presenti uno o più componenti _mittenti_ e uno o più componenti _destinatari_.</span><span class="sxs-lookup"><span data-stu-id="7d933-108">Typically, you will have one or more _sender_ components and one or more _receiver_ components.</span></span> <span data-ttu-id="7d933-109">I componenti mittenti aggiungono messaggi alla coda.</span><span class="sxs-lookup"><span data-stu-id="7d933-109">Sender components add message to the queue.</span></span> <span data-ttu-id="7d933-110">I componenti destinatario recuperano i messaggi dall'inizio della coda per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="7d933-110">Receiver components retrieve messages from the front of the queue for processing.</span></span> <span data-ttu-id="7d933-111">L'illustrazione seguente mostra più applicazioni mittente che aggiungono messaggi alla coda di Azure e un'applicazione destinataria che li recupera.</span><span class="sxs-lookup"><span data-stu-id="7d933-111">The following illustration shows multiple sender applications adding messages to the Azure Queue and one receiver application retrieving the messages.</span></span>

![Illustrazione di un'architettura generale di Archiviazione code di Azure](../media/2-queue-overview.png)

<span data-ttu-id="7d933-113">Il prezzo è basato sulle dimensioni della coda e sul numero di operazioni.</span><span class="sxs-lookup"><span data-stu-id="7d933-113">Pricing is based on queue size and number of operations.</span></span> <span data-ttu-id="7d933-114">Più grandi sono le dimensioni di una coda di messaggi, maggiore è il costo.</span><span class="sxs-lookup"><span data-stu-id="7d933-114">Larger message queues cost more than smaller queues.</span></span> <span data-ttu-id="7d933-115">Viene inoltre addebitato un costo per ogni operazione, come l'aggiunta e l'eliminazione di un messaggio.</span><span class="sxs-lookup"><span data-stu-id="7d933-115">Charges are also incurred for each operation, such as adding a message and deleting a message.</span></span> <span data-ttu-id="7d933-116">Per informazioni sui prezzi, vedere [Prezzi di Archiviazione code di Azure](https://azure.microsoft.com/pricing/details/storage/queues/).</span><span class="sxs-lookup"><span data-stu-id="7d933-116">For pricing details, see [Azure Queue storage pricing](https://azure.microsoft.com/pricing/details/storage/queues/).</span></span>

## <a name="why-use-queues"></a><span data-ttu-id="7d933-117">Perché usare le code?</span><span class="sxs-lookup"><span data-stu-id="7d933-117">Why use queues?</span></span>

<span data-ttu-id="7d933-118">Una coda aumenta la resilienza archiviando temporaneamente i messaggi in attesa.</span><span class="sxs-lookup"><span data-stu-id="7d933-118">A queue increases resiliency by temporarily storing waiting messages.</span></span> <span data-ttu-id="7d933-119">Nei periodi in cui le richieste sono limitate o normali, le dimensioni della coda rimangono ridotte in quanto il componente di destinazione rimuove i messaggi dalla coda più velocemente di quanto vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="7d933-119">At times of low or normal demand, the size of the queue remains small because the destination component removes messages from the queue faster than they are added.</span></span> <span data-ttu-id="7d933-120">Se invece il numero di richieste è elevato, le dimensioni della coda possono aumentare, ma i messaggi non vanno persi.</span><span class="sxs-lookup"><span data-stu-id="7d933-120">At times of high demand, the queue may increase in size, but messages are not lost.</span></span> <span data-ttu-id="7d933-121">Il componente di destinazione può aggiornare e svuotare la coda quando il numero di richieste torna alla normalità.</span><span class="sxs-lookup"><span data-stu-id="7d933-121">The destination component can catch up and empty the queue as demand returns to normal.</span></span>

<span data-ttu-id="7d933-122">Una singola coda può raggiungere dimensioni massime di **500 TB**, quindi potenzialmente può archiviare _milioni_ di messaggi.</span><span class="sxs-lookup"><span data-stu-id="7d933-122">A single queue can be up to **500 TB** in size, so it can potentially store _millions_ of messages.</span></span> <span data-ttu-id="7d933-123">La velocità effettiva da raggiungere per una singola coda è di 2000 messaggi al secondo, velocità che le consente di gestire situazioni di traffico elevato.</span><span class="sxs-lookup"><span data-stu-id="7d933-123">The target throughput for a single queue is 2000 messages per second, allowing it to handle high-volume scenarios.</span></span>

<span data-ttu-id="7d933-124">Le code consentono una scalabilità automatica e immediata dell'applicazione man mano che cambia il numero di richieste.</span><span class="sxs-lookup"><span data-stu-id="7d933-124">Queues let your application scale automatically and immediately when demand changes.</span></span> <span data-ttu-id="7d933-125">Sono quindi utili per i dati aziendali di importanza critica, la cui perdita causerebbe danni all'organizzazione.</span><span class="sxs-lookup"><span data-stu-id="7d933-125">This makes them useful for critical business data that would be damaging to lose.</span></span> <span data-ttu-id="7d933-126">Azure offre molti altri servizi a scalabilità automatica.</span><span class="sxs-lookup"><span data-stu-id="7d933-126">Azure offers many other services that scale automatically.</span></span> <span data-ttu-id="7d933-127">Ad esempio, la funzionalità **Scalabilità automatica** è disponibile nei set di scalabilità di macchine virtuali, nei servizi cloud, nei piani di servizio app e negli ambienti di servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="7d933-127">For example, the **Autoscale** feature is available on Azure virtual machine scale sets, cloud services, Azure App Service plans, and App Service environments.</span></span> <span data-ttu-id="7d933-128">Grazie a questa funzionalità è possibile definire regole che Azure userà per identificare i periodi di picco delle richieste e aggiungere automaticamente capacità senza coinvolgere un amministratore.</span><span class="sxs-lookup"><span data-stu-id="7d933-128">This lets you define rules that Azure uses to identify periods of high demand and automatically add capacity without involving an administrator.</span></span> <span data-ttu-id="7d933-129">La scalabilità automatica risponde alle richieste rapidamente, ma non immediatamente.</span><span class="sxs-lookup"><span data-stu-id="7d933-129">Autoscaling responds to demand quickly, but not instantaneously.</span></span> <span data-ttu-id="7d933-130">Archiviazione code di Azure, invece, gestisce istantaneamente i picchi di richieste archiviando i messaggi finché non sono disponibili risorse di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="7d933-130">By contrast, Azure Queue storage instantaneously handles high demand by storing messages until processing resources are available.</span></span>

## <a name="what-is-a-message"></a><span data-ttu-id="7d933-131">Che cos'è un messaggio?</span><span class="sxs-lookup"><span data-stu-id="7d933-131">What is a message?</span></span>

<span data-ttu-id="7d933-132">Un messaggio in una coda è una matrice di byte con dimensioni massime di 64 KB.</span><span class="sxs-lookup"><span data-stu-id="7d933-132">A message in a queue is a byte array of up to 64 KB.</span></span> <span data-ttu-id="7d933-133">Il contenuto dei messaggi non viene interpretato in alcun modo da nessun componente di Azure.</span><span class="sxs-lookup"><span data-stu-id="7d933-133">Message contents are not interpreted at all by any Azure component.</span></span>

<span data-ttu-id="7d933-134">Se si vuole creare un messaggio strutturato, è possibile formattarne il contenuto tramite XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="7d933-134">If you want to create a structured message, you could format the message content using XML or JSON.</span></span> <span data-ttu-id="7d933-135">Il codice è responsabile della generazione e dell'interpretazione del formato personalizzato.</span><span class="sxs-lookup"><span data-stu-id="7d933-135">Your code is responsible for generating and interpreting your custom format.</span></span> <span data-ttu-id="7d933-136">Ad esempio, si potrebbe creare un messaggio JSON personalizzato con un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7d933-136">For example, you could make a custom JSON message that looks like the following:</span></span>

```json
{
    "Message": {
        "To": "news@contoso.com",
        "From": "writer@contoso.com",
        "Subject": "Support request",
        "Body": "Send me a photographer!"
    }
}
```

## <a name="creating-a-storage-account"></a><span data-ttu-id="7d933-137">Creazione di un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="7d933-137">Creating a storage account</span></span>

<span data-ttu-id="7d933-138">Una coda deve far parte di un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7d933-138">A queue must be part of a storage account.</span></span> <span data-ttu-id="7d933-139">È possibile creare un account di archiviazione usando l'interfaccia della riga di comando di Azure (o PowerShell) oppure il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="7d933-139">You can create a storage account using the Azure CLI (or PowerShell), or Azure portal.</span></span> <span data-ttu-id="7d933-140">Il portale è il mezzo più semplice perché è completamene guidato e richiede esplicitamente l'immissione di ogni singola informazione.</span><span class="sxs-lookup"><span data-stu-id="7d933-140">The portal is easiest because it's all guided and prompts you for each piece of information.</span></span> 

<span data-ttu-id="7d933-141">Lo screenshot seguente mostra la posizione della categoria degli account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7d933-141">The following screenshot shows the location of the Storage accounts category.</span></span>

![Screenshot del pannello Tutti i servizi con la categoria Account di archiviazione evidenziata.](../media/2-create-storage-account-1.png)

<span data-ttu-id="7d933-143">Per creare l'account sono disponibili diverse opzioni, ma nella maggior parte dei casi è possibile usare la selezione predefinita.</span><span class="sxs-lookup"><span data-stu-id="7d933-143">There are several options you can supply when you create the account, most of which you can use the default selection.</span></span> <span data-ttu-id="7d933-144">Queste opzioni sono state illustrate in un modulo precedente, ma è possibile passare il puntatore sopra il suggerimento `(i)` associato a ciascuna opzione per visualizzarne una breve descrizione.</span><span class="sxs-lookup"><span data-stu-id="7d933-144">We covered these options in a previous module, but you can hover over the `(i)` tip associated with each option to get a reminder of what it does.</span></span> <span data-ttu-id="7d933-145">Ecco un esempio di inserimento delle informazioni nel pannello del portale.</span><span class="sxs-lookup"><span data-stu-id="7d933-145">Here's an example of filling out the portal blade.</span></span>

<span data-ttu-id="7d933-146">Lo screenshot seguente mostra il pannello Crea account di archiviazione e le informazioni necessarie per creare un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7d933-146">The following screenshot displays the Create storage account blade and the information required to create a storage account.</span></span>

![Screenshot del pannello Crea account di archiviazione con le opzioni da specificare per creare un account di archiviazione.](../media/2-create-storage-account-2.png)

### <a name="settings-for-queues"></a><span data-ttu-id="7d933-148">Impostazioni per le code</span><span class="sxs-lookup"><span data-stu-id="7d933-148">Settings for queues</span></span>
<span data-ttu-id="7d933-149">Quando si crea un account di archiviazione che conterrà code, è opportuno considerare le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7d933-149">When you create a storage account that will contain queues, you should consider the following settings:</span></span>

- <span data-ttu-id="7d933-150">Le code sono disponibili solo all'interno degli account di archiviazione per utilizzo generico di Azure (v1 o v2).</span><span class="sxs-lookup"><span data-stu-id="7d933-150">Queues are only available as part of Azure general-purpose storage accounts (v1 or v2).</span></span> <span data-ttu-id="7d933-151">Non è possibile aggiungerle ad account di archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="7d933-151">You cannot add them to Blob storage accounts.</span></span>
- <span data-ttu-id="7d933-152">L'impostazione **Livello di accesso** disponibile per gli account di archiviazione v2 si applica solo all'archiviazione BLOB e non ha alcun effetto sulle code.</span><span class="sxs-lookup"><span data-stu-id="7d933-152">The **Access tier** setting which is shown for StorageV2 accounts applies only to Blob storage and does not affect queues.</span></span>
- <span data-ttu-id="7d933-153">È consigliabile scegliere una posizione vicina ai componenti di origine o a quelli di destinazione oppure (preferibilmente) a entrambi.</span><span class="sxs-lookup"><span data-stu-id="7d933-153">You should choose a location that is close to either the source components or destination components or (preferably) both.</span></span>
- <span data-ttu-id="7d933-154">I dati vengono sempre replicati su più server come misura di protezione da errori del disco e altri problemi a livello di hardware.</span><span class="sxs-lookup"><span data-stu-id="7d933-154">Data is always replicated to multiple servers to guard against disk failures and other hardware problems.</span></span> <span data-ttu-id="7d933-155">È possibile scegliere fra due strategie di replica: l'**archiviazione con ridondanza locale** ha costi contenuti ma è vulnerabile agli eventi critici che colpiscono un intero data center, mentre l'**archiviazione con ridondanza geografica** esegue la replica dei dati in altri data center di Azure.</span><span class="sxs-lookup"><span data-stu-id="7d933-155">You have a choice of replication strategies: **Locally Redundant Storage (LRS)** is low-cost but vulnerable to disasters that affect an entire data center while **Geo-Redundant Storage (GRS)** replicates data to other Azure data centers.</span></span> <span data-ttu-id="7d933-156">Scegliere la strategia di replica più adatta alle proprie esigenze di ridondanza.</span><span class="sxs-lookup"><span data-stu-id="7d933-156">Choose the replication strategy that meets your redundancy needs.</span></span>
- <span data-ttu-id="7d933-157">Il livello di prestazioni determina la modalità di archiviazione dei messaggi: il livello **Standard** usa unità magnetiche, mentre il livello **Premium** usa unità SSD.</span><span class="sxs-lookup"><span data-stu-id="7d933-157">The performance tier determines how your message are stored: **Standard** uses magnetic drives while **Premium** uses solid-state drives.</span></span> <span data-ttu-id="7d933-158">Scegliere il livello Standard se si prevede che i picchi di richieste siano di breve durata.</span><span class="sxs-lookup"><span data-stu-id="7d933-158">Choose Standard if you expect peaks in demand to be short.</span></span> <span data-ttu-id="7d933-159">Scegliere il livello Premium se la lunghezza della coda talvolta aumenta notevolmente e occorre ridurre al minimo il tempo di accesso ai messaggi.</span><span class="sxs-lookup"><span data-stu-id="7d933-159">Consider Premium if queue length sometimes becomes long and you need to minimize the time to access messages.</span></span>
- <span data-ttu-id="7d933-160">Richiedere il trasferimento sicuro se esiste la possibilità che attraverso la coda passino informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="7d933-160">Require secure transfer if sensitive information may pass through the queue.</span></span> <span data-ttu-id="7d933-161">Questa impostazione assicura che tutte le connessioni alla coda vengano crittografate tramite SSL (Secure Sockets Layer).</span><span class="sxs-lookup"><span data-stu-id="7d933-161">This setting ensures that all connections to the queue are encrypted using Secure Sockets Layer (SSL).</span></span>