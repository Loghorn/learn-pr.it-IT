Ora che gli ambienti informatici passano da data center controllati dal cliente a data center nel cloud, anche la responsabilità della sicurezza cambia. La sicurezza a questo punto è essenziale condivisi sia dai clienti e fornitori di servizi cloud. Per ogni applicazione e la soluzione, è importante comprendere che cos'è responsabilità del cliente e quali Azure gestirà automaticamente. 

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvj]

## <a name="share-security-responsibility-with-azure"></a>Condividono la responsabilità di sicurezza con Azure

Lo spostamento prima è dai Data Center locale all'infrastruttura come servizio (IaaS). Con IaaS, sfruttando il servizio di livello più basso si che chiede di Azure per creare le reti virtuali e macchine virtuali (VM). A questo livello, è comunque responsabilità dell'utente per applicare patch e proteggere i sistemi operativi e software, nonché configurare la rete per essere protetti. Spedizioni di Contoso è sfruttando i vantaggi delle soluzioni IaaS quando si inizia a usare macchine virtuali di Azure anziché i server fisici in locale. Oltre ai vantaggi operativi, l'utente riceve il vantaggio di sicurezza di presenza in outsourcing problema tramite la protezione di parti della rete fisiche.

Passaggio alla piattaforma come servizio (PaaS) affida numerosi problemi di sicurezza. A questo livello, Azure si sta occupando del sistema operativo e del software fondamentale, ad esempio sistemi di gestione di database. Tutto ciò che viene aggiornato con le ultime patch di sicurezza e può essere integrato con Azure Active Directory per i controlli di accesso. PaaS include anche molti vantaggi operativi. Piuttosto che creare infrastrutture intere e le subnet per gli ambienti manualmente, è possibile "puntare e fare clic su" nel portale di Azure oppure esecuzione automatizzata degli script per portare i sistemi complessi e protetti e ridurre le prestazioni e ridimensionarli in base alle esigenze. Spedizioni di Contoso USA hub eventi di Azure per l'inserimento di dati di telemetria da loro Van. È anche possibile usare un'app web con un back-end di Azure Cosmos DB con le proprie App per dispositivi mobili. Tali servizi sono tutti esempi di PaaS.

Con il software come servizio (SaaS), si affidano quasi tutti gli elementi. SaaS è un software che viene eseguito con un'infrastruttura internet. Il codice è controllato dal fornitore, ma configurato per essere usata dal cliente. Come molte aziende, Contoso Shipping Usa Office 365, che è un ottimo esempio delle soluzioni SaaS.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![shared_responsibility.png](../media-COPIED-FROM-DESIGNFORSECURITY/shared_responsibilities.png)

## <a name="a-layered-approach-to-security"></a>Approccio alla sicurezza strutturato su più livelli

*Difesa avanzata* è una strategia che impiega una serie di meccanismi per rallentare l'avanzamento di un attacco volto ad accedere a informazioni senza autorizzazione. Ogni livello offre protezione in modo che se un livello verrà violato, un livello successivo è già in grado di impedire ulteriori l'esposizione. Microsoft applica un approccio a livelli di sicurezza, nei nostri Data Center fisico e nei servizi di Azure. L'obiettivo di difesa in profondità consiste nel proteggere e impedire che informazioni prevenirne il furto da parte di individui che non sono autorizzati ad accedervi.

La difesa avanzata può essere considerata come una serie di anelli concentrici in cui i dati da proteggere stanno al centro. Ogni anello aggiunge un ulteriore livello di protezione ai dati. Questa strategia annulla la dipendenza da un qualsiasi singolo livello di protezione e opera rallentando l'attacco e fornendo dati di telemetria in base ai quali è possibile intervenire automaticamente o manualmente. Esaminiamo ciascuno di questi livelli.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Difesa in profondità](../media-COPIED-FROM-DESIGNFORSECURITY/defense_in_depth_layers_small.PNG)

