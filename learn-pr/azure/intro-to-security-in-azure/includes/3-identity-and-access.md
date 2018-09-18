I perimetri di rete con i relativi firewall e controlli di accesso fisico sono stati a lungo il tipo di protezione principale per i dati aziendali. Ma i perimetri di rete sono diventati estremamente permeabili con l'esplosione delle tecnologie BYOD (Bring Your Own Device), delle app per dispositivi mobili e delle applicazioni cloud. 

La gestione delle identità è diventata il nuovo limite di sicurezza principale. L'autenticazione e l'assegnazione di privilegi corrette sono essenziali per garantire il controllo dei dati.

Contoso sta affrontando queste problematiche. La nuova soluzione cloud ibrido deve tenere conto delle app per dispositivi mobili che hanno accesso a dati riservati quando un utente autorizzato esegue l'accesso. L'azienda ha inoltre una flotta di veicoli che inviano un flusso costante di dati di telemetria essenziali per l'ottimizzazione dell'attività.

## <a name="single-sign-on"></a>Single Sign-On

Maggiore è il numero di identità che un utente deve gestire, maggiore sarà il rischio di un incidente di sicurezza correlato alle credenziali. Più identità vuol dire più password da tenere a mente e modificare. I criteri delle password possono variare tra le applicazioni e all'aumentare dei requisiti di complessità, per gli utenti diventa sempre più difficile ricordarle.

Occorre anche considerare la gestione necessaria per tutte queste identità. Aumenta il carico di lavoro dell'help desk, che si occupa dei blocchi degli account e delle richieste di reimpostazione della password. Se un utente abbandona un'organizzazione, può essere difficile tenere traccia di tutte le identità e garantire che siano state disabilitate. Se un'identità da eliminare viene tralasciata, potrebbe consentire un accesso indesiderato.

Con il Single Sign-On, gli utenti devono ricordare solo un ID e una password. L'accesso alle varie applicazioni viene concesso a una singola identità associata a un utente, semplificando il modello di sicurezza. Quando gli utenti cambiano ruolo o lasciano l'azienda, le modifiche all'accesso sono legate alla singola identità, riducendo significativamente l'impegno necessario per modificare o disabilitare gli account. L'uso del Single Sign-On per gli account renderà più semplice la gestione delle identità da parte degli utenti e aumenterà le funzionalità di sicurezza nell'ambiente.

### <a name="sso-with-azure-active-directory"></a>SSO con Azure Active Directory

Azure Active Directory (Azure AD) è un servizio di gestione delle identità basato sul cloud. Offre supporto integrato per la sincronizzazione con l'implementazione locale di Active Directory esistente oppure può essere usato autonomamente.

Questo vuol dire che tutte le applicazioni, locali, nel cloud (compreso Office 365) o persino per dispositivi mobili possono condividere le stesse credenziali. 

Amministratori e sviluppatori possono controllare l'accesso ai dati e alle applicazioni usando regole e criteri centralizzati configurati in Azure AD.

Inoltre, Microsoft è in posizione ideale per combinare più origini dati in un grafico della sicurezza intelligente, che può fornire analisi delle minacce e protezione in tempo reale dell'identità per tutti gli account in Azure Active Directory, compresi quelli sincronizzati dall'istanza di AD locale.

Contoso sta integrando l'istanza di Active Directory esistente con Azure AD per fare in modo che il controllo degli accessi risulti coerente in tutta l'organizzazione e agevolare così gli utenti che potranno accedere alla posta elettronica e ai documenti di Office 365 senza dover eseguire di nuovo l'autenticazione.

## <a name="multi-factor-authentication"></a>Autenticazione a più fattori

L'autenticazione a più fattori (MFA) offre sicurezza aggiuntiva per le identità, richiedendo due o più elementi per l'autenticazione completa. Questi elementi sono suddivisi in tre categorie:

- *un'informazione nota*
- *un oggetto fisico*
- *una caratteristica fisica*

L'**informazione nota** è in genere una password o la risposta a una domanda di sicurezza. L'**oggetto fisico** può essere un dispositivo mobile con un'app che riceve una notifica o un dispositivo per la generazione di token. La **caratteristica fisica** è in genere un dato biometrico, ad esempio un'impronta digitale o una scansione del viso, usate in molti dispositivi mobili.

L'uso dell'autenticazione a più fattori aumenta la protezione delle identità, limitando l'impatto di un'esposizione delle credenziali. Un utente malintenzionato che ha la password di un utente dovrà essere in possesso anche del suo telefono o avere il suo stesso viso per completamente l'autenticazione. L'autenticazione con un solo fattore verificato è insufficiente e l'utente malintenzionato non sarebbe in grado di usare le credenziali per l'autenticazione. I vantaggi per la sicurezza sono immensi ed è consigliabile abilitare l'autenticazione a più fattori ovunque sia possibile.

