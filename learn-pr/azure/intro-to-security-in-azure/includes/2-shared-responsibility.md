Ora che gli ambienti informatici passano da data center controllati dal cliente a data center nel cloud, anche la responsabilità della sicurezza cambia. La sicurezza ora è un compito condiviso tra i provider di servizi cloud e i clienti. Per ogni applicazione e soluzione, è importante comprendere quali sono le responsabilità del cliente e quelle di Azure.

#### <a name="understand-security-threats"></a>Informazioni sulla minacce alla sicurezza

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWkotg]

#### <a name="azure-security-you-versus-the-cloud"></a>Sicurezza di Azure: il cliente e il cloud

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvj]

## <a name="share-security-responsibility-with-azure"></a>Condividere la responsabilità della sicurezza con Azure

Il primo passaggio da realizzare è quello dai data center locali agli ambienti IaaS (Infrastructure-as-a-Service). In un ambiente IaaS il cliente usa il servizio di livello più basso e chiede ad Azure di creare macchine virtuali e reti virtuali. A questo livello, è comunque responsabilità dell'utente applicare patch e proteggere i sistemi operativi e il software, nonché configurare la rete in modo sicuro. In Contoso Shipping si sfruttano i vantaggi dei servizi IaaS quando si iniziano a usare macchine virtuali di Azure anziché i propri server fisici locali. Oltre ai vantaggi operativi, si gode del vantaggio in termini di sicurezza di aver esternalizzato il compito di proteggere le parti fisiche della rete.

Il passaggio a una soluzione PaaS (Platform-as-a-Service) esternalizza molti problemi di sicurezza. A questo livello, Azure si occupa del sistema operativo e della maggior parte dei software di base, come i sistemi di gestione dei database. Tutto è aggiornato con le ultime patch di sicurezza e può essere integrato con Azure Active Directory per il controllo degli accessi. Gli ambienti PaaS presentano anche molti vantaggi operativi. Invece di costruire intere infrastrutture e subnet per i propri ambienti manualmente, è possibile effettuare selezioni all'interno del portale di Azure o eseguire script automatici per attivare e disattivare complessi sistemi protetti, nonché ridimensionarli a seconda delle necessità. Contoso Shipping usa Hub eventi di Azure per inserire i dati di telemetria trasmessi da droni e veicoli, oltre a un'app Web con un back-end Azure Cosmos DB con le app per dispositivi mobili. Questi sono tutti esempi di PaaS.

Con una soluzione SaaS (Software-as-a-Service), si esternalizza quasi tutto. SaaS è il software che viene eseguito con un'infrastruttura Internet. Il codice viene controllato dal fornitore, ma è configurato per poter essere usato dal cliente. Come molte aziende, Contoso Shipping usa Office 365, che è un perfetto esempio di SaaS.

![shared_responsibility.png](../media/shared_responsibilities.png)

## <a name="a-layered-approach-to-security"></a>Approccio alla sicurezza strutturato su più livelli

*Difesa avanzata* è una strategia che impiega una serie di meccanismi per rallentare l'avanzamento di un attacco volto ad accedere a informazioni senza autorizzazione. Ogni livello offre funzioni di protezione, quindi, se un livello viene violato, vi è già un livello successivo pronto a impedire un'ulteriore esposizione. Microsoft applica un approccio alla sicurezza strutturato su più livelli, sia nei data center fisici che nei servizi di Azure. Lo scopo della difesa avanzata è proteggere le informazioni e prevenirne il furto da parte di soggetti non autorizzati.

La difesa avanzata può essere visualizzata come una serie di anelli concentrici, al cui centro si trovano i dati da proteggere. Ogni anello aggiunge un livello di protezione attorno ai dati. Questa strategia consente di evitare la dipendenza da un singolo livello di protezione e opera rallentando l'attacco e generando avvisi con dati di telemetria, in base ai quali è possibile intervenire automaticamente o manualmente. Esaminiamo ciascuno di questi livelli.

![Difesa avanzata](../media/defense_in_depth_layers_small.PNG)

:::row:::
  :::column:::
    ![Immagine che rappresenta i dati](../media/2-data.png)
  :::column-end:::
    :::column span="3"::: **Dati**

Nella quasi totalità dei casi, gli utenti malintenzionati cercano:

