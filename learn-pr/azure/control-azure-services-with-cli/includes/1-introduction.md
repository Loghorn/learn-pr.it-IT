Il portale di Azure è ideale per l'esecuzione di singole attività e per avere una rapida panoramica dello stato delle risorse. Per le attività che devono essere ripetute ogni giorno, o anche ogni ora, l'uso della riga di comando e di un set di script o comandi testati può essere però utile per svolgere il proprio lavoro più rapidamente ed evitare errori. 

Si supponga di lavorare presso una società che sviluppa app Web di Azure. Queste sono applicazioni ospitate in Azure, con tutti i vantaggi della configurazione automatica di sicurezza, bilanciamento del carico, gestione e così via. Attualmente si sta testando un'app Web che genera previsioni di vendita, in base a vari input da diversi database e altre origini dati. Gli sviluppatori usano computer Windows, Linux e Mac e un repository di GitHub per le compilazioni giornaliere delle applicazioni. 

Come parte del test, si vogliono confrontare le prestazioni delle app per origini dati diverse e per tipi diversi connessioni dati. Si è notato che quando il team di sviluppo usa il portale di Azure per creare una nuova istanza di test dell'app, non sempre vengono usati esattamente gli stessi parametri. Si intende risolvere questo problema usando un set di comandi di distribuzione standard per ogni test di app, che possono essere automatizzati, se necessario, e che funzioneranno nello stesso modo in tutti i computer usati dal team del software.

In questo modulo, verrà illustrato come gestire le risorse di Azure tramite l'interfaccia della riga di comando di Azure. 

## <a name="learning-objectives"></a>Obiettivi di apprendimento
In questo modulo verrà descritto come:

- Installare l'interfaccia della riga di comando di Azure in Linux, macOS e/o Windows.
- Connettersi a una sottoscrizione di Azure tramite l'interfaccia della riga di comando di Azure.
- Creare risorse di Azure tramite l'interfaccia della riga di comando di Azure.

## <a name="prerequisites"></a>Prerequisiti
- Esperienza con un'interfaccia della riga di comando, ad esempio PowerShell o Bash
- Conoscenza dei concetti di base di Azure, ad esempio i gruppi di risorse
- Esperienza di amministrazione delle risorse di Azure tramite il portale di Azure