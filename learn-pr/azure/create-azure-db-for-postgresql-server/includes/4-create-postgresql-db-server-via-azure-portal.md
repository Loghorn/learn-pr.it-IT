Il portale di Azure consente di gestire e ridimensionare i server di database PostgreSQL. Si decide di creare un Database di Azure per il server PostgreSQL per archiviare i dati sulle prestazioni runner. Basato su volumi di dati acquisita cronologici, si conosce che i requisiti di archiviazione server devono essere impostati su 10 GB. Per supportare i requisiti di elaborazione, è necessario calcolare il supporto di generazione 5 con 1 vCore. È inoltre possibile conoscere in genere archiviano i backup da 25 giorni.

[!include[](../../../includes/azure-sandbox-activate.md)]

Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true). Si noterà il menu di creazione e la gestione delle risorse di Azure sulla sinistra e il dashboard di riempire il resto della schermata.

## <a name="create-an-azure-database-for-postgresql-server"></a>Creare un database di Azure per il server PostgreSQL

Dopo l'accesso, si noterà il valore predefinito che Dashboard visualizzato. È necessario un paio di opzioni disponibili per creare un Database di Azure per il server PostgreSQL. Dal Dashboard, è possibile:

- Selezionare il **tutti i servizi** opzione e quindi cercare **per Database di Azure per il server PostgreSQL**. Questa schermata verrà visualizzati tutti i server configurati che sono già nell'account. A questo punto, si seleziona **Add**, che verrà visualizzata per il nuovo pannello di creazione di server.

oppure

- Selezionare il **crea una risorsa** opzione, che verrà visualizzate le opzioni delle risorse di Azure Marketplace. A questo punto, si seleziona il **database** opzione e scegliere **per Database di Azure per PostgreSQL**.

### <a name="configure-the-server"></a>Configurare il server

Si vedrà ora il server PostgreSQL crea pannello simile alla figura seguente.

![Screenshot del portale di Azure che mostra il pannello di creazione per un nuovo database PostgreSQL](../media-draft/4-create-blade.png)

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

> [!NOTE]
> È necessario ricordare o annotare alcuni dettagli di come si crea il server PostgreSQL. Ad esempio, il nome utente e password per accedere al server. Si userà queste informazioni per connettersi al server in un secondo momento.

1. Scegliere un nome univoco per il server. Si tenga presente che sarà quindi nome deve contenere solo lettere minuscolo e può contenere numeri e trattini.

1. Selezionare una sottoscrizione. Verificare che questo campo è impostato per la sottoscrizione che si desidera utilizzare.

1. È ora possibile creare o riusare un gruppo di risorse. Selezionare **Usa esistente** e scegliere <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn> dall'elenco a discesa. Si userà questo gruppo per il resto di questo modulo.

1. Selezionare l'origine del nuovo server. Per questa esercitazione, lasciare l'opzione impostata su _vuoto_. È importante ricordare che è possibile modificare l'opzione _eseguire il backup_ se si desidera ripristinare un backup esistente del server.

1. Scegliere un nome di account di accesso di Usa un account di accesso di amministratore per il nuovo server. È importante ricordare che il nome dell'account di accesso amministratore non può essere azure_superuser, azure_pg_admin, admin, administrator, root, guest o pubblico. Non può iniziare con PG _. Ricordare o annotare il nome per un uso futuro.

1. Scegliere una password da usare con il nome dell'account di accesso amministratore precedente. È importante ricordare, = che la password deve includere caratteri di tre delle categorie seguenti:
   - Lettere maiuscole
   - Lettere minuscole
   - Numeri (da 0 a 9)
   - Caratteri non alfanumerici (!, $, #, % e così via)

   Ricordare o annotare la password per un uso futuro.

1. La password per confermarla.

1. Scegliere un percorso per il server. È opportuno scegliere una località più vicina all'utente.

1. A questo punto si selezionerà la versione del server. Selezionare la versione più recente di PostgreSQL.

1. Come il secondo per l'ultimo passaggio, selezionare la **tariffario** opzione.

    È importante ricordare che è necessario configurare il server con archiviazione specifico e opzioni di calcolo:

    - 10 GB di archiviazione su disco
    - Supporto di 5 di generazione di calcolo
    - Periodo di conservazione di 25 giorni

    Fare clic su **tariffario** accedere il pannello del piano tariffario e apportare le modifiche seguenti:

    - Scegliere il **base** opzione tab.
    - Scegliere il **generazione di calcolo di generazione 5** opzione.
    - 1 vCore da scegliere la **vCore** dispositivo di scorrimento. Si noti che gli effetti delle modifiche nel dispositivo di scorrimento la **riepilogo prezzo**.
    - 10 GB da scegliere la **archiviazione** dispositivo di scorrimento. Se si riescono a scorrimento a esattamente 10 GB, è possibile utilizzare i tasti di direzione sinistra e destra sulla tastiera per ottenere un valore preciso.
    - Scegliere 25 giorni dal **periodo di conservazione dei Backup** dispositivo di scorrimento.

        ![Screenshot del portale di Azure con il database per un database PostgreSQL nuovo piano tariffario](../media-draft/4-azure-db-pricing-tier.png)

    - Fare clic su **OK** dopo aver completato la selezione di confermare le selezioni e chiudere le opzioni del piano tariffario.

1. Tutto ciò che resta è ora a esaminare i valori immessi e fare clic su **Create**. La creazione può richiedere alcuni minuti. È possibile selezionare l'icona delle notifiche (un campanello) nella parte superiore della schermata del portale Azure per monitorare lo stato di avanzamento.

È ora disponibile un server PostgreSQL. Nell'unità di successivo, verrà illustrato come creare lo stesso server tramite la CLI di Azure.
