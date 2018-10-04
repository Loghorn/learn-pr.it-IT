Nell'esercizio precedente è stato implementato uno scenario per cercare i segnalibri in un database Azure Cosmos DB. È stata inoltre configurata un'associazione di input per leggere i dati dalla raccolta di segnalibri. Ma si può fare di più. Ad esempio, è possibile espandere lo scenario per includere anche la scrittura. Prendere in considerazione il diagramma di flusso seguente:

![Diagramma di flusso che illustra il processo per trovare un segnalibro nel back-end di Cosmos DB. Quando la funzione di Azure riceve una richiesta con l'ID del segnalibro, verifica innanzitutto se la richiesta è valida. Se non lo è viene generata una risposta di errore. Per le richieste valide, la funzione controlla se l'ID del segnalibro è presente in Cosmos DB. Se non è presente viene generata una risposta di errore. Se viene trovato l'ID del segnalibro, viene generata una risposta di esito positivo.](../media/7-add-bookmark-flow-small.png)

In questo scenario si riceveranno richieste per aggiungere segnalibri alla raccolta. Le richieste passano la chiave desiderata, o ID, insieme all'URL del segnalibro. Come si può notare nel diagramma di flusso, si riceverà un errore se la chiave esiste già nel back-end.

Se la chiave che è stata passata *non* viene trovata, il nuovo segnalibro verrà aggiunto al database. È possibile andare anche oltre.

Si può infatti notare un altro passaggio nel diagramma di flusso. Finora non si è fatto molto con i dati ricevuti in termini di elaborazione. Sono stati semplicemente spostati in un database. Tuttavia, in una soluzione reale è possibile che i dati vengano in qualche modo elaborati. Si può decidere di eseguire l'intera elaborazione nella stessa funzione, ma in questo lab verrà illustrato un modello che trasferisce l'ulteriore elaborazione a un altro componente o a un'altra parte della logica di business.

Un valido esempio di questo offload del lavoro nello scenario basato sui segnalibri potrebbe essere l'invio di un nuovo segnalibro a un servizio di generazione di codice a matrice. Questo servizio potrebbe, a sua volta, generare un codice a matrice per l'URL, archiviare l'immagine nell'archivio BLOB e aggiungere l'indirizzo dell'immagine del codice a matrice nella voce della raccolta di segnalibri. Chiamare un servizio per generare un'immagine del codice a matrice può richiedere molto tempo, pertanto invece di attendere il risultato è consigliabile trasferire l'operazione a una funzione che se ne occuperà in modalità asincrona.

Proprio come Funzioni di Azure supporta associazioni di input per varie origini di integrazione, include anche un set di modelli di associazioni di output per semplificare la scrittura dei dati nelle origini dati. Le associazioni di output vengono configurate anche nel file *function.json*.  Come si vedrà in questo esercizio, è possibile configurare la funzione per lavorare con più origini dati e servizi.

> [!IMPORTANT]
> Questo esercizio si basa su quello precedente. Usa lo stesso database di Azure Cosmos DB e la stessa associazione di input. Se non si è ancora svolta tale unità, è consigliabile farlo prima di procedere con questa.

## <a name="create-an-http-triggered-function"></a>Creare una funzione attivata da HTTP

1. Assicurarsi di aver eseguito l'accesso al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.

2. Nel portale passare all'app per le funzioni creata in questo modulo.

3. Selezionare il pulsante Aggiungi (**+**) accanto a **Funzioni**. Questa azione avvia il processo di creazione della funzione. 
4. La pagina mostra il set corrente di trigger supportati. Selezionare **Trigger HTTP**.

5. Compilare il riquadro **Nuova funzione** visualizzato a destra usando i valori seguenti:

    |Campo  |Valore  |
    |---------|---------|
    |Nome     |   [!INCLUDE [func-name-add](./func-name-add.md)]     |
    | Livello di autorizzazione | **Funzione** |

6. Selezionare **Crea** per creare la funzione. Verrà aperto il file **index.js** nell'editor di codice e visualizzata un'implementazione predefinita della funzione attivata da HTTP.

    > [!NOTE]
    > In questo esercizio sarà possibile velocizzare le operazioni usando il *codice* e la *configurazione* dell'unità precedente come punto di partenza.

7. Sostituire tutto il codice nel file **index.js** con il codice del frammento seguente e quindi selezionare **Salva** per salvare la modifica:

   [!code-javascript[](../code/find-bookmark-single.js)]

   Se questo codice ha un'aria familiare, è perché si tratta dell'implementazione della funzione [!INCLUDE [func-name-find](./func-name-find.md)]. Come si può immaginare, la funzione non potrà essere usata finché non si definiscono le stesse associazioni.

1. Aprire il file **function.json** dalla funzione [!INCLUDE [func-name-add](./func-name-add.md)].

11. Sostituire il contenuto del file con il codice JSON seguente:

    ```json
    {
      "bindings": [
        {
          "authLevel": "function",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req"
        },
        {
          "type": "http",
          "direction": "out",
          "name": "res"
        },
        {
          "type": "documentDB",
          "name": "bookmark",
          "databaseName": "func-io-learn-db",
          "collectionName": "Bookmarks",
          "connection": "unit3test_DOCUMENTDB",
          "direction": "in",
          "id": "{id}"
        }
      ],
      "disabled": false
    }
    ```

12. Assicurarsi di fare clic su **Salva** per salvare tutte le modifiche.

Nei passaggi precedenti sono state configurate le associazioni per la nuova funzione copiando le rispettive definizioni da un'altra funzione. Naturalmente è possibile creare una nuova associazione tramite l'interfaccia utente, ma è importante comprendere che è disponibile anche questa alternativa.

## <a name="try-it-out"></a>Provare questa operazione

1. Selezionare **Recupera URL della funzione** in alto a destra, selezionare **default (Function key)** (predefinita - tasto funzione) e quindi selezionare **Copia** per copiare l'URL della funzione.

2. Incollare l'URL copiato nella barra degli indirizzi del browser. Aggiungere il valore di stringa di query `&id=docs` alla fine dell'URL e quindi premere INVIO per eseguire la richiesta. Se tutto funziona, verrà visualizzata una risposta che include un URL per tale risorsa.

Come procedere a questo punto? Finora si è semplicemente replicata la procedura eseguita nel lab precedente ed è corretto. Quello che è stato fatto nell'ultimo lab serve infatti come punto di partenza per questo. Ora è il momento di provare nuove funzionalità, nello specifico per scrivere nel database. A questo scopo, è necessaria un'*associazione di output*.

## <a name="define-azure-cosmos-db-output-binding"></a>Definire l'associazione di output di Azure Cosmos DB

Invece di definire una nuova associazione di output tramite l'interfaccia utente, l'associazione verrà creata aggiornando manualmente il file di configurazione *function.json*.

1. Assicurarsi che il file *function.json* per [!INCLUDE [func-name-add](./func-name-add.md)] sia aperto nell'editor.

1. Copiare l'associazione denominata `bookmark` in tale file.

1. Posizionare il cursore subito dopo la parentesi graffa chiusa (}) e immediatamente prima della parentesi quadra chiusa (]). Aggiungere una virgola (,) e quindi incollare la copia dell'associazione in questo punto. Il file config *function.json* dovrebbe avere un aspetto simile al seguente:

   [!code-json[](../code/config-new-entry.json?highlight=22-31)]

