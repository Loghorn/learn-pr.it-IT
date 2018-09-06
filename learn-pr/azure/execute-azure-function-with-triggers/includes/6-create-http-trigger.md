In questo esercizio verrà creata una funzione di Azure che accetta una richiesta HTTP con un'unica stringa. La funzione restituisce una stringa al chiamante per indicare l'esito positivo o negativo.

> [!NOTE]
> Per completare questo esercizio, assicurarsi di essere connessi al [portale di Azure](https://portal.azure.com?azure-portal=true) con un account valido.

## <a name="create-an-http-trigger"></a>Creare un trigger HTTP

È possibile continuare a usare l'applicazione Funzioni di Azure esistente e aggiungere un trigger HTTP.

1. Selezionare **Funzioni**, quindi l'icona del segno più (+).

    ![Selezionare Funzioni, quindi il segno più (+) al passaggio del mouse](../media-drafts/4-hover-function.png)

2. Selezionare **Trigger HTTP**.

3. Selezionare il linguaggio di programmazione **C#**. 

4. Lasciare **Nome** impostato sul valore predefinito.

5. Impostare il **livello di autorizzazione** su **Anonimo**.

6. Selezionare **Crea**.

7. Verificare rapidamente il codice generato automaticamente per capirne i contenuti. Il parametro *req* rappresenta la richiesta in ingresso e contiene un parametro *name*. È possibile controllare se *name* ha un valore. In caso affermativo, viene restituito un messaggio di saluto. In caso contrario, viene restituito un messaggio di errore.

## <a name="get-your-function-url"></a>Ottenere l'URL della funzione

Dopo aver creato il trigger HTTP, è possibile ottenere l'URL della funzione così da poter iniziare a effettuare una richiesta.

1. Selezionare il trigger HTTP per aprire la schermata di codice.

2. A destra di **Esegui** selezionare **Ottieni URL della funzione**.

3. Selezionare **Copia**.

4. Selezionare **Esegui** per avviare la funzione.

## <a name="issue-a-get-request-to-your-http-trigger"></a>Inviare una richiesta GET al trigger HTTP

L'URL della funzione è stato copiato negli Appunti. È possibile inviare una richiesta GET per vedere se si ottiene una risposta.

1. Aprire una nuova scheda nel Web browser.

2. Incollare l'URL nella barra degli indirizzi.

3. Aggiungere un parametro della stringa di query denominato *name* con il nome, ad esempio `.../api/HttpTriggerCSharp1?name=Jesse`

4. Premere INVIO per inviare la richiesta.

## <a name="clean-up"></a>Eseguire la pulizia

Per assicurarsi che non sono previsti costi per questa funzione, selezionare **Sospendi** sopra la finestra del log.

![Sospendi](../media-drafts/4-pause-timer.png)