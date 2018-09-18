In questa unità viene creata un'app per le funzioni di Azure che verrà richiamata ogni 20 secondi con un trigger timer.

## <a name="create-an-azure-function-app"></a>Creare un'app per le funzioni di Azure

Per iniziare, creare un'app per le funzioni di Azure nel portale.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Nel riquadro di spostamento a sinistra selezionare **Crea una risorsa**.

1. Selezionare **Calcolo**.

1. Individuare e selezionare **App per le funzioni**. Facoltativamente, è anche possibile usare la barra di ricerca per individuare il modello.

    ![Screenshot del portale di Azure che illustra il pannello Crea una risorsa con App per le funzioni evidenziato.](../media/4-click-function-app.png)

1. Scegliere un **Nome dell'app** globalmente univoco.

1. Selezionare una **sottoscrizione**.

1. Creare un nuovo **gruppo di risorse**.

1. Scegliere **Windows** come **sistema operativo**.

1. Scegliere **Piano A consumo** in **Piano di hosting**. Verranno addebitati costi per ogni esecuzione della funzione. Le risorse verranno allocate automaticamente in base al carico di lavoro dell'applicazione.

1. Selezionare una **località**.

1. Creare un nuovo account di **archiviazione**. Per impostazione predefinita, il nome sarà una variante del nome dell'app, ma è possibile modificarlo.

1. Disattivare **Application Insights**.

1. Selezionare **Crea**. Il completamento richiederà alcuni minuti. Al termine della creazione della risorsa, l'icona **Notifiche** nell'area della barra degli strumenti avrà un pulsante per aprirla nel portale di Azure.

## <a name="create-a-timer-trigger"></a>Creare un trigger timer

Verrà ora creato un trigger timer all'interno della funzione.

1. Una volta creata la funzione, selezionare **Tutte le risorse** nel riquadro di spostamento a sinistra.

1. Individuare e selezionare la funzione.

1. Nel nuovo pannello scegliere **Funzioni** e selezionare l'icona con il segno più (+).

    ![Screenshot del portale di Azure che illustra un pannello App per le funzioni con il pulsante (+) del sottomenu Funzioni evidenziato.](../media/4-hover-function.png)

1. Selezionare **Timer**.

1. Selezionare **CSharp** come linguaggio.

1. Selezionare **Creare questa funzione**.

## <a name="configure-the-timer-trigger"></a>Configurare il trigger timer

Si ha un'app per le funzioni di Azure la cui logica stampa un messaggio nella finestra di log. Ora verrà impostata la pianificazione del timer per l'esecuzione ogni 20 secondi.

1. Selezionare **Integrazione**.

1. Immettere il valore seguente nella casella **Pianifica**:

    ```log
    */20 * * * * *
    ```

1. Selezionare **Salva**.

## <a name="start-the-timer"></a>Avviare il timer

Dopo aver configurato il timer, è possibile avviarlo.

1. Selezionare **TimerTriggerCSharp1**.

    > [!NOTE]
    > **TimerTriggerCSharp1** è un nome predefinito. Viene selezionato automaticamente quando si crea il trigger.

1. Selezionare **Esegui**.

A questo punto, nella finestra di log dovrebbe essere visualizzato un messaggio ogni 20 secondi.

## <a name="clean-up"></a>Eseguire la pulizia
<!---TODO: Update for sandbox?--->

Per evitare che vengano addebitati costi per questa funzione, selezionare **Sospendi** sopra la finestra di log per arrestare il timer.

![Screenshot del portale di Azure che illustra un pannello di output dei log delle app per le funzioni di Azure con il pulsante Sospendi evidenziato.](../media/4-pause-timer.png)
