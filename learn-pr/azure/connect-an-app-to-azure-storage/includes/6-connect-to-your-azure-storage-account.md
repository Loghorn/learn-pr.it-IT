Sono state aggiunte le librerie client necessarie all'applicazione ed è ora possibile connettersi all'account di archiviazione di Azure. Tuttavia, l'applicazione è in grado di riconoscere l'account a cui connettersi e l'area in cui si trova l'account di archiviazione? Senza la configurazione appropriata, l'app non saprà come connettersi all'account di archiviazione di Azure. 

In questo esercizio si individueranno le informazioni di connessione all'account di archiviazione nel portale di Azure, che verranno quindi aggiunte nella configurazione dell'applicazione, in modo che l'app sia pronta per connettersi all'account di archiviazione di Azure.

## <a name="access-keys"></a>Chiavi di accesso

Ogni account di archiviazione include un set di chiavi di accesso che permettono l'accesso all'account di archiviazione stesso. Se l'app deve connettersi a più account di archiviazione, dovrà avere chiavi di accesso per ogni account.

![Più account](..\media-draft\6-multiple-accounts.png)

Poiché Azure è un provider di servizi cloud, oltre alle chiavi di accesso per l'autenticazione negli account di archiviazione, l'app dovrà identificare gli endpoint del servizio di archiviazione che forniscono l'accesso tramite Internet all'account di archiviazione. Il metodo più semplice per gestire queste informazioni consiste nell'usare la stringa di connessione dell'account di archiviazione, che contiene tutte le informazioni di connettività necessarie in un'unica stringa.

Le stringhe di connessione di Archiviazione di Azure hanno un aspetto simile a quelle mostrate nell'esempio seguente, ma con la chiave di accesso e il nome dell'account di archiviazione specifico in uso:

```csharp
DefaultEndpointsProtocol=https;AccountName={your-storage};AccountKey={your-access-key};EndpointSuffix=core.windows.net
```

## <a name="security"></a>Sicurezza

Le chiavi di accesso sono fondamentali per permettere l'accesso all'account di archiviazione e di conseguenza non devono essere fornite ad alcun sistema o utente che non deve avere accesso all'account di archiviazione. Le chiavi di accesso sono l'equivalente di un nome utente e una password per accedere al computer.

In genere, le informazioni di connettività dell'account di archiviazione sono archiviate all'interno di una variabile di ambiente, un database o un file di configurazione.

È importante notare che l'archiviazione di queste informazioni in un file di configurazione può essere pericolosa se il file viene incluso nel controllo del codice sorgente e salvato in un archivio pubblico. Questo è un errore comune e fa sì che chiunque possa esplorare il codice sorgente nell'archivio pubblico e prendere visione delle informazioni di connessione dell'account di archiviazione.

Gli account di archiviazione offrono un meccanismo di autenticazione separato denominato firme di accesso condiviso che supporta la scadenza e autorizzazioni limitate per scenari in cui è necessario concedere accesso con restrizioni. Questo argomento non verrà trattato qui e non è necessario per questo modulo, ma altre informazioni al riguardo sono disponibili [qui](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

Ogni account di archiviazione include due chiavi di accesso. Il motivo di questo approccio è consentire la rotazione (rigenerazione) periodica delle chiavi nell'ambito delle procedure consigliate per la sicurezza per proteggere l'account di archiviazione. Questo processo può essere eseguito dal portale di Azure o dall'interfaccia della riga di comando di Azure.

La rotazione di una chiave annulla immediatamente la validità del valore della chiave originale e revoca l'accesso a chiunque abbia ottenuto la chiave in modo inappropriato. Con il supporto per due chiavi, è possibile ruotare le chiavi senza causare tempo di inattività nelle applicazioni che le usano. L'app può passare a usare la chiave di accesso alternativa mentre viene rigenerata l'altra chiave. Se più app usano questo account di archiviazione, devono usare tutte la stessa chiave per poter supportare questa tecnica. Per altre informazioni, vedere [qui](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#manage-your-storage-access-keys).

## <a name="advanced-security-options"></a>Opzioni di sicurezza avanzata

Azure offre un archivio di sicurezza avanzato denominato Azure Key Vault, progettato per archiviare in modo sicuro qualsiasi forma di segreto, come chiavi di accesso, password o certificati. Usando questa funzionalità, è possibile archiviare in modo sicuro le chiavi di accesso e abilitarne la rotazione automatica.

Questa funzionalità esula dall'ambito di questo modulo, ma altre informazioni al riguardo sono disponibili [qui](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).

## <a name="summary"></a>Riepilogo

Per connettersi a un account di archiviazione di Azure, sono necessari due elementi. Una chiave di accesso, simile a un nome utente e una password, che deve essere mantenuta riservata. Un endpoint servizio di archiviazione, rappresentato da una stringa di connessione.