1. Modificare l'associazione incollata, con le modifiche seguenti:

    |Proprietà   |Valore precedente  |Nuovo valore  |
    |---------|---------|---------|
    |name     |   bookmark      |  **newbookmark**       |
    |direction     |   in      |   **out**      |
    |id     |      {id}   |   **eliminare questa proprietà. Non esiste per l'associazione di output.**      |

1. Dopo aver apportato queste modifiche, il file è simile al codice JSON seguente:

    [!code-json[](../code/config-q-complete.json?highlight=22-30)]

Questa è una semplice dimostrazione di come sia anche possibile creare associazioni direttamente nel file di configurazione. In questo esempio, questa operazione ha senso perché si riutilizzano le proprietà di un'altra associazione, vale a dire le proprietà `databaseName`, `collectionName` e `connection` che sono già state configurate per l'associazione di input di Azure Cosmos DB.

> [!NOTE]
> Il valore effettivo di `connection` nel file JSON precedente è il nome assegnato alla connessione al momento della creazione.

Prima di aggiornare il codice, è possibile aggiungere un'altra associazione che consentirà di pubblicare messaggi in una coda.

## <a name="define-azure-queue-storage-output-binding"></a>Definire l'associazione di output dell'archiviazione code di Azure

Archiviazione code di Azure è un servizio che consente di archiviare messaggi a cui è possibile accedere da qualsiasi parte del mondo. Le dimensioni di un singolo messaggio possono essere al massimo 64 KB e una coda può contenere milioni di messaggi, fino alla capacità complessiva dell'account di archiviazione in cui viene definita. Il diagramma seguente illustra a livello generale come viene usata una coda in questo scenario:

![Un'illustrazione che mostra una coda di archiviazione e due funzioni, una che esegue il push dei messaggi e l'altra che preleva i messaggi nella coda](../media/7-q-logical-small.png)

