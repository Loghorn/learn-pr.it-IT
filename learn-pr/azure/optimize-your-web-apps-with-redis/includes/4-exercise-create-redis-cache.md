È possibile creare un'istanza di Cache Redis di Azure per archiviare e restituire valori usati comunemente.

<!-- TODO: do we need to activate the sandbox here? -->

## <a name="create-a-redis-cache-in-azure"></a>Creare una cache Redis in Azure

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Fare clic su **crea una risorsa**, fare clic su **database**, fare clic su **Cache Redis**.

    Lo screenshot seguente mostra la posizione di Cache Redis all'interno delle varie opzioni per le risorse di database nel portale di Azure.

    ![Screenshot che mostra le opzioni per i database del portale di Azure, con le opzioni Crea una risorsa, Database e Cache Redis evidenziate.](../media/4-create-a-cache-1.png)

### <a name="identify-the-location-for-the-cache"></a>Identificare il percorso della cache

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="configure-your-cache"></a>Configurare la cache

Applicare le impostazioni seguenti nella cache.

1. **Nome DNS:** creare un nome globalmente univoco, ad esempio **ContosoSportsApp1028**.

1. **Sottoscrizione:** selezionare la sottoscrizione di Azure Sandbox.

1. **Gruppo di risorse:** selezionate <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn> per il gruppo di risorse.

1. **Percorso:** in genere, è possibile selezionare un percorso vicini ai clienti, in questo caso, costa orientale degli stati. Tuttavia, l'ambiente Sandbox di Azure consente solo aree specifiche da selezionare per le risorse come indicato in precedenza. Selezionare una di queste posizioni.

1. **Piano tariffario:** selezionare **Basic C0**. Questo è il livello più basso che è possibile usare. Le app di produzione probabile che si voglia archiviare più dati e utilizzare alcune delle funzionalità Premium come clustering cosa che richiederebbe una selezione di livello superiore.

1. Fare clic su **Crea**.

    Lo screenshot seguente mostra una configurazione rappresentativa usata per creare una nuova risorsa di Cache Redis. Si noti che sarà leggermente diverso a causa della Sandbox di Azure.

    ![Screenshot che mostra il pannello del portale di Azure quando si crea una nuova risorsa di Cache Redis, popolato con un nome DNS della configurazione di esempio, sottoscrizione, nuovo gruppo di risorse, area e piano tariffario.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> Si dovrà attendere che la cache venga distribuita prima di continuare. Questo processo può richiedere del tempo.

## <a name="verify-your-data"></a>Verificare i dati

È possibile usare la **Console** funzionalità nel portale di Azure per inviare comandi all'istanza di cache Redis, dopo che è stato creato.

1. Individuare la cache Redis selezionando **tutte le risorse** nella barra laterale sinistra e usando la casella di filtro a sinistra selezionare Cache Redis di istanze. In alternativa, è possibile usare la casella di ricerca nella parte superiore e digitare il nome della cache.

1. Selezionare l'istanza di cache Redis.

1. Nel **Overview** pannello per la Cache Redis, select **Console**. Verrà aperta una console di Redis, che consente di immettere i comandi di Redis a basso livello.

1. Tipo di **ping**. Verificare che il valore restituito sia **PONG**.

1. Tipo di **testare set uno**. Verificare che il valore restituito sia **OK**.

1. Tipo di **ottenere test**. Verificare che il valore restituito sia **"uno"**.

1. Tornare alla **Panoramica** tramite la barra di navigazione nella parte superiore del pannello oppure utilizzare la barra di scorrimento per scorrere la visualizzazione torna a sinistra.

## <a name="retrieve-the-access-keys-and-host-name"></a>Recuperare le chiavi di accesso e il nome host

1. Selezionare **le impostazioni** > **chiavi di accesso**. 

1. Copia il **stringa di connessione primaria (stackexchange. Redis)** a una posizione sicura, poiché servirà per esercizio successivo.

    Questa chiave include il nome host e chiave primario in una stringa di connessione completa per l'uso all'interno di impostazioni dell'applicazione per il **stackexchange. Redis** pacchetto si intende usare.

Quindi, cerchiamo di comprendere alcuni dei comandi è possibile usare per interrogare la cache.