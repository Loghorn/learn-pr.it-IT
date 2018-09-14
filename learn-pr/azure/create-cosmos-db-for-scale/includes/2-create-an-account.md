L'azienda ha scelto Azure Cosmos DB per soddisfare le esigenze della crescente base di prodotti e clienti. Si è ricevuto l'incarico di creare il database.

Il primo passaggio consiste nel creare un account Azure Cosmos DB.

## <a name="what-is-an-azure-cosmos-db-account"></a>Informazioni sugli account Azure Cosmos DB

Un account Azure Cosmos DB è una risorsa di Azure che funge da un'entità dell'organizzazione per i database. Connette l'utilizzo alla sottoscrizione di Azure ai fini della fatturazione.

Ogni account Azure Cosmos DB è associato a uno o più dei diversi modelli di dati supportati da Azure Cosmos DB ed è possibile creare il numero di account desiderato. 

Se si crea una nuova applicazione, il modello di dati preferito è l'API SQL. Se si usano grafi o tabelle oppure si esegue la migrazione di dati MongoDB o Cassandra ad Azure, creare account aggiuntivi e selezionare i modelli di dati pertinenti.

Quanto si crea un account, scegliere un ID significativo con cui verrà identificato l'account. Creare inoltre l'account nell'area di Azure più vicina agli utenti per ridurre al minimo la latenza tra il data center e gli utenti.

Facoltativamente, è possibile configurare le reti virtuali e la ridondanza geografica durante la creazione dell'account, ma queste impostazioni possono essere definite anche in seguito. In questo modulo non verranno abilitate.

## <a name="creating-an-azure-cosmos-db-account-in-the-portal"></a>Creare un account Azure Cosmos DB nel portale

[!include[](../../../includes/azure-sandbox-activate.md)]

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Fare clic su **Crea una risorsa** > **Database** > **Azure Cosmos DB**.
   
   ![Riquadro Database nel portale di Azure](../media-draft/2-create-nosql-db-databases-json-tutorial.png)

1. Nel **nuovo account** pagina, immettere le impostazioni per il nuovo account Azure Cosmos DB, incluso il percorso.

    [!INCLUDE[](../../../includes/azure-sandbox-regions-first-mention-note.md)]
 
    Impostazione|Valore|Descrizione
    ---|---|---
    ID|*Immettere un nome univoco*|Immettere un nome univoco per identificare l'account Azure Cosmos DB. Dato che all'ID specificato viene aggiunto *documents.azure.com* per creare l'URI, usare un ID univoco ma identificabile.<br><br>L'ID può contenere solo lettere minuscole, numeri e il segno meno (-) e deve avere una lunghezza compresa tra 3 e 50 caratteri.
    API|SQL|L'API determina il tipo di account da creare. Azure Cosmos DB offre cinque API per soddisfare le esigenze dell'applicazione: SQL (database di documenti), Gremlin (database a grafo), MongoDB (database di documenti), Tabella di Azure e Cassandra. Per ogni API è attualmente necessario un account separato. <br><br>Selezionare **SQL** perché in questo modulo si crea un database di documenti disponibile per query con sintassi SQL e accessibile con l'API SQL.|
    Sottoscrizione|*Sottoscrizione in uso*|Selezionare la sottoscrizione di Azure da usare per l'account Azure Cosmos DB.
    Gruppo di risorse|Usa esistente<br><br><rgn>[Nome gruppo di risorse di tipo sandbox]</rgn>|Selezionare **Usa esistente**, quindi immettere <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>. 
    Posizione|*Selezionare l'area più vicina all'utente*|Selezionare l'area più vicina all'utente.
    Abilita ridondanza geografica| Lasciare vuoto | Questa impostazione crea una versione replicata del database in una seconda area associata. Lasciare vuoto per il momento, perché il database può essere replicato in seguito.
    Abilita multimaster | Select | Questa impostazione consente di scrivere in più aree nello stesso momento. Questa impostazione può essere configurata solo durante la creazione dell'account.
    Reti virtuali|Disabilitate|Per il momento, non abilitare le reti virtuali. Possono essere abilitate in seguito.

1. Fare clic su **Crea**.

    ![Pagina del nuovo account per Azure Cosmos DB](../media-draft/2-azure-cosmos-db-create-new-account.png)

1. La creazione dell'account richiede alcuni minuti. Attendere che nel portale venga visualizzata la notifica del completamento della distribuzione e fare clic sulla notifica. 

    ![Avviso di notifica](../media-draft/2-azure-cosmos-db-notification.png)

1. Nella finestra della notifica fare clic su **Vai alla risorsa**.

    ![Vai alla risorsa](../media-draft/2-azure-cosmos-db-go-to-resource.png)

    Nel portale verrà visualizzata la pagina **L'account Azure Cosmos DB è stato creato**.

    ![Riquadro delle notifiche nel portale di Azure](../media-draft/2-azure-cosmos-db-account-created.png)

## <a name="summary"></a>Riepilogo

È stata completata la creazione di un account Azure Cosmos DB, che è il primo passaggio per creare un database Azure Cosmos DB. Sono state selezionate le impostazioni appropriate per i tipi di dati e si è impostata la località dell'account in modo da ridurre al minimo la latenza per gli utenti.
