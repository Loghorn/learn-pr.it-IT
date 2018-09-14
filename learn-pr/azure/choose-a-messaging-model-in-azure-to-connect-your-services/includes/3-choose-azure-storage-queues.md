Si supponga di dover pianificare l'architettura di un'applicazione per la condivisione di musica. Si vuole essere certi che i file musicali vengano caricati in modo affidabile nell'API Web dall'app per dispositivi mobili. È quindi necessario che i dettagli sui nuovi brani aggiunti da un artista alla propria raccolta vengano distribuiti direttamente nell'app. Si tratta di un uso ideale di un sistema basato su messaggi e Azure offre due soluzioni a questo problema:

- Archiviazione code di Azure
- Bus di servizio di Azure

## <a name="what-is-azure-queue-storage"></a>Che cos'è Archiviazione code di Azure?
Archiviazione code è un servizio che usa Archiviazione di Azure per archiviare grandi quantità di messaggi accessibili in modo sicuro ovunque nel mondo usando una semplice interfaccia basata su REST. Le code possono contenere milioni di messaggi, limitati solo dalla capacità dell'account di archiviazione che ne è proprietario.

## <a name="what-is-azure-service-bus-queues"></a>What ' s code del Bus di servizio di Azure?
Il bus di servizio è un sistema broker di messaggi destinato alle applicazioni aziendali. Queste app spesso utilizzano più protocolli di comunicazione, hanno contratti di dati diversi, requisiti di sicurezza più elevati e possono includere servizi sia cloud che locali. Il bus di servizio si basa su un'infrastruttura di messaggistica dedicata, progettata specificamente per questi scenari.

Questi servizi si basano entrambi sul concetto di una "coda" che contiene i messaggi inviato fino a quando la destinazione è pronta a riceverli.

## <a name="what-are-azure-service-bus-topics"></a>Quali sono gli argomenti del Bus di servizio di Azure?
Argomenti del Bus di servizio di Azure sono simili a code, ma possono avere più sottoscrittori. Quando viene inviato un messaggio a un argomento anziché una coda di più componenti possono essere attivati per svolgere il proprio lavoro. Si supponga in una condivisione dell'applicazione, un utente in ascolto di un brano di brani musicali. L'app per dispositivi mobili potrebbe inviare un messaggio all'argomento "Listened". Tale argomento avrà una sottoscrizione per una sottoscrizione diversa e "UpdateUserListenHistory" per "UpdateArtistsFanList". Ognuna di queste funzioni viene gestita da un altro componente che riceve la propria copia del messaggio.

Internamente, negli argomenti di usano le code. Quando si registra per un argomento, il messaggio viene copiato e rilasciato nella coda per ogni sottoscrizione. La coda significa che la copia del messaggio rimangono circa per essere elaborato **da ciascun ramo sottoscrizione** anche se il componente di elaborazione di tale sottoscrizione è troppo occupato per gestirla.

## <a name="benefits-of-queues"></a>Vantaggi delle code
Infrastrutture della coda possono supportare numerose funzionalità avanzate che rendono molto utili in 

### <a name="increased-reliability"></a>Maggiore affidabilità
Le code vengono usate dalle applicazioni distribuite come posizioni di archiviazione temporanee per i messaggi in attesa di recapito a un componente di destinazione. Il componente di origine può aggiungere un messaggio alla coda e i componenti di destinazione possono recuperare il messaggio all'inizio della coda per elaborarlo. Le code migliorano l'affidabilità dello scambio di messaggi perché, nei momenti di picco, i messaggi possono attendere semplicemente fino a quando un componente di destinazione è pronto per elaborarli.

### <a name="message-delivery-guarantees"></a>Garanzie di recapito dei messaggi
I sistemi di accodamento in genere garantiscono il recapito di ogni messaggio nella coda a un componente di destinazione. Per queste garanzie si possono adottare tuttavia approcci diversi:

- **Recapito At-Least-Once.** Con questo approccio, è garantito il recapito di ogni messaggio ad almeno uno dei componenti che recuperano i messaggi dalla coda. Si noti, tuttavia, che in determinate circostanze, è possibile che lo stesso messaggio venga recapitato più di una volta. Ad esempio, se sono presenti due istanze di un'app Web che recuperano i messaggi da una coda, in genere ogni messaggio viene recapitato a una sola di tali istanze. Tuttavia, se un'istanza richiede molto tempo per elaborare il messaggio e scade un timeout, il messaggio potrebbe essere inviato anche all'altra istanza. Il codice dell'app Web deve essere progettato tenendo in considerazione questa eventualità.

