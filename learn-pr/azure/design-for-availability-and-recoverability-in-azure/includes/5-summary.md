È stato illustrato come effettuare rendere l'applicazione disponibili e recuperabili dalla definizione dell'architettura per la disponibilità elevata, ripristino di emergenza e backup. Diamo un'occhiata ad alcune delle considerazioni principali.

## <a name="high-availability"></a>Disponibilità elevata

- La disponibilità elevata riguarda la capacità di gestire la perdita o una grave riduzione delle prestazioni di un componente di un sistema.
- Valutare l'applicazione per la disponibilità elevata focalizzando l'attenzione su tre aree principali:
  - Il contratto di servizio definito.
  - Le funzionalità a disponibilità elevata dell'applicazione.
  - Le funzionalità a disponibilità elevata delle applicazioni dipendenti.
- Usare set di disponibilità e le zone di disponibilità in Azure per fornire disponibilità elevata per i carichi di lavoro di macchina virtuale.
- Usare il bilanciamento del carico di servizi, ad esempio Gestione traffico di Azure, Gateway applicazione di Azure e Azure Load Balancer per distribuire il carico tra i sistemi disponibili.
- I servizi PaaS sono a disponibilità elevata incorporata, pertanto usare questi servizi nell'architettura laddove possibile.

## <a name="disaster-recovery"></a>Ripristino di emergenza

- Il ripristino di emergenza riguarda il *ripristino da eventi a impatto elevato* che comportano tempi di inattività e perdite di dati.
- Creare un piano di ripristino di emergenza per definire le procedure, le responsabilità e le attività necessarie per il ripristino di emergenza.
- Definire gli obiettivi RPO e RTO per l'applicazione determinare i requisiti di ripristino di emergenza per l'applicazione.
- Usare backup e la replica per fornire copie dei sistemi e dati da ripristinare.
- Usare Azure Site Recovery per la funzionalità di ripristino di processo di ripristino di emergenza per l'applicazione.
- Testare il piano di ripristino di emergenza per identificare i gap e la rilevanza dei passaggi.

## <a name="backup-and-restore"></a>Backup e ripristino

- Usare i backup per ripristinare i dati come parte della strategia di ripristino di emergenza o per scenari di perdita di dati più isolati.
- Usare Backup di Azure per completo VM backup file e backup di cartelle e backup dei sistemi in un ambiente locale.
- Funzionalità di backup sono spesso integrate nei servizi PaaS, ad esempio Database SQL di Azure e servizio App di Azure. Sfruttare i vantaggi di queste funzionalità di backup quando possibile, conoscere le loro configurazione predefinita e le caratteristiche complessive.
- Testare i processi di ripristino e procedure regolarmente per assicurarsi che siano validi e sufficienti.
