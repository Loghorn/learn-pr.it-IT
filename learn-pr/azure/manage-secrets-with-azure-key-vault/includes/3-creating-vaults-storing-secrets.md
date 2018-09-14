## <a name="creating-key-vaults-for-your-applications"></a>Creazione di insiemi di credenziali delle chiavi per le applicazioni

È consigliabile creare un insieme di credenziali separato per ogni ambiente di distribuzione, ad esempio sviluppo, test e produzione, in ogni applicazione. È possibile usare gli insiemi di credenziali per condividere segreti tra le app. Tuttavia, se un utente malintenzionato ottiene l'accesso in lettura a un insieme di credenziali, l'impatto è proporzionale al numero di segreti nell'insieme di credenziali.

> [!TIP]
> Se si usano gli stessi nomi per i segreti nei diversi ambienti di un'applicazione, l'unica configurazione specifica dell'ambiente da modificare nell'app è l'URL dell'insieme di credenziali.

Per creare un insieme di credenziali non è necessaria alcuna configurazione iniziale. All'identità utente viene automaticamente concesso il set completo di autorizzazioni per la gestione dei segreti e si può iniziare ad aggiungere segreti immediatamente. Quando si ha un insieme di credenziali, l'aggiunta e la gestione dei segreti possono essere eseguite da qualsiasi interfaccia amministrativa di Azure, come il portale di Azure, l'interfaccia della riga di comando di Azure e Azure PowerShell. Quando si configura l'applicazione per l'uso dell'insieme di credenziali, è necessario assegnare le autorizzazioni corrette. Questo verrà illustrato nell'unità successiva.

## <a name="vault-authentication-and-permissions"></a>Autenticazione e autorizzazioni per gli insiemi di credenziali

Per l'autenticazione degli utenti e delle applicazioni, l'API di Azure Key Vault usa Azure Active Directory. I criteri di accesso agli insiemi di credenziali sono basati su *azioni* e vengono applicati in un intero insieme di credenziali. Ad esempio, un'applicazione con autorizzazioni **Get** (per leggere i valori dei segreti), **List** (per elencare i nomi di tutti i segreti) e **Set** (per creare o aggiornare i valori dei segreti) per un insieme di credenziali può creare segreti, elencare i nomi di tutti i segreti e recuperarne e impostarne i valori in tale insieme di credenziali.

Per *tutte* le azioni eseguite su un insieme di credenziali sono necessarie l'autenticazione e l'autorizzazione. Non è possibile concedere alcun tipo di accesso anonimo.

> [!TIP]
> Quando si concede l'accesso all'insieme di credenziali a sviluppatori e app, concedere solo il set di autorizzazioni minimo necessario. Le limitazioni delle autorizzazioni consentono di evitare gli incidenti causati da bug nel codice e di ridurre l'impatto del furto di credenziali o dell'inserimento di codice dannoso nell'app.

Per un insieme di credenziali dell'ambiente di sviluppo, per gli sviluppatori sono in genere sufficienti le autorizzazioni **Get** e **List**. Un responsabile o uno sviluppatore senior dovrà avere autorizzazioni complete per l'insieme di credenziali per modificare e aggiungere i segreti quando necessario. Le autorizzazioni complete per gli insiemi di credenziali dell'ambiente di produzione sono in genere riservate al personale operativo senior.

Per le app sono in genere sufficienti autorizzazioni **Get**. A seconda delle modalità di implementazioni delle app, per alcune potrebbero essere necessarie autorizzazioni **List**. A causa della tecnica usata per leggere i segreti dall'insieme di credenziali, l'app che verrà implementata nell'esercizio di questo modulo richiede l'autorizzazione **List**.

## <a name="exercise"></a>Esercizio

A causa di tutti i problemi verificatisi in azienda con i segreti delle applicazioni, i dirigenti hanno chiesto di creare una piccola app di base per indicare agli altri sviluppatori il percorso corretto. L'app deve illustrare le procedure consigliate per gestire i segreti nel modo più semplice e sicuro possibile.

Per iniziare, si creerà un insieme di credenziali e si archivierà un segreto.

### <a name="create-a-resource-group"></a>Creare un gruppo di risorse
<!---TODO: Update for sandbox?--->

Creare un gruppo di risorse denominato `keyvault-exercise-group` per tutte le risorse di questa unità. Alla fine di questo modulo si eliminerà questo gruppo di risorse per eseguire la pulizia di tutte le risorse contemporaneamente. Come località si userà `eastus` per tutti gli elementi dell'unità.

Usare il terminale Azure Cloud Shell a destra per eseguire questo comando dell'interfaccia della riga di comando di Azure. Verrà così creato il gruppo di risorse nella sottoscrizione.

```azurecli
az group create --name keyvault-exercise-group --location eastus
```

### <a name="create-the-vault-and-store-the-secret-in-it"></a>Creare l'insieme di credenziali e archiviare il segreto

Si creerà quindi l'insieme di credenziali e vi si archivierà il segreto.

**I nomi degli insiemi di credenziali delle chiavi devono essere univoci a livello globale. Sarà quindi necessario scegliere un nome univoco**. I nomi degli insiemi di credenziali devono avere una lunghezza compresa tra 3 e 24 caratteri e contenere solo caratteri alfanumerici e trattini.

```azurecli
az keyvault create --name <your-unique-vault-name> --resource-group keyvault-exercise-group --location eastus
```

Al termine verrà visualizzato l'output JSON che descrive il nuovo insieme di credenziali.

Aggiungere quindi un segreto denominato **SecretPassword** con il valore **reindeer_flotilla**.

```azurecli
az keyvault secret set --name SecretPassword --value reindeer_flotilla --vault-name <your-unique-vault-name>
```

Prendere nota del nome dell'insieme di credenziali, perché sarà presto nuovamente necessario.

A breve si scriverà il codice per l'applicazione, ma è prima necessario apprendere come l'app eseguirà l'autenticazione a un insieme di credenziali.