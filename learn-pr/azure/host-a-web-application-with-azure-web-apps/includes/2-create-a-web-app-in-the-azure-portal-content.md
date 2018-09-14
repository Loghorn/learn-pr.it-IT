In questo modulo si apprenderà come creare un'app Web di Azure usando il portale di Azure.

## <a name="why-use-the-azure-portal"></a>Perché usare il portale di Azure?

Il primo passaggio per ospitare un'applicazione Web consiste nel creare un'**app Web** all'interno della sottoscrizione di Azure.

Ci sono diversi modi per creare un'app Web di Azure. È possibile usare il portale di Azure, l'interfaccia della riga di comando di Azure, uno script o un ambiente di sviluppo integrato (IDE).

In questo caso verrà usato il portale, che offre un'esperienza grafica ideale per l'apprendimento. Il portale consente di individuare le funzionalità disponibili, aggiungere altre risorse e personalizzare le risorse esistenti.

## <a name="what-is-web-apps-in-azure"></a>Che cos'è App Web di Azure?

App Web è una piattaforma di calcolo completamente gestita all'interno dell'ambiente Azure, ottimizzata per l'hosting di app Web, API REST e back-end per dispositivi mobili.

Questa piattaforma distribuita come servizio (PaaS, Platform as a Service), offerta da Microsoft Azure, consente di concentrarsi sulle attività di compilazione, mentre Azure gestisce l'infrastruttura per l'esecuzione e il ridimensionamento delle applicazioni.

## <a name="how-to-create-an-azure-web-app"></a>Come creare un'app Web di Azure

Quando è il momento di gestire l'hosting della propria app, visitare il portale di Azure e creare una nuova **risorsa** di tipo **App Web**. Creando tale risorsa nel portale di Azure, in realtà si crea un **contenitore** o un progetto che è possibile usare per l'hosting di qualsiasi applicazione basata su Web che sia supportata da Azure, ad esempio ASP.NET Core, Node.js, PHP e così via. La figura seguente dimostra quanto sia facile configurare il linguaggio o il framework usato dall'app.

