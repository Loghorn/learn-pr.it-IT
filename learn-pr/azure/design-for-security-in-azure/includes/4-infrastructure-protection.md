Di recente Lamna Healthcare ha sperimentato un'interruzione significativa in un'applicazione Web lato clienti. A un tecnico è stato concesso l'accesso completo a un gruppo di risorse contenente l'applicazione Web di produzione. Questa persona ha accidentalmente eliminato il gruppo di risorse e tutte le risorse figlio, incluso il database che ospitava i dati dei clienti in tempo reale. 

Per fortuna, il codice sorgente dell'applicazione e le risorse erano disponibili nel controllo del codice sorgente e venivano eseguiti automaticamente backup periodici in base a una pianificazione. Pertanto il servizio è stato ripristinato in modo relativamente semplice. In questo articolo verrà illustrato il modo in cui questa interruzione avrebbe potuto essere evitata usando le funzionalità in Azure per proteggere l'accesso all'infrastruttura.

## <a name="criticality-of-infrastructure"></a>Criticità dell'infrastruttura

L'infrastruttura cloud sta diventando un elemento fondamentale di molte aziende. È essenziale garantire che persone e processi abbiano solo i diritti strettamente necessari per svolgere il proprio compito. Assegnazione dell'accesso non corretto può comportare la perdita di dati, perdita di dati, o causare indisponibilità dei servizi. 

Gli amministratori di sistema possono essere responsabili di un numero elevato di utenti, i sistemi, e set di autorizzazioni. Di conseguenza, la concessione dell'accesso appropriato può diventare rapidamente ingestibile, traducendosi nell'applicazione di un approccio unico in tutti i casi. Questo approccio può ridurre la complessità dell'amministrazione, ma rende molto più facile concedere involontariamente un accesso più permissivo di quanto necessario.

## <a name="role-based-access-control"></a>Controllo degli accessi in base al ruolo

Il controllo degli accessi in base al ruolo offre un approccio leggermente diverso. I ruoli sono definiti come raccolte di autorizzazioni di accesso. Le entità di sicurezza vengono mappate ai ruoli direttamente o tramite l'appartenenza a gruppi. La separazione di entità di sicurezza, autorizzazioni di accesso e risorse fornisce una gestione semplificata degli accessi e un controllo più granulare.

In Azure, gli utenti, i gruppi e i ruoli vengono archiviati in Azure Active Directory (Azure AD). L'API di Azure Resource Manager usa il controllo degli accessi in base al ruolo per proteggere la gestione dell'accesso a tutte le risorse all'interno di Azure.

![Accesso basato su ACL](../media-draft/ACL_Based_Access.png)

<!-- ![Role-based access control](../media-draft/Role_Based_Access.png)
 -->

### <a name="roles-and-management-groups"></a>Ruoli e gruppi di gestione

I ruoli sono set di autorizzazioni, ad esempio "Read-only" o "Collaboratore", che è possono concedere agli utenti per accedere a un'istanza di servizio di Azure. I ruoli possono essere concesse a livello di istanza di servizio singoli, ma sono anche fluiscono verso il basso della gerarchia di Azure Resource Manager. I ruoli assegnati in un ambito più elevato, ad esempio un'intera sottoscrizione, vengono ereditati dagli ambiti figlio, come le istanze del servizio. 

I gruppi di gestione sono un ulteriore livello gerarchico recentemente introdotto nel modello di controllo degli accessi in base al ruolo. I gruppi di gestione aggiungono la capacità di raggruppare le sottoscrizioni e applicare criteri a un livello ancora superiore.

La possibilità di propagare i ruoli tramite una gerarchia definita in modo arbitrario consente inoltre agli amministratori di concedere l'accesso temporaneo a un intero ambiente per gli utenti autenticati. Ad esempio, un revisore potrebbe richiedere l'accesso temporaneo in sola lettura a tutte le sottoscrizioni.

![Gruppi di gestione](../media-draft/management_groups.png)

### <a name="privileged-identity-management"></a>Privileged Identity Management

