 In questa esercitazione si seguire lo scenario in cui un computer remoto caffè sia connesso a Azure IoT Central per il monitoraggio e gestione dei problemi. È possibile monitorare i dati di telemetria, ad esempio acqua temperatura e umidità, osservare lo stato della macchina, impostare temperatura ottima, ricevere lo stato della garanzia e inviare comandi. Quando la temperatura dell'acqua della macchina per il caffè supera determinati valori di soglia mentre la macchina è coperta da garanzia, Microsoft Flow invia una notifica al dispositivo mobile del tecnico remoto. Allo stesso modo, se la temperatura dell'acqua non rientra nell'intervallo previsto e la garanzia è scaduta, viene inviato un messaggio di posta elettronica da IoT Central al reparto di manutenzione del cliente per un ulteriore intervento.

Per implementare lo scenario, inizia creando un modello di dispositivo in Azure IoT Central per definire le misure (dati di telemetria e stato), le impostazioni, le proprietà e i comandi. La macchina per il caffè viene quindi connessa ad Azure IoT Central e vengono configurate le regole per le notifiche di manutenzione quando la temperatura dell'acqua non è compresa nell'intervallo ottimale.

In questo modulo, sarà possibile a:
- Creare un'applicazione personalizzata di Azure IoT Central 
- Creare e definire il modello di dispositivo
- Connettere la macchina di caffè all'applicazione
- Convalidare la connessione e il flusso di dati
- Configurare le regole per le notifiche di manutenzione
 
## <a name="sign-in-to-azure-iot-central"></a>Accedere ad Azure IoT Central
In questa unità, accedi a IoT Central per creare una nuova applicazione personalizzata. Una versione di valutazione di 7 giorni è sufficiente per completare le unità da 1 a 4. Se si desidera completare l'esercizio facoltativo sull'uso di Microsoft Flow per inviare una notifica sul dispositivo mobile di 5 unità, è necessario estendere la versione di valutazione IoT Central per 30 giorni. L'estensione viene abilitata se si dispone di una sottoscrizione di Azure.  

1. Passare alla pagina [Application Manager](https://aka.ms/iotcentral) (Gestione applicazioni) di Azure IoT Central. 

1. Nella pagina di accesso, immettere l'indirizzo di posta elettronica e password usati per accedere all'account Microsoft.

## <a name="create-a-new-custom-application"></a>Creare una nuova applicazione personalizzata

1. Per creare una nuova applicazione Azure IoT Central, scegliere **nuova applicazione**. 

1. Nella pagina Crea un'applicazione: 
    * Scegli **gratuito** per il piano di pagamento
    * Selezionare **dell'applicazione personalizzata** come il modello di applicazione
    * Scegliere un nome descrittivo dell'applicazione, ad esempio **caffé 01**
    * Azure IoT Central genera un prefisso URL univoco per l'utente seleziona **Create**
    
   > [!NOTE]
   > Estendere la versione di valutazione per 30 giorni è facoltativo, ma è un prerequisito se si desidera svolgere l'esercizio disponibile sull'uso di Microsoft Flow per inviare una notifica sul dispositivo mobile di 5 unità. Se si dispone di una sottoscrizione di Azure, è abilitata l'estensione di 30 giorni. Per istruzioni sull'abilitazione dell'estensione, vedere la sezione 5 unità nella configurazione delle regole e azioni da monitorare nel computer di caffè.

In questa unità, viene creata un'applicazione personalizzata di Azure IoT. ed è stata effettuata l'iscrizione per una sottoscrizione di Azure. Nell'unità successiva, si continuerà compilare in framework dell'applicazione che è stato creato. 