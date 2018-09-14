È possibile ottenere le risorse necessarie usando una macchina virtuale grande o diverse macchine virtuali di piccole dimensioni con un servizio di bilanciamento del carico per distribuire le richieste tra le macchine virtuali.

Il pool di macchine Virtuali offre il vantaggio interessante che è possibile aggiungere o rimuovere rapidamente le macchine virtuali al mutare delle esigenze. Nello scenario aziendale di giocattoli, questa strategia potrebbe essere utile per gestire i picchi imprevisti della domanda. È possibile aggiungere le macchine virtuali nel pool quando richiesta aumentata e rimuoverle quando richiesta torna allo stato normale. Il pool fornisce anche ridondanza; Se una macchina virtuale non riesce, gli altri possibile continuare a gestire le richieste senza interruzione del servizio.

In questa sezione, si verrà illustrato come effettuare il provisioning di più macchine virtuali usando i set di scalabilità e su come aggiungere e rimuovere le istanze in risposta alla richiesta di modifica automaticamente. 

## <a name="what-is-horizontal-scaling"></a>Che cos'è la scalabilità orizzontale?

*Scalabilità orizzontale* è il processo di aggiunta o rimozione di macchine virtuali da un pool per regolare la quantità di risorse disponibili. Aggiunta di computer viene chiamato _la scalabilità orizzontale_, e la rimozione delle macchine viene chiamata _scalabilità in_. Le soluzioni che usano la scalabilità orizzontale includono un load balancer o gateway per distribuire le richieste tra le macchine virtuali nel pool. La figura seguente mostra un esempio di modifica del numero di istanze di macchine virtuali.

