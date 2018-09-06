In questo esercizio si crea una funzione di Azure che verrà richiamata ogni 20 secondi con un trigger timer.

> [!NOTE] 
> Per completare l'esercizio, assicurarsi di aver eseguito l'accesso al [portale di Azure](https://portal.azure.com?azure-portal=true) con un account valido.

## <a name="create-an-azure-function"></a>Creare una funzione di Azure

Per iniziare, creare una funzione di Azure nel portale.

1. Nel riquadro di spostamento a sinistra selezionare **Crea una risorsa**.

2. Selezionare **Calcolo**.

3. Individuare e selezionare **App per le funzioni**. Facoltativamente, è anche possibile usare la barra di ricerca per individuare il modello.

    ![Selezionare App per le funzioni](../media-drafts/4-click-function-app.png)

4. Immettere un **nome dell'app** univoco.

5. Selezionare una **sottoscrizione**.

6. Creare un nuovo **gruppo di risorse**.

7. Scegliere **Windows** come **sistema operativo**.

8. Scegliere **Piano A consumo** in **Piano di hosting**. Verranno addebitati costi per ogni esecuzione della funzione. Le risorse verranno allocate automaticamente in base al carico di lavoro dell'applicazione.

9. Selezionare una **località**.

10. Creare un nuovo account di **archiviazione**. Per impostazione predefinita, il nome sarà una variante del nome dell'app, ma è possibile modificarlo.

11. Disattivare **Application Insights**.

12. Selezionare **Crea**. Il completamento richiederà alcuni minuti. Al termine della creazione della risorsa, l'icona **Notifiche** nell'area della barra degli strumenti avrà un pulsante per aprirla nel portale di Azure.

## <a name="create-a-timer-trigger"></a>Creare un trigger timer

Verrà ora creato un trigger timer all'interno della funzione di Azure.

1. Al termine della creazione della funzione di Azure, selezionare **Tutte le risorse** nel riquadro di spostamento a sinistra.

2. Individuare e selezionare la funzione di Azure.

3. Nel nuovo pannello scegliere **Funzioni** e selezionare l'icona con il segno più (+).

    ![Selezionare Funzioni, quindi il segno più](../media-drafts/4-hover-function.png)

4. Selezionare **Timer**.

5. Selezionare **CSharp** come linguaggio.

6. Selezionare **Creare questa funzione**.

## <a name="configure-the-timer-trigger"></a>Configurare il trigger timer

Si ha una funzione di Azure la cui logica stampa un messaggio nella finestra di log. Si imposterà la pianificazione del timer per l'esecuzione ogni 20 secondi.

1. Selezionare **Integrazione**.

2. Immettere il valore seguente nella casella **Pianifica**:

    ```
    */20 * * * * *
    ```

3. Selezionare **Salva**.

## <a name="start-the-timer"></a>Avviare il timer

Dopo aver configurato il timer, è possibile avviarlo.

1. Selezionare **TimerTriggerCSharp1**. 

    > [!NOTE]
    > **TimerTriggerCSharp1** è un nome predefinito. Viene selezionato automaticamente quando si crea il trigger.

2. Selezionare **Esegui**. 

A questo punto, nella finestra di log dovrebbe essere visualizzato un messaggio ogni 20 secondi.

## <a name="clean-up"></a>Eseguire la pulizia

Per evitare che vengano addebitati costi per questa funzione, selezionare **Sospendi** sopra la finestra di log per arrestare il timer.

![Sospensione](../media-drafts/4-pause-timer.png)


