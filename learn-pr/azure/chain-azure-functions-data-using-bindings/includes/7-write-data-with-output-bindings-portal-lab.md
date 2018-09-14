Nel nostro esercizio precedente, abbiamo implementato uno scenario per cercare i segnalibri in un database Azure Cosmos DB. È configurata un'associazione di input per leggere i dati dalla nostra raccolta di segnalibri. Ma solo la lettura dei dati è un'operazione noiosa, eseguiamo più. È possibile espandere lo scenario in modo da includere la scrittura. Si consideri il seguente diagramma di flusso.

![Diagramma di flusso che illustra il processo di aggiunta di segnalibri nel nostro Cosmos DB back-end](../media-draft/add-bookmark-flow-small.png)

In questo scenario, si riceverà le richieste per aggiungere segnalibri per l'elenco. Le richieste di passano la chiave desiderata, o ID, insieme all'URL di segnalibro. Come può notare nel diagramma di flusso, si riceverà una risposta con un errore se la chiave esiste già nel nostro back-end.

Se la chiave che è stata passata a noi *non* trovato, verrà aggiunto il nuovo segnalibro al database. È stato possibile fermarsi, ma voglio un po' più.

Si noti che un altro passaggio nel diagramma di flusso? Finora è stata ancora eseguita gran parte con i dati ricevuti in termini di elaborazione. Parliamo di cosa è visualizzato in un database. Tuttavia, in una soluzione reale, è possibile che probabilmente si elaboreranno i dati in qualche modo. È possibile decidere di eseguire un'elaborazione tutti nella stessa funzione, ma in questo laboratorio verrà illustrato un modello che esegue l'offload di un'ulteriore elaborazione per un altro componente o una porzione della logica di business.

Quali potrebbero essere un buon esempio di questo processo di offload del lavoro in questo scenario i segnalibri? Anche se inviare il nuovo segnalibro a un servizio di generazione di codice a matrice? Tale servizio verrebbe, a sua volta, genera un codice a matrice per l'URL, archiviare l'immagine nell'archivio blob e aggiungere l'indirizzo dell'immagine di codici a matrice nella voce nella nostra raccolta di segnalibri. Chiamata di un servizio per generare un'immagine di codici a matrice può richiedere molto tempo quindi, invece di attendere il risultato, è trasferirlo a una funzione e lasciarlo risolvere questo problema in modo asincrono.

Proprio come funzioni di Azure supporta associazioni di input per le origini di integrazione diversi, include anche un set di modelli di binding di output per semplificare la scrittura dei dati in origini. Le associazioni di output vengono configurate nel *Function. JSON* file.  Come si vedrà in questo esercizio, è possibile configurare nostra funzione per lavorare con più origini dati e servizi.

> [!IMPORTANT]
> Questo esercizio si basa sull'esercizio disponibile nell'ultima unità, vale a dire, Usa nello stesso database di Azure Cosmos DB e associazione di input. Se si è mai lavorato con quell'unità, è consigliabile farlo prima di procedere con questa esercitazione.

## <a name="create-an-httptriggered-function"></a>Creare una funzione HTTP_triggered

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

2. Nel portale di Azure, passare all'app di funzione che è stato creato in questo modulo.

3. Espandere l'app per le funzioni, quindi passare il mouse per la raccolta di funzioni e selezionare Aggiungi (**+**) accanto al pulsante **funzioni**. Questa azione avvia il processo di creazione della funzione. L'animazione seguente viene illustrata questa azione.

