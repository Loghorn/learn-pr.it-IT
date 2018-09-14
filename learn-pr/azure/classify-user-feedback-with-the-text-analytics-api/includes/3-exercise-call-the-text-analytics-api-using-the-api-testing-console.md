Per visualizzare l'API di Analitica di testo in azione, è possibile effettuare alcune chiamate usando l'API test console strumento incorporato che si trova nella documentazione di riferimento online. Prima di procedere, è necessario una chiave di accesso per eseguire tali chiamate.

## <a name="create-an-access-key"></a>Creare una chiave di accesso

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Ogni chiamata all'API traduzione testuale Analitica richiede una chiave di sottoscrizione. Spesso chiamato una chiave di accesso, viene utilizzato per verificare di avere accesso a effettuare la chiamata. Si userà il portale di Azure per ottenere una chiave. 

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Fare clic su **crea una risorsa**

1. In Azure Marketplace, selezionare **intelligenza artificiale e Machine Learning** per visualizzare un elenco delle API disponibili. Fare clic su **Vedi tutto** nell'angolo superiore destro della pagina per visualizzare l'elenco completo di API servizi cognitivi.

1. Trovare **Analitica testo** nell'elenco di servizi cognitivi e selezionarlo.
    ![Riquadro schermata di intelligenza artificiale e Machine Learning, con l'API di Analitica testo selezionato nell'elenco.](../media/select-text-analytics.PNG)

1. Nella pagina Crea visualizzata, immettere i valori seguenti in ogni campo.

