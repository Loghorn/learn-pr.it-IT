<span data-ttu-id="566e9-101">Ora che gli ambienti informatici passano da data center controllati dal cliente a data center nel cloud, anche la responsabilità della sicurezza cambia.</span><span class="sxs-lookup"><span data-stu-id="566e9-101">As computing environments move from customer-controlled data centers to cloud data centers, the responsibility of security also shifts.</span></span> <span data-ttu-id="566e9-102">La sicurezza ora è un compito condiviso tra i provider di servizi cloud e i clienti.</span><span class="sxs-lookup"><span data-stu-id="566e9-102">Security is now a concern shared both by cloud providers and customers.</span></span> <span data-ttu-id="566e9-103">Per ogni applicazione e soluzione, è importante comprendere quali sono le responsabilità del cliente e quelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="566e9-103">For every application and solution, it's important to understand what's your responsibility and what's Azure's responsibility.</span></span>

#### <a name="understand-security-threats"></a><span data-ttu-id="566e9-104">Informazioni sulla minacce alla sicurezza</span><span class="sxs-lookup"><span data-stu-id="566e9-104">Understand security threats</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWkotg]

#### <a name="azure-security-you-versus-the-cloud"></a><span data-ttu-id="566e9-105">Sicurezza di Azure: il cliente e il cloud</span><span class="sxs-lookup"><span data-stu-id="566e9-105">Azure security: you versus the cloud</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvj]

## <a name="share-security-responsibility-with-azure"></a><span data-ttu-id="566e9-106">Condividere la responsabilità della sicurezza con Azure</span><span class="sxs-lookup"><span data-stu-id="566e9-106">Share security responsibility with Azure</span></span>

<span data-ttu-id="566e9-107">Il primo passaggio da realizzare è quello dai data center locali agli ambienti IaaS (Infrastructure-as-a-Service).</span><span class="sxs-lookup"><span data-stu-id="566e9-107">The first shift you’ll make is from on-premises data centers to infrastructure as a service (IaaS).</span></span> <span data-ttu-id="566e9-108">In un ambiente IaaS il cliente usa il servizio di livello più basso e chiede ad Azure di creare macchine virtuali e reti virtuali.</span><span class="sxs-lookup"><span data-stu-id="566e9-108">With IaaS, you are leveraging the lowest-level service and asking Azure to create virtual machines (VMs) and virtual networks.</span></span> <span data-ttu-id="566e9-109">A questo livello, è comunque responsabilità dell'utente applicare patch e proteggere i sistemi operativi e il software, nonché configurare la rete in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="566e9-109">At this level, it's still your responsibility to patch and secure your operating systems and software, as well as configure your network to be secure.</span></span> <span data-ttu-id="566e9-110">In Contoso Shipping si sfruttano i vantaggi dei servizi IaaS quando si iniziano a usare macchine virtuali di Azure anziché i propri server fisici locali.</span><span class="sxs-lookup"><span data-stu-id="566e9-110">At Contoso Shipping, you are taking advantage of IaaS when you start using Azure VMs instead of your on-premises physical servers.</span></span> <span data-ttu-id="566e9-111">Oltre ai vantaggi operativi, si gode del vantaggio in termini di sicurezza di aver esternalizzato il compito di proteggere le parti fisiche della rete.</span><span class="sxs-lookup"><span data-stu-id="566e9-111">In addition to the operational advantages, you receive the security advantage of having outsourced concern over protecting the physical parts of the network.</span></span>

