Si supponga di lavorare per una società che esegue l'elaborazione e l'archiviazione dei dati delle telecamere del traffico. I flussi video vengono analizzati, suddivisi in categorie ed elaborati per identificare visi e targhe in orari specifici. Le informazioni vengono caricate in Azure Data Lake e viene generato un indice di ricerca per le autorità giudiziarie.

Questi flussi video usano una serie di risoluzioni e codec diversi. È necessario eseguire diversi pacchetti software proprietari basati su Windows per effettuare l'elaborazione iniziale e codificarli in un formato video comune. Poiché vengono rilasciati regolarmente nuovi formati, è utile eseguire l'elaborazione video nelle macchine virtuali. I pacchetti proprietari possono quindi essere aggiunti e aggiornati senza interrompere l'intero sistema.

Azure offre una solida soluzione di hosting di macchine virtuali che riesce a soddisfare ogni esigenza. Verrà ora illustrato come creare e usare le macchine virtuali Windows in Azure.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Comprendere le opzioni disponibili per le macchine virtuali in Azure.
- Creare una macchina virtuale Windows con il portale di Azure.
- Connettersi alla macchina virtuale Windows in esecuzione usando Desktop remoto.
- Installare il software e modificare la configurazione di rete in una macchina virtuale usando il portale di Azure.