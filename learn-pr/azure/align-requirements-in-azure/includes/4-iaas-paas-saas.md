Le risorse di calcolo del cloud vengono distribuite con tre modelli di servizio diverso.

- **IaaS (Infrastructure-as-a-service)** offre un'infrastruttura di calcolo di accesso immediato di cui è possibile effettuare il provisioning e che può essere gestita tramite Internet.
- **PaaS (Platform-as-a-service)** offre ambienti di sviluppo e distribuzione pronti che è possibile usare per fornire servizi cloud personalizzati.
- **SaaS (Software-as-a-service)** offre applicazioni tramite Internet come servizio basato sul Web.

Per la scelta di un modello di servizio, valutare quale parte deve essere responsabile per la risorsa di calcolo. In base allo scenario, è possibile decidere quante responsabilità di gestione condivise si vogliono assumere.

![Modello di responsabilità condivisa](../media/3-shared-responsibility.png)

## <a name="iaas"></a>IaaS

IaaS (Infrastructure-as-a-service) è un'infrastruttura di calcolo di accesso immediato di cui è viene effettuato il provisioning e che viene gestita tramite Internet. IaaS consente di ridimensionare rapidamente le risorse per soddisfare la domanda e pagare solo per le risorse usate. IaaS consente di evitare i costi e le complicazioni legati all'acquisto e alla gestione di server fisici propri e di altri elementi dell'infrastruttura del data center. Ogni risorsa viene offerta come un componente del servizio separato e si *noleggiano* le risorse fino a quando servono. Di conseguenza, il modello IaaS è molto flessibile. È possibile eseguire il provisioning dell'infrastruttura comune, ad esempio macchine virtuali, archiviazione, subnet virtuali, firewall, e VPN per creare una soluzione. Non è necessario gestire le appliance e i server fisici. Tuttavia, si è responsabili direttamente della configurazione e della gestione dei componenti. Ad esempio, la configurazione dei firewall, l'aggiornamento del sistema operativo delle macchine virtuali, l'aggiornamento dei sistemi DBMS e dei runtime.

### <a name="common-scenarios"></a>Scenari comuni 

Si supponga che la propria società che opera nel settore sanitario abbia la necessità di eseguire una versione speciale di software desktop. Il software è supportato solo in una versione specifica di un sistema operativo e sono necessari un solo utente e una licenza. È possibile creare una macchina virtuale con il software richiesto. L'utente può usare desktop remoto per connettersi alla macchina virtuale per usare il software.

Si consideri un altro scenario. I team di sviluppo devono avere a disposizione vari ambienti di sviluppo univoci. Durante il ciclo di sviluppo, hanno la necessità di testare diverse versioni del prodotto. Gli sviluppatori possono eseguire il provisioning degli ambienti all'occorrenza. Quando un ambiente non è più necessario, può essere eliminato facilmente.

Alcuni altri scenari comuni includono:

**Hosting di siti Web:** se serve maggiore controllo per l'hosting di un sito Web, l'esecuzione di siti Web con il modello IaaS può essere un'opzione migliore rispetto all'hosting Web tradizionale.

**App Web:** IaaS fornisce tutta l'infrastruttura per supportare le app Web, tra cui archiviazione, server Web e server applicazioni e risorse di rete. Le organizzazioni possono distribuire rapidamente app Web con il modello IaaS e ridimensionare facilmente l'infrastruttura quando la richiesta per le app è imprevedibile.

**Archiviazione, backup e ripristino:** la gestione dell'archiviazione può essere complessa e richiedere notevoli investimenti di capitale e personale esperto per la gestione dei dati e per soddisfare i requisiti legali e di conformità. IaaS può essere utile per semplificare la pianificazione, la gestione, richieste imprevedibili ed esigenze di archiviazione in continua crescita.

**Calcoli ad alte prestazioni:** in presenza di un carico di lavoro che richiede calcoli ad alte prestazioni, è possibile eseguire il carico di lavoro nel cloud evitando di anticipare costi per l'hardware e pagando solo a consumo quando necessario. 

**Analisi dei big data:** con set di dati di grandi dimensioni che contengono modelli, tendenze e associazioni potenzialmente preziosi, IaaS può offrire la potenza di elaborazione per eseguire il data mining nei set di dati per individuare i modelli.

### <a name="advantages"></a>Vantaggi

**Elimina le spese in conto capitale e riduce i costi correnti:** IaaS consente di evitare la spesa iniziale per la configurazione e la gestione di un data center locale e rappresenta quindi un'opzione conveniente per le startup e le aziende che vogliono mettere alla prova idee innovative. Non appena si decide di lanciare un nuovo prodotto o una nuova iniziativa, l'infrastruttura di calcolo necessaria può essere pronta in minuti o ore, anziché richiedere giorni o settimane o persino mesi, come nel caso di una configurazione interna.

