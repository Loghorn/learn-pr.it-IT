A questo punto è possibile iniziare a implementare il servizio relativo alla temperatura. Nell'unità precedente si è stabilito che una soluzione senza server sarebbe la più adatta alle specifiche esigenze. Ora verrà creata un'app per le funzioni per contenere la funzione di Azure.

## <a name="what-is-a-function-app"></a>Informazioni sulle app per le funzioni
Le funzioni sono ospitate in un contesto di esecuzione chiamato **app per le funzioni**. Le app per le funzioni vengono definite per raggruppare e strutturare in modo logico le funzioni e una risorsa di calcolo in Azure. Nell'esempio dell'ascensore, si crea un'app per le funzioni per ospitare il servizio relativo alla temperatura dell'ingranaggio di trasmissione dell'ascensore. Per creare l'app per le funzioni è necessario prendere alcune decisioni. In particolare, scegliere un piano di servizio e selezionare un account di archiviazione compatibile.

### <a name="choosing-a-service-plan"></a>Scelta di un piano di servizio
Le app per le funzioni possono usare uno dei due tipi di piani di servizio disponibili. Il primo è il **piano di servizio a consumo**. Questo è il piano più adatto quando si usa la piattaforma applicativa serverless di Azure. Consente la scalabilità automatica e applica un addebito quando le funzioni sono in esecuzione. Il piano di servizio a consumo prevede un periodo di timeout configurabile per l'esecuzione di una funzione. Per impostazione predefinita il timeout è di 5 minuti, ma può essere configurato per una durata massima di 10 minuti. 

Il secondo piano è detto **piano di servizio app di Azure**. Questo piano consente di evitare periodi di timeout consentendo l'esecuzione della funzione in modalità continua in una macchina virtuale definita dall'utente. Quando si usa un piano di servizio app, si è responsabili della gestione di risorse dell'app per cui viene eseguita la funzione, quindi tecnicamente non è un piano serverless. Può tuttavia essere la scelta migliore se le funzioni vengono eseguite in modalità continua o richiedono una potenza di elaborazione o un tempo di esecuzione più elevati rispetto a quelli che può offrire il piano a consumo. 

### <a name="storage-account-requirements"></a>Requisiti dell'account di archiviazione
Quando si crea un'app per le funzioni, è necessario collegarla a un account di archiviazione. È possibile selezionare un account esistente o crearne uno nuovo. L'app per le funzioni usa questo account di archiviazione per le operazioni interne, ad esempio la registrazione dei dati di esecuzione delle funzioni e la gestione dei trigger di esecuzione. Nell'ambito del piano di servizio a consumo è anche la posizione in cui vengono archiviati il codice e il file di configurazione della funzione.

## <a name="create-a-function-app"></a>Creare un'app per le funzioni
Ora verrà creata un'app per le funzioni nel portale di Azure.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true) con il proprio account Azure.

2. Selezionare il pulsante **Crea una risorsa** visualizzato nell'angolo superiore sinistro del portale di Azure, quindi selezionare **Inizia > Serverless Function App (App per le funzioni serverless)** per aprire il pannello *Crea* di App per le funzioni. In alternativa, è possibile usare l'opzione **Calcolo > App per le funzioni** opzione che apre lo stesso pannello.
  
  ![Portale di Azure in cui è evidenziata la selezione *Crea una risorsa* seguita da Calcolo e App per le funzioni](../media-draft/3-create-function-app-blade.png)

3. Scegliere un nome app globalmente univoco. Questo nome sarà l'URL di base del servizio. Ad esempio, è possibile assegnare all'app il nome **escalator-functions-xxxxxxx**, sostituendo la x con le iniziali e l'anno di nascita dell'utente. Se il nome non risulta globalmente univoco, è possibile provare un'altra combinazione. I caratteri validi sono a-z, 0-9 e -.

4. Selezionare la sottoscrizione di Azure in cui ospitare l'app per le funzioni.

5. Creare un nuovo gruppo di risorse denominato **escalator-functions-group**. L'impiego di un gruppo per contenere tutte le risorse usate in questo modulo semplificherà le successive operazioni di pulizia.

6. Selezionare **Windows** come sistema operativo.

7. Per **Piano di hosting** selezionare il **piano a consumo**, ovvero l'opzione di hosting senza server.

8. Selezionare la località geografica più vicina all'utente (o ai clienti).

9. Creare un nuovo account di archiviazione. Azure assegna un nome all'account in base al nome dell'app. È possibile modificarlo, se necessario, ma deve anche essere univoco.

10. Verificare che Azure Application Insights sia **attivato** e selezionare l'area più vicina all'utente (o ai clienti).
Al termine, la configurazione sarà simile a quella rappresentata nello screenshot seguente.

  ![Schermata di configurazione *Crea* di App per le funzioni, con tutti i campi configurati in base alle istruzioni precedenti.](../media-draft/3-create-function-app-settings.png)

11. Selezionare **Crea**; il processo di distribuzione richiederà alcuni minuti. L'utente riceverà una notifica al completamento dell'operazione.

## <a name="verify-your-azure-function-app"></a>Verificare l'app per le funzioni di Azure

1. Nel menu a sinistra del portale di Azure selezionare **Gruppi di risorse**. Viene visualizzato **escalator-functions-group** nell'elenco dei gruppi disponibili.

  ![Schermata Gruppi di risorse nel portale di Azure con il gruppo di risorse escalator-functions-group visualizzato.](../media-draft/3-resource-group.png)

2. Selezionare **escalator-functions-group**. Viene visualizzato un elenco di risorse simile al seguente.
  
  ![Tutte le risorse contenute nel gruppo escalator-functions-group, comprese le voci relative a piano di servizio app, account di archiviazione, Application Insights e servizio app](../media-draft/3-resource-list.png)

L'elemento con l'icona a forma di fulmine, elencato come servizio app, è la nuova app per le funzioni. Fare clic sull'elemento per aprire i dettagli sulla nuova funzione, a cui è stato assegnato un URL pubblico. Aprendo l'URL in un browser, viene visualizzata una pagina Web predefinita che indica che l'app per le funzioni è in esecuzione.
