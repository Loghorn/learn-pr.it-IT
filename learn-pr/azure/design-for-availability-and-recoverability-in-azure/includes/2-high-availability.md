La disponibilità elevata garantisce che l'architettura sia in grado di gestire gli errori. Si immagini di essere il responsabile di un sistema che deve essere sempre completamente operativo. Dal momento che è inevitabile che si verifichino errori, come è possibile assicurarsi che il sistema rimanga online in caso di problemi? Come si esegue la manutenzione senza interrompere il servizio? 

In questo modulo si apprenderà quando è necessaria la disponibilità elevata, come valutare i requisiti di disponibilità elevata dell'applicazione e in che modo la piattaforma Azure aiuta a soddisfare gli obiettivi di disponibilità.

## <a name="what-is-high-availability"></a>Che cos'è la disponibilità elevata?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvc]

Un servizio a disponibilità elevata assorbe le fluttuazioni di disponibilità, carico ed errori temporanei nell'hardware e nei servizi dipendenti. L'applicazione rimane online e disponibile (o continua a sembrare tale) con un livello di prestazioni accettabile. Questa disponibilità è spesso definita da requisiti di business, obiettivi del livello di servizio o contratti di servizio dell'applicazione.

In ultima analisi la disponibilità elevata è la capacità di gestire la perdita o una grave riduzione delle prestazioni di un componente di un sistema. Il problema può essere causato dal passaggio alla modalità offline di una macchina virtuale che ospita un'applicazione perché si è verificato un problema dell'host. Può essere dovuto a una manutenzione pianificata per un aggiornamento del sistema. Può anche essere causato dall'errore di un servizio nel cloud. Identificando le posizioni in cui potrebbe verificarsi un errore del sistema e creando le funzionalità per gestire tali errori, è possibile garantire la costante disponibilità online dei servizi offerti ai clienti.

In genere la disponibilità elevata di un servizio richiede la disponibilità elevata dei componenti che costituiscono il servizio. Si pensi a un sito Web che offre un marketplace online per l'acquisto di articoli. Il servizio offerto ai clienti è la possibilità di visualizzare in un elenco, acquistare e vendere articoli online. Per rendere disponibile questo servizio saranno necessari più componenti: un database, dei server Web, dei server applicazioni e così via. Poiché ognuno di questi componenti potrebbe presentare un problema, è necessario identificare i punti di errore, stabilire dove si trovano e determinare come gestire questi punti di errore nell'architettura.

## <a name="evaluate-high-availability-for-your-architecture"></a>Valutare la disponibilità elevata per l'architettura in uso

La valutazione di un'applicazione per la disponibilità elevata comprende tre passaggi:

1. Determinare il contratto di servizio dell'applicazione
1. Valutare le funzionalità per la disponibilità elevata dell'applicazione
1. Valutare le funzionalità per la disponibilità elevata delle applicazioni dipendenti

Esaminiamo questi passaggi in dettaglio.

### <a name="determine-the-service-level-agreement-of-your-application"></a>Determinare il contratto di servizio dell'applicazione

Un contratto di servizio è un contratto tra il provider di un servizio e un consumer del servizio, in cui il provider del servizio si impegna per il rispetto di uno standard del servizio basato su metriche misurabili e responsabilità definite. I contratti di servizio possono essere accordi contrattuali legalmente vincolanti o aspettative di disponibilità da parte dei clienti. Le metriche del servizio in genere riguardano la velocità effettiva, la capacità e la disponibilità del servizio, ognuna delle quali può essere misurata in vari modi. Indipendentemente dalle specifiche metriche che costituiscono il contratto di servizio, il mancato rispetto del contratto di servizio può avere gravi conseguenze finanziarie per il provider del servizio. Un componente comune dei contratti di servizio è un rimborso finanziario garantito in caso di mancato rispetto del contratto.

Gli obiettivi del livello di servizio (SLO) sono valori di metriche target usati per misurare le prestazioni, l'affidabilità o la disponibilità. Possono essere metriche che definiscono ad esempio le prestazioni di elaborazione richieste in millisecondi, la disponibilità dei servizi in minuti al mese o il numero di richieste elaborate ogni ora. Tramite la valutazione delle metriche prodotte dall'applicazione e il riconoscimento degli elementi usati dai clienti per misurare la qualità, è possibile definire intervalli accettabili e non accettabili per questi obiettivi. Mediante la definizione di questi obiettivi si definiscono risultati e attese sia per i team che supportano i servizi sia per i clienti che li usano. I punti di disconnessione singolo (SLO) verranno usati per determinare se il contratto di servizio complessivo è soddisfatto.

