Molte applicazioni sono costituite da programmi eseguiti in vari computer o dispositivi diversi. Nelle applicazioni distribuite di questo tipo, devono essere inviati messaggi tra i componenti attraverso le reti e su lunghe distanze. Anche nello stesso server o nello stesso data center, per le architetture con accoppiamento debole sono necessari meccanismi per le comunicazioni tra i componenti. L'affidabilità della messaggistica è spesso un problema critico.

Si supponga di lavorare presso una società di software che sviluppa un'applicazione per la condivisione di musica. I musicisti possono caricare la musica creata nella piattaforma tramite un front-end Web o un'app per dispositivi mobili e possono ascoltare le creazioni degli altri membri e aggiungere commenti. L'applicazione è costituita da un sito Web eseguito presso il provider di servizi Internet, un'app per dispositivi mobili eseguita nei dispositivi mobili degli utenti, un'API Web in esecuzione in Azure e un database SQL di Azure in cui sono archiviati i dati.

Dai monitoraggi risulta che, nei periodi di richieste elevate, alcuni file musicali non vengono caricati correttamente e alcuni commenti non vengono pubblicati. I test indicano che questi problemi sono causati dalla perdita di messaggi tra i componenti front-end e l'API Web. Si intende risolvere questi problemi usando una o più delle tecnologie seguenti: code di Archiviazione di Azure, Hub eventi di Azure, Griglia di eventi di Azure e bus di servizio di Azure.

In questo modulo si apprenderà come scegliere la tecnologia di messaggistica appropriata in Azure per ogni attività di comunicazione in un'applicazione distribuita.

## <a name="learning-objectives"></a>Obiettivi di apprendimento
In questo modulo verrà descritto come:

- Descrivere gli eventi e i messaggi e indicare per quali scenari è possibile usarli in un'applicazione distribuita.
- Identificare gli scenari in cui una coda di archiviazione è la tecnologia di messaggistica ottimale per un'applicazione.
- Identificare gli scenari in cui Griglia di eventi è la tecnologia di messaggistica ottimale per un'applicazione.
- Identificare gli scenari in cui Hub eventi è la tecnologia di messaggistica ottimale per un'applicazione.
- Identificare gli scenari in cui il bus di servizio è la tecnologia di messaggistica ottimale per un'applicazione.
