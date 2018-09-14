Di seguito è riportato un'alto livello illustrazione di andremo a creare in questo esercizio.

![Rappresentazione visiva del trigger HTTP predefinito, che mostra res e richiesta HTTP e risposta, nonché req rispettivi parametri di associazione.](../media-draft/default-http-trigger-visual-small.PNG)

Si creerà una funzione che verrà avviata quando riceve una richiesta HTTP e risponderà a ogni richiesta inviando nuovamente un messaggio. I parametri `req` e `res` sono l'associazione di trigger e binding rispettivamente di output. Andiamo!

### <a name="create-a-function-app"></a>Creare un'app per le funzioni

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Creare un'app per le funzioni che verranno utilizzate in tutta l'intero modulo. Una funzione app consente di raggruppare le funzioni come un'unità logica per semplificare la gestione, distribuzione e la condivisione delle risorse.

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).
1. Selezionare il **crea una risorsa** pulsante trovato nell'angolo superiore sinistro del portale di Azure, quindi seleziona **Compute** > **App per le funzioni**.
1. Impostare la funzione di proprietà dell'app come segue:

    | Proprietà      | Valore consigliato  | Descrizione                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome app** | Nome globalmente univoco | Nome che identifica la nuova app per le funzioni. I caratteri validi sono `a-z`, `0-9` e `-`.  | 
    | **Sottoscrizione** | Sottoscrizione in uso | Sottoscrizione in cui viene creata questa nuova app per le funzioni. | 
    | **Gruppo di risorse**|  Selezionare **Usa esistente** e scegliere <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn> | Nome del gruppo di risorse in cui creare l'app per le funzioni. | 
    | **Sistema operativo** | Windows | Il sistema operativo che ospita l'app per le funzioni.  |
    | **Hosting** |   Piano a consumo | Piano di hosting che definisce come vengono allocate le risorse all'app per le funzioni. Nel **piano a consumo** predefinito le risorse vengono aggiunte dinamicamente in base alle esigenze delle funzioni. In questo hosting [senza server](https://azure.microsoft.com/overview/serverless-computing/) si paga solo per il periodo in cui le funzioni sono in esecuzione.   |
    | **Posizione** | Selezionare dall'elenco | Scegliere un'[area](https://azure.microsoft.com/regions/) nelle vicinanze o vicino ad altri servizi a cui accedono le funzioni. |
    | **Account di archiviazione** |  Nome globalmente univoco |  Nome del nuovo account di archiviazione usato dall'app per le funzioni. I nomi degli account di archiviazione devono avere una lunghezza compresa tra 3 e 24 caratteri e possono contenere solo numeri e lettere minuscole. Questa finestra di dialogo consente di popolare il campo con un nome univoco derivato dal nome assegnato all'app. Tuttavia, è possibile usare un nome diverso o anche un account esistente. |


3. Selezionare **Crea** per effettuare il provisioning dell'app per le funzioni e distribuirla.

4. Selezionare l'icona di notifica nell'angolo superiore sinistro del portale e attendere un **distribuzione in corso** messaggio analogo al seguente.

![Notifica che la distribuzione di app di funzione è in corso](../media-draft/func-app-deploy-progress-small.PNG)

5. Distribuzione può richiedere tempo. Quindi, rimanere nell'hub di notifica e attendere un **distribuzione ha avuto esito positivo** messaggio analogo al seguente.

![Notifica che è stata completata la distribuzione di app (funzione)](../media-draft/func-app-deploy-success-small.PNG)

6. La procedura è stata completata. Aver creato e distribuito l'app per le funzioni. Selezionare **Vai alla risorsa** per visualizzare la nuova app per le funzioni.

>[!TIP]
>Se si verificano problemi durante l'individuazione di App per le funzioni nel portale, Scopri [aggiungere App per le funzioni ai Preferiti nel portale di](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

### <a name="create-a-function"></a>Creare una funzione

Ora che abbiamo app per le funzioni, è possibile creare una funzione. Una funzione viene attivata tramite un trigger. In questo modulo, si userà un trigger HTTP.

1. Espandere la nuova app di funzione, quindi passare il mouse per la raccolta di funzioni e selezionare Aggiungi (**+**) accanto al pulsante **funzioni**. Questa azione avvia il processo di creazione della funzione. L'animazione seguente viene illustrata questa azione.

![Animazione del segno più che viene visualizzato quando l'utente posiziona il mouse sulla voce di menu funzioni.](../media-draft/func-app-plus-hover-small.gif)

2. Nel **iniziare a usare rapidamente** pagina, selezionare **WebHook e API**, selezionare una lingua per la funzione e fare clic su **creare questa funzione**.

3. Viene creata una funzione nel linguaggio prescelto usando il modello per una funzione attivata tramite HTTP. In questo esercizio si creerà una funzione JavaScript.

### <a name="try-it-out"></a>Provare il servizio

È possibile testare quello che abbiamo finora eseguendo le operazioni seguenti:

1. Nella nuova funzione fare clic su **</> Recupera URL della funzione** nell'angolo in alto a destra, selezionare **default (Function key)** (predefinita - tasto funzione) e quindi fare clic su **Copia**.

2. Incollare l'URL della funzione è stato copiato nella barra degli indirizzi del browser. Aggiungere il valore della stringa di query `&name=<yourname>` alla fine dell'URL e premere il tasto `Enter` per eseguire la richiesta. Verrà visualizzata una risposta simile alla seguente risposta restituita dalla funzione visualizzata nel browser.  

State eseguite le operazioni. Abbiamo aggiunto una funzione attivata da HTTP per l'app per le funzioni e testate per verificare che funzioni come previsto.

![Screenshot del messaggio di risposta di una chiamata alla funzione.](../media-draft/default-http-trigger-response-small.PNG)

Come può notare da questo esercizio fino ad ora, è necessario selezionare un tipo di trigger durante la creazione di una funzione. Ogni funzione ha un unico trigger. In questo esempio, utilizziamo un trigger HTTP, ovvero che la funzione viene avviato quando viene ricevuta una richiesta HTTP. L'implementazione predefinita, illustrato nella schermata riportata di seguito in JavaScript, risponde con il valore di un parametro *nome* ricevuto nelle stringhe di query o nel corpo della richiesta. Se è stata fornita alcuna stringa, la funzione risponde con un messaggio che chiede chi viene eseguita la chiamata per fornire un valore di nome.

![Screenshot dell'implementazione di JavaScript predefinita di una funzione di Azure attivate da HTTP.](../media-draft/default-http-trigger-implementation-small.PNG)

Tutto il codice è nel *index. js* file nella cartella della funzione. È possibile esaminare brevemente la funzione di altro file, il *Function. JSON* file config. Questi dati di configurazione sono illustrati nel seguente elenco di JSON.

```json
{
  "disabled": false,
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
    }
  ]
}
```

Come può notare, questa funzione ha un'associazione di trigger denominata **req** typu `httpTrigger` e un'associazione di output denominato **res** typu `HTTP`. Nel codice precedente per la funzione, abbiamo visto come si accede il payload della richiesta HTTP in ingresso tramite nostri **req** parametro. Allo stesso modo, abbiamo inviato una risposta HTTP impostando semplicemente nostri **res** parametro. Le associazioni davvero ci occupiamo della parte del carico di lavoro per noi!

>[!TIP]
>È possibile visualizzare index. js e Function. JSON, espandendo il **visualizzare file** menu a destra del Pannello di funzione nel portale di Azure.  

### <a name="explore-binding-types"></a>Esplorare i tipi di associazione

1. Si noti che nell'ingresso nella funzione è presente un set di voci di menu come illustrato nello screenshot seguente.

![Screenshot che mostra le voci di menu in una funzione nel pannello dell'App per le funzioni.](../media-draft/func-menu-small.PNG)

2. Selezionare la voce di menu per aprire la scheda integrazione per la funzione di integrazione. Se sono state eseguite insieme a questa unità, scheda Integrazione dovrebbe essere molto simile allo screenshot seguente.

![Screenshot che mostra integra dell'interfaccia utente o scheda.](../media-draft/func-integrate-tab-small.PNG)

Si noti che è stata già definito un trigger e un'associazione di output come mostrato in questo screenshot. È anche possibile vedere che è possibile aggiungere più di un trigger. In effetti, per modificare il trigger per la funzione avremmo prima eliminare il trigger e crearne uno nuovo.

D'altra parte, il **input** e **Outputs** sezioni di questo modulo Visualizza un segno più `+` accedere per aggiungere più associazioni.

3. Selezionare **+ nuovo Input** sotto il **input** colonna. Come illustrato nello screenshot seguente, viene visualizzato un elenco di tutti i tipi di associazione di input possibili.

![Screenshot che mostra l'elenco di associazioni di input possibili.](../media-draft/func-input-bindings-selector-small.PNG)

Si consiglia di considerare ognuna di queste associazioni di input e come è possibile usarli in una soluzione. Esistono molti tra cui scegliere. Questo elenco potrebbe essere modificato anche da quando che è possibile leggere questo modulo per continuare a supportare ulteriori origini dati.

4. Selezionare **annullare** per ignorare questo elenco.

5. Selezionare **+ nuovo Output** sotto il **output** colonna. Come illustrato nello screenshot seguente, viene visualizzato un elenco di tutti i tipi di associazione di output possibili.

![Screenshot che mostra l'elenco di associazioni di output possibili.](../media-draft/func-output-bindings-selector-small.PNG)

Anche in questo caso, sono disponibili numerose opzioni qui, come illustrato dall'esigenza di una barra di scorrimento a destra in questa schermata.

>[!TIP]
>Per altre informazioni dettagliate sulle associazioni supportate, consultare il [elenco di associazioni supportate](https://docs.microsoft.com/azure/azure-functions/functions-versions) nella documentazione di funzioni di Azure.

Finora abbiamo imparato come creare un'app per le funzioni e aggiungervi una funzione. Abbiamo visto una semplice funzione in azione che viene eseguito quando viene inviata una richiesta HTTP a esso. Abbiamo inoltre esplorato l'interfaccia utente e i tipi di input e binding di output disponibili per funzioni del portale. Nell'unità di next, useremo un'associazione di input per leggere il testo da un un database.