In questo esempio è possibile vedere che la nuova funzione [!INCLUDE [func-name-add](./func-name-add.md)] aggiunge messaggi a una coda. Un'altra funzione, ad esempio una funzione fittizia denominata *gen-qr-code*, preleverà i messaggi dalla stessa coda ed elaborerà la richiesta.  Scrivendo o eseguendo il *push* dei messaggi nella coda da [!INCLUDE [func-name-add](./func-name-add.md)], si aggiungerà una nuova associazione di output alla soluzione. Questa volta l'associazione verrà creata tramite l'interfaccia utente del portale.

1. Selezionare **Integrazione** nel menu della funzione a sinistra per aprire la scheda Integrazione.

2. Selezionare **Nuovo output** nella colonna **Output**.
    Viene visualizzato un elenco di tutti i tipi di associazioni di output possibili.

3. Nell'elenco selezionare **Archiviazione code di Azure** e quindi **Seleziona**.
    Questa azione apre la pagina di configurazione di output di Archiviazione code di Azure.

   Successivamente, si configurerà una connessione dell'account di archiviazione per l'hosting della coda.

4. A destra del campo **Connessione dell'account di archiviazione** selezionare **Nuova**.
   Verrà visualizzato il riquadro di selezione **Account di archiviazione**.

5. All'inizio di questo modulo quando è stata creata l'app per le funzioni, è stato creato anche un account di archiviazione. L'account è elencato in questo riquadro, quindi selezionarlo. Il campo **Connessione dell'account di archiviazione** viene popolato con il nome di una connessione. Se si vuole visualizzare il valore della stringa di connessione, selezionare **Mostra valore**.

6. Anche se per tutti gli altri campi possono essere lasciati i valori predefiniti, verranno modificati i valori seguenti per rendere più significative le proprietà:

    |Proprietà  |Valore precedente  |Nuovo valore  | Descrizione |
    |---------|---------|---------|---------|
    |Nome della coda     |    outqueue     |  **bookmarks-post-process**      | Si tratta del nome della coda usata per l'inserimento dei segnalibri, in modo che possano essere ulteriormente elaborati da un'altra funzione. |
    | Nome del parametro del messaggio    |  outputQueueItem       |   **newmessage**      | Proprietà di associazione che verrà usata nel codice. |

7. Ricordarsi di selezionare **Salva** per salvare le modifiche.

## <a name="update-function-implementation"></a>Aggiornare l'implementazione della funzione

Tutte le associazioni sono ora impostate per la funzione [!INCLUDE [func-name-add](./func-name-add.md)]. È il momento di usarle nell'ambito della funzione.

1.  Selezionare la funzione [!INCLUDE [func-name-add](./func-name-add.md)] per aprire il file **index.js** nell'editor di codice.

2. Sostituire tutto il codice nel file *index.js* con il codice del frammento seguente:

   [!code-javascript[](../code/add-bookmark.js)]

Verrà ora esaminato in dettaglio il funzionamento di questo codice:

* Dato che questa funzione modifica i dati, si prevede che la richiesta HTTP sia di tipo POST e che i dati del segnalibro facciano parte del corpo della richiesta.
* L'associazione di input di Azure Cosmos DB tenta di recuperare un documento o un segnalibro usando l'oggetto `id` ricevuto. Se viene trovata una voce, l'oggetto `bookmark` verrà impostato. La condizione `if(bookmark)` verifica se è stata trovata una voce.
* L'aggiunta al database è un'operazione estremamente semplice che implica l'impostazione del parametro di associazione `context.bindings.newbookmark` sulla nuova voce del segnalibro, creata sotto forma di stringa JSON.
* Anche pubblicare un messaggio nella coda è un'operazione semplice che prevede l'impostazione di `context.bindings.newmessage parameter`.

> [!NOTE]
> L'unica attività eseguita è stata quella di creare un'associazione di coda. In modo esplicito non è stata creata alcuna coda, il tutto è avvenuto automaticamente grazie alle associazioni. Come riportato nel callout seguente, la coda viene infatti creata automaticamente se non esiste già.

![Screenshot che illustra che la coda verrà creata automaticamente.](../media/7-q-auto-create-small.png)

Questo è tutto. Nella sezione successiva verrà messo in pratica il lavoro svolto.

## <a name="try-it-out"></a>Provare questa operazione

Ora che sono disponibili più associazioni di output, i test diventano un po' più complessi. Nei lab precedenti i test sono stati eseguiti inviando una richiesta HTTP e una stringa di query, ma questa volta viene inviata una richiesta HTTP POST. È anche necessario verificare se i messaggi vengono inseriti in una coda.

