 In questa esercitazione verrà seguito lo scenario in cui una macchina per il caffè remota è connessa ad Azure IoT Central per il monitoraggio e la gestione dei problemi. È possibile monitorare i dati di telemetria, ad esempio la temperatura dell'acqua e l'umidità, osservare lo stato della macchina, impostare la temperatura ottimale, ricevere lo stato della garanzia e inviare i comandi. Quando la temperatura dell'acqua della macchina per il caffè supera determinati valori di soglia mentre la macchina è coperta da garanzia, Microsoft Flow invia una notifica al dispositivo mobile del tecnico remoto. Allo stesso modo, se la temperatura dell'acqua non rientra nell'intervallo previsto e la garanzia è scaduta, viene inviato un messaggio di posta elettronica da IoT Central al reparto di manutenzione del cliente per un ulteriore intervento.

Per implementare lo scenario, è necessario creare un modello di dispositivo in Azure IoT Central per definire le misure (telemetria e stato), le impostazioni, le proprietà e i comandi. La macchina per il caffè viene quindi connessa ad Azure IoT Central e vengono configurate le regole per le notifiche di manutenzione quando la temperatura dell'acqua non è compresa nell'intervallo ottimale.

In questo modulo verrà descritto come:
- Creare un'applicazione Azure IoT Central personalizzata 
- Creare e definire il modello di dispositivo
- Connettere la macchina per il caffè all'applicazione
- Convalidare la connessione e il flusso di dati
- Configurare le regole per le notifiche di manutenzione
 
## <a name="sign-in-to-azure-iot-central"></a>Accedere ad Azure IoT Central
In questa unità si accede a IoT Central per creare una nuova applicazione personalizzata. Una versione di valutazione gratuita di 7 giorni è sufficiente per completare le unità d 1 a 4. Per completare l'esercizio facoltativo sull'uso di Microsoft Flow per inviare una notifica per dispositivi mobili nell'unità 5, è necessario estendere la versione di valutazione gratuita di IoT Central a 30 giorni. L'estensione è consentita se è disponibile una sottoscrizione di Azure.  

1. Passare alla pagina [Application Manager](https://aka.ms/iotcentral) (Gestione applicazioni) di Azure IoT Central. 

1. Nella pagina di accesso immettere l'indirizzo di posta elettronica e la password usati per accedere all'account Microsoft.

## <a name="create-a-new-custom-application"></a>Creare una nuova applicazione personalizzata

1. Per creare una nuova applicazione Azure IoT Central, scegliere **New Application** (Nuova applicazione). 

1. Nella pagina di creazione dell'applicazione: 
    * Per il piano di pagamento, scegliere **Free** (Gratuito)
    * Per il modello dell'applicazione, scegliere **Custom Application** (Applicazione personalizzata)
    * Scegliere un nome descrittivo per l'applicazione, ad esempio **Coffee Maker 01**
    * Azure IoT Central genera automaticamente un prefisso URL univoco. Scegliere **Create** (Crea)
    
   > [!NOTE]
   > Estendere la versione di valutazione per 30 giorni è facoltativo, ma è un prerequisito se si vuole completare l'esercizio sull'uso di Microsoft Flow per inviare una notifica per dispositivi mobili nell'unità 5. L'estensione a 30 giorni è consentita se è disponibile una sottoscrizione di Azure. Per istruzioni sull'abilitazione dell'estensione, vedere l'unità 5 sulla configurazione di regole e azioni per monitorare la macchina per il caffè.

In questa unità è stata creata un'applicazione Azure IoT personalizzata ed è stata effettuata l'iscrizione per una sottoscrizione di Azure. Nell'unità successiva si procederà a partire dal framework dell'applicazione appena creato. 