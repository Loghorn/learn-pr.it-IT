Non possiamo passeremo un'immagine dell'emoji all'API viso per ottenere relative emozioni perché, anche perché non umano. In modo che per ogni emoji, avevo bisogno di un proxy risorse umane, me.

Ho fornito le immagini di uso personale _accuratamente_ , che rispecchia ogni emoji e usato la _punto emotivo_ per quell'immagine come proxy per l'emoji. Per semplificare le operazioni interessanti anche scelto persone del mio team e li associato emoji anche, come illustrato di seguito:

![Team Moji](/media-drafts/team.jpg)

È possibile visualizzare l'elenco di immagini di proxy per ogni emoji nel `bin/proxy-images` cartella nel codice di esempio associato a questa esercitazione.

## <a name="goal"></a>Obiettivo

In questo capitolo è generare le chiavi di autenticazione necessari in modo che è possibile usare l'API viso di Azure e quindi usare l'API viso per calibrare tutti gli emoji usando immagini con proxy dell'utente corrente.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

- Come generare API le chiavi SSH per l'uso con servizi cognitivi.
- Come eseguire le immagini tramite l'API viso ed estrarre le informazioni delle emozioni.

## <a name="generate-an-azure-face-api-key"></a>Generare una chiave API viso di Azure

Per usare l'API viso di Azure, è necessaria una chiave di autenticazione.

Il modo più rapido per ottenere una chiave consiste nel passare a https://azure.microsoft.com/try/cognitive-services/?api=face-api e l'iscrizione alla versione di valutazione l'API viso.

Dopo l'iscrizione è in questo caso sarà poche informazioni necessarie per l'archiviazione per in un secondo momento.

1. Scarica la _endpoint_. Dovrebbe essere simile https://westcentralus.api.cognitive.microsoft.com/face/v1.0

2. Mostra le due chiavi API, archivio uno di essi per l'utilizzo in un secondo momento (non importa quale)

## <a name="setup-the-environment-variables"></a>Configurare le variabili di ambiente

Lo script di calibrazione deve conoscere l'URL dell'API viso e la chiave per eseguire le chiamate corrette, anziché impostare come hardcoded questi nello script che si intende usare le variabili di ambiente, eseguire questi comandi nel terminale si prevede di eseguire l'applicazione:

```bash
export FACE_API_URL=<the-face-api-url>
export FACE_API_KEY=<your-face-api-key>
```

Viene inoltre usato un pacchetto denominato `dotenv` nella nostra applicazione di nodo. Questo pacchetto è possibile usare archiviare le variabili di ambiente in locale in un file denominato `.env`. Il `dotenv` pacchetto Carica le variabili in questo file e li presenta come variabili di ambiente per l'applicazione.

> **NOTA**
>
> Non archivia `.env` file nel controllo del codice sorgente.

Funzioni di Azure dispongono di un altro metodo per la gestione di variabili di ambiente tramite le `local.settings.json` file, più avanti.

## <a name="create-some-proxy-images-for-emojis"></a>Creare alcune immagini di proxy per emoji

Ho fornito tutte le immagini di proxy Me, ma è possibile generare il proprio.

Per ogni emoji nel `bin/proxy-images` cartella, acquisire un'immagine di se stessi, che rispecchia tale emoji e sostituire l'immagine con l'immagine.

## <a name="try-it-out"></a>Provare il servizio

Ora viene il bello parte! Si userà per eseguire ognuna delle immagini nel `bin/proxy-images` tramite l'API viso per calcolare un punto emotivo per tale emoji nel _spazio emotivo_, eseguire:

```bash
node bin/calibrate.js
```

L'output di questo comando avrà un aspetto come segue:

```json
...
Processing 🤓
Processing 🤔
Processing 🦄
Processing 😃
Processing 😆
...
{
    emotiveValues: new EmotivePoint({
        anger: 0,
        contempt: 0.005,
        disgust: 0.001,
        fear: 0,
        happiness: 0,
        neutral: 0.922,
        sadness: 0.071,
        surprise: 0
    }),
    emojiIcon: "😴"
}
```

Stampato prima di tutto l'emoji è l'elaborazione e quindi infine stampare sulla console una matrice che definisce il `EmotivePoint` di tutte l'emoji. Questo è lo stesso formato di matrice in `shared/mojis.ts`.

Se sono state modificate alcune delle immagini con proxy, quindi copiare l'output di questo script per la relativa sezione del `mojis.ts`