![Un'illustrazione che mostra la scalabilità orizzontale di risorse per gestire la domanda e la scalabilità nelle risorse per ridurre i costi.](../media/4-ScaleInOut.png)

Questa tecnica funziona meglio per le applicazioni che possono essere eseguite tra più server identici. Ad esempio, è possibile duplicare i server web e pagine web in più macchine virtuali e lo faranno forniscono tutti la stessa risposta, indipendentemente dal server che riceve la richiesta. D'altra parte, una macchina virtuale che esegue il database back-end non è un candidato ideale perché l'esecuzione di più copie del database richiede qualche sforzo per mantenere sincronizzati le copie.

## <a name="what-is-a-scale-set"></a>Che cos'è un set di scalabilità?

Oggetto *set di scalabilità* è un pool di macchine virtuali identiche, un servizio di bilanciamento del carico o un gateway per distribuire le richieste e un set facoltativo di regole che controlla quando le macchine virtuali vengono aggiunte o rimosse dal pool. In questo caso, "identiche" significa che ogni macchina virtuale nel set viene creato utilizzando la stessa immagine e abbia le stesse dimensioni.

Una certa flessibilità nel modo in cui una nuova macchina virtuale è configurata con il software è necessario. È possibile iniziare con un'immagine predefinita per il sistema operativo di base e quindi usare gli script per installare o copiare i file automaticamente dopo aver configurato il sistema operativo. In alternativa, è possibile creare un'immagine di macchina virtuale personalizzata con il sistema operativo e il software applicativo già installato.

## <a name="how-to-distribute-requests"></a>Come distribuire le richieste

È possibile usare un servizio di bilanciamento del carico o un gateway applicazione per distribuire le richieste a istanze di macchina virtuale in un set di scalabilità.

Un servizio di bilanciamento del carico di Azure opera a livello dello stack OSI 4 (TCP e UDP) e indirizza il traffico in base a indirizzo IP di origine e porta combinata con l'indirizzo IP di destinazione e la porta. Può fornire affinità, l'instradamento del traffico dallo stesso indirizzo IP di origine al server di destinazione stessa per ottenere la coerenza tra una sessione client. Il servizio di bilanciamento del carico ha anche un meccanismo di probe di integrità che determina la disponibilità di istanze del server. Se una macchina virtuale smette di rispondere al probe di integrità, il servizio di bilanciamento del carico eviterà di routing di tutte le nuove connessioni a tale macchina.

Un gateway applicazione opera a livello dello stack OSI 7 (il livello di applicazione). Ad esempio, se le macchine virtuali eseguono un server web, quindi il gateway può usare URL richiesto per eseguire il routing. Ciò significa che è possibile inoltrare le richieste con `*/customers*` nell'URL per un pool di server e le richieste con `*/partners*` nell'URL per un pool diverso. Il gateway applicazione può anche fornire HTTP al reindirizzamento HTTPS, la terminazione Secure Sockets Layer (SSL) per ridurre l'esigenza di elaborazione in macchine virtuali per la crittografia e un web application firewall (WAF) che utilizza regole per rilevare web noto sfrutta ed evitare queste richieste di raggiungere i server web.

## <a name="what-is-autoscaling"></a>Che cos'è la scalabilità automatica?

_La scalabilità automatica_ è il processo di scalabilità automatica orizzontale o in basato su un set di regole. Le regole possono essere attivate dal carico del computer o a una pianificazione. La figura seguente mostra come la funzionalità scalabilità automatica gestisce le istanze per gestire il carico.

![Un'illustrazione che mostra come la scalabilità automatica consente di monitorare i livelli di CPU di un pool di macchine virtuali e aggiunge istanze quando l'utilizzo della CPU è superiore alla soglia.](../media/4-autoscale.png)

Per abilitare la scalabilità automatica per un set di scalabilità, è necessario creare un profilo di scalabilità automatica. Il profilo definisce il numero minimo e massimo di istanze di macchina virtuale per il set e le regole di ridimensionamento. Regole di scalabilità automatica sono i seguenti elementi:

* Origine della metrica: l'origine di informazioni o i dati che attiva la regola di scalabilità automatica. Vi sono quattro opzioni:
  * *Set di scalabilità corrente* fornisce le metriche basate su host che non necessitano di tutti gli altri agenti.
  * *Account di archiviazione*. L'estensione diagnostica di Azure scrive le metriche delle prestazioni per archiviazione di Azure. Queste metriche vengono usate per attivare le regole di scalabilità automatica.
  * *Coda del Bus di servizio di Azure* specificabile basato sull'applicazione o altri messaggi del Bus di servizio di Azure per attivare la scalabilità automatica.
  * *Azure Application Insights* Usa un pacchetto di strumentazione che deve essere installato nell'applicazione in esecuzione nel set di scalabilità per i dati delle metriche di flusso direttamente dall'applicazione.
* Criteri delle regole - questa è la metrica specifica da usare per attivare una regola di scalabilità automatica. Se si usa metriche basate su host, può trattarsi di aspetti come utilizzo della CPU, il volume di traffico di rete, le operazioni del disco o i crediti di CPU. Ad esempio, è possibile configurare una regola di scalabilità orizzontale se operazioni di scrittura su disco al secondo superano una soglia. Usando l'estensione diagnostica di Azure o Application Insights consente di usare le misure disponibili per attivare la regola, ma richiede la configurazione dell'agente appropriato.
* Tipo di aggregazione - questo consente di specificare come si desidera misurare i dati delle metriche e sarà una delle opzioni seguenti:
  * Media
  * Minima
  * Massima
  * Totale
  * Last (Ultimo)
  * Conteggio
* Operatore - indica come una metrica deve essere diversa rispetto a una soglia definita per attivare l'azione di regole. Ciò è particolarmente importante quando si identificano se la regola verrà aumentare o ridurre. Gli operatori possono essere:
  * Maggiore di
  * Maggiore o uguale a
  * Minore di
  * Minore o uguale a
  * Uguale a
  * Diverso da
* Azione - determina come verrà modificato il numero di istanze quando viene attivata la regola. Sono disponibili le seguenti azioni:
  * *Aumenta numero di* un numero fisso di macchine virtuali.
  * *Aumenta percentuale di* percentuale delle istanze esistenti.
  * *Aumenta numero a* un numero specifico di macchine virtuali.
  * *Riduci numero di* un numero fisso di macchine virtuali.
  * *Riduci percentuale di* percentuale delle istanze esistenti.
  * *Riduci numero a* un numero specifico di macchine virtuali.

È anche possibile creare regole di scalabilità automatica che attivano in una pianificazione. Ad esempio, si potrebbe definire una regola che viene scalata orizzontalmente al mattino quando si conosce la domanda è elevata e quindi ridimensiona nel dopo pranzo quando si riduce in genere richiesta.

## <a name="how-to-create-a-scale-set"></a>Come creare un set di scalabilità

È possibile creare un set di scalabilità tramite il portale di Azure, Azure PowerShell o l'interfaccia CLI di Azure.

### <a name="portal"></a>Portale

Se si usa il portale di Azure per creare il set di scalabilità, si specificherà l'immagine del sistema operativo da usare per le macchine virtuali e il numero di istanze di macchina virtuale per creare all'avvio. Inoltre, si specificherà le dimensioni della macchina virtuale per ogni istanza e la possibilità di usare Azure load balancer o gateway applicazione per il bilanciamento del carico. Se si sceglie un bilanciamento del carico, il portale creerà un probe di integrità predefinito sulla porta 80 per tale.

### <a name="powershell"></a>PowerShell

È possibile creare un macchina virtuale set di scalabilità con il **New-AzureRmVmss** cmdlet di PowerShell. Questo cmdlet è possibile creare un nuovo set di scalabilità, bilanciamento del carico e controllare l'indirizzo IP e le assegnazioni di rete virtuale. A meno che non sono specificate nel cmdlet, **New-AzureRmVmss** utilizzerà le impostazioni predefinite seguenti:

* Creare due istanze di macchina virtuale
* Usare l'immagine di Windows Server 2016 Datacenter
* Usare dimensioni della macchina virtuale DS1_v2 Standard
* Creare un servizio di bilanciamento del carico
* Creare regole di bilanciamento del carico per le porte 3389 e 5985 per Windows, la porta 22 per Linux

**New-AzureRmVmss** non crea un probe di integrità per il bilanciamento del carico. Le procedure consigliate, è possibile creare questo usando **Add-AzureRmLoadBalancerProbeConfig** dopo aver creato il set di scalabilità.

Scalabilità orizzontale con set di scalabilità offre più server per eseguire l'applicazione. L'utilizzo di più server consente che maneggiare alto carica e garantisce che i servizi rimangono disponibili anche se si blocca un server. È possibile aggiungere la scalabilità automatica per i set di scalabilità, in modo che il sistema si adatta automaticamente alle modifiche impreviste della domanda.