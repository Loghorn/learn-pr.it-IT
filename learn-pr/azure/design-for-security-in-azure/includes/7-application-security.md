L'hosting delle applicazioni in una piattaforma cloud offre numerosi vantaggi rispetto alle tradizionali distribuzioni locali. Il modello di responsabilità condivisa del cloud porta la sicurezza a livello di rete fisica, edificio e host sotto il controllo del provider di servizi cloud. Un utente malintenzionato che tentasse di compromettere la piattaforma a questo livello vedrebbe rendimenti decrescenti, considerati i notevoli investimenti e studi approfonditi messi in campo dai provider per proteggere e monitorare la propria infrastruttura.

Per gli utenti malintenzionati è quindi molto più conveniente approfittare delle vulnerabilità introdotte a livello di applicazione da parte dei clienti della piattaforma cloud. Inoltre, adottando una piattaforma distribuita come servizio (PaaS) per ospitare le proprie applicazioni, i clienti possono liberare le risorse impegnate nella gestione della sicurezza dei sistemi operativi per destinarle al rafforzamento del codice delle applicazioni e al monitoraggio del perimetro di identità attorno alle applicazioni. In questa unità verranno illustrati alcuni dei modi per migliorare la sicurezza delle applicazioni in fase di progettazione.

## <a name="scenario"></a>Scenario

I clienti di Lamna Healthcare devono accedere alle proprie cartelle cliniche personali tramite un portale Web online. La conformità alla normativa HIPAA (Health Insurance Portability e Accountability Act) è obbligatoria e pone l'azienda a serio rischio di sanzioni pecuniarie in caso di violazione dei dati personali, quindi proteggere l'applicazione e i dati personali che gestisce è un requisito prioritario.

Le aree principali che riguardano le applicazioni dei clienti sono:

- Progettazione sicura dell'applicazione
- Sicurezza dei dati
- Gestione delle identità e dell'accesso
- Sicurezza degli endpoint

## <a name="security-development-lifecycle"></a>Security Development Lifecycle

