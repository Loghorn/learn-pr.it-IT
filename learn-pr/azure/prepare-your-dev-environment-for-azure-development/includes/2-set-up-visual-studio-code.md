Visual Studio Code è una scelta diffusa per lo sviluppo di applicazioni per Azure. L'ambiente di sviluppo integrato (IDE) è leggero perché occupa solo alcuni megabyte di spazio di archiviazione, contrariamente ai gigabyte occupati da alcuni ambienti di sviluppo integrato. Visual Studio Code è multipiattaforma: funziona in Windows, Linux e macOS. Inoltre, è flessibile. È possibile usare Visual Studio Code per distribuire le app tramite l'interfaccia della riga di comando di Azure, il servizio App di Azure oppure usando un'immagine del contenitore Docker. È inoltre possibile distribuire le app con Funzioni di Azure e la modalità senza server. 

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code, o semplicemente VS Code, è un editor semplice e potente. Supporta la maggior parte dei linguaggi di programmazione, addirittura centinaia, ed è progettato per connettersi ai servizi cloud.

Con particolare attenzione al supporto multipiattaforma, può essere infatti eseguito in Windows, Linux e macOS, all'agilità e alla portabilità dell'esperienza, l'installazione di base di Visual Studio Code include un editor che riconosce una gamma di sintassi dei linguaggi di programmazione in continua crescita. Tuttavia, non è presente alcun compilatore. La compilazione avviene nel cloud o tramite un'estensione.

Si otterrà un notevole supporto incorporato per il controllo del codice sorgente tramite Gestione controllo del codice sorgente di Git. Ciò significa che è necessario installare innanzitutto il framework di Git.

## <a name="extension-model"></a>Modello di estensione

Una delle funzionalità più potenti di Visual Studio Code è il modello di estensione. Consente l'esecuzione di funzionalità di terze parti come parte integrante dell'IDE di Visual Studio Code e di estendere le funzionalità dell'IDE in qualunque modo immaginabile.

Un'estensione viene scritta in JavaScript o TypeScript e può anche essere sviluppata in Visual Studio Code stesso. È possibile usare Yeoman per eseguire lo scaffolding di un'estensione. Tutti questi strumenti supportano IntelliSense, l'esplorazione del codice e un'esperienza di debug completa.

Esistono tre categorie generali di estensioni per Visual Studio Code: le estensioni, i server di linguaggio e il debugger. Gli ultimi due hanno protocolli aggiuntivi che consentono di fornire funzionalità specializzate: multilinguaggio per l'editor oppure per arricchire l'esperienza di debug.

Le estensioni disponibili nel marketplace delle estensioni includono il supporto di linguaggio per Python, Go, C++ e molti altri. Le estensioni includono anche strumenti di formattazione del codice, ad esempio linter, strumenti per la connettività cloud, ad esempio Azure, nuovi temi, formattatori di codice e librerie di frammenti di codice. Tutte queste estensioni sono disponibili nel [marketplace di Visual Studio Code](https://marketplace.visualstudio.com/).

## <a name="azure-extensions"></a>Estensioni di Azure

Molte estensioni sono rivolte alle funzionalità e ai prodotti di Azure e molte altre vengono aggiunte continuamente. Riguardano ambiti quali il supporto per docker, la gestione delle sottoscrizioni, gli strumenti per l'interfaccia della riga di comando di Azure, l'accesso al database, l'integrazione di API in Archiviazione di Azure e Azure in generale.

Ogni estensione Azure aggiunge un set di funzionalità di Visual Studio Code che rende più semplice ed efficace lo sviluppo con i punti di integrazione di Azure.

## <a name="getting-vs-code-and-preparing-for-azure-development"></a>Ottenere Visual Studio Code e prepararsi per lo sviluppo per Azure

Esistono tre diverse piattaforme che supportano Visual Studio Code: Windows, macOS e Linux. Nonostante vengano tutte installate da un file scaricabile, la loro configurazione è diversa.

Per eseguire Visual Studio Code in Windows, scaricare il file per la propria versione di Windows, 32 bit o 64 bit, e installarlo come qualsiasi altra applicazione di Windows.

Per macOS, scaricare il file ed espandere il contenuto. È consigliabile aggiungere Visual Studio Code a LaunchPad e Dock.

Per Linux la procedura è un po' più complessa e dipende dalla distribuzione scelta. Si dovrà scaricare e installare Visual Studio Code in Debian e Ubuntu oppure usare il repository Yum in RHEL, Fedora, CentOS, openSUSE e SLE. Per altre distribuzioni, potrebbero esserci meno Community Edition supportate e disponibili.

Per preparare Visual Studio Code per lo sviluppo per Azure su qualsiasi piattaforma, usare il marketplace delle estensioni per installare le estensioni di Azure necessarie. Se si lavora con il servizio app, usare l'estensione del servizio app. Se si lavora con Node.js, procurarsi il pacchetto Node per Azure.

## <a name="summary"></a>Riepilogo

Visual Studio Code è un complemento perfetto per lo sviluppo e la creazione di applicazioni per Azure. Questo ambiente di sviluppo integrato semplice e multipiattaforma, associato a un'ampia gamma di estensioni per migliorare l'efficienza e l'affidabilità delle app, rende Visual Studio Code ideale per lo sviluppo per Azure.
