Si è stati assunti da una società di corse automobilistiche internazionale per modernizzarne l'intera piattaforma Web e di monitoraggio. La società ha deciso di sostituire i server Linux esistenti con un'infrastruttura basata sul cloud che sfrutta le ultime tendenze nel campo dell'architettura. Parte del sistema verrà eseguita sulla piattaforma senza server di Azure usando Funzioni di Azure per elaborare i dati delle corse in tempo reale e inviando statistiche, dati e altre informazioni analizzate rilevanti ai cluster dei database. L'azienda vuole mantenere il proprio sito Web esistente, che è stato riscritto appena un anno fa, ma collegandolo a questo flusso di dati moderno.

Il sito Web viene eseguito in Apache con Linux e poiché è già operativo, si decide di spostarlo direttamente in Azure usando una macchina virtuale di Azure. In questo modo il sito Web è in grado di accedere ai dati riducendo al minimo l'intervento dell'utente.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo viene descritto come:

- Comprendere le opzioni disponibili per le macchine virtuali in Azure
- Creare una macchina virtuale Linux usando il portale di Azure
- Connettersi a una macchina virtuale Linux in esecuzione usando SSH
- Installare il software e modificare la configurazione di rete in una macchina virtuale usando il portale di Azure