**Migliora la continuità aziendale e il ripristino di emergenza :** implementare soluzioni per disponibilità elevata, continuità aziendale e ripristino di emergenza è costoso, perché richiede una quantità significativa di tecnologia e personale. Ma con un contratto di servizio (SLA) appropriato, IaaS può ridurre questi costi e consentire l'accesso alle applicazioni e ai dati come di consueto durante un'emergenza o un'interruzione del servizio.

**Rispondere più velocemente a condizioni aziendali mutevoli:** IaaS consente di aumentare rapidamente le risorse per rispondere a picchi della domanda per l'applicazione, ad esempio durante un periodo festivo, quindi ridurle nuovamente quando l'attività diminuisce per risparmiare. Dato che non è necessario configurare l'infrastruttura prima di poter sviluppare e distribuire le app, è possibile renderle disponibili più rapidamente agli utenti con IaaS.

**Aumentare stabilità, affidabilità e supportabilità:** con IaaS non vi è alcuna necessità di gestire e aggiornare software e hardware o risolvere i problemi delle apparecchiature. Con il contratto appropriato, il provider di servizi garantisce che l'infrastruttura sia affidabile e che soddisfi i contratti di servizio.

## <a name="paas"></a>PaaS

PaaS (Platform-as-a-service) è un ambiente di sviluppo e distribuzione completo nel cloud. Con PaaS è possibile sviluppare e distribuire semplici app basate sul cloud fino ad applicazioni aziendali sofisticate abilitate per il cloud. Le risorse vengono acquistate da un provider di servizi cloud con pagamento a consumo e sono accessibili tramite una connessione Internet sicura. Come IaaS, un ambiente PaaS include l'infrastruttura, ad esempio i server, l'archiviazione e la rete. Include inoltre il middleware, gli strumenti di sviluppo e altri servizi. PaaS supporta il ciclo di vita completo delle applicazioni Web: sviluppo, test, distribuzione, gestione e aggiornamento. Con PaaS non occorre gestire le licenze software, il middleware e l'infrastruttura dei servizi. Rimane invece necessario gestire le applicazioni e i servizi sviluppati, mentre la gestione di tutto il resto è in genere a carico del provider dei servizi cloud.

### <a name="common-scenarios"></a>Scenari comuni

Si immagini che la propria società che opera nel settore sanitario voglia realizzare un sito Web per descrivere un prodotto. Gli sviluppatori vogliono usare PHP. Con PaaS, gli sviluppatori hanno la possibilità di *creare un'app Web*. I dettagli dell'infrastruttura, ad esempio la creazione di una macchina virtuale, l'installazione di un server Web e l'installazione del middleware non rientrano tra le attività da completare. Non è necessario preoccuparsi del sistema operativo o del tipo di hardware fisico richiesti. Gli sviluppatori distribuiscono i file del sito Web nel cloud e il sito Web è disponibile su Internet.

Si consideri un altro scenario. L'azienda necessita di un database SQL per supportare gli analisti dei dati per un progetto speciale. Non si dispone dell'infrastruttura per soddisfare la richiesta. È possibile eseguire rapidamente il provisioning di un server SQL nel cloud che soddisfa le esigenze del progetto. Gli analisti dei dati possono connettersi al server oppure Il database di SQL Server viene fornito come servizio. Pertanto, non occorre preoccuparsi di aggiornamenti, patch di sicurezza o dell'ottimizzazione dello spazio di archiviazione fisico per letture e scritture.

Alcuni altri scenari comuni includono:

**Framework di sviluppo:** PaaS fornisce un framework su cui si possono basare gli sviluppatori per sviluppare o personalizzare applicazioni basate sul cloud. Analogamente al modo in cui si crea una macro di Excel, PaaS consente agli sviluppatori di creare applicazioni usando componenti software predefiniti. Le funzionalità del cloud, ad esempio scalabilità, disponibilità elevata e multi-tenant sono incluse e si riduce così la quantità di codice che gli sviluppatori devono scrivere.

**Analisi o business intelligence:** gli strumenti di analisi forniti come servizio consentono di analizzare i dati ed eseguire operazioni di data mining. Le organizzazioni possono trovare informazioni dettagliate e modelli per stimare i risultati per migliorare le previsioni, le decisioni per la progettazione dei prodotti, il ritorno sugli investimenti e altre decisioni aziendali.

### <a name="advantages"></a>Vantaggi

Offrendo un'infrastruttura distribuita come servizio, PaaS garantisce vantaggi simili a quelli del modello IaaS. Ma le funzionalità aggiuntive, tra cui middleware, strumenti di sviluppo e altri strumenti aziendali, garantiscono ulteriori vantaggi:

**Riduzione dei tempi di sviluppo:** gli strumenti di sviluppo PaaS possono ridurre i tempi di sviluppo per le nuove applicazioni. Gli sviluppatori possono usare componenti applicativi pre-codificati integrati nella piattaforma, ad esempio flusso di lavoro, servizi di directory, funzionalità di sicurezza e ricerca. I componenti PaaS possono offrire nuove potenzialità al team di sviluppo senza dover assumere personale con le competenze necessarie.

