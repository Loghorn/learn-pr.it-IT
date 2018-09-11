Spesso le organizzazioni dispongono di account di archiviazione multipli per essere in grado di implementare diversi set di requisiti. Prendendo ad esempio un produttore di cioccolato, potrebbe esistere un account di archiviazione per i dati aziendali privati e uno per i file destinati ai consumatori. Qui sono riportati i fattori dei criteri controllati da un account di archiviazione che consentono di decidere il numero di account necessari.

## <a name="what-is-azure-storage"></a>Che cos'è Archiviazione di Azure?

Azure offre diversi modi per archiviare i dati. Ci sono diverse opzioni di database come Azure SQL, Cosmos DB, Tabelle di Azure, ecc. Sono possibili diversi modi per memorizzare messaggi come Code di Azure e Hub eventi. È anche possibile archiviare file separati con opzioni come File di Azure e BLOB di Azure.

Azure ha selezionato quattro di questi servizi dati e li ha riuniti sotto il nome _Archiviazione di Azure_. I quattro servizi sono: BLOB di Azure, File di Azure, Code di Azure e Tabelle di Azure. L'illustrazione seguente mostra gli elementi di Archiviazione di Azure.

![Illustrazione che elenca i servizi dati di Azure che fanno parte di Archiviazione di Azure.](../media-drafts/2-azure-storage.png)

A questi quattro servizi è stato riservato un trattamento speciale in quanto sono tutti servizi di archiviazione semplici basati sul cloud e sono spesso utilizzati insieme nella stessa applicazione.

## <a name="what-is-a-storage-account"></a>Che cos’è un account di archiviazione?

Un _account di archiviazione_ è un contenitore che raggruppa un set di servizi di Archiviazione di Azure. Solo i servizi di dati di Archiviazione di Azure possono essere inclusi in un account di archiviazione (BLOB di Azure, File di Azure, Code di Azure e Tabelle di Azure). La figura seguente mostra un account di archiviazione che contiene diversi servizi dati.

![Illustrazione di un account di archiviazione di Azure contenente una raccolta mista di servizi dati.](../media-drafts/2-what-is-a-storage-account.png)

La combinazione di servizi di dati in un account di archiviazione consente di gestirli come gruppo. Le impostazioni specificate al momento della creazione dell'account, o quelle modificate dopo la creazione, vengono applicate a tutti gli elementi dell'account. L'eliminazione dell'account di archiviazione comporta l'eliminazione di tutti i dati archiviati al suo interno.

Un account di archiviazione è una risorsa di Azure ed è incluso in un gruppo di risorse. La seguente illustrazione mostra una sottoscrizione di Azure contenente più gruppi di risorse in cui ogni gruppo contiene uno o più account di archiviazione.

![Illustrazione di una sottoscrizione di Azure che contiene più gruppi di risorse e account di archiviazione.](../media-drafts/2-resource-groups-and-storage-accounts.png)

Altri servizi di dati di Azure come Azure SQL e Cosmos DB sono gestite come risorse di Azure indipendenti e non possono essere inclusi in un account di archiviazione. La figura seguente mostra una tipica disposizione: BLOB, File, Code e Tabelle si trovano all'interno degli account di archiviazione mentre gli altri servizi no.

![Illustrazione di una sottoscrizione di Azure che mostra alcuni servizi dati che non possono essere inseriti in un account di archiviazione.](../media-drafts/2-typical-subscription-organization.png)

## <a name="storage-account-settings"></a>Impostazioni account di archiviazione

Un account di archiviazione definisce un criterio che si applica a tutti i servizi di archiviazione nell'account. Ad esempio, è possibile specificare che tutti i servizi indipendenti vengano archiviati nel Data Center Stati Uniti occidentali, accessibile solo tramite https e addebitati alla sottoscrizione del reparto vendite.

Le impostazioni controllate da un account di archiviazione sono:

- **Sottoscrizione**: sottoscrizione di Azure che verrà fatturata per i servizi presenti nell'account.

- **Percorso**: il Data Center in cui verranno archiviati i servizi nell'account.

- **Prestazioni**: determina i servizi di dati che è possibile avere nel proprio account di archiviazione e il tipo di disco dell'hardware sottostante. **Standard** consente di disporre di qualsiasi servizio di dati (Blob, File, Coda, Tabella) e sfrutta le unità disco magnetiche. **Premium** limita a un tipo specifico di BLOB denominato _BLOB di pagina_ e utilizza unità a stato solido per l'archiviazione.

