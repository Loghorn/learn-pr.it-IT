Ora il sito è operativo in Azure. Come fare in modo che il sito rimanga in esecuzione ininterrottamente?

Cosa accade, ad esempio, quando è necessario eseguire la manutenzione settimanale? Il servizio continuerà a essere non disponibile durante il periodo di manutenzione. E poiché il sito raggiunge gli utenti in tutto il mondo, non esiste un buon momento per arrestare i sistemi per la manutenzione. È anche possibile riscontrare problemi di prestazioni se troppi utenti si connettono allo stesso tempo.

## <a name="what-are-availability-and-high-availability"></a>Che cosa sono la durabilità e la disponibilità elevata?

Il termine _disponibilità_ indica il periodo di tempo in cui il servizio rimane operativo senza interruzioni. Il termine _disponibilità elevata___ indica che un servizio rimane operativo per un lungo periodo di tempo.

Si pensi a un sito di social media o di notizie che si visita ogni giorno. È sempre possibile accedere al sito o spesso vengono visualizzati messaggi di errore, ad esempio "503 - Servizio non disponibile"? Tutti sanno quanto sia frustrante non poter accedere alle informazioni necessarie.

Si è già termini come "cinque nove disponibilità". Disponibilità cinque nove significa che l'esecuzione del servizio è garantita per il 99,999% del tempo. Nonostante sia difficile ottenere una disponibilità pari al 100%, molti team si impegnano per ottenere almeno quella cinque nove.

## <a name="what-is-resiliency"></a>Cos'è la resilienza?

Il termine _resilienza_ indica la capacità di un sistema di rimanere operativo in condizioni anomale.

Queste condizioni includono:

- Calamità naturali.
- Manutenzione dei sistemi, pianificate, inclusi gli aggiornamenti software e patch di sicurezza e non pianificate.
- Picchi di traffico del sito.
- Minacce effettuate da parti non autorizzate, ad esempio attacchi distributed denial of service, o, DDoS.

Si supponga che il team di marketing vuole avere una vendita lampo per alzare di livello una riga dei nuovi prodotti. È previsto un notevole picco di traffico durante questo periodo. Questo picco potrebbe sovraccaricare il sistema di elaborazione, fino a rallentarlo o interromperlo, creando disappunto tra gli utenti. Forse si è già provata questa delusione in prima persona. I ticket per un evento solo trovare che il sito Web non risponde non avete mai desiderato?

## <a name="what-is-a-load-balancer"></a>Che cos'è un servizio di bilanciamento del carico?

Un _servizio di bilanciamento del carico_ distribuisce uniformemente il traffico tra ogni sistema di un pool. Un servizio di bilanciamento del carico può consentire di ottenere la disponibilità elevata e la resilienza.

Si immagini di iniziare aggiungendo altre VM, ognuna configurata in modo identico, a ogni livello. L'idea è avere altri sistemi pronti, nel caso in cui uno è inattivo o gestisce Troppi utenti nello stesso momento.

Il problema è che ogni VM avrà il proprio indirizzo IP. Non è inoltre possibile distribuire il traffico, nel caso in cui un sistema diventi inattivo o sia occupato. Come connettere le VM in modo che per l'utente siano un unico sistema?

La risposta consiste nell'usare un servizio di bilanciamento del carico per distribuire il traffico. Il servizio di bilanciamento del carico diventa il punto di ingresso per l'utente. L'utente non sa (o non deve necessariamente sapere) quale sistema venga scelto dal servizio di bilanciamento del carico per ricevere la richiesta.

La figura seguente mostra il ruolo di un servizio di bilanciamento del carico.

![Un'illustrazione che mostra il livello web di un'architettura a tre livelli. Il livello web ha più macchine virtuali per soddisfare le richieste utente. È un servizio di bilanciamento del carico che distribuisce le richieste degli utenti tra le macchine virtuali.](../media/3-load-balancer.png)

Si noterà che il servizio di bilanciamento del carico riceve la richiesta dell'utente. Il servizio di bilanciamento del carico indirizza la richiesta a una delle VM nel livello Web. Se una VM non è disponibile o smette di rispondere, il servizio di bilanciamento del carico smette di inviarle il traffico. Il servizio di bilanciamento del carico indirizza quindi il traffico a uno dei server reattivi.

Il bilanciamento del carico consente di eseguire le attività di manutenzione senza interrompere il servizio. È ad esempio possibile distribuire la finestra di manutenzione per ogni VM. Durante la finestra di manutenzione, il servizio di bilanciamento del carico rileva che la macchina virtuale non risponde e indirizza il traffico ad altre macchine virtuali nel pool.

Per il sito di e-commerce, anche i livelli app e dati possono avere un servizio di bilanciamento del carico. Tutto dipende dai requisiti del servizio.

## <a name="what-is-azure-load-balancer"></a>Che cos'è Azure Load Balancer?

Azure Load Balancer è un servizio di bilanciamento del carico fornito da Microsoft.

È possibile configurare manualmente il software del servizio di bilanciamento carico in una macchina virtuale. Lo svantaggio è che ora si ha un altro sistema che richiede manutenzione. Se il servizio di bilanciamento del carico si arresta o richiede la manutenzione di routine, si ripresenta il problema originale.

In alternativa, è possibile usare Azure Load Balancer poiché non esiste alcuna infrastruttura o software per poter mantenere. Azure si occupa della manutenzione per l'utente.

La figura seguente mostra il ruolo di servizi di bilanciamento del carico di Azure in un'architettura a più livelli.

![Un'illustrazione che mostra il livello web di un'architettura a tre livelli. Il livello web ha più macchine virtuali per soddisfare le richieste utente. È un servizio di bilanciamento del carico che distribuisce le richieste degli utenti tra le macchine virtuali.](../media/3-azure-load-balancer.png)

## <a name="what-about-dns"></a>E DNS?

DNS (Domain Name System) è un modo per eseguire il mapping dei nomi descrittivi agli indirizzi IP. È possibile pensare DNS come la Rubrica di internet.

È ad esempio possibile eseguire il mapping del nome di dominio contoso.com all'indirizzo IP del servizio di bilanciamento del carico nel livello Web, 40.65.106.192.

È possibile usare il proprio server DNS oppure DNS di Azure, un servizio di hosting per i domini DNS che viene eseguito nell'infrastruttura di Azure.

La figura seguente mostra DNS di Azure. Quando l'utente passa a contoso.com, DNS di Azure instrada il traffico al servizio di bilanciamento del carico.

![Un'illustrazione che mostra il sistema di nome di dominio di Azure posizionato davanti al servizio di bilanciamento del carico.](../media/3-dns.png)

## <a name="summary"></a>Riepilogo

Con il bilanciamento del carico il sito di e-commerce è ora a disponibilità più elevata e più resiliente. Quando si esegue la manutenzione o si verifica un aumento del traffico, il servizio di bilanciamento del carico può distribuire il traffico a un altro sistema disponibile.

Sebbene sia possibile configurare il proprio servizio di bilanciamento del carico in una macchina virtuale, Azure Load Balancer consente di ridurre manutenzione poiché non esiste alcuna infrastruttura o software da gestire.

DNS esegue il mapping dei nomi descrittivi agli indirizzi IP, in modo simile a una rubrica che esegue il mapping dei nomi di persone o aziende ai numeri di telefono. È possibile portare il proprio server DNS o usare DNS di Azure.