<span data-ttu-id="566e9-112">Il passaggio a una soluzione PaaS (Platform-as-a-Service) esternalizza molti problemi di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="566e9-112">Moving to platform as a service (PaaS) outsources a lot of security concerns.</span></span> <span data-ttu-id="566e9-113">A questo livello, Azure si occupa del sistema operativo e della maggior parte dei software di base, come i sistemi di gestione dei database.</span><span class="sxs-lookup"><span data-stu-id="566e9-113">At this level, Azure is taking care of the operating system and of most foundational software like database management systems.</span></span> <span data-ttu-id="566e9-114">Tutto è aggiornato con le ultime patch di sicurezza e può essere integrato con Azure Active Directory per il controllo degli accessi.</span><span class="sxs-lookup"><span data-stu-id="566e9-114">Everything is updated with the latest security patches and can be integrated with Azure Active Directory for access controls.</span></span> <span data-ttu-id="566e9-115">Gli ambienti PaaS presentano anche molti vantaggi operativi.</span><span class="sxs-lookup"><span data-stu-id="566e9-115">PaaS also comes with a lot of operational advantages.</span></span> <span data-ttu-id="566e9-116">Invece di costruire intere infrastrutture e subnet per i propri ambienti manualmente, è possibile effettuare selezioni all'interno del portale di Azure o eseguire script automatici per attivare e disattivare complessi sistemi protetti, nonché ridimensionarli a seconda delle necessità.</span><span class="sxs-lookup"><span data-stu-id="566e9-116">Rather than building whole infrastructures and subnets for your environments by hand, you can "point and click" within the Azure portal or run automated scripts to bring complex, secured systems up and down, and scale them as needed.</span></span> <span data-ttu-id="566e9-117">Contoso Shipping usa Hub eventi di Azure per inserire i dati di telemetria trasmessi da droni e veicoli, oltre a un'app Web con un back-end Azure Cosmos DB con le app per dispositivi mobili. Questi sono tutti esempi di PaaS.</span><span class="sxs-lookup"><span data-stu-id="566e9-117">Contoso Shipping uses Azure Event Hubs for ingesting telemetry data from drones and trucks &mdash; as well as a web app with an Azure Cosmos DB back end with their mobile apps &mdash; which are all examples of PaaS.</span></span>

<span data-ttu-id="566e9-118">Con una soluzione SaaS (Software-as-a-Service), si esternalizza quasi tutto.</span><span class="sxs-lookup"><span data-stu-id="566e9-118">With software as a service (SaaS), you outsource almost everything.</span></span> <span data-ttu-id="566e9-119">SaaS è il software che viene eseguito con un'infrastruttura Internet.</span><span class="sxs-lookup"><span data-stu-id="566e9-119">SaaS is software that runs with an internet infrastructure.</span></span> <span data-ttu-id="566e9-120">Il codice viene controllato dal fornitore, ma è configurato per poter essere usato dal cliente.</span><span class="sxs-lookup"><span data-stu-id="566e9-120">The code is controlled by the vendor but configured to be used by the customer.</span></span> <span data-ttu-id="566e9-121">Come molte aziende, Contoso Shipping usa Office 365, che è un perfetto esempio di SaaS.</span><span class="sxs-lookup"><span data-stu-id="566e9-121">Like so many companies, Contoso Shipping uses Office 365, which is a great example of SaaS!</span></span>

![shared_responsibility.png](../media/shared_responsibilities.png)

## <a name="a-layered-approach-to-security"></a><span data-ttu-id="566e9-123">Approccio alla sicurezza strutturato su più livelli</span><span class="sxs-lookup"><span data-stu-id="566e9-123">A layered approach to security</span></span>

<span data-ttu-id="566e9-124">*Difesa avanzata* è una strategia che impiega una serie di meccanismi per rallentare l'avanzamento di un attacco volto ad accedere a informazioni senza autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="566e9-124">*Defense in depth* is a strategy that employs a series of mechanisms to slow the advance of an attack aimed at acquiring unauthorized access to information.</span></span> <span data-ttu-id="566e9-125">Ogni livello offre funzioni di protezione, quindi, se un livello viene violato, vi è già un livello successivo pronto a impedire un'ulteriore esposizione.</span><span class="sxs-lookup"><span data-stu-id="566e9-125">Each layer provides protection so that if one layer is breached, a subsequent layer is already in place to prevent further exposure.</span></span> <span data-ttu-id="566e9-126">Microsoft applica un approccio alla sicurezza strutturato su più livelli, sia nei data center fisici che nei servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="566e9-126">Microsoft applies a layered approach to security, both in physical data centers and across Azure services.</span></span> <span data-ttu-id="566e9-127">Lo scopo della difesa avanzata è proteggere le informazioni e prevenirne il furto da parte di soggetti non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="566e9-127">The objective of defense in depth is to protect and prevent information from being stolen by individuals who are not authorized to access it.</span></span>

<span data-ttu-id="566e9-128">La difesa avanzata può essere visualizzata come una serie di anelli concentrici, al cui centro si trovano i dati da proteggere.</span><span class="sxs-lookup"><span data-stu-id="566e9-128">Defense in depth can be visualized as a set of concentric rings, with the data to be secured at the center.</span></span> <span data-ttu-id="566e9-129">Ogni anello aggiunge un livello di protezione attorno ai dati.</span><span class="sxs-lookup"><span data-stu-id="566e9-129">Each ring adds an additional layer of security around the data.</span></span> <span data-ttu-id="566e9-130">Questa strategia consente di evitare la dipendenza da un singolo livello di protezione e opera rallentando l'attacco e generando avvisi con dati di telemetria, in base ai quali è possibile intervenire automaticamente o manualmente.</span><span class="sxs-lookup"><span data-stu-id="566e9-130">This approach removes reliance on any single layer of protection and acts to slow down an attack and provide alert telemetry that can be acted upon, either automatically or manually.</span></span> <span data-ttu-id="566e9-131">Esaminiamo ciascuno di questi livelli.</span><span class="sxs-lookup"><span data-stu-id="566e9-131">Let's take a look at each of the layers.</span></span>

