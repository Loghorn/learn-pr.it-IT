Abbiamo creato tutte le necessarie funzioni di Azure e sia possibile eseguirli in locale.

In questa lezione, è connettersi nostro funzioni di Azure con Slack e creare un Slack _barra_ comando che chiama la funzione di Azure e visualizza l'immagine risultante mojified nella finestra di slack.

Al termine di questa lezione si apprenderà:

- Come creare un'app Slack e la connessione di un comando barra con un endpoint di funzioni di Azure.
- Come testare il comando barra in locale, senza la distribuzione nel cloud.

## <a name="using-ngrok-to-develop-with-slash-commands"></a>Uso di ngrok per sviluppare comandi barra

Le funzioni di Azure sono attualmente in esecuzione in locale; solo Tuttavia, slack viene eseguito nel cloud, su internet.

Quando si configura slack _barra_ comando nella sezione successiva, sta per richiedere un URL pubblico che viene chiamato ogni volta che un utente esegue un comando della barra.

Vogliamo fornire loro i `RespondToSlackCommand` URL, ma che esiste solo nel nostro computer locale.

Come posso avere servizi nell'accesso al cloud alle risorse nel computer locale per lo sviluppo?

È un'ottima soluzione chiave del rebus `ngrok` è uno strumento che consente di creare un tunnel sicuro nel computer locale da internet.

1. Visit https://ngrok.com/download
2. Seguire le istruzioni per scaricare e installare il `ngrok` pacchetto nel computer locale.
3. Eseguire `ngrok http 7071` in un terminale.
   ![ngrok](/media-drafts/9.ngrok.png)
4. Questo visualizza un URL pubblico che terminano con `ngrok.io` ad esempio `http://bbade778.ngrok.io`, prendere nota di questo è necessario nella sezione successiva.

> **Nota**
>
> Ciò `http://*.ngrock.io` URL è possibile usare esattamente come è stato usato `http://localhost:7071`, provare con un'immagine come illustrato di seguito `http://[ngrok-url]/api/MojifyImage?imageUrl=[url-to-an-image]`

## <a name="create-a-slack-app"></a>Creare un'app slack

Esiste un comando barra come parte di un'app slack, slack _bot_.

Visitare [creare App Slack](https://api.slack.com/apps/new)

Scegliere un' `App Name` e associarlo con il `Workspace` creato all'inizio di questa esercitazione e quindi premere `Create App`, come illustrato di seguito:

![Creare App Slack](/media-drafts/9.create-slack-app.png)

Una volta che viene creato quindi dobbiamo andare in app slack e creare un comando della barra.

## <a name="create-a-slash-command"></a>Creare un comando della barra

Fare clic su di `Slash Commands` voce di menu.

![Comandi barra](/media-drafts/9.slash-commands.png)

Fare clic su`Create New Command`.

- Immettere qualsiasi nome desiderato per il comando barra nel `command` campo.
- Immettere l'URL pubblico ngrok nel `Request URL` campo

  > **IMPORTANTE**
  >
  > Ricordarsi di aggiungere `/api/RespondToSlackCommand` all'URL

- Aggiungere un hint per la breve descrizione e l'utilizzo.
- Premere `Save`

![Comandi barra](/media-drafts/9.create-slash-command.png)

Dopo il salvataggio verrà visualizzata una schermata come segue:

![Barra i comandi riuscita](/media-drafts/9.create-slash-commands-success.png)

## <a name="install-the-slack-app-to-the-workspace"></a>Installare l'app slack all'area di lavoro

Per poter usare l'App nell'app slack nell'area di lavoro, è necessario installarlo.

1. Selezionare il `Basic Information` voce di menu

2. Espandere la `Install your app to your workspace` opzione, quindi premere il `Install app to workspace` pulsante.

   ![Installare l'App all'area di lavoro](/media-drafts/9.install-app-to-workspace.png)

3. Potrebbe chiedere di autenticare l'applicazione come segue:

   ![Eseguire l'autenticazione dell'App dell'area di lavoro](/media-drafts/9.authenticate-slack-app.png)

## <a name="try-it-out"></a>Provare il servizio

Ora che il comando della barra è stato creato e connesso alla nostra istanza in esecuzione in locale di funzioni di Azure tramite il collegamento ngrok è possibile eseguirne il test.

1. Passare all'area di lavoro slack che è associato all'app slack.
2. Passare a qualsiasi finestra di chat e tipo `/mojify` (o nome assegnato all'app)
3. Se tutto ha funzionato correttamente, dovrebbe essere `mojify` come un'opzione

   ![Controllo Mojify](/media-drafts/9.slack-check-mojify.png)

4. A questo punto digitare `/mojify` e aggiungere un URL immagine, quindi premere INVIO, come illustrato di seguito:

   ![Tipo Mojify](/media-drafts/9.slack-type-mojify.png)
