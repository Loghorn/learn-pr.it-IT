In questa unità viene creata un'app per le funzioni di Azure che verrà richiamata ogni 20 secondi con un trigger timer.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-function-app"></a>Creare un'app per le funzioni di Azure

Per iniziare, creare un'app per le Funzioni di Azure nel portale.

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Nel riquadro di spostamento a sinistra selezionare **Crea una risorsa**.

1. Selezionare **Calcolo**.

1. Individuare e selezionare **App per le funzioni**. Facoltativamente, è anche possibile usare la barra di ricerca per individuare il modello.

    ![Screenshot del portale di Azure che illustra il pannello Crea una risorsa con App per le funzioni evidenziato.](../media/4-click-function-app.png)

1. Scegliere un **Nome dell'app** globalmente univoco.

1. Selezionare una **Sottoscrizione**.

1. Selezionare il **Gruppo di risorse** esistente <rgn>[nome gruppo di risorse sandbox]</rgn>.

1. Scegliere **Windows** come **sistema operativo**.

1. Scegliere **Piano A consumo** in **Piano di hosting**. Verranno addebitati costi per ogni esecuzione della funzione. Le risorse verranno allocate automaticamente in base al carico di lavoro dell'applicazione.

1. Selezionare una **Posizione** dall'elenco disponibile di seguito.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. Per **Stack di runtime**, lasciare il valore predefinito *.NET*, che identifica il linguaggio in cui si implementano gli esempi di funzione in questo esercizio.

1. Creare un nuovo account di **Archiviazione**. Per impostazione predefinita, il nome sarà una variante del nome dell'app, ma è possibile modificarlo.

1. Selezionare **Crea**. Al completamento della distribuzione dell'app per le funzioni, accedere al portale e selezionare **Tutte le risorse**. L'app per le funzioni sarà inclusa nell'elenco con il tipo **Servizio app** e il nome che le è stato assegnato.
 
<!-- Start temporary fix for issue #2498. -->
> [!IMPORTANT]
> Gli esercizi in questo modulo attualmente funzionano con Funzioni di Azure V1. Seguire questa procedura con attenzione per assicurarsi che l'app per le funzioni usi la versione del runtime V1. 

1. Una volta creata l'app per le funzioni, selezionare **Tutte le risorse** nel riquadro di spostamento a sinistra.

1. Selezionare l'app per le funzioni nell'elenco **App per le funzioni**.
1. Selezionare **Funzionalità della piattaforma**.
1. Nella schermata **Funzionalità della piattaforma** selezionare **Impostazioni dell'app per le funzioni** in **Impostazioni generali**.
1. Selezionare *~1* in **Versione runtime**.
1. Chiudere **Impostazioni dell'app per le funzioni**.

L'app per le funzioni è ora configurata per usare il runtime V1 di Funzioni di Azure. È ora possibile continuare a creare la prima funzione.
<!-- End temporary fix for issue #2498. --> 

## <a name="create-a-timer-trigger"></a>Creare un trigger timer

Verrà ora creato un trigger timer all'interno della funzione.



1. Nel nuovo pannello scegliere **Funzioni** e selezionare l'icona con il segno più (+).

    ![Screenshot del portale di Azure che illustra un pannello App per le funzioni con il pulsante (+) del sottomenu Funzioni evidenziato.](../media/4-hover-function.png)

1. Selezionare **Timer**.

1. Selezionare **Creare questa funzione**.

## <a name="configure-the-timer-trigger"></a>Configurare il trigger timer

Si ha un'app per le funzioni di Azure la cui logica stampa un messaggio nella finestra di log. Ora verrà impostata la pianificazione del timer per l'esecuzione ogni 20 secondi.

1. Selezionare **Integrazione**.

1. Immettere il valore seguente nella casella **Pianifica**:

    ```log
    */20 * * * * *
    ```

1. Selezionare **Salva**.

## <a name="test-the-timer"></a>Testare il timer

Ora che è stato configurato, il timer richiama la funzione nell'intervallo definito.

1. Selezionare **TimerTriggerCSharp1**.

    > [!NOTE]
    > **TimerTriggerCSharp1** è un nome predefinito. Viene selezionato automaticamente quando si crea il trigger.

1. Aprire il pannello **Log** nella parte inferiore della schermata.

1. Osservare che i nuovi messaggi arrivano ogni 20 secondi nella finestra del log.

1. Per interrompere l'esecuzione della funzione, selezionare **Gestisci** e quindi impostare **Stato funzione** su *Disabilitato*.
