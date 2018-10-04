Dopo aver presentato i servizi di calcolo di Azure disponibili, ognuno di questi verrà esaminato più in dettaglio per capire meglio in quale situazioni usarlo.

## <a name="azure-virtual-machines"></a>Macchine virtuali di Azure

:::row:::
  :::column:::
    ![Immagine che rappresenta le macchine virtuali di Azure](../media/3-azure-vms.png)
  :::column-end:::
  :::column span="3":::
Le macchine virtuali sono la scelta ideale quando è necessario esercitare un controllo totale sul sistema operativo e sull'ambiente. Consentono infatti di personalizzare tutto il software in esecuzione nella macchina virtuale, proprio come con un computer fisico. Questa soluzione è particolarmente indicata quando si eseguono configurazioni di hosting o software personalizzati,
  :::column-end:::
:::row-end:::

ma anche quando si passa da un server fisico al cloud. È spesso possibile creare un'immagine del server fisico e ospitarla in una macchina virtuale. Si ha così totale libertà di scelta dei sistemi operativi e degli ambienti di esecuzione dell'applicazione, vale a dire che è possibile sviluppare in qualsiasi linguaggio che usi gli strumenti scelti dall'utente.

Sarà tuttavia necessario gestire la macchina virtuale ovvero aggiornare il sistema operativo e il software che esegue. 

È anche necessario valutare l'eventuale ridimensionamento. È possibile aumentare le prestazioni della macchina virtuale, ad esempio aggiungendo risorse di calcolo e memoria. Se è necessario eseguire più istanze parallele, ovvero scalare orizzontalmente, può essere utile aggiungere altri servizi, ad esempio il bilanciamento del carico.

Si supponga di eseguire un sito Web che consente agli scienziati di caricare immagini astronomiche per l'elaborazione. Se la macchina virtuale è stata duplicata, sarà necessario un servizio aggiuntivo per instradare le richieste tra le diverse istanze della macchina virtuale del sito Web.

Azure rende disponibili anche servizi avanzati per le macchine virtuali:

- Usare l'opzione **Set di scalabilità di macchine virtuali** quando è necessario eseguire in modo coerente istanze disponibili della stessa app, o set di app, su macchine virtuali configurate in modo analogo. In pochi minuti, l'opzione genera automaticamente migliaia di macchine virtuali identiche caricate con l'applicazione, evitando agli utenti qualsiasi attesa. Non essendo necessario il pre-provisioning delle macchine virtuali, vengono sempre usate esclusivamente le risorse di calcolo necessarie all'applicazione.

- Usare l'opzione **Batch** quando sono necessarie funzionalità di gestione del calcolo e di pianificazione dei processi a livello di cloud, con la possibilità di scalare fino a decine, centinaia o migliaia di macchine virtuali. Nei casi in cui è necessaria potenza di elaborazione non elaborata o potenza di calcolo da supercomputer, Azure offre queste capacità.

## <a name="azure-containers"></a>Contenitori di Azure

:::row:::
  :::column:::
    ![Immagine che rappresenta i contenitori di Azure](../media/3-azure-containers.png)
  :::column-end:::
  :::column span="3":::
I contenitori sono la scelta ideale per eseguire più istanze di un'applicazione in una singola macchina virtuale. L'agente di orchestrazione del contenitore è in grado di avviare, arrestare e aumentare il numero di istanze dell'applicazione in base alle esigenze.
  :::column-end:::
:::row-end:::

#### <a name="vms-versus-containers"></a>Macchine virtuali o contenitori

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuaq]

I contenitori vengono spesso usati per creare soluzioni usando un'architettura di microservizi, dato che sono impiegati di frequente per suddividere le soluzioni in parti più piccole. È possibile ad esempio suddividere un sito Web in tre contenitori: uno che ospita il front-end, un altro per il back-end e un terzo per l'archiviazione. Ciò consente di separare i componenti dell'app in sezioni logiche che possono quindi essere gestite, ridimensionate o aggiornate in modo indipendente.

#### <a name="what-is-a-microservice"></a>Che cos'è un microservizio?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yual]

Si supponga che il back-end del sito Web abbia raggiunto i limiti di capacità, mentre il front-end e l'archiviazione non risultano ancora sovraccaricati. In questo caso, è possibile ridimensionare il back-end separatamente per migliorare le prestazioni oppure è possibile decidere di usare un servizio di archiviazione diverso, o persino di sostituire il contenitore di archiviazione senza che ciò abbia impatto sul resto dell'applicazione.

Se il team ha familiarità con l'orchestrazione dei contenitori Kubernetes, è opportuno valutare l'opzione **Azure Kubernetes Service (AKS)**. Questo servizio è in grado di semplificare e automatizzare la gestione, la distribuzione e le operazioni di orchestrazione Kubernetes.

#### <a name="what-is-kubernetes"></a>Che cos'è Kubernetes?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEuX]

## <a name="azure-functions"></a>Funzioni di Azure

:::row:::
  :::column:::
    ![Immagine che rappresenta Funzioni di Azure](../media/3-azure-functions.png)
  :::column-end:::
  :::column span="3":::
Funzioni di Azure è la scelta ideale se si è interessati solo al codice che esegue il servizio e non alla piattaforma o all'infrastruttura sottostante. Funzioni di Azure consente di eseguire operazioni in risposta a un evento, spesso tramite una richiesta REST, un timer o un messaggio proveniente da un altro servizio di Azure e quando l'operazione può essere completata in pochi secondi al massimo.
  :::column-end:::
:::row-end:::

Grazie alla scalabilità automatica, Funzioni di Azure rappresenta una scelta affidabile quando la domanda è variabile e l'addebito avviene solo quando la funzione viene attivata. Si ipotizzi ad esempio di ricevere messaggi da una soluzione IoT usata per monitorare una flotta di mezzi adibiti alle consegne. È probabile che nelle ore lavorative si avrà un incremento dei dati in arrivo.

Inoltre, Funzioni di Azure è un servizio senza stato e si comporta come se venisse riavviato ogni volta che risponde a un evento. Per tale motivo è ideale per l'elaborazione dei dati in ingresso. Se è necessario lo stato, i dati possono essere connessi a un servizio di archiviazione di Azure.
