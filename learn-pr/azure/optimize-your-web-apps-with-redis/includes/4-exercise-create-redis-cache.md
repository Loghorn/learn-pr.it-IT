È ora possibile creare un'istanza di Cache Redis di Azure per archiviare e restituire valori usati comunemente.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-redis-cache-in-azure"></a>Creare una cache Redis in Azure

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Fare quindi clic su **Crea una risorsa**, su **Database** e quindi su **Cache Redis**.

    Lo screenshot seguente mostra la posizione di Cache Redis all'interno delle varie opzioni per le risorse di database nel portale di Azure.

    ![Screenshot che mostra le opzioni per i database del portale di Azure, con le opzioni Crea una risorsa, Database e Cache Redis evidenziate.](../media/4-create-a-cache-1.png)

### <a name="configure-your-cache"></a>Configurare la cache

Applicare le impostazioni seguenti nella cache.

1. **Nome DNS:** creare un nome globalmente univoco, ad esempio **ContosoSportsApp [nnn]**, dove `[nnn]` viene sostituito con numeri casuali.

1. **Sottoscrizione:** selezionare la sottoscrizione Concierge.

1. **Gruppo di risorse:** selezionare <rgn>[nome del gruppo di risorse sandbox]</rgn> per il gruppo di risorse.

1. **Località:** è in genere consigliabile selezionare una località vicina ai clienti, in questo caso la costa orientale degli Stati Uniti.

    [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]

5. **Piano tariffario:** selezionare **Basic C0**. Questo è il livello minimo consentito. È probabile che le app di produzione richiedano l'archiviazione di una quantità maggiore di dati e l'utilizzo di alcune funzionalità Premium come il clustering, che necessita di un livello superiore.

1. Fare clic su **Crea**. Azure creerà e distribuirà l'istanza di Cache Redis.

    > [!IMPORTANT]
    > In genere, la risorsa di Cache Redis sarà creata e visibile molto rapidamente nel portale di Azure, ma non sarà disponibile per alcuni minuti. I passaggi successivi illustrano come verificare lo stato della cache.

## <a name="verify-your-data"></a>Verificare i dati

È possibile usare la funzionalità **Console** nel portale di Azure per inviare comandi all'istanza di Cache Redis dopo la creazione.

1. Al termine della distribuzione, individuare la cache Redis tramite il popup **Notifica** selezionando **Tutte le risorse** nella barra laterale sinistra e usando la casella di filtro a sinistra per selezionare le istanze di Cache Redis. In alternativa, è possibile usare la casella di ricerca nella parte superiore e digitare il nome della cache.

1. Selezionare l'istanza della cache Redis.

1. Controllare il valore del campo "Stato". La cache non è pronta finché lo stato non è "In esecuzione". Potrebbe essere necessario attendere qualche minuto prima di procedere.

1. Quando la cache è in esecuzione, fare clic sul pulsante `>_ Console` sulla barra degli strumenti nel pannello **Panoramica** della Cache Redis. Verrà aperta una console di Redis, che consente di immettere i comandi di Redis a basso livello. Provare alcune delle operazioni seguenti:

    ```console
    ping
    ```

    ```output
    PONG
    ```

    ```console
    set test one
    ```

    ```output
    OK
    ```

    ```console
    get test
    ```

    ```output
    "one"
    ```

Tornare al pannello **Panoramica** tramite la barra di navigazione nella parte superiore oppure usare la barra di scorrimento per scorrere la visualizzazione verso sinistra.

## <a name="retrieve-the-access-keys-and-host-name"></a>Recuperare le chiavi di accesso e il nome host

1. Selezionare **Impostazioni** > **Chiavi di accesso**.

1. Copiare la **Stringa di connessione primaria (StackExchange.Redis)** in una posizione sicura. Sarà necessaria per l'esercizio successivo.

    Questa chiave include la chiave primaria e il nome host in una stringa di connessione completa per l'uso all'interno delle impostazioni dell'applicazione per il pacchetto **StackExchange.Redis** che verrà usato.

Nelle unità seguenti verranno illustrati i comandi disponibili per interrogare la cache.