- **Replica**: determina la strategia usata per creare copie dei dati per la protezione da errori hardware o disastri naturali. Come minimo, Azure manterrà automaticamente una copia dei dati all'interno del Data Center associato all'account di archiviazione. Questo viene chiamato LRS (Locally Redundant Storage, archiviazione con ridondanza locale) e protegge da guasti hardware, ma non protegge da un evento che inabilita l'intero Data Center. È possibile eseguire l'aggiornamento a una delle altre opzioni, ad esempio archiviazione con ridondanza geografica (GRS) per ottenere la replica in altri Data Center in tutto il mondo.

- **Livello di accesso**: controlla dopo quanto tempo l’utente sarà in grado di accedere ai BLOB nell'account di archiviazione. La funzione di accesso frequente consente un accesso più rapido rispetto all’accesso sporadico, ma a un costo maggiore. Questo si applica solo ai BLOB e viene usato come valore predefinito per nuovi BLOB.

- **Trasferimento sicuro obbligatorio**: funzionalità di sicurezza che determina i protocolli supportati per l'accesso: abilitato richiede https mentre disabilitato consente http.

- **Reti virtuali**: funzionalità di sicurezza che consente le richieste di accesso in ingresso solo da reti virtuali specificate.

## <a name="how-many-storage-accounts-do-you-need"></a>Quanti account di archiviazione sono necessari?

Un account di archiviazione rappresenta un insieme di impostazioni quali posizione, strategia di replica, sottoscrizione, ecc. È necessario un account di archiviazione per ogni gruppo di impostazioni che si desidera applicare ai dati. La figura seguente mostra due account di archiviazione che si differenziano per una sola impostazione; ovvero una differenza sufficiente per richiedere account di archiviazione separati.

![Illustrazione che mostra due account di archiviazione con impostazioni diverse.](../media-drafts/2-multiple-storage-accounts.png)

Il numero di account di archiviazione necessari è in genere determinato dalla diversità dei dati, dalla sensibilità ai costi e dalla tolleranza per i costi di gestione.

### <a name="data-diversity"></a>Diversità dei dati

Le organizzazioni spesso generano dati che variano a seconda di dove vengono consumati, di quanto sono sensibili, di quale gruppo paga le bollette, ecc. La diversità relativa a uno qualsiasi di questi vettori può portare a più account di archiviazione. Si prendano in considerazione due esempi:

1. Si dispone di dati specifici per un paese o una regione? In tal caso, per motivi di prestazioni o conformità, potrebbe essere opportuno collocarlo in un Data Center del paese in questione. È necessario un account di archiviazione per ogni posizione.

1. Si dispone di dati proprietari e di dati di consumo pubblico? In questo caso, è possibile abilitare le reti virtuali per i dati proprietari ed evitarla per i dati pubblici. Ciò richiederà anche account di archiviazione separati.

In generale, una maggiore diversità indica la necessità di un maggior numero di account di archiviazione.

### <a name="cost-sensitivity"></a>Sensibilità ai costi

Un account di archiviazione di per sé non ha alcun costo finanziario, tuttavia, le impostazioni scelte per l'account influenzano il costo dei servizi nell'account. L’archiviazione con ridondanza geografica è più costosa dell’archiviazione con ridondanza locale. Prestazioni Premium e il livello ad accesso frequente aumentano il costo dei BLOB.

È possibile usare più account di archiviazione per ridurre i costi. Ad esempio, è possibile suddividere i dati in categorie critiche e non critiche. È possibile inserire i dati critici in un account di archiviazione con archiviazione con ridondanza geografica e inserire i dati non critici in un account di archiviazione diverso con archiviazione con ridondanza geografica.

### <a name="tolerance-for-management-overhead"></a>Tolleranza per il sovraccarico di gestione

Ogni account di archiviazione richiede tempo e attenzione da parte di un amministratore per la creazione e la manutenzione. Ciò aumenta anche la complessità per gli utenti che aggiungono i dati nell'archiviazione cloud; tutti gli utenti in questo ruolo devono capire lo scopo di ogni account di archiviazione in modo da aggiungere nuovi servizi all’account corretto.

## <a name="summary"></a>Riepilogo

Gli account di archiviazione sono un potente strumento per ottenere le prestazioni e sicurezza necessari, riducendo al minimo i costi. Una strategia tipica è quella di iniziare con un'analisi dei dati e creare partizioni che condividono caratteristiche come posizione, fatturazione, strategia di replica, ecc. e quindi creare un account di archiviazione per ogni partizione.