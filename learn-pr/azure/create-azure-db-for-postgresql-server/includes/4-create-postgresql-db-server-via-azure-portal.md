Il portale di Azure consente di gestire e ridimensionare i server di database PostgreSQL. Si decide di creare un server di Database di Azure per PostgreSQL per archiviare i dati sulle prestazioni dei runner. In base ai volumi di dati cronologici acquisiti si sa che i requisiti di archiviazione del server devono essere impostati su 10 GB. Per supportare i requisiti di elaborazione è necessario il supporto di una risorsa di calcolo Gen 5 con 1 vCore. Si sa anche che in genere si archiviano backup per 25 giorni.

[!include[](../../../includes/azure-sandbox-activate.md)]

Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.

Il menu per la creazione e la gestione delle risorse di Azure verrà visualizzato sulla sinistra, mentre il resto della schermata sarà occupato dal dashboard.

## <a name="create-an-azure-database-for-postgresql-server"></a>Creare un server di Database di Azure per PostgreSQL

Dopo l'accesso verrà visualizzato il dashboard predefinito. Sono disponibili due opzioni per creare un server di Database di Azure per PostgreSQL. Nel dashboard è possibile:

- Selezionare l'opzione **Tutti i servizi** e quindi cercare **Database di Azure per il server PostgreSQL**. In questa schermata verranno visualizzati tutti i server configurati già presenti nell'account. Selezionare quindi **Aggiungi** per visualizzare il nuovo pannello per la creazione del server.

**oppure**

- Selezionare l'opzione **Crea una risorsa** per visualizzare le opzioni per le risorse di Azure Marketplace. Selezionare quindi l'opzione **Database** e scegliere **Database di Azure per PostgreSQL**.

### <a name="configure-the-server"></a>Configurare il server

Verrà visualizzato ora il pannello per la creazione del server PostgreSQL simile a quello della figura seguente.

![Screenshot del portale di Azure che illustra il pannello di creazione di un nuovo database PostgreSQL](../media/4-create-blade.png)

> [!NOTE]
> È necessario ricordare alcuni dettagli quando si crea il server PostgreSQL. Ad esempio, il nome utente e la password per accedere al server. Queste informazioni verranno usate in seguito per connettersi al server.

1. Scegliere un nome univoco per il server. Il nome del server deve essere composto solo da lettere minuscole e può contenere numeri e trattini.

1. Selezionare una sottoscrizione. Verificare che questo campo sia impostato sulla sottoscrizione da usare.

1. È ora possibile creare o usare di nuovo un gruppo di risorse esistente. Selezionare **Usa esistente** e scegliere "<rgn>[Nome gruppo di risorse sandbox]</rgn>" nell'elenco a discesa. Questo gruppo verrà usato per il resto del modulo.

1. Selezionare l'origine del nuovo server. Per questo lab lasciare l'opzione impostata su _Vuoto_. È possibile impostare l'opzione su _Backup_ se si vuole ripristinare un backup del server esistente.

1. Scegliere un nome di accesso da usare come account di accesso dell'amministratore per il nuovo server. Il nome di accesso dell'amministratore non può essere azure_superuser, azure_pg_admin, admin, administrator, root, guest o public. Non può iniziare con pg_. Ricordare o annotare il nome per un uso futuro.

1. Scegliere una password da usare con il nome di accesso dell'amministratore. Ricordare che la password deve contenere caratteri di tre delle categorie seguenti:
   - Lettere maiuscole
   - Lettere minuscole
   - Numeri (da 0 a 9)
   - Caratteri non alfanumerici (!, $, #, % e così via)

1. Digitare di nuovo la password per conferma.

1. Scegliere una località per il server. È opportuno scegliere la località più vicina all'utente nell'elenco seguente.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]


1. Selezionare ora la versione del server. Selezionare la versione più recente di PostgreSQL.

1. Come secondo e ultimo passaggio selezionare l'opzione **Piano tariffario**.

    È necessario configurare il server con opzioni di calcolo e di archiviazione specifiche:

    - 10 GB di spazio di archiviazione su disco
    - Supporto di calcolo Generazione 5
    - Periodo di conservazione di 25 giorni

    Fare clic su **Piano tariffario** per accedere al pannello del piano tariffario e apportare le modifiche seguenti:

    - Scegliere la scheda **Basic**.
    - Scegliere l'opzione **Generazione calcolo Gen 5**.
    - Scegliere 1 vCore dal dispositivo di scorrimento **vCore**. Si noti come le modifiche nel dispositivo di scorrimento influiscano sul **Riepilogo prezzi**.
    - Scegliere 10 GB dal dispositivo di scorrimento **Archiviazione**. Se si ha difficoltà a selezionare esattamente 10 GB, è possibile usare i tasti di direzione destro e sinistro della tastiera per ottenere il valore preciso.
    - Scegliere 25 giorni dal dispositivo di scorrimento **Periodo di conservazione backup**.
    - Lasciare le opzioni di ridondanza del backup impostate su **Con ridondanza locale**.

    ![Screenshot del portale di Azure che illustra il piano tariffario del database per un nuovo database PostgreSQL](../media/4-azure-db-pricing-tier.png)

1. Controllare il riepilogo dei prezzi sulla destra. Fornisce una suddivisione dei costi del server e un costo mensile stimato.

1. Fare clic su **OK** dopo aver completato le selezioni per confermarle e chiudere le opzioni del piano tariffario.

1. A questo punto, rimane solo da controllare i valori immessi e fare clic su **Crea**. La creazione può richiedere alcuni minuti. È possibile selezionare l'icona delle notifiche (un campanello) nella parte superiore della schermata del portale di Azure per monitorare lo stato di avanzamento.

È ora disponibile un server PostgreSQL. Nell'unità successiva verrà illustrato come creare lo stesso server tramite l'interfaccia della riga di comando di Azure.
