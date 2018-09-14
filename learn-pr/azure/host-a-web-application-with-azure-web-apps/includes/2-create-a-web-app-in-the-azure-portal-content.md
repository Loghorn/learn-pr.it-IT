In questo caso, si apprenderà come creare un'app Web di Azure usando il portale di Azure.

## <a name="why-use-the-azure-portal"></a>Perché usare il portale di Azure?

Il primo passaggio della procedura di hosting di un'applicazione Web è creare un'**app Web** all'interno della sottoscrizione di Azure.

Esistono diversi modi per creare un'app Web di Azure. Si può usare il portale di Azure, l'interfaccia della riga di comando di Azure, uno script o un ambiente di sviluppo integrato.

In questo caso verrà usato il portale poiché è un'esperienza grafica e per questo è un ottimo strumento per l'apprendimento. Il portale consente di individuare le funzionalità disponibili, aggiungere ulteriori risorse e personalizzare le risorse esistenti.

## <a name="what-is-web-apps-in-azure"></a>Che cos'è App Web di Azure?

App Web è una piattaforma di calcolo completamente gestita all'interno dell'ambiente Azure, ottimizzata per l'hosting di app Web, API REST e back-end per dispositivi mobili.

Questa piattaforma distribuita come servizio (PaaS, Platform as a Service), offerta da Microsoft Azure, consente di concentrarsi sulle attività di compilazione, mentre Azure gestisce l'infrastruttura per l'esecuzione e il ridimensionamento delle applicazioni.

## <a name="how-to-create-an-azure-web-app"></a>Come creare un'app Web di Azure

Quando è il momento di gestire l'hosting della propria app, visitare il portale di Azure e creare una nuova **risorsa** di tipo **App Web**. Creando tale risorsa nel portale di Azure, in realtà si crea un **contenitore** o un progetto che è possibile usare per l'hosting di qualsiasi applicazione basata su Web che sia supportata da Azure, ad esempio ASP.NET Core, Node.js, PHP e così via. La figura seguente dimostra quanto sia facile configurare il linguaggio o framework usato dall'app.

![Impostazioni app Web](../media-draft/2-web-app-settings.png)

Il portale di Azure offre un modello per creare una nuova app Web. Il modello richiede i seguenti campi:

- **Nome app** : il nome dell'app Web.
- **Sottoscrizione**: una sottoscrizione valida e attiva.
- **Gruppo di risorse**: un gruppo di risorse valido. Le sezioni seguenti spiegano in dettaglio che cos'è un gruppo di risorse.
- **Sistema operativo**: il sistema operativo. Le opzioni sono: Windows, Linux e contenitori Docker. In Windows è possibile ospitare qualsiasi tipo di applicazione da un'ampia gamma di tecnologie. Lo stesso vale per l'hosting in Linux, ad eccezione del fatto che in ASP.NET Core è possibile creare solo app Web che usano il framework .NET Core per Linux. Per altre app ASP.NET che usano la versione completa di .NET Framework, l'hosting deve essere eseguito in un sistema operativo Windows. L'opzione finale sono i contenitori Docker, in cui è possibile distribuire i contenitori Docker locali usando direttamente i contenitori ospitati e gestiti da Azure. Quindi, in pratica qualsiasi app Web che usa una tecnologia open source (PHP, ASP.NET Core, e così via) può essere ospitata in un sistema operativo Linux.
- **Piano di servizio app/Località**: un piano di servizio app di Azure valido. Le sezioni seguenti spiegano in dettaglio che cos'è un piano di servizio app.
- **Application Insights**: è possibile attivare l'opzione Azure Application Insights e usufruire degli strumenti per il monitoraggio e le metriche offerti dal portale di Azure per tenere sotto controllo le prestazioni delle app.

Il portale di Azure offre un supporto ottimale in termini di gestione, monitoraggio e controllo dell'app Web attraverso i numerosi strumenti disponibili.

### <a name="deployment-slots"></a>Slot di distribuzione

Nel portale di Azure è possibile aggiungere facilmente **slot di distribuzione** a un'app Web. Ad esempio, è possibile creare uno slot di distribuzione di **staging** in cui inserire il codice da testare in Azure. Quando si è soddisfatti del codice, è possibile **sostituire** facilmente lo slot di produzione allo slot di distribuzione di staging. Tutte queste operazioni si eseguono con pochi clic del mouse nel portale di Azure.

