Si dispone di un data center locale che si prevede di continuare, ma si vuole usare Azure per l'offload di picco di traffico usando macchine virtuali (VM) ospitate in Azure. Si desidera mantenere l'indirizzo IP esistente addressing Appliance di rete e lo schema, garantendo che qualsiasi trasferimento di dati sia sicuro.

## <a name="what-is-azure-virtual-networking"></a>Che cos'è rete virtuale di Azure?

**Reti virtuali di Azure** consentono alle risorse di Azure, ad esempio le macchine virtuali, App web e database, per comunicare con: ogni altro, gli utenti su Internet e i computer client locale. Una rete di Azure può essere considerata un set di risorse che consente il collegamento di altre risorse di Azure.

Reti virtuali di Azure forniscono le funzionalità di rete principali:

- Isolamento e segmentazione
- Comunicazioni Internet
- Comunicazione tra risorse di Azure
- Comunicazione con le risorse locali
- Routing del traffico di rete
- Filtri per il traffico di rete
- Connessione di reti virtuali

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEve]

### <a name="isolation-and-segmentation"></a>Isolamento e segmentazione

Azure consente di creare più reti virtuali isolate. Quando è stata configurata una rete virtuale, si definisce uno spazio di indirizzi IP (Internet Protocol) privati, usando intervalli di indirizzi IP pubblici o privati. È quindi possibile segmentare tale spazio indirizzi IP in subnet e allocare parte dello spazio indirizzi definito a ogni subnet denominata.

Per la risoluzione dei nomi, è possibile usare il servizio di risoluzione dei nomi integrata Azure oppure è possibile configurare la rete virtuale per l'uso interna o un server di sistema DNS (Domain Name) esterno.

### <a name="internet-communications"></a>Comunicazioni Internet

Per impostazione predefinita, una macchina virtuale in Azure possa connettersi a Internet. Tuttavia, è necessario connettersi a e controllare tale macchina virtuale, con il comando di Azure, Remote Desktop Protocol (RDP) o Secure Shell (SSH). È possibile abilitare le comunicazioni in ingresso definendo un indirizzo IP pubblico o un servizio di bilanciamento del carico pubblico.

### <a name="communicate-between-azure-resources"></a>Comunicazione tra risorse di Azure

È opportuno abilitare le risorse di Azure di comunicare in modo sicuro tra loro. È possibile ottenere questo risultato in due modi:

- **Reti virtuali**
    
    Le reti virtuali possono connettersi le macchine virtuali non solo, ma altre risorse di Azure, ad esempio l'ambiente del servizio App, Azure Kubernetes Service fornisce e set di scalabilità di macchine virtuali di Azure.

- **Endpoint servizio**
     
     È possibile usare gli endpoint di servizio per connettersi ad altri tipi di risorse di Azure, ad esempio account di archiviazione e database SQL di Azure. Questo approccio consente di collegare più risorse di Azure alle reti virtuali, migliorando quindi la sicurezza e offrendo un routing ottimale tra le risorse.

### <a name="communicate-with-on-premises-resources"></a>Comunicazione con le risorse locali

Reti virtuali di Azure consentono di collegare tra loro le risorse nell'ambiente locale e nella sottoscrizione di Azure, in effetti la creazione di una rete che si estende su entrambi locale e ambienti cloud. Sono disponibili tre meccanismi per ottenere tale connettività:

- **Point-to-site reti Private virtuali**

   Questo approccio è simile a una connessione di rete privata virtuale (VPN, Virtual Private Network) che un computer all'esterno dell'organizzazione effettua nuovamente nella tua rete aziendale, ad eccezione del fatto che funziona nella direzione opposta. In questo caso il computer client attiva una connessione VPN crittografata ad Azure, connettendo il computer alla rete virtuale di Azure.

- **Le reti Private virtuali Site-to-site** una VPN site-to-site collega il dispositivo VPN locale o un gateway con gateway VPN di Azure in una rete virtuale. I dispositivi in Azure possono essere in effetti visualizzati come se si trovassero nella rete locale. La connessione è crittografata e funziona su Internet.

- **ExpressRoute di Azure**

    Per gli ambienti in cui è necessario maggiore larghezza di banda e livelli superiori di sicurezza, Azure ExpressRoute è l'approccio migliore. Azure ExpressRoute offre connettività privata dedicata per Azure, che non vengano trasmessi tramite Internet.

