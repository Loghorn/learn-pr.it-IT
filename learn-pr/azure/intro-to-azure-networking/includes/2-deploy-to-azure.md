La prima operazione sarà probabilmente ricreare la configurazione locale nel cloud.

Questa configurazione di base offre un'idea di come vengono configurate le reti e di come il traffico di rete entra ed esce da Azure.

## <a name="your-e-commerce-site-at-a-glance"></a>Riepilogo del sito di e-commerce

I sistemi aziendali più grandi sono spesso costituiti da più applicazioni e servizi interconnessi che interagiscono tra loro. Potrebbe essere presente un sistema Web front-end che visualizza l'inventario e consente ai clienti di creare un ordine. Tale sistema potrebbe comunicare con svariati servizi Web per fornire i dati dell'inventario, gestire i profili utente, elaborare le carte di credito e richiedere l'evasione degli ordini elaborati.

Esistono diverse strategie e modelli adottati dagli architetti e progettisti di software per semplificare la progettazione, la compilazione, la gestione e la manutenzione di questi sistemi complessi. Ne verranno analizzati alcuni, tra cui le _architetture a regime di controllo libero_.

### <a name="benefits-of-a-loosely-coupled-architecture"></a>Vantaggi di un'architettura a regime di controllo libero

<!-- TOOD: verify video -->
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2yHrc]

### <a name="using-an-n-tier-architecture"></a>Uso di un'architettura a più livelli

Un modello architettonico che può essere usato per compilare sistemi a regime di controllo libero è _a più livelli_.

Un'[architettura a più livelli](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/n-tier) consente di dividere un'applicazione in due o più livelli logici. Dal punto di vista dell'architettura, un livello superiore può accedere ai servizi da un livello inferiore, ma un livello inferiore non deve mai accedere a un livello superiore.

I livelli consentono di separare le problematiche e idealmente sono progettati in modo da poter essere usati più volte. L'uso di un'architettura a livelli semplifica anche la manutenzione. I livelli possono essere aggiornati o sostituiti in modo indipendente e, se necessario, si possono inserire nuovi livelli.

_A tre livelli_ si riferisce a un'applicazione a più livelli che include tre livelli. L'applicazione Web di e-commerce segue questa architettura a tre livelli:

* Il **livello Web** rende disponibile agli utenti l'interfaccia Web in un browser.
* Il **livello applicazione** esegue la logica di business.
* Il **livello dati** include i database e altre risorse di archiviazione che contengono le informazioni sui prodotti e gli ordini dei clienti.

La figura seguente illustra il flusso della richiesta dall'utente al livello dati.

