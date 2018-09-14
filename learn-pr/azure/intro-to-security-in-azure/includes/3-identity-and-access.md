I perimetri di rete e firewall e ai controlli di accesso fisico usato per la protezione principale per i dati aziendali. Ma perimetri di rete sono diventati sempre più permeabili enorme diffusione di portare il proprio dispositivo (BYOD), App per dispositivi mobili, applicazioni e cloud. 

Identity è diventato il nuovo limite di sicurezza primario. L'autenticazione corretta e l'assegnazione dei privilegi è fondamentale per mantenere il controllo dei dati.

Contoso Shipping risolve questi problemi sin da subito. La nuova soluzione cloud ibrida deve tenere conto delle App per dispositivi mobili che hanno accesso a dati riservati quando un utente autorizzato ha eseguito l'accesso. Dispongono anche di spedizione veicoli l'invio di un flusso costante di dati di telemetria che sono fondamentali per l'ottimizzazione della propria azienda.

## <a name="single-sign-on"></a>Single Sign-On

Maggiore è il numero di identità che un utente deve gestire, maggiore sarà il rischio di un incidente di sicurezza correlato alle credenziali. Più identità vuol dire più password da tenere a mente e modificare. I criteri password possono variare tra le applicazioni e, quando i requisiti di complessità aumentano, rende più difficile per gli utenti per ricordarle.

Occorre anche considerare la gestione necessaria per tutte queste identità. Aumenta il carico di lavoro dell'help desk, che si occupa dei blocchi degli account e delle richieste di reimpostazione della password. Se un utente abbandona un'organizzazione, può essere difficile tenere traccia di tutte le identità e garantire che siano state disabilitate. Se un'identità da eliminare viene tralasciata, potrebbe consentire un accesso indesiderato.

Con single sign-on (SSO), gli utenti devono ricordare un solo ID e una password. L'accesso alle varie applicazioni viene concesso a una singola identità associata a un utente, semplificando il modello di sicurezza. Quando gli utenti cambiano ruolo o lasciano l'azienda, le modifiche all'accesso sono legate alla singola identità, riducendo significativamente l'impegno necessario per modificare o disabilitare gli account. Utilizza single sign-on per gli account rendono più facile per gli utenti gestire le identità e aumenteranno le funzionalità di sicurezza nell'ambiente in uso.

### <a name="sso-with-azure-active-directory"></a>SSO con Azure Active Directory

Azure Active Directory (Azure AD) è un servizio per la gestione delle identità basato sul cloud. Include supporto incorporato per la sincronizzazione con l'istanza di Active Directory locale esistente oppure può essere usato autonomo.

Questo vuol dire che tutte le applicazioni, locali, nel cloud (compreso Office 365) o persino per dispositivi mobili possono condividere le stesse credenziali. 

Amministratori e sviluppatori possono controllare l'accesso ai dati e alle applicazioni usando regole e criteri centralizzati configurati in Azure AD.