![Animazione del segno più che viene visualizzato quando l'utente posiziona il mouse sulla voce di menu funzioni.](../media-draft/func-app-plus-hover-small.gif)

4. La pagina Mostra il set corrente di trigger supportate. Selezionare **trigger HTTP**, ovvero la prima voce nello screenshot seguente.

![Screenshot della parte della selezione del modello di trigger dell'interfaccia utente, con il trigger TTP visualizzato per primi, nell'angolo superiore sinistro dell'immagine.](../media-draft/trigger-templates-small.PNG)

5. Compilare il **nuova funzione** finestra di dialogo che viene visualizzato a destra con i valori seguenti.

|Campo  |Valore  |
|---------|---------|
|Lingua     | **JavaScript**        |
|Nome     |   [!INCLUDE [func-name-add](./func-name-add.md)]     |
| Livello di autorizzazione | **Funzione** |

5. Selezionare **Create** per creare la funzione, che apre il file index. js nell'editor del codice e consente di visualizzare un'implementazione predefinita della funzione attivata tramite HTTP.

In questo esercizio, è possibile velocizzare le operazioni usando il *codice* e *configurazione* dall'unità precedente come punto di partenza.

6. Sostituire tutto il codice in Index. js con il codice il seguente frammento di codice e fare clic su **salvare** per salvare questa modifica. 

[!code-javascript[](../code/find-bookmark-single.js)]

Se questo codice si ha familiarità, infatti, è l'implementazione del nostro [!INCLUDE [func-name-find](./func-name-find.md)] (funzione). Come si può immaginare, la funzione non funzionerà fino a quando non si definiscono le stesse associazioni.  

7. Aprire il *Function. JSON* del file dal [!INCLUDE [func-name-find](./func-name-find.md)] (funzione). È possibile trovarlo aprendo il **visualizzare i file** menu a destra dell'editor del codice.

8. Copiare l'intero contenuto di questo file.

9. Aprire il *Function. JSON* del file dal [!INCLUDE [func-name-add](./func-name-add.md)] (funzione).

10. Sostituire il contenuto di questo file con il contenuto è stato copiato dal *Function. JSON* file associato il [!INCLUDE [func-name-find](./func-name-find.md)] (funzione). Al termine, il file Function. JSON deve contenere il codice JSON seguente.

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

11. Verificare di aver **salvare** tutte le modifiche.

Nei passaggi precedenti, le associazioni sono configurati per la nuova funzione copiando le definizioni di associazione da un altro. È possibile, ovviamente, creati una nuova associazione tramite l'interfaccia utente, ma è importante comprendere che questa alternativa è disponibile all'utente.

## <a name="try-it-out"></a>Provare il servizio

1. Come di consueto, fare clic su **<> / Recupera URL della funzione** in alto a destra, selezionare **predefinito (tasto funzione)**, quindi fare clic su **copia** per copiare la funzione dell'URL.

2. Incollare l'URL della funzione è stato copiato nella barra degli indirizzi del browser. Aggiungere il valore della stringa di query `&id=docs` alla fine dell'URL e premere il tasto `Enter` per eseguire la richiesta. Tutto funziona bene, che verrà visualizzata una risposta che include un URL per tale risorsa.

Quindi, in cui sono in? Bene, finora abbiamo abbiamo semplicemente replicato è stato fatto nell'ambiente di laboratorio precedente. Ma è perfetta. Abbiamo stiamo la copia è stato fatto nel lab ultimo da usare come punto di partenza per questo. Si sarà sta lavorando le nuove funzionalità successivamente, vale a dire, la scrittura al database. A tal fine, è necessario un *associazione di output*.

## <a name="define-azure-cosmos-db-output-binding"></a>Definire associazione di output per Azure Cosmos DB

Invece di definire una nuova associazione di output passando attraverso l'interfaccia utente, verrà creata questa associazione aggiornando il file di configurazione *Function. JSON*, manualmente. 

1. Aprire il **Function. JSON** file per questa funzione nell'editor, selezionarlo nel **visualizzare file** elenco.

2. Copiare l'associazione con il nome `bookmark` in tale file.

3. Posizionare il cursore subito dopo la parentesi graffa di chiusura, subito prima della parentesi di chiusura quadrato. Aggiungere una virgola `,` e quindi incollare la copia dell'associazione di seguito. I *Function. JSON* configurazione dovrebbe ora essere simile al seguente.

[!code-json[](../code/config-new-entry.json?highlight=22-31)]

4. Modificare il binding che è stato incollato, con le modifiche seguenti.


|Proprietà   |Valore precedente  |Nuovo valore  |
|---------|---------|---------|
|name     |   Segnalibro      |  **newbookmark**       |
|direction     |   in      |   **out**      |
|id     |      {id}   |   **eliminare questa proprietà. Non esiste per l'associazione di output.**      |

Quando si apportano queste modifiche, si finisce con un file che è simile al codice JSON seguente.

[!code-json[](../code/config-q-complete.json?highlight=22-30)]

Si tratta solo di una dimostrazione di come è possibile creare associazioni direttamente nel file di configurazione. In questo esempio, ha senso perché si sta riutilizzando le proprietà da un'altra associazione, vale a dire, il `databaseName`, `collectionName` e `connection` associazione di input che è già configurato per il Cosmos DB.

> [!NOTE]
> Il valore effettivo del `connection` nel codice JSON precedente file sarà qualsiasi nome di connessione è stata fornita al momento della creazione.

Prima viene aggiornato il codice, è possibile aggiungere un'associazione più che ci consentirà di inviare messaggi a una coda.

## <a name="define-azure-queue-storage-output-binding"></a>Definire associazione di output archiviazione code di Azure

Archiviazione code di Azure è un servizio per l'archiviazione dei messaggi che possono accedere da qualsiasi parte del mondo. Un singolo messaggio può essere fino a 64 KB e una coda può contenere milioni di messaggi fino al limite di capacità totale dell'account di archiviazione in cui è definito. Il diagramma seguente illustra a livello generale come verrà utilizzata una coda in questo scenario.

![Diagramma che mostra concetto di una coda di archiviazione e due funzioni di eseguire il push e si estraggono i messaggi nella coda.](../media-draft/q-logical-small.png)

Qui possiamo vedere che la nuova funzione, [!INCLUDE [func-name-add](./func-name-add.md)], aggiunge i messaggi a una coda. Un'altra funzione, ad esempio una funzione fittizia denominata *codice a matrice di generazione*, verranno visualizzati i messaggi dalla stessa coda e di elaborare la richiesta.  Dal momento che si scrivono, oppure *push*, i messaggi nella coda da [!INCLUDE [func-name-add](./func-name-add.md)], si aggiungerà una nuova associazione di output per la nostra soluzione. È possibile creare l'associazione tramite l'interfaccia utente in questo momento.

1. Selezionare **integra** nel menu a sinistra per aprire la scheda integrazione (funzione).

2. Selezionare **+ nuovo Output** sotto il **output** colonna. Viene visualizzato un elenco di tutti i tipi di associazione di output possibili.

3. Fare clic su **archiviazione code di Azure** dall'elenco e quindi la **seleziona** pulsante. Questa azione apre la pagina di configurazione di output archiviazione code di Azure.

Successivamente, verrà configurato una connessione dell'account di archiviazione. Si tratta in cui verrà ospitata la coda.

4. Nel campo denominato **connessione dell'account di archiviazione** questa pagina, fare clic su *nuove* a destra del campo vuoto. Questa azione apre la **Account di archiviazione** finestra di dialogo di selezione. 

5. Quando abbiamo avviato questo modulo e creato l'app per le funzioni, è stato creato anche un account di archiviazione in quel momento. Verranno elencati in questa finestra di dialogo, quindi andare avanti e selezionarlo. Il **connessione dell'account di archiviazione** campo viene popolato con il nome di una connessione. Se si desidera visualizzare il valore di stringa di connessione, fare clic su **mostrano valore**.

6. Anche se è stato possibile lasciare tutti gli altri campi in questa pagina con i valori predefiniti, è possibile modificare il comando seguente per rendere disponibili i maggiori vantaggi alle proprietà.


|Proprietà  |Valore precedente  |Nuovo valore  | Descrizione |
|---------|---------|---------|---------|
|Nome della coda     |    outqueue     |  **bookmarks-post-process**      | Si tratta del nome della coda viene utilizzato per posizionare i segnalibri in modo che possono essere elaborati ulteriormente da un'altra funzione. |
| Nome del parametro messaggio    |  outputQueueItem       |   **newmessage**      | Si tratta della proprietà di associazione che verrà usato nel codice. |


7. Ricordarsi di fare clic su **salvare** per salvare le modifiche.

## <a name="update-function-implementation"></a>Implementazione della funzione Update

È ora disponibile in tutti i binding impostati per il [!INCLUDE [func-name-add](./func-name-add.md)] (funzione). È possibile usarli nella nostra funzione.

1.  Fare clic sulla nostra funzione [!INCLUDE [func-name-add](./func-name-add.md)]per aprire *index. js* nell'editor del codice.

2. Sostituire tutto il codice in Index. js con il codice il frammento di codice seguente.

[!code-javascript[](../code/add-bookmark.js)]

Verrà ora suddivisione del funzionamento di questo codice.

* Poiché questa funzione consente di modificare i dati, si prevede che la richiesta HTTP POST e i dati di segnalibro a far parte del corpo della richiesta.
* Tenta di recuperare un documento o un segnalibro, l'associazione di input di Cosmos DB usando il `id` ricevute. Se trova una voce, il `bookmark` imposterà l'oggetto. Il `if(bookmark)` condizione controlla se è stata trovata una voce.
* L'aggiunta al database è facile quanto impostare la `context.bindings.newbookmark` parametro di associazione per la nuova voce segnalibro, che è stata creata come una stringa JSON.
* Invio di un messaggio per la coda è semplice come l'impostazione di `context.bindings.newmessage parameter`.

> [!NOTE]
> Per creare un'associazione di coda è stata l'unica attività che è stata eseguita. È stato mai creato la coda in modo esplicito. Si è osservazione la potenza delle associazioni. Come afferma il callout seguente, la coda viene creata automaticamente se non esiste.

![Schermata di chiamata che la coda verrà auto-creato.](../media-draft/q-auto-create-small.png)

Pertanto, questo è tutto: di seguito viene illustrato il nostro lavoro in azione nella sezione successiva.

## <a name="try-it-out"></a>Provare il servizio

Ora che abbiamo più associazioni di output, i test diventa un po' più complicato. Mentre nella serie di esercitazioni precedenti che è stati contenuti da testare inviando una richiesta HTTP e una stringa di query, è opportuno eseguire una richiesta HTTP Post in questo momento. È inoltre necessario verificare se i messaggi messi in una coda.

1.  Con la funzione [!INCLUDE [func-name-add](./func-name-add.md)], selezionata nel portale di App per le funzioni, fare clic sulla voce di menu Test all'estrema sinistra per espanderla.

2. Selezionare il **testare** menu item e verificare che è aperto il pannello test. Lo screenshot seguente mostra cosa sarà simile. 

![Screenshot che mostra la funzione Test pannello espanso.](../media-draft/test-panel-open-small.png)

> [!IMPORTANT]
> Assicurarsi che **POST** sia selezionato nell'elenco a discesa metodo HTTP.

3. Sostituire il contenuto del corpo della richiesta con il payload JSON seguente.

```json
  {
      "id": "docs",
      "url": "https://docs.microsoft.com/azure"
  }
  ```

4. Fare clic su **eseguire** nella parte inferiore del pannello del test. 

5. Verificare che il *Output* finestra consente di visualizzare il "segnalibro esiste già". messaggio come illustrato nel diagramma seguente. 

![Screenshot che mostra il pannello di Test e il risultato di un test non riuscito.](../media-draft/test-exists-small.png)

6. A questo punto sostituire il corpo della richiesta con il payload seguente. 

```json
  {
      "id": "github",
      "URL": "https://www.github.com"
  }
  ```
7. Fare clic su **eseguire** nella parte inferiore del pannello del test.

8. Verificare che *Output* finestra Visualizza il messaggio "segnalibro aggiunto" come illustrato nel diagramma seguente.

![Screenshot che mostra il pannello di Test e risultati di un test ha esito positivo.](../media-draft/test-success-small.png)

La procedura è stata completata. Il [!INCLUDE [func-name-add](./func-name-add.md)] funziona come previsto, ma per quanto riguarda tale operazione coda avevamo nel codice? Beh, torniamo vedere se un elemento è stato scritto in una coda.

### <a name="verify-that-a-message-is-written-to-our-queue"></a>Verificare che la coda viene scritto un messaggio

Le code di archiviazione di Accodamento di Azure sono ospitate in un account di archiviazione. L'account di archiviazione è selezionato in questo esercizio già quando si crea l'associazione di output. 

1. Nella casella di ricerca principale nel portale di Azure, digitare *gli account di archiviazione* e nei risultati della ricerca selezionare **account di archiviazione** sotto il *Services* categoria. Come illustrato nello screenshot seguente. 

![Screenshot che mostra i risultati della ricerca per l'Account di archiviazione nella casella di ricerca principale.](../media-draft/search-for-sa-small.png)

2. Nell'elenco degli account di archiviazione restituite, selezionare l'account di archiviazione usato per creare il **newmessage** associazione di output. Le impostazioni dell'account di archiviazione vengono visualizzati nella finestra principale il portale.

3. Selezionare il **code** elemento dall'elenco dei servizi. Ciò consente di visualizzare un elenco di code ospitate da questo account di archiviazione. Verificare che il **segnalibri-post-processo** coda esiste, come illustrato nello screenshot seguente.

![Screenshot che mostra la coda nell'elenco delle code ospitate da questo account di archiviazione](../media-draft/q-in-list-small.png)

4. Fare clic su **segnalibri-post-processo** per aprire la coda. I messaggi che sono nella coda vengono visualizzati in un elenco. In assenza di errori in base al piano, il messaggio che viene registrati quando è stato aggiunto un segnalibro al database deve rimanere nella coda e sarà simile alla voce seguente. 

![Screenshot che mostra il messaggio nella coda](../media-draft/message-in-q-small.png)

In questo esempio, si noterà che il messaggio è stato assegnato un ID univoco e il **testo del messaggio** campo viene visualizzato il segnalibro nel formato di stringa JSON.

5. È possibile testare la funzione ulteriormente modificando il corpo della richiesta nel Pannello di Test con nuovi set di id/url e l'esecuzione della funzione. Guarda questa coda per visualizzare altri messaggi arrivano. È anche possibile cercare a livello di database per verificare sono state aggiunte nuove voci. 

In questo laboratorio è espansa nostre conoscenze di associazioni per le associazioni di output, la scrittura dei dati in Azure Cosmos DB. Si è andato oltre e aggiungere un'altra associazione di output per inviare messaggi a una coda di Azure. Ciò dimostra la vera potenza delle associazioni che consentono il data shaping e spostare i dati dalle origini in ingresso a una varietà di destinazioni. È ancora stato scritto alcun codice di database o dovevano gestire le stringhe di connessione noi stessi. In alternativa, è stato configurato le associazioni in modo dichiarativo e fare in modo che la piattaforma di protezione delle connessioni, la funzione di ridimensionamento e scalabilità le nostre connessioni.