1. Con la funzione [!INCLUDE [func-name-add](./func-name-add.md)] selezionata nel portale delle app per le funzioni, selezionare la voce di menu Test all'estrema sinistra per espanderla.

2. Selezionare la voce di menu **Test** e verificare che il riquadro Test sia aperto. Lo screenshot seguente riporta il risultato:

    ![Screenshot che mostra il pannello Test della funzione espanso.](../media/7-test-panel-open-small.png)

    > [!IMPORTANT]
    > Assicurarsi che nell'elenco a discesa del metodo HTTP sia selezionato **POST**.

3. Sostituire il contenuto del corpo della richiesta con il payload JSON seguente:

    ```json
    {
        "id": "docs",
        "url": "https://docs.microsoft.com/azure"
    }
    ```

4. Selezionare **Esegui** nella parte inferiore del riquadro Test.

5. Verificare che nella finestra **Output** sia visualizzato il messaggio che indica che il segnalibro esiste già, come illustrato nella figura seguente:

    ![Screenshot che mostra il pannello Test e il risultato di un test con esito negativo.](../media/7-test-exists-small.png)

6. Sostituire il corpo della richiesta con il payload seguente:

    ```json
    {
        "id": "github",
        "url": "https://www.github.com"
    }
    ```
7. Selezionare **Esegui** nella parte inferiore del riquadro Test.

8. Verificare che nella casella *Output* sia visualizzato un messaggio che indica che il segnalibro è stato aggiunto, come illustrato nella figura seguente.

    ![Screenshot che mostra il pannello Test e il risultato di un test con esito positivo.](../media/7-test-success-small.png)

La procedura è stata completata. [!INCLUDE [func-name-add](./func-name-add.md)] funziona come previsto, ma è necessario occuparsi dell'operazione di accodamento presente nel codice. Per prima cosa, occorre verificare se è stato scritto qualcosa in una coda.

### <a name="verify-that-a-message-is-written-to-the-queue"></a>Verificare che sia stato scritto un messaggio nella coda

Le code di Archiviazione code di Azure sono ospitate in un account di archiviazione. L'account di archiviazione è già stato selezionato in questo esercizio quando si è creata l'associazione di output.

1. Nella casella di ricerca principale nel portale di Azure digitare **account di archiviazione** e nei risultati della ricerca selezionare **Account di archiviazione** in **Servizi**.

      ![Screenshot che mostra i risultati della ricerca per Account di archiviazione nella casella di ricerca principale.](../media/7-search-for-sa-small.png)

2. Nell'elenco degli account di archiviazione restituiti selezionare l'account di archiviazione usato per creare l'associazione di output **newmessage**.
   Le impostazioni dell'account di archiviazione sono visualizzate nella finestra principale del portale.

3. Selezionare la voce **Code** nell'elenco **Servizi**.
   Viene visualizzato un elenco di code ospitate da questo account di archiviazione. Verificare che la coda **bookmarks-post-process** esista, come illustrato nello screenshot seguente:

      ![Screenshot che mostra la coda nell'elenco delle code ospitate da questo account di archiviazione](../media/7-q-in-list-small.png)

4. Selezionare **bookmarks-post-process** per aprire la coda.
   I messaggi presenti nella coda vengono visualizzati sotto forma di elenco. In assenza di errori, la coda include il messaggio inviato al momento dell'aggiunta di un segnalibro al database. Il messaggio sarà simile al seguente:

    ![Screenshot che mostra il messaggio nella coda](../media/7-message-in-q-small.png)

   In questo esempio si noterà che al messaggio è stato assegnato un ID univoco e che nel campo **MESSAGE TEXT** (Testo del messaggio) viene visualizzato il segnalibro in formato di stringa JSON.

5. È possibile testare la funzione ulteriormente modificando il corpo della richiesta nel riquadro Test con nuovi set di ID/URL ed eseguendo la funzione. Prestare attenzione alla coda per vedere se arrivano altri messaggi. È anche possibile esaminare il database per verificare se sono state aggiunte nuove voci.

In questo lab sono state approfondite le competenze sulle associazioni prendendo in esame le associazioni di output e scrivendo dati in Azure Cosmos DB. È stata inoltre aggiunta un'altra associazione di output per pubblicare messaggi in una coda di Azure. Ciò dimostra tutta l'efficacia delle associazioni, che consentono di modellare e spostare i dati dalle origini in ingresso a una vasta gamma di destinazioni. Non è stato necessario scrivere il codice del database, né gestire manualmente le stringhe di connessione. Al contrario, le associazioni sono state configurate in modo dichiarativo e si è fatto in modo che la piattaforma si occupasse automaticamente della protezione delle connessioni, oltre che del ridimensionamento della funzione e delle connessioni.