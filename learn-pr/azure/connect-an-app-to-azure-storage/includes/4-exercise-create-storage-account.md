Ora che abbiamo un'app, è necessario un account di archiviazione di Azure per lavorare con. Abbiamo creato uno usando il portale di Azure nel **creare un account di archiviazione di Azure** modulo. Utilizziamo la CLI di Azure in questo momento.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a>Usare il comando di Azure per creare un account di archiviazione di Azure

Si userà il `az storage account create` comando per creare un nuovo account di archiviazione. Accetta diversi parametri che è necessario fornire (o necessario) per configurarlo nel modo desiderato.

> [!div class="mx-tableFixed"]
> | Opzione | Descrizione |
> |--------|-------------|
> | `--name` | Oggetto **nome account di archiviazione**. Il nome verrà usato per generare l'URL pubblico usato per accedere ai dati nell'account. Deve essere univoco in tutti i nomi account di archiviazione esistenti in Azure. Deve essere compreso tra 3 e 24 caratteri e può contenere solo lettere minuscole e numeri. |
> | `--resource-group` | Uso <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn> inserisca l'account di archiviazione nel sandbox gratuito. |
> | `--location` | Selezionare una località vicino a te (vedere sotto). |
> | `--kind` | Questo parametro determina l'account di archiviazione _tipo_. Le opzioni includono `BlobStorage`, `Storage`, e `StorageV2`. |
> | `--sku` | Si decide di modello delle prestazioni e la replica di account di archiviazione. Le opzioni includono `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, e `Standard_ZRS`. |
> | `--access-tier` | Il **livello di accesso** viene utilizzato solo per l'archiviazione Blob, le opzioni disponibili sono [`Cool` | `Hot`]. Il **livello di accesso frequente** ideale per i dati utilizzati di frequente e il **livello di accesso sporadico** è preferibile per i dati ad accesso sporadico. Si noti che viene impostata solo la _predefinite_ valore&mdash;quando si crea un Blob, è possibile impostare un valore diverso per i dati. |
    
Usare la tabella sopra riportata per elaborare una riga di comando in Cloud Shell a destra per creare l'account.
- Usare un nome univoco. È consigliabile simile a "photostore" con le proprie iniziali e un numero casuale. Si otterrà un errore se non è univoco.
- In genere, si creerà un nuovo gruppo di risorse per contenere le risorse dell'app, ma in questo caso, usare il gruppo di risorse di Sandbox.
- Usare "Standard_LRS" per il **sku**. Si userà archiviazione standard con la replica locale, adatta per questo esempio.
- Usare "A freddo" per il **livello di accesso**.

### <a name="selecting-a-location"></a>Selezione di un percorso
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a>Comando di esempio

```azurecli
az storage account create \
        --name <name> \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \ 
        --access-tier Cool
```

> [!TIP]
> Se si è interessati a esplorare le opzioni per l'account di archiviazione, assicurarsi di passare attraverso il **creare un account di archiviazione di Azure** in cui andiamo a ognuna di esse in modo approfondito.

Richiederà alcuni minuti per distribuire l'account. Mentre Azure funziona su esso, è possibile esplorare le API verrà usato con questo account.