L'hosting delle applicazioni in una piattaforma cloud offre numerosi vantaggi rispetto alle tradizionali distribuzioni locali. Modello di responsabilità condivisa della cloud consente di spostare la protezione di rete fisica, la compilazione e i livelli di host sotto il controllo del provider di servizi cloud. Un utente malintenzionato che tentasse di compromettere la piattaforma a questo livello avrebbe risultati scadenti considerati i notevoli investimenti e studi approfonditi che i provider fanno per proteggere e monitorare la propria infrastruttura.

Per gli utenti malintenzionati è quindi molto più efficace approfittare delle vulnerabilità introdotte a livello di applicazione da parte dei clienti della piattaforma cloud. Inoltre, adottando una piattaforma distribuita come servizio (PaaS) per ospitare le proprie applicazioni, i clienti sono in grado di liberare risorse dalla gestione della sicurezza del sistema operativo per destinarle al rafforzamento del codice dell'applicazione e al monitoraggio del perimetro di identità dell'applicazione. In questa unità verranno illustrati alcuni dei modi per migliorare la sicurezza dell'applicazione in fase di progettazione.

## <a name="scenario"></a>Scenario

I clienti di Lamna Healthcare devono accedere alle cartelle cliniche personali tramite un portale Web online. Conformità con l'Health Insurance Portability e Accountability Act (HIPAA) è obbligatoria e pone l'azienda a rischio di penali se si verifica una violazione dei dati personali. Pertanto, è fondamentale proteggere l'applicazione e interagisce con i dati personali.

Le aree principali che riguardano le applicazioni dei clienti sono:

- Progettazione sicura dell'applicazione
- Sicurezza dei dati
- Gestione delle identità e dell'accesso
- Sicurezza degli endpoint

## <a name="security-development-lifecycle"></a>Security Development Lifecycle

