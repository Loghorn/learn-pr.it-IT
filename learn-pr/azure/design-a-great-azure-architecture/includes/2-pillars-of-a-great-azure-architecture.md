In informatica l'architettura si occupa della pianificazione, della progettazione, dell'implementazione e del miglioramento continuo di un sistema tecnologico. L'architettura di un sistema deve bilanciare e allineare i requisiti aziendali con le funzionalità tecniche necessarie per rispettare tali requisiti. Include la valutazione di rischi, costi e funzionalità nell'intero sistema e nei rispettivi componenti.

Benché non sia disponibile un approccio unico per la progettazione di un'architettura, esistono alcuni principi universali applicabili indipendentemente dal tipo di architettura, di tecnologia o di provider di servizi cloud. Concentrandosi su questi principi sarà possibile creare una base affidabile, sicura e flessibile per l'applicazione.

Un'architettura ottimale parte da una base solida costituita da quattro elementi principali:

* Sicurezza
* Prestazioni e scalabilità
* Disponibilità e possibilità di ripristino
* Efficienza e operazioni

![Elementi principali per un'architettura ottimale](../media-draft/pillars.png)

## <a name="security"></a>Sicurezza

I dati in diverse forme costituiscono spesso l'aspetto più prezioso del footprint tecnico di un'organizzazione. La sezione relativa a questo elemento fondamentale sarà incentrata sulla protezione dell'accesso all'architettura tramite l'autenticazione e la protezione dell'applicazione e dei dati dalle vulnerabilità di rete. È necessario proteggere anche l'integrità dei dati, usando strumenti quali la crittografia. È possibile che i requisiti includano anche indicazioni relative alle normative. Se i clienti non ritengono una società sicura, non la considereranno attendibile e non useranno il rispettivo prodotto.

![Tipi di attacchi](../media-draft/security.png)

## <a name="performance-and-scalability"></a>Prestazioni e scalabilità

Per assicurare prestazioni eccellenti e scalabilità per un'architettura, è necessaria una corrispondenza corretta tra le capacità delle risorse e la domanda. Le architetture cloud ottengono in genere questo risultato ridimensionando le applicazioni in modo dinamico in base alle attività dell'applicazione. La domanda per i servizi è variabile, quindi è importante che l'architettura sia in grado di adattarsi anche alla domanda. Una progettazione dell'architettura incentrata su prestazioni e scalabilità consentirà di offrire un'esperienza ottimale ai clienti, risultando al tempo stesso economicamente conveniente.

![Illustrazione grafica di un afflusso elevato di dati o richieste](../media-draft/performance-demand.png))

## <a name="availability-and-recoverability"></a>Disponibilità e possibilità di ripristino

Ogni architetto teme i problemi irreversibili dell'architettura. Un ambiente cloud ottimale è progettato in modo da anticipare gli errori a qualunque livello. L'anticipazione di tali errori comporta anche la progettazione di un sistema in grado di riprendesi da un errore, nei tempi richiesti da stakeholder e clienti.

![Errore di sistema](../media-draft/system-failure.png)

## <a name="efficiency-and-operations"></a>Efficienza e operazioni

È consigliabile progettare l'ambiente cloud in modo che la gestione e lo sviluppo risultino economicamente convenienti. È necessario identificare l'inefficienza e gli sprechi nella spesa per il cloud, in modo da assicurare che l'utilità della spesa sia massimizzata. È necessario avere a disposizione un'architettura di monitoraggio valida, che consenta di rilevare errori e problemi prima che si verifichino o almeno prima che vengano rilevati dai clienti. Occorre anche ottenere la visibilità a livello di utilizzo delle risorse disponibili da parte dell'applicazione, tramite un framework di monitoraggio affidabile.

![Efficienza](../media-draft/efficiency.png)

## <a name="design-choices"></a>Scelte di progettazione

In un'architettura ideale si crea l'ambiente più sicuro, con le prestazioni e la disponibilità più elevate e più efficiente possibile. Come in qualsiasi altra situazione, esistono tuttavia vantaggi e svantaggi. La creazione di un ambiente con i livelli più elevati per tutti questi aspetti comporta dei costi a livello di denaro, tempo necessario per la distribuzione o flessibilità operativa. Ogni organizzazione avrà priorità diverse che influiranno sulle scelte di progettazione relative a ogni elemento fondamentale.

Quando si crea un'architettura di Azure, è necessario tenere in considerazione alcuni aspetti. Si vuole ottenere un'architettura sicura, ridimensionabile, disponibile e ripristinabile. Per ottenere questo risultato, sarà necessario prendere decisioni basate sui costi e sulle priorità dell'organizzazione.