![Figura che illustra un'architettura a tre livelli in cui ogni livello è ospitato in una macchina virtuale dedicata.](../media/2-three-tier.png)

Quando l'utente fa clic sul pulsante per effettuare l'ordine, la richiesta viene inviata al livello Web, insieme all'indirizzo dell'utente e alle informazioni di pagamento. Il livello Web passa queste informazioni al livello applicazione per la convalida delle informazioni di pagamento e il controllo dell'inventario. Il livello applicazione può quindi archiviare l'ordine nel livello dati in modo da selezionarlo in un secondo tempo per evadere l'ordine.

## <a name="your-e-commerce-site-running-on-azure"></a>Il sito di e-commerce in esecuzione in Azure

Azure offre diversi modi per ospitare le applicazioni Web, da ambienti completamente preconfigurati che ospitano il codice per le macchine virtuali configurate, personalizzate e gestite dall'utente.

Si supponga di scegliere di eseguire il sito di e-commerce in macchine virtuali. Di seguito è riportato un esempio di ciò che potrebbe apparire nell'ambiente di test in esecuzione in Azure. La figura seguente mostra un'architettura a tre livelli in esecuzione in macchine virtuali con funzionalità di sicurezza abilitate per limitare le richieste in ingresso. 

![Figura che illustra un'architettura a tre livelli in cui ogni livello è in esecuzione in una macchina virtuale separata. Ogni macchina virtuale è etichettata con l'indirizzo IP e si trova all'interno della propria rete virtuale. Ogni rete virtuale ha un gruppo di sicurezza di rete in cui sono elencate le porte aperte.](../media/2-test-deployment.png)

L'app si può scomporre come segue.

### <a name="what-is-an-azure-region"></a>Informazioni sull'area di Azure

Un'_area_ è un data center di Azure in un'area geografica specifica. Stati Uniti orientali, Stati Uniti occidentali ed Europa settentrionale sono esempi di aree. Come si può notare, l'applicazione viene eseguita nell'area Stati Uniti orientali.

### <a name="what-is-a-virtual-network"></a>Informazioni sulla rete virtuale

Una _rete virtuale_ è una rete isolata logicamente in Azure. L'utente avrà dimestichezza con le reti virtuali di Azure se ha configurato le reti in Hyper-V, VMware o anche in altri cloud pubblici.

Ogni livello Web, applicazione e dati ha un'unica macchina virtuale. Ogni macchina virtuale appartiene a una rete virtuale.

Gli utenti interagiscono con il livello Web direttamente, quindi la macchina virtuale ha un indirizzo IP pubblico. Gli utenti non interagiscono con i livelli dati o applicazione. Quindi queste macchine virtuali hanno un indirizzo IP privato.

I data center di Azure gestiscono l'hardware fisico per l'utente. Le reti virtuali si configurano usando il software, che consente di gestire una rete virtuale esattamente come la propria rete. Ad esempio, è possibile suddividere una rete virtuale in subnet per controllare meglio il modo in cui la rete assegna gli indirizzi IP. È anche possibile scegliere quali altre reti può raggiungere la rete virtuale, se la rete Internet pubblica o altre reti nello spazio indirizzi IP privato.

### <a name="whats-a-network-security-group"></a>Informazioni sul gruppo di sicurezza di rete

Un _gruppo di sicurezza di rete_, o NSG, consente o nega il traffico di rete in ingresso alle risorse di Azure. Un gruppo di sicurezza di rete può essere paragonato a un firewall a livello cloud per la rete.

Si può notare ad esempio che la macchina virtuale nel livello Web consente il traffico in ingresso sulle porte 22 (SSH) e 80 (HTTP). Ogni gruppo di sicurezza di rete qui consente il traffico da tutte le origini. È possibile configurare un gruppo di sicurezza di rete per accettare il traffico solo da origini note, ad esempio gli indirizzi IP considerati attendibili.

> [!NOTE]
> La porta 22 consente di connettersi direttamente ai sistemi Linux con SSH. Di seguito è illustrata la porta 22 aperta ai fini dell'apprendimento. In pratica, è possibile configurare l'accesso VPN alla rete virtuale per avere una maggiore protezione.

## <a name="summary"></a>Riepilogo

L'applicazione a tre livelli è ora in esecuzione in Azure nell'area Stati Uniti orientali. Un'_area_ è un data center di Azure in un'area geografica specifica.

Ogni livello può accedere ai servizi solo da un livello inferiore. La macchina virtuale in esecuzione nel livello Web ha un indirizzo IP pubblico perché riceve il traffico da Internet. Le macchine virtuali nei livelli inferiori, ovvero i livelli applicazione e dati, hanno tutte indirizzi IP privati perché non comunicano direttamente su Internet.

Le _reti virtuali_ consentono di raggruppare e isolare i sistemi correlati. Si definiscono i _gruppi di sicurezza di rete_ per stabilire quale traffico può passare attraverso una rete virtuale.

La configurazione illustrata qui è un buon punto di partenza. Quando tuttavia si distribuisce il sito di e-commerce nell'ambiente di produzione nel cloud, è probabile che si incontrino gli stessi problemi incontrati nella distribuzione a livello locale.
