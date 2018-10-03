Fornire ai clienti l'accesso più rapido ai prodotti in un sito di abbigliamento online è fondamentale per i clienti e il successo di un'azienda. Riducendo la distanza tra i dati e l'utente, è possibile distribuire più contenuti più rapidamente. Se i dati sono archiviati in Azure Cosmos DB, per la replica dei dati del sito in più aree in tutto il mondo è sufficiente un clic.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

In questa unità vengono illustrati i vantaggi della distribuzione globale e un servizio di database multimaster nativo, quindi l'account verrà replicato in tre aree aggiuntive.

## <a name="global-distribution-basics"></a>Nozioni fondamentali sulla distribuzione globale

La distribuzione globale consente di replicare i dati da un'area in più aree di Azure. È possibile aggiungere o rimuovere le aree in cui il database viene replicato in qualsiasi momento e Azure Cosmos DB garantisce che, quando si aggiunge una nuova area, i dati siano disponibili per le operazioni entro 30 minuti, presupponendo che i dati siano di 100 TB o meno.

Esistono due scenari comuni per la replica dei dati in due o più aree:

1. Offerta di accesso ai dati con bassa latenza indipendentemente dalla posizione del mondo in cui si trovano gli utenti finali
2. Aggiunta di resilienza a livello di area per garantire continuità aziendale e ripristino di emergenza (BCDR)

Per offrire accesso con bassa latenza ai clienti, è consigliabile replicare i dati nelle aree più vicine alla posizione degli utenti. L'azienda di abbigliamento online di esempio ha clienti a Los Angeles, New York e Tokyo. Esaminiamo la pagina [Aree di Azure](https://azure.microsoft.com/global-infrastructure/regions/)e determiniamo le aree più vicine a questi gruppi di clienti, in quanto si tratta delle posizioni in cui verranno replicati gli utenti.

Per offrire una soluzione BCDR, è consigliabile aggiungere le aree in base alle coppie di aree descritte nell'articolo [Continuità aziendale e ripristino di emergenza nelle aree geografiche abbinate di Azure](https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/).

Quando un database viene replicato, vengono replicate in modo uniforme anche la velocità effettiva e la risorsa di archiviazione. Pertanto, se il database originale ha uno spazio di archiviazione di 10 GB e una velocità effettiva di 1.000 UR/s e viene replicato in altre tre aree, ogni area includerà 10 GB di dati e 1.000 UR/s di velocità effettiva. Poiché la risorsa di archiviazione e la velocità effettiva vengono replicate in ogni area, il costo della replica in un'area è lo stesso dell'area originale, pertanto la replica in 3 aree aggiuntive costerà circa quattro volte in più rispetto al database originale non replicato.

## <a name="creating-an-azure-cosmos-db-account-in-the-portal"></a>Creare un account Azure Cosmos DB nel portale

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato il sandbox.

    > [!IMPORTANT]
    > Accedere al portale di Azure e all'ambiente sandbox con lo stesso account.
    >
    > Accedere al portale di Azure usando il collegamento precedente per assicurarsi di connettersi connessi al sandbox, che fornisce l'accesso a una sottoscrizione Concierge.

1. Fare clic su **Crea una risorsa** > **Database** > **Azure Cosmos DB**.

   ![Riquadro Database nel portale di Azure](../media/2-global-distribution/2-create-nosql-db-databases-json-tutorial.png)

1. Nella pagina **Crea un account Azure Cosmos DB** immettere le impostazioni per il nuovo account Azure Cosmos DB, inclusa la posizione.

    <!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    Impostazione|Valore|Descrizione
    ---|---|---
    Sottoscrizione|*Concierge Subscription* (Sottoscrizione Concierge)|Selezionare la sottoscrizione Concierge. Se la sottoscrizione Concierge non è elencata, nella sottoscrizione sono abilitati più tenant ed è necessario cambiare tenant. A tale scopo, accedere nuovamente usando il collegamento del portale seguente: [portale di Azure per sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).
    Gruppo di risorse|Usa esistente<br><br><rgn>[Nome gruppo di risorse sandbox]</rgn>|Selezionare **Usa esistente**, quindi immettere il <rgn>[Nome gruppo di risorse sandbox]</rgn>.
    Nome account|*Immettere un nome univoco*|Immettere un nome univoco per identificare l'account Azure Cosmos DB. Dato che, per creare l'URI, all'ID specificato viene aggiunto *documents.azure.com*, usare un ID univoco ma identificabile.<br><br>L'ID può contenere solo lettere minuscole, numeri e il segno meno (-) e deve avere una lunghezza compresa tra 3 e 31 caratteri.
    API|SQL|L'API determina il tipo di account da creare. Azure Cosmos DB offre cinque API per soddisfare le esigenze dell'applicazione: SQL (database di documenti), Gremlin (database a grafo), MongoDB (database di documenti), Tabella di Azure e Cassandra. Per ogni API è attualmente necessario un account separato. <br><br>Selezionare **SQL** perché in questo modulo si crea un database di documenti disponibile per query con sintassi SQL e accessibile con l'API SQL.|
    Posizione|*Selezionare l'area più vicina*|Selezionare l'area più vicina all'utente nell'elenco delle aree precedente.
    Ridondanza geografica| Disabilita | Questa impostazione crea una versione replicata del database in una seconda area associata. Per il momento lasciarla disabilitata, in quanto il database verrà replicato in un secondo momento.
    Multi-region Writes | Abilita | Questa impostazione consente di scrivere in più aree nello stesso momento. Questa impostazione può essere configurata solo durante la creazione dell'account.

1. Fare clic su **Rivedi e crea**.

    ![Pagina del nuovo account per Azure Cosmos DB](../media/2-global-distribution/2-azure-cosmos-db-create-new-account.png)

1. In seguito alla convalida delle impostazioni, fare clic su **Crea** per creare l'account.

1. La creazione dell'account richiede alcuni minuti. Attendere che nel portale venga visualizzata la notifica del completamento della distribuzione e fare clic sulla notifica.

    ![Avviso di notifica](../media/2-global-distribution/2-azure-cosmos-db-notification.png)

1. Nella finestra della notifica fare clic su **Vai alla risorsa**.

    ![Vai alla risorsa](../media/2-global-distribution/2-azure-cosmos-db-go-to-resource.png)

    Nel portale verrà visualizzata la pagina **L'account Azure Cosmos DB è stato creato**.

    ![Riquadro delle notifiche nel portale di Azure](../media/2-global-distribution/2-azure-cosmos-db-account-created.png)

## <a name="replicate-data-in-multiple-regions"></a>Replicare i dati in più aree

È possibile ora eseguire la replica del database più vicino agli utenti globali a Los Angeles, New York e Tokyo.

1. Nella pagina dell'account fare clic su **Replica i dati a livello globale** dal menu.
1. Nella pagina **Replica i dati a livello globale** selezionare le aree Stati Uniti occidentali 2, Stati Uniti orientali e Giappone orientale e quindi fare clic su **Salva**.

    Se non viene visualizzata la mappa nel portale di Azure, ridurre a icona i menu del lato sinistro della schermata.

    La pagina mostra un messaggio di **aggiornamento** mentre i dati vengono scritti nelle nuove aree. I dati nelle nuove aree saranno disponibili entro 30 minuti.

    ![Fare clic sulle aree nella mappa per aggiungerle](../media/2-global-distribution/2-global-replication.gif)

## <a name="summary"></a>Riepilogo

In questa unità il database è stato replicato nelle aree del mondo in cui sono concentrati più utenti, garantendo loro accesso a latenza inferiore ai dati nel sito.
