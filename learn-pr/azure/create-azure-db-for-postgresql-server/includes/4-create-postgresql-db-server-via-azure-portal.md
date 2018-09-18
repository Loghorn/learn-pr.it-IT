Il portale di Azure consente di gestire e ridimensionare i server di database PostgreSQL. Si decide di creare un server di Database di Azure per PostgreSQL per archiviare i dati sulle prestazioni dei runner. In base ai volumi di dati cronologici acquisiti si sa che i requisiti di archiviazione del server devono essere impostati su 10 GB. Per supportare i requisiti di elaborazione è necessario il supporto di una risorsa di calcolo Gen 5 con 1 vCore. Si sa anche che in genere si archiviano backup per 25 giorni.

> [!TIP]
> Tutti gli esercizi di Microsoft Learn sono gratuiti, ma quando si inizia a esplorare per proprio conto, è necessario procurarsi una sottoscrizione di Azure. Se non si dispone di una sottoscrizione, creare un account [gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) in pochi minuti.

Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true). Si noterà il menu per la creazione di risorse e la gestione di Azure sulla sinistra, mentre il dashboard riempirà il resto della schermata.

## <a name="create-an-azure-database-for-postgresql-server"></a>Creare un server di Database di Azure per PostgreSQL

Dopo l'accesso verrà visualizzato il Dashboard predefinito. Sono disponibili due opzioni per creare un server di Database di Azure per PostgreSQL. Nel dashboard è possibile:

- Selezionare l'opzione **Tutti i servizi** e quindi cercare l'opzione **Database di Azure per il server PostgreSQL**. In questa schermata verranno visualizzati tutti i server configurati già presenti nell'account. Selezionare quindi **Aggiungi** per visualizzare il nuovo pannello per la creazione del server.

oppure

- Selezionare l'opzione **Crea una risorsa** per visualizzare le opzioni per le risorse di Azure Marketplace. Selezionare quindi l'opzione Database e scegliere **Database di Azure per PostgreSQL**.

### <a name="configure-the-server"></a>Configurare il server

Verrà visualizzato ora il pannello per la creazione del server PostgreSQL simile a quello della figura seguente.

![Pannello dell'interfaccia utente per la creazione del server PostgreSQL nel portale di Azure](../media-draft/4-create-blade.png)

> [!NOTE]
> È necessario ricordare o annotare alcuni dettagli quando si crea il server PostgreSQL. Ad esempio, il nome utente e la password per accedere al server. Queste informazioni verranno usate in seguito per connettersi al server.

1. Scegliere un nome univoco per il server. Il nome del server deve essere composto solo da lettere minuscole e può contenere numeri e trattini.

1. Selezionare una sottoscrizione, verificare che questo campo sia impostato sulla sottoscrizione da usare.

1. È ora possibile creare o riusare una risorsa esistente. Per creare un nuova risorsa, selezionare il pulsante di opzione **Crea nuovo** e immettere un nome per il nuovo gruppo di risorse. Questo gruppo verrà usato per il resto del modulo. Assegnare alla risorsa un nome descrittivo in modo che in seguito risulti semplice eliminare la risorsa.

1. Selezionare l'origine del nuovo server. Per questo lab lasciare l'opzione impostata su _Vuoto_. È possibile impostare l'opzione su _Backup_ se si vuole ripristinare un backup del server esistente.

1. Scegliere un nome di accesso da usare come account di accesso dell'amministratore per il nuovo server. Il nome di accesso dell'amministratore non può essere azure_superuser, azure_pg_admin, admin, administrator, root, guest o public. Non può iniziare con pg_. Ricordare o annotare il nome per un uso futuro.

1. Scegliere una password da usare con il nome di accesso dell'amministratore. La password deve contenere caratteri di tre delle categorie seguenti: lettere maiuscole, lettere minuscole, numeri (da 0 a 9) e caratteri non alfanumerici (!, $, #, % e così via). Ricordare o annotare la password per un uso futuro.

1. Ridigitare la password per conferma.

1. Scegliere una località per il server. È opportuno scegliere una località più vicina all'utente.

1. Selezionare ora la versione del server. Selezionare la versione più recente di PostgreSQL.

1. Come secondo e ultimo passaggio selezionare l'opzione **Piano tariffario**.

    È necessario configurare il server con opzioni di calcolo e archiviazione specifiche.

    - 10 GB di spazio di archiviazione su disco
    - Supporto di calcolo Generazione 5
    - Periodo di conservazione di 25 giorni

    Fare clic su **Piano tariffario** per accedere al pannello del piano tariffario e apportare le modifiche seguenti: ![Pulsante del piano tariffario](../media-draft/4-azure-db-pricing-tier-button.png)

    - Scegliere la scheda **Basic**.
    - Scegliere l'opzione **Generazione calcolo Gen 5**.
    - Scegliere 1 vCore dal dispositivo di scorrimento **vCore**. Si noti come le modifiche nel dispositivo di scorrimento influiscano sul **Riepilogo prezzi**.
    - Scegliere 10 GB dal dispositivo di scorrimento **Archiviazione**. Se si ha difficoltà a selezionare esattamente 10 GB, è possibile usare i tasti di direzione destro e sinistro della tastiera per ottenere il valore preciso.
    - Scegliere 25 giorni dal dispositivo di scorrimento **Periodo di conservazione backup**.

    ![Piano tariffario](../media-draft/4-azure-db-pricing-tier.png)
    - Fare clic su **OK** dopo aver completato le selezioni per confermarle e chiudere le opzioni del piano tariffario.

1. A questo punto, rimane solo da controllare i valori immessi e fare clic su **Crea**. La creazione può richiedere alcuni minuti. È possibile selezionare l'icona delle notifiche (un campanello) nella parte superiore della schermata del portale di Azure per monitorare lo stato di avanzamento.

È ora disponibile un server PostgreSQL. Nell'unità successiva verrà illustrato come creare lo stesso server tramite l'interfaccia della riga di comando di Azure.
