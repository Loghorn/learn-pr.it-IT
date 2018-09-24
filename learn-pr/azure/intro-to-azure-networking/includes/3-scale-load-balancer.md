Ora il sito è operativo in Azure. Ma come fare in modo che il sito rimanga in esecuzione ininterrottamente?

Cosa accade, ad esempio, quando è necessario eseguire la manutenzione settimanale? Il servizio continuerà a essere non disponibile durante il periodo di manutenzione. E poiché il sito raggiunge gli utenti in tutto il mondo, non esiste un buon momento per arrestare i sistemi per la manutenzione. È anche possibile riscontrare problemi di prestazioni se troppi utenti si connettono allo stesso tempo.

## <a name="what-are-availability-and-high-availability"></a>Che cosa sono la durabilità e la disponibilità elevata?

:::row:::
  :::column:::
    ![Treno veloce che rappresenta la disponibilità elevata](../media/3-availability.png)
  :::column-end:::
    :::column span="3"::: Il termine _disponibilità_ indica il periodo di tempo in cui il servizio rimane operativo senza interruzioni. Il termine _disponibilità elevata_ oppure _a disponibilità elevata_ indica che un servizio rimane operativo per un lungo periodo di tempo.

È notoriamente frustrante non poter accedere alle informazioni necessarie. Si pensi a un sito di social media o di notizie che si visita ogni giorno. È sempre possibile accedere al sito o spesso vengono visualizzati messaggi di errore, ad esempio "503 - Servizio non disponibile"?
  :::column-end:::
 :::row-end:::

È probabile che si siano già sentite espressioni come "disponibilità cinque nove". Disponibilità cinque nove significa che l'esecuzione del servizio è garantita per il 99,999% del tempo. Nonostante sia difficile ottenere una disponibilità pari al 100%, molti team si impegnano per ottenere almeno quella cinque nove.

## <a name="what-is-resiliency"></a>Cos'è la resilienza?

:::row:::
  :::column:::
    ![Cartella clinica che rappresenta la resilienza](../media/3-resiliency.png)
  :::column-end:::
    :::column span="3"::: Il termine _resilienza_ indica la capacità di un sistema di rimanere operativo in condizioni anomale.

Queste condizioni includono:

- Calamità naturali.
- Manutenzione del sistema, sia pianificata che non pianificata, con aggiornamenti software e patch di sicurezza.
- Picchi di traffico verso il sito.
- Minacce effettuate da malintenzionati, ad esempio attacchi Distributed Denial of Service (DDoS).
  :::column-end:::
:::row-end:::

Si immagini che il team di marketing voglia organizzare una vendita lampo per promuovere una nuova linea di integratori vitaminici. Durante questo periodo si può prevedere un notevole picco di traffico. Questo picco potrebbe sovraccaricare il sistema di elaborazione, fino a rallentarlo o interromperlo, creando disappunto tra gli utenti. È probabile che si sia già provata questa delusione in prima persona. È mai capitato di provare ad accedere a una vendita online e scoprire che il sito Web non risponde?

## <a name="what-is-a-load-balancer"></a>Informazioni sui servizi di bilanciamento del carico

:::row:::
  :::column:::
    ![Bilancia che rappresenta il bilanciamento del carico](../media/3-lb.png)
  :::column-end:::
    :::column span="3"::: Un _servizio di bilanciamento del carico_ distribuisce uniformemente il traffico tra ogni sistema di un pool. Un servizio di bilanciamento del carico può consentire di ottenere la disponibilità elevata e la resilienza.

Si potrebbe iniziare aggiungendo a ogni livello altre VM, ognuna con configurazione identica. L'obiettivo è avere altri sistemi pronti nel caso in cui uno diventi inattivo o gestisca troppi utenti contemporaneamente.
  :::column-end:::
:::row-end:::

Il problema in questo caso è che ogni VM avrà il proprio indirizzo IP. Non è inoltre possibile distribuire il traffico, nel caso in cui un sistema diventi inattivo o sia occupato. Come connettere le VM in modo che per l'utente siano un unico sistema?

La risposta consiste nell'usare un servizio di bilanciamento del carico per distribuire il traffico. Il servizio di bilanciamento del carico diventa il punto di ingresso per l'utente. L'utente non sa (o non deve necessariamente sapere) quale sistema venga scelto dal servizio di bilanciamento del carico per ricevere la richiesta.