|Proprietà  | Valore  | Descrizione  |
|---------|---------|---------|
|Nome     |    MyTextAnalyticsAPIAccount     |  Il nome dell'account servizi cognitivi. È consigliabile usare un nome descrittivo. I caratteri validi sono `a-z`, `0-9` e `-`.    |
|Sottoscrizione     |  Sottoscrizione in uso       |   La sottoscrizione in cui l'account con nuove API di servizi cognitivi **API traduzione testuale Analitica** viene creato.      |
|Posizione     |  Stati Uniti occidentali       |  Scegliere una [regione](https://azure.microsoft.com/regions/) vicino a te.       |
|Piano tariffario     | **F0 gratuito**     |   Il costo dell'account di servizi cognitivi dipende dall'uso effettivo e dalle opzioni che scelte. È consigliabile selezionare il livello gratuito per il nostro scopo.      |
|Gruppo di risorse     |  Selezionare **Usa esistente** e scegliere <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>       |  Nome del nuovo gruppo di risorse in cui creare l'account API servizi cognitivi testo Analitica.       |

Di seguito è riportata una schermata di ciò che il **Create** pagina è simile a quando è stata completata.

![Screenshot dell'interfaccia utente per la creazione di un account di Analitica di testo con tutti i campi vengono compilati con i valori suggeriti nella tabella precedente.](../media/create-text-analytics-account.PNG)

1. Selezionare **Create** nella parte inferiore della pagina per avviare il processo di creazione di account.  Guarda per una notifica che la distribuzione è in corso. Quindi, si riceverà una notifica che l'account sia stato distribuito correttamente al gruppo di risorse.

![Distribuzione riuscita notifica con un pulsante Vai alle risorse e un Pin al pulsante di dashboard.](../media/deploy-resource-group-success.PNG)

Ora che abbiamo l'account servizi cognitivi, è possibile trovare la chiave di accesso in modo che è possibile iniziare a chiamare l'API.

1. Fare clic sui **Vai alla risorsa** pulsante il *distribuzione ha avuto esito positivo* notifica. Si aprirà l'account di avvio rapido.

1. Selezionare il **tasti** voce di menu dal menu a sinistra o nel *individuare le chiavi* sezione della Guida rapida.  Questa azione apre la **gestire le chiavi** pagina.

1. Copiare una delle chiavi usando il pulsante Copia.

![Gestire le chiavi utente interfaccia che visualizza il nome dell'account servizi cognitivi e le voci chiave 1 e la chiave 2.](../media/manage-keys.PNG)

> [!IMPORTANT]
> Mantieni sempre le chiavi di accesso sicuro e non condividerle.

1. Store questa chiave per il resto di questo modulo. Useremo per chiamare le API dalla console di test e nella parte restante od del modulo.

Ora che abbiamo la chiave, possiamo passare alla console di test e richiedere l'API per un giro di prova.

## <a name="call-the-api-from-the-testing-console"></a>Chiamare l'API dalla console di test

1. Passare il [API traduzione testuale Analitica](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7?azure-portal=true) fare riferimento alla documentazione in un browser a scelta.

    La pagina di destinazione viene visualizzato un menu a sinistra e il contenuto verso destra. Il menu di scelta Elenca i metodi POST che è possibile chiamare l'API di Analitica di testo. Questi endpoint vengono **linguaggio rilevare**, **entità**, **frasi chiave**, e **Sentiment**.  Per chiamare una di queste operazioni, è necessario eseguire alcune operazioni.

    - Selezionare il metodo da chiamare.
    - Selezionare un test console che corrisponde all'area o posizione che sono selezionate più indietro in questa lezione.
    - Aggiungere la chiave di accesso che è stato salvato in precedenza nella lezione a ogni chiamata.

1. Primo piano il menu a sinistra, seleziona **Sentiment**. Questa selezione consente di aprire la documentazione di Sentiment a destra. Come illustrato nella documentazione, si effettua una chiamata REST nel formato seguente:

    `https://[location].api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`

Passiamo la chiave di sottoscrizione o chiave di accesso, nelle **ocp-Apim-Subscription-Key** intestazione.

## <a name="make-some-api-calls"></a>Effettuare alcune chiamate API

1. Selezionare un paese dall'elenco in questa pagina che corrisponda al percorso che abbiamo scelto quando si crea l'account servizi cognitivi in precedenza in questa lezione.  Ad esempio, se è stato scelto *Stati Uniti occidentali* precedenti durante la creazione dell'account, quindi selezionarla qui come indicato di seguito.
    ![Sito di riferimento screenshot dell'API traduzione testuale Analitica con la voce di menu del Sentiment è selezionata, seguito da West US.](../media/select-testing-console-region.png)

1. La pagina successiva visualizzata è una console API interattiva, in tempo reale.  Incollare la chiave di accesso è stato salvato in precedenza nel campo della pagina con l'etichetta **Ocp-Apim-Subscription-Key**. Si noti che la chiave viene scritta automaticamente nella finestra di richiesta HTTP come un valore di intestazione.

1. Scorrere fino alla parte inferiore della pagina e fare clic su **inviare**. Scomponendo cosa accade, esaminando in dettaglio questa schermata.

Nella sezione intestazioni dell'interfaccia utente, impostiamo la chiave di accesso o la sottoscrizione, nell'intestazione della richiesta.

![Screenshot della sezione di intestazioni.](../media/2-marker.PNG)

Successivamente, abbiamo il corpo della richiesta sezione che contiene un **documenti** matrice. Ogni documento nella matrice come tre proprietà. Le proprietà sono *"lingua"*, *"id"*, *"text"*. Il *"id"* è un numero in questo esempio, ma può essere un nome qualsiasi, purché sia univoco nella matrice di documenti. In questo esempio viene anche passata documenti scritti in tre lingue differenti. Oltre 15 lingue sono supportate nella funzionalità Sentiment dell'API di Analitica di testo. Per altre informazioni, consultare [lingue supportate nell'API di Analitica testo](https://docs.microsoft.com//azure/cognitive-services/text-analytics/text-analytics-supported-languages). Le dimensioni massime di un singolo documento sono di 5.000 caratteri e una richiesta può avere fino a 1.000 documenti.

![Sezione di schermata del corpo della richiesta](../media/3-marker.PNG)

La richiesta completa, incluse le intestazioni e l'URL della richiesta vengono visualizzati nella sezione successiva. In questo esempio, si noterà che le richieste vengono indirizzate a un URL che iniziano con `westus`. L'URL è diverso a seconda dell'area selezionata.

![Numero di sezione 4. ](../media/4-marker.PNG)
 ![Sezione numero 5.](../media/5-marker.PNG)

Quindi abbiamo informazioni relative alla risposta. Nell'esempio, noteremo che la richiesta è stata un successo e il codice `200` è stato restituito. È anche possibile osservare che il round trip ha impiegato ms 38.

![Numero di sezione cinque.](../media/6-marker.PNG)

Infine, viene visualizzata la risposta alla richiesta. La risposta contiene le informazioni dettagliate che l'API di Analitica testo aveva sui nostri documenti. Viene restituita una matrice di documenti di Microsoft, senza il testo originale. Si otterrà un' *"id"* e *"punteggio"* per ogni documento. L'API restituisce un punteggio numerico compreso tra 0 e 1. I valori prossimi a 1 indicano una valutazione positiva, mentre i valori prossimi a 0 indicano una valutazione negativa. Un punteggio pari a 0,5 indica la mancanza di sentiment, un'istruzione neutro. In questo esempio abbiamo due documenti molto positivi e un solo documento negativo.

![Numero di sezione cinque.](../media/7-marker.PNG)

La procedura è stata completata. La prima chiamata all'API di Analitica di testo apportate senza dover scrivere una riga di codice. È possibile rimanere nella console e provare altre chiamate. Di seguito sono riportati alcuni suggerimenti:

- Modificare i documenti nella sezione numero 2 e vedere l'API restituisce.
- Provare gli altri metodi, **linguaggio rilevare**, **entità** e **frasi chiave**, usando la stessa chiave di sottoscrizione.
- Provare a effettuare una chiamata da un'area diversa con l'abbonamento e osservare cosa accade.

La console di test API è un ottimo modo per esplorare le funzionalità di questa API. Ora che è stata esaminata per se stessi, è possibile passare a inserire l'intelligence in uno scenario reale.