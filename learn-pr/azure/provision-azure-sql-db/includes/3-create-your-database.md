L'azienda di trasporti vuole distinguersi dalla concorrenza, ma senza costi eccessivi. È necessario comprendere a fondo come configurare il database per offrire il miglior servizio e al tempo stesso controllare i costi.

[!include[](../../../includes/azure-sandbox-activate.md)]

Si apprenderà:

- Quali aspetti prendere in considerazione al momento della creazione di un database SQL di Azure, ad esempio:
  - Ruolo di un server logico come contenitore amministrativo per i database.
  - Differenze tra i modelli di acquisto.
  - Come usare i pool elastici per condividere la potenza di elaborazione tra i database.
  - Influenza delle regole di confronto sul modo in cui vengono confrontati e ordinati i dati.
- Come accedere al database SQL di Azure dal portale.
- Come aggiungere regole del firewall in modo da rendere accessibile il database solo da origini attendibili.

Esaminiamo rapidamente alcuni degli aspetti da considerare al momento della creazione di un database SQL di Azure.

## <a name="one-server-many-databases"></a>Un server, molti database

Quando si crea il primo database SQL di Azure, viene creato anche un _server logico SQL di Azure_. Un server logico è paragonabile a un contenitore amministrativo per i database. È possibile controllare gli account di accesso, le regole del firewall e i criteri di sicurezza tramite il server logico. È anche possibile eseguire l'override di questi criteri in ogni database all'interno del server logico.

Per il momento, è sufficiente un solo database. Tuttavia, un server logico consente di aggiungerne altri più avanti e di ottimizzare le prestazioni tra tutti i database.

## <a name="choose-performance-dtus-versus-vcores"></a>Scegliere le prestazioni: DTU o vCore

Il database SQL di Azure offre due modelli di acquisto: DTU e vCore.

### <a name="what-are-dtus"></a>Che cosa sono le DTU?

Le DTU (Database Transaction Unit) o unità di transazione di database rappresentano una misura combinata di risorse di calcolo, archiviazione e I/O. Il modello basato su DTU offre un'opzione di acquisto semplice e preconfigurata.

Poiché il server logico può contenere più di un database, esistono anche le eDTU (elastic Database Transaction Unit), o unità di transazione di database elastico. Con questa opzione è possibile scegliere un unico prezzo, ma consentire a ogni database nel pool di usare un numero inferiore o superiore di risorse in base al carico corrente.

### <a name="what-are-vcores"></a>Che cosa sono i vCore?

I vCore offrono maggiore controllo sulle risorse di calcolo e archiviazione create e per cui vengono effettuati pagamenti.

Mentre il modello basato su DTU offre combinazioni fisse di risorse di calcolo, archiviazione e I/O, il modello basato su vCore consente di configurare le risorse in modo indipendente. Ad esempio, con il modello basato su vCore è possibile aumentare la capacità di archiviazione, ma mantenere la quantità esistente di risorse di calcolo e velocità effettiva di I/O.

Il prototipo per i trasporti e la logistica richiede una sola istanza del database SQL di Azure. Si sceglie l'opzione DTU perché fornisce un buon compromesso tra calcolo, archiviazione e prestazioni di I/O, oltre a essere meno costosa per iniziare.

## <a name="what-are-sql-elastic-pools"></a>Che cosa sono i pool elastici SQL?

Quando si crea il database SQL di Azure, è possibile creare un _pool elastico SQL_.

I pool elastici SQL sono correlati alle eDTU. Consentono di acquistare un set di risorse di calcolo e archiviazione che vengono condivise tra tutti i database nel pool. Ogni database può usare le risorse necessarie, entro i limiti impostati, a seconda del carico corrente.

Per il prototipo, non è necessario un pool elastico SQL perché è richiesto un solo database SQL.

## <a name="what-is-collation"></a>Che cosa sono le regole di confronto?

Le regole di confronto si riferiscono alle regole per l'ordinamento e il confronto dei dati. Consentono di definire le regole di ordinamento quando la distinzione tra maiuscole e minuscole, gli accenti e altre caratteristiche linguistiche sono importanti.

Esaminiamo velocemente il significato delle regole di confronto predefinite **SQL_Latin1_General_CP1_CI_AS**.

- **Latin1_General** si riferisce alla famiglia di lingue dell'Europa occidentale.
- **CP1** fa riferimento alla tabella codici 1252, una codifica dei caratteri dell'alfabeto latino molto diffusa.
- **CI** indica che nei confronti non viene fatta distinzione tra maiuscole e minuscole. Ad esempio, "HELLO" viene confrontato in modo equivalente a "hello".
- **AS** indica che nei confronti viene fatta distinzione tra le lettere accentate. Ad esempio, "résumé" non viene confrontato in modo equivalente a "resume".

