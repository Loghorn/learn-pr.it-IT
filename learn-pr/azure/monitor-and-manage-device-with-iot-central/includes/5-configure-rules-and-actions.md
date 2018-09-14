Si è connessa la macchina per il caffè all'applicazione Azure IoT Central, abilitando lo scambio di dati che consente di monitorare e gestire la macchina per il caffè. In questa unità, vengono create regole che attivano azioni quando la temperatura di acqua del machine caffè è compreso nell'intervallo normale. Le azioni sono messaggi di posta elettronica o notifiche a dispositivi mobili, a seconda che la macchina sia o meno coperta da garanzia. Per aggiungere Microsoft Flow come azione, è necessaria una sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, l'aggiunta di Microsoft Flow come azione è facoltativa.

## <a name="create-rules-in-iot-central-with-email-as-the-action"></a>Creare regole in IoT Central con invio di messaggio di posta elettronica come azione
Azure IoT Central include funzionalità native di posta elettronica per l'invio di notifiche. In questo scenario, se la macchina per il caffè è fuori dall'intervallo di temperatura ottimale e non è coperta dalla garanzia, IoT Central invia un messaggio di posta elettronica al reparto di manutenzione del cliente.

Passare il **regole** pagina per gli esercizi in questa unità. Selezionare **+ nuova regola**, quindi **telemetria**. Aggiungere le due regole seguenti quando la garanzia della macchina per il caffè è scaduta e la temperatura dell'acqua non è compresa nell'intervallo ottimale. Al termine, scegliere **Save** (Salva). 

> [!NOTE]
> Quando vengono applicate condizioni, per l'esecuzione delle regole tutte le affermazioni devono essere vere. Se la condizione è un'istruzione "o" come in questo scenario (ad esempio, la temperatura ottima è minore o maggiore dei valori predefiniti durante la garanzia è scaduto), dividere l'istruzione in due regole come illustrato di seguito.

1. Assegnare alla regola il nome: Coffee Maker Water Too Cold (Expired)

    Aggiungere le condizioni:      
    * Device Warranty Expired equals 1
    * Temperatura Water è minore di temperatura Min caffé

    ![Uso di regole](../images/5-flow-a.png)

1. Assegnare alla regola il nome: Coffee Maker Water Too Hot (Expired)

    Aggiungere le condizioni:      
    * Device Warranty Expired equals 1
    * Water temperatura è superiore a caffé Max Temperature

1. Per aggiungere un **azione**, scorrere verso il basso nel pannello Configura regola i dati di telemetria e scegliere **+** accanto alle azioni, quindi scegliere **indirizzo di posta elettronica**.

1. Per definire l'azione, aggiungere l'indirizzo di posta elettronica usato per accedere all'applicazione IoT Central. Aggiungere il messaggio di notifica quando la temperatura dell'acqua è troppo alta: "Coffee maker's water is too hot. Maintenance is required.  Warranty has expired." Ripetere gli stessi passaggi per quando la temperatura dell'acqua è troppo fredda. Aggiungere il messaggio: "Coffee maker's water is too cold. Maintenance is required.  Warranty has expired."

1. Scegliere **Save** (Salva). La regola è elencata nella pagina regole.

1. Per attivare la regola, impostare la temperatura ottima nelle impostazioni compreso nell'intervallo specificato nella sezione delle proprietà. Dopo aver completato la convalida, disattivare le regole per evitare il flooding posta in arrivo con messaggi di posta elettronica. 

## <a name="create-rules-in-iot-central-with-microsoft-flow-as-the-action"></a>Creare regole in IoT Central con Microsoft Flow come azione

Microsoft Flow automatizza i flussi di lavoro tra molte applicazioni. È una delle azioni che possono essere attivate quando viene eseguita una regola in IoT Central. In questo scenario, Microsoft Flow invia una notifica sul dispositivo mobile di un tecnico locale quando la macchina per il caffè raggiunge una determinata soglia di temperatura ed è in garanzia. Passare a **Rules** (Regole) per configurare le condizioni e aggiungere un flusso come azione quando la regola viene attivata. 
 
> [!NOTE]
> Questo esercizio è facoltativo se non si ha una sottoscrizione di Azure per attivare Microsoft Flow.


### <a name="extend-your-iot-central-trial-to-30-days"></a>Estendere la versione di valutazione IoT Central per 30 giorni