Inoltre, Microsoft è posizionato in modo univoco per combinare più origini dati in un grafico della sicurezza intelligente che può fornire l'analisi delle minacce e la protezione dell'identità in tempo reale a tutti gli account in Azure Active Directory (anche gli account che vengono sincronizzati l'istanza di Active Directory in locale).

Spedizioni di Contoso prevede di integrare l'istanza di Active Directory esistente con Azure AD. In questo modo il controllo dell'accesso coerente nell'intera organizzazione. Inoltre consentirà un gioco da ragazzi per gli utenti a ottenere nel messaggio di posta elettronica e documenti di Office 365 senza la necessità di ripetere l'autenticazione.

## <a name="multi-factor-authentication"></a>Autenticazione a più fattori

L'autenticazione a più fattori (MFA) offre sicurezza aggiuntiva per le identità, richiedendo due o più elementi per l'autenticazione completa. Questi elementi sono suddivisi in tre categorie:

- *un'informazione nota*
- *un oggetto fisico*
- *una caratteristica fisica*

**Un elemento noto** sarebbe una password o la risposta alla domanda di sicurezza. **Un elemento possiedono** potrebbe essere un'app per dispositivi mobili che riceve una notifica o un dispositivo di generazione di token. **Sono** viene in genere una sorta di proprietà biometrica, ad esempio una scansione di impronta digitale o viso utilizzata in molti dispositivi mobili.

Uso di MFA aumenta la protezione della tua identità, limitando l'impatto di esposizione delle credenziali. Un utente malintenzionato che ha la password di un utente dovrà essere in possesso anche del suo telefono o avere il suo stesso viso per completamente l'autenticazione. L'autenticazione con un singolo fattore verificato è insufficiente e l'utente malintenzionato sarebbe in grado di utilizzare tali credenziali per l'autenticazione. I vantaggi per la sicurezza sono immensi ed è consigliabile abilitare l'autenticazione a più fattori ovunque sia possibile.

Azure AD offre funzionalità MFA incorporata e si integrerà con altri provider di autenticazione a più fattori di terze parti. Viene fornito gratuitamente a qualsiasi utente che ha il ruolo di amministratore globale in Azure AD, poiché si tratta di account riservati. Per tutti gli altri account è possibile abilitare MFA acquistando una licenza con questa funzionalità e assegnandola all'account in questione.

Per la spedizione di Contoso, si decide di abilitare l'autenticazione a più fattori ogni volta che un utente esegue l'accesso da un computer non connesso-dominio. Che include l'App per dispositivi mobili.

## <a name="providing-identities-to-services"></a>Assegnazione di identità ai servizi

Spesso è utile assegnare ai servizi un'identità. Spesso e rispetto alle procedure consigliate, informazioni sulle credenziali vengono incorporate nel file di configurazione. Se questi file di configurazione non sono protetti, chiunque disponga di accesso ai sistemi o ai repository può accedere alle credenziali e rischiare l'esposizione.

Azure AD risolve questo problema tramite due metodi: entità servizio e identità del servizio gestite.

### <a name="service-principals"></a>Entità servizio

Per comprendere le entità servizio, è utile per capire le parole **identity** e **dell'entità**, perché vengono usate in tutto il mondo di gestione delle identità.

Un'**identità** è semplicemente un elemento che può essere autenticato. Ovviamente, inclusi gli utenti con un nome utente e password, ma può includere anche le applicazioni o altri server, che potrebbe eseguire l'autenticazione con certificati o chiavi private. Un **account** rappresenta i dati associati a un'identità.

Un'**entità di sicurezza** è un'identità che agisce con determinati ruoli o attestazioni. Non è spesso utile prendere in considerazione identity e principal separatamente, ma è opportuno utilizzare `sudo` in un prompt di bash o in Windows tramite "Esegui come amministratore". In entrambi i casi, è ancora connessi come la stessa identità, ma è stato modificato il ruolo in cui si sta eseguendo. I gruppi vengono considerati entità spesso anche perché possono disporre di diritti assegnati.

Pertanto, un **entità servizio** letteralmente denominato. un'identità che viene usata da un servizio o un'applicazione. Come altre identità, può essere assegnato i ruoli. 

### <a name="managed-service-identities"></a>Identità del servizio gestito

La creazione dell'entità servizio può essere un'operazione laboriosa, ed esistono molti punti di tocco che può rendere su come gestirli difficile. Le identità del servizio gestito sono molto più semplice e verranno eseguita la maggior parte del lavoro per l'utente. 

È possibile creare immediatamente un'identità del servizio gestito per qualsiasi servizio di Azure che lo supporta (l'elenco è in continua crescita). Quando si crea un'identità del servizio gestito per un servizio, si sta creando un account nel tenant di Azure Active Directory. L'infrastruttura di Azure si occuperà automaticamente di autenticazione del servizio e la gestione dell'account. È quindi possibile usare tale account come qualsiasi altro account di Azure AD, tra cui consentire in modo sicuro il servizio autenticato accedere ad altre risorse di Azure.

## <a name="role-based-access-control"></a>Controllo degli accessi in base al ruolo

I ruoli sono set di autorizzazioni, ad esempio "Read-only" o "Collaboratore", che è possono concedere agli utenti per accedere a un'istanza di servizio di Azure. 

Le identità vengono mappate ai ruoli direttamente o tramite l'appartenenza al gruppo. La separazione delle entità di sicurezza, le autorizzazioni di accesso e delle risorse fornisce la gestione degli accessi semplice e il controllo con granularità fine. Gli amministratori sono in grado di verificare che siano concesse le autorizzazioni minime necessarie.

I ruoli possono essere concesse a livello di istanza di servizio singoli, ma sono anche fluiscono verso il basso della gerarchia di Azure Resource Manager. I ruoli assegnati in un ambito più elevato, ad esempio un'intera sottoscrizione, vengono ereditati dagli ambiti figlio, come le istanze del servizio. 

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Gruppi di gestione](../media-draft/3-role-assignment-scope.png)

### <a name="privileged-identity-management"></a>Privileged Identity Management

Oltre a gestire l'accesso alle risorse di Azure con accesso in base al ruolo (RBAC) di controllo, un approccio completo alla protezione dell'infrastruttura è necessario considerare l'inclusione in corso il controllo dei membri del ruolo come le modifiche all'organizzazione e si evolve. Azure AD Privileged Identity Management (PIM) è un'offerta aggiuntiva, a pagamento che offre la supervisione di assegnazioni di ruolo, funzionalità Self-Service e just-in-time attivazione del ruolo e Azure AD e le verifiche di accesso alle risorse di Azure.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Gestione delle identità con privilegi](../media-COPIED-FROM-DESIGNFORSECURITY/PIM_Dashboard.png)

Identità consente di mantenere un perimetro di sicurezza, anche all'esterno del controllo fisico. Con accesso single sign-on e la configurazione di accesso in base al ruolo appropriato, possiamo sempre essere certi che ha la possibilità di visualizzare e modificare i dati e infrastruttura.