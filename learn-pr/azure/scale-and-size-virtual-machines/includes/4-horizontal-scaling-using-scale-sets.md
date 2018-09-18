Per ottenere le risorse necessarie, è possibile usare un'unica macchina virtuale di grandi dimensioni oppure più macchine virtuali di dimensioni inferiori con un bilanciamento del carico che distribuisce le richieste tra le VM.

Il pool di macchine virtuali ha il vantaggio di consentire l'aggiunta o la rimozione rapida di macchine virtuali a seconda delle richieste. Nello scenario dell'azienda che vende giocattoli, questa strategia sarebbe utile per gestire i picchi imprevisti di richieste. Si potrebbero aggiungere macchine virtuali al pool quando il numero di richieste aumenta e rimuoverle quando torna alla normalità. Il pool assicura anche la ridondanza: in caso di guasto di una macchina virtuale, le altre possono continuare a gestire le richieste senza alcuna interruzione del servizio.

In questa sezione vedremo come effettuare il provisioning di più macchine virtuali usando i set di scalabilità e come aggiungere e rimuovere automaticamente istanze al variare dei volumi delle richieste. 

## <a name="what-is-horizontal-scaling"></a>Che cos'è la scalabilità orizzontale?

La *scalabilità orizzontale* è il processo di aggiunta o rimozione di macchine virtuali da un pool per regolare la quantità di risorse disponibili. L'aggiunta di macchine virtuali è detta _aumento_ e la rimozione è detta _riduzione_. Le soluzioni che usano la scalabilità orizzontale includono un servizio di bilanciamento del carico o un gateway che distribuisce le richieste tra le macchine virtuali del pool. La figura seguente mostra un esempio di modifica del numero di istanze di macchine virtuali.

