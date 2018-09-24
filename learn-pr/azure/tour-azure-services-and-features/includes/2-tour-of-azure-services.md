Microsoft Azure è un set di servizi cloud in continua espansione che consente alle organizzazioni di rispondere alle sfide aziendali attuali e future. Azure offre la libertà di creare, gestire e distribuire applicazioni in un'ampia rete globale usando gli strumenti e i framework preferiti. Di seguito viene fornita una rapida panoramica dei principali servizi offerti da Azure.

#### <a name="azure-the-big-picture"></a>Azure: quadro generale

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuas]

## <a name="azure-services"></a>Servizi di Azure

Azure offre un'ampia gamma di servizi basati sul cloud, introducendo ogni mese funzionalità sempre nuove e migliorate. 

![Diagramma che mostra il quadro generale dei servizi di Azure](../media/2-azure-services.png "servizi di Azure")

Di seguito sono illustrate in dettaglio alcune delle funzionalità più diffuse: 

- Calcolo
- Rete
- Archiviazione
- Dispositivi mobili
- Database
- Web

### <a name="compute"></a>Calcolo

I servizi di calcolo sono uno dei motivi principali per cui le aziende passano alla piattaforma Azure. Azure offre una gamma di opzioni per ospitare applicazioni e servizi, tra cui:

![Figura che mostra tre tipi di servizio di calcolo di Azure: infrastruttura distribuita come servizio, piattaforma distribuita come servizio e funzioni come servizio. Sono inoltre mostrati i componenti gestiti dall'utente e i componenti gestiti da Azure per ogni tipo di servizio.](../media/2-iaas-paas-faas.png)

Di seguito sono riportati alcuni esempi di IaaS, PaaS e FaaS in Azure.

|  Tipo  |  Nome del servizio             | Funzione del servizio                                                         |
|--------|---------------------------|--------------------------------------------------------------------------|
| IaaS   | Macchine virtuali di Azure    | Macchine virtuali Windows o Linux ospitate in Azure                  | 
| IaaS   | Azure Kubernetes Service  | Consente la gestione di un cluster di macchine virtuali che eseguono servizi basati su contenitore   |
| PaaS   | Azure Service Fabric      | Piattaforma di sistemi distribuiti, eseguita in Azure o in locale               |
| PaaS   | Azure Batch               | Servizio gestito per applicazioni di calcolo parallele ad alte prestazioni |
| PaaS   | Servizi cloud di Azure      | Servizio gestito per l'esecuzione di applicazioni cloud                           |
| FaaS   | Istanze di contenitore di Azure | Rende disponibili contenitori senza che siano necessari servizi di provisioning di macchine virtuali o di livello superiore    |
| FaaS   | Funzioni di Azure           | Servizio FaaS gestito                                                     |

### <a name="networking"></a>Rete

La funzione principale dei servizi di rete di Azure è quella di collegare le risorse di calcolo e fornire l'accesso alle applicazioni. I servizi di rete di Azure includono una gamma di opzioni per connettere il mondo esterno ai servizi e alle funzionalità nei data center globali di Microsoft Azure.

Di seguito sono illustrate le caratteristiche dei servizi di rete di Azure:

|  Nome del servizio             | Funzione del servizio                                                                     |
| -------------             | -------------                                                                        |
| Rete virtuale di Azure     | Connette le macchine virtuali alle connessioni di rete privata virtuale (VPN) in ingresso                   |
| Azure Load Balancer       | Bilancia le connessioni in ingresso e in uscita ad applicazioni o endpoint di servizio       |
| Gateway applicazione di Azure | Ottimizza il recapito alle farm di server app aumentando al tempo stesso la sicurezza delle applicazioni             |
| Gateway VPN di Azure         | Accede a Reti virtuali di Azure attraverso gateway VPN ad alte prestazioni                |
| DNS di Azure                 | Consente risposte DNS eccezionalmente rapide e altissimi livelli di disponibilità dei domini                 |
| Rete per la distribuzione di contenuti di Azure  | Distribuisce contenuti con requisiti di larghezza di banda elevata a clienti a livello globale                          |
| Protezione DDoS di Azure     | Protegge le applicazioni ospitate da Azure da eventuali attacchi di tipo DDoS (Distributed Denial of Service) |
| Gestione traffico di Azure     | Distribuisce il traffico di rete tra le aree di Azure a livello mondiale                           |
| Azure ExpressRoute        | Si connette ad Azure usando connessioni sicure dedicate a larghezza di banda elevata                   |
| Azure Network Watcher     | Monitora e diagnostica i problemi di rete con l'analisi basata su scenario                  |
| Firewall di Azure            | Implementa un firewall con livelli elevati di sicurezza e disponibilità e con scalabilità illimitata      |
| Rete WAN virtuale di Azure         | Crea una rete WAN unificata per la connessione di siti locali e remoti         |

### <a name="storage"></a>Archiviazione

Azure offre quattro tipi principali di servizi di archiviazione. I servizi includono:

- **Archiviazione BLOB di Azure**: fornisce risorse di archiviazione per oggetti di dimensioni molto grandi, ad esempio file video o bitmap
- **Archiviazione file di Azure**: consente di creare condivisioni file accessibili e gestibili come un file server
- **Archiviazione code di Azure**: implementa un archivio per l'accodamento e il recapito affidabile dei messaggi tra le applicazioni
- **Archiviazione tabelle di Azure**: consiste in un archivio NoSLQ che ospita dati non strutturati, indipendenti da qualsiasi schema

Ognuno di questi servizi condivide caratteristiche comuni, che comprendono:

- Durabilità e disponibilità elevata con ridondanza e replica.
- Sicurezza tramite crittografia automatica e controllo degli accessi in base al ruolo.
- Scalabilità con archiviazione potenzialmente illimitata.
- Gestibilità, con funzionalità automatiche per la manutenzione e la risoluzione di problemi critici.
- Accessibilità da ogni parte del mondo tramite HTTP o HTTPS.

### <a name="mobile"></a>Dispositivi mobili

Azure consente agli sviluppatori di creare app coinvolgenti per iOS, Android e Windows in modo semplice e rapido usando un'ampia gamma di linguaggi nell'ambiente di sviluppo preferito. Le funzionalità che in passato richiedevano tempo e aumentavano i rischi del progetto, come l'aggiunta di una procedura di accesso aziendale e la connessione a risorse locali quali SAP, Oracle, SQL Server e SharePoint, sono ora facili da includere.

Tra le altre caratteristiche di questo servizio sono incluse:

- Sincronizzazione dei dati offline.
- Connettività ai dati locali.
- Trasmissione di notifiche push.
- Scalabilità automatica in base alle esigenze aziendali.

### <a name="databases"></a>Database

Azure offre più servizi di database per archiviare un'ampia gamma di volumi e tipi di dati. Inoltre, grazie alla connettività globale, questi dati sono immediatamente disponibili agli utenti.

|  Nome del servizio              | Funzione del servizio                                                                                |
| -------------              | -------------                                                                                   |
| Azure Cosmos DB            | Database distribuito a livello globale che supporta opzioni NoSQL                                       |
| Database SQL di Azure         | Database relazionale completamente gestito con scalabilità automatica, intelligenza integrata e sicurezza elevata    |
| Database di Azure per MySQL   | Database relazionale MySQL completamente gestito e scalabile con livelli elevati di scalabilità e sicurezza        |
| Database di Azure per PostgreSQL   | Database relazionale PostgreSQL completamente gestito e scalabile con livelli elevati di scalabilità e sicurezza   |
| SQL Server in VM          | Ospita app SQL Server aziendali nel cloud                                                    |
| Azure SQL Data Warehouse   | Data warehouse completamente gestito con sicurezza integrata a ogni livello di scalabilità senza costi aggiuntivi    |
| Servizio Migrazione del database di Azure    | Esegue la migrazione dei database nel cloud senza modifiche al codice delle applicazioni                  |
| Cache Redis di Azure          | Memorizza nella cache i dati statici di uso frequente per ridurre la latenza dei dati e delle applicazioni                   |
| Database di Azure per MariaDB | Database relazionale MySQL completamente gestito e scalabile con livelli elevati di scalabilità e sicurezza        |

### <a name="web"></a>Web

I servizi Web di Azure includono le funzionalità seguenti:

| Nome del servizio | Descrizione |
|--------------|-------------|
| Servizio app di Azure | Consente di creare rapidamente app cloud potenti per il Web e i dispositivi mobili. |
| Hub di notifica di Azure |Consente di inviare notifiche push a qualsiasi piattaforma da qualsiasi back-end. |
| Gestione API di Azure | Consente di pubblicare API per sviluppatori, partner e dipendenti in modo sicuro e scalabile. |
| Ricerca di Azure | Offre un servizio di ricerca completamente gestito. |
| Funzionalità App Web del servizio app di Azure | Consente di creare e distribuire app Web di importanza strategica su vasta scala. |
| Servizio Azure SignalR | Consente di aggiungere facilmente funzionalità Web in tempo reale. |

Ora che sono state identificate alcune delle aree che possono risultare interessanti per un'azienda che intenda eseguire la migrazione ad Azure verrà illustrato come usare i servizi e le funzionalità.