![Difesa avanzata](../media/defense_in_depth_layers_small.PNG)

:::row:::
  :::column:::
    <span data-ttu-id="566e9-133">![Immagine che rappresenta i dati](../media/2-data.png)</span><span class="sxs-lookup"><span data-stu-id="566e9-133">![Image representing data](../media/2-data.png)</span></span>
  :::column-end:::
    <span data-ttu-id="566e9-134">:::column span="3"::: **Dati**</span><span class="sxs-lookup"><span data-stu-id="566e9-134">:::column span="3"::: **Data**</span></span>

<span data-ttu-id="566e9-135">Nella quasi totalità dei casi, gli utenti malintenzionati cercano:</span><span class="sxs-lookup"><span data-stu-id="566e9-135">In almost all cases, attackers are after data:</span></span>

- <span data-ttu-id="566e9-136">Dati archiviati in un database</span><span class="sxs-lookup"><span data-stu-id="566e9-136">Data stored in a database</span></span>
- <span data-ttu-id="566e9-137">Dati archiviati su disco all'interno di macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="566e9-137">Data stored on disk inside virtual machines</span></span>
- <span data-ttu-id="566e9-138">Dati archiviati in applicazioni SaaS, come Office 365</span><span class="sxs-lookup"><span data-stu-id="566e9-138">Data stored on a SaaS application such as Office 365</span></span>
- <span data-ttu-id="566e9-139">Dati archiviati nel cloud</span><span class="sxs-lookup"><span data-stu-id="566e9-139">Data stored in cloud storage</span></span>

<span data-ttu-id="566e9-140">È responsabilità di chi archivia i dati e di chi ne controlla l'accesso assicurarsi che siano adeguatamente protetti.</span><span class="sxs-lookup"><span data-stu-id="566e9-140">It's the responsibility of those storing and controlling access to data to ensure that it's properly secured.</span></span> <span data-ttu-id="566e9-141">Esistono spesso requisiti normativi che determinano i controlli e i processi da predisporre per garantire la riservatezza, l'integrità e la disponibilità dei dati.</span><span class="sxs-lookup"><span data-stu-id="566e9-141">Often, there are regulatory requirements that dictate the controls and processes that must be in place to ensure the confidentiality, integrity, and availability of the data.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="566e9-142">![Immagine di un file in rete](../media/2-application.png)</span><span class="sxs-lookup"><span data-stu-id="566e9-142">![Image of a file on the network](../media/2-application.png)</span></span>
  :::column-end:::
    <span data-ttu-id="566e9-143">:::column span="3"::: **Applicazione**</span><span class="sxs-lookup"><span data-stu-id="566e9-143">:::column span="3"::: **Application**</span></span>

- <span data-ttu-id="566e9-144">Verificare che le applicazioni siano protette e prive di vulnerabilità.</span><span class="sxs-lookup"><span data-stu-id="566e9-144">Ensure applications are secure and free of vulnerabilities.</span></span>
- <span data-ttu-id="566e9-145">Archiviare i segreti sensibili delle applicazioni in un supporto di archiviazione sicuro.</span><span class="sxs-lookup"><span data-stu-id="566e9-145">Store sensitive application secrets in a secure storage medium.</span></span>
- <span data-ttu-id="566e9-146">Prevedere la sicurezza come requisito di progettazione per lo sviluppo di tutte le applicazioni.</span><span class="sxs-lookup"><span data-stu-id="566e9-146">Make security a design requirement for all application development.</span></span>

<span data-ttu-id="566e9-147">L'integrazione della sicurezza nel ciclo di sviluppo delle applicazioni consentirà di ridurre il numero di vulnerabilità introdotte nel codice.</span><span class="sxs-lookup"><span data-stu-id="566e9-147">Integrating security into the application development life cycle will help reduce the number of vulnerabilities introduced in code.</span></span> <span data-ttu-id="566e9-148">Tutti i team di sviluppo sono invitati a garantire la sicurezza predefinita delle proprie applicazioni e rendere i requisiti di sicurezza imprescindibili.</span><span class="sxs-lookup"><span data-stu-id="566e9-148">We encourage all development teams to ensure their applications are secure by default, and that they're making security requirements non-negotiable.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="566e9-149">![Terminale che rappresenta il calcolo](../media/2-compute.png)</span><span class="sxs-lookup"><span data-stu-id="566e9-149">![A terminal representing compute](../media/2-compute.png)</span></span>
  :::column-end:::
    <span data-ttu-id="566e9-150">:::column span="3"::: **Calcolo**</span><span class="sxs-lookup"><span data-stu-id="566e9-150">:::column span="3"::: **Compute**</span></span>

