Non esiste un "pulsante" né una soluzione facile in grado di risolvere tutte le questioni di sicurezza. Immaginiamo che Lamna Healthcare abbia trascurato la sicurezza nel proprio ambiente, ma che ora si renda conto che deve dare maggiore spazio a questo ambito. I responsabili non sanno esattamente da dove iniziare, né se possono limitarsi ad acquistare una soluzione che protegga il loro ambiente. Sanno che devono adottare un approccio globale, ma non sanno che cosa deve farne parte. Ora individueremo i concetti chiave della difesa avanzata, identificheremo le tecnologie di sicurezza chiave e le strategie per supportare una difesa avanzata e parleremo di come applicare questi concetti in fase di progettazione dei servizi di Azure.

## <a name="a-layered-approach-to-security"></a>Approccio alla sicurezza strutturato su più livelli

*Difesa avanzata* è una strategia che impiega una serie di meccanismi per rallentare l'avanzamento di un attacco volto ad accedere a informazioni senza autorizzazione. Ogni livello offre funzioni di protezione, quindi, se un livello viene violato, vi è già un livello successivo pronto a impedire un'altra esposizione. Microsoft applica un approccio alla sicurezza strutturato su più livelli, sia nei propri data center fisici che nei servizi di Azure. Lo scopo della difesa avanzata è proteggere le informazioni e prevenirne il furto da parte di utenti non autorizzati. I principi comuni usati per definire lo stato di sicurezza sono la riservatezza, l'integrità e la disponibilità.

- __Riservatezza__: principio del privilegio minimo. Limitare l'accesso alle informazioni a quegli utenti a cui l'accesso è esplicitamente concesso. Queste informazioni includono la protezione delle password degli utenti, dei certificati per l'accesso remoto e del contenuto della posta elettronica.

- __Integrità__: impedire modifiche non autorizzate alle informazioni quando sono inattive o in transito. Un approccio comune utilizzato per la trasmissione dei dati è che il mittente crea un'impronta digitale univoca dei dati usando un algoritmo hash unidirezionale. L'hash viene inviato al destinatario insieme ai dati. Il destinatario ricalcola l'hash dei dati e lo confronta con l'originale per verificare che i dati non siano stati persi o modificati in transito.

- __Disponibilità__: assicurarsi che i servizi siano disponibili per gli utenti autorizzati. Gli attacchi Denial of Service sono l'esempio di danno più comune. Anche l'eventualità di calamità naturali porta la progettazione dei sistemi ad evitare i singoli punti di vulnerabilità (SPOF) e a distribuire più istanze di un'applicazione in posizioni geograficamente isolate.

## <a name="security-layers"></a>Livelli di sicurezza

La difesa avanzata può essere considerata come una serie di anelli concentrici in cui i dati da proteggere stanno al centro. Ogni anello aggiunge un ulteriore livello di protezione ai dati. Questa strategia annulla la dipendenza da un qualsiasi singolo livello di protezione e opera rallentando l'attacco e fornendo dati di telemetria in base ai quali è possibile intervenire automaticamente o manualmente. Esaminiamo ciascuno di questi livelli.

![Difesa avanzata](../media-draft/defense_in_depth_layers_small.PNG)

### <a name="data"></a>Dati

Nella quasi totalità dei casi, gli utenti malintenzionati cercano:

- Dati archiviati in un database
- Dati archiviati su disco all'interno di macchine virtuali
- Dati archiviati in un'applicazione SaaS come Office 365
- Dati archiviati nel cloud

È responsabilità di chi archivia i dati e di chi ne controlla l'accesso assicurarsi che siano adeguatamente protetti. Esistono spesso requisiti normativi che determinano i controlli e i processi da predisporre per garantire la riservatezza, l'integrità e la disponibilità dei dati.

### <a name="applications"></a>Applicazioni

- Verificare che le applicazioni siano protette e prive di vulnerabilità
- Archiviare i segreti sensibili dell'applicazione in un supporto di archiviazione sicura
- Considerare la sicurezza un requisito di progettazione per lo sviluppo di tutte le applicazioni

L'integrazione della sicurezza nel ciclo di vita dello sviluppo dell'applicazione consentirà di ridurre il numero di vulnerabilità introdotte nel codice. Incoraggiare tutti i team di sviluppo ad assicurarsi che le proprie applicazioni nascano sicure e rendere i requisiti di sicurezza non negoziabili.

### <a name="compute"></a>Calcolo

- Accesso sicuro alle macchine virtuali
- Implementare la protezione degli endpoint e mantenere aggiornati i sistemi con le patch di sicurezza

Malware, sistemi privi di patch e sistemi protetti in modo non corretto aprono l'ambiente agli attacchi. Lo scopo a questo livello è assicurarsi che le risorse di calcolo siano sicure e che siano stati predisposti controlli appropriati per ridurre al minimo le vulnerabilità.