- Dati archiviati in un database
- Dati archiviati su disco all'interno di macchine virtuali
- Dati archiviati in applicazioni SaaS, come Office 365
- Dati archiviati nel cloud

È responsabilità di chi archivia i dati e di chi ne controlla l'accesso assicurarsi che siano adeguatamente protetti. Esistono spesso requisiti normativi che determinano i controlli e i processi da predisporre per garantire la riservatezza, l'integrità e la disponibilità dei dati.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Immagine di un file in rete](../media/2-application.png)
  :::column-end:::
    :::column span="3"::: **Applicazione**

- Verificare che le applicazioni siano protette e prive di vulnerabilità.
- Archiviare i segreti sensibili delle applicazioni in un supporto di archiviazione sicuro.
- Prevedere la sicurezza come requisito di progettazione per lo sviluppo di tutte le applicazioni.

L'integrazione della sicurezza nel ciclo di sviluppo delle applicazioni consentirà di ridurre il numero di vulnerabilità introdotte nel codice. Tutti i team di sviluppo sono invitati a garantire la sicurezza predefinita delle proprie applicazioni e rendere i requisiti di sicurezza imprescindibili.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Terminale che rappresenta il calcolo](../media/2-compute.png)
  :::column-end:::
    :::column span="3"::: **Calcolo**

- Proteggere l'accesso alle macchine virtuali.
- Implementare la protezione degli endpoint e mantenere aggiornati i sistemi con le patch di sicurezza.

Malware, sistemi privi di patch e sistemi non protetti in modo corretto espongono l'ambiente agli attacchi. Lo scopo a questo livello è assicurarsi che le risorse di calcolo siano sicure e che siano stati predisposti controlli appropriati in grado di ridurre al minimo i problemi di sicurezza.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Tre sistemi connessi che rappresentano la rete](../media/2-networking.png)
  :::column-end:::
    :::column span="3"::: **Rete**

- Limitare la comunicazione tra risorse.
- Negare per impostazione predefinita.
- Limitare l'accesso Internet in ingresso e in uscita, dove appropriato.
- Implementare una connettività sicura verso le reti locali.

A questo livello, lo scopo è limitare la connettività di rete di tutte le risorse, consentendo unicamente quella necessaria. Limitando la comunicazione, si riduce il rischio di spostamento laterale attraverso la rete.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Barriera fisica che rappresenta il perimetro di rete](../media/2-perimeter.png)
  :::column-end:::
    :::column span="3"::: **Perimetro**

- Implementare Protezione DDoS (attacco Distributed Denial of Service) per filtrare gli attacchi su larga scala prima che possano bloccare il servizio per gli utenti finali.
- Usare firewall perimetrali per identificare e segnalare gli attacchi dannosi alla rete.

A livello di perimetro della rete, lo scopo è impedire gli attacchi di rete diretti alle risorse. Identificare gli attacchi, neutralizzarne l'impatto e segnalarli quando si verificano sono modi importanti per proteggere la rete.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Badge che rappresenta un accesso sicuro](../media/2-policies-and-access.png)
  :::column-end:::
    :::column span="3"::: **Criteri e accesso**

- Controllare l'accesso all'infrastruttura e implementare il controllo delle modifiche.
- Usare l'autenticazione a più fattori e Single Sign-On.
- Controllare eventi e modifiche.

Al livello criteri e accesso lo scopo è verificare che le identità siano protette, che l'accesso venga concesso solo se necessario e che le modifiche vengano registrate.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Telecamera di sicurezza che rappresenta la sicurezza fisica](../media/2-physical-security.png)
  :::column-end:::
    :::column span="3"::: **Sicurezza fisica**

- La sicurezza fisica dell'edificio e il controllo degli accessi all'hardware nel data center è la prima linea di difesa.

Al livello della sicurezza fisica, la finalità è predisporre protezioni fisiche per l'accesso alle risorse. In questo modo si impedisce l'aggiramento degli altri livelli e si assicura una gestione appropriata di perdite e furti.
  :::column-end:::
:::row-end:::

## <a name="summary"></a>Riepilogo

Abbiamo visto come Azure aiuta molto a risolvere i problemi di sicurezza. La sicurezza resta comunque una **responsabilità condivisa**. Quanta di questa responsabilità ricade sull'utente dipende dal modello adottato con Azure.

Gli anelli della *difesa avanzata* possono essere usati come linea guida per determinare quali protezioni sono adeguate ai propri dati e ambienti.