- <span data-ttu-id="566e9-151">Proteggere l'accesso alle macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="566e9-151">Secure access to virtual machines.</span></span>
- <span data-ttu-id="566e9-152">Implementare la protezione degli endpoint e mantenere aggiornati i sistemi con le patch di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="566e9-152">Implement endpoint protection and keep systems patched and current.</span></span>

<span data-ttu-id="566e9-153">Malware, sistemi privi di patch e sistemi non protetti in modo corretto espongono l'ambiente agli attacchi.</span><span class="sxs-lookup"><span data-stu-id="566e9-153">Malware, unpatched systems, and improperly secured systems open your environment to attacks.</span></span> <span data-ttu-id="566e9-154">Lo scopo a questo livello è assicurarsi che le risorse di calcolo siano sicure e che siano stati predisposti controlli appropriati in grado di ridurre al minimo i problemi di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="566e9-154">The focus in this layer is on making sure your compute resources are secure, and that you have the proper controls in place to minimize security issues.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="566e9-155">![Tre sistemi connessi che rappresentano la rete](../media/2-networking.png)</span><span class="sxs-lookup"><span data-stu-id="566e9-155">![Three connected systems representing networking](../media/2-networking.png)</span></span>
  :::column-end:::
    <span data-ttu-id="566e9-156">:::column span="3"::: **Rete**</span><span class="sxs-lookup"><span data-stu-id="566e9-156">:::column span="3"::: **Networking**</span></span>

- <span data-ttu-id="566e9-157">Limitare la comunicazione tra risorse.</span><span class="sxs-lookup"><span data-stu-id="566e9-157">Limit communication between resources.</span></span>
- <span data-ttu-id="566e9-158">Negare per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="566e9-158">Deny by default.</span></span>
- <span data-ttu-id="566e9-159">Limitare l'accesso Internet in ingresso e in uscita, dove appropriato.</span><span class="sxs-lookup"><span data-stu-id="566e9-159">Restrict inbound internet access and limit outbound, where appropriate.</span></span>
- <span data-ttu-id="566e9-160">Implementare una connettività sicura verso le reti locali.</span><span class="sxs-lookup"><span data-stu-id="566e9-160">Implement secure connectivity to on-premises networks.</span></span>

<span data-ttu-id="566e9-161">A questo livello, lo scopo è limitare la connettività di rete di tutte le risorse, consentendo unicamente quella necessaria.</span><span class="sxs-lookup"><span data-stu-id="566e9-161">At this layer, the focus is on limiting the network connectivity across all your resources to allow only what is required.</span></span> <span data-ttu-id="566e9-162">Limitando la comunicazione, si riduce il rischio di spostamento laterale attraverso la rete.</span><span class="sxs-lookup"><span data-stu-id="566e9-162">By limiting this communication, you reduce the risk of lateral movement throughout your network.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="566e9-163">![Barriera fisica che rappresenta il perimetro di rete](../media/2-perimeter.png)</span><span class="sxs-lookup"><span data-stu-id="566e9-163">![A physical barrier representing the network perimeter](../media/2-perimeter.png)</span></span>
  :::column-end:::
    <span data-ttu-id="566e9-164">:::column span="3"::: **Perimetro**</span><span class="sxs-lookup"><span data-stu-id="566e9-164">:::column span="3"::: **Perimeter**</span></span>

- <span data-ttu-id="566e9-165">Implementare Protezione DDoS (attacco Distributed Denial of Service) per filtrare gli attacchi su larga scala prima che possano bloccare il servizio per gli utenti finali.</span><span class="sxs-lookup"><span data-stu-id="566e9-165">Use distributed denial of service (DDoS) protection to filter large-scale attacks before they can cause a denial of service for end users.</span></span>
- <span data-ttu-id="566e9-166">Usare firewall perimetrali per identificare e segnalare gli attacchi dannosi alla rete.</span><span class="sxs-lookup"><span data-stu-id="566e9-166">Use perimeter firewalls to identify and alert on malicious attacks against your network.</span></span>

