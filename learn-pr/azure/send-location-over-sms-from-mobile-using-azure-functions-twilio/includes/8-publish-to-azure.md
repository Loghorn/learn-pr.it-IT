L'app e la funzione di Azure sono ora complete e in esecuzione in locale. In questa unità la funzione viene pubblicata in Azure per l'esecuzione nel cloud.

> In questa unità la funzione verrà pubblicata da Visual Studio. Si tratta di un modo efficace per iniziare a usare modelli di prova, prototipi e apprendimento, ma **non** per un'app di qualità idonea ad ambienti di produzione. È consigliabile usare un tipo di distribuzione basata sull'integrazione continua. Altre informazioni sono disponibili nei [documenti di distribuzione di Funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment).
>
## <a name="publishing-your-app-to-azure"></a>Pubblicazione di un'app in Azure

È possibile pubblicare le funzioni di Azure in Azure da Visual Studio.

1. Arrestare il runtime di Funzioni di Azure locale se è ancora in esecuzione dall'unità precedente.

2. Fare clic con il pulsante destro del mouse sull'app `ImHere.Functions` in Esplora soluzioni e scegliere *Pubblica..*.

    ![Pubblicazione diretta nell'app per le funzioni](../media-drafts/8-right-click-publish.png)

3. Nella finestra di dialogo **Selezionare una destinazione di pubblicazione** selezionare *App per le funzioni di Azure* e per **Servizio app di Azure** selezionare *Crea nuovo*. Fare clic su **Pubblica**.

    ![Creazione di un nuovo servizio app di Azure per la pubblicazione](../media-drafts/8-pick-publish-target.png)

4. Selezionare l'account di Azure dall'elenco a discesa nell'angolo superiore destro se si hanno più account di Azure e non è selezionato quello giusto.

5. Fornire un nome all'app per le funzioni. Questo nome deve essere globalmente univoco tra tutte le app per le funzioni nell'intero ambiente di Azure. Usare quindi un nome come "ImHere-\<Nome\>".

6. Selezionare la sottoscrizione in cui si intende creare questa app per le funzioni.

7. Creare un nuovo gruppo di risorse per questa app per le funzioni scegliendo il pulsante **Nuovo...** accanto all'elenco a discesa **Gruppo di risorse** e assegnargli un nome, ad esempio "ImHere". I nomi dei gruppi di risorse devono essere univoci per la sottoscrizione, non globalmente univoci in Azure. Fare quindi clic su **OK**.

    ![Creare un nuovo gruppo di risorse](../media-drafts/8-create-new-resource-group.png)

   La creazione di un nuovo gruppo di risorse ne semplifica la pulizia in un secondo momento. È possibile eliminare il gruppo di risorse con la certezza che tutti gli elementi creati per questa app per le funzioni verranno eliminati contemporaneamente.

8. Creare un nuovo piano di hosting facendo clic sul pulsante **Nuovo...**  accanto all'elenco a discesa **Piano di hosting**. Il nome del piano del servizio app predefinito sarà il nome dell'app con "Plan" alla fine. Impostare la **posizione** sulla posizione più vicina e assicurarsi che **Dimensioni** sia impostata su Consumo. Fare quindi clic su **OK**.

    ![Configurare il piano di hosting](../media-drafts/8-configure-hosting-plan.png)

9. Creare un nuovo account di archiviazione facendo clic sul pulsante **Nuovo...** accanto all'elenco a discesa **Account di archiviazione**. Verrà fornito un nome predefinito. Mantenere tutti i valori predefiniti e fare clic su **OK**.

    ![Creare un account di archiviazione](../media-drafts/8-create-storage-account.png)

10. Fare clic su **Crea** per effettuare il provisioning di tutte le risorse in Azure e pubblicare l'app per le funzioni di Azure.

    ![Creare il servizio app](../media-drafts/8-create-app-service.png)

L'esecuzione del provisioning richiede pochi minuti. Verrà effettuato il provisioning delle risorse seguenti:

* Un account di archiviazione per archiviare i file necessari per l'app per le funzioni di Azure
* Un'app del piano di servizio per gestire le risorse di calcolo necessarie per l'app per le funzioni di Azure
* Il servizio app che esegue la funzione di Azure

La funzione verrà ora pubblicata e sarà disponibile per la chiamata all'indirizzo https://<nome-app>.azurewebsites.net/api/SendLocation.

## <a name="configuring-your-app"></a>Configurazione dell'app

Quando la funzione di Azure era in esecuzione in locale, usava le credenziali di Twilio archiviate in un file `local.settings.json`. Come suggerisce il nome, questo file è per le impostazioni locali, non per le impostazioni di Azure. Prima che la funzione di Azure possa essere chiamata all'interno di Azure, è necessario configurare le impostazioni `TwilioAccountSid` e `TwilioAuthToken`.

1. Nella scheda Pubblica fare clic sull'opzione **Gestisci impostazioni applicazione**.

    ![Opzione Gestisci impostazioni applicazione](../media-drafts/8-application-settings-option.png)

2. Fare clic sul pulsante **Aggiungi** per aggiungere una nuova impostazione. Denominarla "TwilioAccountSid" e impostare il valore sul SID dell'account Twilio. Ripetere questo passaggio per il token di autenticazione usando il nome "TwilioAuthToken".

    ![Impostare le credenziali Twilio nelle impostazioni dell'applicazione](../media-drafts/8-set-creds-in-app-settings.png)

3. Fare clic su **OK**.

4. Fare clic su **pubblica** per ripubblicare l'app per le funzioni di Azure con le nuove impostazioni dell'applicazione.

    ![Pulsante Pubblica](../media-drafts/8-publish-application-button.png)

## <a name="pointing-the-mobile-app-to-azure"></a>Come puntare l'app per dispositivi mobili ad Azure

1. Dalla scheda Pubblica copiare l'**URL del sito** usando il pulsante Copia accanto al valore.

    ![Copiare l'URL del sito dalla scheda Pubblica](../media-drafts/8-copy-site-url.png)

2. Aprire il file `MainViewModel` dal progetto `ImHere`.

3. Aggiornare il valore del campo `baseUrl` in modo che indichi l'URL del sito copiato dalla scheda Pubblica.

4. Modificare il protocollo per questo valore da `http` in `https`. L'URL del sito viene sempre fornito con HTTP, ma è necessario usare HTTPS per chiamare una funzione di Azure.

## <a name="test-it-out"></a>Eseguire il test

1. Impostare l'app `ImHere.UWP` come app di avvio ed eseguirla.

2. Immettere un numero di telefono, quindi fare clic sul pulsante **Invia posizione**.

3. La posizione dovrebbe essere ricevuta con un SMS.

## <a name="summary"></a>Riepilogo

In questa unità si è appreso come pubblicare un progetto di funzioni di Azure in Azure da Visual Studio e configurare le impostazioni dell'applicazione.