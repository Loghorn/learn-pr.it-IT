Le risorse chiave per l'architettura della banca online sono state create. In questa esercitazione viene completata l'installazione configurando le regole di bilanciamento del carico.

Al termine dell'installazione, viene eseguito il test del bilanciamento del carico rimuovendo una macchina virtuale dal pool e verificando che le richieste client non vengano più inviate a questa macchina virtuale.

L'esercitazione ha inizio con la definizione del pool back-end nel servizio di bilanciamento del carico. Il pool specifica la posizione a cui vengono indirizzate le richieste in ingresso.

## <a name="create-a-backend-address-pool"></a>Creare un pool di indirizzi back-end

1. Nel [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) nel menu a sinistra selezionare **Tutte le risorse**. Quindi, nell'elenco delle risorse selezionare il servizio di bilanciamento del carico (**woodgrove-LB**).

1. Selezionare **Impostazioni** > **Pool back-end**. Si noti che non è ancora stato definito alcun elemento.

1. Fare clic su **Aggiungi** per aggiungere un nuovo pool back-end.

    ![Screenshot che illustra come aggiungere un pool back-end.](../media/6-backend-pools.png)

1. Nel pannello **Aggiungi pool back-end** aggiungere le informazioni seguenti:
    - Assegnare il nome _woodgrove-BEP_.
    - Associare il pool a un **set di disponibilità**.
    - Per il set di disponibilità, selezionare _woodgrove-AS_.

1. Fare clic su **Aggiungi una configurazione IP della rete di destinazione**.

1. Nell'elenco **Macchina virtuale di destinazione** selezionare _woodgrove-VM1_.

1. Nell'elenco **Configurazione IP di rete** della VM selezionare il pool di indirizzi IP della macchina virtuale.

1. Ripetere i passaggi per aggiungere _woodgrove-VM2_ e _woodgrove-VM3_ al pool back-end.

    ![Screenshot che mostra il pannello Aggiungi pool back-end con tutte e tre le macchine virtuali selezionate.](../media/6-add-backend-pool.png)

1. Fare clic su **OK** per chiudere il pannello.

Attendere che la configurazione del bilanciamento del carico venga aggiornata prima di procedere con l'esercitazione.

## <a name="create-a-health-probe-for-the-load-balancer"></a>Creare un probe di integrità per il bilanciamento del carico

Aggiungere ora un probe di integrità per il protocollo HTTP sulla porta 80.

1. Nel pannello **woodgrove-LB** in **Impostazioni** fare clic su **Probe integrità**. Si noti che è attualmente vuoto.

1. Fare clic su **Aggiungi** per aggiungere un nuovo probe di integrità.

1. Nel pannello **Aggiungi probe integrità** impostare la configurazione seguente:
    - **Nome:** _woodgrove-HP_
    - **Protocollo:** _HTTP_
    - **Porta:** _80_
    - **Percorso:** _/_
    - **Intervallo:** _15_
    - **Soglia di non integrità:** _2_

1. Fare clic su **OK** per salvare le modifiche.

Attendere che la configurazione del bilanciamento del carico venga aggiornata prima di procedere con l'esercitazione.

## <a name="create-a-load-balancer-rule"></a>Creare una regola di bilanciamento del carico

Infine, creare una regola di bilanciamento del carico per HTTP sulla porta 80 che associa il pool back-end al probe di integrità.

1. Nel pannello **woodgrove-LB**, in **Impostazioni** fare clic su **Regole di bilanciamento del carico**.

1. Fare clic su **Aggiungi** per aggiungere una nuova regola.

1. Nel pannello **Aggiungi regola di bilanciamento del carico** impostare **Nome** su _woodgrove-HTTP-LBRule_.

1. Verificare che le informazioni seguenti vengano immesse automaticamente:
    - Versione indirizzo IP: **IPv4**
    - Indirizzo IP front-end: **LoadBalancerFrontEnd**
    - Protocollo: **TCP**
    - Porta: **80**
    - Porta back-end: **80**
    - Pool back-end: **woodgrove-BEP**
    - Probe integrità: **woodgrove-HP**
    - Salvataggio permanente sessione: **Nessuno**
    - Timeout di inattività: **4 minuti**
    - Indirizzo IP mobile: **Disabilitato**

1. Fare clic su **OK** per salvare la regola.

Attendere che la configurazione del bilanciamento del carico venga aggiornata prima di procedere con l'esercitazione.

## <a name="test-the-load-balancer"></a>Testare il bilanciamento del carico

1. Tornare alla pagina **Panoramica** del servizio di bilanciamento del carico e fare clic sul collegamento **Indirizzo IP pubblico** nella pagina per assegnare l'indirizzo IP. In alternativa, è possibile usare **Tutte le risorse** e quindi nell'elenco delle risorse selezionare **woodgrove-LB-ip**.

1. Nel pannello **Panoramica** selezionare l'**indirizzo IP** e quindi fare clic sul pulsante **Copia** accanto all'indirizzo. Questo indirizzo è l'indirizzo IP _pubblico_ del servizio di bilanciamento del carico.

    ![Screenshot che mostra il pannello Panoramica dell'indirizzo IP pubblico](../media/6-public-ip.png)

1. Aprire una nuova scheda del browser e incollare l'indirizzo IP nella barra degli indirizzi del browser. Prendere nota del nome del server e mantenere aperta questa scheda.

1. Nel portale di Azure, nel menu a sinistra, fare clic su **Tutte le risorse**. Quindi, nell'elenco di risorse, fare clic su server annotato in precedenza.

1. Nel pannello **Panoramica** fare clic su **Arresta** e quindi fare clic su **Sì**.

1. Attendere fino al completamento dell'arresto della macchina virtuale e quindi passare alla scheda visualizzata nel passaggio 3. Aggiornare la pagina.

1. Il servizio di bilanciamento del carico invia la richiesta HTTP a una delle altre macchine virtuali.

In questo esercizio è sta completata la distribuzione di macchine virtuali back-end e del servizio di bilanciamento del carico.
