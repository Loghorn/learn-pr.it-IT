In questa unità, creiamo un'app per le funzioni di Azure che viene richiamata ogni 20 secondi usando un trigger del timer.

## <a name="create-an-azure-function-app"></a>Creare un'app per le funzioni di Azure

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Iniziamo creando un'app per le funzioni di Azure nel portale.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Nel riquadro di spostamento a sinistra selezionare **Crea una risorsa**.

1. Selezionare **Calcolo**.

1. Individuare e selezionare **App per le funzioni**. Facoltativamente, è anche possibile usare la barra di ricerca per individuare il modello.

    ![Screenshot del portale di Azure con la creazione di un pannello della risorsa con l'App per le funzioni evidenziato.](../media/4-click-function-app.png)

1. Immettere un valore univoco a livello globale **nome App**.

1. Selezionare una **sottoscrizione**.

1. Seleziona esistente **gruppo di risorse** <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>.

1. Scegliere **Windows** come **sistema operativo**.

1. Scegliere **Piano A consumo** in **Piano di hosting**. Verranno addebitati costi per ogni esecuzione della funzione. Le risorse verranno allocate automaticamente in base al carico di lavoro dell'applicazione.

1. Selezionare una **località**.

1. Creare un nuovo account di **archiviazione**. Per impostazione predefinita, il nome sarà una variante del nome dell'app, ma è possibile modificarlo.

1. Disattivare **Application Insights**.

1. Selezionare **Crea**. Il completamento richiederà alcuni minuti. Al termine della creazione della risorsa, l'icona **Notifiche** nell'area della barra degli strumenti avrà un pulsante per aprirla nel portale di Azure.

## <a name="create-a-timer-trigger"></a>Creare un trigger timer

A questo punto dobbiamo creare un trigger timer all'interno della funzione.

1. Dopo aver creato la funzione, selezionare **tutte le risorse** dal riquadro di spostamento a sinistra.

1. Individuare e selezionare la funzione.

1. Nel nuovo pannello scegliere **Funzioni** e selezionare l'icona con il segno più (+).

    ![Screenshot del portale di Azure che mostra un pannello App per le funzioni con il pulsante Aggiungi (+) di funzioni sottomenu evidenziato.](../media/4-hover-function.png)

1. Selezionare **Timer**.

1. Selezionare **CSharp** come linguaggio.

1. Selezionare **Creare questa funzione**.

## <a name="configure-the-timer-trigger"></a>Configurare il trigger timer

Abbiamo un'app per le funzioni di Azure con la logica per stampare un messaggio alla finestra del log. Si imposterà la pianificazione del timer per l'esecuzione ogni 20 secondi.

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

Per evitare che vengano addebitati costi per questa funzione, selezionare **Sospendi** sopra la finestra di log per arrestare il timer.

![Screenshot del portale di Azure con i log di un'App per le funzioni Pannello di output con il pulsante di pausa evidenziato.](../media/4-pause-timer.png)
