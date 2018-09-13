Ogni sviluppatore di software vuole risparmiare quanto più tempo possibile quando scrive il codice. Per interagire con Archiviazione di Azure, è possibile usare l'endpoint REST del servizio, ma questa operazione implica una notevole quantità di lavoro. A causa di questo problema, sono state sviluppate librerie per accelerare la connessione e l'uso di Archiviazione di Azure. Queste vengono chiamate comunemente *librerie client* e costituiscono in genere il modo più semplice per interagire con Archiviazione di Azure. 

Ecco come aggiungere la libreria client appropriata come riferimento nell'applicazione .NET.

## <a name="application-programming-interface-api"></a>API (Application Programming Interface)

Archiviazione di Azure espone servizi tramite un set di API (Application Programming Interface) su Internet usando protocolli REST (Representational State Transfer). Queste API sono completamente documentate in [Azure Storage Services REST API Reference](https://docs.microsoft.com/rest/api/storageservices/) (Informazioni di riferimento sulle API REST dei servizi di Archiviazione di Azure). Benché sia possibile interagire direttamente con queste API, l'accesso ad Archiviazione di Azure avviene in genere tramite una libreria client. Le librerie client permettono agli sviluppatori di applicazioni di ridurre notevolmente il lavoro, perché l'API è ben testata.

## <a name="language-support"></a>Supporto per vari linguaggi

Per l'accesso ad Archiviazione di Azure sono disponibili diverse librerie client che supportano numerosi linguaggi. Questa [pagina della documentazione degli strumenti SDK](https://docs.microsoft.com/azure/#pivot=sdkstools) contiene i collegamenti alle librerie client o agli SDK per tutti i linguaggi attualmente supportati. Sono inclusi .NET, Java, Python, Node.js e Go.

## <a name="summary"></a>Riepilogo

Archiviazione di Azure fornisce un endpoint REST per interagire con tutti i servizi. Anche se è possibile usare questo endpoint, in genere si usa una libreria client. Archiviazione di Azure supporta diversi linguaggi, tra cui .NET, Java, Python e Node.js.
