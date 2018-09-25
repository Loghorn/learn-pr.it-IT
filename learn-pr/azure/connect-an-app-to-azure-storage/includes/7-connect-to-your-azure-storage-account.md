Sono state aggiunte le librerie client necessarie all'applicazione ed è ora possibile connettersi all'account di archiviazione di Azure.

Per lavorare con i dati in un account di archiviazione, l'app richiede due dati:

1. Una chiave di accesso
1. L'endpoint dell'API REST

## <a name="security-access-keys"></a>Chiavi di accesso di sicurezza

Ogni account di archiviazione ha due _chiavi di accesso_ univoche, usate a scopi di protezione. Se l'app deve connettersi a più account di archiviazione, dovrà avere chiavi di accesso per ogni account.

![Illustrazione che mostra un'applicazione connessa a due diversi account di archiviazione nel cloud. Ogni account di archiviazione è accessibile con una chiave univoca.](..\media\6-multiple-accounts.png)

## <a name="rest-api-endpoint"></a>Endpoint API REST

Oltre alle chiavi di accesso per l'autenticazione negli account di archiviazione, l'app dovrà conoscere gli endpoint del servizio di archiviazione per inviare le richieste REST. 

L'endpoint REST è una combinazione di _nome_ dell'account di archiviazione, tipo di dati e un dominio noto. Ad esempio:

| Tipo di dati | Endpoint di esempio |
|-----------|------------------|
| BLOB     | `https://[name].blob.core.windows.net/` |
| Code    | `https://[name].queue.core.windows.net/` |
| Tabella     | `https://[name].table.core.windows.net/` |
| File     | `https://[name].file.core.windows.net/` |

Se si ha un dominio personalizzato associato ad Azure, è anche possibile creare un URL di dominio personalizzato per l'endpoint.

## <a name="connection-strings"></a>Stringhe di connessione

Il modo più semplice per gestire le chiavi di accesso e gli URL dell'endpoint all'interno delle applicazioni è usare le **stringhe di connessione dell'account di archiviazione**. Una stringa di connessione contiene tutte le informazioni necessarie sulla connettività in un'unica stringa di testo.

Le stringhe di connessione di Archiviazione di Azure sono simili a quelle illustrate nell'esempio seguente, ma con la chiave di accesso e il nome account di archiviazione specifico in uso:

```
DefaultEndpointsProtocol=https;AccountName={your-storage};
   AccountKey={your-access-key};
   EndpointSuffix=core.windows.net
```

## <a name="security"></a>Sicurezza

Le chiavi di accesso sono fondamentali per permettere l'accesso all'account di archiviazione e di conseguenza non devono essere fornite ad alcun sistema o utente che non deve avere accesso all'account di archiviazione. Le chiavi di accesso sono l'equivalente di un nome utente e una password per accedere al computer.

In genere, le informazioni di connettività dell'account di archiviazione sono archiviate all'interno di una variabile di ambiente, un database o un file di configurazione.

> [!IMPORTANT]
> È importante notare che l'archiviazione di queste informazioni in un file di configurazione può essere pericolosa se il file viene incluso nel controllo del codice sorgente e salvato in un archivio pubblico. Questo è un errore comune e fa sì che chiunque possa esplorare il codice sorgente nell'archivio pubblico e prendere visione delle informazioni di connessione dell'account di archiviazione.

Ogni account di archiviazione include due chiavi di accesso. Il motivo di questo approccio è consentire la rotazione (rigenerazione) periodica delle chiavi nell'ambito delle procedure consigliate per la sicurezza per proteggere l'account di archiviazione. Questo processo può essere eseguito dal portale di Azure o dall'interfaccia della riga di comando di Azure o dalla riga di comando di PowerShell.

La rotazione di una chiave annulla immediatamente la validità del valore della chiave originale e revoca l'accesso a chiunque abbia ottenuto la chiave in modo inappropriato. Con il supporto per due chiavi, è possibile ruotare le chiavi senza causare tempo di inattività nelle applicazioni che le usano. L'app può passare a usare la chiave di accesso alternativa mentre viene rigenerata l'altra chiave. Se più app usano questo account di archiviazione, devono usare tutte la stessa chiave per poter supportare questa tecnica. Ecco l'idea di base:

1. Aggiornare le stringhe di connessione nel codice dell'applicazione in modo che facciano riferimento alla chiave di accesso secondaria dell'account di archiviazione.
2. Rigenerare la chiave di accesso primaria per l'account di archiviazione tramite lo strumento da riga di comando o il portale di Azure.
3. Aggiornare le stringhe di connessione nel codice in modo che facciano riferimento alla nuova chiave di accesso primaria.
4. Rigenerare la chiave di accesso secondaria nello stesso modo.

> [!TIP]
> Come si fa per le password, è consigliabile ruotare periodicamente le chiavi di accesso per garantire che rimangano private. Se la chiave di accesso viene usata in un'applicazione server, è possibile usare un'istanza di **Azure Key Vault** per archiviarla. Azure Key Vault include il supporto per la sincronizzazione diretta con l'account di archiviazione e la rotazione automatica periodica delle chiavi. L'uso di Key Vault offre un altro livello di sicurezza. L'app non deve infatti mai usare direttamente una chiave di accesso.

### <a name="shared-access-signatures-sas"></a>Firme di accesso condiviso (SAS)

Le chiavi di accesso sono l'approccio più semplice per autenticare l'accesso a un account di archiviazione. Offrono però totale accesso a qualsiasi elemento nell'account di archiviazione, come una password radice in un computer.

Gli account di archiviazione offrono un meccanismo di autenticazione separato denominato _firme di accesso condiviso_ che supporta la scadenza e autorizzazioni limitate per scenari in cui è necessario concedere accesso limitato. È consigliabile usare questo approccio quando si consente ad altri utenti di leggere e scrivere dati nell'account di archiviazione. Al termine del modulo sono disponibili collegamenti alla documentazione su questo argomento avanzato.
