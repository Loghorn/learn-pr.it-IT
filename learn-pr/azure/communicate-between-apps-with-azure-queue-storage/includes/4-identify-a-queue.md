Dopo aver creato l'account di archiviazione, sarà necessario esaminare come lavorare con la coda che conterrà.

Per accedere a una coda, sono necessarie tre informazioni:

 1. Nome dell'account di archiviazione
 2. Nome della coda
 3. Token di autorizzazione

Queste informazioni vengono usate da entrambe le applicazioni che comunicano con la coda (il front-end Web che aggiunge i messaggi e il livello intermedio che li elabora).

## <a name="queue-identity"></a>Identità della coda

A ogni coda è stato assegnato un nome durante la creazione. Il nome deve essere univoco all'interno dell'account di archiviazione ma non deve necessariamente essere globalmente univoco (a differenza del nome dell'account di archiviazione).

La combinazione di nome dell'account di archiviazione e nome della coda identifica in modo univoco una coda.

## <a name="access-authorization"></a>Autorizzazione all'accesso

Tutte le richieste a una coda devono essere autorizzate e sono disponibili diverse opzioni tra cui scegliere.

| Tipo di autorizzazione | Descrizione |
|--------------------|-------------|
| **Azure Active Directory** | È possibile usare l'autenticazione basata sui ruoli e identificare i client specifici in base alle credenziali AAD. |
| **Chiave condivisa** | Talvolta chiamata **chiave dell'account**, si tratta di una firma di chiave crittografata associata all'account di archiviazione. Ogni account di archiviazione dispone di due di queste chiavi che possono essere passate con ogni richiesta per autenticare l'accesso. Questo approccio è simile all'uso di una password radice: fornisce _l'accesso completo_ all'account di archiviazione. |
| **Firma di accesso condiviso** | Una firma di accesso condiviso (SAS) è un URI generato che concede ai client l'accesso limitato agli oggetti nell'account di archiviazione. È possibile limitare l'accesso a risorse e autorizzazioni specifiche ed esaminare un intervallo di dati per disattivare automaticamente l'accesso dopo un periodo di tempo.  |

> [!NOTE]
> L'autorizzazione tramite chiave dell'account viene usata perché è il modo più semplice per iniziare a lavorare con le code, tuttavia si consiglia di usare la firma di accesso condiviso (SAS) o Azure Active Directory (AAD) nelle app di produzione.

### <a name="retrieving-the-account-key"></a>Recupero della chiave dell'account
 
La chiave dell'account è disponibile nella sezione **Impostazioni > Chiavi di accesso** dell'account di archiviazione nel portale di Azure, oppure è possibile recuperarla tramite la riga di comando:

```azurecli
az storage account keys list ...
```

```powershell
Get-AzureRmStorageAccountKey ...
```

## <a name="accessing-queues"></a>Accesso alle code

È possibile accedere a una coda usando un'API REST. A tale scopo, si userà un URL che combina il nome assegnato all'account di archiviazione con il dominio `queue.core.windows.net` e il percorso della coda con cui si desidera lavorare. Ad esempio: `http://<storage account>.queue.core.windows.net/<queue name>`. È necessario includere un'intestazione `Authorization` con ogni richiesta. Il valore può essere uno dei tre stili di autorizzazione.

### <a name="using-the-azure-storage-client-library-for-net"></a>Uso della libreria client Archiviazione di Azure per .NET

La libreria client Archiviazione di Azure per .NET è una libreria fornita da Microsoft che formula le richieste REST e analizza le risposte REST per l'utente. Questo riduce notevolmente la quantità di codice che è necessario scrivere. L'accesso tramite la libreria client richiede le stesse informazioni (nome account di archiviazione, nome della coda e chiave dell'account); tuttavia, sono organizzate in modo diverso.

La libreria client usa una **stringa di connessione** per stabilire la connessione. La stringa di connessione è disponibile nella sezione **Impostazioni** dell'account di archiviazione nel portale di Azure, oppure tramite l'interfaccia della riga di comando di Azure e PowerShell.

Una stringa di connessione è una stringa che combina il nome dell'account di archiviazione e la chiave dell'account e deve essere nota all'applicazione per accedere all'account di archiviazione. Il formato è simile al seguente:

```csharp
string connectionString = "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net"
```

> [!WARNING]
> Questo valore della stringa deve essere archiviato in una posizione sicura, poiché chiunque abbia accesso a questa stringa di connessione sarà in grado di modificare la coda.

Si noti che la stringa di connessione non include il nome della coda. Il nome della coda viene fornito nel codice quando si stabilisce una connessione alla coda.

È necessario ottenere la stringa di connessione da Azure e configurare una nuova applicazione per usarla.