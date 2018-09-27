Si supponga di lavorare per Woodgrove Bank e che si stia per lanciare i servizi di banking online. Trattandosi di un settore estremamente competitivo, è necessario garantire una disponibilità minima dei servizi del 99,99%. È stato determinato che un'istanza di Azure Load Balancer con un pool di tre macchine virtuali consentirà di raggiungere questo obiettivo.

In questo esercizio verranno creati un servizio di bilanciamento del carico e una rete virtuale usando il portale di Azure. Poiché sarà necessario uno solo di questi elementi, il portale rappresenta uno strumento semplice per procedere alla creazione.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-public-load-balancer"></a>Creare un servizio di bilanciamento del carico pubblico

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Nella barra laterale fare clic su **Crea una risorsa**.

1. Selezionare la sezione **Rete** e quindi fare clic su **Servizio di bilanciamento del carico**. Se l'opzione non è visualizzata, è possibile usare la casella di ricerca.

    ![Screenshot che mostra Azure Marketplace con la sezione Rete selezionata e l'opzione Servizio di bilanciamento del carico evidenziata](../media/3-azure-marketplace.png)

1. Nel pannello **Crea servizio di bilanciamento del carico** immettere o selezionare le informazioni seguenti:
    - **Nome:** _woodgrove-LB_
    - **Tipo:** _Pubblico_
    - **SKU:** _Basic_
    - **Indirizzo IP pubblico:** selezionare **Crea nuovo**. Nella casella di testo digitare _woodgrove-LB-ip_. Lasciare l'assegnazione impostata su _Dinamica_.
    - **Sottoscrizione:** dovrebbe essere già impostata su _Sottoscrizione Concierge_.
    - **Gruppo di risorse:** selezionare **Usa esistente** e scegliere _<rgn>[nome gruppo risorse sandbox]</rgn>_.
    - **Località:** selezionare un'area vicina dall'elenco seguente.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    ![Screenshot che mostra la schermata di creazione di un servizio di bilanciamento del carico](../media/3-create-load-balancer.png)

1. Fare clic su **Crea**.

Durante la creazione e la distribuzione della risorsa del servizio di bilanciamento del carico, creare la rete virtuale di Microsoft Azure che verrà usata per la subnet back-end.

## <a name="create-a-virtual-network"></a>Creare una rete virtuale

1. Nel menu di sinistra fare clic su **Crea una risorsa**. Nel pannello **Nuovo** fare clic su **Rete** e quindi su **Rete virtuale**.

    ![Screenshot che mostra Azure Marketplace con la sezione Rete selezionata e l'opzione Servizio di bilanciamento del carico evidenziata](../media/3-azure-marketplace-2.png)

1. Nel pannello **Crea rete virtuale** immettere o selezionare le informazioni seguenti:
    - **Nome:** _woodgrove-VNET_
    - **Spazio indirizzi:** _172.20.0.0/16_
    - **Sottoscrizione:** dovrebbe essere già impostata su _Sottoscrizione Concierge_.
    - **Gruppo di risorse:** selezionare il gruppo di risorse _<rgn>[Nome gruppo risorse]</rgn>_ esistente dall'elenco.
    - **Nome subnet:** _backendSubnet_
    - **Intervallo di indirizzi della subnet:** _172.20.0.0/24_
    - **Protezione DDoS:** _Basic_
    - **Endpoint servizio:** _Disabilitato_

    ![Screenshot che mostra la schermata di creazione di un servizio di bilanciamento del carico](../media/3-create-vnet.png)

1. Fare clic su **Crea**.

Durante la distribuzione della rete virtuale, creare un altro elemento, ovvero un gruppo di sicurezza di rete.

## <a name="create-and-configure-a-network-security-group"></a>Creare e configurare un gruppo di sicurezza di rete

1. Fare clic su **Crea una risorsa**

1. Selezionare il gruppo **Rete** e fare clic su **Gruppo di sicurezza di rete**.

    ![Screenshot che mostra Azure Marketplace con la sezione Rete selezionata e l'opzione Gruppo di sicurezza di rete evidenziata](../media/3-azure-marketplace-3.png)


1. Assegnare il nome **woodgrove-NSG** al gruppo e inserirlo nel gruppo di risorse dell'ambiente sandbox di Azure.

1. Assicurarsi che si trovi nello stesso percorso di Azure Load Balancer.

    ![Screenshot che mostra la schermata di creazione del gruppo di sicurezza di rete](../media/3-create-nsg.png)

1. Fare clic su **Crea**.

Attendere che il servizio di bilanciamento del carico, la rete virtuale e il gruppo di sicurezza di rete vengono distribuiti. È quindi possibile configurare la sicurezza di rete.

## <a name="configure-the-network-security-group"></a>Configurare il gruppo di sicurezza di rete

1. Selezionare il nuovo gruppo di sicurezza di rete dalla notifica di distribuzione o tramite la barra di ricerca nella parte superiore del portale di Azure.

    ![Screnshot che mostra la distribuzione completata nel pannello Notifica](../media/3-deployment-success.png)

1. Selezionare la sezione **Impostazioni** > **Regole di sicurezza in ingresso**. Si noti che sono presenti tre regole predefinite che vengono sempre applicate. Queste regole sono:
    - **AllowVnetInbound**: consentire tutto il flusso di traffico interno nella rete virtuale. Questa regola consente alle macchine virtuali che condividono la rete di comunicare l'una con l'altra.
    - **AllowAzureLoadBalancerInBound**: consentire al servizio di bilanciamento del carico di effettuare il ping dei servizi della rete per verificare se sono attivi.
    - **DenyAllInbound**: blocca tutto il traffico rimanente.

    La regola **DenyAllInbound** è particolarmente importante poiché garantisce che tutto il traffico in ingresso senza una regola di priorità più elevata venga bloccato. Per questa ragione sarà necessario aggiungere una nuova regola per consentire il traffico HTTP (80) per i server Web.

    > [!NOTE]
    > La priorità delle regole dei gruppi di sicurezza di rete può avere un valore compreso tra 0 e 65500 e le regole vengono valutate nell'ordine. La decisione sarà basata sulla prima regola corrispondente. È sempre consigliabile creare regole con priorità piuttosto bassa, a partire da valori pari a 1000 in modo che abbiano la precedenza sulle regole predefinite.

1. Fare clic su **Aggiungi** per aggiungere una nuova regola.

    ![Screenshot che mostra le regole di sicurezza di rete in ingresso con il pulsante Aggiungi evidenziato](../media/3-inbound-security-rules.png)

1. Fare clic sul pulsante **Base** nella parte superiore per passare alla vista di base.

1. Immettere i dettagli per la nuova regola.
    - Selezionare _HTTP_ per **Servizio**.
    - Impostare **Priorità** su _1000_.
    - Assegnare un nome alla regola o lasciare il nome predefinito.

    ![Screenshot che mostra la finestra di dialogo Aggiungi regola di sicurezza in ingresso con i dettagli HTTP specificati](../media/3-add-inbound-rule.png)

1. Fare clic su **Aggiungi** per aggiungere la regola.

    ![Screenshot che mostra la regola HTTP appena aggiunta nell'elenco delle regole dei gruppi di sicurezza di rete](../media/3-new-added-rule.png)

## <a name="apply-the-network-security-group-to-the-vnet"></a>Applicare il gruppo di sicurezza di rete alla rete virtuale

Si procede ora ad applicare il gruppo di sicurezza di rete alla rete virtuale.

1. Individuare la rete virtuale (è possibile usare la casella di ricerca nella parte superiore). La rete è denominata **woodgrove-VNET**.

1. Selezionare **Impostazioni** > **Subnet** per passare alle subnet definite.

1. Fare clic sulla voce **backendSubnet** per visualizzare le proprietà.

    ![Screenshot che mostra le voci backendSubnet nell'area Subnet delle subnet](../media/3-subnets.png)

1. Fare clic sulla voce "Nessuno" in **Gruppo di sicurezza di rete**

    ![Screenshot che mostra il gruppo di sicurezza di rete vuoto in backendSubnet](../media/3-add-network-security-group.png)

1. Selezionare **woodgrove-NSG** per aggiungerlo alla rete virtuale.

1. Fare clic su **Salva** per applicare la modifica.

Dopo aver configurato la rete, è possibile creare alcune macchine virtuali da aggiungere alla rete.