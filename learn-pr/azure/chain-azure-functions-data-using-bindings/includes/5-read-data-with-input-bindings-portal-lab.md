Si supponga di voler creare un semplice servizio di ricerca di segnalibri. Il servizio inizialmente è di sola lettura. Se un utente vuole trovare una voce, invia una richiesta con l'ID della voce e viene restituito l'URL. Nel diagramma seguente viene illustrato il flusso:

![Diagramma di flusso che illustra il processo per trovare un segnalibro nel back-end di Cosmos DB. Quando la funzione di Azure riceve una richiesta con l'ID del segnalibro, verifica innanzitutto se la richiesta non è valida. Se non lo è viene generata una risposta di errore. Per le richieste valide, la funzione controlla se l'ID del segnalibro è presente in Cosmos DB. Se non è presente viene generata una risposta di errore. Se viene trovato l'ID del segnalibro, viene generata una risposta di esito positivo.](../media/5-find-bookmark-flow-small.png)

Quando un utente invia una richiesta con un testo, si prova a trovare nel database back-end una voce contenente questo testo come chiave o ID. Viene restituito un risultato indicante se la voce è stata trovata o meno.

È necessario archiviare i dati altrove. In questo diagramma di flusso l'archivio dati è un'istanza di Azure Cosmos DB. Ma come connettersi a tale database da una funzione e leggere i dati? Nell'ambito delle funzioni si configura un'*associazione di input* per tale processo.  Configurare un'associazione tramite il portale di Azure è davvero semplice. Come si vedrà a breve, non è necessario scrivere il codice per attività come l'apertura di una connessione di archiviazione. Queste attività vengono eseguite dal runtime di Funzioni di Azure e dalle associazioni.

## <a name="create-an-azure-cosmos-db-account"></a>Creare un account Azure Cosmos DB 

> [!NOTE]
> Questa unità non deve essere considerata un'esercitazione su Azure Cosmos DB. Se dopo il completamento di questo modulo si è interessati a ottenere ulteriori informazioni, è disponibile un percorso di apprendimento completo in Cosmos DB.

### <a name="create-a-database-account"></a>Creare un account di database

Un account di database è un contenitore per la gestione di uno o più database. Prima di poter creare un database è necessario creare un account di database.

1. Assicurarsi di aver eseguito l'accesso al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui è stato attivata la sandbox.

1. Selezionare il pulsante **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure, quindi selezionare **Database** > **Azure Cosmos DB**.

1. Nella pagina **Nuovo account** immettere le impostazioni per il nuovo account Azure Cosmos DB.

    | Impostazione | Valore | Descrizione |
    |---------|-------|-------------|
    | ID |*Immettere un nome univoco*|Immettere un nome univoco per identificare l'account Azure Cosmos DB. Poiché per creare l'URI viene aggiunto `documents.azure.com` all'ID specificato, usare un ID univoco ma identificabile.<br><br>L'ID può contenere solo lettere minuscole, numeri e il segno meno (-) e deve avere una lunghezza compresa tra 3 e 50 caratteri. |
    | API |SQL|L'API determina il tipo di account da creare. Azure Cosmos DB offre cinque API per soddisfare le esigenze dell'applicazione, ovvero SQL (database di documenti), Gremlin (database di grafi), MongoDB (database di documenti), Tabella di Azure e Cassandra, per ognuna delle quali è attualmente necessario un account separato. <br><br>Selezionare **SQL**. Al momento, il trigger e le associazioni di input e output di Azure Cosmos DB funzionano solo con gli account API SQL e API Graph. |
    | Sottoscrizione | Sottoscrizione Concierge | Selezionare la sottoscrizione di Azure che si desidera usare per l'account Azure Cosmos DB. |
    Gruppo di risorse|Usa esistente<br><br>Quindi, selezionare il **<rgn>[nome gruppo di risorse sandbox]</rgn>**. | Si seleziona **Usa esistente** perché si vogliono raggruppare tutte le risorse create per questo modulo nello stesso gruppo di risorse sandbox. |
    | Località | Viene inserita automaticamente dopo che **Usa esistente** è stato impostato. | Selezionare la località geografica in cui ospitare l'account Azure Cosmos DB. Usare la località più vicina agli utenti per offrire loro la massima velocità di accesso ai dati. In questo lab la località è predeterminata come località impostata per il gruppo di risorse esistente.|
    
    Lasciare tutti gli altri campi nel pannello **Nuovo account** impostati sul valore predefinito in quanto verranno usati in questo modulo.  Sono inclusi **Abilita ridondanza geografica**, **Abilita Multimaster** e **Reti virtuali**.

1. Selezionare **Crea** per effettuare il provisioning dell'account del database e distribuirlo.

1. La distribuzione potrebbe richiedere tempo. Quindi, prima di procedere, attendere il messaggio **Distribuzione riuscita** nell'hub di notifica.

    ![Notifica di completamento della distribuzione dell'account database](../media/5-db-deploy-success.PNG)

1. Per passare all'account del database nel portale, selezionare **Vai alla risorsa**. Successivamente verrà aggiunta una raccolta al database.

### <a name="add-a-collection"></a>Aggiungere una raccolta

In Cosmos DB un *contenitore* include le entità arbitrarie generate dall'utente. Per gli account dell'API MongoDB e SQL un contenitore esegue il mapping a una *raccolta*. All'interno di una raccolta vengono archiviati i documenti.

Verrà ora usato lo strumento Esplora dati nel portale di Azure per creare un database e una raccolta.

1. Fare clic su **Esplora dati** > **Nuova raccolta**.

2. In **Aggiungi raccolta** immettere le impostazioni per la nuova raccolta.

    >[!TIP]
    >L'area **Aggiungi raccolta** viene visualizzata all'estrema destra. Potrebbe essere necessario scorrere verso destra per visualizzarlo.

    |Impostazione|Valore consigliato|Descrizione
    |---|---|---|
    |ID database|[!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]| I nomi dei database devono avere una lunghezza compresa tra 1 e 255 caratteri e non possono contenere /, \\, #, ? o spazi finali.<br><br>È possibile immettere qualsiasi valore, ma è consigliabile usare [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)] come nome del nuovo database, perché è quello che verrà usato in questa unità. |
    |ID raccolta|[!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)]|Immettere [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] come nome della nuova raccolta. Gli ID delle raccolte prevedono gli stessi requisiti relativi ai caratteri dei nomi di database.|
    |Capacità di archiviazione| Fissa (10 GB)|Usare il valore predefinito **Fisso (10 GB)**. Questo valore indica la capacità di archiviazione del database.|
    |Velocità effettiva|1000 UR|Modificare la velocità effettiva in 1000 unità richiesta al secondo (UR/sec). Per impostare la velocità effettiva su 400 UR/sec la capacità di archiviazione deve essere impostata su **Fisso (10 GB)**. Se si vuole ridurre la latenza, è possibile aumentare le prestazioni in un secondo momento.|
        
3. Fare clic su **OK**. In Esplora dati verranno visualizzati il nuovo database e la nuova raccolta. È stato quindi creato un database. All'interno del database è stata definita una raccolta. Verranno ora aggiunti alcuni dati, noti anche come documenti.

### <a name="add-test-data"></a>Aggiungere i dati del test

All'interno del database è stata definita una raccolta denominata [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)]. Si vuole archiviare un URL e un ID in ogni documento, come ad esempio un elenco di segnalibri di pagine Web. 

I dati verranno aggiunti alla nuova raccolta usando Esplora dati.

1. In Esplora dati il nuovo database viene visualizzato nel riquadro Raccolte. Espandere il database [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)], espandere la raccolta [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)], selezionare **Documenti**, quindi **Nuovo documento**.
  
