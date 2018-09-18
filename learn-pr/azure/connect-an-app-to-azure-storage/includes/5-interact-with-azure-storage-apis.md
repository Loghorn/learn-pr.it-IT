<span data-ttu-id="a3b12-101">Archiviazione di Azure offre un'API Web basata su REST (Representational State Transfer, trasferimento di stato rappresentativo) da usare con i contenitori e i dati archiviati in ogni account.</span><span class="sxs-lookup"><span data-stu-id="a3b12-101">Azure Storage provides a Representational State Transfer (REST) based web API to work with the containers and data stored in each account.</span></span> <span data-ttu-id="a3b12-102">Sono disponibili API indipendenti da usare con ogni tipo di dati che è possibile archiviare.</span><span class="sxs-lookup"><span data-stu-id="a3b12-102">There are independent APIs available to work with each type of data you can store.</span></span> <span data-ttu-id="a3b12-103">È importante ricordare che sono disponibili quattro tipi di dati specifici:</span><span class="sxs-lookup"><span data-stu-id="a3b12-103">Recall that we have four specific data types:</span></span>

- <span data-ttu-id="a3b12-104">**BLOB** per i dati non strutturati, ad esempio file binari e file di testo.</span><span class="sxs-lookup"><span data-stu-id="a3b12-104">**Blobs** for unstructured data such as binary and text files.</span></span>
- <span data-ttu-id="a3b12-105">**Code** per la messaggistica persistente.</span><span class="sxs-lookup"><span data-stu-id="a3b12-105">**Queues** for persistent messaging.</span></span>
- <span data-ttu-id="a3b12-106">**Tabelle** per l'archiviazione strutturata di chiavi/valori.</span><span class="sxs-lookup"><span data-stu-id="a3b12-106">**Tables** for structured storage of key/values.</span></span>
- <span data-ttu-id="a3b12-107">**File** per le condivisioni file SMB tradizionali.</span><span class="sxs-lookup"><span data-stu-id="a3b12-107">**Files** for traditional SMB file shares.</span></span>

## <a name="using-the-rest-api"></a><span data-ttu-id="a3b12-108">Uso dell'API REST</span><span class="sxs-lookup"><span data-stu-id="a3b12-108">Using the REST API</span></span>

<span data-ttu-id="a3b12-109">Le API REST di archiviazione sono accessibili dai servizi in esecuzione in Azure tramite una rete virtuale oppure tramite Internet da qualsiasi applicazione in grado di inviare una richiesta HTTP/HTTPS e ricevere una risposta HTTP/HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a3b12-109">The Storage REST APIs are accessible from services running in Azure over a virtual network, or over the Internet from any application that can send an HTTP/HTTPS request and receive an HTTP/HTTPS response.</span></span>

<span data-ttu-id="a3b12-110">Ad esempio, per elencare tutti i BLOB in un contenitore, si potrebbe inviare una richiesta simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="a3b12-110">For example, if you wanted to list all the blobs in a container, you would send something like:</span></span>

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

<span data-ttu-id="a3b12-111">Questa restituirebbe un blocco XML con i dati specifici per l'account:</span><span class="sxs-lookup"><span data-stu-id="a3b12-111">This would return an XML block with data specific to the account:</span></span>

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

<span data-ttu-id="a3b12-112">Tuttavia, questo approccio richiede molta analisi manuale e la creazione di pacchetti HTTP per ogni API.</span><span class="sxs-lookup"><span data-stu-id="a3b12-112">However, this approach requires a lot of manual parsing and creation of HTTP packets to work with each API.</span></span> <span data-ttu-id="a3b12-113">Per questo motivo, Azure offre _librerie client_ precompilate che semplificano l'uso del servizio per i linguaggi e i framework più diffusi.</span><span class="sxs-lookup"><span data-stu-id="a3b12-113">For this reason, Azure provides pre-built _client libraries_ that make working with the service easier for common languages and frameworks.</span></span>

## <a name="using-a-client-library"></a><span data-ttu-id="a3b12-114">Uso di una libreria client</span><span class="sxs-lookup"><span data-stu-id="a3b12-114">Using a client library</span></span>

<span data-ttu-id="a3b12-115">Le librerie client possono far risparmiare una notevole quantità di lavoro agli sviluppatori di applicazioni, perché l'API viene testato e spesso fornisce wrapper migliori per i modelli di dati inviati e ricevuti dall'API REST.</span><span class="sxs-lookup"><span data-stu-id="a3b12-115">Client libraries can save a significant amount of work for application developers because the API is tested and it often provides nicer wrappers around the data models sent and received by the REST API.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="a3b12-116">Microsoft offre librerie client di Azure che supportano numerosi linguaggi e framework tra cui: .NET, Java Python, Node.js e Go :::column-end::::</span><span class="sxs-lookup"><span data-stu-id="a3b12-116">Microsoft has Azure client libraries that support a number of languages and frameworks including: - .NET - Java - Python - Node.js - Go :::column-end::::</span></span> :::column:::
        <br> <span data-ttu-id="a3b12-117">![Logo di esempio dei framework supportati che è possibile usare con Azure](../media/4-common-tools.png)</span><span class="sxs-lookup"><span data-stu-id="a3b12-117">![Sample logos of supported frameworks you can use with Azure](../media/4-common-tools.png)</span></span>
    :::column-end:::
:::row-end:::

<span data-ttu-id="a3b12-118">Ad esempio, per recuperare lo stesso elenco di BLOB in C# si può usare il frammento di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a3b12-118">For example, to retrieve the same list of blobs in C#, we could use the following code snippet:</span></span>

```csharp
CloudBlobDirectory directory = ...;
foreach (IEnumerable<IListBlobItem> blob in directory.ListBlobs(
                useFlatBlobListing: true,
                blobListingDetails: BlobListingDetails.All))
{
    // Work with blob item .. could be page blob, block blob, etc.
}
```

<span data-ttu-id="a3b12-119">O in JavaScript:</span><span class="sxs-lookup"><span data-stu-id="a3b12-119">Or in JavaScript:</span></span>

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
> <span data-ttu-id="a3b12-120">Le librerie client sono semplicemente _wrapper_ leggeri per l'API REST.</span><span class="sxs-lookup"><span data-stu-id="a3b12-120">The client libraries are just thin _wrappers_ over the REST API.</span></span> <span data-ttu-id="a3b12-121">Fanno esattamente ciò che si farebbe usando i servizi Web direttamente.</span><span class="sxs-lookup"><span data-stu-id="a3b12-121">They are doing exactly what you would do if you used the web services directly.</span></span> <span data-ttu-id="a3b12-122">Queste librerie sono anche open source e dunque sono molto trasparenti.</span><span class="sxs-lookup"><span data-stu-id="a3b12-122">These libraries are also open source making them very transparent.</span></span> <span data-ttu-id="a3b12-123">Sono reperibili su GitHub.</span><span class="sxs-lookup"><span data-stu-id="a3b12-123">Look for them on GitHub.</span></span>

<span data-ttu-id="a3b12-124">È il momento di aggiungere all'applicazione il supporto delle librerie client.</span><span class="sxs-lookup"><span data-stu-id="a3b12-124">Let's add the client library support for our application.</span></span>