<span data-ttu-id="566e9-167">A livello di perimetro della rete, lo scopo è impedire gli attacchi di rete diretti alle risorse.</span><span class="sxs-lookup"><span data-stu-id="566e9-167">At the network perimeter, it's about protecting from network-based attacks against your resources.</span></span> <span data-ttu-id="566e9-168">Identificare gli attacchi, neutralizzarne l'impatto e segnalarli quando si verificano sono modi importanti per proteggere la rete.</span><span class="sxs-lookup"><span data-stu-id="566e9-168">Identifying these attacks, eliminating their impact, and alerting you when they happen are important ways to keep your network secure.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="566e9-169">![Badge che rappresenta un accesso sicuro](../media/2-policies-and-access.png)</span><span class="sxs-lookup"><span data-stu-id="566e9-169">![A badge representing a secure access](../media/2-policies-and-access.png)</span></span>
  :::column-end:::
    <span data-ttu-id="566e9-170">:::column span="3"::: **Criteri e accesso**</span><span class="sxs-lookup"><span data-stu-id="566e9-170">:::column span="3"::: **Policies and access**</span></span>

- <span data-ttu-id="566e9-171">Controllare l'accesso all'infrastruttura e implementare il controllo delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="566e9-171">Control access to infrastructure and change control.</span></span>
- <span data-ttu-id="566e9-172">Usare l'autenticazione a più fattori e Single Sign-On.</span><span class="sxs-lookup"><span data-stu-id="566e9-172">Use single sign-on and multi-factor authentication.</span></span>
- <span data-ttu-id="566e9-173">Controllare eventi e modifiche.</span><span class="sxs-lookup"><span data-stu-id="566e9-173">Audit events and changes.</span></span>

<span data-ttu-id="566e9-174">Al livello criteri e accesso lo scopo è verificare che le identità siano protette, che l'accesso venga concesso solo se necessario e che le modifiche vengano registrate.</span><span class="sxs-lookup"><span data-stu-id="566e9-174">The policy and access layer is all about ensuring identities are secure, access granted is only what is needed, and changes are logged.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="566e9-175">![Telecamera di sicurezza che rappresenta la sicurezza fisica](../media/2-physical-security.png)</span><span class="sxs-lookup"><span data-stu-id="566e9-175">![A security camera representing physical security](../media/2-physical-security.png)</span></span>
  :::column-end:::
    <span data-ttu-id="566e9-176">:::column span="3"::: **Sicurezza fisica**</span><span class="sxs-lookup"><span data-stu-id="566e9-176">:::column span="3"::: **Physical security**</span></span>

- <span data-ttu-id="566e9-177">La sicurezza fisica dell'edificio e il controllo degli accessi all'hardware nel data center è la prima linea di difesa.</span><span class="sxs-lookup"><span data-stu-id="566e9-177">Physical building security and controlling access to computing hardware within the data center is the first line of defense.</span></span>

<span data-ttu-id="566e9-178">Al livello della sicurezza fisica, la finalità è predisporre protezioni fisiche per l'accesso alle risorse.</span><span class="sxs-lookup"><span data-stu-id="566e9-178">With physical security, the intent is to provide physical safeguards against access to assets.</span></span> <span data-ttu-id="566e9-179">In questo modo si impedisce l'aggiramento degli altri livelli e si assicura una gestione appropriata di perdite e furti.</span><span class="sxs-lookup"><span data-stu-id="566e9-179">This ensures that other layers can't be bypassed, and loss or theft is handled appropriately.</span></span>
  :::column-end:::
:::row-end:::

## <a name="summary"></a><span data-ttu-id="566e9-180">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="566e9-180">Summary</span></span>

<span data-ttu-id="566e9-181">Abbiamo visto come Azure aiuta molto a risolvere i problemi di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="566e9-181">We've seen here that Azure helps a lot with your security concerns.</span></span> <span data-ttu-id="566e9-182">La sicurezza resta comunque una **responsabilità condivisa**.</span><span class="sxs-lookup"><span data-stu-id="566e9-182">But security is still a **shared responsibility**.</span></span> <span data-ttu-id="566e9-183">Quanta di questa responsabilità ricade sull'utente dipende dal modello adottato con Azure.</span><span class="sxs-lookup"><span data-stu-id="566e9-183">How much of that responsibility falls on us depends on which model we use with Azure.</span></span>

<span data-ttu-id="566e9-184">Gli anelli della *difesa avanzata* possono essere usati come linea guida per determinare quali protezioni sono adeguate ai propri dati e ambienti.</span><span class="sxs-lookup"><span data-stu-id="566e9-184">We use the *defense in depth* rings as a guideline for considering what protections are adequate for our data and environments.</span></span>