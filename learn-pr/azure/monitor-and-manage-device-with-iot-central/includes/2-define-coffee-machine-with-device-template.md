In Azure IoT Central i dati che un dispositivo può scambiare con l'applicazione sono specificati in un modello di dispositivo che definisce il comportamento e le funzionalità di un dispositivo o, in questo caso, di una macchina per il caffè. Quando si crea un modello di dispositivo, viene generato un dispositivo simulato dal modello.

Il dispositivo simulato genera dati di telemetria che consentono di testare il comportamento dell'applicazione prima di collegare un dispositivo fisico/reale. 

In questa unità si creerà un modello di dispositivo per una macchina per il caffè che specifica le funzionalità e i comportamenti seguenti:

### <a name="measurements"></a>Misure

Le misure sono i dati provenienti dal dispositivo. È possibile aggiungere più misure al modello di dispositivo per replicare le funzionalità del dispositivo.

* **Misure di telemetria**: sono i punti dati numerici che il dispositivo raccoglie nel tempo, rappresentati come flusso continuo. In questo scenario le misure di telemetria sono l'umidità dell'aria e la temperatura dell'acqua. 

* **Misure di stato**: rappresentano lo stato del dispositivo o dei relativi componenti in un periodo di tempo. In questo scenario si impostano gli stati Brewing/Not Brewing and Cup Detected/Cup Not Detected.

### <a name="settings"></a>Impostazioni

Le impostazioni consentono di inviare i dati di configurazione dall'applicazione a un dispositivo. In questo scenario, nelle impostazioni si regola la temperatura ottimale dell'acqua e la si invia alla macchina per il caffè. Quando viene aggiornata, l'impostazione viene contrassegnata come in sospeso nell'interfaccia utente finché il dispositivo non riconosce la modifica.

### <a name="properties"></a>Proprietà

I metadati del dispositivo associati al dispositivo. Esistono due tipi di proprietà.

* Le *proprietà dell'applicazione* sono usate per registrare informazioni sul dispositivo nell'applicazione. In questo scenario si usano le proprietà dell'applicazione per impostare l'intervallo di temperatura dell'acqua ideale della macchina per il caffè. Le proprietà dell'applicazione vengono archiviate nell'applicazione e non sono sincronizzate con il dispositivo. 

* Le *proprietà dispositivo* sono usate per permettere a un dispositivo di inviare i valori delle proprietà all'applicazione. Queste proprietà possono essere modificate solo dal dispositivo. In questo scenario si configura la proprietà del dispositivo che in IoT Central è denominata Device Warranty Expired. Il campo Device Warranty Expired rimane vuoto fino a quando la macchina per il caffè non viene connessa a IoT Central. Dopo essere stata connessa, la macchina per il caffè invia lo stato della garanzia all'applicazione. 

### <a name="commands"></a>Comandi

Si usano i comandi per gestire il dispositivo in remoto dall'applicazione. Per controllare il dispositivo, è possibile eseguire i comandi direttamente sul dispositivo dal cloud. In questo scenario si eseguono i comandi sulla macchina per il caffè per impostarne la manutenzione o l'avvio della preparazione del caffè. 

## <a name="create-a-device-template-for-the-coffee-maker"></a>Creare un modello di dispositivo per la macchina per il caffè
Un modello di dispositivo definisce il comportamento e le funzionalità di un dispositivo o, in questo caso, di una macchina per il caffè.

1. Passare alla pagina **Homepage** dell'applicazione nel portale di Azure IoT Central, quindi scegliere **Create Device Templates** (Crea modelli di dispositivo).

> [!TIP]
> È possibile raggiungere la home page facendo clic sull'icona Home nel menu a sinistra.

1. Immettere *Connected Coffee Maker* per il modello di dispositivo personalizzato. 
 
1. Selezionare **Create** (Crea). È stato creato un modello di dispositivo vuoto per la macchina per il caffè, nel quale vengono definiti il comportamento e le funzionalità della macchina. 

## <a name="define-telemetry-measurements-of-temperature-and-humidity"></a>Definire le misure di telemetria per temperatura e umidità
1. Nel modello di dispositivo **Connected Coffee Maker** assicurarsi di essere nella pagina **Measurements** (Misure) in cui si definiscono i dati di telemetria. 

1. Per aggiungere la misura di telemetria per la temperatura, scegliere **Edit Template** (Modifica modello), poi **+ New Measurement** (+ Nuova misura). Scegliere quindi **Telemetry** (Telemetria) come tipo di misura.

