L'app e la funzione di Azure sono ora complete e in esecuzione in locale. In questa unità la funzione viene pubblicata in Azure per l'esecuzione nel cloud.

> [!Note]
> Si pubblicherà la funzione da Visual Studio. Si tratta di un modo efficace per iniziare a usare modelli di prova, prototipi e apprendimento, ma **non** per un'app di qualità idonea ad ambienti di produzione. È consigliabile usare un tipo di distribuzione basata sull'integrazione continua. Altre informazioni sono disponibili nei [documenti sulla distribuzione di Funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment?azure-portal=true).

## <a name="publishing-your-app-to-azure"></a>Pubblicazione di un'app in Azure

È possibile pubblicare le funzioni di Azure in Azure da Visual Studio.

1. Arrestare il runtime di Funzioni di Azure locale se è ancora in esecuzione dall'unità precedente.

1. Fare clic con il pulsante destro del mouse sull'app `ImHere.Functions` in Esplora soluzioni e scegliere *Pubblica*.

    ![Pubblicazione diretta nell'app per le funzioni](../media/8-right-click-publish.png)

1. Nella finestra di dialogo **Selezionare una destinazione di pubblicazione** selezionare *App per le funzioni di Azure* e per **Servizio app di Azure** selezionare *Crea nuovo*. Fare clic su **Pubblica**.

    ![Creazione di una nuova istanza di Servizio app di Azure in cui eseguire la pubblicazione](../media/8-pick-publish-target.png)

1. Accedere ad Azure usando il nome utente e la password nella scheda **Risorse** di queste istruzioni. Se si sta compilando l'app in locale, anziché tramite la macchina virtuale, accedere con l'account di Azure, creandone uno nuovo, se necessario, attraverso i collegamenti nella finestra di dialogo.

1. Lasciare le impostazioni predefinite per tutti i valori. In questo modo verrà creata l'infrastruttura necessaria per l'esecuzione dell'app per le funzioni.

1. Fare clic su **Crea** per effettuare il provisioning di tutte le risorse in Azure e pubblicare l'app di Funzioni di Azure.

    ![Creare il servizio app](../media/8-create-app-service.png)

1. Può essere visualizzata la richiesta di specificare se si vuole aggiornare la versione di Funzioni in Azure. Se viene visualizzata la finestra di dialogo con tale richiesta, selezionare **Sì** per assicurarsi che l'app per le funzioni venga pubblicata con la versione più recente del runtime di Funzioni di Azure.
    ![Finestra di dialogo di aggiornamento di Funzioni di Azure](../media/8-update-functions-on-azure.png)

Il provisioning richiede pochi minuti. Verrà effettuato il provisioning delle risorse seguenti:

- Un account di archiviazione per archiviare i file necessari per l'app per le funzioni di Azure
- Un'app del piano di servizio per gestire le risorse di calcolo necessarie per l'app per le funzioni di Azure
- Il servizio app che esegue la funzione di Azure

La funzione verrà ora pubblicata e sarà disponibile per la chiamata all'indirizzo **https://\<nome-app\>.azurewebsites.net/api/SendLocation**.

## <a name="configuring-your-app"></a>Configurazione dell'app

Quando la funzione di Azure era in esecuzione in locale, usava le credenziali di Twilio archiviate in un file `local.settings.json`. Come suggerisce il nome, questo file è per le impostazioni locali, non per le impostazioni di Azure. Prima che la funzione di Azure possa essere chiamata all'interno di Azure, è necessario configurare le impostazioni `TwilioAccountSid` e `TwilioAuthToken`.

1. Nella scheda Pubblica fare clic sull'opzione **Gestisci impostazioni applicazione**.

    ![Opzione Gestisci impostazioni applicazione](../media/8-application-settings-option.png)

1. La finestra di dialogo **Impostazioni applicazione** illustra le impostazioni dell'applicazione, sia quelle con un valore locale che quelle con un valore remoto. Il valore locale deriva dal file `local.settings.json` e il valore remoto è quello usato dalla funzione quando è ospitata in Azure. Copiare i valori dalla casella *Locale* alla casella *Remoto* per i valori **TwilioAccountSid** e **TwilioAuthToken**.

    ![Impostare le credenziali Twilio nelle impostazioni dell'applicazione](../media/8-set-creds-in-app-settings.png)

1. Fare clic su **OK**. I valori verranno pubblicati nell'app per le funzioni di Azure.

## <a name="pointing-the-mobile-app-to-azure"></a>Come puntare l'app per dispositivi mobili ad Azure

1. Dalla scheda Pubblica copiare l'**URL del sito** usando il pulsante **Copia negli Appunti** accanto al valore.

    ![Copiare l'URL del sito dalla scheda Pubblica](../media/8-copy-site-url.png)

1. Aprire il file `MainViewModel` dal progetto `ImHere`.

1. Aggiornare il valore del campo `baseUrl` in modo che indichi l'URL del sito copiato dalla scheda Pubblica.

## <a name="test-it-out"></a>Eseguire il test

1. Impostare l'app `ImHere.UWP` come app di avvio ed eseguirla.

1. Immettere un numero di telefono, quindi fare clic sul pulsante **Invia posizione**.

1. Si dovrebbe ricevere la posizione tramite SMS.

1. Se viene visualizzato un errore *Il servizio non è disponibile*, verificare la versione del pacchetto NuGet "Microsoft.Azure.WebJobs.Extensions.Twilio" usato dall'app per le funzioni. Deve corrispondere alla versione 3.0.0-rc1, NON alla versione 3.0.0.
1. Se viene visualizzato un errore *Il servizio non è disponibile*, verificare la versione del pacchetto NuGet "Microsoft.Azure.WebJobs.Extensions.Twilio" usato dall'app per le funzioni. Deve corrispondere alla versione 3.0.0-rc1, **NON** alla versione 3.0.0.

## <a name="summary"></a>Riepilogo

In questa unità si è appreso come pubblicare un progetto di funzioni di Azure in Azure da Visual Studio e configurare le impostazioni dell'applicazione.