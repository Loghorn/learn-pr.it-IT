Il server è pronto per l'elaborazione dei dati video. Resta solo da aprire le porte che verranno usate dalle telecamere del traffico per caricare i file video nel server.

## <a name="create-a-network-security-group"></a>Creare un gruppo di sicurezza di rete

Azure dovrebbe aver creato un gruppo di sicurezza perché si era indicato di volere l'accesso Desktop remoto. Verrà tuttavia creato un nuovo gruppo di sicurezza per poter eseguire l'intero processo. Questo è particolarmente importante se si decide di creare la rete virtuale _prima_ delle macchine virtuali. Come indicato in precedenza, i gruppi di sicurezza sono _facoltativi_ e non vengono necessariamente creati con la rete.

> [!NOTE]
> Poiché si _presume_ che questa sia la seconda macchina virtuale, sarà già disponibile un gruppo di sicurezza da applicare alla rete, ma si supponga per un momento che non sia così o che le regole per questa macchina virtuale siano diverse.

1. Nel [portale di Azure](https://portal.azure.com?azure-portal=true) fare clic sul pulsante **Crea una risorsa** nella barra laterale nell'angolo a sinistra per avviare la creazione di una nuova risorsa.

1. Digitare "Gruppo di sicurezza di rete" nella casella del filtro e selezionare l'elemento corrispondente nell'elenco.

1. Verificare che sia selezionato il modello di distribuzione **Resource Manager** e fare clic su **Crea**.

1. Fornire un **nome** per il gruppo di sicurezza. Anche in questo caso, sono utili le convenzioni di denominazione. Si userà "test-vp-nsg2" per "Test Video Processor Network Security Group #2".

1. Selezionare appropriate **abbonamento** e utilizzare le proprie **gruppo di risorse**, <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>.

1. Inserirlo infine nella stessa **località** della macchina virtuale/rete virtuale. Questa operazione è importante perché non sarà possibile applicare questa risorsa se si trova in una località diversa.

1. Fare clic su **Crea** per creare il gruppo.

## <a name="add-a-new-inbound-rule-to-our-network-security-group"></a>Aggiungere una nuova regola in ingresso al gruppo di sicurezza di rete

La distribuzione verrà completata rapidamente.

1. Trovare la nuova risorsa gruppo di sicurezza e selezionarla nel portale di Azure.

1. Nella pagina Panoramica si noterà che sono state create alcune regole predefinite per bloccare la rete.

    Sul lato in ingresso:

    - Tutto il traffico in ingresso da una rete virtuale a un'altra è consentito. In questo modo le risorse nella rete virtuale possono comunicare tra di loro.
    - Il "probe" di Azure Load Balancer richiede di verificare che la macchina virtuale sia attiva
    - Tutto il traffico in ingresso viene rifiutato.
    Sul lato in uscita:
    - Tutto il traffico all'interno della rete virtuale è consentito.
    - Tutto il traffico in uscita verso Internet è consentito.
    - Tutto il traffico in uscita di altro tipo viene rifiutato.

> [!NOTE]
> Queste regole predefinite vengono impostate con valori di priorità elevata e quindi vengono valutate per _ultime_. Non possono essere modificate o eliminate, ma è possibile eseguirne l'_override_ creando regole più specifiche per il traffico con un valore di priorità più basso.

1. Fare clic sulla sezione **Regole di sicurezza in ingresso** nel pannello **Impostazioni** del gruppo di sicurezza.

1. Fare clic su **+ Aggiungi** per aggiungere una nuova regola di sicurezza.

    ![Screenshot che mostra la finestra di dialogo "Aggiungi un regola di sicurezza".](../media/8-add-rule.png)

    Esistono due modi per immettere le informazioni necessarie per una regola di sicurezza: di base e avanzato. È possibile passare da uno all'altro facendo clic sul pulsante nella parte superiore del pannello "Aggiungi".

    ![Una coppia di screenshot del portale di Azure che mostra l'alternanza tra Basic e Advanced regola di input, con una freccia che collega tra i due stati dell'interruttore evidenziato.](../media/8-advanced-create-rule.png)

    La modalità avanzata offre la possibilità di personalizzare completamente la regola. Se tuttavia è sufficiente configurare un protocollo noto, è più semplice usare la modalità di base.

1. Passare alla modalità di base.

1. Aggiungere le informazioni per la regola FTP.

    - Impostare il **servizio** su FTP. Verrà automaticamente configurato l'intervallo di porte.
    - Impostare la **priorità** su "1000". Deve essere un numero più basso di quello della regola predefinita **Nega**. È possibile far iniziare l'intervallo con qualsiasi valore, ma è consigliabile fare in modo di poter creare un'eccezione in un secondo momento, se necessario.
    - Assegnare un nome alla regola, ad esempio "traffic-cam-ftp-upload-2".
    - Assegnare una descrizione alla regola.

1. Tornare alla modalità **avanzata**. Si noti che le impostazioni sono ancora presenti. È possibile usare questo pannello per creare impostazioni con granularità più fine. In particolare, l'**origine** deve essere un indirizzo IP specifico o un intervallo di indirizzi IP specifici delle telecamere. Se si conosce l'indirizzo IP corrente del computer locale, è possibile provare a usarlo. In caso contrario, lasciare l'impostazione "Qualsiasi" in modo che sia possibile testare la regola.

1. Fare clic su **Aggiungi** per creare la regola. L'elenco di regole in ingresso verrà aggiornato. Si noti che sono in ordine di priorità, ovvero l'ordine in cui verranno esaminate.

## <a name="apply-the-security-group"></a>Applicare il gruppo di sicurezza

Si ricordi che è possibile applicare il gruppo di sicurezza a un'interfaccia di rete per proteggere una singola macchina virtuale o a una subnet per applicarlo a tutte le risorse in tale subnet. Si userà il secondo approccio, che è il più comune. È stato possibile ottenere per questa risorsa in Azure tramite la risorsa rete virtuale o indirettamente tramite la macchina virtuale, che usa la rete virtuale.

1. Passare al pannello **Panoramica** per la macchina virtuale. È possibile trovare la macchina virtuale in **Tutte le risorse**.

1. Selezionare l'elemento **Rete** nella sezione **Impostazioni**.

    ![Screenshot che mostra i dettagli dell'elemento di rete nelle impostazioni della VM con il gruppo di sicurezza di rete applicato.](../media/8-network-settings.png)

1. Nelle proprietà della rete sono disponibili informazioni sulla rete applicata alla macchina virtuale, tra cui la **rete virtuale/subnet**. Questo è un collegamento su cui è possibile fare clic per ottenere la risorsa. Fare clic per aprire la rete virtuale. Questo collegamento è disponibile _anche_ nel pannello **Panoramica** della macchina virtuale. Entrambi i collegamenti apriranno la **Panoramica** della rete virtuale.

1. Nella sezione **Impostazioni** selezionare l'elemento **Subnet**.

1. Dovrebbe essere presente una sola subnet definita (predefinita) da quando prima sono state create la macchina virtuale e la rete. Fare clic sull'elemento nell'elenco per aprire i dettagli.

1. Fare clic sulla voce **Gruppo di sicurezza di rete**.

1. Selezionare il nuovo gruppo di sicurezza: **test-vp-nsg2**.

1. Fare clic su **Salva** per salvare la modifica. L'applicazione alla rete richiederà un minuto.

## <a name="verify-the-rules"></a>Verificare le regole

La modifica verrà ora convalidata.

1. Tornare al pannello **Panoramica** per la macchina virtuale. È possibile trovare la macchina virtuale in **Tutte le risorse**.

1. Selezionare l'elemento **Rete** nella sezione **Impostazioni**.

1. Nel pannello **Panoramica** della rete è presente un collegamento **Regole di sicurezza effettive** che consente di visualizzare rapidamente come verranno valutate le regole. Fare clic sul collegamento per aprire l'analisi e assicurarsi che venga visualizzata la regola FTP.

    ![Screenshot che mostra le regole di sicurezza effettive per la rete.](../media/8-effective-rules.png)

1. Questa regola consente di connettersi a un server FTP. Se avessimo aggiunto il ruolo di lavoro FTP e configurato le cartelle, sarà possibile usare un client FTP per la connessione al server.

## <a name="one-more-thing"></a>Ultimo passaggio

Le regole di sicurezza sono difficili da configurare correttamente. In realtà è stato commesso un errore quando si è applicato questo nuovo gruppo di sicurezza perché si è perso l'accesso Desktop remoto. Per risolvere questo problema, è possibile aggiungere un'altra regola al gruppo di sicurezza per supportare l'accesso RDP. Assicurarsi di limitare gli indirizzi TCP/IP in ingresso della regola a quelli di cui si è proprietari.

> [!WARNING]
> Assicurarsi sempre di bloccare le porte usate per l'accesso amministrativo. Un approccio ancora migliore consiste nel creare una VPN per collegare la rete virtuale alla rete privata e consentire solo le richieste RDP o SSH da tale intervallo di indirizzi. È anche possibile sostituire la porta usata da RDP con una diversa da quella predefinita 3389. Tenere presente che cambiare le porte non è sufficiente per arrestare gli attacchi, ma semplicemente diventa un po' più difficile individuarle.