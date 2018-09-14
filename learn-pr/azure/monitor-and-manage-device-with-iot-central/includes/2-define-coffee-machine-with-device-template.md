In Azure IoT Central, i dati che un dispositivo può scambiare con l'applicazione vengono specificati in un modello di dispositivo che definisce il comportamento e le funzionalità di un dispositivo o in questo caso, un machine caffè. Quando si crea un modello di dispositivo, viene generato un dispositivo simulato dal modello. Il dispositivo simulato genera dati di telemetria che ti permette di testare il comportamento dell'applicazione prima di un dispositivo fisico o real è connesso. 

In questa unità, creerai un modello di dispositivo per una macchina di caffè che specifica le funzionalità e i comportamenti seguenti:
* **Le misurazioni**: I dati provenienti dal dispositivo. È possibile aggiungere più misure al modello di dispositivo per replicare le funzionalità del dispositivo.
    * Le misurazioni di dati di telemetria: I punti dati numerici del dispositivo raccolti nel corso del tempo. Sono rappresentati come un flusso continuo. In questo scenario, le misurazioni di dati di telemetria sono air umidità e temperatura di acqua. 

    * Le misurazioni di stato: lo stato del dispositivo o i relativi componenti in un periodo di tempo. In questo scenario è impostare stati come della birra Brewing/non Cup rilevate/Cup non rilevato

* **Impostazioni**: si usano le impostazioni per inviare i dati di configurazione in un dispositivo dall'applicazione. In questo scenario è regolare la temperatura di acqua ottimale nelle impostazioni e inviarle per la macchina di caffè. Quando l'impostazione viene aggiornata, viene contrassegnato come in sospeso nell'interfaccia utente fino a quando il dispositivo riconosce che ha risposto alla modifica dell'impostazione.

* **Proprietà**: I metadati del dispositivo che ha associato al dispositivo. Esistono due tipi di proprietà.
    * Si utilizza *le proprietà dell'applicazione* per registrare le informazioni sul dispositivo nell'applicazione. In questo scenario, si usano le proprietà dell'applicazione per impostare l'intervallo di temperatura water ideale della macchina caffè. Le proprietà dell'applicazione vengono archiviate nell'applicazione e non vengono sincronizzati con il dispositivo. 

    * Le *proprietà dispositivo* sono usate per permettere a un dispositivo di inviare valori di proprietà all'applicazione. Queste proprietà possono essere modificate solo dal dispositivo. In questo scenario, si configura la proprietà dispositivo chiamato dispositivo garanzia scaduto in IoT Central. Il campo dispositivo garanzia scaduta rimane vuoto fino a quando non è connesso il machine caffè a IoT Central. Una volta connessi, la macchina coffee Invia lo stato della garanzia all'applicazione. 

* **I comandi**: si usano comandi per gestire in remoto il dispositivo dall'applicazione. È possibile eseguire i comandi direttamente sul dispositivo dal cloud per controllare i dispositivi. In questo scenario è eseguire i comandi nel computer per impostarlo per manutenzione o avviare della birra caffè. 

## <a name="create-a-device-template-for-the-coffee-maker"></a>Creare un modello di dispositivo per la macchina per il caffè
Un modello di dispositivo definisce il comportamento e le funzionalità di un dispositivo o in questo caso, un caffè.

1. Passare alla Home page e scegliere **creare modelli di dispositivo**.

1. Immettere *connessi caffé* per il modello di dispositivo personalizzato. 
 
1. Scegliere **Create** (Crea). Si è creato un modello di dispositivo vuoto per il caffé in cui si definiscono il comportamento e le funzionalità della macchina. 

## <a name="define-telemetry-measurement-temperature-and-humidity"></a>Definire le misure di telemetria per temperatura e umidità
1.  Nel modello di dispositivo **Connected Coffee Maker** assicurarsi di trovarsi nella pagina **Measurements** (Misure) in cui si definiscono i dati di telemetria. 

1.  Per aggiungere la misura della temperatura telemetria, scegliere **+ nuova misurazione**. Quindi scegliere **telemetria** come tipo di unità di misura.

1.  Ogni tipo di dati di telemetria definito per un modello di dispositivo include opzioni di configurazione, ad esempio:
    * Opzioni di visualizzazione.
    * Dettagli dei dati di telemetria.
    * Parametri di simulazione.

    Per configurare i dati di telemetria su temperatura e umidità, usare le informazioni nella tabella seguente. Quando si creano elementi di telemetria, è necessario aggiungere una nuova misura selezionando **+ nuova misurazione** per ogni elemento nella tabella.
    
    |Nome visualizzato|Nome campo|Unità|Min|Max|Cifre decimali|
    |---|---|---|---|---|---|
    |Temperatura dell'acqua|waterTemperature|Celsius|86|100|1|
    |Umidità dell'aria|airHumidity|%|20|100|0|
   
    È anche possibile scegliere un colore per la visualizzazione dei dati di telemetria. Per salvare la definizione di dati di telemetria, scegliere **salvare**. Quando si creano più definizioni per le misurazioni, impostazioni, le proprietà e i comandi nell'unità di rimanenti, ricordarsi di salvare ogni volta che si è finito.  
    
    ![Creare un modello di dispositivo](../images/2-device-template-a.png)

    Immettere i nomi di campo esattamente come indicato nella tabella nel modello di dispositivo. Se i nomi delle proprietà nel codice del dispositivo corrispondente non corrispondono ai nomi dei campi, i dati di telemetria non può essere visualizzata nell'applicazione. Procedere nello stesso modo quando si inseriscono le impostazioni e le informazioni sulle proprietà. 