![Illustrazione che mostra l'aumento del numero di risorse per gestire la domanda e la riduzione del numero di risorse per ridurre i costi.](../media/4-ScaleInOut.png)

Questa tecnica è ottimale per le applicazioni che possono essere eseguite su più server identici. Ad esempio, se si duplica il server Web e le pagine Web su più macchine virtuali, daranno la stessa risposta indipendentemente dal server che riceve la richiesta. Una macchina virtuale che esegue il database back-end non è invece un candidato ideale, in quanto l'esecuzione di più copie del database implica la necessità di mantenere tali copie sincronizzate.

## <a name="what-is-a-scale-set"></a>Che cos'è un set di scalabilità?

Un *set di scalabilità* è costituito da un pool di macchine virtuali identiche, un servizio di bilanciamento del carico o un gateway che distribuisce le richieste e un set di regole facoltativo che controlla quando le macchine virtuali vengono aggiunte o rimosse dal pool. In questo contesto "identiche" significa che ogni macchina virtuale nel set viene creata con la stessa immagine e con le stesse dimensioni.

La configurazione di ogni nuova macchina virtuale con il software necessario è invece più flessibile. È possibile iniziare con un'immagine predefinita del sistema operativo di base e quindi usare script per installare o copiare i file automaticamente dopo l'installazione del sistema operativo. In alternativa, è possibile creare un'immagine di macchina virtuale personalizzata con il sistema operativo e il software applicativo già installati.

## <a name="how-to-distribute-requests"></a>Come distribuire le richieste

Per distribuire le richieste alle istanze di macchina virtuale in un set di scalabilità, è possibile usare un servizio di bilanciamento del carico o un gateway applicazione.

Un servizio di bilanciamento del carico di Azure opera al livello OSI 4 (TCP e UDP) ed esegue il routing del traffico da un indirizzo IP e una porta di origine a un indirizzo IP e una porta di destinazione. Può fornire affinità, che consiste nel routing del traffico proveniente dallo stesso indirizzo IP di origine allo stesso server di destinazione, per garantire la coerenza in una sessione client. Il servizio di bilanciamento del carico ha anche un meccanismo di probe integrità che determina la disponibilità delle istanze server. Se una macchina virtuale smette di rispondere al probe integrità, il servizio di bilanciamento del carico eviterà di eseguire il routing di nuove connessioni a tale VM.

Un gateway applicazione opera al livello OSI 7, ossia il livello dell'applicazione. Ad esempio, se le macchine virtuali eseguono un server Web, il gateway può usare l'URL richiesto per eseguire il routing. Questo significa che è possibile inoltrare le richieste con `*/customers*` nell'URL a un pool di server e le richieste con `*/partners*` nell'URL a un altro pool. Il gateway applicazione può anche fornire il reindirizzamento da HTTP a HTTPS, la terminazione SSL (Secure Sockets Layer) per ridurre i requisiti di elaborazione sulle macchine virtuali per la crittografia e un web application firewall (WAF) che usa regole per rilevare gli exploit Web noti ed evitare che le richieste di questo tipo raggiungano i server Web.

## <a name="what-is-autoscaling"></a>Che cos'è la scalabilità automatica?

La _scalabilità automatica_ è il processo di aumento o riduzione automatica in base a un set di regole. Le regole possono essere attivate dal carico delle macchine virtuali o in base a una pianificazione. La figura seguente mostra come la funzionalità di scalabilità automatica gestisce le istanze per gestire il carico.

![Illustrazione che mostra come la scalabilità automatica monitora i livelli di CPU di un pool di macchine virtuali e aggiunge istanze quando l'utilizzo della CPU è superiore alla soglia.](../media/4-autoscale.png)

Per abilitare la scalabilità automatica per un set di scalabilità, è necessario creare un profilo di scalabilità automatica. Il profilo definisce il numero minimo e massimo di istanze di macchina virtuale per il set e le regole di scalabilità. Le regole di scalabilità automatica sono costituite dagli elementi seguenti:

* Origine metrica: l'origine delle informazioni o dei dati che attiva la regola di scalabilità automatica. Sono disponibili quattro opzioni:
  * *Set di scalabilità corrente*: fornisce metriche basate sull'host che non richiedono ulteriori agenti.
  * *Account di archiviazione*: l'estensione di diagnostica di Azure scrive le metriche delle prestazioni in Archiviazione di Azure, che vengono usate per attivare le regole di scalabilità automatica.
  * *Coda del bus di servizio*: può specificare messaggi basati sull'applicazione o altri messaggi del bus di servizio di Azure per l'attivazione della scalabilità automatica.
  * *Application Insights*: usa un pacchetto di strumentazione che deve essere installato nell'applicazione eseguita nel set di scalabilità per lo streaming dei dati di metrica direttamente dall'applicazione.
* Criteri regola: è la metrica specifica che si vuole usare per attivare una regola di scalabilità automatica. Se si usano metriche basate sull'host, questi criteri possono includere aspetti come l'utilizzo della CPU, il volume del traffico di rete, le operazioni su disco o i crediti CPU. Ad esempio, è possibile configurare una regola di aumento se le operazioni di scrittura su disco al secondo superano una determinata soglia. Tramite l'estensione di diagnostica di Azure o Application Insights è possibile usare qualsiasi misura disponibile per attivare la regola, ma è necessario configurare l'agente appropriato.
* Tipo di aggregazione: specifica come si vogliono misurare i dati di metrica e offre le opzioni seguenti:
  * Media
  * Minimo
  * Massimo
  * Totale
  * Ultimo
  * Conteggio
* Operatore: l'operatore indica quanto una metrica deve essere diversa da una soglia definita per attivare l'azione delle regole. È particolarmente importante per stabilire se la regola dovrà aumentare o ridurre le istanze. Gli operatori possono essere:
  * Maggiore di
  * Maggiore o uguale a
  * Minore di
  * Minore o uguale a
  * Uguale a
  * Diverso da
* Azione: determina come cambierà il numero di istanze all'attivazione della regola. Sono disponibili le azioni seguenti:
  * *Aumenta numero di* un numero fisso di macchine virtuali.
  * *Aumenta percentuale di* una percentuale delle istanze esistenti.
  * *Aumenta numero a* un numero specifico di macchine virtuali.
  * *Riduci numero di* un numero fisso di macchine virtuali.
  * *Riduci percentuale di* una percentuale delle istanze esistenti.
  * *Riduci numero di* un numero specifico di macchine virtuali.

È possibile anche creare regole di scalabilità automatica che vengono attivate in base a una pianificazione. Ad esempio, è possibile definire una regola che aumenta il numero di istanze al mattino, quando le richieste sono numerose, e le riduce dopo pranzo, quando il numero di richieste generalmente diminuisce.

## <a name="how-to-create-a-scale-set"></a>Come creare un set di scalabilità

È possibile creare un set di scalabilità usando il portale di Azure, Azure PowerShell o l'interfaccia della riga di comando di Azure.

### <a name="portal"></a>Portale

Se si usa il portale di Azure per creare il set di scalabilità, occorre specificare l'immagine del sistema operativo da usare per le macchine virtuali e il numero di macchine virtuali da creare all'avvio. È necessario specificare anche le dimensioni della macchina virtuale per ogni istanza e se si userà il servizio di bilanciamento del carico o il gateway applicazione per il bilanciamento del carico. Se si sceglie un servizio di bilanciamento del carico, il portale creerà un probe integrità predefinito sulla porta 80 dedicato a tale servizio.

### <a name="powershell"></a>PowerShell

È possibile creare un set di scalabilità di macchine virtuali con il cmdlet di PowerShell **New-AzureRmVmss**. Questo cmdlet può creare un nuovo set di scalabilità e un servizio di bilanciamento del carico e controllare le assegnazioni di indirizzo IP e rete virtuale. Se non diversamente specificato nel cmdlet, **New-AzureRmVmss** usa le impostazioni predefinite seguenti:

* Creare due istanze di macchina virtuale
* Usare l'immagine di Windows Server 2016 Datacenter
* Usare le dimensioni di macchina virtuale standard DS1_v2
* Creare un servizio di bilanciamento del carico
* Creare le regole di bilanciamento del carico per le porte 3389 e 5985 per Windows e per la porta 22 per Linux

**New-AzureRmVmss** non crea un probe integrità per il servizio di bilanciamento del carico. La procedura consigliata indica di crearlo tramite **Add-AzureRmLoadBalancerProbeConfig** dopo aver creato il set di scalabilità.

La scalabilità orizzontale con i set di scalabilità offre più server per l'esecuzione dell'applicazione. L'uso di più server consente di gestire carichi elevati e garantisce che i servizi rimangano disponibili anche in caso di arresto anomalo di un server. È possibile aggiungere la scalabilità automatica ai set di scalabilità in modo che il sistema si adatti automaticamente alle variazioni impreviste dei volumi delle richieste.