Archiviazione di Azure offre un'API Web basata su REST (Representational State Transfer, trasferimento di stato rappresentativo) da usare con i contenitori e i dati archiviati in ogni account. Sono disponibili API indipendenti da usare con ogni tipo di dati che è possibile archiviare. È importante ricordare che sono disponibili quattro tipi di dati specifici:

- **BLOB** per i dati non strutturati, ad esempio file binari e file di testo.
- **Code** per la messaggistica persistente.
- **Tabelle** per l'archiviazione strutturata di chiavi/valori.
- **File** per le condivisioni file SMB tradizionali.

## <a name="using-the-rest-api"></a>Uso dell'API REST

Le API REST di archiviazione sono accessibili dai servizi in esecuzione in Azure tramite una rete virtuale oppure tramite Internet da qualsiasi applicazione in grado di inviare una richiesta HTTP/HTTPS e ricevere una risposta HTTP/HTTPS.

Ad esempio, per elencare tutti i BLOB in un contenitore, si potrebbe inviare una richiesta simile alla seguente:

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

Questa restituirebbe un blocco XML con i dati specifici per l'account:

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

Tuttavia, questo approccio richiede molta analisi manuale e la creazione di pacchetti HTTP per ogni API. Per questo motivo, Azure offre _librerie client_ precompilate che semplificano l'uso del servizio per i linguaggi e i framework più diffusi.

## <a name="using-a-client-library"></a>Uso di una libreria client

Le librerie client possono far risparmiare una notevole quantità di lavoro agli sviluppatori di applicazioni, perché l'API viene testato e spesso fornisce wrapper migliori per i modelli di dati inviati e ricevuti dall'API REST.

:::row:::
    :::column:::
        Microsoft offre librerie client di Azure che supportano numerosi linguaggi e framework tra cui: .NET, Java Python, Node.js e Go :::column-end:::: :::column:::
        <br> ![Logo di esempio dei framework supportati che è possibile usare con Azure](../media/4-common-tools.png)
    :::column-end:::
:::row-end:::

Ad esempio, per recuperare lo stesso elenco di BLOB in C# si può usare il frammento di codice seguente:

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
> Le librerie client sono semplicemente _wrapper_ leggeri per l'API REST. Fanno esattamente ciò che si farebbe usando i servizi Web direttamente. Queste librerie sono anche open source e dunque sono molto trasparenti. Sono reperibili su GitHub.

È il momento di aggiungere all'applicazione il supporto delle librerie client.