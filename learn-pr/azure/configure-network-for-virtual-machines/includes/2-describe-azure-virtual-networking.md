È presente un data center locale che si prevede di mantenere, ma si vuole usare Azure per l'offload dei picchi di traffico tramite macchine virtuali ospitate in Azure. Si vuole sapere se è possibile mantenere lo schema di indirizzi IP e le appliance di rete esistenti, assicurando al tempo stesso la protezione di qualsiasi trasferimento di dati.

## <a name="what-is-azure-virtual-networking"></a>Informazioni sulla rete virtuale di Azure

Le **reti virtuali di Azure** consentono alle risorse di Azure, ad esempio le macchine virtuali, le app Web e i database, di comunicare tra loro, con gli utenti su Internet e con i computer client locali. Una rete di Azure può essere considerata come un set di risorse che consente il collegamento di altre risorse di Azure.

Le reti virtuali di Azure offrono funzionalità di rete essenziali, tra cui:

- Isolamento e segmentazione
- Comunicazioni Internet
- Comunicazione tra risorse di Azure
- Comunicazione con le risorse locali
- Routing del traffico di rete
- Filtri per il traffico di rete
- Connessione di reti virtuali

### <a name="isolation-and-segmentation"></a>Isolamento e segmentazione

Azure consente di creare più reti virtuali isolate. Quando si configura una rete virtuale, si definisce uno spazio indirizzi IP (Internet Protocol) privato mediante intervalli di indirizzi IP pubblici o privati. Quindi è possibile segmentare tale spazio indirizzi IP in subnet e allocare parte dello spazio indirizzi definito a ogni subnet denominata.

Per la risoluzione dei nomi è possibile usare il servizio di risoluzione dei nomi incorporato in Azure o configurare la rete virtuale per l'uso di un server DNS (Domain Name System) interno o esterno.

### <a name="internet-communications"></a>Comunicazioni Internet

Una macchina virtuale (VM) in Azure può connettersi a Internet per impostazione predefinita. È tuttavia necessario connettersi a tale VM e controllarla tramite l'interfaccia della riga di comando di Azure, Remote Desktop Protocol (RDP) o Secure Shell (SSH). È possibile abilitare le comunicazioni in ingresso definendo un indirizzo IP pubblico o un servizio di bilanciamento del carico pubblico.

### <a name="communicate-between-azure-resources"></a>Comunicazione tra risorse di Azure

È consigliabile consentire alle risorse di Azure di comunicare tra loro in modo sicuro. È possibile ottenere questo risultato in due modi:

- **Reti virtuali**
    
    Le reti virtuali possono connettere non solo le VM ma anche altre risorse di Azure, tra cui l'ambiente del servizio app, il servizio Kubernetes di Azure e i set di scalabilità delle macchine virtuali di Azure.

- **Endpoint di servizio**
     
     È possibile usare gli endpoint di servizio per la connessione ad altri tipi di risorse di Azure, ad esempio i database SQL di Azure e gli account di archiviazione. Questo approccio consente di collegare più risorse di Azure alle reti virtuali, migliorando la sicurezza e offrendo un routing ottimale tra le risorse.

### <a name="communicate-with-on-premises-resources"></a>Comunicazione con le risorse locali

Le reti virtuali di Azure consentono di collegare tra loro le risorse nell'ambiente locale ed entro la sottoscrizione di Azure, creando di fatto una rete che include gli ambienti locale e cloud. Sono disponibili tre meccanismi per ottenere tale connettività:

- **VPN da punto a sito**

   Questo approccio è analogo a una connessione VPN stabilita con la rete aziendale da un computer esterno all'organizzazione, ma con direzione opposta. In questo caso il computer client attiva una connessione VPN crittografata ad Azure, connettendo il computer alla rete virtuale di Azure.

- **VPN da sito a sito** Una VPN da sito a sito collega il dispositivo o il gateway VPN locale al gateway VPN di Azure in una rete virtuale. I dispositivi in Azure possono essere in effetti visualizzati come se si trovassero nella rete locale. La connessione è crittografata e viene effettuata tramite Internet.

