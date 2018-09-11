Si è deciso di spostare i database di SQL Server nel database SQL di Azure per sfruttare la scalabilità e la disponibilità di Azure. Si vuole però verificare che il server SQL Azure fornisca tutte le funzionalità che attualmente si usano nel server SQL locale.

Si userà uno strumento denominato Data Migration Assistant (DMA) per valutare se il database esistente è compatibile con il server SQL Azure.

## <a name="what-is-the-data-migration-assistant-dma"></a>Che cos'è Data Migration Assistant (DMA)?

Lo strumento Data Migration Assistant (DMA) è stato appositamente progettato per analizzare le istanze di SQL Server locali. Rileva e segnala i problemi più comuni che impediscono la migrazione dei database SQL al server SQL Azure.

Rileva i problemi di compatibilità che bloccano la migrazione e riconosce le funzionalità usate nel server locale che sono supportate parzialmente o non supportate affatto.

Offre anche indicazioni esaustive sulle operazioni da eseguire sul server locale prima della migrazione.

### <a name="why-do-you-need-data-migration-assistant"></a>Perché Data Migration Assistant è necessario?

Il database SQL di Azure è costantemente in fase di sviluppo e attualmente non supporta tutte le funzionalità di SQL Server.

Per un elenco aggiornato, vedere l'[elenco delle funzionalità del database SQL di Azure](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-features).

## <a name="how-to-assess-your-database-using-data-migration-assistant"></a>Come si valuta il database con Data Migration Assistant?

La valutazione del database con Data Migration Assistant consiste generalmente nei passaggi seguenti:

- **Installare Data Migration Assistant**: scaricare e installare DMA da https://www.microsoft.com/en-us/download/details.aspx?id=53595. Il database SQL di Azure viene aggiornato regolarmente, così come DMA per comprendere eventuali nuove funzionalità. È consigliabile eseguire il programma di installazione per assicurarsi di avere la versione più recente.
- **Creare una valutazione**: occorre creare una nuova valutazione che definisca il database locale e quello di destinazione. È anche possibile valutare un'altra destinazione di SQL Server su Macchine virtuali di Microsoft Azure per poter confrontare migrazioni alternative.
- **Selezionare le opzioni per la valutazione e le origini database**: selezionare le opzioni desiderate, ad esempio se si vuole verificare la compatibilità o la parità delle funzionalità fra i due database. Quindi selezionare il database di origine. È possibile selezionare più origini.
- **Esaminare i risultati**: i risultati dettagliati consentono di esaminare gli errori ed eseguire un'azione correttiva. I risultati elencano le funzionalità non supportate con riferimenti tra database e SQL Server Agent. Elencano anche le funzionalità supportate parzialmente, con ricerca full-text e controllo. I risultati suggeriscono i possibili errori e offrono suggerimenti su come correggerli. È possibile esportare i risultati DMA come file JSON.