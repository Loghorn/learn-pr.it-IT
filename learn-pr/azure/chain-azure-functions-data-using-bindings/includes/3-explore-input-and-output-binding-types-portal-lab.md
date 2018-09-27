Di seguito è riportata un'illustrazione generale di ciò che si creerà in questo esercizio.

![Un'illustrazione del trigger HTTP predefinito che mostra la richiesta e la risposta HTTP oltre ai rispettivi parametri di associazione req e res](../media/3-default-http-trigger-visual-small.PNG)

Si creerà una funzione che verrà avviata alla ricezione di una richiesta HTTP e che risponderà a ogni richiesta inviando un messaggio. I parametri `req` e `res` corrispondono rispettivamente all'associazione di trigger e all'associazione di output.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-function-app"></a>Creare un'app per le funzioni

Verrà ora creata un'app per le funzioni da usare in tutto questo modulo. Un'app per le funzioni consente di raggruppare le funzioni come un'unità logica per semplificare la gestione, la distribuzione e la condivisione delle risorse.

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.

1. Selezionare il pulsante **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure, quindi selezionare **Calcolo** > **App per le funzioni**.

1. Impostare le proprietà dell'app per le funzioni come segue:

    | Proprietà     | Valore consigliato  | Descrizione  |
    |--------------|------------------|--------------|
    | **Nome app** | Nome globalmente univoco | Nome che identifica la nuova app per le funzioni. I caratteri validi sono `a-z`, `0-9` e `-`.  |
    | **Sottoscrizione** | Sottoscrizione in uso | Sottoscrizione in cui viene creata questa nuova app per le funzioni. |
    | **Gruppo di risorse**|  Selezionare **Usa esistente** e scegliere _<rgn>[Nome gruppo di risorse sandbox]</rgn>_ | Nome del gruppo di risorse in cui creare l'app per le funzioni. |
    | **Sistema operativo** | Windows | Il sistema operativo che ospita l'app per le funzioni.  |
    | **Hosting** |   Piano a consumo | Piano di hosting che definisce come vengono allocate le risorse all'app per le funzioni. Nel **piano a consumo** predefinito le risorse vengono aggiunte in modo dinamico come richiesto dalle funzioni. In questo modello di hosting serverless si paga solo per il periodo in cui le funzioni sono in esecuzione.   |
    | **Account di archiviazione** |  Nome globalmente univoco |  Nome del nuovo account di archiviazione usato dall'app per le funzioni. I nomi degli account di archiviazione devono avere una lunghezza compresa tra 3 e 24 caratteri e possono contenere solo numeri e lettere minuscole. In questa finestra di dialogo il campo è popolato con un nome univoco derivato dal nome assegnato all'app. Tuttavia, è possibile usare un nome diverso o perfino un account esistente. |
    | **Località** | Selezionarla dall'elenco | Selezionare la località più vicina tra quelle disponibili elencate di seguito. |

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. Per effettuare il provisioning dell'app per le funzioni e distribuirla, selezionare **Crea**.

1. Selezionare l'icona di notifica nell'angolo in alto a destra del portale e attendere la visualizzazione di un messaggio **Distribuzione in corso** analogo al seguente.

    ![Notifica che la distribuzione dell'app per le funzioni è in corso](../media/3-func-app-deploy-progress-small.PNG)

1. La distribuzione potrebbe richiedere tempo. Rimanere pertanto nell'hub di notifica e attendere la visualizzazione di un messaggio **Distribuzione completata** analogo al seguente.

    ![Notifica di completamento della distribuzione dell'app per le funzioni](../media/3-func-app-deploy-success-small.PNG)

 1. Al completamento della distribuzione dell'app per le funzioni, accedere al portale e selezionare **Tutte le risorse**. L'app per le funzioni sarà inclusa nell'elenco con il tipo **Servizio app** e il nome che le è stato assegnato. Per aprire l'app per le funzioni, selezionarla dall'elenco.

    >[!TIP]
    >In caso di problemi nel trovare le app per le funzioni nel portale, vedere come [aggiungere le app per le funzioni ai preferiti nel portale](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

## <a name="create-a-function"></a>Creare una funzione

Ora che si dispone di un'app per le funzioni, è il momento di creare una funzione. Una funzione viene attivata tramite un trigger. In questo modulo si userà un trigger HTTP.

1. Espandere la nuova app per le funzioni, quindi passare il puntatore sulla raccolta di funzioni e selezionare il pulsante Aggiungi (**+**) accanto a **Funzioni**. Questa azione avvia il processo di creazione della funzione. L'animazione seguente illustra questa azione.

    ![Animazione del segno più che viene visualizzata quando l'utente passa il puntatore sulla voce di menu delle funzioni.](../media/3-func-app-plus-hover-small.gif)

1. Dalla pagina **Operatività immediata**, accedere alla sezione **Iniziare autonomamente** e selezionare **Funzione personalizzata**.

1. Questa operazione consente di visualizzare tutti i modelli inclusi nell'elenco, di trovare il modello del **trigger HTTP** e di selezionare JavaScript come linguaggio di programmazione.

    ![Screenshot della casella di creazione della funzione HTTP con il collegamento JavaScript evidenziato](../media/3-http-function.png)

1. Nel pannello **Nuova funzione** modificare il nome (se lo si desidera), lasciare il **Livello di autorizzazione** impostato su _Funzione_ e fare clic su **Crea**.

1. Nella nuova funzione fare clic sul collegamento **</> Recupera URL della funzione** nell'angolo in alto a destra, selezionare **predefinita (Tasto funzione)**, quindi selezionare **Copia**.

1. Incollare l'URL della funzione copiato nella barra degli indirizzi di una nuova scheda del browser.

1. Aggiungere il valore della stringa di query `&name=Azure` alla fine dell'URL e premere il tasto Invio per eseguire la richiesta. Verrà visualizzata una risposta simile alla seguente, restituita dalla funzione visualizzata nel browser.

    ```output
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Hello Azure</string>
    ```

Come si è potuto osservare finora in questo esercizio, quando si crea una funzione è necessario selezionare un tipo di trigger. Ogni funzione ha un unico trigger. In questo esempio si usa un trigger HTTP, vale a dire che la funzione viene avviata quando riceve una richiesta HTTP. L'implementazione predefinita, illustrata nello screenshot seguente in JavaScript, risponde con il valore di parametro *name* ricevuto nella stringa di query o nel corpo della richiesta. Se non sono state fornite stringhe, la funzione risponde con un messaggio che chiede a chi sta eseguendo la chiamata di specificare un valore per il parametro name.

![Implementazione JavaScript predefinita di una funzione di Azure attivata da HTTP](../media/3-default-http-trigger-implementation-small.PNG)

Tutto il codice si trova nel file **index.js** nella cartella di questa funzione. Si esaminerà ora brevemente l'altro file della funzione, il file config **function.json**. Questi dati di configurazione sono visualizzati nell'elenco JSON seguente.

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

Come si può osservare, questa funzione ha un'associazione di trigger denominata **req** di tipo `httpTrigger` e un'associazione di output denominata **res** di tipo `HTTP`. Nel codice precedente per la funzione si è visto come è stato eseguito l'accesso al payload della richiesta HTTP in ingresso tramite il parametro **req**. Analogamente, è stata inviata una risposta HTTP semplicemente impostando il parametro **res**. Le associazioni risultano davvero utili in questi casi.

>[!TIP]
>È possibile visualizzare i file **index.js** e **function.json** espandendo il menu **Visualizza file** a destra del pannello della funzione nel portale di Azure.

### <a name="explore-binding-types"></a>Esplorare i tipi di associazioni

1. Si noti che sotto la voce della funzione è presente un set di voci di menu come illustrato nello screenshot seguente.

    ![Screenshot che mostra le voci di menu sotto una funzione nel pannello App per le funzioni.](../media/3-func-menu-small.PNG)

1. Selezionare la voce di menu Integrazione per aprire la scheda Integrazione della funzione. Se sono state seguite le indicazioni di questa unità, la scheda Integrazione dovrebbe essere molto simile allo screenshot seguente.

    ![Screenshot che mostra l'interfaccia utente o la scheda per l'integrazione.](../media/3-func-integrate-tab-small.PNG)

    > [!NOTE]
    > Un'associazione di trigger e una di output sono già state definite, come illustrato in questo screenshot. È possibile notare che l'aggiunta di più di _un_ trigger non è consentita. In effetti, per cambiare il trigger per la funzione, sarebbe prima necessario eliminare il trigger e crearne uno nuovo. Tuttavia, nelle sezioni **Input** e **Output** di questa interfaccia utente viene visualizzato un segno più (+) per aggiungere più associazioni che consente di accettare più di un valore di input e di generare più di un valore di output.

1. Dalla colonna **Input**, selezionare **+ Nuovo input**. Come illustrato nello screenshot seguente, viene visualizzato un elenco di tutti i tipi di associazioni di input possibili.

    ![Screenshot che mostra l'elenco di associazioni di input possibili.](../media/3-func-input-bindings-selector-small.PNG)

   Si consiglia di considerare ognuna di queste associazioni di input e come sia possibile usarle in una soluzione. Ne esistono molte tra cui scegliere. L'elenco potrebbe anche essere cambiato dall'ultima volta che si è letto questo modulo, in quanto sono supportate sempre più origini dati.

1. Nel corso del modulo si tornerà all'aggiunta di associazioni di input ma, per il momento, selezionare **Annulla** per ignorare questo elenco.

1. Nella colonna **Output**, selezionare **+ Nuovo output**. Come illustrato nello screenshot seguente, viene visualizzato un elenco di tutti i tipi di associazioni di output possibili.\

    ![Screenshot che mostra l'elenco di associazioni di output possibili.](../media/3-func-output-bindings-selector-small.PNG)

   Come è possibile notare, sono disponibili molti tipi di associazioni di output. Nel corso del modulo si tornerà all'aggiunta di associazioni di output ma, per il momento, selezionare **Annulla** per ignorare questo elenco.

Finora è stato illustrato come creare un'app per le funzioni e come aggiungervi una funzione. Si è vista una semplice funzione in azione, che viene eseguita quando riceve una richiesta HTTP. Sono stati esaminati anche l'interfaccia utente del portale di Azure e i tipi di associazioni di input e di output disponibili per le funzioni. Nella prossima unità si userà un'associazione di input per leggere il testo da un database.