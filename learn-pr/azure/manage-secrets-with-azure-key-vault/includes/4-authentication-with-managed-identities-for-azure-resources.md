Per l'autenticazione degli utenti e delle applicazioni che tentano di accedere a un insieme di credenziali, Azure Key Vault usa **Azure Active Directory**. Per concedere l'accesso delle applicazioni web all'insieme di credenziali, è necessario innanzitutto registrare l'app con Azure Active Directory. La registrazione consente di creare un'identità per l'app. Quando l'app ha ottenuto un'identità, è possibile assegnare le autorizzazioni dell'insieme di credenziali.

Le app e gli utenti eseguono l'autenticazione a Key Vault usando un token di autenticazione di Azure Active Directory. Ottenere un token da Azure Active Directory implica la richiesta di un segreto o certificato, perché tutti gli utenti con un token usano l'identità dell'applicazione per accedere a tutti i segreti nell'insieme di credenziali.

I segreti dell'applicazione sono protetti nell'insieme di credenziali, ma è comunque necessario mantenere un segreto o un certificato all'esterno dell'insieme di credenziali per potervi accedere. Questo problema viene definito  *problema di bootstrap*, ma Azure offre una soluzione.

## <a name="managed-identities-for-azure-resources"></a>Identità gestite per le risorse di Azure

Identità gestita per le risorse di Azure è una funzionalità di Azure che l'app può usare per accedere a Key Vault e altri servizi di Azure senza dover gestire anche un singolo segreto di fuori di insieme di credenziali. Usando un'identità gestita è un modo semplice e sicuro per sfruttare i vantaggi di Key Vault dall'app web.

Quando si abilita identità gestita nell'app web, Azure abilitano un servizio REST il concessione di token separato appositamente per l'utilizzo dalla tua app. L'app richiederà i token da questo servizio anziché direttamente da Azure Active Directory. L'app deve usare un segreto per accedere a questo servizio, che però viene inserito nelle variabili di ambiente dell'app dal servizio app all'avvio. Non è necessario gestire o archiviare questo valore del segreto in un punto qualsiasi, e nulla di fuori l'app può accedere a questo segreto o l'endpoint di servizio token di identità gestita.

Identità gestita per le risorse di Azure anche di registrare l'app in Azure Active Directory per l'utente ed elimina la registrazione se si elimina l'app web o disabilitare la relativa identità gestita.

Identità gestite sono disponibili in tutte le edizioni di Azure Active Directory, inclusa l'edizione gratuita inclusa in una sottoscrizione di Azure. Il relativo uso in Servizio app non ha costi aggiuntivi, non richiede alcuna configurazione e può essere abilitato o disabilitato in un'app in qualsiasi momento.

> [!NOTE]
> Identità gestita per le risorse di Azure non è attualmente supportato per le app web Linux o un contenitore.

Abilitare un'identità gestita per un'app web è necessario solo un singolo comando di Azure senza alcuna configurazione. Questo argomento verrà trattato più avanti durante la configurazione di un'app del servizio app e la relativa distribuzione in Azure. In precedenza, tuttavia, dobbiamo applicare nostre conoscenze di identità gestite per scrivere il codice per l'App nell'app.