Si è deciso di spostare i database di SQL Server nel database SQL di Azure per sfruttare la scalabilità e la disponibilità di Azure. Si vuole però verificare che il server SQL Azure fornisca tutte le funzionalità che attualmente si usano nel server SQL locale.

Si userà uno strumento denominato **Data Migration Assistant** per valutare se il database esistente è compatibile con il server SQL Azure SQL.

## <a name="what-is-the-data-migration-assistant-dma"></a>Che cos'è Data Migration Assistant (DMA)?

Lo strumento Data Migration Assistant è stato appositamente progettato per analizzare le istanze di SQL Server locali. Rileva e segnala i problemi più comuni che impediscono la migrazione dei database SQL al server SQL Azure.

Rileva i problemi di compatibilità che bloccano la migrazione e riconosce le funzionalità usate nel server locale che sono supportate parzialmente o non supportate affatto.

Offre anche indicazioni esaustive sulle operazioni da eseguire sul server locale prima della migrazione.

### <a name="why-do-you-need-the-data-migration-assistant"></a>Perché Data Migration Assistant è necessario?

Il database SQL di Azure ha una base di codice in comune con SQL Server. Tuttavia, non tutte le funzionalità sono presenti nel cloud. Data Migration Assistant può contribuire a identificare le funzionalità utilizzate in SQL Server in locale che non sono disponibili nel Server SQL di Azure. Per un elenco aggiornato delle differenze tra i due prodotti, vedere l'[elenco delle funzionalità del database SQL di Azure](https://docs.microsoft.com/azure/sql-database/sql-database-features).

## <a name="how-to-assess-your-database-using-the-data-migration-assistant"></a>Come si valuta il database con Data Migration Assistant?

La valutazione del database con Data Migration Assistant consiste generalmente nei passaggi seguenti:

1. **Installare Data Migration Assistant**: scaricare e installare Data Migration Assistant da https://www.microsoft.com/download/details.aspx?id=53595. Il database SQL di Azure viene aggiornato regolarmente, così come Data Migration Assistant per includere eventuali nuove funzionalità. È consigliabile eseguire il programma di installazione per assicurarsi di avere la versione più recente.
2. **Creare una valutazione**: occorre creare una nuova valutazione che definisca il database locale e quello di destinazione. È anche possibile valutare un'altra destinazione di SQL Server in Macchine virtuali di Azure per poter confrontare migrazioni alternative.
3. **Selezionare le opzioni per la valutazione e le origini del database**: selezionare le opzioni, ad esempio se si vuole verificare la compatibilità o la parità delle funzionalità tra i due database. Quindi, selezionare il database di origine. È possibile selezionare più origini.
4. **Esaminare i risultati**: i risultati dettagliati consentono di esaminare gli errori ed eseguire un'azione correttiva. I risultati elencano le funzionalità non supportate con riferimenti tra database e SQL Server Agent. Elencano anche le funzionalità supportate parzialmente, con ricerca full-text e controllo. I risultati riportano anche i possibili errori ed eventuali consigli su come correggerli. È possibile esportare i risultati di Data Migration Assistant come file JSON.
