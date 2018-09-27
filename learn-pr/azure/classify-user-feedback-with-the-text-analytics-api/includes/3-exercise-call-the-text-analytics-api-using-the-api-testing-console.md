Per vedere l'API Analisi del testo in azione, verranno eseguite alcune chiamate tramite lo strumento incorporato della console di test dell'API accessibile dalla documentazione di riferimento online. Prima di proseguire, è necessaria una chiave di accesso per eseguire tali chiamate.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-access-key"></a>Creare una chiave di accesso

Ogni chiamata all'API Analisi del testo richiede una chiave di sottoscrizione. Spesso chiamata chiave di accesso, viene usata per verificare di disporre dell'accesso per effettuare la chiamata. Per ottenere una chiave si userà il portale di Azure.

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.

1. Fare clic su **Crea una risorsa**.

1. Nella casella di ricerca **Cerca nel Marketplace** digitare *analisi del testo* e premere INVIO.

1. Selezionare **Analisi del testo** nei risultati della ricerca e quindi selezionare il pulsante **Crea** nella parte inferiore destra della schermata.

1. Nella pagina **Crea** che si aprirà immettere i valori seguenti in ogni campo.

    |Proprietà  | Valore  | Descrizione  |
    |---------|---------|---------|
    |Nome     |    MyTextAnalyticsAPIAccount     |  Nome dell'account di Servizi cognitivi. È consigliabile usare un nome descrittivo. I caratteri validi sono `a-z`, `0-9` e `-`.    |
    |Sottoscrizione     |  Sottoscrizione Concierge    |   La sottoscrizione in cui viene creato il nuovo account dell'API Servizi cognitivi con l'**API Analisi del testo**.      |
    |Località     |  *scegliere una regione dall'elenco a discesa*       |  Scegliere un'area vicina alla propria località dall'elenco seguente. |
    |Piano tariffario     | **F0 gratuito**     |   Il costo dell'account di Servizi cognitivi dipende dall'uso effettivo e dalle opzioni che vengono selezionate. È consigliabile scegliere il livello gratuito per questo esercizio.      |
    |Gruppo di risorse     |  Selezionare **Usa esistente** e scegliere <rgn>[Nome gruppo di risorse sandbox]</rgn>       |  Nome del nuovo gruppo di risorse in cui creare l'account dell'API Analisi del testo di Servizi cognitivi.       |

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]


1. Selezionare **Crea** nella parte inferiore della pagina per avviare il processo di creazione dell'account. Attendere la notifica che la distribuzione è in corso. Si riceverà quindi una notifica che l'account è stato distribuito correttamente al gruppo di risorse.

    ![Notifica della corretta distribuzione con un pulsante Vai alla risorsa e un pulsante Aggiungi al dashboard.](../media/deploy-resource-group-success.PNG)

## <a name="get-the-access-key"></a>Ottenere la chiave di accesso

Ora che si possiede l'account di Servizi cognitivi, è il momento di trovare la chiave di accesso in modo da iniziare a chiamare l'API.

1. Fare clic sul pulsante **Vai alla risorsa** all'interno della notifica di *distribuzione completata*. Questa azione apre l'Avvio rapido dell'account.

1. Selezionare la voce di menu **Chiavi** dal menu a sinistra o nella sezione *Individuare le chiavi* dell'Avvio rapido. Questa azione apre la pagina **Gestisci chiavi**.