![Slot di distribuzione](../media-draft/2-deployment-slots.png)

### <a name="continuous-integrationdeployment-support"></a>Supporto a integrazione e distribuzione continue

Il portale di Azure offre opzioni di integrazione e distribuzione continue pronte per l'uso con Visual Studio Team Services, GitHub, Bitbucket, Dropbox, OneDrive o un repository Git locale gestito completamente da Azure. Se si connette l'app Web a una qualsiasi delle origini precedenti, Azure eseguirà le altre operazioni per l'utente, sincronizzando automaticamente il codice ed eventuali modifiche apportate in futuro al codice nell'app Web. Inoltre, con Visual Studio Team Services, è possibile definire il proprio processo di build e rilascio che completa la compilazione del codice sorgente, con l'esecuzione dei test, la creazione della versione e infine il push della versione in un'app Web ogni volta che si esegue il commit del codice. Tutto ciò che avviene in modo implicito senza alcun intervento da parte dell'utente.

![Configurare l'integrazione continua](../media-draft/2-continuous-integration.PNG)

### <a name="integrated-visual-studio-publishing-and-ftp-publishing"></a>Pubblicazione di Visual Studio e pubblicazione FTP integrate

Oltre ad avere la possibilità di configurare l'integrazione/distribuzione continua per l'app Web, si può sempre usufruire della stretta integrazione con Visual Studio per pubblicare l'app Web in Azure usando la tecnologia Distribuzione Web. Inoltre, Azure supporta il protocollo FTP, anche se è preferibile non usare FTP per la pubblicazione perché in Distribuzione Web non è in grado di individuare e scegliere solo i file che sono stati modificati o aggiunti e pubblica tutto in Azure.

### <a name="built-in-auto-scale-support-automatically-scale-updown-based-on-real-world-load"></a>Supporto integrato per la scalabilità automatica (aumento e riduzione automatici delle prestazioni in base al carico di lavoro reale)

L'app Web comprende la possibilità di aumentare/ridurre le prestazioni e scalare orizzontalmente. In base all'uso dell'app Web, è possibile aumentare o diminuire le prestazioni dell'app aumentando o diminuendo le risorse del computer sottostante che ospita l'app Web. Risorse può significare il numero di core o la quantità di RAM disponibile.

La scalabilità orizzontale è invece la possibilità di aumentare il numero di istanze di computer che eseguono l'app Web.

## <a name="what-is-a-resource-group"></a>Informazioni sui gruppi di risorse

Un gruppo di risorse è un metodo di raggruppamento di risorse e servizi interdipendenti, ad esempio macchine virtuali, app Web, database e altro ancora, per un'applicazione e un ambiente specifici. Può essere considerato come una **cartella**, un posto in cui raggruppare gli elementi dell'app.

I gruppi di risorse consentono di gestire ed eliminare le risorse con facilità. Consentono inoltre di monitorare, controllare l'accesso, eseguire il provisioning e gestire la fatturazione per le raccolte di risorse necessarie per eseguire un'applicazione o richieste da un client.

## <a name="what-is-an-app-service-plan"></a>Informazioni sui piani di servizio app

Un piano di servizio app è un set di risorse fisiche e capacità disponibile per la distribuzione delle app del servizio app.

Il portale di Azure offre un modello per creare un nuovo piano di servizio app. Il modello richiede le seguenti informazioni di base:

- Area (Stati Uniti occidentali, Stati Uniti centrali, Europa settentrionale e così via)
- Numero di scala (una, due, tre istanze e così via)
- Dimensione dell'istanza (Small, Medium o Large)
- Codice di riferimento del prodotto - SKU (Gratuito, Condiviso, Basic, Standard, Premium e più di recente, Premium v2)

Le app Web, le app per dispositivi mobili, le funzionalità delle app per le API del Servizio app di Azure e Funzioni di Azure vengono tutte eseguite in un piano di servizio app. Sebbene sia possibile distribuire un numero illimitato di applicazioni in un piano di servizio app, il numero varia notevolmente in base ai tipi di applicazioni distribuiti e alle relative risorse necessarie in termini di utilizzo della CPU.

È sempre possibile usare il piano di servizio app nel portale di Azure per visualizzare l'utilizzo della CPU e della memoria e determinare se è necessario ridimensionare o spostare le applicazioni in un altro piano di servizio app.
