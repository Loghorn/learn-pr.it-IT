Ora che l'app è pronta e in esecuzione nel computer locale, è possibile pubblicarla in Azure. 

In questo esercizio, verrà creata, compilata ed eseguita una nuova applicazione Web ASP.NET nel computer locale.

## <a name="create-a-new-project"></a>Creare un nuovo progetto

### <a name="visual-studio-for-windows"></a>Visual Studio per Windows

Il primo passaggio è avviare Visual Studio e creare un'applicazione Web ASP.NET Core locale.

1. Nella pagina iniziale di Visual Studio selezionare **File**, quindi fare clic su **Nuovo** e su **Progetto**.

1. Nella finestra di dialogo **Nuovo progetto**, nel riquadro di sinistra, selezionare **Web**.

1. Nel riquadro centrale fare clic su **Applicazione Web ASP.NET Core**.

1. Nella parte inferiore della finestra di dialogo, nel campo **Nome**, immettere **Alpine Ski House**.

1. Selezionare una **Posizione** per la nuova soluzione.

1. Fare clic sul pulsante **OK** per creare il progetto.

1. Nella finestra di dialogo **Nuova applicazione Web di ASP.NET Core** verrà visualizzata una selezione di modelli di partenza. Per questo esercizio, selezionare **Applicazione Web** e quindi fare clic su **OK** per creare il progetto.

    ![Finestra di dialogo Nuovo progetto](../media-draft/3-aspnet-templates.png)

    > [!NOTE]
    > È anche possibile selezionare modelli di partenza diversi nella finestra di dialogo in base alle esigenze di sviluppo Web. Nella parte superiore della finestra di dialogo, è anche possibile selezionare la versione di ASP.NET Core. È consigliabile selezionare ASP.NET Core 2.0 o versione successiva.

1. Dovrebbe essere ora disponibile la nuova soluzione per un'applicazione Web ASP.NET Core.

    ![Finestra di dialogo Nuovo progetto](../media-draft/3-new-solution.png)

### <a name="visual-studio-mac"></a>Visual Studio per Mac

1. Nella pagina iniziale di Visual Studio selezionare **File**, quindi fare clic su **Nuovo** e su **Progetto**.

1. In **.NET Core** selezionare **App Web ASP.NET Core** e quindi fare clic su **Avanti**.

1. In **Nome progetto** digitare **AlpineSkiHouse**. In questo modo viene popolato automaticamente anche il nome della soluzione.

1. Selezionare una **posizione** nel computer locale per il progetto.

1. Fare clic su **Crea**.

## <a name="build-and-test-on-your-local-machine"></a>Compilare e testare nel computer locale

A questo punto, è possibile compilare e testare il nuovo progetto nel computer locale per assicurarsi di compilarlo e distribuirlo in locale prima della distribuzione in Azure.

1. Premere **F5** per eseguire l'app in modalità di debug oppure **CTRL+F5** per eseguirla senza collegare il debugger.

    ![Finestra di dialogo Nuovo progetto](../media-draft/3-webapp-launch.png)

Visual Studio avvia IIS Express ed esegue l'app. Quando Visual Studio crea un progetto Web, viene usata una porta casuale per il server Web. Nell'immagine precedente il numero di porta è 44381. Quando si esegue l'app, si noterà probabilmente un numero di porta diverso.

> [!TIP]
> L'avvio dell'app con **CTRL+F5** (modalità non di debug) consente di apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche al codice. Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.

> [!IMPORTANT]
> È possibile notare la sezione nella parte superiore della pagina Web, che offre una posizione per la privacy e i criteri di uso dei cookie. Selezionare **Accetto** per acconsentire al rilevamento. Questa app non tiene traccia delle informazioni personali. Il codice generato dal modello include asset che consentono di soddisfare i requisiti del Regolamento generale sulla protezione dei dati (GDPR).

## <a name="summary"></a>Riepilogo

Il primo passaggio per rendere operativo il sito ASP.NET consiste nel crearlo ed eseguirlo in locale. Ora che il sito è stato creato, si è pronti per distribuirlo in Azure.