- **Recapito At-Most-Once.** Con questo approccio, il recapito di ogni messaggio non è garantito ed esiste una probabilità molto piccola che il messaggio possa non arrivare. Tuttavia, diversamente dal recapito At-Least-Once, non vi è alcuna possibilità che il messaggio venga recapitato due volte. Questo viene a volte definito "rilevamento duplicati automatico".

- **First-In-First-Out (FIFO).** Nella maggior parte dei sistemi di messaggistica, i messaggi in genere lasciano la coda nello stesso ordine in cui sono stati aggiunti, ma è necessario considerare se quest'ordine è garantito. Se l'applicazione distribuita richiede che i messaggi vengano elaborati con precisione nell'ordine corretto, è necessario scegliere un sistema di accodamento che include una garanzia FIFO.

### <a name="transactional-support"></a>Supporto transazionale
Alcuni gruppi di messaggi strettamente correlati possono causare problemi quando si verifica un errore di recapito per un messaggio nel gruppo.

Si consideri, ad esempio, un'applicazione di e-commerce. Quando un utente fa clic sul pulsante **Acquista**, una serie di messaggi potrebbe essere generata e inviata a diverse destinazioni di elaborazione:

- Un messaggio con i dettagli dell'ordine viene inviato a un centro di evasione degli ordini
- Un messaggio con i dettagli di pagamento e il totale viene inviato a un sistema di elaborazione di carte di credito. 
- Un messaggio con le informazioni sulla ricevuta viene inviato a un database per generare una fattura per il cliente

In questo caso si vuole essere certi che vengano elaborati _tutti_ i messaggi oppure nessuno. Non si resterà in affari a lungo se il messaggio relativo alla carta di credito non viene recapitato e tutti gli ordini vengono evasi senza pagamento. È possibile evitare questi tipi di problemi raggruppando i due messaggi in una transazione. Le transazioni contenenti i messaggi hanno esito positivo o negativo come singola unità, esattamente come nei database. Se il recapito del messaggio dei dettagli della carta di credito non riesce, anche il messaggio dei dettagli dell'ordine non verrà recapitato.

## <a name="which-service-should-i-choose"></a>Qual è il servizio da scegliere?
Dopo aver stabilito che la strategia di comunicazione per questa architettura deve essere un messaggio, è necessario scegliere se usare le code di archiviazione di Azure o il bus di servizio di Azure, entrambi servizi che possono essere usati per archiviare e recapitare messaggi tra i componenti. Ogni tecnologia presenta un set di funzionalità leggermente diverso, quindi è possibile scegliere l'una o l'altra, o entrambe, a seconda del problema da risolvere.

#### <a name="choose-service-bus-topics-if"></a>Scegliere gli argomenti del Bus di servizio se

- è necessario gestire ogni messaggio dei vari ricevitori


#### <a name="choose-service-bus-queues-if"></a>Scegliere le code del bus di servizio se:

- È necessaria una garanzia di recapito At-Most-Once.
- È necessaria una garanzia FIFO.
- È necessario raggruppare i messaggi in transazioni.
- Si vuole ricevere messaggi senza polling della coda.
- È necessario fornire alle code un modello di accesso basato sui ruoli.
- È necessario gestire messaggi di dimensioni superiori a 64 KB, ma inferiori a 256 KB.
- Le dimensioni della coda non supereranno gli 80 GB.
- Si vuole poter pubblicare e utilizzare batch di messaggi.

Archiviazione code non ha altrettante funzionalità, ma se tali funzionalità non sono necessarie, può essere una scelta più semplice. È inoltre la soluzione migliore se l'app presenta uno qualsiasi dei requisiti seguenti.

#### <a name="choose-queue-storage-if"></a>Scegliere Archiviazione code se:

- È necessario un audit trail di tutti i messaggi che passano attraverso la coda.
- Si prevede che le dimensioni della coda possano superare gli 80 GB.
- Si vuole tenere traccia dello stato dell'elaborazione di un messaggio all'interno della coda.

## <a name="summary"></a>Riepilogo

Una coda è una semplice posizione di archiviazione temporanea per i messaggi inviati tra i componenti di un'applicazione distribuita. Usare una coda per organizzare i messaggi e gestire correttamente i picchi nella domanda non prevedibili. 

Usare le code di archiviazione quando si vuole un sistema di accodamento semplice e facile da programmare. Per esigenze più avanzate, usare le code del bus di servizio. Se si dispone di più destinazioni per un singolo messaggio, ma è necessario un comportamento simile a code, usare gli argomenti.