- **Azure ExpressRoute**

    Per gli ambienti che richiedono larghezza di banda superiore e livelli di sicurezza ancora più elevati, Azure ExpressRoute è l'approccio ottimale. Azure ExpressRoute offre ad Azure una connettività privata dedicata, non basata su Internet.

### <a name="route-network-traffic"></a>Routing del traffico di rete

Per impostazione predefinita, Azure indirizzerà il traffico tra subnet su qualsiasi rete virtuale connessa, su reti locali e su Internet. È tuttavia possibile controllare il routing ed eseguire l'override di queste impostazioni, come indicato di seguito:

- **Tabelle di route**

    Una tabella di route consente di definire regole per l'indirizzamento del traffico. È possibile creare tabelle di route personalizzate che controllano il modo in cui i pacchetti vengono indirizzati tra subnet.

- **Border Gateway Protocol**

    Border Gateway Protocol (BGP) interagisce con i gateway VPN di Azure o ExpressRoute per propagare route BGP locali nelle reti virtuali di Azure.

### <a name="filter-network-traffic"></a>Filtrare il traffico di rete

Le reti virtuali di Azure consentono di filtrare il traffico tra le subnet mediante gli approcci seguenti:

- **Gruppi di sicurezza di rete**

    Un gruppo di sicurezza di rete è una risorsa di Azure in grado di contenere più regole di sicurezza in ingresso e in uscita. È possibile definire queste regole per consentire o bloccare il traffico in base a fattori quali l'indirizzo IP di origine e di destinazione, la porta e il protocollo.

- **Appliance virtuali di rete**
    
    Un'appliance virtuale di rete è una macchina virtuale specializzata, analoga a un'appliance virtuale con protezione avanzata. L'appliance virtuale di rete svolge una funzione di rete specifica, ad esempio l'esecuzione di un firewall o di un'ottimizzazione WAN.

## <a name="connect-virtual-networks"></a>Connessione di reti virtuali

È possibile collegare le reti virtuali mediante il _peering_ di rete virtuale. Il peering consente alle risorse in ogni rete virtuale di comunicare tra loro. Queste reti virtuali possono trovarsi in aree separate e consentono di creare una rete interconnessa globale tramite Azure.

## <a name="azure-virtual-network-settings"></a>Impostazione della rete virtuale di Azure

È possibile creare e configurare reti virtuali di Azure dal portale di Azure, tramite Azure PowerShell nel computer locale o tramite Azure Cloud Shell.

### <a name="create-a-virtual-network"></a>Creare una rete virtuale

Quando si crea una rete virtuale di Azure, è necessario configurare alcune impostazioni di base. È anche possibile configurare impostazioni avanzate, ad esempio più subnet, protezione da attacchi Distributed Denial of Service (DDoS) ed endpoint di servizio.

![Screenshot del portale di Azure con un esempio dei campi del pannello Crea rete virtuale.](../media/2-create-virtual-network.PNG)

Ecco le impostazioni da configurare per una rete virtuale di base:

- **Nome rete**

    Questo nome deve essere univoco nella sottoscrizione, ma non è necessario che sia univoco a livello globale. Specificare un nome descrittivo, facile da ricordare e distinguere da altre reti virtuali.

- **Spazio indirizzi**
    
    Quando si configura una rete virtuale, si definisce lo spazio indirizzi interno in formato CIDR (Classless Interdomain Routing). Questo spazio indirizzi deve essere univoco nella sottoscrizione, quindi non è possibile definire uno spazio indirizzi pari ad esempio a 10.0.0.0/24 per una rete virtuale e quindi a 10.1.0.0/8 per un'altra, perché la rete 10.1.0.0/8 si sovrapporrà a 10.0.0.0/24. È tuttavia possibile specificare 10.0.0.0/16 e 10.1.0.0/16 e così via. 
    > [!NOTE] 
    > È possibile aggiungere spazi indirizzi dopo avere creato la rete virtuale.

- **Sottoscrizione**

    Applicabile solo se sono presenti più sottoscrizioni.

