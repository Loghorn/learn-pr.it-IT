 In questa esercitazione verrà seguito lo scenario in cui una macchina per il caffè remota è connessa ad Azure IoT Central per il monitoraggio e la gestione dei problemi. È possibile monitorare i dati di telemetria, ad esempio la temperatura dell'acqua e l'umidità, lo stato della macchina, impostare la temperatura ottimale, ricevere lo stato della garanzia e inviare i comandi, ad esempio iniziare a preparare il caffè. Quando la temperatura dell'acqua della macchina per il caffè supera determinati valori di soglia mentre la macchina è coperta da garanzia, Microsoft Flow invia una notifica al dispositivo mobile del tecnico remoto. Allo stesso modo, se la temperatura dell'acqua non rientra nell'intervallo previsto e la garanzia è scaduta, viene inviato un messaggio di posta elettronica da IoT Central al reparto di manutenzione del cliente per un ulteriore intervento.

Per implementare lo scenario, è necessario creare un modello di dispositivo in Azure IoT Central, una soluzione SaaS, per definire le misure (telemetria e stato), le impostazioni, le proprietà e i comandi. La macchina per il caffè viene quindi connessa ad Azure IoT Central e vengono configurate le regole per le notifiche di manutenzione quando la temperatura dell'acqua non è compresa nell'intervallo ottimale.

![Flusso dei dati dalla macchina a IoT Central](../images/1-data-flow.png)

In questa esercitazione si apprenderà come:
> [!div class="checklist"]
> * Creare un modello di applicazione Azure IoT Central personalizzato
> * Creare e definire il modello di dispositivo
> * Connettere l'applicazione al simulatore della macchina per il caffè
> * Convalidare la connessione e il flusso di dati
> * Configurare le regole per le notifiche di manutenzione
 
## <a name="sign-in-to-azure-iot-central"></a>Accedere ad Azure IoT Central

Azure IoT Central è una soluzione SaaS (Software as a Service) completamente gestita che semplifica la connessione, il monitoraggio e la gestione degli asset IoT su vasta scala. In questo modulo si accede a IoT Central per creare una nuova applicazione personalizzata. È anche possibile scegliere di estendere la versione di valutazione per 30 giorni quando si effettua l'iscrizione per un account Azure. Una versione di valutazione di 30 giorni è un prerequisito per poter completare il modulo 5 quando si usa Microsoft Flow per inviare una notifica a un dispositivo mobile.

1. Passare alla pagina [Application Manager](https://aka.ms/iotcentral) (Gestione applicazioni) di Azure IoT Central. 

1. Immettere l'indirizzo di posta elettronica e la password usati per accedere all'account Microsoft.
![Accedere all'account Microsoft](../images/1-create-app-a.png)

## <a name="create-a-new-custom-application"></a>Creare una nuova applicazione personalizzata

1. Per creare una nuova applicazione Azure IoT Central, scegliere **New Application** (Nuova applicazione). 
![Creare un'applicazione IoT Central](../images/1-create-app-b.png)

1. Creare una nuova applicazione Azure IoT Central:
* Per il piano di pagamento, scegliere **Free** (Gratuito).
* Per il modello dell'applicazione, scegliere **Custom Application** (Applicazione personalizzata).
* Scegliere un nome descrittivo per l'applicazione, ad esempio **Coffee Maker 01**. 
* Azure IoT Central genera automaticamente un prefisso URL univoco. Scegliere **Create** (Crea).

![Scegliere il piano dell'applicazione IoT Central](../images/1-create-app-c.png)

* Estendere la versione di valutazione per 30 giorni è facoltativo, ma è un prerequisito se si vuole completare il modulo con l'integrazione di Microsoft Flow per attivare un'azione che invii le notifiche ai dispositivi mobili. Per estendere la versione di valutazione per 30 giorni, scegliere un'istanza di Azure Active Directory e una sottoscrizione di Azure da usare. Una sottoscrizione di Azure consente di creare istanze dei servizi di Azure. Azure IoT Central rileva automaticamente tutte le sottoscrizioni di Azure a cui è possibile accedere e le mostra in un elenco a discesa.
        
![Estendere la versione di valutazione](../images/1-create-app-d.png)
    
* Se non si ha una sottoscrizione di Azure, è possibile crearne una nella [pagina di iscrizione ad Azure](https://aka.ms/createazuresubscription). Dopo aver creato la sottoscrizione di Azure, tornare alla pagina **Create Application** (Crea applicazione). La nuova sottoscrizione nell'elenco a discesa **Azure Subscription** (Sottoscrizione di Azure).
        
![Iscrizione per una sottoscrizione](../images/1-create-app-e.png)

## <a name="summary"></a>Riepilogo

In questo modulo è stata creata un'applicazione Azure IoT personalizzata ed è stata effettuata l'iscrizione per una sottoscrizione di Azure. Nel modulo successivo si procederà a partire dal framework dell'applicazione appena creato. 