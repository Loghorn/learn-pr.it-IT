Questo modulo ha riguardato l'integrazione di dati e servizi nelle funzioni. Per prima cosa è stata presentata una breve panoramica dei tipi di associazioni disponibili per l'aggiunta a una funzione. È stata quindi esaminata la lettura dei dati da Azure Cosmos DB tramite un'associazione di input. La piattaforma si occupa della gestione delle stringhe di connessione e si è visto come sia semplice leggere i dati nel codice tramite l'associazione. Infine, ci si è concentrati sulla scrittura di dati in origini diverse con l'aiuto delle associazioni di output. Il percorso svolto è riepilogato nella tabella seguente:

[!INCLUDE [summary table](./summary-table.md)]

È possibile applicare gli approcci appresi finora per aggiungere e testare le associazioni all'interno delle funzioni. Di seguito sono presentati alcuni spunti interessanti per acquisire maggiore familiarità con le associazioni e procedere alla compilazione in base a quanto si è appreso.

* Creare un'altra funzione per leggere dall'archiviazione BLOB e altre associazioni di input che non sono state usate in questo modulo.

* Creare un'altra funzione per scrivere in più destinazioni tramite altri tipi di associazioni di output supportati.

* Nell'unità precedente è stata introdotta una coda in cui sono stati pubblicati messaggi tramite un'associazione di output. Come passaggio successivo, prendere in considerazione l'aggiunta di un'altra funzione che legge i messaggi nella coda e visualizza il **TESTO DEL MESSAGGIO** nella console con `Console.Log()`.

Come si è visto in questo modulo, il portale di Azure offre funzionalità facili da usare per iniziare a creare funzioni e connetterle a dati e altri servizi.

Se si è interessati alla realizzazione di integrazioni serverless come queste, con i flussi di lavoro visivi e senza codice personalizzato, o con una quantità minima di codice, provare anche App per la logica di Azure.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>Risorse aggiuntive

Anche se non si tratta di un elenco esaustivo, di seguito sono riportate alcune risorse correlate agli argomenti trattati in questo modulo che potrebbero risultare interessanti:

* [Documentazione di Funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/)
* [Azure Functions Challenge](https://aka.ms/afc) (Sfida di Funzioni di Azure)
* [Azure Serverless Computing Cookbook](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/) (Guida di riferimento dettagliata all'elaborazione serverless in Azure)
* [Come usare l'archiviazione di accodamento da Node.js](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)
* [Introduzione ad Azure Cosmos DB: API SQL](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)
* [Panoramica tecnica di Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)
* [Documentazione di Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
