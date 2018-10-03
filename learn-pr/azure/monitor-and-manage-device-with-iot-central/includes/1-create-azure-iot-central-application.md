Azure IoT Central è una soluzione Internet delle cose (IoT) completamente gestita che semplifica la connessione, il monitoraggio e la gestione degli asset IoT globali.

Si seguirà qui uno scenario in cui una macchina per il caffè remota viene connessa ad Azure IoT Central per consentire il monitoraggio e la gestione dei problemi. È possibile monitorare i dati di telemetria, ad esempio la temperatura dell'acqua e l'umidità, osservare lo stato della macchina, impostare la temperatura ottimale, ricevere lo stato della garanzia e inviare i comandi. Se la temperatura dell'acqua non rientra nell'intervallo previsto e la garanzia è scaduta, viene inviato un messaggio di posta elettronica da IoT Central al reparto di manutenzione del client per un intervento.

Si creerà prima di tutto un dispositivo in Azure IoT Central che definisca i dati e i comandi che possono essere scambiati con il dispositivo IoT.

In questo modulo verrà descritto come:
  - Creare un'applicazione Azure IoT Central personalizzata
  - Creare e definire il modello di dispositivo
  - Connettere il simulatore di una macchina per il caffè all'applicazione in Azure IoT Central
  - Convalidare la connessione e il flusso di dati
  - Configurare le regole per le notifiche di manutenzione
 
## <a name="sign-in-to-azure-iot-central"></a>Accedere ad Azure IoT Central
In questa unità si accede a IoT Central per creare una nuova applicazione personalizzata. Per completare questo modulo, è sufficiente una versione di valutazione gratuita di 7 giorni. 

1. Passare alla pagina [Application Manager](https://aka.ms/iotcentral?azure-portal=true) (Gestione applicazioni) di Azure IoT Central. 

1. Nella pagina di accesso immettere l'indirizzo di posta elettronica e la password usati per accedere all'account Microsoft.

## <a name="create-a-new-custom-application"></a>Creare una nuova applicazione personalizzata

1. Per creare una nuova applicazione Azure IoT Central, scegliere **New Application** (Nuova applicazione). 

1. Nella pagina **Create Application** (Crea applicazione): 
    * Per il piano di pagamento, scegliere **Free** (Gratuito)
    * Per il modello dell'applicazione, scegliere **Custom Application** (Applicazione personalizzata)
    * Scegliere un nome descrittivo per l'applicazione, ad esempio **Coffee Maker 01-A**
    * Facoltativamente, modificare l'URL. Questa operazione è obbligatoria se il nome selezionato è già in uso
    * Scegliere **Create** (Crea)