### <a name="data"></a>Dati

Nella quasi totalità dei casi, gli utenti malintenzionati cercano:

- Dati archiviati in un database
- Dati archiviati su disco all'interno di macchine virtuali
- Dati archiviati in un'applicazione SaaS come Office 365
- Dati archiviati nel cloud

È responsabilità di chi archivia i dati e di chi ne controlla l'accesso assicurarsi che siano adeguatamente protetti. Spesso, esistono requisiti normativi che determinano i controlli e i processi che devono essere in grado di garantire la riservatezza, integrità e disponibilità dei dati.

### <a name="application"></a>Applicazione

- Verificare che le applicazioni siano protetti e privo di vulnerabilità.
- Store i segreti sensibili dell'applicazione in un supporto di archiviazione sicura.
- Prendere un requisito di progettazione per lo sviluppo di tutte le applicazioni di sicurezza.

L'integrazione della sicurezza nel ciclo di vita dello sviluppo dell'applicazione consentirà di ridurre il numero di vulnerabilità introdotte nel codice. Incoraggiare tutti i team di sviluppo per verificare le proprie applicazioni sono protette per impostazione predefinita e che si desidera apportare i requisiti di sicurezza non negoziabili.

### <a name="compute"></a>Calcolo

- Proteggere l'accesso alle macchine virtuali.
- Implementazione di endpoint protection e mantenere i sistemi con patch e corrente.

Malware, sistemi privi di patch e sistemi protetti in modo non corretto aprono l'ambiente agli attacchi. In questo livello è attiva assicurandosi che le risorse di calcolo sono sicure e di disporre i controlli appropriati in grado di ridurre al minimo i problemi di sicurezza.

### <a name="networking"></a>Rete

- Limitazione delle comunicazioni tra le risorse.
- Nega per impostazione predefinita.
- Limitare l'accesso a internet in ingresso e in uscita, limite laddove appropriato.
- Implementare una connettività sicura da reti locali.

A questo livello, lo stato attivo è sulla limitazione della connettività di rete in tutte le risorse per consentire solo ciò che è obbligatorio. Limitando queste comunicazioni, si riduce il rischio di movimento laterale in tutta la rete.

### <a name="perimeter"></a>Perimetro

- Usare distribuito protezione denial of service (DDoS) per filtrare gli attacchi su larga scala, prima che possano causare un attacco denial of service per gli utenti finali.
- Usare firewall perimetrali per identificare e inviare un avviso per attacchi dannosi rispetto alla rete.

A livello di perimetro della rete, lo scopo è impedire gli attacchi di rete diretti alle risorse. Identificare gli attacchi, eliminarne l'impatto e segnalarli è importante per proteggere la rete.

### <a name="policies--access"></a>Criteri e accesso

- Controllare l'accesso all'infrastruttura e controllo delle modifiche.
- Usare l'autenticazione single sign-on e a più fattori.
- Controllo degli eventi e le modifiche.

Il livello di accesso e criteri è tutto su come verificare che le identità sono sicure, l'accesso viene concesso solo degli elementi necessari e le modifiche vengono registrate.

### <a name="physical-security"></a>Sicurezza fisica

- La sicurezza fisica e controllo dell'accesso all'hardware nel Data Center è la prima linea di difesa.

Sicurezza fisica, l'intento consiste nel fornire protezioni fisiche contro l'accesso agli asset. Ciò garantisce che non sia possibile superare gli altri livelli e che la perdita o il furto vengano gestiti in modo appropriato.

Abbiamo visto qui che Azure aiuta molto con le esigenze di protezione. Ma la sicurezza è comunque una **responsabilità condivise**. Quantità di tale responsabilità rientra in Stati Uniti dipende dal modello di cui è utilizzato con Azure.

Usiamo il *difesa in profondità* anelli come linea guida per prendere in considerazione quali misure sono adeguati per i dati e gli ambienti.