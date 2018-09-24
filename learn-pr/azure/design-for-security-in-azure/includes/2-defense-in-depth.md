Non esiste una "scorciatoia" né una soluzione in grado di risolvere tutti i problemi di sicurezza. Si supponga che Lamna Healthcare abbia trascurato la sicurezza nel proprio ambiente. La società si rende conto di dover dedicare grande attenzione a questa area. I responsabili non sanno esattamente da dove iniziare, né se possono limitarsi ad acquistare una soluzione che protegga l'ambiente. Sanno che devono adottare un approccio olistico, ma non sono certi riguardo a quali elementi debbano farne parte. Verranno ora identificati i concetti principali di una difesa avanzata e le tecnologie e le strategie di sicurezza essenziali per supportare la difesa avanzata e verrà descritto come applicare questi concetti in fase di progettazione dei servizi di Azure.

#### <a name="defense-in-depth"></a>Difesa avanzata

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWjTfZ]

## <a name="a-layered-approach-to-security"></a>Approccio alla sicurezza strutturato su più livelli

*Difesa avanzata* è una strategia che impiega una serie di meccanismi per rallentare l'avanzamento di un attacco volto ad accedere a informazioni senza autorizzazione. Ogni livello offre funzioni di protezione, quindi, se un livello viene violato, vi è già un livello successivo pronto a impedire un'ulteriore esposizione. Microsoft applica un approccio alla sicurezza strutturato su più livelli, sia nei propri data center fisici che nei servizi di Azure. Lo scopo della difesa avanzata è proteggere le informazioni e prevenirne il furto da parte di soggetti non autorizzati. I principi comuni usati per definire il comportamento di sicurezza sono la riservatezza, l'integrità e la disponibilità.

- __Riservatezza__: principio dei privilegi minimi. Limita l'accesso alle informazioni ai soli utenti cui è concesso esplicitamente l'accesso. Le informazioni protette includono le password degli utenti, i certificati di accesso remoto e il contenuto della posta elettronica.

- __Integrità__: impedire modifiche non autorizzate alle informazioni quando sono inattive o in transito. Un approccio comune usato per la trasmissione dei dati consiste nella creazione da parte del mittente di un'impronta digitale univoca dei dati mediante un algoritmo hash unidirezionale. L'hash viene inviato al destinatario insieme ai dati. Il destinatario ricalcola l'hash dei dati e lo confronta con l'originale per verificare che i dati non siano stati persi o modificati durante la trasmissione.

- __Disponibilità__: garantire la disponibilità dei servizi per gli utenti autorizzati. Gli attacchi Denial of Service sono la causa prevalente della perdita di disponibilità per gli utenti. Nella progettazione di sistemi anche la minaccia di calamità naturali porta a evitare singoli punti di errore e a distribuire più istanze di un'applicazione in aree geografiche diverse.

## <a name="security-layers"></a>Livelli di sicurezza

La difesa avanzata può essere visualizzata come una serie di anelli concentrici, al cui centro si trovano i dati da proteggere. Ogni anello aggiunge un livello di protezione attorno ai dati. Questa strategia consente di evitare la dipendenza da un singolo livello di protezione e opera rallentando l'attacco e generando avvisi con dati di telemetria, in base ai quali è possibile intervenire automaticamente o manualmente. Di seguito viene esaminato ciascun livello.

![Figura che mostra la difesa avanzata con i dati al centro. Gli anelli di sicurezza attorno ai dati sono: applicazione, calcolo, rete, perimetro, identità e accesso e sicurezza fisica.](../media/defense_in_depth_layers_small.PNG)

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

L'integrazione della sicurezza nel ciclo di sviluppo delle applicazioni consentirà di ridurre il numero di vulnerabilità introdotte nel codice. Invitare tutti i team di sviluppo a garantire la sicurezza predefinita delle applicazioni. Definire requisiti di sicurezza non negoziabili.

### <a name="compute"></a>Calcolo

- Proteggere l'accesso alle macchine virtuali
- Implementare la protezione degli endpoint e mantenere aggiornati i sistemi con le patch di sicurezza

