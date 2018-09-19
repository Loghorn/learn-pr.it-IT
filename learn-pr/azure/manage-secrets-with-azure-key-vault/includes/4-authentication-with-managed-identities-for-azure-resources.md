Per l'autenticazione degli utenti e delle applicazioni che tentano di accedere a un insieme di credenziali, Azure Key Vault usa **Azure Active Directory**. Per concedere l'accesso delle applicazioni web all'insieme di credenziali, è necessario innanzitutto registrare l'app con Azure Active Directory. La registrazione consente di creare un'identità per l'app. Quando l'app ha ottenuto un'identità, è possibile assegnare le autorizzazioni dell'insieme di credenziali.

Le app e gli utenti eseguono l'autenticazione a Key Vault usando un token di autenticazione di Azure Active Directory. Ottenere un token da Azure Active Directory implica la richiesta di un segreto o certificato, perché tutti gli utenti con un token usano l'identità dell'applicazione per accedere a tutti i segreti nell'insieme di credenziali.

I segreti dell'applicazione sono protetti nell'insieme di credenziali, ma è comunque necessario mantenere un segreto o un certificato all'esterno dell'insieme di credenziali per potervi accedere. Questo problema viene definito *problema di bootstrap* e Azure offre una soluzione.

## <a name="managed-identities-for-azure-resources"></a>Identità gestite per le risorse di Azure

Identità gestite per le risorse di Azure è una funzionalità di Azure che l'app può usare per accedere a Key Vault e ad altri servizi di Azure senza dover gestire alcun segreto all'esterno dell'insieme di credenziali. L'uso di un'identità gestita è un modo semplice e sicuro per sfruttare i vantaggi di Key Vault dall'app Web.

Quando si abilita l'identità gestita nell'app Web, Azure attiva un servizio REST separato per la concessione di token appositamente per l'app. L'app richiederà i token a questo servizio anziché direttamente ad Azure Active Directory. Per accedere a questo servizio, l'app deve usare un segreto, che però viene inserito nelle variabili di ambiente dell'app dal servizio app all'avvio. Non è necessario gestire o archiviare questo valore segreto da qualche parte e nulla all'esterno dell'app può accedere a questo segreto o all'endpoint servizio token dell'identità gestita.

La funzionalità Identità gestite per le risorse di Azure registra inoltre automaticamente l'app in Azure Active Directory ed eliminerà la registrazione nel caso in cui si elimini l'app Web o ne si disabiliti l'identità gestita.

Le identità gestite sono disponibili in tutte le edizioni di Azure Active Directory, inclusa la versione gratuita fornita con la sottoscrizione di Azure. L'utilizzo di questa funzionalità nel Servizio app non prevede costi aggiuntivi, non richiede alcuna configurazione e può essere abilitata o disabilitata in un'app in qualsiasi momento.

> [!NOTE]
> La funzionalità Identità gestite per le risorse di Azure non è attualmente supportata per le app Web Linux o contenitore.

Per abilitare un'identità gestita per un'app Web è necessario solo un singolo comando dell'interfaccia della riga di comando di Azure senza alcuna configurazione. Questa procedura verrà effettuata più avanti durante la configurazione di un'app del servizio app e la successiva distribuzione in Azure. Prima però sarà necessario applicare le conoscenze relative alle identità gestite per scrivere il codice per l'app.