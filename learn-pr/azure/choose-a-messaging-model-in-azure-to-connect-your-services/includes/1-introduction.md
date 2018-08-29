Molte applicazioni sono costituite da programmi eseguiti in vari computer o dispositivi diversi. Nelle applicazioni distribuite di questo tipo, devono essere inviati messaggi tra i componenti attraverso le reti e su lunghe distanze. Anche nello stesso server o nello stesso data center, per le architetture con accoppiamento debole sono necessari meccanismi per le comunicazioni tra i componenti. L'affidabilità della messaggistica è spesso un problema critico.

Si supponga di lavorare presso una società di software che sviluppa un'applicazione per la condivisione di musica. I musicisti possono caricare la musica creata nella piattaforma tramite un front-end Web o un'app per dispositivi mobili e possono ascoltare le creazioni degli altri membri e aggiungere commenti. L'applicazione è costituita da un sito Web eseguito presso il provider di servizi Internet, un'app per dispositivi mobili eseguita nei dispositivi mobili degli utenti, un'API Web in esecuzione in Azure e un database SQL di Azure in cui sono archiviati i dati.

Dai monitoraggi risulta che, nei periodi di richieste elevate, alcuni file musicali non vengono caricati correttamente e alcuni commenti non vengono pubblicati. I test indicano che questi problemi sono causati dalla perdita di messaggi tra i componenti front-end e l'API Web. Si intende risolvere questi problemi usando una o più delle seguenti tecnologie di Azure: code di archiviazione, hub eventi, griglie di eventi e bus di servizio.

In questo modulo si apprenderà come scegliere la tecnologia di messaggistica appropriata in Azure per ogni attività di comunicazione in un'applicazione distribuita.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

- Descrivere gli eventi e i messaggi e indicare per quali scenari è possibile usarli in un'applicazione distribuita.
- Identificare gli scenari in cui una coda di archiviazione di Azure è la tecnologia di messaggistica ottimale per un'applicazione.
- Identificare gli scenari in cui una griglia di eventi di Azure è la tecnologia di messaggistica ottimale per un'applicazione.
- Identificare gli scenari in cui un hub eventi di Azure è la tecnologia di messaggistica ottimale per un'applicazione.
- Identificare gli scenari in cui il bus di servizio di Azure è la tecnologia di messaggistica ottimale per un'applicazione.