Oltre alla gestione dell'accesso alle risorse di Azure tramite il controllo degli accessi in base al ruolo, per un approccio completo alla protezione dell'infrastruttura è consigliabile includere il controllo continuativo dei membri dei ruoli man mano che l'organizzazione cambia e si evolve. Azure AD Privileged Identity Management (PIM) è un'offerta a pagamento aggiuntiva che fornisce supervisione delle assegnazioni di ruolo, funzionalità self-service, attivazione dei ruoli just-in-time e verifiche di accesso alle risorse di Azure AD e Azure.

![Privileged Identity Management](../media-draft/PIM_Dashboard.png)

## <a name="providing-identities-to-services"></a>Assegnazione di identità ai servizi

Spesso è utile assegnare ai servizi un'identità. Di frequente, e in contrasto con le procedure consigliate, le informazioni relative alle credenziali vengono incorporate nei file di configurazione. Se questi file di configurazione non sono protetti, chiunque disponga di accesso ai sistemi o ai repository può accedere alle credenziali e rischiare l'esposizione.

Windows Azure risolve questo problema sono disponibili due metodi: identità gestita per servizi di Azure e le entità servizio.

### <a name="service-principals"></a>Entità servizio

Per comprendere le entità servizio, è utile per capire le parole **identity** e **dell'entità** perché sono usati nell'ambiente di gestione di identità.

Un'**identità** è semplicemente un elemento che può essere autenticato. Ovviamente sono inclusi gli utenti con nome utente e password, ma può includere anche le applicazioni o altri server, che potrebbe eseguire l'autenticazione con certificati o chiavi private. Un **account** rappresenta i dati associati a un'identità.

Un'**entità di sicurezza** è un'identità che agisce con determinati ruoli o attestazioni. Spesso non è utile considerare identità ed entità di sicurezza separatamente. Si pensi all'uso di "sudo" in un prompt di Bash o al comando "Esegui come amministratore" di Windows. In entrambi i casi, è ancora connessi come la stessa identità, ma è stato modificato il ruolo in cui si sta eseguendo.

Pertanto, un'**entità servizio** è un'identità che viene usata da un servizio o un'applicazione. Come altre identità, può essere assegnato i ruoli. 

Ad esempio, Lamna Healthcare può assegnare gli script di distribuzione in modo che vengano autenticati come entità servizio. Se questo è l'unica identità che dispone dell'autorizzazione per eseguire operazioni distruttive, Lamna uscirà molto oltre il assicurandosi che non hanno una ripetizione del processo di eliminazione accidentale di risorse.

### <a name="managed-identities-for-azure-resources"></a>Identità gestite per le risorse di Azure

La creazione dell'entità servizio può essere un'operazione laboriosa, ed esistono molti punti di tocco che può rendere su come gestirli difficile. Gestire le identità per le risorse di Azure sono molto più semplice e verranno eseguita la maggior parte del lavoro per l'utente.

È possibile creare immediatamente un'identità gestita per qualsiasi servizio di Azure che lo supporta (l'elenco è in continua crescita). Quando si crea un'identità gestita per un servizio, si sta creando un account nel tenant di Azure AD. L'infrastruttura di Azure si occuperà automaticamente di autenticare il servizio e gestire l'account. Si può quindi usare questo account come qualsiasi altro account di Active Directory, incluso permettere in tutta sicurezza che il servizio autenticato acceda ad altre risorse di Azure.

Lamna Healthcare accetta ulteriormente un passaggio di gestione di identità e Usa identità gestite per tutti i servizi supportati che è necessario avere la possibilità di eseguire le distribuzioni e la gestione dell'infrastruttura.

## <a name="infrastructure-protection-at-lamna-healthcare"></a>Protezione dell'infrastruttura in Lamna Healthcare

Abbiamo visto come Lamna Healthcare ha risolto i problemi emersi dall'incidente in cui l'infrastruttura è stata eliminata involontariamente. Si è utilizzato controllo degli accessi in base al ruolo per gestire meglio la sicurezza della propria infrastruttura e usano identità gestite per mantenere le proprie credenziali all'esterno di codice e la facilità di amministrazione delle identità necessari per i propri servizi.

## <a name="summary"></a>Riepilogo

Per garantire la disponibilità e l'integrità dell'infrastruttura, è importante proteggerla adeguatamente. In modo corretto usando funzionalità quali accessi e identità gestite aiuterà a proteggere l'ambiente di Azure da accessi non autorizzati o non intenzionali e migliorerà le funzionalità di sicurezza delle identità nell'architettura.