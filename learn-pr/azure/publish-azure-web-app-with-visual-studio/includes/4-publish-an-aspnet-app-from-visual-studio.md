È stato creato un sito che ora si desidera distribuire in Azure. Dobbiamo ora considerare quali servizi di Azure sfruttare per soddisfare al meglio le esigenze specifiche. Servizio app offre alle applicazioni un servizio di hosting Web ad alta scalabilità e con applicazione automatica delle patch.

In questo caso, verrà illustrato come usare Visual Studio per pubblicare l'applicazione Web di ASP.NET Core in un piano di Servizio app di Azure.

## <a name="azure-subscription"></a>Sottoscrizione di Azure

Per pubblicare in Azure, è necessario avere una sottoscrizione di Azure. È possibile usare una [sottoscrizione gratuita di Azure](https://azure.microsoft.com/free/) per testare le funzionalità di Servizio app di Azure.

## <a name="what-is-azure-app-service"></a>Che cos'è Servizio app di Azure?

## <a name="what-is-web-apps"></a>Che cos'è App Web?

Servizio app di Azure è un servizio per l'hosting di applicazioni Web, API REST e back-end mobili. Servizio app supporta codice scritto in una vasta gamma di linguaggi, ad esempio .NET, .NET Core, Java, Ruby, Node.js, PHP e Python. Servizio app è ideale per la maggior parte dei siti Web, in particolare se non occorre un grande controllo sull'infrastruttura di hosting.

## <a name="what-is-the-app-service-plan"></a>Che cos'è il piano di servizio app?

Il piano di servizio app definisce le risorse di calcolo che l'app utilizzerà, dove si trovano tali risorse, il numero di risorse aggiuntivo che il piano può usare e il tipo di piano di servizio in uso. Queste risorse di calcolo sono analoghe alla server farm di un hosting Web tradizionale. È possibile configurare una o più app da eseguire nello stesso piano di servizio app.

Quando si distribuiscono le app, è possibile creare un piano di servizio app o continuare ad aggiungere le app a un piano esistente.  Tuttavia, solo le app nello stesso piano di servizio app condividono le stesse risorse di calcolo. Per determinare se la nuova app ha le risorse adeguate, è necessario valutare la capacità del piano di servizio app esistente e il carico previsto per la nuova app. Il sovraccarico di un piano di servizio app può potenzialmente causare tempi di inattività per le app nuove ed esistenti.

È possibile definire in anticipo un piano di servizio app nel portale di Azure con PowerShell o l'interfaccia della riga di comando di Azure oppure configurarne uno quando si pubblica l'applicazione in Visual Studio.

Ogni piano di servizio app definisce:

- Area (Stati Uniti occidentali, Stati Uniti orientali e così via)
- Numero di istanze della macchina virtuale
- Dimensioni delle istanze della macchina virtuale (piccola, media, grande)
- Piano tariffario (Gratuito, Condiviso, Basic, Standard, Premium, Premium V2, Isolato)

## <a name="specify-the-region"></a>Specificare l'area

Quando si crea un piano di servizio app, è necessario definire un'area o una località in cui verrà ospitato tale piano. In genere, si tende a scegliere un'area geograficamente vicina ai clienti previsti.

## <a name="what-are-the-pricing-and-reliability-levels"></a>Quali sono i prezzi e i livelli di affidabilità?

Un fattore chiave per una distribuzione efficace delle app in servizio app di Azure è la scelta del piano di servizio corretto. È possibile passare a un piano di servizio app superiore o inferiore in qualsiasi momento. È possibile scegliere prima un piano tariffario inferiore e passare a uno superiore in seguito, quando sono necessarie altre funzionalità del servizio app.

Esistono alcune categorie di piani tariffari:

**Calcolo condiviso**: i due piani di base **Gratuito** e **Condiviso** eseguono un'app nella stessa macchina virtuale di Azure delle altre app del servizio app, incluse le app di altri clienti. Questi piani allocano quote di CPU a ogni app eseguita nelle risorse condivise e non è possibile aumentare il numero di istanze delle risorse.

I piani Gratuito e Condiviso sono ottimali per i progetti personali su scala ridotta con richieste di traffico molto limitate, con un limite predefinito di 165 MB di dati in uscita ogni 24 ore.

**Calcolo dedicato**: i piani **Basic, Standard, Premium e Premium V2** eseguono le app nelle macchine virtuali di Azure dedicate. Solo le app nello stesso piano di servizio app condividono le stesse risorse di calcolo. È possibile aumentare il numero di istanze delle macchine virtuali in misura direttamente proporzionale al livello del piano.

Il piano di servizio Standard è ideale per i carichi di lavoro di produzione in tempo reale, usati per pubblicare applicazioni commerciali per i clienti.
I piani di servizio Premium supportano le app Web ad alta capacità in cui si desidera evitare i costi aggiuntivi di un piano dedicato (Isolato).

**Isolato**: questo piano esegue le macchine virtuali di Azure dedicate in reti virtuali di Azure dedicate, che forniscono alle app l'isolamento della rete oltre all'isolamento del calcolo. Offre funzionalità ottimali per aumentare il numero di istanze. Selezionare un piano di servizio Isolato solo in presenza del requisito specifico per garantire livelli massimi di prestazioni e sicurezza.

Isolare l'app in un nuovo piano di servizio app nei casi seguenti:

- L'app usa molte risorse
- Si vuole ridimensionare l'app indipendentemente dalle altre app nel piano esistente
- L'app necessita di risorse dislocate in un'area geografica diversa

## <a name="specify-the-resource-group"></a>Specificare il gruppo di risorse

Come accade per la maggior parte delle risorse di Azure, è necessario specificare il gruppo di risorse da usare. Si può usare un gruppo di risorse esistente o crearne uno direttamente da Visual Studio. Un gruppo di risorse è un contenitore logico in cui vengono distribuite e gestite risorse di Azure come app Web, database e account di archiviazione. È un meccanismo utile per mantenere le risorse associate tra loro.

## <a name="deploy-your-web-app-from-visual-studio"></a>Distribuire l'app Web da Visual Studio

La procedura per pubblicare l'app in Azure da Visual Studio è breve.

### <a name="select-the-project"></a>Selezionare il progetto

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.

1. Selezionare **Pubblica > Pubblica in Azure**.

### <a name="configure-the-app-service-plan-in-windows"></a>Configurare il piano di servizio app in Windows

1. Configurare il piano di servizio app

    - Hosting: in questa scheda si configura il piano di servizio app. Specifica la località, le dimensioni e le funzionalità del server Web che ospita l'app. È possibile selezionare un piano di hosting esistente o crearne uno nuovo. Windows genererà automaticamente nomi univoci a livello globale che possono essere modificati durante la configurazione.
    - Servizi: qui è possibile configurare un database SQL per il sito.

        > [!NOTE]
        > Sebbene sia possibile connettere e gestire un database SQL di Azure da Visual Studio, questa operazione esula dall'ambito di questo modulo.

1. Fare clic su **Crea** per distribuire l'app. Al termine, Visual Studio avvierà la pagina Web in cui è ospitato il sito.

### <a name="configure-the-app-service-plan-for-mac"></a>Configurare il piano di servizio app per Mac

1. È possibile selezionare un piano di servizio app esistente se in Azure se ne è già configurato uno, oppure crearne uno.

1. Configurare il piano di servizio app in questa scheda. Specifica la località, le dimensioni e le funzionalità del server Web che ospita l'app. È possibile selezionare un piano di hosting esistente o crearne uno nuovo. Tenere presente che Azure richiede che i nomi del sito Web e di tutte le risorse siano globalmente univoci.

1. Fare clic su **Crea** per distribuire l'app. Al termine, Visual Studio avvierà la pagina Web in cui è ospitato il sito.

## <a name="summary"></a>Riepilogo

Le applicazioni Web di ASP.NET Core e Servizio app di Azure rappresentano una soluzione flessibile e scalabile per l'hosting dell'app Web di ASP.NET. Come per tutti i servizi di Azure, è necessario specificare un gruppo di risorse e selezionare il piano tariffario che meglio soddisfa le proprie esigenze.
