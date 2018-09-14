Si supponga che si desidera creare un servizio di ricerca semplice segnalibro. Il nostro servizio è di sola lettura inizialmente. Se un utente vuole trovare una voce, inviano una richiesta con l'ID della voce e viene restituito l'URL. Il diagramma seguente illustra il flusso.

![Diagramma di flusso che illustra il processo di individuazione di un segnalibro nel nostro Cosmos DB back-end](../media-draft/find-bookmark-flow-small.png)

Quando un utente a invia una richiesta con un testo, si tenta di trovare una voce nel nostro database back-end che contiene il testo come una chiave o un ID. Viene restituito un risultato che indica se la voce viene trovata o non.

È necessario archiviare i dati da qualche parte. In questo diagramma di flusso, archivio dati viene visualizzato come un database Azure Cosmos DB. Ma, come abbiamo connettersi al database da una funzione e leggere i dati? Nel mondo delle funzioni, si configura un *associazione di input* per tale processo.  Configurazione di un'associazione tramite il portale di Azure è semplice. Come vedremo tra breve, non devi scrivere codice per attività quali l'apertura di una connessione di archiviazione. Il runtime di funzioni di Azure e l'associazione richiedere automaticamente di tali attività.

> [!IMPORTANT]
> Dobbiamo fare riferimento ad alcune risorse (database, funzione, associazioni, code) create qui nel nostro laboratorio successivo. Tenere queste risorse relativi a almeno fino al termine di questo corso.

Come avveniva per il precedente modulo, faremo tutto nel portale di Azure.

## <a name="create-an-azure-cosmos-db"></a>Creare un Azure Cosmos DB 

### <a name="create-a-database-account"></a>Creare un account di database

Un account di database è un contenitore per la gestione di uno o più database. Prima di poter creare un database, è necessario creare un account di database.

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

2. Selezionare il **crea una risorsa** pulsante trovato nell'angolo superiore sinistro del portale di Azure, quindi seleziona **database** > **Azure Cosmos DB**.

3. Nella pagina **Nuovo account** immettere le impostazioni per il nuovo account Azure Cosmos DB.

    Impostazione|Valore|Descrizione
    ---|---|---
    ID|*Immettere un nome univoco*|Immettere un nome univoco per identificare l'account Azure Cosmos DB. Dato che all'ID specificato viene aggiunto *documents.azure.com* per creare l'URI, usare un ID univoco ma identificabile.<br><br>L'ID può contenere solo lettere minuscole, numeri e il segno meno (-) e deve avere una lunghezza compresa tra 3 e 50 caratteri.
    API|SQL|L'API determina il tipo di account da creare. Azure Cosmos DB offre cinque API per soddisfare le esigenze dell'applicazione: SQL (database di documenti), Gremlin (grafo), MongoDB (database di documenti), tabelle di Azure e Cassandra, ognuna delle quali è attualmente necessario un account separato. <br><br>Selezionare **SQL**. A questo punto, il trigger, le associazioni di input e le associazioni di output Azure Cosmos DB funzionano solo con gli account API SQL e l'API Graph. 
    Sottoscrizione|*Sottoscrizione in uso*|Selezionare la sottoscrizione di Azure da usare per l'account Azure Cosmos DB.
    Gruppo di risorse|Usa esistente<br><br>*Quindi selezionare <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>, il gruppo di risorse creati in un'unità per le risorse dal modulo precedente.*| Selezioniamo HyperV **Usa esistente**perché si desidera raggruppare tutte le risorse create per questo modulo nello stesso gruppo di risorse.
    Posizione|Compilato automaticamente una volta **Usa esistente** è impostata. | Selezionare la località geografica in cui ospitare l'account Azure Cosmos DB. Usare la località più vicina agli utenti per offrire loro la massima velocità di accesso ai dati. In questo laboratorio, il percorso è predeterminato per noi come percorso impostato per il gruppo di risorse esistente.

Lasciare tutti gli altri campi di **nuovo account** pannello i valori predefiniti per i valori poiché utilizziamo li in questo modulo.  Che include **Abilita ridondanza geografica**, **Abilita multimaster**, **reti virtuali**.

4. Selezionare **Create** per effettuare il provisioning e distribuire l'account del database.

5. Distribuzione può richiedere tempo. Quindi, attendere che un **distribuzione ha avuto esito positivo** messaggio analogo al seguente prima di procedere.

<!-- TODO figure out how to center these image -->

![Notifica che è stata completata la distribuzione di account del database](../media-draft/db-deploy-success.PNG)

6. La procedura è stata completata. Aver creato e distribuito all'account di database.

7. Selezionare **Vai alla risorsa** per passare all'account del database nel portale. Successivamente si aggiungerà una raccolta nel database.

### <a name="add-a-collection"></a>Aggiungere una raccolta