La tabella seguente illustra il potenziale tempo totale di inattività per vari livelli di contratto di servizio. 

| Contratto di servizio | Tempo di inattività settimanale | Tempo di inattività mensile | Tempo di inattività annuale |
| --- | --- | --- | --- |
| 99% |1,68 ore |7,2 ore |3,65 giorni |
| 99,9% |10,1 minuti |43,2 minuti |8,76 ore |
| 99,95% |5 minuti |21,6 minuti |4,38 ore |
| 99,99% |1,01 minuti |4,32 minuti |52,56 minuti |
| 99,999% |6 secondi |25,9 secondi |5,26 minuti |

Naturalmente, a parità di condizioni, una maggiore disponibilità è migliore. Tuttavia, quando si cercano di ottenere più 9, i costi e la complessità per raggiungere quel livello di disponibilità aumentano. Un tempo di attività pari al 99,99% si traduce in circa 5 minuti di inattività totale al mese. Vale la pena sostenere questa maggiore complessità e questi costi aggiuntivi per raggiungere cinque 9? La risposta dipende dai requisiti aziendali. 

Di seguito sono riportate alcune altre considerazioni che emergono quando si definisce un contratto di servizio:

* Per ottenere quattro 9 (99,99%), non ci si può probabilmente basare su un intervento manuale per correggere gli errori. L'applicazione, infatti, deve essere in grado di eseguire automaticamente la diagnosi e la riparazione. 
* Dopo il quarto 9, è difficile rilevare le interruzioni in modo sufficientemente rapido per soddisfare il contratto di servizio.
* Si pensi all'intervallo di tempo rispetto al quale viene misurato il contratto di servizio: minore è la finestra, più ristretto sarà il livello di tolleranza. Probabilmente non ha senso definire il contratto di servizio in termini di tempo di attività oraria o giornaliera. 

L'identificazione dei contratti di servizio è un primo passaggio importante per determinare le funzionalità con disponibilità elevata che richiederà l'architettura. Queste funzionalità consentiranno di identificare i metodi da usare per garantire la disponibilità elevata dell'applicazione.

### <a name="evaluate-the-ha-capabilities-of-the-application"></a>Valutare le funzionalità per la disponibilità elevata dell'applicazione

Per valutare le funzionalità per la disponibilità elevata dell'applicazione, eseguire un'analisi degli errori. È necessario concentrarsi sui singoli punti di errore e sui componenti critici che potrebbero avere un notevole impatto sull'applicazione se risultassero non raggiungibili, configurati in modo errato o con un comportamento imprevisto. Per le aree che presentano ridondanza, determinare se l'applicazione è in grado di rilevare le condizioni di errore e di eseguire la riparazione automatica.

È necessario valutare con attenzione tutti i componenti dell'applicazione, tra cui le parti progettate per fornire funzionalità per la disponibilità elevata, ad esempio i servizi di bilanciamento del carico. Sarà necessario modificare i singoli punti di errore per integrare funzionalità per la disponibilità elevata o sostituirli con servizi in grado di fornire funzionalità per la disponibilità elevata.

### <a name="evaluate-the-ha-capabilities-of-dependent-applications"></a>Valutare le funzionalità per la disponibilità elevata delle applicazioni dipendenti

È necessario riconoscere non solo i requisiti del contratto di servizio dell'applicazione per i consumer, ma anche i contratti di servizio di tutte le risorse da cui l'applicazione può dipendere. Se ci si impegna con i clienti per un tempo di attività pari al 99,9%, ma un servizio da cui dipende l'applicazione ha un requisito di tempo di attività che raggiunge solo il 99%, questo potrebbe mettere a rischio la capacità di rispettare il contratto di servizio per i clienti. Se un servizio dipendente non riesce a offrire un contratto di servizio sufficiente, potrebbe essere necessario modificare il proprio contratto di servizio, sostituire la dipendenza con un'alternativa o trovare un modo per rispettare il contratto di servizio quando la dipendenza non è disponibile. A seconda dello scenario e della natura della dipendenza, è possibile usare soluzioni alternative per i problemi delle dipendenze, come cache e code di lavoro.

## <a name="azures-highly-available-platform"></a>Piattaforma a disponibilità elevata di Azure

La piattaforma cloud di Azure è stata progettata per garantire una disponibilità elevata in tutti i servizi. Come qualsiasi sistema, le applicazioni possono essere interessate da eventi della piattaforma hardware e software. La necessità di progettare l'architettura dell'applicazione per gestire gli errori è di importanza critica. La piattaforma cloud di Azure offre strumenti e funzionalità che consentono di garantire la disponibilità elevata dell'applicazione. Quando si valuta la disponibilità elevata per l'architettura in Azure, è necessario tenere presenti vari concetti di base:

