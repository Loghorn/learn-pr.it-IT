Servizi cognitivi di Azure è una suite completa di servizi intelligenti utili per arricchire le app. L'API Analisi del testo offre diverse operazioni per trasformare il testo in informazioni dettagliate importanti. Il servizio è stato usato per conoscere il sentiment nel feedback di testo dei clienti. È stata creata una soluzione ospitata in Funzioni di Azure per ordinare questi messaggi di testo in diversi bucket, o code, e rielaborarli.

Ora che si hanno le competenze per chiamare un'API REST, è possibile integrare facilmente questi servizi intelligenti nelle soluzioni in uso. Seguono tutti un modello simile:

- Iscriversi per ottenere una chiave di accesso.
- Esplorare la console di test dell'API.
- Formulare richieste usando la chiave di accesso e l'area corretta.
- Inviare richieste POST dalla soluzione e analizzare le risposte per informazioni dettagliate.

Questa funzionalità di intelligence è stata aggiunta alla logica serverless creata in Funzioni di Azure. È possibile chiamare facilmente questi servizi da altri tipi di app. Sono disponibili numerose librerie client, esercitazioni e guide introduttive per iniziare.

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>Suggerimenti per ottimizzare la soluzione

Di seguito sono riportati alcuni spunti da prendere in considerazione per proseguire.

- Testare la soluzione con altri esempi di testo. Decidere se le soglie impostate per classificare i punteggi del sentiment in positivo, negativo e neutrale sono appropriate.
- È consigliabile aggiungere un'altra funzione nell'app per le funzioni per leggere i messaggi dalla coda [!INCLUDE [negative-q](./q-name-negative.md)] e chiamare l'API Analisi del testo per trovare frasi chiave nel testo.
- La coda di input contiene feedback di testo non elaborato. In uno scenario reale è necessario associare il feedback a uno specifico ID utente come indirizzo di posta elettronica, numero di account e così via. Ottimizzare quindi gli elementi della coda di input trasformandoli in documenti JSON contenenti un campo ID e del testo. Usare quindi tale ID quando si lavora sul messaggio di testo.
- Attualmente la soluzione è "hardcoded" per l'inglese. Valutare le modifiche da apportare per fare in modo che sia possibile gestire il testo in tutte le lingue supportate dall'API Analisi del testo.
- Se si ha familiarità con App per la logica, visitare il collegamento al connettore integrato per Analisi del testo, disponibile nella sezione Altre informazioni.

Ora che si hanno le competenze per chiamare una di queste API Servizi cognitivi, è il momento di esaminare alcuni degli altri servizi e pensare a come usarli nelle soluzioni adottate.

## <a name="further-reading"></a>Altre informazioni

- [Panoramica di Analisi del testo](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [Demo di Analisi del testo](https://azure.microsoft.com/services/cognitive-services/text-analytics/)
- [Come rilevare il sentiment in Analisi del testo](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Documentazione dei servizi cognitivi](https://docs.microsoft.com/azure/cognitive-services/)

- [Connettore di App per la logica per Analisi del testo](https://docs.microsoft.com/connectors/cognitiveservicestextanalytics/)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