### <a name="networking"></a>Rete

- Limitare la comunicazione tra risorse
- Negare per impostazione predefinita
- Limitare l'accesso da Internet in ingresso e in uscita, dove appropriato
- Implementare una connettività sicura verso le reti locali

A questo livello, lo scopo è limitare la connettività di rete di tutte le risorse per consentire unicamente quella necessaria. Limitando queste comunicazioni, si riduce il rischio di movimento laterale in tutta la rete.

### <a name="perimeter"></a>Perimetro

- Utilizzare la protezione Distributed Denial-of-Service (DDoS) per filtrare gli attacchi su larga scala prima che possano bloccare il servizio per gli utenti finali
- Usare firewall perimetrali per identificare e segnalare gli attacchi dannosi alla rete

A livello di perimetro della rete, lo scopo è impedire gli attacchi di rete diretti alle risorse. Identificare gli attacchi, eliminarne l'impatto e segnalarli è importante per proteggere la rete.

### <a name="policies--access"></a>Criteri e accesso

- Controllare l'accesso all'infrastruttura, controllo delle modifiche
- Usare l'autenticazione Single Sign-On e a più fattori
- Verificare eventi e modifiche

Al livello criteri e accesso lo scopo è verificare che le identità siano protette, che l'accesso venga concesso solo se necessario e che le modifiche vengano registrate.

### <a name="physical-security"></a>Sicurezza fisica

- La sicurezza fisica dell'edificio e il controllo degli accessi all'hardware nel data center è la prima linea di difesa.

La finalità della sicurezza fisica è predisporre protezioni fisiche per l'accesso alle risorse. Ciò garantisce che non sia possibile superare gli altri livelli e che la perdita o il furto vengano gestiti in modo appropriato.

Ogni livello può riguardare uno o più dei principi di riservatezza, integrità e disponibilità.

|#|Anello|Esempio|Principio
|---|---|---|---|
|1|Dati|Crittografia dei dati inattivi in archivi blob di Azure|Integrità|
|2|Applicazione|Sessioni SSL/TLS crittografate|Integrità|
|3|Calcolo|Applicare regolarmente patch software al sistema operativo e a più livelli|Disponibilità|
|4|Rete|Regole di sicurezza di rete|Riservatezza|
|5|Perimetro|Protezione DDoS|Disponibilità|
|6|Criteri e accesso|Autenticazione utente con Azure Active Directory|Integrità|
|7|Sicurezza fisica|Controlli di accesso biometrici ai data center di Azure|Riservatezza|

## <a name="shared-responsibilities"></a>Responsabilità condivise

Ora che gli ambienti informatici passano da data center controllati dal cliente a data center nel cloud, anche la responsabilità della sicurezza cambia. La sicurezza ora è un compito condiviso dai provider di servizi cloud e dai clienti.

![shared_responsibility.png](../media-draft/shared_responsibilities.png)

## <a name="continuous-improvement"></a>Miglioramento continuo

Il panorama delle minacce evolve in tempo reale e su larga scala, pertanto un'architettura di sicurezza non può mai essere considerata completa. Microsoft e i suoi clienti devono poter rispondere a queste minacce in modo intelligente, rapido e su larga scala.

Il [Centro sicurezza di Azure](https://azure.microsoft.com/services/security-center/) offre ai clienti una gestione unificata della sicurezza e la protezione avanzata dalle minacce per rilevare e reagire agli attacchi che avvengono localmente e in Azure. A loro volta, i clienti di Azure hanno la responsabilità di continuare a valutare e far evolvere la loro architettura di sicurezza.

## <a name="defense-in-depth-at-lamna-healthcare"></a>Difesa avanzata presso Lamna Healthcare

Lamna Healthcare ha posto una grande attenzione alla difesa avanzata in tutti i reparti informatici. Poiché è responsabile di una notevole quantità di dati clinici riservati, l'organizzazione ha capito che un approccio globale è la strada migliore. 

Un team virtuale composto da membri di ogni team IT, affiancato dal team che si occupa della sicurezza, si è impegnato a cambiare la cultura dell'organizzazione. Offrono formazione a sviluppatori e architetti sulle vulnerabilità, spiegano come affrontarle e offrono consulenza per i progetti che nascono nell'organizzazione.

L'azienda si rende conto che questa attività non potrà mai essere considerata conclusa e ha predisposto revisioni regolari dei criteri, dei processi e degli aspetti tecnici e architetturali per cercare costantemente modi per migliorare la sicurezza.

## <a name="summary"></a>Riepilogo

Abbiamo esaminato che cosa si intende per difesa avanzata, quali sono i livelli di questo approccio e qual è lo scopo di ogni livello. L'uso di questo approccio per la sicurezza dell'architettura offre le basi corrette per una gestione globale della sicurezza nell'intero ambiente aziendale, evitando di focalizzarsi su una singola tecnologia o un singolo livello.