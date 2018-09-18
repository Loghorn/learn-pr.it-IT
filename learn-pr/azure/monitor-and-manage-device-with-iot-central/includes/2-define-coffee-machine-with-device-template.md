In Azure IoT Central i dati che un dispositivo può scambiare con l'applicazione sono specificati in un modello di dispositivo che definisce il comportamento e le funzionalità di un dispositivo o, in questo caso, di una macchina per il caffè. Quando si crea un modello di dispositivo, viene generato un dispositivo simulato dal modello. Il dispositivo simulato genera dati di telemetria che consentono di testare il comportamento dell'applicazione prima di collegare un dispositivo fisico/reale. 

In questa unità si creerà un modello di dispositivo per una macchina per il caffè che specifica le funzionalità e i comportamenti seguenti:
* **Measurements** (Misure): dati provenienti dal dispositivo. È possibile aggiungere più misure al modello di dispositivo per replicare le funzionalità del dispositivo.
    * Misure di telemetria: punti dati numerici che il dispositivo raccoglie nel tempo, rappresentati come un flusso continuo. In questo scenario le misure di telemetria sono l'umidità dell'aria e la temperatura dell'acqua. 

    * Misure di stato: rappresentano lo stato del dispositivo o dei relativi componenti nell'arco di un periodo di tempo. In questo scenario, si impostano gli stati come In preparazione/Non in preparazione, Tazza rilevata/Tazza non rilevata

* **Settings** (Impostazioni): le impostazioni sono usate per inviare dati di configurazione dall'applicazione a un dispositivo. In questo scenario, si regola la temperatura dell'acqua ottimale nelle impostazioni e la si invia alla macchina per il caffè. Quando si aggiorna un'impostazione, questa viene contrassegnata come in sospeso nell'interfaccia utente finché il dispositivo non riconosce la modifica.

* **Properties** (Proprietà): metadati del dispositivo associati al dispositivo. Sono disponibili due tipi di proprietà.
    * Le *proprietà dell'applicazione* sono usate per registrare informazioni sul dispositivo nell'applicazione. In questo scenario si usano le proprietà dell'applicazione per impostare l'intervallo di temperatura dell'acqua ideale della macchina per il caffè. Le proprietà dell'applicazione vengono archiviate nell'applicazione e non sono sincronizzate con il dispositivo. 

    * Le *proprietà dispositivo* sono usate per permettere a un dispositivo di inviare i valori delle proprietà all'applicazione. Queste proprietà possono essere modificate solo dal dispositivo. In questo scenario si configura la proprietà del dispositivo denominata Garanzia dispositivo scaduta in IoT Central. Il campo Garanzia dispositivo scaduta rimane vuoto fino a quando la macchina per il caffè non viene connessa a IoT Central. Dopo la connessione, la macchina per il caffè invia lo stato della garanzia all'applicazione. 

* **Commands** (Comandi): i comandi consentono di gestire il dispositivo in modalità remota dall'applicazione. È possibile eseguire i comandi direttamente sul dispositivo dal cloud per controllare i dispositivi. In questo scenario si eseguono i comandi sulla macchina per il caffè per impostarla sulla manutenzione o sull'avvio della preparazione. 

## <a name="create-a-device-template-for-the-coffee-maker"></a>Creare un modello di dispositivo per la macchina per il caffè
Un modello di dispositivo che definisce il comportamento e le funzionalità di un dispositivo o, in questo caso, di una macchina per il caffè.

1. Passare alla home page e scegliere **Create Device Templates** (Crea modelli di dispositivo).

1. Immettere *Connected Coffee Maker* per il modello di dispositivo personalizzato. 
 
1. Scegliere **Create** (Crea). È stato creato un modello di dispositivo vuoto per la macchina per il caffè, nel quale verranno definiti il comportamento e le funzionalità della macchina. 

## <a name="define-telemetry-measurement-temperature-and-humidity"></a>Definire le misure di telemetria per temperatura e umidità
1.  Nel modello di dispositivo **Connected Coffee Maker** assicurarsi di trovarsi nella pagina **Measurements** (Misure) in cui si definiscono i dati di telemetria. 

1.  Per aggiungere la misura di telemetria della temperatura, scegliere **+ New Measurement** (+ Nuova misura). Quindi scegliere **Telemetry** (Telemetria) come tipo di misura.

1.  Ogni tipo di dati di telemetria definito per un modello di dispositivo include opzioni di configurazione, ad esempio:
    * Opzioni di visualizzazione.
    * Dettagli dei dati di telemetria.
    * Parametri di simulazione.

    Per configurare i dati di telemetria di temperatura e umidità, usare le informazioni nella tabella seguente. Quando si creano gli elementi di telemetria, aggiungere una nuova misura scegliendo **+ New Measurement** (+ Nuova misura) per ogni elemento nella tabella.
    
    |Nome visualizzato|Nome campo|Unità|Min|Max|Cifre decimali|
    |---|---|---|---|---|---|
    |Temperatura dell'acqua|waterTemperature|Celsius|86|100|1|
    |Umidità dell'aria|airHumidity|%|20|100|0|
   
    È anche possibile scegliere un colore per la visualizzazione dei dati di telemetria. Per salvare la definizione dei dati di telemetria, scegliere **Save** (Salva). Quando si creano più definizioni per le misure, le impostazioni, le proprietà e i comandi nell'unità rimanente, ricordare di salvare al termine di ogni operazione.  
    
    ![Creare un modello di dispositivo](../images/2-device-template-a.png)

    Immettere nel modello di dispositivo i nomi dei campi esattamente come sono visualizzati nella tabella. Se i nomi dei campi non corrispondono ai nomi delle proprietà nel codice del dispositivo corrispondente, i dati di telemetria non possono essere visualizzati nell'applicazione. Procedere nello stesso modo quando si immettono le impostazioni e le informazioni sulle proprietà. 

