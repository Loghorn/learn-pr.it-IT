I segreti non sono più tali se vengono condivisi con tutti. L'inserimento nel codice di elementi di natura riservata come stringhe di connessione, token di sicurezza, certificati e password è un invito a prenderli e usarli per scopi diversi da quello legittimo. Anche l'archiviazione di questo tipo di dati nella configurazione Web è una cattiva idea, in quanto essenzialmente si consente a chiunque abbia accesso al codice sorgente o al server Web di impossessarsi di dati privati.

È invece opportuno archiviare sempre questi segreti in **Azure Key Vault**.

## <a name="what-is-azure-key-vault"></a>Che cos'è Azure Key Vault
Azure Key Vault è un *archivio segreti*, ossia un servizio cloud centralizzato per l'archiviazione dei segreti delle applicazioni. Key Vault tiene al sicuro i dati riservati mantenendo i segreti delle applicazioni in un'unica posizione centrale e offrendo accesso sicuro, controllo delle autorizzazioni e registrazione degli accessi.

I segreti vengono archiviati in singoli *insiemi di credenziali*, ognuno con la propria configurazione e i propri criteri di sicurezza per controllare l'accesso. È possibile accedere ai propri dati tramite un'API REST o tramite un SDK client disponibile per la maggior parte delle lingue.

> [!IMPORTANT]
> **Key Vault è progettato per archiviare i segreti della configurazione delle applicazioni server.** Non è destinato ad archiviare i dati appartenenti agli utenti dell'app e non dovrebbe essere usato sul lato client di un'app. Ciò si riflette nelle caratteristiche di prestazioni, nell'API e nel modello di costi.
>
> I dati utente dovranno essere archiviati altrove, ad esempio in un database SQL di Azure con Transparent Data Encryption o in un account di archiviazione con la crittografia del servizio di archiviazione. I segreti usati dall'applicazione per accedere a tali archivi dati possono essere conservati in Key Vault.

## <a name="why-use-a-key-vault-for-my-secrets"></a>Perché usare Key Vault per i segreti

La gestione delle chiavi e l'archiviazione dei segreti possono essere operazioni complesse e soggette a errori, se eseguite manualmente. Se si ruotano i certificati manualmente, si rischia potenzialmente di rimanere senza per alcune ore, se non giorni. Come già accennato, salvando le stringhe di connessione nel file di configurazione o nel repository di codice si espongono le credenziali al rischio di furto.

Key Vault consente agli utenti di archiviare stringhe di connessione, segreti, password, criteri di accesso, blocchi di file (che impostano gli elementi in Azure come di sola lettura) e script di automazione.  Registra anche gli accessi e le attività, permette di monitorare il controllo di accesso (IAM) nella sottoscrizione e offre metriche, avvisi e strumenti di diagnostica e risoluzione dei problemi che assicurano l'accesso di cui si ha bisogno.

Per altre informazioni sull'uso di Azure Key Vault, vedere [Gestire i segreti nelle app server con Azure Key Vault](../../manage-secrets-with-azure-key-vault/index.yml).

## <a name="summary"></a>Riepilogo

Il furto di credenziali, la rotazione manuale delle chiavi e il rinnovo dei certificati possono essere relegati nel passato, se si gestiscono i segreti in modo efficace con Azure Key Vault.
