In Azure IoT Central i dati che un dispositivo può scambiare con l'applicazione sono specificati in un modello di dispositivo che definisce il comportamento e la capacità di un dispositivo o, in questo caso, di una macchina per il caffè. Quando si crea un modello di dispositivo, viene generato un dispositivo simulato dal modello. Il dispositivo simulato genera dati di telemetria che consentono di testare il comportamento dell'applicazione prima di collegare un dispositivo fisico. Più avanti nel modulo successivo si collegherà un'applicazione Node.js generica che rappresenta una macchina per il caffè fisica. 

In questo modulo si crea un modello di dispositivo per una macchina per il caffè che specifica le capacità e i comportamenti seguenti:
* Misurazioni 
    * Dati di telemetria: umidità dell'aria, temperatura dell'acqua
    * Stato: In preparazione/Non in preparazione, Tazza rilevata/Tazza non rilevata
* Impostazioni: temperatura ottimale
* Proprietà: temperatura minima e massima della macchina per il caffè, garanzia del dispositivo scaduta.
* Comandi: Imposta modalità di manutenzione, Avvia preparazione


## <a name="create-a-device-template-for-the-coffee-maker"></a>Creare un modello di dispositivo per la macchina per il caffè
Un modello di dispositivo che definisce il comportamento e la capacità di un dispositivo o, in questo caso, di una macchina per il caffè.

1. Passare alla home page e scegliere Create Device Templates (Crea modelli di dispositivo).
![Creare un modello di dispositivo](../images/2-device-template-a1.png)

1. Immettere un nome per il modello di dispositivo personalizzato. In questo modulo immettere ad esempio Connected Coffee Maker.
![Creare un modello di dispositivo](../images/2-device-template-a2.png)
 
1. È stato creato un modello di dispositivo vuoto per la macchina per il caffè, nel quale verranno definiti il comportamento e le capacità della macchina. 

## <a name="define-telemetry-measurement-temperature-and-humidity"></a>Definire le misure di telemetria per temperatura e umidità
1.  Nel modello di dispositivo **Connected Coffee Maker** assicurarsi di trovarsi nella pagina **Measurements** (Misure) in cui si definiscono i dati di telemetria. Ogni modello di dispositivo che viene definito ha pagine separate in cui è possibile:
    * Specificare le misure, ad esempio dati di telemetria, evento e stato, inviate dal dispositivo.
    * Definire le impostazioni usate per controllare il dispositivo.
    * Definire le proprietà usate per registrare le informazioni sul dispositivo e le proprietà del dispositivo inviate dal dispositivo.
    * Definire le regole associate al dispositivo.
    * Personalizzare il dashboard del dispositivo per visualizzare le informazioni relative al dispositivo. 

1.  Per aggiungere la misura di telemetria della temperatura, scegliere **New Measurement** (Nuova misura). Scegliere quindi **Telemetry** (Telemetria) come tipo di misura: ![Creare un modello di dispositivo](../images/2-device-template-c.png)

1.  Ogni tipo di dati di telemetria definito per un modello di dispositivo include opzioni di configurazione, ad esempio:
    * Opzioni di visualizzazione.
    * Dettagli dei dati di telemetria.
    * Parametri di simulazione.

    Per configurare i dati di telemetria di temperatura e umidità, usare le informazioni nella tabella seguente:
    
    |Nome visualizzato|Nome campo|Unità|Min|Max|Cifre decimali|
    |---|---|---|---|---|---|
    |Temperatura dell'acqua|waterTemperature|Celsius|90|98|1|
    |Umidità dell'aria|airHumidity|%|20|100|0|
   
    È anche possibile scegliere un colore per la visualizzazione dei dati di telemetria. Per salvare la definizione della telemetria, scegliere **Save** (Salva): ![Creare un modello di dispositivo](../images/2-device-template-d.png)

    Immettere nel modello di dispositivo i nomi dei campi esattamente come sono visualizzati nella tabella. Se i nomi dei campi non corrispondono, i dati di telemetria non possono essere visualizzati nell'applicazione. Procedere nello stesso modo quando si inseriscono le impostazioni e le informazioni sulle proprietà. 