In Cosmos DB, un *contenitore* contiene entità generati dall'utente non autorizzata. In un database API SQL supporta un'API orientata ai documenti, un contenitore è un *raccolta*. All'interno di una raccolta, permette di archiviare documenti. Si spera tutto sarà più chiara una volta che viene creata una raccolta e aggiungere alcuni documenti.

È possibile usare lo strumento Esplora dati nel portale di Azure per creare un database e una raccolta.

1. Fare clic su **Esplora dati** > **Nuova raccolta**.

2. Nel **Aggiungi raccolta**, immettere le impostazioni per la nuova raccolta.

    >[!TIP]
    >A destra verrà visualizzata l'area **Aggiungi raccolta**. Per vederla potrebbe essere necessario scorrere verso destra.

    Impostazione|Valore consigliato|Descrizione
    ---|---|---
    ID database|[!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]| I nomi dei database devono avere una lunghezza compresa tra 1 e 255 caratteri e non possono contenere /, \\, #, ? o spazi finali.<br><br>Si è liberi di immettere le operazioni desiderate, ma è consigliabile [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)] come nome per il nuovo database e che è ciò che si farà riferimento in questa unità. |
    ID raccolta|[!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)]|Immettere [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] come nome per la nuova raccolta. Gli ID delle raccolte hanno gli stessi requisiti di caratteri di nomi di database.
    Capacità di archiviazione| Fissa (10 GB)|Usare il valore predefinito **Fissa (10 GB)**. Questo valore indica la capacità di archiviazione del database.
    Velocità effettiva|400 UR|Modificare la velocità effettiva in 400 unità richiesta al secondo (UR/sec). La capacità di archiviazione deve essere impostata su **Fisso (10 GB)** per impostare la velocità effettiva su 400 UR/sec. Se si vuole ridurre la latenza, è possibile aumentare la velocità effettiva in un secondo momento.
    
3. Fare clic su **OK**. Esplora dati consente di visualizzare il nuovo database e la raccolta. Quindi, ora abbiamo un database. All'interno del database, abbiamo definito una raccolta. Quindi aggiungiamo alcuni dati, noti anche come documenti.

### <a name="add-test-data"></a>Aggiungere dati di test

Abbiamo definito una raccolta nel nostro database denominato [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)], pertanto ciò che stiamo si intende archiviare nella raccolta? Beh, l'idea consiste nell'archiviare un URL e l'ID in ogni documento, ad esempio un elenco di segnalibri di pagina web. 

Si aggiungerà i dati per la nuova raccolta usando Esplora dati.

1. In Esplora dati il nuovo database viene visualizzato nel riquadro Raccolte. Espandere il [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)] del database, espandere il [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] raccolta, fare clic su **documenti**, quindi fare clic su **nuovo documento**.
  
2. Aggiungere ora un documento alla raccolta con la struttura seguente. Ogni documento è file JSON senza schema.

     ```json
     {
         "id": "docs",
         "URL": "https://docs.microsoft.com/azure"
     }
     ```

3. Dopo avere aggiunto il codice JSON alla scheda **Documenti**, fare clic su **Salva**.

Quando il documento viene salvato, si noti che sono presenti più proprietà di quelle che è stato aggiunto. Iniziano tutte con un carattere di sottolineatura ( RID, Self, ETag, _attachments, TS). Queste sono proprietà generata dal sistema per semplificare la gestione del documento. Nella tabella seguente viene illustrato brevemente che cosa sono.

|Proprietà  |Descrizione  |
|---------|---------|
|_rid     |     L'ID di risorsa ( RID) è un identificatore univoco che è anche gerarchico per ogni stack delle risorse nel modello di risorsa. Viene usato internamente per il posizionamento e l'esplorazione della risorsa del documento.    |
|_self     |   L'URI indirizzabile univoco per la risorsa.      |
|_etag     |   Obbligatorio per il controllo della concorrenza ottimistica.     |
|_attachments     |  Il percorso indirizzabile della risorsa degli allegati.       |
|_ts     |    Il timestamp dell'ultimo aggiornamento di questa risorsa.    |
 
4. Aggiungiamo alcuni documenti più nella nostra raccolta. Per ognuna delle operazioni seguenti, usare il **nuovo documento** nuovo comando per creare una voce per ognuno. Non dimenticare di fare clic su # # Save * * per acquisire le aggiunte.

|id  |value  |
|---------|---------|
|portal     |  https://portal.azure.com       |
|Informazioni su     |   https://docs.microsoft.com/learn |
|Marketplace     |    https://azuremarketplace.microsoft.com/marketplace/apps  |
|blog | https://azure.microsoft.com/blog |

Al termine, la raccolta dovrebbe essere simile allo screenshot seguente.

