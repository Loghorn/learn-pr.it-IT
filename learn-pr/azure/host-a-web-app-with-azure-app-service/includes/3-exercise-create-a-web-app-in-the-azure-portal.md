In questa unità si userà il portale di Azure per creare un'app Web.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-web-app"></a>Creare un'app Web

Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Fare clic sul collegamento **Crea una risorsa** nella parte superiore del riquadro di spostamento a sinistra. Tutti gli oggetti creati in Azure sono risorse.

1. Nel portale viene visualizzata la pagina **Marketplace**. Da qui è possibile cercare la risorsa da creare oppure selezionare una delle risorse più comuni create da altri utenti nel portale di Azure.

1. Fare clic su **Web** > **App Web**. Il portale reindirizza alla pagina **Crea una nuova app Web**.

1. Quando si crea una nuova app Web, il portale di Azure richiede alcune informazioni per creare l'app. In questa sezione è necessario specificare le seguenti informazioni di base:

    1. **Nome dell'app**: il cliente vuole assegnare all'applicazione il nome `BestBike`. Digitare il nome in questo campo. Questo valore deve essere globalmente univoco tra tutte le altre app web ospitate in Azure e il portale si assicurerà che nessun altro utente abbia usato il nome dell'app. Per assicurarsi che il nome sia univoco, aggiungere alcuni numeri al nome dell'app finché non si trova una variante univoca.

    2. **Sottoscrizione**: in questo campo è necessario selezionare una sottoscrizione di Azure attiva dall'elenco a discesa. Selezionare la sottoscrizione Concierge.

    3. **Sistema operativo**: in questo campo è necessario decidere se usare **Windows** o **Linux** per ospitare la nuova app Web. Questa impostazione influisce direttamente sul piano di servizio app che verrà selezionato o creato di seguito. Come già visto, un piano di servizio app è simile a una macchina virtuale ovvero un sistema operativo con tutte le risorse (CPU, RAM e così via) necessarie per eseguire l'applicazione su quel computer. In questo caso, il cliente preferisce usare l'app Web in un computer Windows. Quindi, selezionare **Windows**.

    4. **Application Insights**: Azure Application Insights consente di rilevare e diagnosticare i problemi relativi alla qualità nelle app Web e nei servizi Web nonché di capire quali sono le operazioni effettivamente eseguite dagli utenti. Uno dei requisiti del cliente è la possibilità di visualizzare alcuni report con informazioni dettagliate sul traffico in ingresso nel sito Web e di studiare alcune tendenze riguardo all'aumento e alla diminuzione del traffico. In questo caso, selezionare l'opzione **Attivato** per attivare Application Insights per questa app Web. Quando si seleziona l'opzione **Attivato** viene anche richiesto di selezionare la località o l'area in cui verranno archiviati i dati di Application Insights. Si noti che Application Insights è disponibile solo in un numero limitato di aree. Per questa demo, selezionare una delle aree disponibili.

## <a name="use-the-sandbox-resource-group"></a>Usare il gruppo di risorse sandbox

Un'app Web di Azure deve far parte di un gruppo di risorse. Selezionare **Usa esistente** e scegliere <rgn>[Nome gruppo di risorse sandbox]</rgn>.

## <a name="create-an-app-service-plan"></a>Creare un piano di servizio app

In questo campo è necessario selezionare un piano di servizio app per eseguire l'applicazione. Per impostazione predefinita, il portale seleziona il piano di servizio app più recente creato. Fare clic sul campo **Piano di servizio app/Località** per passare alla pagina **Piano di servizio app**.

Fare clic sul collegamento **Crea nuovo** per passare alla pagina **Nuovo piano di servizio app**. Il portale richiede all'utente di immettere alcune informazioni per creare il nuovo piano di servizio app.

1. **Piano di servizio app**: in questo campo specificare un nome per il nuovo piano di servizio app. Per questa app digitare lo stesso nome dell'app Web scelto in precedenza e aggiungere il suffisso `-app-service-plan` per distinguere facilmente questa risorsa dalle altre.

