Il team di ricerca deve eseguire un processo di elaborazione dei dati ad elevata intensità di calcolo e non dispone delle apparecchiature per svolgere il lavoro. Si è deciso di usare Azure per eseguire l'analisi dei dati.

## <a name="what-is-azure-compute"></a>Che cos'è il calcolo di Azure?
Il calcolo di Azure è un servizio di risorse di calcolo on demand per l'esecuzione di applicazioni basate su cloud. Fornisce le risorse di calcolo, ad esempio processori multi-core e supercomputer, tramite macchine virtuali e contenitori. Fornisce inoltre l'elaborazione serverless per abilitare l'esecuzione di app senza richiedere l'impostazione o la configurazione dell'infrastruttura. Le risorse sono disponibili on demand e in genere possono essere create in pochi minuti o addirittura secondi. Si paga solo per le risorse usate e solo per il periodo in cui le si usa.

Esistono tre tecniche comuni per l'esecuzione del calcolo in Azure:
1. Macchine virtuali
1. Contenitori
1. Elaborazione serverless

## <a name="what-are-virtual-machines"></a>Che cosa sono le macchine virtuali?

Una **macchina virtuale** è un'emulazione software di un computer fisico. Include un processore virtuale, memoria, archiviazione e risorse di rete. Ospita un sistema operativo e consente di installare ed eseguire software come in un computer fisico. Mediante un client desktop remoto, è possibile usare e controllare la macchina virtuale come se si stesse usando un terminale fisico.

### <a name="virtual-machines-in-azure"></a>Macchine virtuali Linux in Azure

Le macchine virtuali possono essere create e ospitate in Azure. In genere, è possibile creare nuove macchine virtuali ed eseguirne il provisioning in pochi minuti selezionando un'immagine preconfigurata della macchina virtuale.

La scelta dell'immagine è una delle fasi più importanti durante la creazione di una macchina virtuale. Un'immagine è un modello usato per creare una macchina virtuale. Questi modelli includono già un sistema operativo e, spesso, anche altri software, ad esempio strumenti di sviluppo o ambienti di hosting web.

## <a name="what-are-containers"></a>Che cosa sono i contenitori?

I contenitori sono ambienti di virtualizzazione ma, a differenza di una macchina virtuale, non includono un sistema operativo. Al contrario, fanno riferimento al sistema operativo dell'ambiente host che esegue il contenitore. Ad esempio, se cinque contenitori sono in esecuzione su un server con un kernel Linux specifico, tutti e cinque i contenitori sono in esecuzione sullo stesso kernel. 

In questo diagramma viene fatto un confronto tra le applicazioni eseguite direttamente in una macchina virtuale e quelle eseguite come contenitori.

![Confronto tra le app eseguite direttamente in una VM e i contenitori](../media/vm-versus-containers.png)

I contenitori includono in genere un'applicazione scritta dall'utente, oltre alle librerie necessarie per eseguire l'applicazione nel kernel dell'ambiente host. 

I contenitori sono concepiti per essere leggeri e sono stati progettati per essere creati, scalati e interrotti in modo dinamico in base all'ambiente e alla domanda.

Un vantaggio dell'uso dei contenitori è la possibilità di eseguire più applicazioni isolate in una macchina virtuale. Poiché i contenitori stessi sono protetti e isolati, non è necessario separare le macchine virtuali per carichi di lavoro separati.

Azure supporta i contenitori Docker, nonché diverse modalità per la loro gestione. I contenitori possono essere gestiti manualmente o con servizi di Azure, ad esempio Azure Kubernetes Service.

### <a name="what-is-serverless-computing"></a>Che cos'è l'elaborazione serverless?

L'elaborazione serverless è un ambiente di esecuzione ospitato sul cloud che esegue il codice ma che astrae completamente l'ambiente di hosting sottostante. Si crea un'istanza del servizio e si aggiunge il codice: non è necessaria, né tantomeno consentita, alcuna configurazione o manutenzione dell'infrastruttura.

Le app serverless vengono configurate per rispondere ad _eventi_. Può trattarsi di un endpoint REST, un timer o anche un messaggio ricevuto da un altro servizio di Azure. L'app serverless viene eseguita solo se è attivata da un evento.

L'infrastruttura non è una responsabilità dell'utente. La scalabilità e le prestazioni vengono gestite automaticamente e vengono addebitate solo le risorse esatte usate. Non è necessario riservare la capacità.

Azure offre due tipi di implementazione dell'elaborazione serverless: 

1. **Funzioni di Azure**, che possono eseguire codice praticamente in tutti i linguaggi moderni
2. **App per la logica di Azure**, che sono state progettate in una finestra di progettazione basata su Web e possono eseguire la logica attivata dai servizi di Azure senza scrivere alcun codice.
