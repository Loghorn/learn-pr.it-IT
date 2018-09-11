Per l'autenticazione degli utenti e delle applicazioni che tentano di accedere a un insieme di credenziali, Azure Key Vault usa **Azure Active Directory**. Per concedere l'accesso delle applicazioni web all'insieme di credenziali, è necessario innanzitutto registrare l'app con Azure Active Directory. La registrazione consente di creare un'identità per l'app. Quando l'app ha ottenuto un'identità, è possibile assegnare le autorizzazioni dell'insieme di credenziali.

Le app e gli utenti eseguono l'autenticazione a Key Vault usando un token di autenticazione di Azure Active Directory. Ottenere un token da Azure Active Directory implica la richiesta di un segreto o certificato, perché tutti gli utenti con un token usano l'identità dell'applicazione per accedere a tutti i segreti nell'insieme di credenziali.

I segreti dell'applicazione sono protetti nell'insieme di credenziali, ma è comunque necessario mantenere un segreto o un certificato all'esterno dell'insieme di credenziali per potervi accedere. Questo problema viene definito  *problema di bootstrap*, ma Azure offre una soluzione.

## <a name="managed-service-identity"></a>Identità del servizio gestita

*L'identità del servizio gestita* è una funzionalità del servizio app di Azure che l'app può usare per accedere a Key Vault e ad altri servizi di Azure senza dover gestire alcun segreto al di fuori dell'insieme di credenziali. L'identità del servizio gestita è un modo semplice e sicuro per sfruttare i vantaggi di Key Vault dall'app Web.

Quando si abilita l'identità del servizio gestita, Azure abilita un servizio REST separato per la concessione di token che l'app può usare. L'app richiederà i token da questo servizio anziché direttamente da Azure Active Directory. L'app deve usare un segreto per accedere a questo servizio, che però viene inserito nelle variabili di ambiente dell'app dal servizio app all'avvio. Non è necessario gestire o archiviare questo valore segreto da qualche parte, e nulla al di fuori dell'app può accedere a questo segreto o all'endpoint di servizio del token dell'identità del servizio gestita.

L'identità del servizio gestita registra l'app in Azure Active Directory ed elimina la registrazione se si disabilita l'identità o si elimina l'app.

L'identità del servizio gestito è disponibile in tutte le edizioni di Azure Active Directory, inclusa la versione gratuita fornita con la sottoscrizione ad Azure. Il relativo uso in Servizio app non ha costi aggiuntivi, non richiede alcuna configurazione e può essere abilitato o disabilitato in un'app in qualsiasi momento.

> [!NOTE]
> L'identità del servizio gestito non è attualmente supportata per le app web Linux o Contenitore.

Per abilitare l'identità del servizio gestita è necessario solo un singolo comando dell'interfaccia della riga di comando di Azure senza alcuna configurazione. Questo argomento verrà trattato più avanti durante la configurazione di un'app del servizio app e la relativa distribuzione in Azure. Prima però sarà necessario applicare le conoscenze relative all'identità del servizio gestita per scrivere il codice per l'app.