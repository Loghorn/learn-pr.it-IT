Analogamente ai binding di input, sono disponibili più tipi di associazioni di output.

Sono disponibili più tipi di associazioni di output, tuttavia, non tutti i tipi supportano sia di input e output. Sarà possibile usarli ogni volta che si vuole inviare o archiviare i dati. In questo caso, esamineremo i tipi che supportano le associazioni di output e quando utilizzarle.

## <a name="output-binding-types"></a>Tipi di associazione di output

- **Archivio BLOB** è possibile usare l'associazione per scrivere i BLOB di output di blob.

- **COSMOS DB** associazione consente di scrivere un nuovo documento a un database di Azure Cosmos DB usando l'API SQL di Azure Cosmos DB output.

- **Hub eventi** Usa l'associazione per scrivere eventi in un flusso di eventi di output di hub eventi. Per scrivervi eventi, è necessario disporre dell'autorizzazione Send verso un Hub eventi.

- **HTTP** Usa associazione per rispondere al mittente della richiesta HTTP di output HTTP. Questa associazione richiede un trigger HTTP e consente di personalizzare la risposta associata alla richiesta del trigger.

- **Microsoft Graph** associazioni di output di Microsoft Graph consentono di scrivere i file in OneDrive, modificare i dati di Excel e invio di posta elettronica tramite Outlook.

- **App per dispositivi mobili** scritture di associazione di output di App per dispositivi mobili un nuovo record in una tabella di App per dispositivi mobili.

- **Hub di notifica** è possibile inviare notifiche push con le associazioni di output di hub di notifica.

- **Archiviazione code** associazione per scrivere messaggi in una coda di output archiviazione code di Azure.

- **Inviare griglia** trasmissione messaggi di posta elettronica con le associazioni di SendGrid.

- **Il Bus di servizio** associazione per inviare messaggi di coda o argomento di output per usare Azure Service Bus.

- **Archivio tabelle** Usa associazione per scrivere in una tabella in un account di archiviazione di Azure di output per una risorsa di archiviazione tabelle di Azure.

- **Twilio** inviare messaggi di testo con Twilio.

- **I Webhook** Usa associazione per rispondere al mittente della richiesta HTTP di output HTTP. Questa associazione richiede un trigger HTTP e consente di personalizzare la risposta associata alla richiesta del trigger.

## <a name="how-to-create-an-output-binding"></a>Come creare un'associazione di output?
Per definire un'associazione di input, è necessario definire il `direction` come `out`.
I parametri per ogni tipo di associazione possono differire; sono quelle descritte nella [documentazione Microsoft](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings)

## <a name="combining-input-and-output-bindings"></a>Combinazione di input e le associazioni di output 
è possibile applicare più associazioni a una singola funzione. In questo modo è possibile definire associazioni di input e di output.

E di input e output possono anche essere dello stesso tipo di binding...