## <a name="define-state-measurement-for-brewingnot-brewing-cup-detectedcup-not-detected"></a>Definire la misura di stato per In preparazione/Non in preparazione, Tazza rilevata/Tazza non rilevata
Aggiungere gli stati seguenti nella pagina **Measurements** (Misure) scegliendo **+ New Measurement** (+ Nuova misura). Quindi scegliere **State** (State) come tipo di misura:
    
   |Nome visualizzato|Nome campo|Valore 1|Nome visualizzato 1|Valore 2|Nome visualizzato 2|
   |---|---|---|---|---|---|
   |In preparazione|stateBrewing|true|In preparazione|false|Non in preparazione|
   |Tazza rilevata|stateCupDetected|true|Tazza rilevata|false|Tazza non rilevata|


Nella pagina State (Stato) > In preparazione aggiungere il valore come true. Aggiungere l'altro valore come false con il nome visualizzato facoltativo Non in preparazione, facendo clic su **+** accanto a **Values** (Valori).

> [!NOTE]
> Dopo aver definito telemetria e stato, i dati simulati generati dal modello di dispositivo verranno visualizzati nella schermata del dispositivo. I dati simulati consentono di testare il comportamento dell'applicazione prima di connettere un dispositivo fisico a IoT Central. 

## <a name="use-settings-to-set-the-optimal-temperature-of-the-coffee-machine"></a>Usare Settings (Impostazioni) per impostare la temperatura ottimale della macchina per il caffè
Passare alla pagina Settings (Impostazioni) e alla scheda accanto a Measurements (Misure). Attivare **Design Mode** (Modalità progettazione). Aggiungere l'impostazione **Number** (Numero) seguente in **Library** (Raccolta) nella pagina **Settings** (Impostazioni):

|Nome visualizzato|Nome campo|Units|Decimals|Min|Max|Initial|
|---|---|---|---|---|---|---|---|
|Temperatura ottimale|setTemperature|Celsius|1|86|100|95|

## <a name="use-properties-to-store-warranty-info-and-water-temperature-range"></a>Usare le proprietà per archiviare le informazioni sulla garanzia e l'intervallo di temperatura dell'acqua

Aggiungere le proprietà **Number** seguenti alla pagina **Properties** (Proprietà) attivando prima **Design Mode** (Modalità progettazione):

|Nome visualizzato|Nome campo|Units|Decimal Places|Min|Max|Initial
|---|---|---|---|---|---|---|
|Temperatura minima macchina per il caffè|propertyMinTemperature|Celsius|1|88|92|90|
|Temperatura massima macchina per il caffè|propertyMaxTemperature|Celsius|1|96|99|98| 

Aggiungere la **proprietà del dispositivo** seguente nella pagina **Properties** (Proprietà):

   |Nome visualizzato|Nome campo|Data Type|
   |---|---|---|
   |Garanzia dispositivo scaduta|propertyWarrantyExpired|number|

> [!NOTE]
> La proprietà del dispositivo viene inviata dal dispositivo, in questo caso dalla macchina per il caffè. Dopo aver connesso la macchina per il caffè ad Azure IoT Central, la proprietà del dispositivo relativa alla garanzia viene inviata all'applicazione e visualizzata nel campo Garanzia dispositivo scaduta. 

## <a name="use-commands-to-set-maintenance-mode-and-start-brewing"></a>Usare i comandi per impostare la modalità di manutenzione e avviare la preparazione

Aggiungere i comandi seguenti alla pagina **Commands** (Comandi) attivando prima **Design Mode** (Modalità progettazione).

|Nome visualizzato|Nome campo|Default Timeout|Data Type|
|---|---|---|---|---|---|---|
|Imposta modalità di manutenzione|cmdSetMaintenance|30|text| 
|Avvia preparazione|cmdStartBrewing|30|text|

## <a name="summary"></a>Riepilogo

In questa unità è stato creato un nuovo tipo di dispositivo, una macchine per il caffè, usando il modello di dispositivo. Nel modello di dispositivo sono stati specificati i dati che una macchina per il caffè può scambiare con l'applicazione. Sono stati definiti dati di telemetria, come temperatura e umidità, e lo stato, ad esempio se il caffè è in fase di preparazione o meno. Sono stati anche definiti il comportamento e le funzionalità della macchina per il caffè configurando le impostazioni, le proprietà e i comandi. 

