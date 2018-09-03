Si supponga di lavorare presso una società che esegue l'elaborazione dei dati delle telecamere del traffico. I flussi video vengono analizzati, suddivisi in categorie ed elaborati per identificare visi e targhe in orari specifici. Queste informazioni vengono quindi caricate in Azure Data Lake e viene generato un indice ricercabile.

Questi flussi video usano una serie di risoluzioni e codec diversi. È necessario eseguire diversi pacchetti software proprietari basati su Windows per effettuare l'elaborazione iniziale e codificarli in un formato video comune. Poiché vengono rilasciati regolarmente nuovi formati, è utile eseguire l'elaborazione video nelle macchine virtuali. I pacchetti proprietari possono quindi essere aggiunti e aggiornati senza interrompere l'intero sistema.

Per amministrare il software di codifica nelle macchine virtuali Windows, è necessario connetterlo all'interfaccia utente.

Questo modulo illustrerà come creare una macchina virtuale Windows e connettersi a essa con Remote Desktop Protocol (RDP).

## <a name="learning-objectives"></a>Obiettivi di apprendimento
> [!div class="checklist"]
> * Creare una macchina virtuale Windows con il portale di Azure.
> * Comprendere le opzioni disponibili per le macchine virtuali in Azure.
> * Connettersi a una macchina virtuale Windows in esecuzione tramite Connessione desktop remoto di Windows.

## <a name="prerequisites"></a>Prerequisiti

- Conoscenza dell'ambiente dei servizi cloud di Azure.
- Familiarità con le macchine virtuali.