Durante la fase di progettazione delle applicazioni può essere usato il processo [Security Development Lifecycle](https://www.microsoft.com/sdl) (SDL) di Microsoft per assicurarsi che nel ciclo di sviluppo del software vengano incorporate misure di sicurezza. I problemi di sicurezza e conformità sono molto più semplici da risolvere in fase di progettazione di un'applicazione. Inoltre, in questo modo, è possibile evitare molti errori comuni che possono introdurre vulnerabilità nel prodotto finale. La risoluzione dei problemi nelle prime fasi di sviluppo del software è anche molto meno costosa. La sequenza tipica delle fasi di un progetto software basato sul processo SDL è la seguente:

1. Formazione

    - Formazione di base in materia di sicurezza

1. Requisiti

    - Definire i requisiti e i controlli di qualità
    - Analizzare i rischi di sicurezza e privacy
 
1. Progettazione

    - Analisi delle superfici di attacco
    - Modellazione delle minacce
 
1. Implementazione

    - Specificare gli strumenti che consentono di misurare la qualità del codice
    - Definire API e funzioni vietate
    - Eseguire l'analisi statica del codice
    - Analizzare i repository per individuare segreti archiviati
 
1. Verifica

    - Test dinamici/con dati casuali
    - Verificare i modelli delle minacce/la superficie di attacco
 
1. Rilascio

    - Definire il piano di risposta di sicurezza
    - Eseguire un esame finale della sicurezza
    - Rilasciare l'archivio
 
1. Risposta 

    - Eseguire la risposta alle minacce

![Figura che mostra Security Development Lifecycle](../media/sdl.png)

Security Development Lifecycle non è solo un processo o un set di strumenti, ma anche un aspetto culturale. Creare una cultura in cui la sicurezza è considerata un obiettivo primario e un requisito base dello sviluppo di qualsiasi applicazione può far evolvere enormemente la capacità delle aziende di gestire la sicurezza.

<!-- Bear in mind that the migration of un-modified applications (especially COTS procured software systems) will not be able to perform many of the steps listed above.
 -->

## <a name="operational-security-assessment"></a>Valutazione della sicurezza operativa

Dopo aver distribuito un'applicazione, è essenziale valutarne continuamente il comportamento di sicurezza, stabilire come attenuare gli eventuali problemi individuati e reintrodurre le informazioni acquisite nel ciclo di sviluppo del software. La profondità a cui questa operazione viene eseguita dipende dal livello di maturità dei team operativi e di sviluppo software, nonché dai requisiti di privacy dei dati.

Per automatizzare questo processo e valutare i problemi di sicurezza con cadenza regolare sono disponibili servizi software per l'analisi delle vulnerabilità, che evitano inoltre di sovraccaricare i team con laboriosi processi manuali, ad esempio i test di penetrazione.

Il Centro sicurezza di Azure è un servizio gratuito, ora abilitato per impostazione predefinita per tutte le sottoscrizioni di Azure, che è strettamente integrato con altri servizi a livello di applicazione di Azure, ad esempio gateway applicazione di Azure e Web application firewall di Azure. Analizzando i log creati da questi servizi, il Centro sicurezza di Azure può segnalare le vulnerabilità note in tempo reale, consigliare interventi per ridurre i rischi e persino essere configurato per eseguire automaticamente dei playbook in risposta agli attacchi.

<!-- SDL culture
Key Vault / MSI
CSE = App  -> DB & App Storage
Mention approach of code scanning & SDL
Scanning for passwords - Git
 -->

## <a name="identity-as-the-perimeter"></a>Identità come perimetro

La convalida dell'identità sta diventando la prima linea di difesa per le applicazioni. Limitare l'accesso a un'applicazione Web tramite l'autenticazione e l'autorizzazione delle sessioni può ridurre drasticamente la superficie di attacco. Azure AD e Azure AD B2C rappresentano un metodo efficace per affidare la responsabilità del controllo dell'identità e dell'accesso a un servizio completamente gestito. I criteri di accesso condizionale di Azure AD, la gestione delle identità con privilegi e i controlli di Identity Protection migliorano ulteriormente la capacità dei clienti di impedire accessi non autorizzati e controllare le modifiche.

## <a name="data-protection"></a>Protezione dati

I dati dei clienti sono il bersaglio della maggior parte, se non di tutti, gli attacchi alle applicazioni Web. Mettere in sicurezza l'archiviazione e il trasporto dei dati tra un'applicazione e il suo livello di archiviazione dati è un requisito prioritario.

Lamna Healthcare archivia e gestisce dati clinici particolarmente sensibili relativi ai pazienti. La normativa HIPAA, adottata dal Congresso degli Stati Uniti nel 1996, definisce, insieme ad altri controlli, gli standard nazionali per le transazioni elettroniche in ambito sanitario da parte degli operatori sanitari e dei datori di lavoro. Lamna deve garantire ai pazienti e ad altre parti autorizzate, ad esempio i medici, un accesso protetto ai dati clinici.

Per garantire la conformità a questi requisiti, Lamna Healthcare ha modificato le proprie applicazioni, che ora crittografano tutti i dati dei pazienti, inattivi e in transito. Ad esempio, viene usato il protocollo Transport Layer Security (TLS) per crittografare i dati scambiati tra l'applicazione Web e i database SQL back-end. Vengono crittografati anche i dati inattivi in SQL Server tramite la tecnologia Transparent Data Encryption (TDE) per garantire che, se l'ambiente viene compromesso, i dati risultino inutilizzabili per chiunque non possieda le chiavi di decrittografia corrette.

Per crittografare i dati archiviati in archivi BLOB, è possibile usare la crittografia lato client per crittografare i dati in memoria prima che vengano scritti nel servizio di archiviazione. Sono disponibili librerie che supportano questo tipo di crittografia per .NET, Java e Python e consentono l'integrazione della crittografia dei dati direttamente nelle applicazioni, per migliorare l'integrità dei dati.

### <a name="secure-key-and-secret-storage"></a>Chiavi di sicurezza e archiviazione segreta

È essenziale separare i segreti dell'applicazione (stringhe di connessione, password e così via) e le chiavi di crittografia dall'applicazione usata per accedere ai dati. Le chiavi di crittografia e i segreti dell'applicazione non devono mai essere scritti nel codice dell'applicazione dei file di configurazione. Bisogna usare invece un archivio protetto, ad esempio Azure Key Vault. L'accesso ai dati sensibili può quindi essere limitato alle identità dell'applicazione tramite le identità del servizio gestito e le chiavi possano essere ruotate a intervalli regolari per limitare l'esposizione nel caso di divulgazione delle chiavi di crittografia. I clienti possono anche scegliere di usare chiavi di crittografia proprie, generate da moduli di protezione hardware locali, e persino decidere che le istanze di Azure Key Vault siano implementate in moduli di protezione hardware discreti a tenant singolo.

<!-- ### Secure and immutable file storage

All Azure storage accounts are encrypted by default using Microsoft managed keys. Azure customers also have the ability to use their own encryption keys (BYOK) to encrypt blob, file and queue data so that even the hosting provider has no access to unencrypted data. Data immutability is often required for auditing purposes or when legal disputes call for data to be effectively frozen for a determined amount of time. Azure has recently introduced an [immutable data storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) option known as Write-Once, Read many (WORM) for this scenario. -->