Malware, sistemi privi di patch e sistemi non protetti in modo corretto espongono l'ambiente agli attacchi. Lo scopo a questo livello è assicurarsi che le risorse di calcolo siano sicure e che siano stati predisposti controlli appropriati in grado di ridurre al minimo i problemi di sicurezza.

### <a name="networking"></a>Rete

- Limitare la comunicazione tra le risorse tramite segmentazione e controlli di accesso
- Negare per impostazione predefinita
- Limitare l'accesso Internet in ingresso e in uscita, dove appropriato
- Implementare una connettività sicura verso le reti locali

A questo livello, lo scopo è limitare la connettività di rete di tutte le risorse, consentendo unicamente quella necessaria. Segmentare le risorse e usare controlli a livello di rete per limitare la comunicazione a quella necessaria. Limitando la comunicazione, si riduce il rischio di spostamento laterale attraverso la rete.

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

Ogni livello può implementare uno o più dei principi di riservatezza, integrità e disponibilità.

|#|Anello|Esempio|Principio
|---|---|---|---|
|1|Dati|Crittografia di dati inattivi nell'archiviazione BLOB di Azure|Integrità|
|2|Applicazione|Sessioni SSL/TLS crittografate|Integrità|
|3|Calcolo|Applicare regolarmente patch software al sistema operativo e ai livelli di sicurezza|Disponibilità|
|4|Rete|Regole di sicurezza di rete|Riservatezza|
|5|Perimetro|Protezione DDoS|Disponibilità|
|6|Criteri e accesso|Autenticazione utente di Azure Active Directory|Integrità|
|7|Sicurezza fisica|Controlli di accesso biometrici ai data center di Azure|Riservatezza|

## <a name="shared-responsibilities"></a>Responsabilità condivise

Ora che gli ambienti informatici passano da data center controllati dal cliente a data center nel cloud, anche la responsabilità della sicurezza cambia. La sicurezza ora è un compito condiviso tra i provider di servizi cloud e i clienti.

![Figura che mostra come i provider di servizi cloud e i clienti condividano le responsabilità in materia di sicurezza per diversi tipi di implementazioni di servizi di calcolo: in locale, infrastruttura distribuita come servizio, piattaforma distribuita come servizio e software come un servizio. ](../media/shared_responsibilities.png)

## <a name="continuous-improvement"></a>Miglioramento continuo

Il panorama delle minacce evolve in tempo reale e su larga scala, pertanto un'architettura di sicurezza non può mai essere considerata completa. Microsoft e i suoi clienti devono essere in grado di rispondere a queste minacce in modo intelligente, rapido e su larga scala.

Il [Centro sicurezza di Azure](https://azure.microsoft.com/services/security-center/) offre ai clienti una gestione unificata della sicurezza e una protezione avanzata dalle minacce per rilevare e reagire agli eventi di sicurezza che avvengono localmente e in Azure. A loro volta, i clienti di Azure hanno la responsabilità di continuare a valutare e far evolvere la loro architettura di sicurezza.

## <a name="defense-in-depth-at-lamna-healthcare"></a>Difesa avanzata presso Lamna Healthcare

Lamna Healthcare ha posto una grande attenzione alla difesa avanzata in tutti i reparti informatici. Poiché è responsabile di una notevole quantità di dati clinici riservati, l'organizzazione ha capito che un approccio globale è la strada migliore. 

Un team virtuale composto da membri di ogni team IT, affiancato dal team che si occupa della sicurezza, si è impegnato a cambiare la cultura dell'organizzazione. Offre formazione a sviluppatori e architetti sulle vulnerabilità, spiega come affrontarle e fornisce consulenza sui progetti in corso nell'organizzazione.

L'azienda si rende conto che questa attività non potrà mai essere considerata conclusa e ha predisposto revisioni regolari dei criteri, dei processi e degli aspetti tecnici e architetturali per cercare costantemente modi per migliorare la sicurezza.

## <a name="summary"></a>Riepilogo

Abbiamo esaminato che cosa si intende per difesa avanzata, quali sono i livelli di questo approccio e qual è lo scopo di ogni livello. L'uso di questo approccio per la sicurezza dell'architettura fornisce le basi corrette per una gestione globale della sicurezza nell'intero ambiente aziendale, evitando di focalizzarsi su una singola tecnologia o un singolo livello.