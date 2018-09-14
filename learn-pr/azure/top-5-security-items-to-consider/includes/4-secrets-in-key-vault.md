I segreti non sono segreti se vengono condivise con tutti gli utenti. Archiviare gli elementi riservati, ad esempio le stringhe di connessione, i token di sicurezza, i certificati e password nel codice è semplicemente l'invito a rimuoverli e usarle per un valore diverso da quelli desiderati per. Anche l'archiviazione di questo tipo di dati nella configurazione web è una buona idea, essenzialmente consente a chiunque abbia accesso al codice sorgente o accesso al server web ai dati privati.

Al contrario, è sempre consigliabile inserire questi segreti in **Azure Key Vault**.

## <a name="what-is-azure-key-vault"></a>Che cos'è Azure Key Vault
Azure Key Vault è un *archivio segreti*, ossia un servizio cloud centralizzato per l'archiviazione dei segreti delle applicazioni. Key Vault mantiene i dati riservati sicuro mantenendo i segreti dell'applicazione in un'unica posizione centrale e fornire accesso sicuro, il controllo delle autorizzazioni e la registrazione di accesso.

I segreti vengono memorizzati in singoli *insiemi di credenziali*, ognuno con i propri criteri di configurazione e sicurezza per controllare l'accesso. È quindi possibile ottenere ai dati tramite un'API REST o tramite un client SDK disponibili per la maggior parte dei linguaggi.

> [!IMPORTANT]
> **Key Vault è progettato per archiviare i segreti della configurazione delle applicazioni server.** Non è destinato ad archiviare i dati appartenenti agli utenti dell'app e non dovrebbe essere usato sul lato client di un'app. Ciò si riflette nelle caratteristiche di prestazioni, nell'API e nel modello di costi.
>
> I dati utente dovranno essere archiviati altrove, ad esempio in un database SQL di Azure con Transparent Data Encryption o in un account di archiviazione con la crittografia del servizio di archiviazione. I segreti usati dall'applicazione per accedere a tali archivi dati possono essere conservati in Key Vault.

## <a name="why-use-a-key-vault-for-my-secrets"></a>Perché usare un insieme di credenziali chiave per i segreti

Gestione delle chiavi e l'archiviazione dei segreti può essere complessa e soggetta a errori se eseguita manualmente. Rotazione dei certificati manualmente significa potenzialmente senza per alcune ore o giorni. Come indicato in precedenza, il salvataggio le stringhe di connessione nella configurazione del repository di codice o file significa che qualcuno potrebbe rubare le credenziali.

Key Vault consente agli utenti di archiviare le stringhe di connessione, i segreti, le password, certificati, i criteri di accesso, i blocchi di file (rendendo gli elementi in Azure sola lettura) e gli script di automazione.  Registra inoltre l'accesso e l'attività, consente di monitorare il controllo di accesso (IAM) nella sottoscrizione, e dispone di diagnostica, metriche, avvisi e gli strumenti di risoluzione dei problemi, avere a disposizione l'accesso che necessario.

![Azure Key Vault](../media-draft/Key-Vault.png)

<!-- TODO: get link to TC module -->
<!--
You can learn more about using a Key Vault in the module [Manage secrets in your server apps with Azure Key Vault](learn/modules/manage-secrets-with-azure-key-vault).-->

## <a name="summary"></a>Riepilogo

Credential theft, manuali rotazione delle chiavi e certificato di rinnovo può essere un ricordo del passato se gestire i segreti, usando Azure Key Vault.