1. Per attivare Microsoft Flow, è necessario estendere la versione di valutazione per 30 giorni. A tale scopo, selezionare **estendere versione di valutazione per 30 giorni** nella pagina fatturazione, scegliere una Azure Active Directory e sottoscrizione di Azure. Una sottoscrizione di Azure consente di creare istanze dei servizi di Azure. Azure IoT Central rileva automaticamente tutte le sottoscrizioni di Azure a cui è possibile accedere e le mostra in un elenco a discesa.
    
1. Se non si ha una sottoscrizione di Azure, è possibile crearne una nella [pagina di iscrizione ad Azure](https://aka.ms/createazuresubscription). Dopo aver creato la sottoscrizione di Azure, passare al **Gestione applicazioni** pagina. La nuova sottoscrizione nell'elenco a discesa **Azure Subscription** (Sottoscrizione di Azure).
        

### <a name="add-the-following-rules-when-the-coffee-machine-is-under-warranty"></a>Aggiungere le regole seguenti quando la macchina per il caffè è in garanzia. 

1. Assegnare alla regola il nome: Coffee Maker Water Too Cold (Warranty)

    Aggiungere le condizioni:      
    * Device Warranty Expired equals 0
    * Temperatura Water è minore di temperatura Min caffé

1. Assegnare alla regola il nome: Coffee Maker Water Too Hot (Warranty)

    Aggiungere le condizioni:      
    * Device Warranty Expired equals 0
    * Water temperatura è superiore a caffé Max Temperature

1. Dopo aver salvato le condizioni della regola, scegliere Microsoft Flow come nuova azione quando la macchina per il caffè è in garanzia. Nel browser si aprirà una nuova scheda o finestra che indirizzerà a Microsoft Flow. Si aprirà una pagina di panoramica con un connettore IoT Central connesso a un'azione personalizzata. Scegliere **Continue** (Continua). 

    Viene visualizzata la finestra di progettazione di Microsoft Flow in cui creare il flusso di lavoro. Il flusso di lavoro include un trigger di IoT Central in cui l'applicazione e la regola sono già inserite.

    A questo punto è possibile aggiungere qualsiasi azione che si desidera al flusso di lavoro. Ad esempio, è possibile inviare una notifica per dispositivo mobile. Cercare notification (notifica), quindi scegliere Notifications - Send me a mobile notification (Notifiche - Inviami una notifica sul dispositivo mobile).

    Compilare il campo Text (Testo) dell'azione con ciò che la notifica deve comunicare. È possibile includere contenuto dinamico proveniente dalla regola di IoT Central per trasmettere informazioni importanti, quali l'ID del dispositivo e il timestamp.
    
    ![Uso di Microsoft Flow come azione](../images/5-flow-b.png)

1. Dopo aver configurato il flusso di lavoro in Microsoft Flow, scaricare l'[app Flow](https://www.microsoft.com/en-us/p/microsoft-flow/9nkn0p5l9n84?activetab=pivot%3aoverviewtab) da Microsoft Store sul dispositivo mobile. Accedere con lo stesso account usato per configurare il flusso nell'app Web Flow. A scopo di test, impostare la temperatura ottimale compreso nell'intervallo per attivare la regola. 

    > [!NOTE]
    > La proprietà del dispositivo Device Warranty Expired (1 per scaduta o 0 per coperto da garanzia nel codice del dispositivo) viene generata in modo casuale dal dispositivo e quindi inviata dal dispositivo all'applicazione Azure IoT Central. Per scopi di test, se desidera controllo dispositivo garanzia scaduto (1 o 0), riavviare il computer di caffè, fino a quando non viene visualizzato lo stato desiderato di garanzia per attivare l'azione che si sta testando. Per ricevere una notifica sul dispositivo mobile, riavviare il computer di caffè fino a quando non si noterà che dispositivo garanzia scaduto è 0 nel log della console. 

    Dopo alcuni minuti, le notifiche vengono visualizzate nell'app per dispositivi mobili Microsoft Flow.

    ![Uso di Microsoft Flow come azione](../images/5-flow-c.png)

## <a name="summary"></a>Riepilogo
Si è appreso come creare regole in IoT Central e attivare azioni quando la regola viene eseguita, ad esempio l'invio di un messaggio di posta elettronica o una di notifica per dispositivi mobili tramite Microsoft Flow. In questo caso, quando la temperatura di acqua della macchina coffee è compreso nell'intervallo ottima, le notifiche vengono inviate a un tecnico specializzato o il client a seconda dello stato della garanzia. 