### <a name="route-network-traffic"></a>Routing del traffico di rete

Per impostazione predefinita, Azure instrada il traffico tra subnet su qualsiasi connesse le reti virtuali, reti locali e Internet. È tuttavia possibile controllare il routing ed eseguire l'override di queste impostazioni, come indicato di seguito:

- **Tabelle di route**

    Una tabella di route consente di definire le regole per quanto riguarda la modalità deve essere indirizzato il traffico. È possibile creare tabelle di route personalizzate che controllano il modo in cui i pacchetti vengono indirizzati tra subnet.

- **Border Gateway Protocol**

    Protocollo BGP (Border Gateway) funziona con i gateway VPN di Azure o ExpressRoute per la propagazione in locale le route BGP alle reti virtuali di Azure.

### <a name="filter-network-traffic"></a>Filtri per il traffico di rete

Le reti virtuali di Azure consentono di filtrare il traffico tra le subnet mediante gli approcci seguenti:

- **Gruppi di sicurezza di rete**

    Un gruppo di sicurezza di rete è una risorsa di Azure che può contenere più regole di sicurezza in ingresso e in uscita. È possibile definire queste regole per consentire o bloccare il traffico in base a fattori quali l'indirizzo IP di origine e di destinazione, la porta e il protocollo.

- **Appliance virtuali di rete**
    
    Appliance virtuale di rete è una macchina virtuale specializzata che può essere confrontato con un'appliance di rete con protezione avanzata. Appliance virtuale di rete esegue una funzione di rete specifica, ad esempio l'esecuzione di un firewall o eseguire l'ottimizzazione WAN.

## <a name="connect-virtual-networks"></a>Connessione di reti virtuali

È possibile collegare le reti virtuali mediante il _peering_ di rete virtuale. Il peering consente alle risorse in ogni rete virtuale di comunicare tra loro. Queste reti virtuali possono trovarsi in aree separate, consentendo di creare una rete interconnessa globale tramite Azure.

## <a name="azure-virtual-network-settings"></a>Impostazioni di rete virtuale di Azure

È possibile creare e configurare reti virtuali di Azure dal portale di Azure, Azure PowerShell nel computer locale o Azure Cloud Shell.

### <a name="create-a-virtual-network"></a>Creare una rete virtuale

Quando si crea una rete virtuale di Azure, si configura un numero di impostazioni di base. È possibile scegliere di configurare le impostazioni avanzate, ad esempio più subnet, attacchi distribuiti denial of dervice (DDoS) protezione e gli endpoint di servizio.

![Screenshot del portale di Azure che mostra un esempio della rete virtuale crea i campi del pannello.](../media/2-create-virtual-network.PNG)

È possibile configurare le seguenti impostazioni per una rete virtuale di base:

- **Nome di rete**

    Il nome di rete deve essere univoco nella sottoscrizione, ma non dovrà essere globalmente univoco. Rendere il nome di un'istanza descrittiva che è facile da ricordare e identificato da altre reti virtuali.

- **Spazio degli indirizzi**
    
    Quando si configura una rete virtuale, si definisce lo spazio degli indirizzi interni in formato Classless Interdomain Routing (CIDR). Questo spazio di indirizzi deve essere univoco nella propria sottoscrizione e altre reti a cui ci si connette.
    
    Si supponga, si sceglie uno spazio indirizzi 10.0.0.0/24 per la prima rete virtuale. Gli indirizzi definiti in questo intervalli di spazi di indirizzi da 10.0.0.1 - 10.0.0.254. Si crea un secondo virtuale di rete e scegliere uno spazio indirizzi 10.1.0.0./8. L'indirizzo in questo spazio di indirizzi compreso tra 10.0.0.1 - 10.255.255.254. Alcuni dell'indirizzo si sovrappongono e non può essere usati per le due reti virtuali.

    Tuttavia, è possibile usare 10.0.0.0/16, con indirizzi compresi tra 10.0.0.1 - 10.0.255.254 e 10.1.0.0/16, con indirizzi compresi tra 10.1.0.1 - 10.1.255.254. Poiché non si verificano sovrapposizioni di indirizzo, è possibile assegnare questi spazi degli indirizzi per le reti virtuali.

    > [!NOTE] 
    > È possibile aggiungere gli spazi degli indirizzi dopo aver creato la rete virtuale.