2. Sostituire il contenuto predefinito del nuovo documento con il JSON seguente.

     ```json
     {
         "id": "docs",
         "URL": "https://docs.microsoft.com/azure"
     }
     ```

3. Dopo avere aggiunto il JSON alla scheda **Documenti**, selezionare **Salva**.

    Si noti che sono presenti più proprietà di quelle aggiunte. Iniziano tutte con un carattere di sottolineatura (_rid, _self, _etag, _attachments, _ts). Si tratta di proprietà generate dal sistema per semplificare la gestione del documento.

    |Proprietà  |Descrizione  |
    |---------|---------|
    | `_rid`     |     L'ID della risorsa è un identificatore univoco e anche gerarchico per ogni stack di risorse nel modello di risorsa. Viene usato internamente per il posizionamento e l'esplorazione della risorsa documento.    |
    | `_self`     |   URI indirizzabile univoco per la risorsa.      |
    | `_etag`     |   Necessario per il controllo della concorrenza ottimistica.     |
    | `_attachments`     |  Percorso indirizzabile della risorsa allegati.       |
    | `_ts`     |    Timestamp dell'ultimo aggiornamento di questa risorsa.    |
     
4. Verranno ora aggiunti alcuni altri documenti alla raccolta. Creare quattro documenti aggiuntivi con il contenuto di seguito. Ricordarsi di salvare il lavoro.

    ```json
    {
        "id": "portal",
        "URL": "https://portal.azure.com"
    }
    ```
    
    ```json
    {
        "id": "learn",
        "URL": "https://docs.microsoft.com/learn"
    }
    ```
    
    ```json
    {
        "id": "marketplace",
        "URL": "https://azuremarketplace.microsoft.com/marketplace/apps"
    }
    ```
    
    ```json
    {
        "id": "blog",
        "URL": "https://azure.microsoft.com/blog"
    }
    ```
    
