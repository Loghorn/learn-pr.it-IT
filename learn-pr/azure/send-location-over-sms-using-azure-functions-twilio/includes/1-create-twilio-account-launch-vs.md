> [!TIP]
> Il nome utente e la password necessari per accedere alla macchina virtuale si trovano nella scheda **Risorse**.

> [!NOTE]
> Se si usa un Mac, dopo che la macchina virtuale è stata avviata, per sbloccarla può essere necessario usare l'icona del fulmine sulla barra degli strumenti o l'opzione **Ctrl+Alt+Canc** nella scheda **Risorse** accanto alle istruzioni.


In questo modulo si creerà un'app Xamarin.Forms multipiattaforma con un back-end serverless. L'app otterrà dal dispositivo di un utente la posizione in cui si trova l'utente stesso e la invierà con un elenco di numeri di telefono a una funzione di Azure. La funzione userà quindi un'associazione a un servizio di terze parti (Twilio) per inviare la posizione in un messaggio SMS a tutti i numeri di telefono elencati.

Il processo include i passaggi seguenti:

1. L'app acquisisce la posizione usando Xamarin.Essentials come astrazione su API di posizione specifiche del dispositivo.

1. La posizione e i numeri di telefono vengono inseriti in un pacchetto in un payload JSON e inviati a una funzione di Azure.

1. La funzione di Azure decodifica il payload JSON e crea i messaggi SMS.

1. I messaggi SMS vengono inviati tramite [Twilio](https://www.twilio.com/?azure-portal=true).

L'illustrazione seguente mostra una panoramica di questo processo.

![Illustrazione che mostra un'architettura di alto livello del processo di condivisione percorso tramite messaggio di testo.](../media/1-architecture.png)

## <a name="create-a-twilio-account"></a>Creare un account Twilio

Per poter inviare messaggi SMS da una funzione di Azure, è necessario disporre di un account Twilio. La versione gratuita dell'account è più che sufficiente per iniziare.

1. Accedere a [twilio.com](https://www.twilio.com?azure-portal=true).

1. Fare clic sul pulsante rosso **Sign Up** (Registrazione) nell'angolo superiore destro.

1. Immettere i propri dati personali e fare clic su **Get Started** (Per iniziare).

1. Sarà necessario verificare il proprio numero di telefono. Gli account Twilio gratuiti consentono di inviare messaggi solo a numeri di telefono verificati al fine di evitare che vengano usati per inviare posta indesiderata. Twilio invierà un codice di verifica da immettere per verificare il telefono.

1. Selezionare la scheda **Products** (Prodotti), fare clic su **Programmable SMS** (SMS programmabile) e quindi su **Continue** (Continua).

1. Immettere un nome per il primo progetto, ad esempio "I'm here" e quindi fare clic su **Continue** (Continua).

1. Ignorare il passaggio relativo all'invito di un membro del team.

1. Dal dashboard di messaggistica di Twilio espandere il pannello **Project Info** (Informazioni progetto).

1. Prendere nota dei valori **ACCOUNT SID** e **AUTH TOKEN**, perché saranno necessari in seguito.

    Quando si crea un account Twilio, viene assegnato un numero di telefono da cui è possibile inviare messaggi. È possibile trovare questo numero di telefono nel dashboard **Numeri di telefono** di Twilio.

1. Dal sito di Twilio, selezionare i puntini di sospensione nella parte inferiore del menu a sinistra. Quindi, selezionare *SUPER NETWORK -> Numeri di telefono*. È possibile aggiungere questo dashboard al menu a sinistra mediante l'icona della puntina. Il numero Twilio si troverà in *Manage Numbers->Active Numbers* (Gestisci numeri -> Numeri attivi).

    ![Ricerca del numero Twilio](../media/7-twilio-find-number.png)

    > [!TIP]
    > Se non si ha ancora un numero attivo, selezionare **Get Started** (Inizia) nella pagina Active Numbers (Numeri attivi) per iniziare il processo di creazione di un numero.

1. Prendere nota del numero di telefono attivo. Verrà usato più avanti in questo modulo.


> [!NOTE]
> Nella fase di iscrizione verrà assegnato un numero di telefono Twilio che verrà usato per inviare i messaggi SMS. In alcuni paesi questi numeri potrebbero non essere in grado di inviare i messaggi SMS. Nella documentazione di Twilio sono elencati i [paesi che prevedono restrizioni](https://support.twilio.com/hc/articles/223183068-Twilio-international-phone-number-availability-and-their-capabilities?azure-portal=true)e vengono descritti i modi per inviare messaggi SMS con un [numero internazionale oppure un Id alfanumerico del mittente](https://support.twilio.com/hc/articles/226690868-Using-Twilio-when-SMS-numbers-are-unavailable-in-your-country?azure-portal=true).

## <a name="launch-visual-studio"></a>Avviare Visual Studio

Per questo modulo si svilupperà l'app per dispositivi mobili e l'app di Funzioni di Azure usando Visual Studio 2017, disponibile tramite una macchina virtuale. Sebbene sia possibile creare app Xamarin.Forms eseguibili in iOS, Android e Universal Windows Platform (UWP), questo modulo si concentrerà solo sulla piattaforma UWP, per consentirne il funzionamento nella macchina virtuale del lab.

Avviare Visual Studio 2017 dal menu Start della macchina virtuale o dal collegamento sul desktop.

## <a name="summary"></a>Riepilogo

In questa unità è stato creato un account Twilio da usare per inviare messaggi SMS ed è stato avviato Visual Studio. Nella prossima unità si apprenderà come creare un'app Xamarin.Forms e come aggiungere il pacchetto NuGet Xamarin.Essentials.

> [!IMPORTANT]
> Prendere nota dei valori **ACCOUNT SID** e **AUTH TOKEN** Twilio e del **numero di telefono attivo** presentati in questa unità, perché serviranno in seguito.