- **Sottoscrizione**

    Si applica solo se si hanno più sottoscrizioni, tra cui scegliere.

- **Gruppo di risorse**
    
    Come qualsiasi altra risorsa di Azure, una rete virtuale deve esistere in un gruppo di risorse. È possibile selezionare un gruppo di risorse esistente o crearne uno nuovo.
    
- **Posizione**

    Selezionare il percorso in cui si desidera la rete virtuale esistente.

- **Subnet**
    
    All'interno di ogni intervallo di indirizzi di rete virtuale, è possibile creare uno o più subnet in cui partizionare lo spazio di indirizzi della rete virtuale. Il routing tra le subnet dipenderà quindi dalle route di traffico predefinite. In alternativa, è possibile definire route personalizzate. In alternativa, è possibile definire una subnet che include intervalli degli tutte le reti virtuali indirizzi.

    > [!NOTE]
    > I nomi di subnet devono iniziare con una lettera o un numero, terminare con una lettera, un numero o un carattere di sottolineatura e possono contenere solo lettere, numeri, caratteri di sottolineatura, punti o trattini.

- **Protezione Denial of Service (DDoS) distribuita**

    È possibile selezionare la protezione di base o Standard DDoS. La Protezione DDoS Standard è un servizio Premium. Il [protezione DDoS di Azure Standard](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview) fornisce per altre informazioni su protezione DDoS Standard. 

- **Endpoint di servizio**
    
    In questo caso, si abilita gli endpoint di servizio e quindi selezionare dall'elenco quali endpoint del servizio di Azure a cui si vuole abilitare. Le opzioni includono Azure Cosmos DB, il Bus di servizio di Azure, Azure Key Vault e così via.

Dopo avere configurato queste impostazioni, fare clic sul pulsante **Crea**.

### <a name="define-additional-settings"></a>Definire le impostazioni aggiuntive

Dopo avere creato una rete virtuale è possibile definire altre impostazioni, tra cui:

- **Gruppo di sicurezza di rete**
    
    Gruppi di sicurezza di rete hanno regole di sicurezza che consentono di filtrare il tipo di traffico di rete che può scorrere da e verso subnet della rete virtuale e le interfacce di rete. Creare il gruppo di sicurezza di rete separatamente e quindi associarlo alla rete virtuale.

- **Tabella di route**

    Azure crea automaticamente una tabella di route per ogni subnet all'interno di una rete virtuale di Azure e aggiunge le route predefinite di sistema alla tabella. È tuttavia possibile aggiungere tabelle di route personalizzate per modificare il traffico tra reti virtuali.

È anche possibile modificare gli endpoint di servizio.

![Screenshot del portale di Azure che mostra un pannello di esempio per la modifica delle impostazioni di rete virtuale.](../media/2-virtual-network-additional-settings.PNG)

### <a name="configure-virtual-networks"></a>Configurare reti virtuali

Dopo avere creato una rete virtuale, è possibile modificare altre impostazioni dal pannello Reti virtuali nel portale di Azure. In alternativa, è possibile usare i comandi di PowerShell o i comandi in Cloud Shell per apportare modifiche.

![Screenshot del portale di Azure che mostra un pannello di esempio per la configurazione di una rete virtuale.](../media/2-configure-virtual-network.PNG)

È quindi possibile verificare e modificare le impostazioni in altri pannelli secondari.
Le impostazioni includono:

- Spazi di indirizzi

    È possibile aggiungere altri spazi degli indirizzi per la definizione iniziale

- Dispositivi connessi

    Usare la rete virtuale per connettere le macchine

- Subnet

    Aggiungere altre subnet

- Peering

    Collegare reti virtuali in modalità di peering

È anche possibile monitorare e risolvere i problemi delle reti virtuali o creare uno script di automazione per generare la rete virtuale corrente.

## <a name="summary"></a>Riepilogo

Le reti virtuali sono meccanismi avanzati e a configurabilità elevata per la connessione di entità in Azure. È possibile connettere le risorse di Azure tra loro o a risorse disponibili in locale. È possibile isolare, filtrare e instradare il traffico di rete e Azure consente di aumento della protezione in cui si ritiene necessario.