![Impostazioni dell'app Web](../media-draft/2-web-app-settings.png)

Il portale di Azure offre un modello per creare una nuova app Web. Il modello richiede i campi seguenti:

- **Nome app**: il nome dell'app Web.
- **Sottoscrizione**: una sottoscrizione valida e attiva.
- **Gruppo di risorse**: un gruppo di risorse valido. Le sezioni seguenti spiegano in dettaglio che cos'è un gruppo di risorse.
- **Sistema operativo**: il sistema operativo. Le opzioni sono: Windows, Linux e contenitori Docker. In Windows è possibile ospitare qualsiasi tipo di applicazione con un'ampia gamma di tecnologie. Lo stesso vale per l'hosting in Linux, ad eccezione del fatto che è possibile creare solo app Web ASP.NET Core che usano il framework .NET Core in Linux. Per altre app ASP.NET che usano la versione completa di .NET Framework, l'hosting deve essere eseguito in un sistema operativo Windows. L'opzione finale è costituita dai contenitori Docker e consente di distribuire i contenitori Docker locali direttamente in contenitori ospitati e gestiti da Azure. Quindi, in pratica, qualsiasi app Web che usa una tecnologia open source (PHP, ASP.NET Core e così via) può essere ospitata in un sistema operativo Linux.
- **Piano di servizio app/Località**: un piano di servizio app di Azure valido. Le sezioni seguenti spiegano in dettaglio che cos'è un piano di servizio app.
- **Application Insights**: è possibile attivare l'opzione Azure Application Insights e usufruire degli strumenti per il monitoraggio e le metriche offerti dal portale di Azure per tenere sotto controllo le prestazioni delle app.

Il portale di Azure offre un supporto ottimale in termini di gestione, monitoraggio e controllo dell'app Web attraverso i numerosi strumenti disponibili.

### <a name="deployment-slots"></a>Slot di distribuzione

Nel portale di Azure è possibile aggiungere facilmente **slot di distribuzione** a un'app Web. È ad esempio possibile creare uno slot di distribuzione di **staging** in cui inserire il codice da testare in Azure. Quando si è soddisfatti del codice, è possibile **scambiare** facilmente lo slot di distribuzione di staging con quello di produzione. Tutte queste operazioni si eseguono con pochi clic del mouse nel portale di Azure.

![Slot di distribuzione](../media-draft/2-deployment-slots.png)

### <a name="continuous-integrationdeployment-support"></a>Supporto di integrazione e distribuzione continue

Il portale di Azure offre opzioni predefinite di integrazione e distribuzione continue con Visual Studio Team Services, GitHub, Bitbucket, Dropbox, OneDrive o un repository Git locale gestito completamente da Azure. Se si connette l'app Web a una qualsiasi delle origini precedenti, Azure eseguirà le altre operazioni per l'utente, sincronizzando automaticamente il codice ed eventuali modifiche apportate in futuro al codice nell'app Web. Inoltre, con Visual Studio Team Services, è possibile definire un processo personalizzato di compilazione e rilascio che consente di compilare il codice sorgente, eseguire i test, creare una versione e infine eseguire il push della versione in un'app Web ogni volta che si esegue il commit del codice. Tutto ciò che avviene in modo implicito senza alcun intervento da parte dell'utente.

![Configurare l'integrazione continua](../media-draft/2-continuous-integration.PNG)

### <a name="integrated-visual-studio-publishing-and-ftp-publishing"></a>Pubblicazione di Visual Studio e pubblicazione FTP integrate

Oltre a configurare l'integrazione e la distribuzione continue per l'app Web, è sempre possibile usufruire della stretta integrazione con Visual Studio per pubblicare l'app Web in Azure usando la tecnologia Distribuzione Web. Azure supporta inoltre il protocollo FTP, anche se è preferibile non usare FTP per la pubblicazione perché mancano alcune funzionalità in Distribuzione Web per l'individuazione e la scelta solo dei file che sono stati modificati o aggiunti, invece di pubblicare tutto in Azure.

### <a name="built-in-auto-scale-support-automatically-scale-updown-based-on-real-world-load"></a>Supporto integrato per la scalabilità automatica (aumento e riduzione automatici delle prestazioni in base al carico di lavoro reale)

L'app Web offre capacità di scalabilità verticale e orizzontale. In base all'uso dell'app Web, è possibile aumentare o diminuire le prestazioni dell'app aumentando o diminuendo le risorse del computer sottostante che ospita l'app Web. Le risorse possono essere rappresentate dal numero di core o dalla quantità di RAM disponibile.

La scalabilità orizzontale è invece la possibilità di aumentare il numero di istanze di computer che eseguono l'app Web.

## <a name="what-is-a-resource-group"></a>Che cos'è un gruppo di risorse?

Un gruppo di risorse è un metodo di raggruppamento di risorse e servizi interdipendenti, ad esempio macchine virtuali, app Web, database e altro ancora, per un'applicazione e un ambiente specifici. Può essere considerato come una **cartella** o una posizione in cui raggruppare gli elementi dell'app.

I gruppi di risorse consentono di gestire ed eliminare le risorse con facilità. Consentono inoltre il monitoraggio, il controllo dell'accesso, il provisioning e la gestione della fatturazione per le raccolte di risorse necessarie per eseguire un'applicazione o usate da un client.

## <a name="what-is-an-app-service-plan"></a>Che cos'è un piano di servizio app?

Un piano di servizio app è un set di risorse fisiche e capacità per la distribuzione delle app del servizio app.

Il portale di Azure offre un modello per creare un nuovo piano di servizio app. Il modello richiede le seguenti informazioni di base:

- Area (Stati Uniti occidentali, Stati Uniti centrali, Europa settentrionale e così via)
- Numero di scala (una, due, tre istanze e così via)
- Dimensione dell'istanza (Small, Medium o Large)
- Codice di riferimento del prodotto - SKU (Gratuito, Condiviso, Basic, Standard, Premium e, più di recente, Premium v2)

Le funzionalità App Web, App per dispositivi mobili e App per le API di Servizio app di Azure e Funzioni di Azure vengono tutte eseguite in un piano di servizio app. Sebbene sia possibile distribuire un numero illimitato di applicazioni in un piano di servizio app, il numero varia notevolmente in base ai tipi di applicazioni distribuiti e alle relative risorse necessarie in termini di utilizzo della CPU.

È sempre possibile usare il piano di servizio app nel portale di Azure per visualizzare l'utilizzo della CPU e della memoria e determinare se è necessario ridimensionare o spostare le applicazioni in un altro piano di servizio app.
