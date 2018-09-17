Quando si parla di cloud computing, esistono tre categorie principali. È importante conoscerle poiché vengono usate nelle conversazioni, nella documentazione e nel training.

## <a name="explore-the-three-categories-of-cloud-computing"></a>Esplorare le tre categorie di cloud computing

<!-- TODO: Verify video -->
> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEbs]

### <a name="infrastructure-as-a-service-iaas"></a>Infrastruttura distribuita come servizio (IaaS, Infrastructure as a Service)

L'infrastruttura distribuita come servizio è la categoria più flessibile dei servizi cloud. Il suo obiettivo è offrire il controllo completo sull'hardware che esegue l'applicazione. Con il modello IaaS l'hardware non viene acquistato ma noleggiato.

### <a name="platform-as-a-service-paas"></a>Piattaforma distribuita come servizio (PaaS, Platform as a Service)

Il modello PaaS offre un ambiente per la compilazione, i test e la distribuzione di applicazioni software. L'obiettivo del modello PaaS è aiutare a creare più rapidamente possibile un'applicazione senza doversi preoccupare di gestire l'infrastruttura sottostante. Quando ad esempio si distribuisce un'applicazione Web con il modello PaaS, non è necessario installare un sistema operativo, un server Web e neanche gli aggiornamenti del sistema.

### <a name="software-as-a-service-saas"></a>Software come un servizio (SaaS, Software as a Service)

Un sistema SaaS (Software as a Service, software come un servizio) è costituito da software ospitato e gestito in modo centralizzato per il cliente finale. In genere si basa su un'architettura che prevede l'uso per tutti i clienti di un'unica versione dell'applicazione, concessa in licenza tramite un abbonamento mensile o annuale. Office 365 è un esempio perfetto di software SaaS.

## <a name="think-about-service-categories-as-layers"></a>Considerare le categorie di servizi come livelli

Occorre comprendere che queste categorie sono livelli che si sovrappongono l'uno all'altro. Il modello PaaS, ad esempio, aggiunge un livello sopra il modello IaaS, offrendo così un livello di astrazione. L'astrazione offre il vantaggio di nascondere i dettagli di scarso interesse e quindi di raggiungere il codice più rapidamente. Lo svantaggio è che si ha meno controllo sull'hardware sottostante. L'illustrazione seguente illustra un elenco di risorse gestite dall'utente e dal provider di servizi in ogni categoria del servizio cloud.

![Illustrazione del livello di astrazione in ogni categoria del servizio cloud.](../media/5-layer-diagram.png)

## <a name="summary"></a>Riepilogo

I modelli IaaS, PaaS e SaaS contengono diversi livelli di servizi gestiti. È possibile usare facilmente una combinazione di questi tipi di infrastruttura. Si può usare Office 365 nei computer dell'azienda (SaaS), ospitare in Azure le macchine virtuali (IaaS) e usare il database SQL di Azure (PaaS) per archiviare i dati. Con la flessibilità del cloud, è possibile usare qualsiasi combinazione che consente di ottenere il massimo risultato.
