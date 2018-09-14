Applichiamo un gruppo di sicurezza di rete per la rete, in modo che è consentito solo il traffico HTTP tramite il server.

## <a name="create-a-network-security-group"></a>Creare un gruppo di sicurezza di rete

Azure dovrebbe aver creato un gruppo di sicurezza perché si era indicato di volere l'accesso SSH. Ma è possibile creare un nuovo gruppo di sicurezza, in modo che è possibile eseguire l'intero processo. Questo è particolarmente importante se si decide di creare la rete virtuale _prima_ delle macchine virtuali. Come indicato in precedenza, i gruppi di sicurezza sono _facoltativi_ e non vengono necessariamente creati con la rete.

1. Nel [portale di Azure](https://portal.azure.com?azure-portal=true), fare clic sui **crea una risorsa** pulsante nella barra laterale angolo sinistro per avviare una nuova creazione di risorse.

1. Tipo di **gruppo di sicurezza di rete** nella casella del filtro e selezionare l'elemento corrisponda nell'elenco.

1. Verificare che sia selezionato il modello di distribuzione **Resource Manager** e fare clic su **Crea**.

1. Fornire un **nome** per il gruppo di sicurezza. Anche in questo caso, le convenzioni di denominazione sono una buona idea. È possibile usare **test-web-eus-nsg1** per **Test Web Network Security gruppo & 1 nell'area Stati Uniti orientali**. Probabilmente vorrai modificare la parte di percorso del nome in modo da riflettere si inserita il gruppo di sicurezza.

1. Selezionare la **sottoscrizione** appropriata e usare il **gruppo di risorse** esistente.

1. Infine, inserirlo nella stessa **posizione** della macchina virtuale / livello virtuale rete. Questa operazione è importante perché non sarà possibile applicare questa risorsa se si trova in una località diversa.

1. Fare clic su **Crea** per creare il gruppo.

## <a name="add-a-new-inbound-rule-to-our-network-security-group"></a>Aggiungere una nuova regola in ingresso per il gruppo di sicurezza di rete

La distribuzione verrà completata rapidamente. Al termine, è possibile aggiungere nuove regole per il gruppo di sicurezza:

1. Trovare la nuova risorsa gruppo di sicurezza e selezionarla nel portale di Azure.

1. Nella pagina Panoramica si noterà che sono state create alcune regole predefinite per bloccare la rete.

    Sul lato in ingresso:

    - Tutto il traffico in ingresso da una rete virtuale a un'altra è consentito. In questo modo le risorse nella rete virtuale possono comunicare tra di loro.
    - Azure Load Balancer **probe** richieste per verificare che la macchina virtuale è attivo.
    - Tutto il traffico in ingresso viene rifiutato.  

    Sul lato in uscita:  
    - Tutto il traffico all'interno della rete virtuale è consentito.
    - Tutto il traffico in uscita a internet è consentito.
    - Tutto il traffico in uscita di altro tipo viene rifiutato.

    > [!NOTE]  
    > Queste regole predefinite vengono impostate con i valori ad alta priorità, che indica che essi valutati _ultimo_. Non possono essere modificate o eliminate, ma è possibile eseguirne l'_override_ creando regole più specifiche per il traffico con un valore di priorità più basso.

1. Fare clic sulla sezione **Regole di sicurezza in ingresso** nel pannello **Impostazioni** del gruppo di sicurezza.

1. Fare clic su **+ Aggiungi** per aggiungere una nuova regola di sicurezza.

    ![Screenshot del portale di Azure che mostra le impostazioni delle regole di sicurezza in ingresso con il pulsante Aggiungi evidenziato.](../media/8-add-rule.png)

    Esistono due modi per immettere le informazioni necessarie per una regola di sicurezza: di base e avanzato. È possibile passare tra di esse facendo clic sul pulsante in cima il **aggiungere** pannello.

    ![Una coppia di screenshot del portale di Azure che mostra l'alternanza tra Basic e Advanced regola di input, con una freccia che collega tra i due stati dell'interruttore evidenziato.](../media/8-advanced-create-rule.png)

    La modalità avanzata offre la possibilità di personalizzare completamente la regola. Tuttavia, se è necessario configurare un protocollo conosciuto, è un po' più facile lavorare con la modalità di base.

1. Passare alla modalità di base.

1. Aggiungere le informazioni per la regola HTTP:

    - Impostare il **servizio** su HTTP. Verrà automaticamente configurato l'intervallo di porte.
    - Impostare il **priorità** al **1000**. Deve essere un numero più basso di quello della regola predefinita **Nega**. È possibile iniziare l'intervallo con qualsiasi valore, ma è consigliabile che è possibile assegnare manualmente spazio nel caso in cui un'eccezione deve essere creato in un secondo momento.
    - Assegnare alla regola un nome. useremo **consentire-http-traffico**.
    - Assegnare una descrizione alla regola.

1. Tornare alla modalità **avanzata**. Si noti che le impostazioni sono ancora presenti. È possibile usare questo pannello per creare impostazioni con granularità più fine. In particolare, l'**origine** deve essere un indirizzo IP specifico o un intervallo di indirizzi IP specifici delle telecamere. Se si conosce l'indirizzo IP corrente del computer locale, è possibile provare a usarlo. In caso contrario, lasciare l'impostazione come **qualsiasi**, pertanto è possibile testare la regola.

1. Fare clic su **Aggiungi** per creare la regola. L'elenco di regole in ingresso verrà aggiornato. Si noti che sono in ordine di priorità, ovvero l'ordine in cui verranno esaminate.

## <a name="apply-the-security-group"></a>Applicare il gruppo di sicurezza

È importante ricordare che è possibile applicare il gruppo di sicurezza per un'interfaccia di rete per la protezione di una singola macchina virtuale o a una subnet in cui esso viene applicato a tutte le risorse in tale subnet. Il secondo approccio tende a essere il più comune, pertanto, è possibile eseguire questa operazione. È stato possibile ottenere per questa risorsa in Azure tramite la risorsa rete virtuale o indirettamente attraverso la macchina virtuale che usa la rete virtuale.

1. Passare al pannello **Panoramica** per la macchina virtuale. È possibile trovare la macchina virtuale in **Tutte le risorse**.

1. Selezionare l'elemento **Rete** nella sezione **Impostazioni**.

1. Nelle proprietà di rete, sono disponibili informazioni sulla connessione di rete applicato alla VM, tra cui il **rete virtuale/subnet**. Questo è un collegamento su cui è possibile fare clic per ottenere la risorsa. Fare clic per aprire la rete virtuale. Questo collegamento è disponibile _anche_ nel pannello **Panoramica** della macchina virtuale. Entrambi i collegamenti apriranno la **Panoramica** della rete virtuale.

1. Nella sezione **Impostazioni** selezionare l'elemento **Subnet**.

1. Dovrebbe essere presente una sola subnet definita (predefinita) da quando prima sono state create la macchina virtuale e la rete. Fare clic sull'elemento nell'elenco per aprire i dettagli.

1. Fare clic sulla voce **Gruppo di sicurezza di rete**.

1. Selezionare il nuovo gruppo di sicurezza: **test-web-eus-nsg1**. Dovrebbe essere presente un altro gruppo oltre a quello creato con la macchina virtuale.

1. Fare clic su **Salva** per salvare la modifica. L'applicazione alla rete richiederà un minuto.

## <a name="verify-the-rules"></a>Verificare le regole

È possibile convalidare la modifica:

1. Tornare al pannello **Panoramica** per la macchina virtuale. È possibile trovare la macchina virtuale in **Tutte le risorse**.

1. Selezionare l'elemento **Rete** nella sezione **Impostazioni**.

1. I dettagli di interfaccia di rete, è presente un collegamento per **regole di sicurezza effettive** che rapidamente illustrerà come regole intende da valutare. Fare clic sul collegamento per aprire l'analisi e assicurarsi che venga visualizzata la nuova regola.

    ![Screenshot del portale di Azure che mostra il pannello delle regole di sicurezza effettive per la rete.](../media/8-effective-rules.png)

1. Naturalmente, il modo migliore per assicurarsi che tutto funzioni correttamente consiste nell'inviare al server una richiesta HTTP. A questo punto dovrebbe funzionare.

    ![Screenshot di un web browser che mostra la pagina web predefinita di Apache ospitati nell'indirizzo IP della nuova VM Linux.](../media/6-apache-works.png)

## <a name="one-more-thing"></a>Ultimo passaggio

Le regole di sicurezza sono difficili da configurare correttamente. È stato commesso un errore quando si è applicato questo nuovo gruppo di sicurezza e l'accesso SSH. Per risolvere questo problema, è possibile aggiungere un'altra regola al gruppo di sicurezza per supportare l'accesso SSH. Assicurarsi di limitare gli indirizzi TCP/IP in ingresso della regola a quelli di cui si è proprietari.

> [!WARNING]  
> Assicurarsi sempre di bloccare le porte usate per l'accesso amministrativo. Un approccio ancora migliore consiste nel creare una VPN per collegare la rete virtuale alla rete privata e consentire solo le richieste RDP o SSH da tale intervallo di indirizzi. È anche possibile sostituire la porta usata da SSH con una diversa da quella predefinita. Tenere presente che la modifica delle porte non è sufficiente arrestare gli attacchi. Semplicemente rende un po' più difficili da individuare.