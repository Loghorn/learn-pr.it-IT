L'applicazione in fase di compilazione è un'app per dispositivi mobili multipiattaforma che comunica con una funzione di Azure per condividere il percorso. In questa unità si creerà l'app per dispositivi mobili vuota mediante Visual Studio e si installerà un pacchetto NuGet con un'API per ottenere la posizione dell'utente.

## <a name="create-the-xamarinforms-project"></a>Creare il progetto Xamarin.Forms

1. In Visual Studio selezionare *File->Nuovo->Progetto...*.

1. Dall'albero sul lato sinistro selezionare *Visual C#->Multipiattaforma* e quindi *App per dispositivi mobili (Xamarin.Forms)* dal pannello al centro.

1. Denominare la soluzione "ImHere".

1. Scegliere un percorso appropriato per la soluzione.

1. Fare clic su **OK**.

    ![Finestra di dialogo Nuova soluzione](../media/2-new-solution-dialog.png)

1. Nella finestra di dialogo **Nuova app multipiattaforma** selezionare il modello *App vuota*.

1. Per questo modulo verrà compilata un'app UWP: deselezionare iOS e Android e lasciare UWP selezionato.

1. Per *Strategia di condivisione codice* selezionare **.NET Standard**.

1. Fare clic su **OK**.

    ![Finestra di dialogo Configura la nuova soluzione](../media/2-configure-solution-dialog.png)

Visual Studio creerà due progetti: un'app UWP denominata `ImHere.UWP` e una libreria .NET Standard, `ImHere`. Le app Xamarin.Forms sono costituite da due parti: uno o più progetti di app specifici della piattaforma e una o più librerie .NET Standard. I progetti di app specifici della piattaforma contengono il codice specifico della piattaforma necessario per eseguire un'app nella piattaforma pertinente. Questi progetti avviano quindi un'app Xamarin.Forms definita in una libreria .NET Standard multipiattaforma. L'app viene compilata in codice multipiattaforma e, in fase di esecuzione, eventuali interfacce utente create verranno convertite nei componenti dell'interfaccia utente specifici della piattaforma pertinente.

## <a name="adding-xamarinessentials"></a>Aggiunta di Xamarin.Essentials

Le piattaforme iOS, Android e UWP forniscono numerose funzionalità simili che sfruttano il sistema operativo e l'hardware. Nonostante queste analogie, le API sono molto diverse. L'uso di queste API dal codice multipiattaforma richiede la scrittura di codice specifico della piattaforma nei progetti di app esposti alle librerie .NET Standard. [Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/?azure-portal=true) è un pacchetto NuGet che fornisce un'astrazione multipiattaforma per numerose API di questo tipo, pertanto non è necessario scrivere codice specifico della piattaforma. Ciò include le API di georilevazione API che verranno usate nell'app per ottenere la posizione dell'utente.

1. In Esplora soluzioni di Visual Studio fare clic con il pulsante destro del mouse sulla soluzione `ImHere` (la soluzione più in alto, non il progetto .NET Standard `ImHere`) e scegliere *Gestisci pacchetti NuGet per la soluzione...*.

1. Selezionare la scheda **Sfoglia** e cercare "Xamarin.Essentials". Questo pacchetto è attualmente disponibile come versione preliminare del pacchetto NuGet, quindi selezionare la casella *Includi versione preliminare*.

    > [!TIP]
    > Se non viene visualizzato il pacchetto Xamarin.Essentials NuGet, controllare che sia selezionata la casella *Includi versione preliminare*. 

1. Selezionare il pacchetto NuGet **Xamarin.Essentials**.

1. Selezionare tutti i progetti nell'elenco sul lato destro.

1. Fare clic sul pulsante **Installa** per installare il pacchetto NuGet. È necessario accettare le condizioni di licenza per continuare.

    ![Aggiunta del pacchetto NuGet Xamarin.Essentials a tutti i progetti nella soluzione](../media/2-add-essentials-nuget.png)

## <a name="building-and-running-the-app"></a>Compilazione ed esecuzione dell'app

1. Fare clic con il pulsante destro del mouse sul progetto `ImHere.UWP` in Esplora soluzioni, quindi scegliere *Imposta come progetto di avvio*.

1. Impostare la configurazione della compilazione su **Debug**, la piattaforma su **x86**e il dispositivo di esecuzione su **Computer locale**.

    ![Impostazione della configurazione di Debug x86 per l'esecuzione nel dispositivo locale](../media/2-debug-configuration.png)

1. Avviare il debug dell'app.

    ![L'app in esecuzione](../media/2-debuging-app.png)

## <a name="summary"></a>Riepilogo

In questa unità è stata creata una nuova app per dispositivi mobili multipiattaforma Xamarin.Forms ed è stato aggiunto il pacchetto NuGet Xamarin.Essentials. Verrà ora illustrato come creare l'interfaccia utente e la logica dell'app per dispositivi mobili.