Finora in questo modulo è stato gestito ottenere un Slack _barra_ comando funziona ma solo mentre è in esecuzione su localhost e da tunnelling per il computer con ngrok. L'operazione funziona per lo sviluppo ma per rilasciare si desidera che il codice sia in esecuzione nel cloud.

In questa lezione, si intende distribuire la funzione di Azure nel cloud e aggiornare la configurazione di slack per usare la nuova versione di produzione del codice.

Al termine di questa lezione si apprenderà:

- Come creare un'App per le funzioni in Azure
- Come distribuire in Azure usando Visual Studio Code
- Come propagare le impostazioni locali nell'ambiente di produzione

## <a name="create-an-azure-function-app-on-azure"></a>Creare un'App per le funzioni di Azure in Azure

Verrà creata la funzione di azure tramite Visual Studio Code e l'estensione di funzione di Azure.

1. Aprire il riquadro comandi con <kbd>Ctrl</kbd>+<kbd>P</kbd> e selezionare `Azure Function: Deploy to Function App`.

   ![Distribuire in App per le funzioni](/media-drafts/10.deploy-to-function-app.png)

2. Viene richiesto all'utente di confermare che si vuole caricare il progetto corrente, andare avanti e selezionarla.

   ![Seleziona cartella da distribuire](/media-drafts/10.select-folder-to-deploy.png)

3. Scegliere la sottoscrizione che si desidera associare all'app.

   Se si ha familiarità con Azure è necessario avere una sottoscrizione, questa selezione.

4. Scegliere Crea nuova app per le funzioni

   ![Creare nuove App per le funzioni](/media-drafts/10.create-new-function-app.png)

5. Scegliere un nome per l'app

   Viene richiesto di scegliere un nome, chiamarlo di ciò che si vuole. Si chiama il mio `mojifier-slack-function-app`.

   ![Scegliere nome dell'App](/media-drafts/10.choose-app-name.png)

6. Creare un nuovo gruppo di risorse

   Gruppi di risorse sono come verranno raggruppate più servizi di Azure in un unico _app_. È possibile creare una nuova oppure usare un oggetto esistente è già stato creato.

   Se si crea un nuovo uno, verrà automaticamente-consente di popolare un nome per l'utente, è possibile selezionarla o digitare un'altra.

7. Scegli _Crea nuovo account di archiviazione_.

   Un'app di funzione deve essere un account di archiviazione, un luogo più permanente per archiviare i dati e file. È possibile creare una nuova oppure usare un oggetto esistente è già stato creato.

   Popolato automaticamente un nome per l'utente; è possibile selezionarla o digitare un'altra.

8. Selezionare un'area

   App per le funzioni ha durata (TTL) in un punto. Consigliabile scegliere una località più vicina all'utente o gli utenti. Sta selezionando `West Europe`

   ![Scegliere nome dell'App](/media-drafts/10.select-region.png)

9. Attendere quindi il caricamento completo, una volta completata che la finestra di output viene visualizzato un URL simile a:

```output
Deployment to "mojifier-slack-function-app" completed.
HTTP Trigger Urls:
  MojifyImage: https://mojifier-slack-function-app.azurewebsites.net/api/MojifyImage
```

## <a name="try-it-out"></a>Provare questa operazione

A questo punto, dovrebbe essere almeno prevediamo di `RespondToSlackCommand` funzione funzionare poiché non dipende dall'eventuali altre dipendenze.

Visitare `[deployed-app-name].azurefunctions.com/api/RespondToSlashCommand?text=[image-url]`

Se tutte le funzioni, allora è necessario un codice JSON restituito nella finestra del browser nel modo seguente:

```json
{
  "response_type": "in_channel",
  "text": "You must provide an image to mojify",
  "attachments": [
    {
      "image_url": "https://mojifier-slack.azurewebsites.net/api/MojifyImage?imageUrl=undefined"
    }
  ]
}
```

## <a name="upload-settings"></a>Impostazioni di caricamento

Ricordare sono disponibili alcune impostazioni inseriamo nel `local.settings.json` queste sono le chiavi e l'URL per i servizi cognitivi.

`local.settings.json` è esattamente, locale, non si è mai copiato nell'ambiente di produzione quando si distribuisce.

In genere, è necessario passare al portale e forse aggiungere manualmente le impostazioni tramite l'interfaccia utente o l'utilizzo di `func` CLI per eseguire la stessa.

Tuttavia, poiché si sta usando Visual Studio Code, è possibile usare l'estensione di Func Azure e il riquadro comandi per caricare le impostazioni locali per noi.

1.  Aprire il riquadro comandi con <kbd>Ctrl</kbd>+<kbd>P</kbd> e selezionare `Azure Functions: Upload Local Settings`.

    ![Caricare le impostazioni locali](/media-drafts/10.upload-local-settings.png)

2.  Scegliere `local.settings.json`

    ![Scegliere le impostazioni locali](/media-drafts/10.choose-localsettings.png)

3.  Scegliere la sottoscrizione che è associata l'app per le funzioni.

4.  Selezionare l'app per le funzioni che si desidera caricare, data mining viene chiamato `mojifier-slack-function-app`

5.  Se viene richiesto di sovrascrivere tutte le impostazioni, scegliere `No To All`

    Si carica solo le nuove impostazioni che è ciò che vogliamo, in caso contrario, sussiste il rischio si esegue l'override delle impostazioni di ambiente di produzione con le impostazioni dall'ambiente di sviluppo.

    ![Scegliere le impostazioni locali](/media-drafts/10.choose-no-to-all.png)

## <a name="try-it-out"></a>Provare questa operazione

Ora se tutto funziona come previsto quindi a questo punto l'endpoint MojifyImage dovrebbe funzionare.

Visitare `[deployed-app-name].azurefunctions.com/api/MojifyImage?imageUrl=[image-url]`

Se tutto funziona correttamente, si dovrebbe vedere l'immagine mojified nella finestra.

Se non funziona, quindi provare a risolvere i problemi di seguito.

## <a name="update-slack"></a>Aggiornare Slack

Ora che abbiamo installato funzioni nel cloud è possibile aggiornare le impostazioni in Slack in modo che punti al nuovo _pubblica_ URL.

1. Aggiornare Slack, in modo che usa l'URL pubblico del `RespondToSlackCommand`.

![Aggiornare Slack con l'URL di produzione](/media-drafts/10.deploy-update-url.png)

> **NOTA**
>
> Si aggiorna il comando per l'ambiente di produzione, usare _pubblici_, versione del `RespondToSlackCommand` ospitato in `*.azurewebsites.net`

## <a name="try-it-out"></a>Provare questa operazione

Per verificare che tutto sia configurato correttamente e che Slack richiede dati da app distribuita, piuttosto che l'app locale, consentire a disattivare `ngrok` per il momento.

Quindi head per slack finestra e digitare il comando della barra di slack che preferisci.

`/mojify <image>`

Se è tutto funzioni come previsto, dovremmo vedere un'immagine visualizzata nella finestra di slack come segue:

![Win](/media-drafts/10.publish-success.png)
