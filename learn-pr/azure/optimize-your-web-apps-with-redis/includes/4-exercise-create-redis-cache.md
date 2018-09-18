È ora possibile creare un'istanza di Cache Redis di Azure per archiviare e restituire valori usati comunemente.

<!-- TODO: do we need to activate the sandbox here? -->

## <a name="create-a-redis-cache-in-azure"></a>Creare una cache Redis in Azure

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Fare quindi clic su **Crea una risorsa**, su **Database** e quindi su **Cache Redis**.

    Lo screenshot seguente mostra la posizione di Cache Redis all'interno delle varie opzioni per le risorse di database nel portale di Azure.

    ![Screenshot che mostra le opzioni per i database del portale di Azure, con le opzioni Crea una risorsa, Database e Cache Redis evidenziate.](../media/4-create-a-cache-1.png)

### <a name="identify-the-location-for-the-cache"></a>Identificare la posizione per la cache

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="configure-your-cache"></a>Configurare la cache

Applicare le impostazioni seguenti nella cache.

1. **Nome DNS:** creare un nome globalmente univoco, ad esempio **ContosoSportsApp1028**.

1. **Sottoscrizione:** selezionare la sottoscrizione di Azure Sandbox.

1. **Gruppo di risorse:** selezionare <rgn>[nome del gruppo di risorse del sandbox]</rgn> per il gruppo di risorse.

1. **Località:** è in genere consigliabile selezionare una località vicina ai clienti, in questo caso la costa orientale degli Stati Uniti. Azure Sandbox consente tuttavia di selezionare solo aree specifiche per le risorse, come indicato in precedenza. Selezionare una di tali località.

1. **Piano tariffario:** selezionare **Basic C0**. Questo è il livello minimo consentito. È probabile che le app di produzione richiedano l'archiviazione di una quantità maggiore di dati e l'utilizzo di alcune funzionalità Premium come il clustering, che necessita di un livello superiore.

1. Fare clic su **Crea**.

    Lo screenshot seguente mostra una configurazione rappresentativa usata per creare una nuova risorsa di Cache Redis. Si noti che la configurazione specifica sarà leggermente diversa a causa di Azure Sandbox.

    ![Screenshot che mostra il pannello del portale di Azure quando si crea una nuova risorsa di Cache Redis, popolato con un nome DNS della configurazione di esempio, sottoscrizione, nuovo gruppo di risorse, area e piano tariffario.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> Si dovrà attendere che la cache venga distribuita prima di continuare. Questo processo può richiedere del tempo.

## <a name="retrieve-the-access-keys-and-host-name"></a>Recuperare le chiavi di accesso e il nome host

1. Passare alla nuova istanza della cache nel portale di Azure e selezionare **Impostazioni** > **Chiavi di accesso**. 

1. Copiare la **Stringa di connessione primaria (StackExchange.Redis)** in una posizione sicura. Sarà necessaria per l'esercizio successivo.

    Questa chiave include la chiave primaria e il nome host in una stringa di connessione completa per l'uso all'interno delle impostazioni dell'applicazione per il pacchetto **StackExchange.Redis** che verrà usato.

Nelle unità seguenti verranno illustrati i comandi disponibili per interrogare la cache.