Si supponga di lavorare per una società che esegue l'elaborazione di dati video e analisi dei modelli e che si stia creando un nuovo prototipo di piattaforma in grado di elaborare i video provenienti dalle telecamere del traffico, analizzare le tendenze e fornire dati utili per migliorare la gestione del traffico e delle strade. 

Per migliorare gli algoritmi, in alcune nuove città sono state adottate determinate disposizioni per raccogliere i dati delle telecamere del traffico. Non tutti i dati video, tuttavia, si trovano nello stesso formato e molti dei formati hanno solo codec Windows per la decodifica dei dati. Si è quindi deciso di usare macchine virtuali per eseguire l'elaborazione iniziale ed eseguire quindi il push dei dati in Funzioni di Azure, che elaborerà un formato standard. Questo approccio consentirà di adottare dinamicamente nuovi formati di dati senza interrompere l'intero sistema.

Azure offre una solida soluzione di hosting di macchine virtuali in grado di soddisfare ogni esigenza. Verrà ora illustrato come creare e usare le macchine virtuali Windows in Azure.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Comprendere le opzioni disponibili per le macchine virtuali in Azure.
- Creare una macchina virtuale Windows con il portale di Azure.
- Connettersi alla macchina virtuale Windows in esecuzione usando Desktop remoto.
- Installare il software e modificare la configurazione di rete in una macchina virtuale usando il portale di Azure.

## <a name="prerequisites"></a>Prerequisiti

- Conoscenza di base delle macchine virtuali di Azure da **Introduzione alle macchine virtuali di Azure**
- Client desktop remoto