1. Copiare una delle chiavi usando il pulsante Copia.

    ![Interfaccia utente di Gestisci chiavi che visualizza il nome dell'account di Servizi cognitivi e le voci Chiave 1 e Chiave 2.](../media/manage-keys.PNG)

    > [!IMPORTANT]
    > Mantenere sempre le chiavi di accesso in un luogo sicuro e non condividerle con altri utenti.

1. Tenere da parte questa chiave per il resto del modulo. Verrà usata a breve per eseguire chiamate API dalla console di test e nella parte restante del modulo.

## <a name="call-the-api-from-the-testing-console"></a>Chiamare l'API dalla console di test

Ora che si ha la chiave, è possibile passare alla console di test e provare l'API.

1. Passare alla documentazione di riferimento dell'[API Analisi del testo](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7?azure-portal=true) nel browser in uso.

    La pagina di destinazione visualizza un menu a sinistra e il contenuto a destra. Il menu elenca i metodi POST che è possibile chiamare nell'API Analisi del testo. Questi endpoint sono **Rileva lingua**, **Entità**, **Frasi chiave** e **Sentiment**. Per chiamare una di queste operazioni, è necessario procedere come segue.

    - Selezionare il metodo da chiamare.
    - Selezionare una console di test corrispondente all'area o alla località selezionate più indietro in questa lezione.
    - Aggiungere la chiave di accesso salvata in precedenza in questa lezione a ogni chiamata.

1. Dal menu a sinistra selezionare **Valutazione**. Questa selezione consente di aprire la documentazione per l'analisi del sentiment a destra. Come illustrato nella documentazione, si effettuerà una chiamata REST nel formato seguente:

    `https://[location].api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`

Verrà passata la chiave di sottoscrizione, o chiave di accesso, nell'intestazione **ocp-Apim-Subscription-Key**.

## <a name="make-some-api-calls"></a>Eseguire alcune chiamate API

1. Selezionare un'area dall'elenco in questa pagina corrispondente alla località selezionata quando si è creato l'account di Servizi cognitivi più indietro in questa lezione. Ad esempio, se in fase di creazione dell'account è stato scelto *Stati Uniti occidentali*, selezionare quest'area come indicato di seguito.
    ![Screenshot del sito di riferimento dell'API Analisi del testo con la voce di menu Sentiment selezionata, seguita da Stati Uniti occidentali.](../media/select-testing-console-region.png)

1. La pagina successiva visualizzata è una console API interattiva in tempo reale. Incollare la chiave di accesso salvata in precedenza nel campo della pagina con l'etichetta **Ocp-Apim-Subscription-Key**. Si noti che la chiave viene scritta automaticamente nella finestra della richiesta HTTP come valore di intestazione.

1. Scorrere fino alla parte inferiore della pagina e fare clic su **Invia**. È possibile comprendere cosa sta succedendo osservando questa schermata in dettaglio.

Nella sezione Intestazioni dell'interfaccia utente viene impostata la chiave di accesso, o di sottoscrizione, nell'intestazione della richiesta.

![Screenshot della sezione Intestazioni.](../media/2-marker.PNG)

Si vede quindi la sezione del corpo della richiesta che contiene una matrice di **documenti**. Ogni documento nella matrice ha tre proprietà. Le proprietà sono *"language"*, *"id"* e *"text"*. *"id"* è un numero in questo esempio, ma può essere un elemento qualsiasi, purché sia univoco nella matrice di documenti. In questo esempio vengono anche passati documenti scritti in tre lingue diverse. La funzionalità di analisi del sentiment dell'API Analisi del testo supporta oltre 15 lingue. Per altre informazioni, vedere [Lingue supportate nell'API Analisi del testo](https://docs.microsoft.com//azure/cognitive-services/text-analytics/text-analytics-supported-languages). La dimensione massima di un singolo documento è di 5.000 caratteri e una richiesta può avere fino a 1.000 documenti.

![Screenshot della sezione Corpo della richiesta](../media/3-marker.PNG)

La richiesta completa, incluse le intestazioni e l'URL della richiesta, è visualizzata nella sezione successiva. In questo esempio si noterà che le richieste vengono indirizzate a un URL che inizia con `westus`. L'URL è diverso a seconda dell'area selezionata.

![Sezione numero quattro.](../media/4-marker.PNG)
![Sezione numero cinque.](../media/5-marker.PNG)

Sono quindi disponibili informazioni sulla risposta. Nell'esempio si noterà che la richiesta è stata completata correttamente ed è stato restituito il codice `200`. È anche possibile osservare che il round trip ha richiesto 38 ms.

![Sezione numero cinque.](../media/6-marker.PNG)

Infine, viene visualizzata la risposta alla richiesta. La risposta contiene le informazioni dettagliate che l'API Analisi del testo ha raccolto sui documenti. Viene restituita una matrice di documenti, senza il testo originale. Si otterrà un valore *"id"* e *"score"* per ogni documento. L'API restituisce un punteggio numerico compreso tra 0 e 1. I valori prossimi a 1 indicano un sentiment positivo, mentre i valori prossimi a 0 indicano un sentiment negativo. Un punteggio pari a 0,5 indica la mancanza di sentiment o una frase neutrale. In questo esempio sono riportati due documenti positivi e un documento negativo.

![Sezione numero cinque.](../media/7-marker.PNG)

La procedura è stata completata. È stata eseguita la prima chiamata all'API Analisi del testo senza aver scritto alcun codice. È possibile rimanere nella console e provare altre chiamate. Di seguito sono riportati alcuni suggerimenti:

- Modificare i documenti nella sezione numero 2 e osservare i valori restituiti dall'API.
- Provare gli altri metodi, **Rileva lingua**, **Entità** e **Frasi chiave**, usando la stessa chiave di sottoscrizione.
- Provare a eseguire una chiamata da un'area diversa con la sottoscrizione in uso e osservare cosa accade.

La console di test dell'API è un ottimo modo per esplorare le funzionalità di questa API. Al termine dell'esplorazione, è possibile procedere e mettere in pratica questa funzionalità di intelligence in uno scenario reale.