1. Al termine dell'operazione, la raccolta sarà simile a quella rappresentata di seguito:

    ![Interfaccia utente dell'API SQL nel portale, che mostra l'elenco di voci aggiunte alla raccolta di segnalibri](../media/5-db-bookmark-coll.PNG)
    
Alcune voci sono ora disponibili nella raccolta di segnalibri. Questo scenario funziona come indicato di seguito. Se ad esempio arriva una richiesta con "id=docs", si cercherà tale ID nella raccolta di segnalibri e si restituirà l'URL `https://docs.microsoft.com/azure`. Verrà ora creata una funzione di Azure che esegue la ricerca di valori in questa raccolta.

## <a name="create-your-function"></a>Creare una funzione

1. Passare all'app per le funzioni creata nell'unità precedente.

1. Espandere l'app per le funzioni, quindi passare il puntatore sulla raccolta di funzioni e selezionare il pulsante **Aggiungi** (**+**) accanto a **Funzioni**.  
   Questa azione avvia il processo di creazione della funzione. L'animazione di seguito illustra l'azione:

   ![Animazione del segno più che viene visualizzata quando l'utente passa il puntatore sulla voce di menu delle funzioni](../media/3-func-app-plus-hover-small.gif)

   La pagina mostra il set completo di trigger supportati. 
   
1. Selezionare **Trigger HTTP**, che è la prima voce nello screenshot di seguito:

    ![Screenshot della parte dell'interfaccia utente di selezione del modello di trigger, con il trigger HTTP visualizzato per primo nell'angolo in alto a sinistra dell'immagine.](../media/5-trigger-templates-small.png)

1. Compilare con i valori seguenti la finestra di dialogo **Nuova funzione** che viene visualizzata a destra.

    | Campo | Valore |
    |----------|--------|
    | Linguaggio | **JavaScript** |
    | Nome     | [!INCLUDE [func-name-find](./func-name-find.md)] |
    | Livello di autorizzazione | **Funzione** |
    
1. Selezionare **Crea** per creare la funzione.  
    Verrà aperto il file *index.js* nell'editor di codice e visualizzata un'implementazione predefinita della funzione attivata da HTTP.

### <a name="verify-the-function"></a>Verificare la funzione

È possibile verificare quanto fatto finora eseguendo test sulla nuova funzione come indicato di seguito:

1. Nella nuova funzione fare clic su **Recupera URL della funzione** nell'angolo in alto a destra, selezionare **predefinito (Tasto funzione)**, quindi fare clic su **Copia**.

1. Incollare l'URL della funzione copiato nella barra degli indirizzi del browser. Aggiungere il valore della stringa di query `&name=<yourname>` alla fine dell'URL e premere **Invio** per eseguire la richiesta. Si dovrebbe ottenere una risposta dalla funzione di Azure direttamente nel browser.  

Ora che la funzione essenziale è pronta, è possibile concentrare l'attenzione sulla lettura dei dati da Azure Cosmos DB, ovvero, in questo scenario, dalla raccolta [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)].

## <a name="add-an-azure-cosmos-db-input-binding"></a>Aggiungere un'associazione di input di Azure Cosmos DB

Per leggere i dati dal database è necessario definire un'associazione di input. Come si vedrà, con pochi passaggi è possibile configurare un'associazione che può comunicare con il database.

1. Per aprire la scheda dell'integrazione, selezionare **Integra** nel riquadro a sinistra.  
   Il modello usato ha creato automaticamente un trigger HTTP e un'associazione di output HTTP. È ora possibile aggiungere la nuova associazione di input di Azure Cosmos DB. 

2. Dalla colonna **Input**, selezionare **+ Nuovo input**.  
   Viene visualizzato un elenco di tutti i tipi di associazione di input possibili.

3. Nell'elenco selezionare **Azure Cosmos DB**, quindi **Seleziona**.  
   Verrà aperta la pagina di configurazione di input di Azure Cosmos DB. Successivamente, verrà configurata una connessione al database.

4. Accanto alla casella della **connessione all'account Azure Cosmos DB**, selezionare **nuova**.  
   Verrà aperta la finestra di dialogo **Connessione**, in cui sono già selezionati l'**account Azure Cosmos DB** e la sottoscrizione ad Azure. L'unica cosa che rimane da fare è selezionare un ID account del database.

5. Nella sezione "Creare un account del database" è stato necessario specificare un valore di ID. Trovare ora tale valore nell'elenco a discesa **Account del database**, quindi fare clic su **Seleziona**.

6. Una nuova connessione al database viene configurata e visualizzata nel campo della **connessione all'account di Azure Cosmos DB**. Per sapere che cosa indica effettivamente questo nome astratto, fare clic su *mostra valore* per visualizzare la stringa di connessione.

Poiché si vuole cercare un segnalibro con un ID specifico, ora si collegherà l'ID ricevuto all'associazione.

7. Nel campo **ID documento (facoltativo)** immettere `{id}`. Questa sintassi è nota come *espressione di associazione*. La funzione viene attivata da una richiesta HTTP che usa una stringa di query per specificare l'ID da cercare. Poiché gli ID sono univoci nella raccolta, l'associazione restituirà 0 (non trovato) o 1 documento (trovato).

8. Compilare con attenzione i campi rimanenti in questa pagina usando i valori della tabella seguente. In qualsiasi momento è possibile fare clic sull'icona delle informazioni a destra del nome di ogni campo per altre informazioni sullo scopo di ogni campo.

    |Impostazione  |Valore  |Descrizione  |
    |---------|---------|---------|
    |Nome del parametro del documento     |  **bookmark**       |  Nome usato per identificare questa associazione nel codice.      |
    |Nome database     |  [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]       | Il database da usare. Questo valore corrisponde al nome del database impostato in precedenza in questa lezione.        |
    |Nome raccolta     |  [!INCLUDE [cosmos-db-name](./cosmos-coll-name.md)]        | Raccolta da cui si leggeranno i dati. Questa impostazione è stata definita in precedenza nella lezione. |
    |Query SQL (facoltativa)    |   lasciare vuoto       |   Si sta recuperando un solo documento alla volta in base all'ID. L'applicazione di filtri con il campo ID documento è quindi meglio che usare una query SQL in questa istanza. Sarebbe possibile creare una query SQL per restituire una voce (`SELECT * from b where b.ID = {id}`). Tale query restituirebbe in effetti un documento, ma lo restituirebbe in una raccolta di documenti. Il codice dovrebbe modificare una raccolta inutilmente. Usare l'approccio basato sulle query SQL quando si vuole ottenere più documenti.   |
    |Chiave di partizione (facoltativa)     |   lasciare vuoto      |  È possibile accettare l'impostazione predefinita.       |
    
9. Per salvare tutte le modifiche apportate a questa configurazione di associazione, selezionare **Salva**. 

Ora che l'associazione è stata definita, è possibile usarla nella funzione.

## <a name="update-function-implementation"></a>Aggiornare l'implementazione della funzione

1. Seleziona la tua funzione, [!INCLUDE [func-name-find](./func-name-find.md)], per aprire *index.js* nell'editor di codice. Poiché è stata aggiunta un'associazione di input per leggere dal database, ora si aggiornerà la logica per usare questa associazione.

2. Sostituire tutto il codice in *index.js* con il codice del frammento di seguito e fare clic su **Salva**.

   [!code-javascript[](../code/find-bookmark-single.js)]

Una richiesta HTTP in ingresso attiva la funzione e il passaggio di un parametro di query `id` all'associazione di input di Cosmos DB. Se nel database viene trovato un documento corrispondente a questo ID, il parametro `bookmark` verrà impostato sul documento trovato. In tal caso, viene creata una risposta che contiene il valore dell'URL trovato nel documento con segnalibro. Se non sono stati trovati documenti corrispondenti a questa chiave, si risponde con un payload e un codice di stato che comunica all'utente la cattiva notizia.

## <a name="try-it-out"></a>Provare questa operazione

1. Selezionare **Recupera URL della funzione** in alto a destra, selezionare **predefinita (Tasto funzione)**, quindi selezionare **Copia** per copiare l'URL della funzione.

2. Incollare l'URL della funzione copiato nella barra degli indirizzi del browser. Aggiungere il valore della stringa di query `&id=docs` alla fine dell'URL e premere il tasto `Enter` per eseguire la richiesta. Verrà visualizzata una risposta che include un URL per tale risorsa.

3. Sostituire `&id=docs` con `&id=missing` e osservare la risposta.

4. Sostituire la stringa di query precedente con `&id=` e osservare la risposta.

    >[!TIP]
    >È anche possibile testare la funzione usando la scheda **Test** nell'interfaccia utente del portale della funzione. È possibile aggiungere un parametro di query o fornire un corpo della richiesta per ottenere gli stessi risultati descritti nei passaggi precedenti.
    
In questa unità è stata creata manualmente la prima associazione di input per leggere da un database Azure Cosmos DB. Grazie alle associazioni, la quantità di codice scritto per eseguire una ricerca nel database e leggere i dati è stata minima. La maggior parte del lavoro è stato finalizzato alla configurazione dichiarativa dell'associazione, mentre la piattaforma ha eseguito le altre operazioni.  

Nell'unità successiva si aggiungeranno altri dati alla raccolta di segnalibri tramite un'associazione di output di Azure Cosmos DB.