## <a name="define-state-measurement-for-brewingnot-brewing-cup-detectedcup-not-detected"></a>Definire la misura di stato per In preparazione/Non in preparazione, Tazza rilevata/Tazza non rilevata
Aggiungere gli stati seguenti nella pagina **Measurements** (Misure) scegliendo **New Measurement** (Nuova misura). Quindi scegliere **Stato** come tipo di misura:
    
|Nome visualizzato|Nome campo|Valore 1|Nome visualizzato|Valore 2|Nome visualizzato|
|---|---|---|---|---|---|
|In preparazione|stateBrewing|true|Non in preparazione|false|Non in preparazione|
|Tazza rilevata|stateCupDetected|true|Tazza non rilevata|false|Tazza non rilevata|

![Creare un modello di dispositivo](../images/2-device-template-f.png)

Quando si crea un modello di dispositivo, viene generato un dispositivo simulato dal modello. Il dispositivo simulato genera dati di telemetria che consentono di testare il comportamento dell'applicazione prima di collegare un dispositivo fisico.
![Creare un modello di dispositivo](../images/2-device-template-m.png)

## <a name="use-settings-to-set-the-optimal-temperature-of-the-coffee-machine"></a>Usare Settings (Impostazioni) per impostare la temperatura ottimale della macchina per il caffè.
Passare a Settings (Impostazioni). Attivare **Design Mode** (Modalità progettazione). Aggiungere le impostazioni numeriche seguenti nella pagina **Settings** (Impostazioni):

|Nome visualizzato|Nome campo|Units|Decimals|Min|Max|Initial|
|---|---|---|---|---|---|---|---|
|Temperatura ottimale|setTemperature|Celsius|1|92|99|95|

![Creare un modello di dispositivo](../images/2-device-template-q.png)

## <a name="use-properties-to-store-warranty-info-and-water-temperature-range"></a>Usare le proprietà per memorizzare le informazioni di garanzia e l'intervallo di temperatura dell'acqua
Aggiungere la proprietà **Number** seguente alla pagina **Properties** (Proprietà) attivando prima **Design Mode** (Modalità progettazione):
|Nome visualizzato|Nome campo|Unità|Cifre decimali|Min|Max|Initial
|---|---|---|---|---|---|---|
|Temperatura minima macchina per il caffè|propertyMinTemperature|Celsius|1|88|92|90|    
|Temperatura massima macchina per il caffè|propertyMaxTemperature|Celsius|1|96|99|98| 

![Creare un modello di dispositivo](../images/2-device-template-n.png)

Aggiungere la **proprietà del dispositivo** seguente nella pagina **Properties** (Proprietà):

|Nome visualizzato|Nome campo|Data Type|
|---|---|---|
|Garanzia dispositivo scaduta|propertyWarrantyExpired|numero|
![Creare un modello di dispositivo](../images/2-device-template-u.png)

> [!NOTE]
> La proprietà del dispositivo viene inviata dal dispositivo, in questo caso dalla macchina per il caffè. Dopo aver connesso la macchina per il caffè ad Azure IoT Central, la proprietà del dispositivo relativa alla garanzia viene inviata all'applicazione e visualizzata nel campo Garanzia dispositivo scaduta. 

## <a name="use-commands-to-set-maintenance-mode-and-start-brewing"></a>Usare i comandi per impostare la modalità di manutenzione e avviare la preparazione

Aggiungere i comandi seguenti alla pagina **Commands** (Comandi) attivando prima **Design Mode** (Modalità progettazione).
|Nome visualizzato|Nome campo|Campi di input|Tipo di dati|
|---|---|---|---|---|---|---|
|Imposta manutenzione|cmdSetMaintance|30|text| 
|Avvia preparazione|cmdStartBrewing|30|testo|

![Creare un modello di dispositivo](../images/2-device-template-s.png)


## <a name="summary"></a>Riepilogo

In questo modulo è stato creato un nuovo tipo di dispositivo, ovvero una macchina per il caffè, usando il modello di dispositivo per specificare i dati che una macchina per il caffè può scambiare con l'applicazione. Sono stati definiti dati di telemetria, come temperatura e umidità, e lo stato, ad esempio se il caffè è in fase di preparazione o meno. Sono stati anche definiti il comportamento e la capacità della macchina per il caffè configurando le impostazioni, le proprietà e i comandi. 

