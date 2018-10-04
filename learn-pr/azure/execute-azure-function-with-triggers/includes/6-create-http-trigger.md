In questa unità verrà creata una funzione di Azure che accetta una richiesta HTTP con un'unica stringa. La funzione restituisce una stringa al chiamante per indicare l'esito positivo o negativo. Continueremo a usare le funzioni partendo dall'esercizio precedente.

## <a name="create-an-http-trigger"></a>Creare un trigger HTTP

Continuare a usare l'applicazione Funzioni di Azure esistente e aggiungere un trigger HTTP.

1. Assicurarsi di aver eseguito l'accesso al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato il sandbox.

1. Passare alla schermata **Tutte le risorse** e selezionare l'app per le funzioni.

1. Selezionare **Funzioni** e quindi l'icona del segno più (+).

1. Selezionare **HTTP trigger** (Trigger HTTP).

1. Lasciare **Nome** impostato sul valore predefinito.

1. Impostare il **livello di autorizzazione** su **Anonimo**.

1. Selezionare **Crea**.

1. Verificare rapidamente il codice generato automaticamente per capirne i contenuti. Il parametro *req* rappresenta la richiesta in ingresso e contiene un parametro *name*. È possibile controllare se *name* ha un valore. In caso affermativo, viene restituito un messaggio di saluto. In caso contrario, viene restituito un messaggio di errore.

## <a name="get-your-function-url"></a>Ottenere l'URL della funzione

Dopo aver creato il trigger HTTP, è possibile ottenere l'URL della funzione così da poter iniziare a effettuare una richiesta.

1. Selezionare il trigger HTTP per aprire la schermata di codice.

1. A destra di **Esegui** selezionare **Ottieni URL della funzione**.

1. Selezionare **Copia** e quindi chiudere la finestra popup URL della funzione.

## <a name="issue-a-get-request-to-your-http-trigger"></a>Inviare una richiesta GET al trigger HTTP

L'URL della funzione è stato copiato negli Appunti. È possibile inviare una richiesta GET per vedere se si ottiene una risposta.

1. Aprire una nuova scheda nel Web browser.

1. Incollare l'URL nella barra degli indirizzi.

1. Aggiungere un parametro della stringa di query denominato *name* con il nome, ad esempio `.../api/HttpTriggerCSharp1?name=Jesse`

1. Premere <kbd>INVIO</kbd> per inviare la richiesta.
