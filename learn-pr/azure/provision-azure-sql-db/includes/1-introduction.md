La gestione dei dati è un componente di importanza critica per qualsiasi azienda. I database relazionali, e in particolare Microsoft SQL Server, sono stati per decenni tra gli strumenti più comuni per la gestione di questi dati. 

Per gestire i dati tramite il cloud, è _possibile_ usare semplicemente macchine virtuali di Azure per ospitare le proprie istanze di Microsoft SQL Server. In alcuni casi, si tratta della soluzione ideale. Tuttavia, Azure offre anche un altro metodo che spesso è più semplice e conveniente. I database SQL di Azure sono una piattaforma distribuita come servizio (PaaS), il che significa che l'infrastruttura e la manutenzione da gestire si riducono notevolmente.

Per comprendere meglio questo concetto, si consideri lo scenario seguente. Si è un responsabile dello sviluppo software presso una società di logistica, Contoso Transport.

Il settore dei trasporti richiede un notevole coordinamento tra tutte le persone coinvolte: responsabili della pianificazione, distributori, autisti e persino clienti.

Il processo attuale prevede pile di moduli cartacei e ore al telefono per coordinare le spedizioni. Si rileva che nei documenti cartacei spesso mancano le firme e i distributori spesso non sono disponibili. A causa di questi problemi, gli autisti devono restare inattivi, con il risultato che spesso spedizioni importanti subiscono ritardi. La soddisfazione del cliente e la ripetizione degli ordini sono fondamentali per i profitti dell'azienda.

Il team decide di passare da moduli cartacei e chiamate telefoniche a documenti digitali e comunicazioni online. L'uso di un sistema digitale consentirà a tutti gli interessati di coordinarsi e tenere traccia dei tempi di spedizione tramite un Web browser o un'app per dispositivi mobili.

Si vuole creare rapidamente un prototipo da condividere con il team. Il prototipo includerà un database per le informazioni sugli autisti, i clienti e gli ordini. Il prototipo verrà usato come base per l'app di produzione. Di conseguenza, le scelte relative alle tecnologie effettuate in questa fase devono essere correlate al prodotto finale che verrà realizzato dal team.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo si apprenderà:

- Perché il database SQL di Azure rappresenta una scelta ottimale per l'esecuzione del database relazionale
- Quali opzioni di configurazione e di prezzo sono disponibili per il database SQL di Azure
- Come creare un database SQL di Azure dal portale
- Come usare Azure Cloud Shell per connettersi al database SQL di Azure, aggiungere una tabella e usare i dati