* Set di disponibilità
* Zone di disponibilità
* Bilanciamento del carico
* Funzionalità di alta disponibilità PaaS (Platform as a Service)

### <a name="availability-sets"></a>Set di disponibilità

I set di disponibilità sono un modo per informare Azure che è necessario distribuire macchine virtuali che appartengono allo stesso carico di lavoro dell'applicazione per evitare l'impatto simultaneo da errori hardware e manutenzione pianificata. I set di disponibilità sono costituiti da *domini di aggiornamento* e *domini di errore*.

![Un'illustrazione che mostra tre set di disponibilità. Il primo set presenta un dominio di aggiornamento, il secondo ne presenta due e il terzo nessuno.](../media/AzAvailSets.png)

I domini di aggiornamento garantiscono che un subset dei server dell'applicazione rimanga sempre in esecuzione quando in un data center di Azure è necessario un periodo di inattività degli host delle macchine virtuali per attività di manutenzione. La maggior parte degli aggiornamenti può essere eseguita senza alcun impatto per le macchine virtuali in esecuzione, ma talvolta questo non è possibile. Per garantire che gli aggiornamenti non vengano eseguiti per un intero data center contemporaneamente, il data center di Azure è suddiviso a livello logico in domini di aggiornamento. Quando è necessario applicare all'host un evento di manutenzione come un aggiornamento per le prestazioni o una patch di sicurezza critica, l'aggiornamento viene eseguito in sequenza tramite i domini di aggiornamento. L'applicazione in sequenza degli aggiornamenti tramite i domini di aggiornamento evita che l'intero data center risulti non disponibile durante gli aggiornamenti della piattaforma e l'applicazione di patch.

Mentre i domini di aggiornamento rappresentano una sezione logica del data center, i domini di errore ne rappresentano le sezioni fisiche e garantiscono la diversità dei rack dei server in un set di disponibilità. I domini di errore sono allineati alla separazione fisica dell'hardware condiviso nel data center. Questo include l'alimentazione, il raffreddamento e l'hardware di rete che supporta i server fisici nei rack. Se l'hardware che supporta un rack di server diventa non disponibile, solo tale rack di server è interessato dall'interruzione. Se si includono le macchine virtuali in un set di disponibilità, tali macchine vengono distribuite automaticamente su più domini di errore. Pertanto un guasto hardware avrà effetto solo su una parte delle macchine virtuali.

Con i set di disponibilità è possibile garantire che l'applicazione rimanga online se è necessario un evento di manutenzione a impatto elevato o se si verificano errori hardware.

### <a name="availability-zones"></a>Zone di disponibilità

Le zone di disponibilità sono data center fisici in località indipendenti all'interno di un'area geografica, ognuno con sistemi autonomi di alimentazione, raffreddamento e rete. Tenendo conto delle zone di disponibilità al momento della distribuzione delle risorse, è possibile proteggere i carichi di lavoro dalle interruzioni dei data center, mantenendo al tempo stesso la presenza in una determinata area geografica. I servizi come le macchine virtuali sono *servizi di zona* e possono essere distribuiti in zone specifiche all'interno di un'area geografica. Altri servizi sono *servizi con ridondanza della zona* e vengono replicati tra le zone di disponibilità nella specifica area di Azure. Entrambi i tipi garantiscono che in un'area di Azure non siano presenti singoli punti di guasto.

![Un'illustrazione che mostra tre zone di disponibilità all'interno di un'area di Azure. Ogni zona di disponibilità ha un proprio set di risorse ed è connessa con le altre per la replica dei servizi con ridondanza della zona.](../media/AzAvailZones.png)

Le aree supportate contengono almeno tre zone di disponibilità. Quando si creano le risorse di un servizio di zona in tali aree, è possibile selezionare la zona in cui deve essere creata la risorsa. Questo consente di progettare l'applicazione in modo che tolleri un'interruzione di zona e continui a funzionare in un'area di Azure, prima che sia necessario spostare l'applicazione in un'altra area di Azure.

Le zone di disponibilità sono un servizio di configurazione della disponibilità elevata più recente per le aree di Azure e sono attualmente disponibili in determinate aree geografiche. Se si intende usare questa funzionalità, è importante verificare la disponibilità di questo servizio nell'area in cui si prevede di distribuire l'applicazione. Le zone di disponibilità sono supportate quando si usano macchine virtuali e diversi servizi PaaS. Le zone di disponibilità si escludono a vicenda con i set di disponibilità. Quando si usano le zone di disponibilità non è più necessario definire un set di disponibilità per i sistemi. Sono disponibili strutture diversificate a livello di data center e gli aggiornamenti non vengono mai eseguiti su più zone di disponibilità nello stesso momento.

### <a name="load-balancing"></a>Bilanciamento del carico

I servizi di bilanciamento del carico consentono di gestire la distribuzione del traffico di rete per un'applicazione. I servizi di bilanciamento del carico sono fondamentali per mantenere l'applicazione resiliente agli errori dei singoli componenti e garantire che sia disponibile per l'elaborazione delle richieste. Per le applicazioni che non dispongono di un servizio di individuazione incorporato, il bilanciamento del carico è necessario sia per i set di disponibilità che per le zone di disponibilità.

Azure dispone di tre servizi con tecnologie di bilanciamento del carico, con diverse capacità per l'instradamento del traffico di rete:

* **Gestione traffico di Azure** offre il bilanciamento del carico DNS globale. È consigliabile prendere in considerazione il servizio Gestione traffico per rendere disponibile il bilanciamento del carico degli endpoint DNS all'interno delle aree di Azure o tra le aree stesse. Gestione traffico distribuirà le richieste agli endpoint disponibili e userà il monitoraggio degli endpoint per rilevare e rimuovere dal carico gli endpoint con esito negativo.
* Il servizio **Gateway applicazione di Azure** offre funzionalità di bilanciamento del carico di livello 7, tra cui la distribuzione round robin del traffico in ingresso, l'affinità di sessione basata su cookie, il routing basato su percorso URL e la possibilità di ospitare più siti Web dietro un unico gateway applicazione. Per impostazione predefinita, il gateway applicazione monitora l'integrità di tutte le risorse nel pool back-end e rimuove automaticamente dal pool le risorse considerate non integre. Il gateway applicazione continua a monitorare le istanze non integre e le aggiunge di nuovo al pool back-end integro, dopo che sono diventate disponibili e rispondono ai probe di integrità.
* **Azure Load Balancer** è un servizio di bilanciamento del carico di livello 4. Consente di configurare endpoint pubblici e interni con carico bilanciato e di definire regole per mappare le connessioni in ingresso a destinazioni pool back-end con opzioni di probe dell'integrità TCP e HTTP per gestire la disponibilità del servizio.

L'uso di una delle tecnologie di bilanciamento del carico di Azure o di una combinazione delle stesse può offrire le opzioni necessarie per progettare una soluzione a disponibilità elevata per l'instradamento del traffico di rete tramite l'applicazione.

![Opzioni di bilanciamento del carico di Azure. Un'illustrazione che mostra la differente tecnologia di bilanciamento del carico in Azure. Gestione traffico bilancia il carico tra due aree. All'interno di ogni area è presente un gateway applicazione che consente di distribuire il carico tra diverse macchine virtuali nel livello Web in base al tipo di richiesta. Tutte le richieste di immagini passano al pool di server delle immagini, mentre qualsiasi altra richiesta viene indirizzata al pool di server predefinito. Ulteriori richieste provenienti dal pool di server predefinito vengono gestite dal servizio di bilanciamento del carico di Azure per essere distribuite tra le macchine virtuali nel livello database.](../media/AzLBOptions.png)

### <a name="paas-ha-capabilities"></a>Funzionalità PaaS per la disponibilità elevata

I servizi PaaS dispongono di funzionalità integrate per la disponibilità elevata. Servizi come il database SQL di Azure, il servizio app di Azure e il bus di servizio di Azure includono funzionalità per la disponibilità elevata e consentono all'applicazione di gestire in modo trasparente gli errori di un singolo componente del servizio. L'uso dei servizi PaaS è uno dei modi migliori per strutturare un'architettura a disponibilità elevata.

Quando si progetta per la disponibilità elevata, è importante comprendere il contratto di servizio che ci si impegna a rispettare per i clienti. È quindi necessario valutare le funzionalità per la disponibilità elevata dell'applicazione e le funzionalità per la disponibilità elevata e i contratti di servizio dei sistemi dipendenti. Dopo aver identificato questi elementi, è possibile usare funzionalità di Azure come i set di disponibilità, le zone di disponibilità e varie tecnologie di bilanciamento del carico per aggiungere all'applicazione funzionalità per la disponibilità elevata. Qualsiasi servizio PaaS si scelga di usare avrà funzionalità integrate per la disponibilità elevata.