Azure AD offre funzionalità MFA incorporate e si integrerà con altri provider di autenticazione a più fattori di terze parti. L'autenticazione a più fattori viene fornita gratuitamente a qualsiasi utente con il ruolo di amministratore globale in Azure AD, poiché si tratta di account altamente sensibili. Per tutti gli altri account è possibile abilitare MFA acquistando una licenza con questa funzionalità e assegnandola all'account in questione.

Per Contoso Shipping si decide di abilitare MFA ogni volta che un utente accede da un computer connesso non di dominio. Questo include le app per dispositivi mobili.

## <a name="providing-identities-to-services"></a>Assegnazione di identità ai servizi

Spesso è utile assegnare ai servizi un'identità. Di frequente, e in contrasto con le procedure consigliate, le informazioni relative alle credenziali vengono incorporate nei file di configurazione. Se questi file di configurazione non sono protetti, chiunque disponga di accesso ai sistemi o ai repository può accedere alle credenziali e rischiare l'esposizione.

Azure AD risolve questo problema tramite due metodi: entità servizio e identità del servizio gestite.

### <a name="service-principals"></a>Entità servizio

Per comprendere le entità servizio, è utile comprendere prima di tutto i termini **identità** ed **entità di sicurezza** usati nell'ambito della gestione delle identità.

Un'**identità** è semplicemente un elemento che può essere autenticato. Ovviamente questo include gli utenti con nome utente e password, ma può anche includere applicazioni o altri server, che possono eseguire l'autenticazione con certificati o chiavi private. Un **account** rappresenta i dati associati a un'identità.

Un'**entità di sicurezza** è un'identità che agisce con determinati ruoli o attestazioni. Spesso non è utile considerare identità ed entità di sicurezza separatamente. Si pensi all'uso di "sudo" in un prompt di Bash o al comando "Esegui come amministratore" di Windows. In entrambi i casi si resta connessi con l'identità precedente, ma cambia il ruolo con cui si sta operando. I gruppi sono spesso considerati entità di sicurezza dal momento che è possibile assegnare loro diritti.

Pertanto, un'**entità servizio** è, letteralmente, un'identità che viene usata da un servizio o un'applicazione. Come accade per le altre identità, è possibile assegnarvi ruoli. 

### <a name="managed-service-identities"></a>Identità del servizio gestite

La creazione d entità servizio può essere un'operazione laboriosa e la presenza di numerosi touch point può complicarne la gestione. Le identità del servizio gestite sono molto più semplici e svolgono la maggior parte del lavoro automaticamente. 

È possibile creare immediatamente un'identità del servizio gestita per qualsiasi servizio di Azure che la supporti (l'elenco è in continua espansione). Quando si crea un'identità del servizio gestita per un servizio, si crea di fatto un account nel tenant di Azure Active Directory. L'infrastruttura di Azure si occuperà automaticamente di autenticare il servizio e gestire l'account. Si può quindi usare questo account come qualsiasi altro account Active Directory ed è possibile consentire al servizio autenticato di accedere ad altre risorse di Azure in tutta sicurezza.

## <a name="role-based-access-control"></a>Controllo degli accessi in base al ruolo

I ruoli sono set di autorizzazioni, ad esempio "Sola lettura" o "Collaboratore", che è possibile concedere agli utenti per l'accesso all'istanza di un servizio di Azure. 

Le identità vengono mappate ai ruoli direttamente o tramite l'appartenenza a gruppi. La separazione di entità di sicurezza, autorizzazioni di accesso e risorse fornisce una gestione semplificata degli accessi e un controllo più granulare. Gli amministratori possono garantire che vengano concesse le autorizzazioni minime necessarie.

I ruoli possono essere concessi a livello di singola istanza del servizio, ma vengono propagati verso il basso nella gerarchia di Azure Resource Manager. I ruoli assegnati in un ambito più elevato, ad esempio un'intera sottoscrizione, vengono ereditati dagli ambiti figlio, come le istanze del servizio. 

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Gruppi di gestione](../media-draft/3-role-assignment-scope.png)

### <a name="privileged-identity-management"></a>Privileged Identity Management

Oltre alla gestione dell'accesso alle risorse di Azure tramite il controllo degli accessi in base al ruolo, per un approccio completo alla protezione dell'infrastruttura è consigliabile includere il controllo continuativo dei membri dei ruoli man mano che l'organizzazione cambia e si evolve. Azure AD Privileged Identity Management (PIM) è un'offerta a pagamento aggiuntiva che fornisce supervisione delle assegnazioni di ruolo, funzionalità self-service, attivazione dei ruoli JIT e verifiche di accesso alle risorse di Azure AD e Azure.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Privileged Identity Management](../media-COPIED-FROM-DESIGNFORSECURITY/PIM_Dashboard.PNG)

La gestione delle identità con privilegi consente di gestire un perimetro di sicurezza anche al di fuori del controllo fisico. Con l'accesso Single Sign-On e la configurazione degli accessi in base al ruolo appropriata, è sempre possibile conoscere sempre con certezza quali utenti sono autorizzati a visualizzare e modificare i dati e l'infrastruttura.