## <a name="define-state-measurement-for-brewingnot-brewing-cup-detectedcup-not-detected"></a>Definire la misura di stato per In preparazione/Non in preparazione, Tazza rilevata/Tazza non rilevata
Aggiungere gli stati seguenti nel **misurazioni** pagina scegliendo **+ nuova misurazione**. Quindi scegliere **Stato** come tipo di misura:
    
   |Nome visualizzato|Nome campo|Valore 1|Nome visualizzato 1|Valore 2|Nome visualizzato 2|
   |---|---|---|---|---|---|
   |In preparazione|stateBrewing|true|In preparazione|false|Non in preparazione|
   |Tazza rilevata|stateCupDetected|true|Tazza rilevata|false|Tazza non rilevata|


Lo stato > della birra pagina, si aggiunge il valore come true. Aggiungere l'altro valore false con il nome visualizzato facoltativo come non della birra facendo **+** accanto a **valori**.

> [!NOTE]
> Dopo aver definito i dati di telemetria e lo stato, vengono visualizzati i dati simulati generati dal modello di dispositivo nella schermata del dispositivo. I dati simulati consentono di testare il comportamento dell'applicazione prima di connettere un dispositivo fisico in IoT Central. 

## <a name="use-settings-to-set-the-optimal-temperature-of-the-coffee-machine"></a>Usare Settings (Impostazioni) per impostare la temperatura ottimale della macchina per il caffè.
Passare alla pagina delle impostazioni della scheda accanto alle misurazioni. Attivare **Design Mode** (Modalità progettazione). Aggiungere il codice seguente **numero** impostazione sotto **libreria** sul **impostazioni** pagina:

|Nome visualizzato|Nome campo|Unità|Decimali|Min|Max|Iniziale|
|---|---|---|---|---|---|---|---|
|Temperatura ottimale|setTemperature|Celsius|1|86|100|95|

## <a name="use-properties-to-store-warranty-info-and-water-temperature-range"></a>Usare le proprietà per memorizzare le informazioni di garanzia e l'intervallo di temperatura dell'acqua

Aggiungere il codice seguente **numero** proprietà di **proprietà** pagina attivando prima **modalità progettazione**:

|Nome visualizzato|Nome campo|Unità|Cifre decimali|Min|Max|Iniziale
|---|---|---|---|---|---|---|
|Temperatura minima macchina per il caffè|propertyMinTemperature|Celsius|1|88|92|90|
|Temperatura massima macchina per il caffè|propertyMaxTemperature|Celsius|1|96|99|98| 

Aggiungere la **proprietà del dispositivo** seguente nella pagina **Properties** (Proprietà):

   |Nome visualizzato|Nome campo|Tipo di dati|
   |---|---|---|
   |Garanzia dispositivo scaduta|propertyWarrantyExpired|numero|

> [!NOTE]
> La proprietà del dispositivo viene inviata dal dispositivo, in questo caso dalla macchina per il caffè. Dopo aver collegato la macchina per il caffè ad Azure IoT Central, la proprietà del dispositivo relativa alla garanzia viene inviata all'applicazione e visualizzata nel campo Garanzia dispositivo scaduta. 

## <a name="use-commands-to-set-maintenance-mode-and-start-brewing"></a>Usare i comandi per impostare la modalità di manutenzione e avviare la preparazione

Aggiungere i comandi seguenti alla pagina **Commands** (Comandi) attivando prima **Design Mode** (Modalità progettazione).

|Nome visualizzato|Nome campo|Timeout predefinito|Tipo di dati|
|---|---|---|---|---|---|---|
|Impostare la modalità di manutenzione|cmdSetMaintenance|30|testo| 
|Avvia preparazione|cmdStartBrewing|30|testo|

## <a name="summary"></a>Riepilogo

In questa unità, è stato creato un nuovo tipo di dispositivo, una macchina di caffè, usando il modello di dispositivo. Nel modello di dispositivo, i dati in grado di scambiare un machine caffè è specificato con l'applicazione. Sono stati definiti dati di telemetria, come temperatura e umidità, e lo stato, ad esempio se il caffè è in fase di preparazione o meno. È definito ulteriormente il comportamento e le funzionalità della macchina caffè configurando le impostazioni, proprietà e i comandi. 

