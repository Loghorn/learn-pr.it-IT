Dopo aver presentato i servizi di elaborazione di Azure disponibili, ognuno di questi verrà esaminato per illustrarne i principali criteri di utilizzo.

## <a name="azure-virtual-machines"></a>Macchine virtuali di Azure

Le macchine virtuali sono una scelta ideale quando è necessario il controllo completo sul sistema operativo e sull'ambiente. Consentono infatti di personalizzare tutto il software in esecuzione nella macchina virtuale, proprio come con un computer fisico. Questa soluzione è indicata quando si eseguono configurazioni di hosting o software personalizzati,

ma anche quando si passa da un server fisico al cloud. È spesso possibile creare un'immagine del server fisico e ospitarla in una macchina virtuale. Si ha così totale libertà di scelta dei sistemi operativi e degli ambienti di esecuzione dell'applicazione, vale a dire che è possibile sviluppare in qualsiasi linguaggio che usi gli strumenti scelti dall'utente.

Sarà tuttavia necessario gestire la macchina virtuale ovvero aggiornare il sistema operativo e il software che esegue. 

È anche necessario valutare l'eventuale ridimensionamento. È possibile incrementare le prestazioni della macchina virtuale, ad esempio aggiungendo risorse di calcolo e memoria. Se è necessario eseguire più istanze parallele, può essere utile aggiungere altri servizi, ad esempio il bilanciamento del carico.

Si ipotizzi di gestire un sito Web che ospita a sua volta un sito Web per la rivendita online. Se la macchina virtuale è stata duplicata, sarà necessario un servizio aggiuntivo per instradare le richieste tra le diverse istanze della macchina virtuale del sito Web.

Azure rende disponibili anche servizi di macchine virtuali avanzati:

* Per eseguire in modo coerente istanze disponibili della stessa app o set di app su macchine virtuali configurate in modo analogo, utilizzare l'opzione **Set di scalabilità di macchine virtuali**. In pochi minuti, l'opzione genera automaticamente migliaia di macchine virtuali identiche caricate con l'applicazione, evitando agli utenti qualsiasi attesa. Non essendo necessario il pre-provisioning delle macchine virtuali, vengono sempre usate esclusivamente le risorse di calcolo necessarie all'applicazione.

* Nei casi in cui è necessaria potenza di elaborazione non elaborata o potenza di calcolo da supercomputer, l'opzione **Batch** consente la gestione del calcolo e della pianificazione dei processi a livello di cloud, con la possibilità di scalare fino a decine, centinaia o migliaia di macchine virtuali. È anche possibile specificare macchine virtuali con capacità di supercomputer.

## <a name="azure-containers"></a>Contenitori di Azure

I contenitori sono la scelta ideale per eseguire più istanze di un'applicazione in una singola macchina virtuale. L'agente di orchestrazione del contenitore è in grado di avviare, arrestare e aumentare il numero di istanze dell'applicazione in base alle esigenze.

I contenitori sono in genere usati per creare soluzioni con un'architettura di microservizi e per suddividere le soluzioni in componenti più piccoli. È possibile ad esempio suddividere un sito Web in tre contenitori: uno che ospita il front-end, un altro per il back-end e un terzo per l'archiviazione. Ciò consente di separare i componenti dell'app in sezioni logiche che possono quindi essere gestite, ridimensionate o aggiornate in modo indipendente.

Si supponga che il back-end del sito Web abbia ha raggiunto i limiti di capacità, mentre il front-end e l'archiviazione non risultano ancora sovraccaricati. In questo caso, è possibile ridimensionare solo il back-end per migliorarne le prestazioni, oppure scegliere di usare un servizio di archiviazione diverso o ancora sostituire il contenitore di archiviazione senza che ciò abbia impatto sul resto dell'applicazione.

 Se il team ha familiarità con l'orchestrazione dei contenitori Kubernetes, è opportuno valutare l'opzione **Azure Kubernetes Service (AKS)**. La soluzione è in grado di semplificare e automatizzare la gestione, la distribuzione e le operazioni di orchestrazione Kubernetes.

## <a name="azure-functions"></a>Funzioni di Azure

Funzioni di Azure è ideale quando si è interessati solo al codice che esegue il servizio e non alla piattaforma o all'infrastruttura sottostante. Funzioni di Azure consente di eseguire operazioni in risposta a un evento, spesso tramite una richiesta REST, un timer o un messaggio proveniente da un altro servizio di Azure e quando l'operazione può essere completata in pochi secondi al massimo.

Grazie alla scalabilità automatica, è una scelta eccellente quando la domanda è variabile; l'addebito avviene solo quando la funzione viene attivata. Si ipotizzi ad esempio di ricevere messaggi da una soluzione IoT usata per monitorare una flotta di mezzi adibiti alle consegne. È probabile che nelle ore lavorative si avrà un incremento dei dati in arrivo.

Funzioni di Azure è un servizio senza stato e si comporta come se venisse riavviato ogni volta che risponde a un evento. Per tale motivo è ideale per l'elaborazione dei dati in ingresso. Se è necessario lo stato, i dati possono essere connessi a un servizio di archiviazione di Azure.
