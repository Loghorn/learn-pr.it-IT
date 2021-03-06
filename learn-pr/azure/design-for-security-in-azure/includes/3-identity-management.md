Lamna Healthcare ospita un'applicazione legacy interna e un portale Web in cui gli specialisti possono gestire i dati sanitari dei pazienti. L'organizzazione ha ricevuto numerose richieste perché l'applicazione sia resa disponibile agli operatori sanitari che sono spesso fuori sede, presso i pazienti, e quindi all'esterno della rete. 

Una recente perdita di dati causata da criminali informatici ha costretto la società a rafforzare i criteri di gestione delle password. Ora gli utenti devono usare password più lunghe e complesse e modificarle più di frequente. Questo ha creato un effetto collaterale indesiderato: gli utenti registrano le password complesse in modo non sicuro, perché hanno difficoltà a ricordare i vari set di credenziali creati per i diversi ruoli amministrativi. 

Si illustrerà qui l'uso dell'identità come livello di sicurezza per le applicazioni interne ed esterne, i vantaggi del Single Sign-On (SSO) e dell'autenticazione a più fattori (MFA) per la protezione delle identità e il motivo per cui è consigliabile replicare le identità locali in Azure Active Directory.

## <a name="identity-as-a-layer-of-security"></a>Identità come livello di sicurezza

Oggi le identità digitali sono parte integrante delle interazioni sociali e di business, in locale e online. In passato, le identità e i servizi di accesso operavano solo entro i confini della rete aziendale e venivano usati protocolli progettati allo scopo, ad esempio Kerberos e LDAP. Più di recente, i dispositivi mobili sono diventati lo strumento principale con cui le persone interagiscono con servizi digitali. Clienti e dipendenti si aspettano di poter accedere ai servizi ovunque in qualsiasi momento e questo ha portato allo sviluppo di protocolli di identità in grado di funzionare su scala Internet e con molti tipi di dispositivi e sistemi operativi.

Lamna Healthcare sta valutando le capacità della propria architettura in termini di identità e cercando modi per implementare nell'applicazione le capacità seguenti:

- Offrire il Single Sign-On agli utenti dell'applicazione
- Migliorare l'applicazione legacy in modo che usi l'autenticazione moderna con il minimo lavoro richiesto
- Imporre l'autenticazione a più fattori per tutti gli accessi esterni alla rete aziendale
- Sviluppare un'applicazione per consentire i pazienti di registrarsi e gestire in modo sicuro i dati dell'account

## <a name="single-sign-on"></a>Single Sign-On

Maggiore è il numero di identità che un utente deve gestire, maggiore sarà il rischio di un incidente di sicurezza correlato alle credenziali. Più identità vuol dire più password da tenere a mente e modificare. I criteri delle password possono variare tra le applicazioni e all'aumentare dei requisiti di complessità, per gli utenti diventa sempre più difficile ricordarle.

Occorre anche considerare la gestione necessaria per tutte queste identità. Aumenta il carico di lavoro dell'help desk, che si occupa dei blocchi degli account e delle richieste di reimpostazione della password. Se un utente abbandona un'organizzazione, può essere difficile tenere traccia di tutte le identità e garantire che siano state disabilitate. Se un'identità da eliminare viene tralasciata, potrebbe consentire un accesso indesiderato.

Con il Single Sign-On, gli utenti devono ricordare solo un ID e una password. L'accesso alle varie applicazioni viene concesso a una singola identità associata a un utente, semplificando il modello di sicurezza. Quando gli utenti cambiano ruolo o lasciano l'azienda, le modifiche all'accesso sono legate alla singola identità, riducendo significativamente l'impegno necessario per modificare o disabilitare gli account. L'uso del Single Sign-On per gli account renderà più semplice la gestione delle identità da parte degli utenti e aumenterà le funzionalità di sicurezza nell'ambiente.

### <a name="sso-with-azure-active-directory"></a>SSO con Azure Active Directory

Azure Active Directory (Azure AD) è un servizio di gestione delle identità basato sul cloud. Offre supporto integrato per la sincronizzazione con l'istanza locale di Active Directory esistente o può essere usato autonomamente. Questo vuol dire che tutte le applicazioni, siano esse locali, nel cloud (compreso Office 365) o persino per dispositivi mobili, possono condividere le stesse credenziali. Amministratori e sviluppatori possono controllare l'accesso ai dati e alle applicazioni usando regole e criteri centralizzati configurati in Azure AD.

Azure AD per SSO consente anche di combinare più origini dati in un grafico della sicurezza intelligente. Questo grafico della sicurezza consente di fornire analisi delle minacce e protezione delle identità in tempo reale a tutti gli account in Azure AD, inclusi gli account sincronizzati dall'istanza di AD locale. Usando un provider di identità centralizzato, saranno centralizzati i controlli di sicurezza, la creazione di report e di avvisi e l'amministrazione dell'infrastruttura di identità.

### <a name="synchronize-directories-with-ad-connect"></a>Sincronizzare le directory con Azure AD Connect

