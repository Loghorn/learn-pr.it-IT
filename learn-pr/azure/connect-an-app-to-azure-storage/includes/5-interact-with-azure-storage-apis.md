Archiviazione di Azure offre un'API web basato su Representational State Transfer (REST) per lavorare con i contenitori e i dati archiviati in ogni account. Esistono API indipendenti disponibili per lavorare con ogni tipo di dati che è possibile archiviare. È importante ricordare che sono disponibili quattro tipi di dati specifici:

- **I BLOB** per dati non strutturati, ad esempio i file binari e testuali.
- **Le code** di messaggistica persistente.
- **Tabelle** per l'archiviazione strutturata di chiavi/valori.
- **File** per SMB tradizionale condivisioni file.

## <a name="using-the-rest-api"></a>Utilizzo dell'API REST

Le API REST di archiviazione sono accessibili dai servizi in esecuzione in Azure tramite una rete virtuale e la rete Internet da qualsiasi applicazione che possono inviare una richiesta HTTP/HTTPS e ricevere una risposta HTTP/HTTPS.

Ad esempio, se si desidera elencare tutti i BLOB in un contenitore, si invierebbe un elemento, ad esempio:

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

Questo restituirebbe un blocco di XML con i dati specifici per l'account:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<EnumerationResults AccountName="https://[url-for-service-account]/">  
  <Containers>  
    <Container>  
      <Name>container1</Name>  
      <Url>https://[url-for-service-account]/container1</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 18:09:03 GMT</Last-Modified>  
        <Etag>0x8CAE7D0C4AF4487</Etag>  
      </Properties>  
      <Metadata>  
        <Color>orange</Color>  
        <ContainerNumber>01</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
    <Container>  
      <Name>container2</Name>  
      <Url>https://[url-for-service-account]/container2</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 17:26:40 GMT</Last-Modified>  
        <Etag>0x8CAE7CAD8C24928</Etag>  
      </Properties>  
      <Metadata>  
        <Color>pink</Color>  
        <ContainerNumber>02</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
    <Container>  
      <Name>container3</Name>  
      <Url>https://[url-for-service-account]/container3</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 17:26:40 GMT</Last-Modified>  
        <Etag>0x8CAE7CAD8EAC0BB</Etag>  
      </Properties>  
      <Metadata>  
        <Color>brown</Color>  
        <ContainerNumber>03</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
  </Containers>  
  <NextMarker>container4</NextMarker>  
</EnumerationResults>  
```

Questo approccio richiede tuttavia numerose l'analisi manuale e la creazione di pacchetti HTTP per lavorare con ogni API. Per questo motivo, Azure offre pre-compilato _librerie client_ che semplificano l'uso con il servizio per i comuni linguaggi e Framework.

## <a name="using-a-client-library"></a>Utilizzando una libreria client

Le librerie client è possono salvare una quantità significativa di lavoro per gli sviluppatori di applicazioni perché l'API viene testato e spesso fornisce wrapper accattivante per i modelli di dati inviati e ricevuti dall'API REST.

:::row:::
    :::column:::
        Microsoft offre librerie client di Azure che supportano un numero di linguaggi e Framework, tra cui: node. js - .NET - Java - Python - - Go :::column-end:::: :::column:::
        <br> ![Logo di esempio dei Framework supportati, che è possibile usare con Azure](../media/4-common-tools.png)
    :::column-end:::
:::row-end:::

Per recuperare lo stesso elenco di BLOB in c#, ad esempio, potremmo utilizziamo il frammento di codice seguente:

```csharp
CloudBlobDirectory directory = ...;
foreach (IEnumerable<IListBlobItem> blob in directory.ListBlobs(
                useFlatBlobListing: true,
                blobListingDetails: BlobListingDetails.All))
{
    // Work with blob item .. could be page blob, block blob, etc.
}
```

O in JavaScript:

```javascript
const containerName = "...";
const blobService = storage.createBlobService();

blobService.listBlobsSegmented(containerName, null, function (error, results) {
    if (results) {
        for (var i = 0, blob; blob = results.entries[i]; i++) {
            // Work with blob item .. could be page blob, block blob, etc.
        }
    }
});
```

> [!NOTE]
> Le librerie client sono semplicemente thin _wrapper_ tramite l'API REST. Operazioni eseguite esattamente ciò che si farebbe se è stato usato i servizi web direttamente. Queste librerie sono anche open source, rendendoli molto agevole. Cercare li su GitHub.

Aggiungere supporto delle librerie client per l'applicazione.