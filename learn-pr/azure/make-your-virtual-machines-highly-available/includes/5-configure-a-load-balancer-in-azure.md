Per il funzionamento corretto del servizio di bilanciamento del carico, è prima necessario configurare le impostazioni che ne controllano il comportamento. Di seguito verrà illustrata la configurazione della rete, del probe di integrità, delle regole di sicurezza, delle regole di bilanciamento del carico e del pool di server.

## <a name="steps-for-configuring-a-basic-public-load-balancer"></a>Passaggi per la configurazione di un servizio di bilanciamento del carico pubblico Basic

Di seguito viene offerta una panoramica dei passaggi di configurazione principali per un servizio di bilanciamento del carico pubblico Basic. I passaggi per un servizio di bilanciamento del carico standard e un servizio di bilanciamento del carico interno sono simili. È necessario configurare quattro elementi principali:

- Server back-end per l'elaborazione delle richieste
- Indirizzo IP pubblico
- Probe di integrità
- Regole del servizio di bilanciamento del carico

### <a name="backend-servers"></a>Server back-end

Per prima cosa, è necessario configurare il pool di VM back-end. Le VM devono essere incluse nello stesso set di disponibilità e avere un proprio indirizzo IP pubblico, nonostante questo non venga effettivamente usato dagli endpoint pubblici.

È necessario creare una nuova rete virtuale e definire una subnet da usare per il pool di VM.

 In presenza di più VM che offrono gli stessi servizi è consigliabile usare un **gruppo di sicurezza di rete (NSG)** per garantire che nell'intero pool di VM vengano applicate le stesse regole del firewall (sebbene questa operazione non sia inclusa nel processo di bilanciamento del carico corrente). Per le VM che ospitano applicazioni Web, ad esempio, sarà necessario creare regole di sicurezza in ingresso sulla porta 80 per HTTP o sulla porta 8080 per HTTPS.

### <a name="public-ip-address"></a>Indirizzo IP pubblico

Quando si crea un servizio di bilanciamento del carico pubblico Basic usando il portale, l'**indirizzo IP pubblico** viene configurato automaticamente come front-end del servizio di bilanciamento del carico.

La configurazione del servizio di bilanciamento del carico include il **pool di indirizzi back-end** che contiene gli indirizzi IP delle schede di interfaccia di rete virtuali di ogni VM che sono connesse al servizio di bilanciamento del carico e vengono usate per distribuire il traffico alle VM.

### <a name="health-probe"></a>Probe di integrità

Il probe di integrità aggiunge o rimuove in modo dinamico le VM nella rotazione del servizio di bilanciamento del carico in base alla rispettiva risposta ai controlli di integrità. Per impostazione predefinita, i tentativi di probe vengono eseguiti ogni 15 secondi. Dopo due errori di probe consecutivi, la VM viene considerata non integra e il traffico non viene instradato fino a quando il server non torna online.

### <a name="rules"></a>Regole

Le regole di bilanciamento del carico identificano le porte su cui è in ascolto il servizio di bilanciamento del carico e le porte usate per l'invio del traffico al set back-end delle VM. È necessaria almeno una regola per instradare il traffico.