Azure AD Connect integra le directory locali con Azure Active Directory. Azure AD Connect offre le funzionalità più recenti e sostituisce le versioni precedenti di strumenti di integrazione delle identità, quali DirSync e Azure AD Sync.

Si tratta di un unico strumento che offre un'esperienza di distribuzione semplificata per la sincronizzazione e l'accesso.

![Un'illustrazione che mostra l'uso di Azure AD Connect per sincronizzare le directory locali con Azure Active Directory e le app nel cloud di Azure. Ciò consente agli utenti accedere con le stesse credenziali a tutte le applicazioni presenti nel cloud di Azure.](../media/AADCONNECTxprs_960.png)

Lamna Healthcare richiede che l'autenticazione avvenga principalmente sui controller di dominio locali, ma richiede anche l'autenticazione cloud in uno scenario di ripristino di emergenza. Tutti i suoi requisiti sono già supportati da Azure AD.

Lamna Healthcare ha preso la decisione di procedere con la configurazione seguente:

- Usare Azure AD Connect per sincronizzare i gruppi, gli account utente e gli hash delle password archiviati nell'istanza di Active Directory locale con Azure AD
  - Se l'autenticazione pass-through non è disponibile, è possibile usarlo come backup
- Configurare l'autenticazione pass-through usando un agente di autenticazione locale installato in un'istanza di Windows Server locale
- Usare la semplice funzionalità di Single Sign-On di Azure Active Directory per l'accesso automatico degli utenti dai PC aggiunti al dominio locale
  - Questo consente di ridurre i fastidi degli utenti eliminando le richieste di autenticazione multiple

## <a name="authentication--access"></a>Autenticazione e accesso

I criteri di sicurezza di Lamna Healthcare richiedono che tutti gli accessi eseguiti all'esterno delle rete perimetrale dell'azienda vengano autenticati con un fattore di autenticazione aggiuntivo. Questo requisito combina due aspetti del servizio Azure AD: l'autenticazione a più fattori e i criteri di accesso condizionale.

### <a name="multi-factor-authentication"></a>Autenticazione a più fattori

L'autenticazione a più fattori (MFA) offre sicurezza aggiuntiva per le identità, richiedendo due o più elementi per l'autenticazione completa. Questi elementi sono suddivisi in tre categorie:

- *un'informazione nota*
- *un oggetto fisico*
- *una caratteristica fisica*

L'**informazione nota** è in genere una password o la risposta a una domanda di sicurezza. L'**oggetto fisico** può essere un dispositivo mobile con un'app che riceve una notifica o un dispositivo per la generazione di token. La **caratteristica fisica** è in genere un dato biometrico, ad esempio un'impronta digitale o una scansione del viso, usate in molti dispositivi mobili.

L'uso dell'autenticazione a più fattori aumenta la protezione delle identità, limitando l'impatto di un'esposizione delle credenziali. Un utente malintenzionato che ha la password di un utente dovrà essere in possesso anche del suo telefono o avere il suo stesso viso per completamente l'autenticazione. L'autenticazione con un solo fattore verificato è insufficiente e l'utente malintenzionato non sarebbe in grado di usare le credenziali per l'autenticazione. I vantaggi per la sicurezza sono immensi ed è consigliabile abilitare l'autenticazione a più fattori ovunque sia possibile.

Le funzionalità di autenticazione a più fattori sono integrate in Azure AD, che si integrerà con altri provider di MFA di terze parti. L'autenticazione a più fattori viene fornita gratuitamente a qualsiasi utente con il ruolo di amministratore globale in Azure AD, poiché si tratta di account altamente sensibili. Per tutti gli altri account è possibile abilitare l'autenticazione a più fattori acquistando una licenza con questa funzionalità e assegnandola all'account in questione.

### <a name="conditional-access-policies"></a>Criteri di accesso condizionale

In aggiunta all'autenticazione a più fattori, la richiesta che siano soddisfatti altri requisiti per concedere l'accesso consente di aggiungere un ulteriore livello di protezione. Bloccare gli accessi da un indirizzo IP sospetto o negare l'accesso da dispositivi privi di protezione contro il malware consente di limitare gli accessi a rischio.

Azure Active Directory fornisce la funzionalità dei criteri di accesso condizionale, che include il supporto per criteri di accesso in base al gruppo, alla posizione o allo stato del dispositivo. Il controllo della posizione consente a Lamna di distinguere gli indirizzi IP che non appartengono alla propria rete e soddisfa i criteri di sicurezza aziendali che richiedono l'autenticazione a più fattori da tutte le posizioni esterne.

Lamna Healthcare ha creato [criteri di accesso condizionale](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) in base a cui gli utenti che accedono all'applicazione da un indirizzo IP esterno alla rete aziendale devono completare l'autenticazione a più fattori.

