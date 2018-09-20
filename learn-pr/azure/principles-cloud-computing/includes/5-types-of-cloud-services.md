Quando si parla di cloud computing, esistono tre categorie principali. È importante conoscerle poiché vengono usate nelle conversazioni, nella documentazione e nel training.

## <a name="explore-the-three-categories-of-cloud-computing"></a>Esplorare le tre categorie di cloud computing

#### <a name="iaas-versus-sass-versus-paas"></a>IaaS, SaaS e PaaS a confronto

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEbs]

:::row:::
  :::column:::
    ![Icona di IaaS](../media/5-iaas.png)
  :::column-end:::
  :::column span="3"::: **Infrastruttura distribuita come servizio (IaaS)**

L'infrastruttura distribuita come servizio è la categoria più flessibile dei servizi cloud. Il suo obiettivo è offrire il controllo completo sull'hardware che esegue l'applicazione. Con il modello IaaS l'hardware non viene acquistato ma noleggiato.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Icona di PaaS](../media/5-paas.png)
  :::column-end:::
  :::column span="3"::: **Piattaforma distribuita come servizio (PaaS)**

Il modello PaaS offre un ambiente per la compilazione, i test e la distribuzione di applicazioni software. L'obiettivo del modello PaaS è aiutare a creare più rapidamente possibile un'applicazione senza doversi preoccupare di gestire l'infrastruttura sottostante. Quando ad esempio si distribuisce un'applicazione Web con il modello PaaS, non è necessario installare un sistema operativo, un server Web e neanche gli aggiornamenti del sistema.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Icona di SaaS](../media/5-saas.png)
  :::column-end:::
  :::column span="3"::: **Software come un servizio (SaaS)**

Un sistema SaaS è costituito da software ospitato e gestito in modo centralizzato per il cliente finale. In genere si basa su un'architettura che prevede l'uso per tutti i clienti di un'unica versione dell'applicazione, concessa in licenza tramite un abbonamento mensile o annuale. Office 365 è un esempio perfetto di software SaaS.
  :::column-end:::
:::row-end:::

## <a name="think-about-service-categories-as-layers"></a>Considerare le categorie di servizi come livelli

Occorre comprendere che queste categorie sono livelli che si sovrappongono l'uno all'altro. Il modello PaaS, ad esempio, aggiunge un livello sopra il modello IaaS, offrendo così un livello di astrazione. L'astrazione offre il vantaggio di nascondere i dettagli di scarso interesse e quindi di raggiungere il codice più rapidamente. Lo svantaggio è che si ha meno controllo sull'hardware sottostante. L'illustrazione seguente illustra un elenco di risorse gestite dall'utente e dal provider di servizi in ogni categoria del servizio cloud.

![Illustrazione del livello di astrazione in ogni categoria del servizio cloud.](../media/5-layer-diagram.png)

## <a name="summary"></a>Riepilogo

I modelli IaaS, PaaS e SaaS contengono diversi livelli di servizi gestiti. È possibile usare facilmente una combinazione di questi tipi di infrastruttura. Si può usare Office 365 nei computer dell'azienda (SaaS), ospitare in Azure le macchine virtuali (IaaS) e usare il database SQL di Azure (PaaS) per archiviare i dati. Con la flessibilità del cloud, è possibile usare qualsiasi combinazione che consente di ottenere il massimo risultato.
