<span data-ttu-id="86b56-101">Archiviazione di Azure mette a disposizione un'API REST da usare con i contenitori e i dati archiviati in ogni account.</span><span class="sxs-lookup"><span data-stu-id="86b56-101">Azure Storage provides a REST API to work with the containers and data stored in each account.</span></span> <span data-ttu-id="86b56-102">Sono disponibili API indipendenti utilizzabili con ogni tipo di dati che è possibile archiviare.</span><span class="sxs-lookup"><span data-stu-id="86b56-102">There are independent APIs available to work with each type of data you can store.</span></span> <span data-ttu-id="86b56-103">È importante ricordare che sono disponibili quattro tipi di dati specifici:</span><span class="sxs-lookup"><span data-stu-id="86b56-103">Recall that we have four specific data types:</span></span>

- <span data-ttu-id="86b56-104">**BLOB** per i dati non strutturati, ad esempio file binari e file di testo.</span><span class="sxs-lookup"><span data-stu-id="86b56-104">**Blobs** for unstructured data such as binary and text files.</span></span>
- <span data-ttu-id="86b56-105">**Code** per la messaggistica persistente.</span><span class="sxs-lookup"><span data-stu-id="86b56-105">**Queues** for persistent messaging.</span></span>
- <span data-ttu-id="86b56-106">**Tabelle** per l'archiviazione strutturata di chiavi/valori.</span><span class="sxs-lookup"><span data-stu-id="86b56-106">**Tables** for structured storage of key/values.</span></span>
- <span data-ttu-id="86b56-107">**File** per le condivisioni file SMB tradizionali.</span><span class="sxs-lookup"><span data-stu-id="86b56-107">**Files** for traditional SMB file shares.</span></span>

## <a name="using-the-rest-api"></a><span data-ttu-id="86b56-108">Uso dell'API REST</span><span class="sxs-lookup"><span data-stu-id="86b56-108">Using the REST API</span></span>

<span data-ttu-id="86b56-109">Le API REST di archiviazione sono accessibili via Internet da qualsiasi posizione e da parte di tutte le applicazioni in grado di inviare una richiesta HTTP/HTTPS e di ricevere una risposta HTTP/HTTPS.</span><span class="sxs-lookup"><span data-stu-id="86b56-109">The Storage REST APIs are accessible from anywhere on the Internet, by any application that can send an HTTP/HTTPS request and receive an HTTP/HTTPS response.</span></span>

<span data-ttu-id="86b56-110">Per elencare tutti i BLOB in un contenitore, ad esempio, è possibile inviare una richiesta simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="86b56-110">For example, if you wanted to list all the blobs in a container, you would send something like:</span></span>

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

<span data-ttu-id="86b56-111">Questa restituisce un blocco XML con i dati specifici per l'account:</span><span class="sxs-lookup"><span data-stu-id="86b56-111">This would return an XML block with data specific to the account:</span></span>

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

<span data-ttu-id="86b56-112">Questo approccio, tuttavia, richiede molta analisi manuale e la creazione di pacchetti HTTP per ogni API.</span><span class="sxs-lookup"><span data-stu-id="86b56-112">However, this approach requires a lot of manual parsing and the creation of HTTP packets to work with each API.</span></span> <span data-ttu-id="86b56-113">Per questo motivo, Azure offre _librerie client_ precompilate che semplificano l'uso del servizio per i linguaggi e i framework più diffusi.</span><span class="sxs-lookup"><span data-stu-id="86b56-113">For this reason, Azure provides pre-built _client libraries_ that make working with the service easier for common languages and frameworks.</span></span>

## <a name="using-a-client-library"></a><span data-ttu-id="86b56-114">Uso di una libreria client</span><span class="sxs-lookup"><span data-stu-id="86b56-114">Using a client library</span></span>

<span data-ttu-id="86b56-115">Le librerie client possono far risparmiare una notevole quantità di lavoro agli sviluppatori di applicazioni, perché l'API viene testato e spesso fornisce wrapper migliori per i modelli di dati inviati e ricevuti dall'API REST.</span><span class="sxs-lookup"><span data-stu-id="86b56-115">Client libraries can save a significant amount of work for application developers because the API is tested and it often provides nicer wrappers around the data models sent and received by the REST API.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="86b56-116">Microsoft offre librerie client di Azure che supportano numerosi linguaggi e framework tra cui: .NET, Java Python, Node.js e Go :::column-end::::</span><span class="sxs-lookup"><span data-stu-id="86b56-116">Microsoft has Azure client libraries that support a number of languages and frameworks including: - .NET - Java - Python - Node.js - Go :::column-end::::</span></span> :::column:::
        <br> <span data-ttu-id="86b56-117">![Logo di esempio dei framework supportati che è possibile usare con Azure](../media/4-common-tools.png)</span><span class="sxs-lookup"><span data-stu-id="86b56-117">![Sample logos of supported frameworks you can use with Azure](../media/4-common-tools.png)</span></span> 
    :::column-end:::
:::row-end:::

<span data-ttu-id="86b56-118">Ad esempio, per recuperare lo stesso elenco di BLOB in C# si può usare il frammento di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="86b56-118">For example, to retrieve the same list of blobs in C#, we could use the following code snippet:</span></span>

```csharp
CloudBlobDirectory directory = ...;
foreach (IEnumerable<IListBlobItem> blob in directory.ListBlobs(
                useFlatBlobListing: true,
                blobListingDetails: BlobListingDetails.All))
{
    // Work with blob item .. could be page blob, block blob, etc.
}
```

<span data-ttu-id="86b56-119">O in JavaScript:</span><span class="sxs-lookup"><span data-stu-id="86b56-119">Or in JavaScript:</span></span>

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
> <span data-ttu-id="86b56-120">Le librerie client sono semplicemente _wrapper_ leggeri per l'API REST.</span><span class="sxs-lookup"><span data-stu-id="86b56-120">The client libraries are just thin _wrappers_ over the REST API.</span></span> <span data-ttu-id="86b56-121">Fanno esattamente ciò che è possibile fare usando i servizi Web direttamente.</span><span class="sxs-lookup"><span data-stu-id="86b56-121">They are doing exactly what you would do if you used the web services directly.</span></span> <span data-ttu-id="86b56-122">Queste librerie sono anche open source e sono quindi molto trasparenti.</span><span class="sxs-lookup"><span data-stu-id="86b56-122">These libraries are also open source, making them very transparent.</span></span> <span data-ttu-id="86b56-123">Sono reperibili su GitHub.</span><span class="sxs-lookup"><span data-stu-id="86b56-123">Look for them on GitHub.</span></span>

<span data-ttu-id="86b56-124">È il momento di aggiungere all'applicazione il supporto delle librerie client.</span><span class="sxs-lookup"><span data-stu-id="86b56-124">Let's add client library support to our application.</span></span>