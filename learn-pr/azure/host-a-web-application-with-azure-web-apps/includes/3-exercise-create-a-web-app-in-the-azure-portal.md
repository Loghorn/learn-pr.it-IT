In questo esercizio si userà il portale di Azure per creare un'app Web di Azure.

## <a name="log-in-to-the-azure-portal"></a>Accedere al portale di Azure

Il primo passaggio è l'accesso al portale di Azure:

1. Aprire un browser e passare al [portale di Azure](https://portal.azure.com).
2. Accedere con le proprie credenziali.
3. Dopo l'accesso, si arriva al **Dashboard** o alla home page del portale di Azure.

    ![Portale di Azure](../media-draft/3-azure-portal.PNG)

## <a name="create-an-azure-web-app"></a>Creare un'app Web di Azure

Dopo aver effettuato l'accesso al portale di Azure, è possibile creare un'app Web di Azure usando il modello.

1. Individuare e selezionare il collegamento **+ Crea una risorsa** in alto a sinistra nella schermata. Tutti gli oggetti creati in Azure sono risorse.

    ![Azure Marketplace](../media-draft/3-market-place.PNG)

1. Nel portale viene visualizzato il pannello **Marketplace**. Da qui è possibile cercare la risorsa da creare oppure selezionare una delle risorse più comuni create da altri utenti nel portale di Azure.

1. Individuare e scegliere la risorsa **App Web** nella sezione delle risorse **più diffuse**. Il portale reindirizza al pannello **Crea una nuova app Web**.

    ![Pannello Crea una nuova app Web](../media-draft/3-create-new-web-app-page.PNG)

1. Quando si crea una nuova app Web il portale di Azure richiede alcune informazioni per creare l'app. In questa sezione è necessario specificare le seguenti informazioni di base:

    1. **Nome dell'app**: il cliente vuole assegnare all'applicazione il nome `BestBike`. Digitare il nome in questo campo. Il portale di Azure controlla e convalida la disponibilità del nome dell'app Web tra tutte le app Web ospitate in Azure per verificare che nessun utente abbia usato il nome `BestBike`.

    2. **Sottoscrizione**: in questo campo è necessario selezionare una sottoscrizione di Azure attiva dall'elenco a discesa.

    3. **Sistema operativo**: in questo campo è necessario decidere se usare **Windows** o **Linux** per ospitare la nuova app Web. Questa impostazione influisce direttamente sul piano di servizio app che verrà selezionato o creato di seguito. Come già visto, un piano di servizio app è simile a una macchina virtuale ovvero un sistema operativo con tutte le risorse (CPU, RAM e così via) necessarie per eseguire l'applicazione su quel computer. In questo caso, il cliente preferisce usare l'app Web in un computer Windows. Quindi, selezionare **Windows**.

    4. **Application Insights**: Azure Application Insights consente di rilevare e diagnosticare i problemi relativi alla qualità nelle app Web e nei servizi Web nonché di capire quali sono le operazioni effettivamente eseguite dagli utenti. Uno dei requisiti del cliente è la possibilità di visualizzare report con informazioni dettagliate sul traffico in ingresso nel suo sito Web e di studiare alcune tendenze riguardo all'aumento e alla diminuzione del traffico. In questo caso, selezionare l'opzione **Attivato** per attivare Application Insights per questa app Web. Quando si seleziona l'opzione **Attivato** viene anche richiesto di selezionare la località o l'area in cui verranno archiviati i dati di Application Insights. Si noti che Application Insights è disponibile solo in un numero limitato di aree. Per questa demo, selezionare l'area **East US** nel campo **Località di Application Insights**.

## <a name="create-a-new-resource-group"></a>Creare un nuovo gruppo di risorse

Un'app Web di Azure deve far parte di un gruppo di risorse. Ora ne verrà creato uno per raggruppare le risorse correlate.

Selezionare l'opzione **Crea nuovo**, che è selezionata per impostazione predefinita. Si noti che Azure ha già generato un nome univoco per il gruppo di risorse in base al **nome dell'app** che si è scelto. Si è liberi di modificare il nome del gruppo di risorse o mantenere quello generato da Azure. In questo caso, verrà mantenuto il nome generato da Azure.

## <a name="create-an-app-service-plan"></a>Creare un piano di servizio app

In questo campo è necessario selezionare un piano di servizio app per eseguire l'applicazione. Per impostazione predefinita, il portale seleziona l'ultimo piano di servizio app creato in precedenza. Fare clic su **>** per passare al pannello **Piano di servizio app**.

![Pannello Piano di servizio app](../media-draft/3-create-app-service-plan.PNG)

Individuare e fare clic sul collegamento **+ Crea nuovo** per creare un nuovo piano di servizio app. Nel portale viene visualizzato il pannello **Nuovo piano di servizio app**. Il portale richiede alcune informazioni da parte dell'utente per creare il nuovo piano di servizio app.

![Pannello Nuovo piano di servizio app](../media-draft/3-new-app-service-plan.PNG)

1. **Piano di servizio app**: in questo campo si specifica un nome univoco per il nuovo piano di servizio app. Digitare lo stesso nome di app Web scelto in precedenza e aggiungere un suffisso di `-app-service-plan` per distinguere facilmente questa risorsa alle altre. Il portale di Azure controlla e convalida di nuovo la disponibilità del nome del piano di servizio app tra tutti i piani di servizio app ospitati in Azure.

2. **Località**: in questo campo è necessario selezionare l'**area** in cui si trova questo piano di servizio app. In altre parole, selezionare la località geografica in cui il piano di servizio app imposterà le macchine virtuali necessarie per eseguire l'applicazione. In questo caso è possibile selezionare una delle opzioni dell'elenco. Il cliente preferisce avere i server di hosting in una località vicina alla costa orientale, dove risiede la maggior parte dei suoi clienti. Di conseguenza, si selezionerà il valore **East US**.

3. **Piano tariffario**: in questo campo è necessario selezionare le dimensioni della macchina virtuale che ospiterà l'applicazione. Fare clic su **>** per passare al pannello **Piano tariffario**.

    ![Pannello Piano tariffario](../media-draft/3-pricing-tier-blade.PNG)

    È possibile scegliere tra diverse opzioni. Il portale raggruppa tali opzioni in base al livello del carico di lavoro necessario. Le tre categorie di carico di lavoro disponibili sono Sviluppo/test, Produzione e Isolato. A seconda dei requisiti dell'applicazione da ospitare in Azure, si selezionerà la categoria di carico di lavoro pertinente. Poiché l'applicazione `BestBike` è in fase di compilazione e perfezionamento, si inizia con la categoria minima di carico di lavoro più adatta. Tenere presente che uno dei requisiti del cliente è la possibilità di testare in tempo reale ogni nuova modifica apportata all'applicazione. Nelle unità successive si noterà che per soddisfare questo requisito è necessario aggiungere **slot di distribuzione**. Gli slot di distribuzione sono disponibili solo per un piano tariffario minimo di **S1**. Selezionare quindi il piano tariffario **S1** nella categoria **Carico di lavoro di produzione**. Fare clic su **Applica** per confermare il piano tariffario selezionato in precedenza.

    > Si noterà in tutto il modulo che solo le categorie di carico di lavoro **Produzione** e **Isolato** consentono di aggiungere **slot di distribuzione** all'app Web.

A questo punto, si torna al pannello **Nuovo piano di servizio app**.

Fare clic su **OK**.

Nel portale viene di nuovo visualizzato il pannello **Crea una nuova app Web**.

![Crea una nuova app Web con i dati](../media-draft/3-new-web-app-with-data.PNG)

Fare clic sul pulsante **Crea** per avviare il processo di creazione dell'app Web.

> In genere, il portale di Azure impiega pochi secondi a creare l'app Web e a predisporla per l'uso.

Il portale reindirizza alla pagina Dashboard e invia una notifica quando la creazione dell'app Web è completata.

Quando l'app è pronta, individuare e selezionare il menu **Tutte le risorse** sul lato sinistro del dashboard. Nel pannello **Tutte le risorse** sono elencate tutte le risorse create dall'utente nel portale di Azure.

Individuare e scegliere l'app Web appena creata. Nel portale viene visualizzato il pannello dell'app Web.

![app bilal-webapp](../media-draft/3-web-app.PNG)

Il portale apre la home page dell'app Web.

![home page di bilal-webapp](../media-draft/3-web-app-home.PNG)

Individuare e scegliere l'**URL** in alto a destra nel portale di Azure. Se viene visualizzata una pagina simile a quella riportata sotto, significa che l'app Web è stata creata correttamente.

![Home page predefinita dell'app Web](../media-draft/3-default-home-page.PNG)
