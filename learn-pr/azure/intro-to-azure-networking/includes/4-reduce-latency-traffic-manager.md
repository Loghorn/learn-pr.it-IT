In precedenza si è visto come **Azure Load Balancer** consente di ottenere la disponibilità elevata e ridurre al minimo il tempo di inattività.

Anche se il sito di e-commerce presenta una disponibilità più elevata, non risolve il problema della latenza o crea resilienza nelle varie aree geografiche.

Come si può fare in modo che il sito, che si trova negli Stati Uniti, venga caricato più rapidamente per gli utenti che si trovano in Europa o Asia?

## <a name="what-is-network-latency"></a>Che cos'è la latenza di rete?

Il termine _latenza_ indica il tempo necessario perché i dati vengano trasferiti attraverso la rete. La latenza viene in genere misurata in millisecondi.

Confrontare la latenza con la larghezza di banda. Per larghezza di banda si intende la quantità di dati che possono essere contenuti nella connessione. Per latenza si intende il tempo necessario affinché tali dati raggiungano la destinazione finale.

Sulla latenza possono influire fattori come il tipo di connessione usata e il modo in cui è stata progettata l'applicazione. Il fattore principale è probabilmente la distanza.

Si pensi al sito di e-commerce in Azure situato nell'area degli Stati Uniti orientali. In genere sarebbe necessario meno tempo per trasferire i dati ad Atlanta (a una distanza di circa 400 miglia) che per trasferirli a Londra (a una distanza di circa 4.000 miglia).

Il sito di e-commerce offre HTML, CSS, JavaScript e immagini standard. La latenza di rete per più file può risultare superiore. Come si può ridurre la latenza per gli utenti che si trovano geograficamente lontani?

## <a name="scale-out-to-different-regions"></a>Applicare la scalabilità orizzontale a diverse regioni

Si ricorda che Azure offre data center in tutto il mondo.

Tenere in considerazione il costo della creazione di un data center. I costi di apparecchiature non sono l'unico fattore. È necessario fornire l'alimentazione, raffreddamento e personale per mantenere i sistemi in esecuzione in ogni posizione. La replica dell'intero data center potrebbe quindi risultare eccessivamente dispendiosa. Ma in questo modo Azure può costare molto meno, Azure le attrezzature e il personale ha già posto.

Un modo per ridurre la latenza è offrire copie esatte del servizio in più aree. HThe figura seguente mostra un esempio di distribuzione globale.

![Un'illustrazione che mostra una mappa del mondo con tre data center di Azure evidenziato. Ogni data center è etichettata con un nome di dominio univoco.](../media/4-global-deployment.png)

Il diagramma illustra il sito di e-commerce in esecuzione in tre aree di Azure: Stati Uniti orientali, Europa settentrionale e Asia orientale. Si noti il nome DNS per ognuna di esse. Come è possibile connettere gli utenti al servizio che si trova geograficamente più vicino ma all'interno del dominio contoso.com?

## <a name="use-traffic-manager-to-route-users-to-the-closest-endpoint"></a>Usare Gestione traffico per instradare gli utenti all'endpoint più vicino

È una risposta **gestione traffico di Azure**. Gestione traffico usa il server DNS più vicino all'utente per indirizzare il traffico dell'utente verso un endpoint distribuito a livello globale. La figura seguente mostra il ruolo di gestione traffico.

![Un'illustrazione che mostra gestione traffico di Azure il routing di una richiesta dell'utente per il data center più vicino. ](../media/4-traffic-manager.png)

Gestione traffico non vede il traffico tra il client e il server. Piuttosto, indirizza il web browser client per un endpoint preferito. Gestione traffico possibile instradare il traffico in modi diversi, ad esempio per l'endpoint con latenza più bassa.

Sebbene non illustrato di seguito, questa configurazione può includere anche la distribuzione in locale in esecuzione in California. È possibile connettersi gestione traffico per le proprie reti locali, potrai mantenere i tuoi investimenti esistenti centro dati. È anche possibile spostare l'applicazione interamente nel cloud. La scelta spetta all'utente.

## <a name="compare-load-balancer-to-traffic-manager"></a>Confrontare il bilanciamento del carico a gestione traffico

Azure Load Balancer distribuisce il traffico nella stessa area per rendere i servizi a disponibilità più elevata e più resilienti. Gestione traffico lavora a livello di DNS e indirizza al client di endpoint preferito. Tale endpoint può essere l'area più vicina agli utenti.

Bilanciamento del carico e gestione traffico entrambi consentono di rendere i servizi più resiliente, ma in modi leggermente diversi. Quando il servizio di bilanciamento del carico rileva una macchina virtuale non risponda, indirizza il traffico ad altre macchine virtuali nel pool. Gestione traffico monitora l'integrità degli endpoint. Al contrario, quando Gestione traffico rileva un endpoint che non risponde, indirizza il traffico all'endpoint più vicino successivo che è reattivo.

## <a name="summary"></a>Riepilogo

La distanza geografica è uno dei fattori principali che contribuisce alla latenza. Con Gestione traffico è possibile ospitare le copie esatte del servizio in più aree geografiche. In questo modo, gli utenti che si trovano in Stati Uniti, Europa e Asia avranno un'esperienza ottimale nell'uso del sito di e-commerce.
