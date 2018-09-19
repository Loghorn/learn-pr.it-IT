Il team di ricerca ha raccolto una quantità di dati di immagine talmente enorme da portare a una scoperta su Marte. Deve ora eseguire un processo di elaborazione dei dati a elevata intensità di calcolo, ma non ha gli strumenti adatti. Di seguito viene illustrato perché Azure è un'ottima scelta per l'analisi dei dati.

## <a name="what-is-azure-compute"></a>Che cos'è il calcolo di Azure?
Il calcolo di Azure è un servizio di calcolo on demand per l'esecuzione di applicazioni basate sul cloud. Offre risorse di calcolo, ad esempio processori multi-core e supercomputer, tramite macchine virtuali e contenitori. Offre inoltre l'elaborazione serverless per eseguire le app senza la necessità di installare o configurare l'infrastruttura. Le risorse sono disponibili on demand e in genere possono essere create in pochi minuti o addirittura secondi. Si paga solo per le risorse usate e solo per il periodo in cui le si usa.

Esistono tre tecniche comuni per l'esecuzione del calcolo in Azure:

- Macchine virtuali
- Contenitori
- Elaborazione serverless

## <a name="what-are-virtual-machines"></a>Che cosa sono le macchine virtuali?

Le **macchine virtuali**, o VM, sono emulazioni software di computer fisici. Includono un processore virtuale, memoria, archiviazione e risorse di rete. Ospitano un sistema operativo e consentono di installare ed eseguire software come in un computer fisico. Tramite un client desktop remoto è inoltre possibile usare e controllare la macchina virtuale proprio come se ci si trovasse di fronte a un computer fisico.

:::row:::
  :::column:::
    ![Immagine che rappresenta una macchina virtuale](../media/2-vm.png)
  :::column-end:::
    :::column span="3"::: **Macchine virtuali in Azure**

È possibile creare e ospitare macchine virtuali in Azure. In genere, è possibile creare nuove macchine virtuali ed eseguirne il provisioning in pochi minuti selezionando l'immagine di una macchina virtuale preconfigurata.

La scelta di un'immagine è una delle decisioni più importanti durante la creazione di una macchina virtuale. Un'immagine è un modello usato per creare una macchina virtuale. Questi modelli includono già un sistema operativo e, spesso, anche altri software, ad esempio strumenti di sviluppo o ambienti di hosting web.

  :::column-end:::
:::row-end:::

## <a name="what-are-containers"></a>Che cosa sono i contenitori?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

I contenitori sono ambienti di virtualizzazione ma, a differenza di una macchina virtuale, non includono un sistema operativo. Al contrario, fanno riferimento al sistema operativo dell'ambiente host che esegue il contenitore. Ad esempio, se cinque contenitori sono in esecuzione su un server con un kernel Linux specifico, tutti e cinque i contenitori sono in esecuzione sullo stesso kernel.

La figura seguente illustra un confronto tra le applicazioni in esecuzione direttamente in una macchina virtuale e le applicazioni in esecuzione all'interno di contenitori in una macchina virtuale.

![Figura che illustra come il sistema operativo sia parte della macchina virtuale e non del contenitore](../media/2-vm-versus-containers.png)

I contenitori includono in genere un'applicazione scritta dall'utente insieme alle librerie necessarie per eseguire l'applicazione nel kernel dell'ambiente host.

I contenitori sono concepiti per essere leggeri e sono stati progettati per essere creati, scalati e arrestati in modo dinamico. Ciò consente di rispondere ai cambiamenti della domanda e di riavviarli rapidamente in caso di arresto anomalo del sistema o interruzione hardware.

Un altro vantaggio dell'uso dei contenitori è la possibilità di eseguire più applicazioni isolate in una macchina virtuale. Poiché i contenitori stessi sono protetti e isolati, non è necessario separare le macchine virtuali per carichi di lavoro separati.

Azure supporta i contenitori Docker, nonché diverse modalità per la loro gestione. I contenitori possono essere gestiti manualmente o con servizi di Azure, ad esempio Azure Kubernetes Service.

### <a name="what-is-serverless-computing"></a>Che cos'è l'elaborazione serverless?

L'elaborazione serverless è un ambiente di esecuzione ospitato sul cloud che esegue il codice ma che astrae completamente l'ambiente di hosting sottostante. Si crea un'istanza del servizio e si aggiunge il codice: non è necessaria, né tantomeno consentita, alcuna configurazione o manutenzione dell'infrastruttura.

#### <a name="what-is-serverless-computing"></a>Che cos'è l'elaborazione serverless?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yzjL]

Le app serverless vengono configurate per rispondere ad _eventi_. Può trattarsi di un endpoint REST, un timer o anche un messaggio ricevuto da un altro servizio di Azure. L'app serverless viene eseguita solo quando viene attivata da un evento.

In pratica, l'utente non è responsabile dell'infrastruttura. La scalabilità e le prestazioni vengono gestite automaticamente e vengono addebitate solo le risorse effettivamente usate. Non è necessario prenotare la capacità.

:::row:::
  :::column:::
    ![Immagine che rappresenta l'elaborazione serverless](../media/2-serverless.png)
  :::column-end:::
    :::column span="3"::: **Elaborazione serverless in Azure**

Azure offre due tipi di implementazione dell'elaborazione serverless:

- **Funzioni di Azure**, che possono eseguire codice praticamente in tutti i linguaggi moderni.
- **App per la logica di Azure**, che sono state progettate in una finestra di progettazione basata su Web e possono eseguire la logica attivata dai servizi di Azure senza scrivere alcun codice.

  :::column-end:::
:::row-end:::