- **Gruppo di risorse**
    
    Come qualsiasi altra risorsa di Azure, una rete virtuale deve trovarsi in un gruppo di risorse. È possibile selezionare un gruppo di risorse esistente o crearne uno nuovo.
    
- **Posizione**

    Selezionare la posizione da usare per la rete virtuale.

- **Subnet**
    
    In ogni intervallo di indirizzi della rete virtuale è possibile creare una o più subnet che partizionano lo spazio indirizzi della rete virtuale. Il routing tra le subnet dipenderà quindi dalle route di traffico predefinite, oppure è possibile definire route personalizzate. È anche possibile definire una subnet che includa tutti gli intervalli di indirizzi della rete virtuale.

    > [!NOTE]
    > I nomi di subnet devono iniziare con una lettera o un numero, terminare con una lettera, un numero o un carattere di sottolineatura e possono contenere solo lettere, numeri, caratteri di sottolineatura, punti o trattini.

- **Protezione DDoS**

    È possibile selezionare la protezione DDoS Basic o Standard. La Protezione DDoS Standard è un servizio Premium. Per altre informazioni sulla protezione DDoS Standard.

- **Endpoint di servizio**
    
    Questa opzione consente di abilitare gli endpoint di servizio e quindi di selezionare dall'elenco gli endpoint di servizio di Azure da abilitare. Le opzioni disponibili includono Azure Cosmos DB, bus di servizio di Azure, Azure Key Vault e così via.

Dopo avere configurato queste impostazioni, fare clic sul pulsante **Crea**.

### <a name="define-additional-settings"></a>Definire impostazioni aggiuntive

Dopo aver creato una rete virtuale è possibile definire altre impostazioni. Queste includono:

- **Gruppo di sicurezza di rete**
    
    I gruppi di sicurezza di rete hanno regole di sicurezza che consentono di filtrare il tipo di traffico di rete consentito in ingresso e in uscita dalle subnet della rete virtuale e dalle interfacce di rete. È possibile creare il gruppo di sicurezza di rete separatamente e quindi associarlo alla rete virtuale.

- **Tabella di route**

    Azure crea automaticamente una tabella di route per ogni subnet all'interno di una rete virtuale di Azure e aggiunge le route predefinite di sistema alla tabella. È tuttavia possibile aggiungere tabelle di route personalizzate per modificare il traffico tra reti virtuali.

È anche possibile modificare gli endpoint di servizio.

![Screenshot del portale di Azure con un esempio del pannello per la modifica delle impostazioni della rete virtuale.](../media/2-virtual-network-additional-settings.PNG)

### <a name="configure-virtual-networks"></a>Configurare reti virtuali

Dopo avere creato una rete virtuale è possibile modificare altre impostazioni dal pannello Reti virtuali nel portale di Azure. In alternativa è possibile usare i comandi di PowerShell o i comandi dell'interfaccia della riga di comando in Cloud Shell per apportare le modifiche.

![Screenshot del portale di Azure con un pannello di esempio per la configurazione della rete virtuale.](../media/2-configure-virtual-network.PNG)

È quindi possibile verificare e modificare le impostazioni in altri pannelli secondari.
Tali impostazioni includono:

- Spazi indirizzi

    È possibile aggiungere altri spazi indirizzi alla definizione iniziale

- Dispositivi connessi

    È possibile usare la rete virtuale per connettere i computer

- Subnet

    È possibile aggiungere altre subnet

- Peering

    È possibile collegare le reti virtuali in modalità di peering

È anche possibile monitorare e risolvere i problemi delle reti virtuali o creare uno script di automazione per generare la rete virtuale corrente.

## <a name="summary"></a>Riepilogo

Le reti virtuali sono meccanismi avanzati e a configurabilità elevata per la connessione di entità in Azure. È possibile connettere le risorse di Azure tra loro o a risorse disponibili in locale. È possibile isolare, filtrare e indirizzare il traffico di rete. Azure consente anche di aumentare la sicurezza in caso di necessità.