Il processo [Security Development Lifecycle](https://www.microsoft.com/sdl) (SDL) di Microsoft può essere utilizzato durante la fase di progettazione dell'applicazione per assicurarsi che le misure di sicurezza vengano incorporate nel ciclo di vita di sviluppo del software. I problemi di sicurezza e conformità sono molto più semplici da risolvere in fase di progettazione di un'applicazione. Inoltre, in questo modo, è possibile evitare molti errori comuni che possono provocare problemi di sicurezza nel prodotto finale. La risoluzione dei problemi nelle prime fasi di sviluppo del software è anche molto meno costosa. La sequenza tipica delle fasi di un progetto software basato sul processo SDL è la seguente:

1. Formazione

    - Formazione di base in materia di sicurezza

1. Requisiti

    - Definire i requisiti e i controlli di qualità
    - Analizzare i rischi di sicurezza e privacy
 
1. Progettazione

    - Analisi delle superfici di attacco
    - Modellazione delle minacce
 
1. Implementazione

    - Specificare gli strumenti da usare per misurare la qualità del codice
    - Applicare funzioni e API escluso
    - Eseguire l'analisi statica del codice
    - Analizzare i repository per i segreti archiviati
 
1. Verifica

    - Test dinamici/con dati casuali
    - Verificare i modelli delle minacce/le superfici di attacco
 
1. Rilascio

    - Piano di risposta per la sicurezza dei moduli
    - Eseguire un esame finale della sicurezza
    - Rilasciare l'archivio
 
1. Risposta 

    - Eseguire la risposta alle minacce

![Security Development Lifecycle](../media/sdl.png)

Il processo SDL non è solo un processo o un set di strumenti, ma anche una risorsa culturale. Creazione di una cultura in cui la sicurezza è un obiettivo primario e requisito qualsiasi dello sviluppo di applicazioni possa compiere grandi progressi nell'evoluzione delle funzionalità di un'organizzazione termini di sicurezza.

<!-- Bear in mind that the migration of un-modified applications (especially COTS procured software systems) will not be able to perform many of the steps listed above.
 -->

## <a name="operational-security-assessment"></a>Valutazione della sicurezza operativa

Dopo aver distribuita un'applicazione, è essenziale per valutare le condizioni di sicurezza continuamente, determinare la modalità attenuare eventuali problemi che vengono individuati e feed la Knowledge Base nel ciclo di sviluppo software. La profondità a cui questa operazione viene eseguita dipende dal livello di competenza dei team operativi e di sviluppo software, nonché dai requisiti di privacy dei dati.

Per automatizzare questo processo e valutare i problemi di sicurezza con cadenza regolare sono disponibili servizi software per la ricerca delle vulnerabilità, che evitano inoltre di sovraccaricare i team con laboriosi processi manuali, ad esempio i test di penetrazione.

Centro sicurezza di Azure è un servizio gratuito, ora abilitato per impostazione predefinita per tutte le sottoscrizioni di Azure, che è strettamente integrato con altri servizi a livello di applicazione di Azure, ad esempio gateway applicazione di Azure e Web application firewall di Azure. Analizzando i log da questi servizi, Centro sicurezza di AZURE possono segnalare la vulnerabilità note ottenute in tempo reale, consiglia le risposte per ridurre tali rischi e persino essere configurati per eseguire automaticamente i Playbook in risposta agli attacchi.

<!-- SDL culture
Key Vault / MSI
CSE = App  -> DB & App Storage
Mention approach of code scanning & SDL
Scanning for passwords - Git
 -->

## <a name="identity-as-the-perimeter"></a>Identità come perimetro

La verifica dell'identità sta diventando la prima linea di difesa per le applicazioni. Limitare l'accesso a un'applicazione Web tramite l'autenticazione e l'autorizzazione delle sessioni può ridurre drasticamente la superficie di attacco. Azure AD e Azure AD B2C rappresentano un metodo efficace per affidare la responsabilità del controllo dell'identità e degli accessi a un servizio completamente gestito. Criteri di accesso condizionale di Azure AD, la gestione delle identità con privilegi e controlli Identity Protection ulteriormente migliorano la capacità di un cliente per impedire accessi non autorizzati e controllare le modifiche.

## <a name="data-protection"></a>Protezione dei dati

I dati dei clienti sono il bersaglio della maggior parte, se non di tutti, gli attacchi alle applicazioni Web. Mettere in sicurezza l'archiviazione e il trasporto dei dati tra un'applicazione e il suo livello di archiviazione dati è un requisito prioritario.

Lamna Healthcare archivia e gestisce dati clinici particolarmente sensibili dei pazienti. La normativa HIPAA, adottata dal Congresso degli Stati Uniti nel 1996 insieme ad altre norme, definisce gli standard nazionali per la trasmissione elettronica dei dati sanitari da parte degli operatori sanitari e dei datori di lavoro. Lamna assicurarsi pazienti e parti autorizzate, ad esempio i medici, disporre di accesso sicuro ai dati medici.

Per garantire la conformità con questi requisiti, Lamna Healthcare ha modificato la propria applicazione per crittografare tutti i dati dei pazienti quando sono inattivi e in transito. Ad esempio, Transport Layer Security (TLS) viene utilizzato per crittografare i dati che transitano tra l'applicazione Web e i database SQL back-end. I dati vengono anche crittografati a riposo in SQL Server tramite Transparent Data Encryption (TDE), verificare che sia anche se l'ambiente è compromesso, i dati in modo efficace inutili a chiunque senza le chiavi di decrittografia corretta.

Per crittografare i dati archiviati in archiviazione blob, la crittografia lato client è utilizzabile per crittografare i dati in memoria prima che venga scritto nel servizio di archiviazione. Le librerie che supportano questo tipo di crittografia sono disponibili per .NET, Java e Python e consentono l'integrazione della crittografia dei dati direttamente nelle applicazioni per migliorare l'integrità dei dati.

### <a name="secure-key-and-secret-storage"></a>Chiavi di sicurezza e archiviazione segreta

Separare i segreti dell'applicazione, stringhe di connessione, password e così via, e le chiavi di crittografia dall'applicazione usata per accedere ai dati è un accorgimento fondamentale. Le chiavi di crittografia e i segreti dell'applicazione non devono mai essere archiviati nel codice dell'applicazione dei file di configurazione. Bisogna utilizzare, in alternativa, un archivio protetto, ad esempio Azure Key Vault. Accesso ai dati sensibili può essere quindi limitato alle identità di applicazione tramite le identità del servizio gestito e possano essere ruotate a intervalli regolari per limitare l'esposizione nel caso di perdita di chiavi di crittografia. I clienti possono anche scegliere di usare le proprie chiavi di crittografia generate da moduli di protezione locali Hardware (HSM) e di indicare anche che le istanze di Azure Key Vault sono implementate in a tenant singolo, moduli di protezione hardware discreti.

<!-- ### Secure and immutable file storage

All Azure storage accounts are encrypted by default using Microsoft managed keys. Azure customers also have the ability to use their own encryption keys (BYOK) to encrypt blob, file and queue data so that even the hosting provider has no access to unencrypted data. Data immutability is often required for auditing purposes or when legal disputes call for data to be effectively frozen for a determined amount of time. Azure has recently introduced an [immutable data storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) option known as Write-Once, Read many (WORM) for this scenario. -->
