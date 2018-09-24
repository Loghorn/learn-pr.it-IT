È stato descritto come rendere l'applicazione disponibile e ripristinabile progettando l'architettura per disponibilità elevata, ripristino di emergenza e backup. Di seguito vengono presentati alcuni dei vantaggi principali.

## <a name="high-availability"></a>Disponibilità elevata

- La disponibilità elevata riguarda la capacità di gestire la perdita o una grave riduzione delle prestazioni di un componente di un sistema.
- Per valutare l'applicazione in termini di disponibilità elevata, è necessario concentrarsi su tre aree principali:
  - Il contratto di servizio definito.
  - Le funzionalità per la disponibilità elevata dell'applicazione.
  - Le funzionalità per la disponibilità elevata delle applicazioni dipendenti.
- Usare set di disponibilità e zone di disponibilità in Azure per fornire disponibilità elevata per i carichi di lavoro delle macchine virtuali.
- Usare servizi di bilanciamento del carico come Gestione traffico di Azure, il gateway applicazione di Azure e Azure Load Balancer per distribuire il carico tra i sistemi disponibili.
- I servizi PaaS hanno disponibilità elevata integrata e di conseguenza è consigliabile sfruttare questi servizi nell'architettura ogni volta che è possibile.

## <a name="disaster-recovery"></a>Ripristino di emergenza

- Il ripristino di emergenza riguarda il *ripristino da eventi a impatto elevato* che comportano tempo di inattività e perdita di dati.
- Creare un piano di ripristino di emergenza per definire le procedure, le responsabilità e le attività necessarie per il ripristino di emergenza.
- Definire gli obiettivi RPO e RTO per l'applicazione per determinarne i requisiti di ripristino di emergenza.
- Usare il backup e la replica per fornire copie dei sistemi e dei dati da cui eseguire il ripristino.
- Usare Azure Site Recovery per le funzionalità di ripristino dei processi di ripristino di emergenza per l'applicazione.
- Testare il piano di ripristino di emergenza per identificare le lacune e la rilevanza dei passaggi.

## <a name="backup-and-restore"></a>Backup e ripristino

- Usare i backup per ripristinare i dati come parte della strategia di ripristino di emergenza o per scenari di perdita di dati più isolati.
- Usare Backup di Azure per il backup completo delle macchine virtuali, il backup di file e cartelle e il backup dei sistemi in un ambiente locale.
- Le funzionalità di backup sono spesso integrate nei servizi PaaS, come il database SQL di Azure e Servizio app di Azure. Sfruttare i vantaggi di queste funzionalità di backup quando possibile, acquisendo familiarità con la configurazione predefinita e le caratteristiche complessive.
- Testare regolarmente i processi e le procedure di ripristino per assicurarsi che siano validi e sufficienti.