1. Ogni tipo di dati di telemetria definito per un modello di dispositivo include opzioni di configurazione, ad esempio le seguenti:
    * Opzioni di visualizzazione
    * Dettagli dei dati di telemetria
    * Parametri di simulazione

    Per configurare i dati di telemetria per temperatura ed umidità, usare le informazioni riportate nella tabella seguente. Quando si creano gli elementi di telemetria, aggiungere una nuova misura scegliendo **+ New Measurement** (+ Nuova misura) per ogni elemento nella tabella.
    
    |Display name (Nome visualizzato)|Field name (Nome campo)|Units (Unità)|Min|Max|Decimal places (Cifre decimali)|
    |---|---|---|---|---|---|
    |Water Temperature (Temperatura dell'acqua)|waterTemperature|Celsius|86|100|1|
    |Umidità dell'aria|airHumidity|%|20|100|0|
   
    È anche possibile scegliere un colore per la visualizzazione dei dati di telemetria. Per salvare la definizione dei dati di telemetria, scegliere **Save** (Salva). Quando si creano più definizioni per misure, impostazioni, proprietà e comandi nell'unità rimanente, ricordarsi di salvare al termine di ogni operazione.  

    > [!NOTE]
    > Immettere nel modello di dispositivo i nomi dei campi esattamente come sono visualizzati nella tabella. Se i nomi dei campi non corrispondono ai nomi delle proprietà nel codice del dispositivo corrispondente, i dati di telemetria non possono essere visualizzati nell'applicazione. Procedere nello stesso modo quando si immettono impostazioni e proprietà.

    ![Creare un modello di dispositivo](../media/2-device-template-a.png)

## <a name="define-state-measurement-for-brewingnot-brewing-and-cup-detectedcup-not-detected"></a>Definire la misura di stato per Brewing/Not Brewing and Cup Detected/Cup Not Detected
Aggiungere gli stati seguenti nella pagina **Measurements** (Misure) scegliendo **Edit Template** (Modifica modello), poi **+ New Measurement** (+ Nuova misura). Quindi scegliere **State** (State) come tipo di misura:

Nella pagina **State** > **Brewing** aggiungere il valore come true. Aggiungere l'altro valore come false con il nome visualizzato facoltativo Not Brewing, facendo clic su **+** accanto a **Values** (Valori). Ricordarsi di selezionare **Save** (Salva) per salvare le modifiche.

   |Display Name (Nome visualizzato)|Nome campo|Valore 1|Nome visualizzato 1|Valore 2|Nome visualizzato 2|
   |---|---|---|---|---|---|
   |In preparazione|stateBrewing|true|In preparazione|false|Non in preparazione|
   |Tazza rilevata|stateCupDetected|true|Tazza rilevata|false|Cup Not Detected|

> [!NOTE]
> Dopo aver definito telemetria e stato, i dati simulati generati dal modello di dispositivo verranno visualizzati nella schermata del dispositivo. I dati simulati consentono di testare il comportamento dell'applicazione prima di connettere un dispositivo fisico a IoT Central. 

## <a name="set-the-optimal-temperature-of-the-coffee-machine"></a>Impostare la temperatura ottimale della macchina per il caffè
Passare alla pagina **Settings** (Impostazioni), vale a dire la scheda accanto a **Measurements** (Misure). Fare clic su **Edit Template** (Modifica modello). Aggiungere l'impostazione **Number** (Numero) seguente in **Library** (Raccolta) nella pagina **Settings** (Impostazioni):

|Display name (Nome visualizzato)|Field name (Nome campo)|Units (Unità)|Decimals|Min|Max|Initial|
|---|---|---|---|---|---|---|---|
|Temperatura ottimale|setTemperature|Celsius|1|86|100|95|

## <a name="use-properties-to-store-warranty-info-and-water-temperature-range"></a>Usare le proprietà per archiviare le informazioni sulla garanzia e l'intervallo di temperatura dell'acqua

Aggiungere le proprietà **Number** (Numero) seguenti nella pagina **Properties** (Proprietà) scegliendo **Edit Template** (Modifica modello):

|Display name (Nome visualizzato)|Field name (Nome campo)|Units (Unità)|Decimal places (Cifre decimali)|Min|Max|Initial (Iniziale)
|---|---|---|---|---|---|---|
|Coffee Maker's Min Temperature (Temperatura minima macchina per il caffè)|propertyMinTemperature|Celsius|1|88|92|90|
|Coffee Maker's Max Temperature (Temperatura massima macchina per il caffè)|propertyMaxTemperature|Celsius|1|96|99|98| 

Aggiungere la proprietà del dispositivo in **Device Property** (Proprietà dispositivo) seguente nella pagina **Properties** (Proprietà):

   |Display name (Nome visualizzato)|Field name (Nome campo)|Data type (Tipo di dati)|
   |---|---|---|
   |Device Warranty Expired (Garanzia dispositivo scaduta)|propertyWarrantyExpired|number (numero)|

> [!NOTE]
> La proprietà del dispositivo viene inviata dal dispositivo, in questo caso dalla macchina per il caffè. Dopo aver connesso la macchina per il caffè ad Azure IoT Central, la proprietà del dispositivo relativa alla garanzia viene inviata all'applicazione e visualizzata nel campo Device Warranty Expired. 

## <a name="use-commands-to-set-maintenance-mode-and-start-brewing"></a>Usare i comandi per impostare la modalità di manutenzione e avviare la preparazione del caffè

Aggiungere i comandi seguenti alla pagina **Commands** (Comandi) scegliendo **Edit Template** (Modifica modello).

|Display name (Nome visualizzato)|Field name (Nome campo)|Default Timeout (Timeout predefinito)|Data type (Tipo di dati)|
|---|---|---|---|---|---|---|
|Set Maintenance Mode (Imposta modalità di manutenzione)|cmdSetMaintenance|30|text| 
|Avvia preparazione|cmdStartBrewing|30|text (testo)|