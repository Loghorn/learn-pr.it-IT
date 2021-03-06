Di recente Lamna Healthcare ha sperimentato un'interruzione significativa in un'applicazione Web lato clienti. A un tecnico è stato concesso l'accesso completo a un gruppo di risorse contenente l'applicazione Web di produzione. Questa persona ha accidentalmente eliminato il gruppo di risorse e tutte le risorse figlio, incluso il database che ospitava i dati dei clienti in tempo reale. 

Per fortuna, il codice sorgente dell'applicazione e le risorse erano disponibili nel controllo del codice sorgente e venivano eseguiti automaticamente backup periodici in base a una pianificazione. Pertanto il servizio è stato ripristinato in modo relativamente semplice. In questo articolo verrà illustrato il modo in cui questa interruzione avrebbe potuto essere evitata usando le funzionalità in Azure per proteggere l'accesso all'infrastruttura.

## <a name="criticality-of-infrastructure"></a>Criticità dell'infrastruttura

L'infrastruttura cloud sta diventando un elemento fondamentale di molte aziende. È essenziale assicurarsi che persone e processi abbiano solo i diritti strettamente necessari per svolgere il proprio compito. L'assegnazione di un accesso non corretto può comportare perdita o divulgazione accidentale di dati o indisponibilità di servizi. 

Gli amministratori di sistema possono essere responsabili di un numero elevato di utenti, sistemi e set di autorizzazioni. Di conseguenza, la concessione dell'accesso appropriato può diventare rapidamente ingestibile, traducendosi nell'applicazione di un approccio unico in tutti i casi. Questo approccio può ridurre la complessità dell'amministrazione, ma rende molto più facile concedere involontariamente un accesso più permissivo di quanto necessario.

## <a name="role-based-access-control"></a>Controllo degli accessi in base al ruolo

Il controllo degli accessi in base al ruolo offre un approccio leggermente diverso. I ruoli sono definiti come raccolte di autorizzazioni di accesso. Le entità di sicurezza vengono mappate ai ruoli direttamente o tramite l'appartenenza a gruppi. La separazione di entità di sicurezza, autorizzazioni di accesso e risorse fornisce una gestione semplificata degli accessi e un controllo più granulare.

In Azure, gli utenti, i gruppi e i ruoli vengono archiviati in Azure Active Directory (Azure AD). L'API di Azure Resource Manager usa il controllo degli accessi in base al ruolo per proteggere la gestione dell'accesso a tutte le risorse all'interno di Azure.