2. **Località**: in questo campo è necessario selezionare l'area in cui si trova questo piano di servizio app. In altre parole, selezionare la località geografica in cui il piano di servizio app configurerà le macchine virtuali necessarie per eseguire l'applicazione. In questo caso è possibile selezionare una delle opzioni dell'elenco seguente.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

3. **Piano tariffario**: in questo campo è necessario selezionare le dimensioni della macchina virtuale che ospiterà l'applicazione. Fare clic su **>** per passare alla pagina **Piano tariffario**.

    È possibile scegliere tra diverse opzioni. Il portale raggruppa tali opzioni in base al livello del carico di lavoro necessario. Le tre categorie di carico di lavoro disponibili sono Sviluppo/test, Produzione e Isolato. A seconda dei requisiti dell'applicazione da ospitare in Azure, si selezionerà la categoria di carico di lavoro pertinente. Poiché l'applicazione **BestBike** è in fase di compilazione e perfezionamento, si inizia con la categoria minima di carico di lavoro più adatta. Tenere presente che uno dei requisiti del cliente è la possibilità di testare in tempo reale ogni nuova modifica apportata all'applicazione. Nelle unità successive si noterà che per soddisfare questo requisito è necessario aggiungere **slot di distribuzione**. Gli slot di distribuzione sono disponibili a partire da un piano tariffario minimo di **S1**. Selezionare quindi il piano tariffario **S1** nella categoria **Carico di lavoro di produzione**. Fare clic su **Applica** per confermare il piano tariffario selezionato in precedenza.

    > [!NOTE]
    > Si noterà in tutto il modulo che solo le categorie di carico di lavoro **Produzione** e **Isolato** consentono di aggiungere **slot di distribuzione** all'app Web.

    A questo punto, si torna alla pagina **Nuovo piano di servizio app**.

    ![Screenshot che mostra la pagina Nuovo piano servizio app con i valori di esempio per questo esercizio nelle impostazioni](../media/3-new-app-service-plan.PNG)

4. Fare clic sul pulsante **OK** per usare il nuovo piano di servizio app.

    Nel portale viene di nuovo visualizzata la pagina **Crea app Web**.

    ![Screenshot che mostra la pagina della nuova risorsa in Azure con l'avanzamento per trovare la risorsa app Web evidenziata.](../media/3-new-web-app.png)

5. Fare clic sul pulsante **Crea** per avviare il processo di creazione dell'app Web.

    > [!NOTE]
    > La creazione dell'app Web e la relativa preparazione all'uso possono richiedere alcuni secondi.

Il portale reindirizza alla pagina Dashboard e invia una notifica quando la creazione dell'app Web è completata.

Quando l'app è pronta, passare alla nuova app nel portale di Azure.

1. Fare clic sul menu **Tutte le risorse** nel riquadro di spostamento a sinistra. Nella pagina **Tutte le risorse** sono elencate tutte le risorse create nel portale di Azure.

2. Esplorare il servizio app BestBike appena creato.

    > [!NOTE]
    > Se si esegue una ricerca dell'app in base al nome "BestBike", è possibile trovare anche le risorse di Application Insights e del piano di servizio app create per la nuova app Web. Assicurarsi di esplorare la risorsa con il tipo **Servizio app**.

    ![Screenshot che mostra un risultato di ricerca di esempio all'interno della pagina Tutte le risorse con il servizio app BestBike123 appena creato evidenziato.](../media/3-web-app.PNG)

Il portale apre la home page del servizio app Web con la sezione **Panoramica** selezionata.

![Screenshot che mostra la pagina del servizio app BestBike con il collegamento all'URL della sezione Panoramica evidenziata.](../media/3-web-app-home.PNG)

Per visualizzare in anteprima il contenuto predefinito della nuova app Web, fare clic sull'**URL** in alto a destra nel portale di Azure. Se viene visualizzata una pagina Web segnaposto, significa che l'app Web è stata creata correttamente.
