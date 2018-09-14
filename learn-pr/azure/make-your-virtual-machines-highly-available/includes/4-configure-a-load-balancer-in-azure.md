Affinché il servizio di bilanciamento del carico funzionino correttamente, è necessario configurare le impostazioni che controllano il comportamento del bilanciamento del carico. In questo caso, esaminerà la configurazione di rete, probe di integrità, le regole di sicurezza, le regole di bilanciamento del carico e pool di server.

## <a name="steps-for-configuring-a-basic-public-load-balancer"></a>Passaggi per la configurazione di un servizio di bilanciamento del carico basic pubblico

Di seguito viene fornita una panoramica dei passaggi di configurazione principale per un servizio di bilanciamento del carico basic pubblico. I passaggi per uno standard di bilanciamento del carico e di carico interno di un servizio di bilanciamento sarà simile.

### <a name="back-end-servers"></a>Server back-end

In primo luogo, è necessario configurare il pool di macchine Virtuali di back-end. Le macchine virtuali devono essere nello stesso set di disponibilità e avere il proprio indirizzo IP pubblico (anche se ciò non effettivamente userà agli endpoint pubblici).

È necessario creare una nuova rete virtuale e definire una subnet per il pool di macchine Virtuali da usare.

 Quando si dispone di più VM che fornisce gli stessi servizi, è consigliabile usare un **gruppo di sicurezza di rete (NSG)** per garantire che le stesse regole del firewall siano presenti nel pool di VM (sebbene non sia parte del processo di bilanciamento del carico effettivo) . Ad esempio, per le VM che ospita le applicazioni web, è necessario creare regole di sicurezza in ingresso sulla porta 80 per HTTP o la porta 8080 per HTTPS.

### <a name="public-ip-address"></a>Indirizzo IP pubblico

Quando si crea un servizio di bilanciamento del carico basic pubblico usando il portale, il **indirizzo IP pubblico** viene configurato automaticamente come front-end di load balancer.

Parte della configurazione del servizio di bilanciamento del carico è il **pool di indirizzi back-end**, che contiene gli indirizzi IP di NIC virtuali ciascuna macchina virtuale che vengono connessi al servizio di bilanciamento del carico e usato per distribuire il traffico alle macchine virtuali. 

### <a name="health-probe"></a>Probe di integrità

Il probe di integrità aggiunge o rimuove in modo dinamico le VM nella rotazione del servizio di bilanciamento del carico in base alla rispettiva risposta ai controlli di integrità.
Per impostazione predefinita, sono presenti 15 secondi tra tentativi del probe. Dopo due errori di probe consecutivi, una macchina virtuale viene considerata non integra.

### <a name="rules"></a>Regole

La regola di bilanciamento del carico specifica la porta front-end è in ascolto e la porta usata per inviare il traffico al back-end.
