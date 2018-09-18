Ora il sito è operativo in Azure. Come fare in modo che il sito rimanga in esecuzione ininterrottamente?

Cosa accade, ad esempio, quando è necessario eseguire la manutenzione settimanale? Il servizio continuerà a essere non disponibile durante il periodo di manutenzione. E poiché il sito raggiunge gli utenti in tutto il mondo, non esiste un buon momento per arrestare i sistemi per la manutenzione. È anche possibile riscontrare problemi di prestazioni se troppi utenti si connettono allo stesso tempo.

## <a name="what-are-availability-and-high-availability"></a>Che cosa sono la durabilità e la disponibilità elevata?

Il termine _disponibilità_ indica il periodo di tempo in cui il servizio rimane operativo senza interruzioni. Il termine _disponibilità elevata_, oppure _disponibilità elevata_indica che un servizio rimane operativo per un lungo periodo di tempo.

Si pensi a un sito di social media o di notizie che si visita ogni giorno. È sempre possibile accedere al sito o spesso vengono visualizzati messaggi di errore, ad esempio "503 - Servizio non disponibile"? È notoriamente frustrante non poter accedere alle informazioni necessarie.

È probabile che si siano già sentite espressioni come "disponibilità cinque nove". Disponibilità cinque nove significa che l'esecuzione del servizio è garantita per il 99,999% del tempo. Nonostante sia difficile ottenere una disponibilità pari al 100%, molti team si impegnano per ottenere almeno quella cinque nove.

## <a name="what-is-resiliency"></a>Cos'è la resilienza?

Il termine _resilienza_ indica la capacità di un sistema di rimanere operativo in condizioni anomale.

Queste condizioni includono:

- Calamità naturali.
- Manutenzione del sistema, sia pianificata che non pianificata, con aggiornamenti software e patch di sicurezza.
- Picchi di traffico verso il sito.
- Minacce effettuate da malintenzionati, ad esempio attacchi Distributed Denial of Service (DDoS).

Si immagini che il team di marketing voglia organizzare una vendita lampo per promuovere una linea di nuovi prodotti. Durante questo periodo si può prevedere un notevole picco di traffico. Questo picco potrebbe sovraccaricare il sistema di elaborazione, fino a rallentarlo o interromperlo, creando disappunto tra gli utenti. È probabile che si sia già provata questa delusione in prima persona. È mai capitato di voler acquistare i biglietti per un evento e scoprire che il sito Web non risponde?

## <a name="what-is-a-load-balancer"></a>Informazioni sui servizi di bilanciamento del carico

Un _servizio di bilanciamento del carico_ distribuisce uniformemente il traffico tra ogni sistema di un pool. Un servizio di bilanciamento del carico può consentire di ottenere la disponibilità elevata e la resilienza.

Si potrebbe iniziare aggiungendo a ogni livello altre VM, ognuna con configurazione identica. L'obiettivo è avere altri sistemi pronti nel caso in cui uno diventi inattivo o gestisca troppi utenti contemporaneamente.

Il problema in questo caso è che ogni VM avrà il proprio indirizzo IP. Non è inoltre possibile distribuire il traffico, nel caso in cui un sistema diventi inattivo o sia occupato. Come connettere le VM in modo che per l'utente siano un unico sistema?

La risposta consiste nell'usare un servizio di bilanciamento del carico per distribuire il traffico. Il servizio di bilanciamento del carico diventa il punto di ingresso per l'utente. L'utente non sa (o non deve necessariamente sapere) quale sistema venga scelto dal servizio di bilanciamento del carico per ricevere la richiesta.

Di seguito è riportato un diagramma.

![Diagramma del bilanciamento del carico del traffico tra macchine virtuali](../media-draft/load-balancer.png)

Si noterà che il servizio di bilanciamento del carico riceve la richiesta dell'utente. Il servizio di bilanciamento del carico indirizza la richiesta a una delle VM nel livello Web. Se una VM non è disponibile o smette di rispondere, il servizio di bilanciamento del carico smette di inviarle il traffico. Il servizio di bilanciamento del carico indirizza quindi il traffico a uno dei server reattivi.

Il bilanciamento del carico consente di eseguire le attività di manutenzione senza interrompere il servizio. È ad esempio possibile sfalsare la finestra di manutenzione per ogni VM. Durante la finestra di manutenzione, il servizio di bilanciamento del carico rileva che la VM non risponde e indirizza il traffico ad altre VM nel pool.

Per un sito di e-commerce, anche i livelli app e dati possono avere un servizio di bilanciamento del carico. Tutto dipende dai requisiti del servizio.

## <a name="what-is-azure-load-balancer"></a>Che cos'è Azure Load Balancer?

Azure Load Balancer è un servizio di bilanciamento del carico fornito da Microsoft.

È possibile configurare manualmente il software del servizio di bilanciamento carico in una macchina virtuale. Lo svantaggio è che ora si ha un altro sistema che richiede manutenzione. Se il servizio di bilanciamento del carico si arresta o necessita di manutenzione di routine, si ripresenta il problema originale.

In alternativa, è possibile usare Azure Load Balancer perché non è necessario occuparsi della manutenzione di infrastrutture o software. La manutenzione viene eseguita da Azure.

Di seguito è riportato un diagramma che mostra diverse VM in ogni livello. Ogni livello include Azure Load Balancer, che distribuisce il traffico tra le VM nel pool.

![Diagramma del bilanciamento del carico del traffico tra macchine virtuali con Azure Load Balancer](../media-draft/azure-load-balancer.png)

## <a name="what-about-dns"></a>Informazioni sul DNS

Il DNS (Domain Name System) è un modo per eseguire il mapping dei nomi descrittivi ai rispettivi indirizzi IP. Si può immaginare il DNS come una rubrica di Internet.

Il nome di dominio contoso.com, ad esempio, potrebbe essere mappato all'indirizzo IP del servizio di bilanciamento del carico nel livello Web, 40.65.106.192.

È possibile usare il proprio server DNS oppure DNS di Azure, un servizio di hosting per i domini DNS che viene eseguito nell'infrastruttura di Azure.

Ecco un diagramma che mostra DNS di Azure. Quando l'utente passa a contoso.com, DNS di Azure indirizza il traffico al servizio di bilanciamento del carico.

![Diagramma dell'uso di DNS di Azure per assegnare un nome DNS](../media-draft/dns.png)

## <a name="summary"></a>Riepilogo

Con il bilanciamento del carico il sito di e-commerce è ora a disponibilità più elevata e più resiliente. In caso di manutenzione o un aumento del traffico, il servizio di bilanciamento del carico può distribuire il traffico a un altro sistema disponibile.

Anche se è possibile configurare un servizio di bilanciamento del carico personalizzato in una VM, Azure Load Balancer riduce le attività di manutenzione perché non sono presenti infrastrutture o software che la richiedono.

Il DNS esegue il mapping dei nomi descrittivi ai rispettivi indirizzi IP, così come una rubrica associa i nomi di persone o aziende ai numeri di telefono. È possibile usare un proprio server DNS o DNS di Azure.