![Un'illustrazione che mostra un esempio di implementazione dei criteri di accesso condizionale e dell'autenticazione a più fattori. Le richieste di accesso alle applicazioni locali e cloud dell'utente vengono inizialmente confrontate con un elenco di condizioni. In base alle condizioni soddisfatte, le richieste di accesso possono essere autorizzate, richiedere il completamento dell'autenticazione a più fattori oppure essere bloccate.](../media/conditional-access.png)

## <a name="securing-legacy-applications"></a>Protezione delle applicazioni legacy

I dipendenti di Lamna Healthcare hanno bisogno di accesso remoto sicuro all'applicazione amministrativa ospitata in locale. Attualmente gli utenti eseguono l'autenticazione all'applicazione usando l'autenticazione integrata di Windows dai propri computer aggiunti al dominio, dietro il firewall aziendale. Nonostante sia stato pianificato un progetto per incorporare meccanismi di autenticazione moderni nell'applicazione, c'è una notevole pressione da parte dell'amministrazione aziendale perché siano abilitate al più presto funzionalità di accesso remoto. Azure Active Directory Application Proxy consente di abilitare l'accesso remoto all'applicazione in modo rapido, semplice e sicuro, senza apportare modifiche al codice.

Azure Active Directory Application Proxy è:

- Semplice
  - Non è necessario modificare o aggiornare le applicazioni per usare AD Application Proxy.
  - Offre agli utenti un'esperienza di autenticazione coerente. Possono usare il portale App personali per ottenere l'accesso Single Sign-On sia alle app SaaS nel cloud, sia alle app locali.
- Sicuro
  - Quando si pubblicano le app usando Azure Active Directory Application Proxy, è possibile sfruttare l'analisi della sicurezza e i controlli di autorizzazione avanzati in Azure. Si ottengono sicurezza a livello di cloud e funzionalità di sicurezza di Azure come l'accesso condizionale e la verifica in due passaggi.
  - Non è necessario aprire connessioni in ingresso attraverso il firewall per consentire agli utenti l'accesso remoto.
- Conveniente
  - AD Application Proxy opera nel cloud, facendo risparmiare tempo e denaro. Le soluzioni locali richiedono generalmente di configurare e gestire reti perimetrali, server perimetrali o altre infrastrutture complesse.

Azure AD Application Proxy è costituito da due componenti: un agente connettore che risiede in un server Windows all'interno della rete aziendale e un endpoint esterno, rappresentato dal portale App personali o da un URL esterno. Quando un utente passa all'endpoint, esegue l'autenticazione con Azure AD e viene instradato all'applicazione locale tramite l'agente connettore.

## <a name="working-with-consumer-identities"></a>Uso delle identità degli utenti

Dopo l'integrazione di sistemi di autenticazione moderni nella propria applicazione esistente, Lamna Healthcare ha compreso rapidamente i vantaggi che un sistema con identità gestite come Azure AD può apportare all'organizzazione. Ora la dirigenza è interessata a esplorare altri modi in cui i servizi di identità Microsoft possono aggiungere valore aziendale. L'attenzione si è concentrata sui clienti esterni e su come la modernizzazione delle interazioni esistenti con i clienti possa fornire una stretta integrazione con provider di identità di terze parti come Google, Facebook e LinkedIn.

Azure AD B2C è un servizio di gestione delle identità basato sulle solide fondamenta di Azure Active Directory, che consente di personalizzare e controllare il modo in cui i clienti si iscrivono, accedono e gestiscono i loro profili quando usano le applicazioni dell'azienda, incluse ad esempio le applicazioni sviluppate per iOS, Android e .NET. Azure AD B2C offre un'esperienza di accesso basata sull'identità sui social network, proteggendo allo stesso tempo le informazioni sul profilo dell'identità dei clienti. Le directory di Azure AD B2C sono diverse dalle directory standard di Azure AD e possono essere create nel portale di Azure.

## <a name="identity-management-at-lamna-healthcare"></a>Gestione delle identità in Lamna Healthcare

Abbiamo visto come Lamna Healthcare ha usato soluzioni di gestione delle identità in Azure per migliorare la sicurezza del proprio ambiente. La società ha iniziato fornendo un'esperienza di Single Sign-On per ridurre al minimo gli account che gli utenti devono gestire e ridurre la complessità operativa associata a un numero eccessivo di account. Ha imposto l'autenticazione a più fattori per l'accesso all'applicazione interna e ha aggiornato un'applicazione legacy per l'uso dell'autenticazione moderna con un lavoro richiesto minimo. Ha inoltre appreso come potenziare le proprie capacità di usare le identità degli utenti, migliorando l'usabilità dell'applicazione per i pazienti.

## <a name="summary"></a>Riepilogo

In questa unità è stato illustrato come si può combinare una serie di funzionalità di Azure Active Directory per fornire una solida soluzione di gestione delle identità, per proteggere l'accesso alle applicazioni, indipendentemente dalla loro posizione. L'identità è un livello di sicurezza critico. Quando è ben progettata e inclusa nell'architettura, è possibile garantire che l'ambiente sia sicuro.