L'illustrazione seguente mostra il ruolo di un servizio di bilanciamento del carico.

![Figura che mostra il livello Web di un'architettura a tre livelli. Il livello Web ha più macchine virtuali per gestire le richieste degli utenti. Un servizio di bilanciamento del carico distribuisce le richieste tra le macchine virtuali.](../media/3-load-balancer.png)

Si noterà che il servizio di bilanciamento del carico riceve la richiesta dell'utente. Il servizio di bilanciamento del carico indirizza la richiesta a una delle VM nel livello Web. Se una VM non è disponibile o smette di rispondere, il servizio di bilanciamento del carico smette di inviarle il traffico. Il servizio di bilanciamento del carico indirizza quindi il traffico a uno dei server reattivi.

Il bilanciamento del carico consente di eseguire le attività di manutenzione senza interrompere il servizio. È ad esempio possibile sfalsare la finestra di manutenzione per ogni VM. Durante la finestra di manutenzione, il servizio di bilanciamento del carico rileva che la VM non risponde e indirizza il traffico ad altre VM nel pool.

Per un sito di e-commerce, anche i livelli app e dati possono avere un servizio di bilanciamento del carico. Tutto dipende dai requisiti del servizio.

## <a name="what-is-azure-load-balancer"></a>Che cos'è Azure Load Balancer?

Azure Load Balancer è un servizio di bilanciamento del carico fornito da Microsoft, che esegue automaticamente la manutenzione.

Quando si configura manualmente un software di bilanciamento del carico tipico in una macchina virtuale, esiste lo svantaggio di dover eseguire la manutenzione di un altro sistema. Se il servizio di bilanciamento del carico si arresta o necessita di manutenzione di routine, si ripresenta il problema originale.

Se tuttavia si usa in alternativa Azure Load Balancer, non è necessario occuparsi della manutenzione di infrastrutture o software.

L'illustrazione seguente mostra il ruolo dei servizi di bilanciamento del carico di Azure in un'architettura multilivello.

![Figura che mostra il livello Web di un'architettura a tre livelli. Il livello Web ha più macchine virtuali per gestire le richieste degli utenti. Un servizio di bilanciamento del carico distribuisce le richieste tra le macchine virtuali.](../media/3-azure-load-balancer.png)

## <a name="what-about-dns"></a>Informazioni sul DNS

:::row:::
  :::column:::
    ![Puntina su una mappa che rappresenta il DNS](../media/3-map-pin.png)
  :::column-end:::
    :::column span="3"::: Il DNS (Domain Name System) è un modo per eseguire il mapping dei nomi descrittivi ai rispettivi indirizzi IP. Si può immaginare il DNS come una rubrica di Internet.

Il nome di dominio contoso.com, ad esempio, potrebbe essere mappato all'indirizzo IP del servizio di bilanciamento del carico nel livello Web, 40.65.106.192.

È possibile usare il proprio server DNS oppure DNS di Azure, un servizio di hosting per i domini DNS che viene eseguito nell'infrastruttura di Azure.
  :::column-end:::
:::row-end:::

Nella figura seguente viene illustrato DNS di Azure. Quando l'utente passa a contoso.com, DNS di Azure indirizza il traffico al servizio di bilanciamento del carico.

![Figura che mostra Azure Domain Name System davanti al servizio di bilanciamento del carico.](../media/3-dns.png)

## <a name="summary"></a>Riepilogo

Con il bilanciamento del carico il sito di e-commerce è ora a disponibilità più elevata e più resiliente. In caso di manutenzione o un aumento del traffico, il servizio di bilanciamento del carico può distribuire il traffico a un altro sistema disponibile.

Anche se è possibile configurare un servizio di bilanciamento del carico personalizzato in una VM, Azure Load Balancer riduce le attività di manutenzione perché non sono presenti infrastrutture o software che la richiedono.

Il DNS esegue il mapping dei nomi descrittivi ai rispettivi indirizzi IP, così come una rubrica associa i nomi di persone o aziende ai numeri di telefono. È possibile usare un proprio server DNS o DNS di Azure.
