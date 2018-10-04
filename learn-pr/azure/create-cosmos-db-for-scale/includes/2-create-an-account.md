L'azienda ha scelto Azure Cosmos DB per soddisfare le esigenze della crescente base di prodotti e clienti. Si è ricevuto l'incarico di creare il database.

Il primo passaggio consiste nel creare un account Azure Cosmos DB.

## <a name="what-is-an-azure-cosmos-db-account"></a>Informazioni sugli account Azure Cosmos DB

Un account Azure Cosmos DB è una risorsa di Azure che funge da entità organizzativa per i database. Connette l'utilizzo alla sottoscrizione di Azure ai fini della fatturazione.

Ogni account Azure Cosmos DB è associato a uno o più dei diversi modelli di dati supportati da Azure Cosmos DB ed è possibile creare il numero di account desiderato.

Se si crea una nuova applicazione, il modello di dati preferito è l'API SQL. Se si usano grafi o tabelle oppure si esegue la migrazione di dati MongoDB o Cassandra ad Azure, creare account aggiuntivi e selezionare i modelli di dati pertinenti.

Quanto si crea un account, scegliere un ID significativo con cui verrà identificato l'account. Creare inoltre l'account nell'area di Azure più vicina agli utenti per ridurre al minimo la latenza tra il data center e gli utenti.

Facoltativamente, è possibile configurare le reti virtuali e la ridondanza geografica durante la creazione dell'account, ma queste impostazioni possono essere definite anche in seguito. In questo modulo non verranno abilitate.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="creating-an-azure-cosmos-db-account-in-the-portal"></a>Creare un account Azure Cosmos DB nel portale

1. Accedere al [portale di Azure per sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

    > [!IMPORTANT]
    > Accedere al portale di Azure usando il collegamento precedente per assicurarsi di connettersi alla sandbox, che fornisce l'accesso a una sottoscrizione Concierge.

1. Fare clic su **Crea una risorsa** > **Database** > **Azure Cosmos DB**.

   ![Riquadro Database nel portale di Azure](../media/2-create-nosql-db-databases-json-tutorial.png)

1. Nella pagina **Crea un account Azure Cosmos DB** immettere le impostazioni per il nuovo account Azure Cosmos DB, inclusa la posizione.

    [!INCLUDE[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    Impostazione|Valore|Descrizione
    ---|---|---
    Sottoscrizione|*Concierge Subscription* (Sottoscrizione Concierge)|Selezionare la sottoscrizione Concierge. Se la sottoscrizione Concierge non è elencata, nella sottoscrizione sono abilitati più tenant ed è necessario cambiare tenant. A tale scopo, accedere nuovamente usando il collegamento del portale seguente: [portale di Azure per sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).
    Gruppo di risorse|Usa esistente<br><br>**<rgn>[nome gruppo di risorse sandbox]</rgn>**|Qui si può creare un nuovo gruppo di risorse o selezionarne uno esistente all'interno della sottoscrizione.
    Nome account|*Immettere un nome univoco*|Immettere un nome univoco per identificare l'account Azure Cosmos DB. Dato che, per creare l'URI, all'ID specificato viene aggiunto *documents.azure.com*, usare un ID univoco ma identificabile.<br><br>L'ID può contenere solo lettere minuscole, numeri e il segno meno (-) e deve avere una lunghezza compresa tra 3 e 31 caratteri.
    API|SQL|L'API determina il tipo di account da creare. Azure Cosmos DB offre cinque API per soddisfare le esigenze dell'applicazione: SQL (database di documenti), Gremlin (database a grafo), MongoDB (database di documenti), Tabella di Azure e Cassandra. Per ogni API è attualmente necessario un account separato. <br><br>Selezionare **SQL** perché in questo modulo si crea un database di documenti disponibile per query con sintassi SQL e accessibile con l'API SQL.|
    Località|*Selezionare l'area più vicina nell'elenco precedente*|Selezionare la località in cui posizionare il database.
    Ridondanza geografica| Disabilita | Questa impostazione crea una versione replicata del database in una seconda area associata. Lasciare l'opzione disabilitata per il momento, perché il database può essere replicato in seguito.
    Multi-region Writes | Abilita | Questa impostazione consente di scrivere in più aree nello stesso momento.

1. Fare clic su **Rivedi e crea**.

    ![Pagina del nuovo account per Azure Cosmos DB](../media/2-azure-cosmos-db-create-new-account.png)

1. In seguito alla convalida delle impostazioni, fare clic su **Crea** per creare l'account.

1. La creazione dell'account richiede alcuni minuti. Attendere che nel portale venga visualizzata la notifica del completamento della distribuzione e fare clic sulla notifica.

    ![Avviso di notifica](../media/2-azure-cosmos-db-notification.png)

1. Nella finestra della notifica fare clic su **Vai alla risorsa**.

    ![Vai alla risorsa](../media/2-azure-cosmos-db-go-to-resource.png)

    Nel portale verrà visualizzata la pagina **L'account Azure Cosmos DB è stato creato**.

    ![Riquadro delle notifiche nel portale di Azure](../media/2-azure-cosmos-db-account-created.png)

## <a name="summary"></a>Riepilogo

È stata completata la creazione di un account Azure Cosmos DB, che è il primo passaggio per creare un database Azure Cosmos DB. Sono state selezionate le impostazioni appropriate per i tipi di dati e si è impostata la località dell'account in modo da ridurre al minimo la latenza per gli utenti.