![Screenshot dell'interfaccia utente di API SQL nel portale che mostra l'elenco di voci è stato aggiunto per la nostra raccolta di segnalibri.](../media-draft/db-bookmark-coll.PNG)

Alcune voci sono ora disponibili nella nostra raccolta di segnalibri. Questo scenario funziona come indicato di seguito. Se arriva una richiesta, ad esempio, "id = docs", verrà cercare tale ID nella nostra raccolta di segnalibri e l'URL restituito https://docs.microsoft.com/azure. Creare una funzione di Azure che esegue la ricerca di valori in questa raccolta.

## <a name="create-our-function"></a>Creare la funzione

1. Passare all'app di funzione che è stato creato nell'unità di precedente.

2. Espandere l'app per le funzioni, quindi passare il mouse per la raccolta di funzioni e selezionare Aggiungi (**+**) accanto al pulsante **funzioni**. Questa azione avvia il processo di creazione della funzione. L'animazione seguente viene illustrata questa azione.

![Animazione del segno più che viene visualizzato quando l'utente posiziona il mouse sulla voce di menu funzioni.](../media-draft/func-app-plus-hover-small.gif)

3. La pagina Mostra il set completo di trigger supportate. Selezionare **trigger HTTP**, ovvero la prima voce nello screenshot seguente.

![Screenshot della parte della selezione del modello di trigger dell'interfaccia utente, con il trigger TTP visualizzato per primi, nell'angolo superiore sinistro dell'immagine.](../media-draft/trigger-templates-small.PNG)

4. Compilare il **nuova funzione** finestra di dialogo che viene visualizzato a destra con i valori seguenti.

|Campo  |Valore  |
|---------|---------|
|Lingua     | **JavaScript**        |
|Nome     |   [!INCLUDE [func-name-find](./func-name-find.md)]     |
| Livello di autorizzazione | **Funzione** |

5. Selezionare **Create** per creare la funzione, che apre il file index. js nell'editor del codice e consente di visualizzare un'implementazione predefinita della funzione attivata tramite HTTP.

È possibile verificare quanto è stato fatto finora testando la nuova funzione come indicato di seguito:

1. Nella nuova funzione fare clic su **</> Recupera URL della funzione** nell'angolo in alto a destra, selezionare **default (Function key)** (predefinita - tasto funzione) e quindi fare clic su **Copia**.

2. Incollare l'URL della funzione è stato copiato nella barra degli indirizzi del browser. Aggiungere il valore della stringa di query `&name=<yourname>` alla fine dell'URL e premere il tasto `Enter` per eseguire la richiesta. Verrà visualizzata una risposta simile alla seguente risposta restituita dalla funzione visualizzata nel browser.  

Ora che abbiamo la funzione pressoché essenziale funziona, è possibile concentrare l'attenzione per la lettura dei dati da Azure Cosmos DB o in questo scenario, nostro [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] raccolta.

## <a name="add-a-cosmos-db-input-binding"></a>Aggiungere un'associazione di input di Cosmos DB

Si vuole leggere i dati dal database di cui che è stato creato, quindi immettere le associazioni di input. Come si vedrà, è possibile configurare un'associazione che può comunicare con il database in pochi passaggi.

1. Selezionare **integra** nel menu a sinistra per aprire la scheda integrazione (funzione).

Il modello è stato usato creato HTTP trigger e un HTTP associazione per noi di output. È possibile aggiungere la nuova associazione di input di Azure Cosmos DB. 

2. Selezionare **+ nuovo Input** sotto il **input** colonna. Viene visualizzato un elenco di tutti i tipi di associazione di input possibili.

3. Fare clic su **Azure Cosmos DB** dall'elenco e quindi la **seleziona** pulsante. Questa azione apre la pagina di configurazione di input di Azure Cosmos DB.

Successivamente, verrà configurato una connessione al database.

4. Nel campo denominato **connessione all'account Azure Cosmos DB** questa pagina, fare clic su *nuove* a destra del campo vuoto. Questa azione apre la **Connection** finestra di dialogo che dispone già **account Azure Cosmos DB** e la sottoscrizione di Azure selezionata. L'unica cosa rimane da fare consiste nel selezionare un id account del database.

5. Nella sezione **creare un account di database**, era necessario specificare un valore ID. A questo punto individuare tale valore nella *Account del Database* elenco a discesa e quindi fare clic su **seleziona**.

Una nuova connessione al database è configurata e viene visualizzata nei **connessione all'account Azure Cosmos DB** campo. Se si vuole sapere cosa è effettivamente protetto da questo nome astratto, fare semplicemente clic *mostrano valore* per visualizzare la stringa di connessione.

Si vuole cercare un segnalibro con un ID specifico, pertanto, è possibile collegare l'ID riceviamo all'associazione.

7. Nel **(facoltativo) ID del documento** immettere `{id}`. Questo è noto come un *espressione di associazione*. La funzione viene attivata da una richiesta HTTP che usa una stringa di query per specificare l'ID da cercare. Poiché gli ID sono univoci nella nostra raccolta, l'associazione restituirà 0 (non trovato) o 1 documenti (trovato).

8. Compilare con attenzione i campi rimanenti in questa pagina usando i valori nella tabella seguente. In qualsiasi momento, è possibile fare clic sull'icona delle informazioni a destra del nome di ogni campo per altre informazioni sullo scopo di ogni campo.

|Impostazione  |Valore  |Descrizione  |
|---------|---------|---------|
|Nome del parametro del documento     |  **bookmark**       |  Nome utilizzato per identificare questa associazione nel codice.      |
|Nome database     |  [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]       | Il database in cui verranno letti i dati. Si tratta del nome di database che è impostati in precedenza in questa lezione.        |
|Nome raccolta     |  [!INCLUDE [cosmos-db-name](./cosmos-coll-name.md)]        | Raccolta da cui si leggeranno i dati. Questa impostazione è stata definita in precedenza nella lezione. |
|Query SQL (facoltativa)    |   Lasciare vuoto       |   Stiamo solo un solo documento alla volta in base all'ID. Pertanto, il filtro con il campo ID documento è una migliore rispetto all'uso di una Query SQL in questa istanza. È stato possibile creare una Query SQL per restituire una voce (`SELECT * from b where b.ID = {id}`). Tale query restituisce in effetti un documento, ma sarebbe restituiscono in una raccolta di documenti. Il nostro codice sarebbe necessario modificare una raccolta inutilmente. Usare l'approccio di Query SQL quando si desidera ottenere più documenti.   |
|Chiave di partizione (facoltativa)     |   Lasciare vuoto      |  Possiamo accettare l'opzione predefinita.       |

9. Fare clic su **salvare** salvare tutte le modifiche alla configurazione di associazione. Ora che abbiamo l'associazione definita, è possibile usarla nella nostra funzione.

## <a name="update-function-implementation"></a>Implementazione della funzione Update

1. Fare clic sulla nostra funzione [!INCLUDE [func-name-find](./func-name-find.md)]per aprire *index. js* nell'editor del codice. È stata aggiunta un'associazione di input per leggere dal database, pertanto, è possibile aggiornare la logica per usare questa associazione.

2. Sostituire tutto il codice in Index. js con il codice il frammento di codice seguente.

[!code-javascript[](../code/find-bookmark-single.js)]

Quando una richiesta HTTP, la funzione di attivare, il `id` viene passato il parametro di query per l'associazione di input di Cosmos DB. Se trovato un documento che corrisponde a questo ID, il `bookmark` parametro verrà impostato ad esso. In tal caso, viene creata una risposta che contiene il valore dell'URL trovato nel documento di segnalibro. Se è stato trovato alcun documento corrispondenti a questa chiave, Microsoft risponde con un payload e codice di stato che indica all'utente le cattive notizie.

## <a name="try-it-out"></a>Provare il servizio

1. Come di consueto, fare clic su **<> / Recupera URL della funzione** in alto a destra, selezionare **predefinito (tasto funzione)**, quindi fare clic su **copia** per copiare la funzione dell'URL.

2. Incollare l'URL della funzione è stato copiato nella barra degli indirizzi del browser. Aggiungere il valore della stringa di query `&id=docs` alla fine dell'URL e premere il tasto `Enter` per eseguire la richiesta. Tutto funziona bene, che verrà visualizzata una risposta che include un URL per tale risorsa.

3. Sostituire `&id=docs` con `&id=missing` e osservare la risposta.

4. Sostituire la stringa di query precedente con `&id=` e osservare la risposta.

>[!TIP]
>È anche possibile testare la funzione utilizzando il **testare** scheda nel portale di funzioni dell'interfaccia utente. È possibile aggiungere un parametro di query o solo fornire un corpo della richiesta per ottenere gli stessi risultati, come descritto in te i passaggi precedenti.

In questa unità, è stato creato l'associazione di input prima manualmente per leggere da un database Azure Cosmos DB. La quantità di codice che scritto per una ricerca nel database e lettura dei dati minimo, grazie alle associazioni. Abbiamo fatto la maggior parte del nostro lavoro configurando l'associazione in modo dichiarativo e la piattaforma ha risolto il resto.  

Nell'unità successiva, si aggiungeranno che associazione di output di più dati per la nostra raccolta di segnalibri tramite un Azure Cosmos DB.

> [!TIP]
> Questa unità non deve essere un'esercitazione su Azure Cosmos DB. Se si desidera approfondire, ecco alcune risorse per iniziare a usare:
>
>* [Introduzione ad Azure Cosmos DB: API SQL](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)
>
>* [Panoramica tecnica di Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)
>
>* [Documentazione di Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)