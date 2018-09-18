Servizi cognitivi Microsoft è una suite completa di servizi intelligenti utilizzabili per arricchire le app. È stata presa in esame una piccola parte del servizio API Analisi del testo per ottenere informazioni più generali sul testo. Il servizio è stato usato per analizzare il sentiment del feedback di testo dei clienti. È stata creata una soluzione ospitata in Funzioni di Azure per ordinare questi messaggi di testo in diversi bucket, o code, per un'ulteriore elaborazione.

Ora che si hanno le competenze per chiamare un'API REST, è possibile integrare facilmente questi servizi intelligenti nelle soluzioni in uso. Seguono tutti un modello simile:

- Iscriversi per una chiave di accesso
- Esplorare la console di test dell'API
- Formulare richieste usando la chiave di accesso e l'area corretta.
- Inviare richieste POST dalla soluzione e analizzare le risposte per informazioni dettagliate.

Questa funzionalità di intelligence è stata aggiunta alla logica serverless creata in Funzioni di Azure. È possibile chiamare facilmente questi servizi da altri tipi di app. Sono disponibili numerose librerie client, esercitazioni e guide introduttive per iniziare.

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>Suggerimenti per un ulteriore miglioramento della soluzione

Di seguito sono riportati alcuni spunti da prendere in considerazione per proseguire. 

- Testare la soluzione con altri esempi di testo e decidere se le soglie impostate per classificare i punteggi del sentiment in positivi, negativi e neutrali sono appropriate. 
- È consigliabile aggiungere un'altra funzione nell'app per le funzioni per leggere i messaggi dalla coda [!INCLUDE [negative-q](./q-name-negative.md)] e chiamare l'API Analisi del testo per trovare frasi chiave nel testo.
- La coda di input contiene feedback di testo non elaborato. In uno scenario reale è necessario associare i commenti a uno specifico ID utente come indirizzo di posta elettronica, numero di account e così via. Pertanto, ottimizzare gli elementi della coda di input trasformandoli in documenti JSON contenenti un campo ID e del testo. Usare quindi tale ID quando si lavora sul messaggio di testo.
 - Attualmente la soluzione è "hardcoded" per l'inglese. Valutare le modifiche da apportare per fare in modo che sia in grado di gestire il testo in tutte le lingue supportate dall'API Analisi del testo.  

Ora che si hanno le competenze per chiamare una di queste API Servizi cognitivi, è il momento di esaminare alcuni degli altri servizi e pensare a come usarli nelle soluzioni adottate. 

## <a name="further-reading"></a>Altre informazioni

- [Panoramica di Analisi del testo](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [Come rilevare il sentiment in Analisi del testo](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Documentazione dei servizi cognitivi](https://docs.microsoft.com/azure/cognitive-services/)

## <a name="clean-up-resources"></a>Pulire le risorse

Il termine *risorse* in Azure si riferisce ad app per le funzioni, funzioni, account di archiviazione e così via. Le risorse sono raggruppate in *gruppi di risorse* ed è possibile eliminare tutti gli elementi in un gruppo eliminando il gruppo.

Per completare questo modulo sono state create risorse. Per tali risorse potrebbero venire addebitati costi, a seconda dello [stato dell'account](https://azure.microsoft.com/account/) e dei [prezzi dei servizi](https://azure.microsoft.com/pricing/). Se le risorse non sono più necessarie, ecco come eliminarle:

1. Nel portale di Azure passare alla pagina **Gruppo di risorse**.

   Per visualizzare tale pagina dalla pagina dell'app per le funzioni, selezionare la scheda **Panoramica** e quindi selezionare il collegamento sotto **Gruppo di risorse**.

   Per visualizzare tale pagina dal dashboard, selezionare **Gruppi di risorse** e quindi selezionare il gruppo di risorse usato per questo modulo. 

> [!NOTE]
> Il nome predefinito del gruppo di risorse suggerito per questo modulo è [!INCLUDE [resource-group-name](./rg-name.md)], ma è possibile che sia stato usato un altro nome.

2. Nella pagina **Gruppo di risorse** esaminare l'elenco delle risorse incluse e verificare che siano quelle da eliminare.

3. Selezionare **Elimina gruppo di risorse** e seguire le istruzioni.

   L'eliminazione potrebbe richiedere alcuni minuti. Al termine, viene visualizzata una notifica per pochi secondi. È anche possibile selezionare l'icona a forma di campana nella parte superiore della pagina per visualizzare la notifica.

## <a name="further-reading"></a>Altre informazioni

Anche se questo non vuole essere un elenco completo, di seguito sono riportate alcune risorse correlate agli argomenti trattati in questo modulo che potrebbero risultare interessanti.

 * [Documentazione di Funzioni di Azure](https://docs.microsoft.com/azure/azure-functions/)

* [Azure Functions Challenge](https://aka.ms/afc) (Sfida di Funzioni di Azure)

* [Azure Serverless Computing Cookbook](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/) (Guida di riferimento dettagliata all'elaborazione serverless in Azure)

 * [Come usare l'archiviazione di accodamento da Node.js](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)

 * [Introduzione ad Azure Cosmos DB: API SQL](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)

* [Panoramica tecnica di Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)

* [Documentazione di Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
