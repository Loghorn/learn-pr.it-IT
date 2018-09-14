Si è connessa la macchina per il caffè all'applicazione Azure IoT Central, abilitando lo scambio di dati che consente di monitorare e gestire la macchina per il caffè. In questo modulo vengono create regole che attivano azioni quando la temperatura dell'acqua della macchina per il caffè è fuori dall'intervallo normale. Le azioni sono messaggi di posta elettronica o notifiche a dispositivi mobili, a seconda che la macchina sia o meno coperta da garanzia. Per aggiungere Microsoft Flow come azione, è necessaria una sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, l'aggiunta di Microsoft Flow come azione è facoltativa.

## <a name="create-rules-in-iot-central-with-email-as-the-action"></a>Creare regole in IoT Central con invio di messaggio di posta elettronica come azione
Azure IoT Central include funzionalità native di posta elettronica per l'invio di notifiche. In questo scenario, se la macchina per il caffè è fuori dall'intervallo di temperatura ottimale e non è coperta dalla garanzia, IoT Central invia un messaggio di posta elettronica al reparto di manutenzione del cliente.

Aggiungere le due regole seguenti quando la garanzia della macchina per il caffè è scaduta e la temperatura dell'acqua non è compresa nell'intervallo ottimale. Al termine, scegliere **Save** (Salva). 

> [!NOTE]
> Quando vengono applicate condizioni, per l'esecuzione delle regole tutte le affermazioni devono essere vere. Se la condizione è un'istruzione "or" come in questo scenario (ad esempio, la temperatura ottimale è minore di 90 gradi Celsius o maggiore di 98 gradi Celsius quando la garanzia è scaduta), dividere l'istruzione in due regole come illustrato di seguito.

1. Assegnare alla regola il nome: Coffee Maker Water Too Cold (Expired)

    Aggiungere le condizioni:      
    * Device Warranty Expired equals 1
    * Water Temperature is less than Coffee Makers Min Temperature ![Uso della regola](../images/5-flow-g.png)

1. Assegnare alla regola il nome: Coffee Maker Water Too Hot (Expired)

    Aggiungere le condizioni:      
    * Device Warranty Expired equals 1
    * Water Temperature is greater than Coffee Makers Max Temperature ![Uso della regola](../images/5-flow-k.png)     

1. Per aggiungere un'**azione**, scorrere verso il basso fino al pannello Configure Telemetry Rule (Configura regola di telemetria) e scegliere l'icona + accanto ad Actions (Azioni), quindi scegliere **Email** (Messaggio di posta elettronica)

1. Per definire l'azione, aggiungere l'indirizzo di posta elettronica usato per accedere all'applicazione IoT Central. Aggiungere il messaggio di notifica quando la temperatura dell'acqua è troppo alta: "Coffee maker's water is too hot. Maintenance is required.  Warranty has expired." Ripetere gli stessi passaggi per quando la temperatura dell'acqua è troppo fredda. Aggiungere il messaggio: "Coffee maker's water is too cold. Maintenance is required.  Warranty has expired."

1. Scegliere **Save** (Salva). La regola viene elencata nella pagina corrispondente:

1. Per attivare la regola, è possibile attendere che la temperatura esca dall'intervallo oppure impostarla intenzionalmente fuori dall'intervallo ottimale nelle impostazioni. Quando si inizia a ricevere notifiche tramite posta elettronica, ricordare di disabilitare la regola per evitare che la Posta in arrivo sia inondata da messaggi di Azure IoT Central. 

> [!NOTE]
> Disattivare regole dopo aver completato la convalida per evitare di ricevere troppi messaggi nella Posta in arrivo. 

## <a name="create-rules-in-iot-central-with-microsoft-flow-as-the-action"></a>Creare regole in IoT Central con Microsoft Flow come azione

Microsoft Flow automatizza i flussi di lavoro tra molte applicazioni. È una delle azioni che possono essere attivate quando viene eseguita una regola in IoT Central. In questo scenario, Microsoft Flow invia una notifica sul dispositivo mobile di un tecnico locale quando la macchina per il caffè raggiunge una determinata soglia di temperatura ed è in garanzia. Passare a **Rules** (Regole) per configurare le condizioni e aggiungere un flusso come azione quando la regola viene attivata. 
 
