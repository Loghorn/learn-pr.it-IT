Si supponga di lavorare per un'agenzia di stampa che diffonde avvisi di notizie in tempo reale. L'azienda si avvale di una rete mondiale di giornalisti che inviano costantemente aggiornamenti attraverso un portale Web e un'app per dispositivi mobili. Un servizio Web di livello intermedio accetta gli aggiornamenti e li pubblica online tramite diversi canali.

Si è rilevato che il sistema non registra alcuni avvisi corrispondenti a eventi di importanza globale. Si tratta di un problema _serio_, perché la concorrenza ottiene un vantaggio competitivo determinante. L'utente è stato selezionato come principale sviluppatore dell'azienda per identificare e risolvere il problema.

Il livello intermedio fornisce capacità più che sufficiente per la gestione di carichi normali. Tuttavia un esame dei log dei server rivela che il sistema ha registrato un overload quando più giornalisti hanno provato a caricare contemporaneamente articoli di rilievo. Alcuni autori hanno segnalato che il portale si bloccava e altri hanno detto che i loro articoli sono andati perduti. Lo sviluppatore ha rilevato una correlazione diretta tra i problemi segnalati e il picco della domanda per i server di livello intermedio.

Ovviamente è necessario un approccio per la gestione di questi picchi imprevisti. Non si vuole aggiungere altre istanze del sito Web e del servizio Web di livello intermedio, perché sono soluzioni costose e ridondanti in condizioni normali. È possibile eseguire lo spin up dinamico delle istanze, ma questa soluzione richiede tempo e lo stesso problema può ripetersi con i nuovi server quando vengono messi online.

Il problema può essere risolto usando l'archiviazione code di Azure. Una coda di archiviazione è un buffer di messaggi ad alte prestazioni che può fare da broker tra i componenti front end ("producer") e il livello intermedio ("consumer"). 

I componenti front end inseriscono un messaggio per ogni nuovo avviso in una coda. Quindi il livello intermedio recupera i messaggi dalla coda uno alla volta per l'elaborazione. In fasi di elaborazione intensiva la lunghezza della coda può crescere, ma nessun articolo va perduto e l'applicazione rimane reattiva. Quando la richiesta torna a livelli normali, il servizio Web recupera ed elabora il backlog della coda.

Le informazioni seguenti illustrano come usare l'Archiviazione code di Azure per gestire i picchi di domanda e migliorare la resilienza delle applicazioni distribuite.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

- Creare un account di Archiviazione di Azure che supporta le code.
- Creare una coda usando C# e la libreria client di Archiviazione di Azure per .NET.
- Aggiungere, recuperare e rimuovere messaggi da una coda usando C# e la libreria client di Archiviazione di Azure per .NET.