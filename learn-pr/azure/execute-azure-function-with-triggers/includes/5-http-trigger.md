Una richiesta HTTP è un'operazione comune sulla maggior parte delle piattaforme e dei dispositivi. Indipendentemente se la richiesta riguarda la ricerca di una parola in un dizionario o informazioni sulle previsioni meteo locali, vengono continuamente inviate richieste HTTP. Funzioni di Azure consente di creare rapidamente una parte di logica da eseguire quando viene ricevuta una richiesta HTTP.

In questo caso, si apprenderà come creare e richiamare una funzione di Azure tramite un trigger HTTP. Verranno inoltre illustrate alcune delle opzioni di personalizzazione disponibili.

## <a name="what-is-an-http-trigger"></a>Cos'è un trigger HTTP?

Un trigger HTTP è un trigger che esegue una funzione quando viene ricevuta una richiesta HTTP. I trigger HTTP presentano molte funzionalità e personalizzazioni, tra cui:

- Fornire accesso autorizzato fornendo le chiavi.
- Limitare i verbi HTTP supportati.
- Restituire i dati al chiamante.
- Ricevere i dati tramite i parametri della stringa di query o tramite il corpo della richiesta.
- Supportare i modelli di route dell'URL per modificare l'URL della funzione.

Quando si crea un trigger HTTP, selezionare un linguaggio di programmazione, fornire un nome trigger e selezionare un livello di autorizzazione.

## <a name="what-is-an-http-trigger-authorization-level"></a>Cos'è un livello di autorizzazione del trigger HTTP?

Un livello di autorizzazione del trigger HTTP è un flag che indica se una richiesta HTTP in ingresso necessita di una chiave API per motivi di autenticazione.

Esistono tre livelli di autorizzazione:

1. Funzione
2. Anonimo
3. Admin

I livelli **Funzione** e **Admin** sono basati sulla "chiave". Per inviare una richiesta HTTP, è necessario fornire una chiave per l'autenticazione. Esistono due tipi di chiavi: *funzione* e *host*. Le differenze tra le due chiavi sono a livello di ambito. Le chiavi *funzione* sono specifiche di una funzione. Le chiavi *host* si applicano a tutte le funzioni all'interno dell'intera applicazione di funzioni di Azure. Se il livello di autorizzazione è impostato su **Funzione**, è possibile usare una chiave *funzione* o *host*. Se il livello di autorizzazione è impostato su **Admin**, è necessario fornire una chiave *host*.

Il livello **Anonimo** indica che non è richiesta alcuna autenticazione. Nell'esercizio viene usato questo livello.

## <a name="how-to-create-an-http-trigger"></a>Come creare un trigger HTTP

Proprio come un trigger timer, è possibile creare un trigger HTTP tramite il portale di Azure. All'interno della funzione di Azure, selezionare **Trigger HTTP** dall'elenco dei tipi di trigger predefiniti. Quindi immettere la logica da eseguire e apportare le eventuali personalizzazioni, ad esempio limitando l'uso di alcuni verbi HTTP.

Un'impostazione importante da comprendere è **Richiedi nome parametro**. Questa impostazione è una stringa che rappresenta il nome del parametro che contiene le informazioni relative a una richiesta HTTP in ingresso. Per impostazione predefinita, il nome del parametro è *req*.

## <a name="how-to-invoke-an-http-trigger"></a>Come richiamare un trigger HTTP

Per richiamare un trigger HTTP, inviare una richiesta HTTP all'URL per la funzione. Per ottenere questo URL, passare alla tabella codici per la funzione e selezionare il collegamento **Ottieni URL della funzione**.

![Screenshot del portale di Azure che mostra un pannello App per le funzioni con il pulsante dell'app Get funzione URL evidenziato.](../media/5-function-url.png)

Dopo aver ottenuto l'URL per la funzione, è possibile inviare richieste HTTP. Se la funzione riceve i dati, tenere presente che è possibile usare i parametri della stringa di query o fornire i dati tramite il corpo della richiesta.

Un trigger HTTP richiama una funzione di Azure quando viene ricevuta una richiesta HTTP per l'URL della funzione. I trigger HTTP consentono di ricevere dati e di restituirli al chiamante.