> [!NOTE]
> Questo esercizio è facoltativo se non si ha una sottoscrizione di Azure per attivare Microsoft Flow.

Aggiungere le regole seguenti quando la macchina per il caffè è in garanzia. 

1. Assegnare alla regola il nome: Coffee Maker Water Too Cold (Warranty)

    Aggiungere le condizioni:      
    * Device Warranty Expired equals 0
    * Water Temperature is less than Coffee Makers Min Temperature ![Uso della regola](../images/5-flow-l.png)

1. Assegnare alla regola il nome: Coffee Maker Water Too Hot (Warranty)

    Aggiungere le condizioni:      
    * Device Warranty Expired equals 0
    * Water Temperature is greater than Coffee Makers Max Temperature ![Uso della regola](../images/5-flow-h.png)

1. Dopo aver salvato le condizioni della regola, scegliere Microsoft Flow come nuova azione quando la macchina per il caffè è in garanzia. Nel browser si aprirà una nuova scheda o finestra che indirizzerà a Microsoft Flow. Si aprirà una pagina di panoramica con un connettore IoT Central connesso a un'azione personalizzata. Scegliere **Continue** (Continua). 

    Viene visualizzata la finestra di progettazione di Microsoft Flow in cui creare il flusso di lavoro. Il flusso di lavoro include un trigger di IoT Central in cui l'applicazione e la regola sono già inserite.

    A questo punto è possibile aggiungere qualsiasi azione che si desidera al flusso di lavoro. Ad esempio, è possibile inviare una notifica per dispositivo mobile. Cercare notification (notifica), quindi scegliere Notifications - Send me a mobile notification (Notifiche - Inviami una notifica sul dispositivo mobile).

    Compilare il campo Text (Testo) dell'azione con ciò che la notifica deve comunicare. È possibile includere contenuto dinamico proveniente dalla regola di IoT Central per trasmettere informazioni importanti, quali l'ID del dispositivo e il timestamp.
    ![Uso di Microsoft Flow come azione](../images/5-flow-f.png)

1. Dopo aver configurato il flusso di lavoro in Microsoft Flow, scaricare l'[app Flow](https://www.microsoft.com/en-us/p/microsoft-flow/9nkn0p5l9n84?activetab=pivot%3aoverviewtab) da Microsoft Store sul dispositivo mobile. Accedere con lo stesso account usato per configurare il flusso nell'app Web Flow. A scopo di test, impostare la temperatura ottimale fuori dall'intervallo per attivare la regola. 

    > [!NOTE]
    > La proprietà del dispositivo Device Warranty Expired (1 per scaduta o 0 per coperto da garanzia nel codice del dispositivo) viene generata in modo casuale dal dispositivo e quindi inviata dal dispositivo all'applicazione Azure IoT Central. A scopo di testing, se si vuole controllare la proprietà Device Warranty Expired (1 o 0), riavviare il simulatore finché non viene visualizzato lo stato di garanzia desiderato per attivare l'azione che si sta testando. Per ricevere una notifica sul dispositivo mobile, riavviare il simulatore finché nel log della console non viene visualizzato che Device Warranty Expired è 0. 

    Dopo alcuni minuti, la notifica compare nell'app per dispositivi mobili Microsoft Flow.

    ![Uso di Microsoft Flow come azione](../images/5-flow-a.png)

## <a name="summary"></a>Riepilogo
Si è appreso come creare regole in IoT Central e attivare azioni quando la regola viene eseguita, ad esempio l'invio di un messaggio di posta elettronica o una di notifica per dispositivi mobili tramite Microsoft Flow. In questo caso, quando la temperatura dell'acqua della macchina per il caffè non è compresa nell'intervallo ottimale, vengono inviate notifiche a un tecnico specializzato o al cliente, a seconda dello stato della garanzia. 

