Questo modulo è stato tutto sull'integrazione dei dati e i servizi nelle funzioni. Abbiamo iniziato con una breve panoramica dei tipi di associazione che vengono visualizzati quando li si aggiunge a una funzione. Abbiamo quindi esaminato la lettura dei dati da Azure Cosmos DB usando un'associazione di input. La piattaforma si occupa di gestire le stringhe di connessione ed è stato illustrato come è facile leggere i dati nel codice utilizzando l'associazione. Infine abbiamo concentrato la nostra attenzione sulla scrittura di origini diverse dei dati con l'aiuto di associazioni di output. Questo percorso è riepilogato nella tabella seguente.

[!INCLUDE [summary table](./summary-table.md)]

È possibile applicare gli approcci che si è appreso qui per aggiungere e testare le associazioni in funzioni. Di seguito sono alcune interessanti idee per ottenere più pratica con le associazioni e per compilare in quanto si è appreso.

* Creare un'altra funzione per la lettura dall'archiviazione Blob e le altre associazioni di input che è ancora stato utilizzato in questo modulo.

* Creare un'altra funzione per scrivere in più destinazioni tramite altri tipi di associazione di output supportati.

* Dell'ultima unità, abbiamo introdotto una coda e inviato messaggi ad esso con un'associazione di output. Come passaggio successivo, è consigliabile aggiungere un'altra funzione che legge i messaggi nella coda e vengono stampati i **testo del messaggio** nella console con `Console.Log()`.

Come abbiamo visto in questo modulo, il portale di Azure offre funzionalità facili da usare per avviare la creazione di funzioni e che li connettono ai dati e altri servizi.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="further-reading"></a>Altre informazioni

Anche se ciò non è un elenco completo, ecco alcune risorse correlate per gli argomenti trattati in questo modulo che potrebbero risultare interessanti.

 * [Documentazione di funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/)

* [La sfida di funzioni di Azure](https://aka.ms/afc)

* [Azure senza server Computing Cookbook](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)

 * [Come usare l'archiviazione di accodamento da Node.js](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)

 * [Introduzione ad Azure Cosmos DB: API SQL](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)

* [Panoramica tecnica di Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)

* [Documentazione di Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