Poiché non esistono requisiti specifici per la modalità di ordinamento e confronto dei dati, è possibile scegliere le regole di confronto predefinite.

## <a name="create-your-azure-sql-database"></a>Creare il database SQL di Azure

A questo punto, è possibile eseguire la configurazione del database, che include la creazione del server logico. Si sceglieranno impostazioni che supportano l'applicazione di logistica per i trasporti. Nella pratica, sarebbe necessario scegliere le impostazioni che supportano il tipo di app da creare.

Se nel corso del tempo ci si rende conto che la domanda richiede un incremento della potenza di calcolo, è possibile modificare le opzioni relative alle prestazioni o passare dal modello di prestazioni DTU al modello vCore e viceversa.

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Nell'angolo superiore sinistro del portale fare clic su **Crea una risorsa**. Selezionare **Database**, quindi selezionare **Database SQL**.

   ![Screenshot del portale di Azure con il pannello Crea una risorsa, la sezione Database selezionata e i pulsanti Crea una risorsa, Database, e Database SQL evidenziati.](../media/3-create-db.png)

1. In **Server** fare clic su **Configura impostazioni necessarie**, compilare il modulo, quindi fare clic su **Seleziona**. Ecco altre informazioni su come compilare il modulo:

    | Impostazione      | Valore |
    | ------------ | ----- |
    | **Nome server** | [Nome server](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) univoco a livello globale. |
    | **Accesso amministratore server** | [Identificatore di database](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) usato come nome di accesso dell'amministratore primario. |
    | **Password** | Qualsiasi password valida che contiene almeno otto caratteri delle tre categorie seguenti: maiuscole, minuscole, numeri e caratteri non alfanumerici. |
    | **Posizione** | Selezionare una posizione valida dall'elenco disponibile di seguito. |

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. Fare clic su **Piano tariffario** per specificare il livello di servizio. Selezionare il livello di servizio **Basic**, quindi fare clic su **Applica**.

1. Usare i valori seguenti per compilare il resto del modulo.

    | Impostazione      | Valore |
    | ------------ | ----- |
    | **Nome database** | **Logistics** |
    | **Sottoscrizione** | Sottoscrizione in uso |
    | **Gruppo di risorse** |  Usare il gruppo esistente <rgn>[Nome gruppo di risorse sandbox]</rgn> |
    | **Seleziona origine** | **Database vuoto** |
    | **Usare il pool elastico SQL?** | **Non ora** |
    | **Regole di confronto** | **SQL_Latin1_General_CP1_CI_AS** |

1. Fare clic su **Crea** per creare il database SQL di Azure.

    > [!IMPORTANT]
    > Prendere nota del nome del server, dell'account di accesso dell'amministratore e della password per usarli in seguito.

1. Nella barra degli strumenti fare clic su **Notifiche** per monitorare il processo di distribuzione.

Al termine del processo, fare clic su **Aggiungi al dashboard** per aggiungere il server di database al dashboard, in modo da potervi accedere rapidamente in un secondo momento.

   ![Screenshot del portale di Azure che visualizza il menu Notifiche e il pulsante Aggiungi al dashboard evidenziato per un messaggio di distribuzione recente riuscita.](../media/3-notifications-complete.png)

## <a name="set-the-server-firewall"></a>Impostare il firewall del server

A questo punto, il database SQL di Azure è operativo. Sono disponibili numerose opzioni per configurare, proteggere, monitorare e risolvere i problemi del nuovo database.

È anche possibile specificare i sistemi che possono accedere al database attraverso il firewall. Inizialmente, il firewall impedisce qualsiasi accesso al server di database dall'esterno di Azure.

Per il prototipo, è sufficiente accedere al database dal proprio portatile. In seguito, sarà possibile consentire altri sistemi, ad esempio l'app per dispositivi mobili.

Vediamo come abilitare il computer di sviluppo per l'accesso al database attraverso il firewall.

1. Passare al pannello Panoramica del database Logistics. Se il database è stato aggiunto al dashboard, è possibile fare clic sul riquadro **Logistica** nel dashboard per accedervi.

1. Fare clic su **Imposta firewall server**.

    ![Screenshot del portale di Azure che visualizza un pannello di panoramica del database SQL con il pulsante Imposta firewall server evidenziato.](../media/3-set-server-firewall.png)

1. Fare clic su **Aggiungi IP client** e fare clic su **Salva**.

    ![Screenshot del portale di Azure con la scheda Impostazioni firewall di un database SQL e il pulsante Aggiungi IP client evidenziato.](../media/3-add-client-ip.png)

Nella parte successiva si eseguiranno alcune esercitazioni pratiche con il nuovo database e con Azure Cloud Shell. Verrà descritto come connettersi al database, creare una tabella, aggiungere alcuni dati di esempio ed eseguire alcune istruzioni SQL.