Azure Active Directory (Azure AD) non è semplicemente un controller di dominio nel cloud. Azure AD è un servizio di gestione di identità e directory multi-tenant basato sul cloud. Combina servizi directory di base, gestione dell'accesso alle applicazioni e protezione delle identità in un'unica soluzione. Azure AD può interagire con un ambiente Windows Server Active Directory esistente attraverso Azure AD Connect, che consente di sfruttare gli investimenti in termini di identità già presenti a livello locale.

Azure AD è disponibile in quattro edizioni. Per questo esercizio, l'utente dovrà concentrarsi solo sulle funzionalità delle edizioni Azure AD Premium P1 e P2.

In questo modulo verrà creata una directory di Azure AD in cui verranno inseriti tutti gli utenti e i gruppi, in modo simile a una directory locale.

Quando si crea la directory, l'account diventa automaticamente un amministratore globale. Gli account con diritti di amministratore globale devono essere regolamentati, poiché hanno accesso completo a tutte le funzionalità amministrative di Azure AD. È possibile avere più amministratori globali, ma solo gli amministratori globali possono assegnare gli altri ruoli di amministratore agli utenti.

## <a name="how-can-azure-ad-help-you-protect-access-to-applications"></a>Come si protegge l'accesso alle applicazioni con Azure AD

Azure AD Premium include alcune funzionalità, ad esempio Multi-Factor Authentication, e criteri di accesso condizionale. Se queste funzionalità vengono usate insieme, offrono la massima granularità per la protezione dell'accesso alle applicazioni.

I criteri di accesso condizionale sono costituiti da:

- Utenti o gruppi
- Applicazioni a cui si tenta di accedere
- Controlli da effettuare, ad esempio Multi-Factor Authentication

## <a name="summary"></a>Riepilogo

In questa unità sono state esaminate le nozioni di base su Azure AD e sulle funzionalità disponibili per proteggere l'accesso alle applicazioni. Ora che si conoscono le informazioni di base, è possibile iniziare a usare Azure AD. Il resto di questo modulo contiene esercizi pratici che consentono di abilitare e verificare Multi-Factor Authentication usando l'accesso condizionale.
