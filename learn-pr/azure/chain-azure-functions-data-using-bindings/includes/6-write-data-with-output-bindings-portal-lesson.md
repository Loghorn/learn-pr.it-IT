Analogamente alle associazioni di input, ci sono diversi tipi di associazioni di output. Non tutti i tipi, tuttavia, supportano sia l'input che l'output. Sarà possibile usare questi tipi ogni volta che si vogliono inviare o archiviare dati. In questa lezione si prenderanno in esame i tipi che supportano le associazioni di output e verrà spiegato quando usarli.

## <a name="output-binding-types"></a>Tipi di associazioni di output

- **Archiviazione BLOB** - È possibile usare l'associazione di output BLOB per scrivere BLOB.

- **Cosmos DB** - L'associazione di output di Azure Cosmos DB consente di scrivere un nuovo documento in un database Azure Cosmos DB tramite l'API SQL.

- **Hub eventi** - È possibile usare l'associazione di output di Hub eventi per scrivere eventi in un flusso di eventi. Per scrivere eventi in un hub eventi, è necessario disporre dell'autorizzazione di invio.

- **HTTP** - Usare l'associazione di output HTTP per rispondere al mittente di una richiesta HTTP. Questa associazione richiede un trigger HTTP e consente di personalizzare la risposta associata alla richiesta del trigger. È possibile usare questa associazione anche per connettersi a webhook.

- **Microsoft Graph** - Le associazioni di output di Microsoft Graph consentono di scrivere nei file in OneDrive, modificare i dati di Excel e inviare posta elettronica tramite Outlook.

- **App per dispositivi mobili** - L'associazione di output di App per dispositivi mobili consente di scrivere un nuovo record in una tabella di App per dispositivi mobili.

- **Hub di notifica** - È possibile inviare notifiche push con le associazioni di output di Hub di notifica.

- **Archiviazione code** - Usare l'associazione di output di Archiviazione code di Azure per scrivere i messaggi in una coda.

- **SendGrid** - È possibile inviare messaggi di posta elettronica tramite le associazioni di SendGrid.

- **Bus di servizio** - Usare l'associazione di output del bus di servizio di Azure per inviare messaggi della coda o dell'argomento.

- **Archiviazione tabelle** - Usare un'associazione di output dell'archiviazione tabelle di Azure per scrivere in una tabella in un account di archiviazione di Azure.

- **Twilio** - È possibile inviare messaggi di testo con Twilio.

Per creare un'associazione come output, è necessario definire `direction` come `out`. I parametri per ogni tipo di associazione possono variare.

## <a name="combining-input-and-output-bindings"></a>Combinazione di associazioni di input e di output 

È possibile applicare più associazioni a una singola funzione. Ciò consente di definire associazioni sia di input che di output, che possono anche essere dello stesso tipo.