Ora che gli ambienti informatici passano da data center controllati dal cliente a data center nel cloud, anche la responsabilità della sicurezza cambia. La sicurezza ora è un compito condiviso dai provider di servizi cloud e dai clienti. Per ogni applicazione e soluzione, è importante comprendere quali sono le responsabilità del cliente e quali gli aspetti gestiti da Azure. 

## <a name="share-security-responsibility-with-azure"></a>Condividere la responsabilità della sicurezza con Azure

Il primo passaggio è quello dai data center locali agli ambienti IaaS (Infrastructure-as-a-Service). In un ambiente IaaS il cliente usa il servizio di livello più basso e chiede ad Azure di creare macchine virtuali e reti virtuali. A questo livello è comunque responsabilità dell'utente applicare patch e proteggere i sistemi operativi e il software, nonché configurare la rete in modo sicuro. Contoso Shipping sfrutta i vantaggi dei servizi IaaS quando inizia a usare macchine virtuali di Azure anziché i propri server fisici locali. Oltre ai vantaggi operativi, gode del vantaggio in termini di sicurezza di aver esternalizzato il compito di proteggere le parti fisiche della rete.

Il passaggio a una soluzione PaaS (Platform-as-a-Service) esternalizza molti problemi di sicurezza. A questo livello, Azure si occupa del sistema operativo e della maggior parte dei software di base, come i sistemi di gestione dei database. Tutto è aggiornato con le ultime patch di sicurezza e può essere integrato con Active Directory per il controllo degli accessi. Gli ambienti PaaS presentano anche molti vantaggi operativi. Invece di costruire intere infrastrutture e subnet manualmente per i propri ambienti, è possibile effettuare selezioni all'interno del portale di Azure o eseguire script automatici per attivare e disattivare complessi sistemi protetti, nonché ridimensionarli a seconda delle necessità. Contoso usa l'hub eventi per inserire i dati di telemetria trasmessi dai veicoli. Usa inoltre un'app Web con un back-end CosmosDb con le proprie app mobili. Questi servizi sono tutti esempi di PaaS.

Con una soluzione SaaS (Software-as-a-Service), si esternalizza quasi tutto. Il SaaS è un software eseguito con l'infrastruttura Internet e il cui codice è controllato dal fornitore, ma è configurato per essere usato dal cliente. Come molte aziende, Contoso usa Office 365, che è un perfetto esempio di SaaS.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![shared_responsibility.png](../media-COPIED-FROM-DESIGNFORSECURITY/shared_responsibilities.png)

## <a name="a-layered-approach-to-security"></a>Approccio alla sicurezza strutturato su più livelli

*Difesa avanzata* è una strategia che impiega una serie di meccanismi per rallentare l'avanzamento di un attacco volto ad accedere a informazioni senza autorizzazione. Ogni livello offre funzioni di protezione, quindi, se un livello viene violato, vi è già un livello successivo pronto a impedire un'ulteriore esposizione. Microsoft applica un approccio alla sicurezza strutturato su più livelli, sia nei propri data center fisici che nei servizi di Azure. Lo scopo della difesa avanzata è proteggere le informazioni e prevenirne il furto da parte di soggetti non autorizzati.

La difesa avanzata può essere visualizzata come una serie di anelli concentrici, al cui centro si trovano i dati da proteggere. Ogni anello aggiunge un livello di protezione attorno ai dati. Questa strategia consente di evitare la dipendenza da un singolo livello di protezione e opera rallentando l'attacco e generando avvisi con dati di telemetria, in base ai quali è possibile intervenire automaticamente o manualmente. Esaminiamo ciascuno di questi livelli.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Difesa avanzata](../media-COPIED-FROM-DESIGNFORSECURITY/defense_in_depth_layers_small.PNG)

### <a name="data"></a>Dati

Nella quasi totalità dei casi, gli utenti malintenzionati cercano:

- Dati archiviati in un database
- Dati archiviati su disco all'interno di macchine virtuali
- Dati archiviati in applicazioni SaaS, come Office 365
- Dati archiviati nel cloud

È responsabilità di chi archivia i dati e di chi ne controlla l'accesso assicurarsi che siano adeguatamente protetti. Esistono spesso requisiti normativi che determinano i controlli e i processi da predisporre per garantire la riservatezza, l'integrità e la disponibilità dei dati.

### <a name="applications"></a>Applicazioni

- Verificare che le applicazioni siano protette e prive di vulnerabilità
- Archiviare i segreti sensibili delle applicazioni in un supporto di archiviazione sicuro
- Prevedere la sicurezza come requisito di progettazione per lo sviluppo di tutte le applicazioni

L'integrazione della sicurezza nel ciclo di sviluppo delle applicazioni consentirà di ridurre il numero di vulnerabilità introdotte nel codice. Sollecitare tutti i team di sviluppo a garantire la sicurezza predefinita delle proprie applicazioni e rendere i requisiti di sicurezza imprescindibili.

### <a name="compute"></a>Calcolo

- Proteggere l'accesso alle macchine virtuali
- Implementare la protezione degli endpoint e mantenere aggiornati i sistemi con le patch di sicurezza

Malware, sistemi privi di patch e sistemi non protetti in modo corretto espongono l'ambiente agli attacchi. Lo scopo a questo livello è assicurarsi che le risorse di calcolo siano sicure e che siano stati predisposti controlli appropriati in grado di ridurre al minimo le vulnerabilità.

### <a name="networking"></a>Rete

- Limitare la comunicazione tra risorse
- Negare per impostazione predefinita
- Limitare l'accesso Internet in ingresso e in uscita, dove appropriato
- Implementare una connettività sicura verso le reti locali

A questo livello, lo scopo è limitare la connettività di rete di tutte le risorse, consentendo unicamente quella necessaria. Limitando queste comunicazioni, si riduce il rischio di spostamento laterale attraverso la rete.

### <a name="perimeter"></a>Perimetro

- Implementare la protezione dalle minacce DDoS (Distributed Denial-of-Service) per filtrare gli attacchi su larga scala prima che possano bloccare il servizio per gli utenti finali
- Usare firewall perimetrali per identificare e segnalare gli attacchi dannosi alla rete

A livello di perimetro della rete, lo scopo è impedire gli attacchi di rete diretti alle risorse. Identificare gli attacchi, neutralizzarne l'impatto e segnalarli è importante per proteggere la rete.

### <a name="policies--access"></a>Criteri e accesso

- Controllare l'accesso all'infrastruttura, Implementare il controllo delle modifiche
- Usare l'autenticazione a più fattori e Single Sign-On
- Controllare eventi e modifiche

Al livello criteri e accesso lo scopo è verificare che le identità siano protette, che l'accesso venga concesso solo se necessario e che le modifiche vengano registrate.

### <a name="physical-security"></a>Sicurezza fisica

- La sicurezza fisica dell'edificio e il controllo degli accessi all'hardware nel data center è la prima linea di difesa.

Al livello della sicurezza fisica, la finalità è predisporre protezioni fisiche per l'accesso alle risorse. In questo modo si impedisce l'aggiramento degli altri livelli e si assicura una gestione appropriata di perdite e furti.

Abbiamo visto come Azure aiuta molto a risolvere i problemi di sicurezza. La sicurezza resta comunque una **responsabilità condivisa**. Quanta di questa responsabilità ricade sull'utente dipende dal modello adottato con Azure.

Gli anelli della *difesa avanzata* possono essere usati come linea guida per determinare quali protezioni sono adeguate ai propri dati e ambienti.