**Sviluppo per più piattaforme:** alcuni provider di servizi offrono opzioni di sviluppo per più piattaforme, ad esempio desktop, dispositivi mobili e browser, rendendo lo sviluppo di app multipiattaforma più veloce e più semplice.

**Uso conveniente di strumenti sofisticati:** grazie al modello di pagamento a consumo, sia i singoli che le organizzazioni hanno la possibilità di usare strumenti avanzati di sviluppo, business intelligence e analisi, che non potrebbero permettersi di acquistare.

**Supporto di team di sviluppo distribuiti geograficamente:** dato che si accede all'ambiente di sviluppo tramite Internet, i team di sviluppo possono collaborare ai progetti anche quando i membri del team si trovano in località remote.

**Gestione efficiente del ciclo di vita delle applicazioni:** PaaS offre tutte le funzionalità necessarie per supportare il ciclo di vita completo delle applicazioni Web, ovvero sviluppo, test, distribuzione, gestione e aggiornamento all'interno dello stesso ambiente integrato.

## <a name="saas"></a>SaaS

Il modello SaaS (Software-as-a-service) consente agli utenti di connettersi ad app basate sul cloud ed usarle tramite Internet. Esempi comuni sono gli strumenti di posta elettronica, per il calendario e per l'ufficio, come Microsoft Office 365. SaaS offre una soluzione software completa che è possibile acquistare con pagamento al consumo da un provider di servizi cloud. Si *noleggia* l'uso di un'applicazione per l'organizzazione. Gli utenti si connettono al servizio tramite Internet, in genere con un Web browser. Tutta l'infrastruttura sottostante, il middleware, il software delle app e i dati delle app si trovano nel data center del provider di servizi. Il provider di servizi gestisce hardware e software e, con un contratto di servizio appropriato, garantirà la disponibilità e la sicurezza dell'app e anche dei dati. Le soluzioni SaaS consentono all'organizzazione di essere rapidamente operativa con un'app con costi iniziali minimi.

Gli utenti che hanno usato un servizio di posta elettronica basato sul Web, come Outlook, Hotmail o Yahoo! Mail, hanno già usato un tipo di soluzione SaaS. Con questi servizi, si accede all'account personale tramite Internet, spesso da un Web browser. Il software di posta elettronica si trova nella rete del provider di servizi e anche i messaggi vengono archiviati in tale rete. È possibile accedere alla posta elettronica e ai messaggi archiviati da un Web browser su qualsiasi computer o dispositivo connesso a Internet.

### <a name="common-scenarios"></a>Scenari comuni

Si immagini che una società che opera nel settore sanitario abbia necessità di una soluzione CRM (Customer Relationship Management) per il team di vendita. Il team è globale. È possibile usare un provider CRM SaaS per implementare rapidamente una soluzione per il team di vendita dell'organizzazione.

Per l'uso nell'organizzazione, è possibile noleggiare app per la produttività, ad esempio posta elettronica, collaborazione e calendario, e applicazioni aziendali sofisticate, ad esempio CRM, ERP (Enterprise Resource Planning) e per la gestione dei documenti. Si paga per l'uso di queste app tramite abbonamenti o in base al livello di utilizzo.

### <a name="advantages"></a>Vantaggi

**Ottenere l'accesso ad applicazioni sofisticate:** per fornire app SaaS agli utenti, non occorre acquistare, installare, aggiornare o gestire qualsiasi hardware, middleware o software. Grazie al modello SaaS, anche le applicazioni aziendali più avanzate come soluzioni ERP e CRM, diventano abbordabili per le organizzazioni che non dispongono delle risorse per acquistare, distribuire e gestire l'infrastruttura e il software necessari autonomamente.
Si paga solo per le risorse usate. È anche possibile risparmiare, perché il servizio SaaS aumenta o riduce le risorse automaticamente in base al livello di utilizzo.

**Usare software client gratuito:** gli utenti possono eseguire la maggior parte delle App SaaS direttamente dal Web browser senza dover scaricare e installare software. Non è necessario acquistare o distribuire software client per gli utenti.

**Mobilità della forza lavoro in modo semplice:** gli utenti possono accedere alle app SaaS e ai dati da qualsiasi computer o dispositivo mobile connesso a Internet. Il provider di servizi è responsabile della fornitura del servizio ai dispositivi.

**Accedere ai dati delle app ovunque:** con i dati archiviati nel cloud, gli utenti possono accedere alle informazioni da qualsiasi computer o dispositivo mobile connesso a Internet. E quando i dati delle app sono archiviati nel cloud, non si verificano perdite di dati in caso di malfunzionamento del computer o del dispositivo di un utente.