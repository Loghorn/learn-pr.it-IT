Il cloud ha rivoluzionato il modo in cui le organizzazioni risolvono le problematiche e ha cambiato la modalità di progettazione di applicazioni e sistemi. Il ruolo di un progettista di soluzioni non si limita allo sviluppo dei requisiti funzionali dell'applicazione per offrire valore di business, ma assicura anche che la soluzione sia progettata in un modo scalabile, resiliente, efficiente e sicuro. L'architettura della soluzione si occupa della pianificazione, della progettazione, dell'implementazione e del miglioramento continuo di un sistema IT. L'architettura di un sistema deve bilanciare e allineare i requisiti aziendali con le funzionalità tecniche necessarie per rispettare tali requisiti. Include la valutazione di rischi, costi e funzionalità nell'intero sistema e nei rispettivi componenti.

#### <a name="design-a-great-azure-architecture"></a>Progettare un'architettura di Azure ottimale

<!-- TODO: revisit this video after Ignite -->
<!-- > VIDEO: https://www.microsoft.com/videoplayer/embed/RE2yEv2 -->

Benché non sia disponibile un approccio unico per la progettazione di un'architettura, esistono alcuni concetti universali applicabili indipendentemente dal tipo di architettura, di tecnologia o di provider di servizi cloud. Benché non siano esaustivi, concentrandosi su questi concetti sarà possibile creare una base affidabile, sicura e flessibile per l'applicazione.

Un'architettura ottimale parte da una base solida costituita da quattro elementi principali:

* Sicurezza
* Prestazioni e scalabilità
* Disponibilità e possibilità di ripristino
* Efficienza e operazioni

![Figura che mostra gli elementi fondamentali di un'architettura di Azure ottimale](../media/pillars.png)

## <a name="security"></a>Sicurezza

I dati costituiscono l'aspetto più prezioso del footprint tecnico di un'organizzazione. La sezione relativa a questo elemento fondamentale sarà incentrata sulla protezione dell'accesso all'architettura tramite l'autenticazione e la protezione dell'applicazione e dei dati dalle vulnerabilità di rete. È necessario proteggere anche l'integrità dei dati, usando strumenti come la crittografia.

La sicurezza deve essere tenuta in considerazione durante l'intero ciclo di vita di un'applicazione, dalla progettazione e implementazione alla distribuzione e alle operazioni. Il cloud fornisce protezione da una varietà di minacce, come le intrusioni in rete e gli attacchi DDoS, ma la sicurezza deve restare comunque una componente fondamentale dell'applicazione, dei processi e della cultura dell'organizzazione.

![Figura che mostra i tipi di minacce alla sicurezza e gli attacchi a cui potrebbero essere esposti i dati nel cloud.](../media/security.png)

## <a name="performance-and-scalability"></a>Prestazioni e scalabilità

Per assicurare prestazioni eccellenti e scalabilità per un'architettura, è necessaria una corrispondenza corretta tra le capacità delle risorse e la domanda. Le architetture cloud ottengono in genere questo risultato ridimensionando le applicazioni in modo dinamico in base alle attività dell'applicazione. La domanda per i servizi è variabile, quindi è importante che l'architettura sia in grado di adattarsi anche alla domanda. Una progettazione dell'architettura incentrata su prestazioni e scalabilità consentirà di offrire un'esperienza ottimale ai clienti, risultando al tempo stesso economicamente conveniente.

![Figura che mostra come le risorse nel cloud vengono ridimensionate in modo dinamico su richiesta, con utilizzo estremamente efficiente. Al contrario, quando le risorse vengono implementate a un livello fisso, questo si traduce in un utilizzo inefficiente quando la domanda è bassa e in una situazione di carenza nei periodi di domanda elevata.](../media/performance-demand.png)

## <a name="availability-and-recoverability"></a>Disponibilità e possibilità di ripristino

Ogni architetto teme i problemi irreversibili dell'architettura. Un ambiente cloud ottimale è progettato in modo da anticipare gli errori a qualunque livello. L'anticipazione di tali errori comporta anche la progettazione di un sistema in grado di riprendersi da un errore, nei tempi richiesti da stakeholder e clienti.

![Figura che mostra due macchine virtuali in una rete virtuale. Una delle due macchine virtuali è in errore, mentre l'altra lavora per soddisfare le richieste dei clienti.](../media/system-failure.png)

## <a name="efficiency-and-operations"></a>Efficienza e operazioni

È consigliabile progettare l'ambiente cloud in modo che la gestione e lo sviluppo risultino economicamente convenienti. Occorre identificare l'inefficienza e gli sprechi nella spesa per il cloud, in modo da assicurare che l'utilità della spesa sia massimizzata. È importante avere a disposizione un'architettura di monitoraggio valida, che consenta di rilevare errori e problemi prima che si verifichino o almeno prima che vengano rilevati dai clienti. Occorre anche avere visibilità sul modo in cui l'applicazione usa le risorse disponibili, tramite un framework di monitoraggio affidabile.

![Figura che mostra l'aumento di qualità, velocità ed efficienza con costi in calo.](../media/efficiency.png)

# <a name="shared-responsibility"></a>Responsabilità condivisa

Il passaggio al cloud introduce un modello di responsabilità condivisa. In questo modello il provider di servizi cloud gestisce determinati aspetti dell'applicazione, lasciando all'organizzazione la responsabilità di tutti gli altri aspetti. In un ambiente locale la responsabilità è totalmente dell'organizzazione. Man mano che si passa all'infrastruttura distribuita come servizio (IaaS), quindi alla piattaforma come servizio (PaaS) e infine al software come un servizio (SaaS), il provider di servizi cloud si assume una responsabilità sempre maggiore. Questa responsabilità condivisa avrà un ruolo nelle decisioni relative all'architettura, che possono avere implicazioni sui costi, le capacità operative, la sicurezza e le funzionalità tecniche dell'applicazione. Passando queste responsabilità al provider ci si potrà concentrare su come portare valore aggiunto all'azienda e abbandonare le attività non fondamentali per l'organizzazione.

![Figura che mostra il livello di responsabilità condivise in ogni tipo di modello di servizio cloud](../media/cloud-responsibility-model.png)

# <a name="design-choices"></a>Scelte di progettazione

In un'architettura ideale si crea l'ambiente più sicuro, con le prestazioni e la disponibilità più elevate e più efficiente possibile. Come in qualsiasi altra situazione, esistono tuttavia vantaggi e svantaggi. La creazione di un ambiente con i livelli più elevati per tutti questi aspetti comporta dei costi a livello di denaro, tempo necessario per la distribuzione o flessibilità operativa. Ogni organizzazione avrà priorità diverse che influiranno sulle scelte di progettazione relative a ogni elemento fondamentale. In fase di progettazione dell'architettura è importante determinare quali compromessi sono accettabili e quali non lo sono.

Quando si crea un'architettura di Azure, è necessario tenere in considerazione diversi aspetti. Si vuole ottenere un'architettura sicura, scalabile, disponibile e ripristinabile. Per ottenere questo risultato, sarà necessario prendere decisioni basate sui costi, sulle priorità dell'organizzazione e sui rischi.