![Figura che mostra l'implementazione del controllo degli accessi in base al ruolo.](../media/ACL_Based_Access.png)

<!-- ![Role-based access control](../media/Role_Based_Access.png)
 -->

### <a name="roles-and-management-groups"></a>Ruoli e gruppi di gestione

I ruoli sono set di autorizzazioni, ad esempio "Sola lettura" o "Collaboratore", che è possibile concedere agli utenti per l'accesso a un'istanza di un servizio di Azure. I ruoli possono essere concessi a livello di singola istanza del servizio, ma vengono anche propagati verso il basso nella gerarchia di Azure Resource Manager. I ruoli assegnati in un ambito più elevato, ad esempio un'intera sottoscrizione, vengono ereditati dagli ambiti figlio, come le istanze del servizio. 

I gruppi di gestione sono un ulteriore livello gerarchico recentemente introdotto nel modello di controllo degli accessi in base al ruolo. I gruppi di gestione aggiungono la capacità di raggruppare le sottoscrizioni e applicare criteri a un livello ancora superiore.

La possibilità di propagare i ruoli tramite una gerarchia definita in modo arbitrario consente inoltre agli amministratori di concedere l'accesso temporaneo a un intero ambiente per gli utenti autenticati. Ad esempio, un revisore potrebbe richiedere l'accesso temporaneo in sola lettura a tutte le sottoscrizioni.

![Figura che mostra la rappresentazione gerarchica dell'accesso in base al ruolo in un gruppo di gestione.](../media/management_groups.png)

### <a name="privileged-identity-management"></a>Privileged Identity Management

Oltre alla gestione dell'accesso alle risorse di Azure tramite il controllo degli accessi in base al ruolo, per un approccio completo alla protezione dell'infrastruttura è consigliabile includere il controllo continuativo dei membri dei ruoli man mano che l'organizzazione cambia e si evolve. Azure AD Privileged Identity Management (PIM) è un'offerta a pagamento aggiuntiva che fornisce supervisione delle assegnazioni di ruolo, funzionalità self-service, attivazione dei ruoli JIT e verifiche di accesso alle risorse di Azure AD e Azure.

![Screenshot del dashboard di Privileged Identity Management](../media/PIM_Dashboard.png)

## <a name="providing-identities-to-services"></a>Assegnazione di identità ai servizi

Spesso è utile assegnare ai servizi un'identità. Di frequente, e in contrasto con le procedure consigliate, le informazioni relative alle credenziali vengono incorporate nei file di configurazione. Se questi file di configurazione non sono protetti, chiunque abbia accesso ai sistemi o agli archivi può accedere alle credenziali e rischiare l'esposizione.

Azure AD risolve questo problema con due metodi: entità servizio e identità gestite per servizi di Azure.

### <a name="service-principals"></a>Entità servizio

Per comprendere le entità servizio, è utile comprendere prima di tutto i termini **identità** ed **entità di sicurezza** usati nell'ambito della gestione delle identità.

Un'**identità** è semplicemente un elemento che può essere autenticato. Ovviamente questo include gli utenti con nome utente e password, ma può anche includere applicazioni o altri server, che possono eseguire l'autenticazione con certificati o chiavi private. Un **account** rappresenta i dati associati a un'identità.

Un'**entità di sicurezza** è un'identità che agisce con determinati ruoli o attestazioni. Si consideri l'uso di "sudo" in un prompt di Bash o in Windows tramite "Esegui come amministratore". In entrambi i casi, si resta connessi con l'identità precedente, ma cambia il ruolo con cui si opera.

Pertanto, un'**entità servizio** è, letteralmente, un'identità che viene usata da un servizio o un'applicazione. Come le altre identità, possono esserle assegnati dei ruoli. 

Ad esempio, Lamna Healthcare può stabilire che l'esecuzione degli script di distribuzione avvenga previa autenticazione come entità servizio. Se questa è l'unica identità autorizzata a eseguire azioni distruttive, Lamna avrà compiuto notevoli progressi per assicurarsi che non si verifichi nuovamente un caso di eliminazione accidentale di risorse.

### <a name="managed-identities-for-azure-resources"></a>Identità gestite per risorse di Azure

La creazione di entità servizio può essere un'operazione laboriosa e la presenza di numerosi punti di contatto può complicarne la gestione. Le identità gestite sono molto più semplici e svolgono la maggior parte del lavoro automaticamente.

È possibile creare immediatamente un'identità gestita per qualsiasi servizio di Azure che la supporta (l'elenco è in continua espansione). Quando si crea un'identità gestita per un servizio, si crea di fatto un account nel tenant di Azure AD. L'infrastruttura di Azure si occuperà automaticamente di autenticare il servizio e gestire l'account. È quindi possibile usare questo account come qualsiasi altro account Active Directory e consentire al servizio autenticato di accedere ad altre risorse di Azure in tutta sicurezza.

Lamna Healthcare compie un ulteriore passo avanti nella gestione delle identità e usa identità gestite per tutti i servizi supportati che devono essere in grado di eseguire distribuzioni e operazioni di gestione dell'infrastruttura.

## <a name="infrastructure-protection-at-lamna-healthcare"></a>Protezione dell'infrastruttura in Lamna Healthcare

È stato descritto in che modo Lamna Healthcare ha risolto i problemi emersi dall'incidente in cui l'infrastruttura è stata eliminata involontariamente. L'azienda ha usato il controllo degli accessi in base al ruolo per gestire meglio la sicurezza dell'infrastruttura e usa identità gestite per mantenere le credenziali all'esterno del codice e semplificare l'amministrazione delle identità necessarie per i propri servizi.

## <a name="summary"></a>Riepilogo

Per garantire la disponibilità e l'integrità dell'infrastruttura, è importante proteggerla adeguatamente. L'uso corretto di funzionalità come il controllo degli accessi in base al ruolo e le identità gestite consente di proteggere l'ambiente di Azure da accessi non autorizzati o non previsti e migliorerà la sicurezza delle identità nell'architettura.