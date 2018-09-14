Il team di ricerca sono state raccolte enormi quantità di dati di immagine che potrebbero causare un'individuazione in Mars. È necessario eseguire l'elaborazione dati di funzionalità di calcolo avanzata, ma non dispone dei dispositivi per svolgere il lavoro. Di seguito viene illustrato il motivo per cui Azure è una scelta ottimale per eseguire l'analisi dei dati.

## <a name="what-is-azure-compute"></a>Che cos'è il calcolo di Azure?
Calcolo di Azure è un servizio di calcolo on demand per l'esecuzione di applicazioni basate su cloud. Fornisce le risorse di calcolo, ad esempio processori multi-core e supercomputer, tramite macchine virtuali e contenitori. Fornisce inoltre l'elaborazione senza server per eseguire le app senza richiedere l'installazione dell'infrastruttura o configurazione. Le risorse sono disponibili on demand e in genere possono essere create in pochi minuti o addirittura secondi. Si paga solo per le risorse usate e solo per il periodo in cui le si usa.

Esistono tre tecniche comuni per l'esecuzione del calcolo in Azure:

- Macchine virtuali
- Contenitori
- Elaborazione serverless

## <a name="what-are-virtual-machines"></a>Che cosa sono le macchine virtuali?

**Le macchine virtuali**, o le macchine virtuali, sono emulazioni software dei computer fisici. Include un processore virtuale, memoria, archiviazione e risorse di rete. Ospita un sistema operativo e consente di installare ed eseguire software come in un computer fisico. E con un client desktop remoto, è possibile usare e controllare la macchina virtuale come se si trovasse in primo piano si.

### <a name="virtual-machines-in-azure"></a>Macchine virtuali Linux in Azure

Le macchine virtuali possono essere create e ospitate in Azure. In genere, nuove macchine virtuali possono essere create e il provisioning in pochi minuti selezionando un'immagine di macchina virtuale preconfigurata.

Selezione di un'immagine è una delle più importanti decisioni che prese quando si crea una macchina virtuale. Un'immagine è un modello usato per creare una macchina virtuale. Questi modelli includono già un sistema operativo e, spesso, anche altri software, ad esempio strumenti di sviluppo o ambienti di hosting web.

## <a name="what-are-containers"></a>Che cosa sono i contenitori?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

I contenitori sono ambienti di virtualizzazione ma, a differenza di una macchina virtuale, non includono un sistema operativo. Al contrario, fanno riferimento al sistema operativo dell'ambiente host che esegue il contenitore. Ad esempio, se cinque contenitori sono in esecuzione su un server con un kernel Linux specifico, tutti e cinque i contenitori sono in esecuzione sullo stesso kernel.

La figura seguente illustra un confronto tra le applicazioni in esecuzione direttamente in una macchina virtuale e le applicazioni in esecuzione all'interno di contenitori in una macchina virtuale.

![Un'illustrazione che mostra come il sistema operativo è una parte della macchina virtuale e non fa parte del contenitore](../media/2-vm-versus-containers.png)

I contenitori contengono in genere un'applicazione che si scrive &mdash; insieme a tutte le librerie necessarie per l'applicazione venga eseguita sul kernel dell'ambiente host.

I contenitori sono concepiti per essere leggero e sono progettati per essere creati, scalabilità orizzontale e arrestato in modo dinamico. In questo modo è possibile rispondere alle modifiche su richiesta e di riavviare rapidamente in caso di un'interruzione di arresto anomalo del sistema o hardware.

Un ulteriore vantaggio dell'uso di contenitori è la possibilità di eseguire più applicazioni isolate in una macchina virtuale. Poiché i contenitori stessi sono protetti e isolati, non è necessario separare le macchine virtuali per carichi di lavoro separati.

Azure supporta i contenitori Docker, nonché diverse modalità per la loro gestione. I contenitori possono essere gestiti manualmente o con servizi di Azure, ad esempio Azure Kubernetes Service.

### <a name="what-is-serverless-computing"></a>Che cos'è l'elaborazione serverless?

L'elaborazione serverless è un ambiente di esecuzione ospitato sul cloud che esegue il codice ma che astrae completamente l'ambiente di hosting sottostante. Si crea un'istanza del servizio e si aggiunge il codice: non è necessaria, né tantomeno consentita, alcuna configurazione o manutenzione dell'infrastruttura.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yzjL]

Le app serverless vengono configurate per rispondere ad _eventi_. Può trattarsi di un endpoint REST, un timer o anche un messaggio ricevuto da un altro servizio di Azure. L'app serverless viene eseguita solo quando viene attivata da un evento.

In pratica, dell'infrastruttura non è responsabilità dell'utente. La scalabilità e le prestazioni vengono gestite automaticamente e vengono addebitate solo le risorse effettivamente usate. Non è necessario riservare la capacità.

Azure offre due tipi di implementazione dell'elaborazione serverless:

- **Funzioni di Azure** che possono eseguire codice in qualsiasi linguaggio moderno.
- **App per la logica di Azure**, che sono state progettate in una finestra di progettazione basata su Web e possono eseguire la logica attivata dai servizi di Azure senza scrivere alcun codice.
