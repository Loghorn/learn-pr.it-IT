Infine, è necessario ospitare il codice da qualche parte nel cloud. La tecnologia Azure che si intende usare per eseguire questa operazione è funzioni di Azure.

In questa lezione si apprenderà:

- Come creare un'App di funzione di Azure e `JavaScript` Http Trigger.
- Modalità di esecuzione e il debug di una funzione di Azure in locale.

## <a name="create-an-azure-function-project"></a>Creare un progetto funzioni di Azure

Un progetto di funzioni di Azure è un contenitore per le funzioni multiple. Le funzioni attivate in modi diversi, si saranno attiva funzioni eseguendo una richiesta HTTP.

Esistono molti modi per creare funzioni di Azure; si intende usare Visual Studio Code con l'estensione di funzioni di Azure che è stato installato in precedenza in questo modulo.

È necessario prima creare il progetto di funzione.

1. Tipo di <kbd>Ctrl</kbd>+<kbd>P</kbd> per aprire il riquadro comandi, quindi cercare e selezionare `Azure Functions: Create New Project...`

   > **NOTA**
   >
   > Potrebbe chiedere se si desidera sovrascrivere un file, ad esempio `.gitignore`, risposte **alcun** a tutti!

   ![Creare un nuovo progetto](/media-drafts/7.create-new-project.png)

2. Selezionare la cartella in cui si desidera creare l'app per le funzioni (la prima opzione è la cartella corrente)

   ![Seleziona cartella](/media-drafts/7.select-folder.png)

3. Scegliere `JavaScript` come il linguaggio desiderato.

   ![Seleziona lingua](/media-drafts/7.select-language.png)

4. Se il codice precedente completata correttamente verrà visualizzato il seguente i file creati nella cartella

   ![App creata](/media-drafts/7.app-created.png)

## <a name="create-an-azure-function"></a>Creare una funzione di Azure

Backup successivo è possibile creare la funzione di Azure, il frammento di codice che risponde a una richiesta HTTP.

Anche in questo caso si intende usare l'estensione di codice di Visual Studio.

1.  Tipo di <kbd>Ctrl</kbd>+<kbd>P</kbd> per aprire il riquadro comandi, quindi cercare e selezionare `Azure Functions: Create Function...`

    ![Creare una nuova funzione](/media-drafts/7.create-function.png)

2.  Selezionare la cartella in cui la creazione del progetto di funzione in precedenza (la prima opzione è la cartella corrente)

    ![Seleziona cartella](/media-drafts/7.select-current-project.png)

3.  Stiamo creando un _HTTP Trigger_, se si desidera selezionare tale opzione.

    ![Seleziona cartella](/media-drafts/7.select-trigger.png)

4.  Digitare `MojifyImage` come il nome della funzione da creare

    ![Scegli nome](/media-drafts/7.choose-function-name.png)

5.  Scegliere `Anonymous` come il livello di autenticazione

    > Nota: scegliendo `Anonymous` la funzione è aperto per tutto il mondo e non sicuri.

    ![Scegli nome](/media-drafts/7.choose-auth-level.png)

## <a name="run-the-function-locally"></a>Eseguire la funzione in locale

Una volta completato correttamente questi comandi aver convertito il progetto iniziale in un progetto di funzione con un _HTTP Trigger_ funzione chiamata `MojifyImage`.

Per verificare che tutto funzioni, è possibile eseguire l'app per le funzioni in locale, come illustrato di seguito:

```bash
func host start
```

Verrà avviata localmente; runtime della funzione Se tutto funziona correttamente alla fine si otterrà un risultato simile di seguito stampato nella console:

```output
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

> **Nota**
>
> Questo è uno dei vantaggi dell'installazione del runtime di funzioni di Azure, consente di eseguire la funzione in locale usando la stessa tecnologia sottostante che potrebbe essere utilizzata per eseguire la funzione nell'ambiente di produzione.

Per garantire che la funzione funziona correttamente, visitare l'URL stampato nella console, questo viene stampato nella finestra del browser:

![App per le funzioni funzionante](/media-drafts/7.default-function-app-working.png)

## <a name="debug-the-function-locally"></a>Eseguire il debug in locale la funzione

> **Importante**
>
> Assicurarsi di avere chiuso la `func host start` comando sono stati eseguiti prima di provare a eseguirne il debug usando Visual Studio Code.

Inoltre, possiamo eseguire _ed eseguire il debug_ l'applicazione all'interno di visual studio code.

Aggiungere un punto di interruzione il `index.js` file, nella parte superiore della funzione JavaScript, come segue:

![Aggiungi punto di interruzione](/media-drafts/7.add-breakpoint.png)

Per eseguire la funzione nella prima visita in modalità debug dal menu debug <kbd>Cmd<kbd> + <kbd>MAIUSC<kbd> + <kbd>1!d<kbd>.

Selezionare quindi `Attach to JavaScript Functions` dalla configurazione di debug prima di premere infine il triangolo verde per avviare la sessione di debug.

> **Nota**
>
> Il `Attach to JavaScript Functions` configurazione di debug è stato aggiunto automaticamente quando è stato creato il progetto di funzione.

In alternativa, è possibile premere <kbd>F5<kbd> da un punto qualsiasi nell'applicazione; viene eseguita l'ultima configurazione di debug.

Il `func host start` comando verrà eseguito automaticamente; deve aprire un terminale alla fine con lo stesso output come indicato in precedenza:

```output
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

Perché si sta eseguendo il debug è anche vedere vengono visualizzati sulla barra di menu debug, come illustrato di seguito:

![Barra dei Menu Debug](/media-drafts/7.debug-menu-bar.png)

Ora quando si visita l'URL riportato sopra si interrompe al punto di interruzione specificato ed è possibile scorrere la funzione.

<!-- TODO Find Link -->

È possibile [altre informazioni](https://code.visualstudio.com/docs/editor/debugging) sul debug in Visual Studio Code
