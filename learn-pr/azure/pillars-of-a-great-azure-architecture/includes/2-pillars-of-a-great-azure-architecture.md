Il cloud ha cambiato il modo le organizzazioni risolvono sfide aziendali e come le applicazioni e sistemi sono stati progettati. Il ruolo di un progettista di soluzioni non è solo per fornire valore aziendale tramite i requisiti funzionali dell'applicazione, ma per assicurarsi che la soluzione è progettata in modo che sia scalabili, resilienti, efficiente e sicuro. Architettura della soluzione riguarda la pianificazione, progettazione, implementazione e miglioramento continuativo di un sistema di tecnologia. L'architettura di un sistema deve bilanciare e allineare i requisiti aziendali con le funzionalità tecniche necessarie per rispettare tali requisiti. Include la valutazione di rischi, costi e funzionalità nell'intero sistema e nei rispettivi componenti.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEv2]

Se non è presente alcun approccio unico per la progettazione di un'architettura, esistono alcuni concetti universale che verranno applicato indipendentemente dall'architettura, la tecnologia o provider di servizi cloud. Anche se questi non sono gratis, con particolare attenzione su questi concetti consentirà le basi per l'applicazione affidabile, sicura e flessibile.

Un'architettura ottimale parte da una base solida costituita da quattro elementi principali:

* Sicurezza
* Prestazioni e scalabilità
* Disponibilità e possibilità di ripristino
* Efficienza e operazioni

![Elementi principali per un'architettura ottimale](../media-draft/pillars.png)

## <a name="security"></a>Sicurezza

I dati in varie forme sono l'informazione più importante del footprint di tecnici della propria organizzazione. La sezione relativa a questo elemento fondamentale sarà incentrata sulla protezione dell'accesso all'architettura tramite l'autenticazione e la protezione dell'applicazione e dei dati dalle vulnerabilità di rete. È necessario proteggere anche l'integrità dei dati, usando strumenti quali la crittografia.

È necessario considerare sulla sicurezza nell'intero ciclo di vita dell'applicazione, dalla progettazione e implementazione alla distribuzione e operazioni. Il cloud offre la protezione da un'ampia gamma di minacce, ad esempio intrusioni di rete e attacchi DDoS, ma è necessario implementare la sicurezza nell'applicazione, processi e delle impostazioni cultura dell'organizzazione.

![Tipi di attacchi](../media-draft/security.png)

## <a name="performance-and-scalability"></a>Prestazioni e scalabilità

Per assicurare prestazioni eccellenti e scalabilità per un'architettura, è necessaria una corrispondenza corretta tra le capacità delle risorse e la domanda. Le architetture cloud ottengono in genere questo risultato ridimensionando le applicazioni in modo dinamico in base alle attività dell'applicazione. La domanda per i servizi è variabile, quindi è importante che l'architettura sia in grado di adattarsi anche alla domanda. Una progettazione dell'architettura incentrata su prestazioni e scalabilità consentirà di offrire un'esperienza ottimale ai clienti, risultando al tempo stesso economicamente conveniente.

![Illustrazione grafica di un afflusso elevato di dati o richieste](../media-draft/performance-demand.png))

## <a name="availability-and-recoverability"></a>Disponibilità e possibilità di ripristino

Ogni architetto teme i problemi irreversibili dell'architettura. Un ambiente cloud ottimale è progettato in modo da anticipare gli errori a qualunque livello. L'anticipazione di tali errori comporta anche la progettazione di un sistema in grado di riprendesi da un errore, nei tempi richiesti da stakeholder e clienti.

![Errore di sistema](../media-draft/system-failure.png)

## <a name="efficiency-and-operations"></a>Efficienza e operazioni

È opportuno progettare l'ambiente cloud in modo che risulti più conveniente per gestire e sviluppare in. Inefficienza e gli sprechi nel cloud spesa deve essere identificata per garantire la spesa denaro in cui è realizzare l'utilizzo massimo di esso. È necessario disporre una buona architettura del monitoraggio in modo che è possibile rilevare gli errori e problemi prima che si verifichino o, come minimo, prima si noti che i clienti. È anche necessario avere alcuni visibilità in al modo in cui l'applicazione usa le relative risorse disponibili tramite un framework di monitoraggio affidabile.

![Efficienza](../media-draft/efficiency.png)

## <a name="shared-responsibility"></a>Responsabilità condivisa

Lo spostamento nel cloud introduce un modello di responsabilità condivisa. In questo modello, il provider di cloud gestiranno alcuni aspetti dell'applicazione, offrendoti la responsabilità rimanente. In un ambiente locale è responsabile per tutte le operazioni. Quando si spostano all'infrastruttura come servizio (IaaS), quindi alla piattaforma distribuita come servizio (PaaS) e software come servizio (SaaS), il provider di cloud richiederà su più di questa responsabilità. Questa responsabilità condivisa svolgerà un ruolo nelle tue decisioni architetturale, come si possono avere implicazioni sul costo, capacità operativa, sicurezza e le funzionalità tecniche dell'applicazione. Grazie al passaggio queste responsabilità al provider di è possibile concentrarsi sulla salamoia valore all'azienda e spostarsi dall'attività che non sono una funzione aziendale principale.

![Modelli di servizio cloud](../media-draft/cloud-responsibility-model.png)

## <a name="design-choices"></a>Scelte di progettazione

In un'architettura ideale si crea l'ambiente più sicuro, con le prestazioni e la disponibilità più elevate e più efficiente possibile. Come in qualsiasi altra situazione, esistono tuttavia vantaggi e svantaggi. La creazione di un ambiente con i livelli più elevati per tutti questi aspetti comporta dei costi a livello di denaro, tempo necessario per la distribuzione o flessibilità operativa. Ogni organizzazione avrà priorità diverse che influiranno sulle scelte di progettazione relative a ogni elemento fondamentale. Quando si progetta l'architettura, è necessario determinare quali compromessi siano accettabili e non quelli attendibili.

Quando si crea un'architettura di Azure, è necessario tenere in considerazione alcuni aspetti. Si vuole ottenere un'architettura sicura, ridimensionabile, disponibile e ripristinabile. A che scopo, è possibile prendere decisioni basate su costi, le priorità dell'organizzazione e al rischio.