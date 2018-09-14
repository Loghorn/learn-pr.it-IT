Servizi cognitivi Microsoft è una suite completa di servizi intelligenti che possiamo utilizzare per arricchire le app. L'API di Analitica testo dispone di diverse operazioni per trasformare il testo in informazioni dettagliate significative. Il servizio viene usato per individuare sentiment commenti e suggerimenti di testo da parte dei clienti. È stata creata una soluzione ospitata in funzioni di Azure per organizzare tali messaggi di testo in diversi bucket oppure code, per un'ulteriore elaborazione.

Una volta che si sa come chiamare un'API REST, è possibile integrare facilmente questi servizi intelligenti nelle tue soluzioni. Tutte le seguono un modello simile:

- Iscriversi a una chiave di accesso
- Esplorare la console di test API
- Formulare richieste usando la chiave di accesso e l'area corretta.
- Registra le richieste dalla soluzione e analizzare le risposte per insights.

È stato aggiunto l'intelligence per la logica senza server creata in funzioni di Azure. È possibile chiamare facilmente questi servizi da altri tipi di App. Sono disponibili numerose librerie client, esercitazioni e guide introduttive per iniziare a usare.

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>Suggerimenti per un'ulteriore miglioramento della nostra soluzione

Di seguito sono riportate alcune idee da prendere in considerazione se si vuole definire ciò che è ulteriormente.

- Testare la soluzione con altri esempi di testo. Decidere se le soglie che impostiamo per classificare i punteggi del sentiment in positivi, negativi e neutro sono appropriate.
- È consigliabile aggiungere un'altra funzione nell'app per le funzioni che legge i messaggi dal [!INCLUDE [negative-q](./q-name-negative.md)] coda e chiama l'API di Analitica di testo per trovare frasi chiave nel testo.
- La coda di input contiene commenti e suggerimenti di testo non elaborato. Del mondo reale, è necessario associare commenti e suggerimenti a qualche forma di ID utente, ad esempio indirizzo di posta elettronica, numero di account e così via. Migliora gli elementi della coda di input da documenti JSON contenenti e campo e il testo ID. Quindi utilizzare tale ID quando si lavora sul testo del messaggio.
- Attualmente la nostra soluzione è "hardcoded" su inglese. Considerare le modifiche è necessario implementare per renderla in grado di gestire testo in tutte le lingue supportate dall'API di Analitica di testo.
- Se si ha familiarità con App per la logica, visitare il collegamento a connector integrato per analitica di testo nella sezione di letture di approfondimento.

Ora che si sa come chiamare una di queste API servizi cognitivi, Esplora alcuni degli altri servizi e pensare a come è possibile usarli nelle soluzioni.

## <a name="further-reading"></a>Altre informazioni

- [Panoramica di Analisi del testo](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [Come rilevare sentimenti in testo Analitica](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Documentazione dei servizi cognitivi](https://docs.microsoft.com/azure/cognitive-services/)

- [Connettore di App per la logica di testo Analitica](https://docs.microsoft.com/connectors/cognitiveservicestextanalytics/)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
