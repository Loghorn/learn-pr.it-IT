Ora che è disponibile un'app, è necessario un account di archiviazione di Azure. Nel modulo **Creare un account di archiviazione di Azure** ne è stato creato uno tramite il portale di Azure. Stavolta verrà usata l'interfaccia della riga di comando di Azure.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a>Usare l'interfaccia della riga di comando di Azure per creare un account di archiviazione di Azure

Si userà il comando `az storage account create` per creare un nuovo account di archiviazione. Il comando accetta vari parametri che è necessario fornire per configurare l'account nel modo desiderato.

> [!div class="mx-tableFixed"]
> | Opzione | Descrizione |
> |--------|-------------|
> | `--name` | **Nome dell'account di archiviazione**. Il nome verrà usato per generare l'URL pubblico necessario per accedere ai dati nell'account. Deve essere univoco in tutti i nomi di account di archiviazione esistenti in Azure. Deve avere una lunghezza compresa tra 3 e 24 caratteri e può contenere solo caratteri minuscoli e numeri. |
> | `--resource-group` | Usare <rgn>[nome del gruppo di risorse del sandbox]</rgn> per inserire l'account di archiviazione nel sandbox gratuito. |
> | `--location` | Selezionare una località nelle vicinanze. |
> | `--kind` | Questo determina il _tipo_ di account di archiviazione. Le opzioni includono BlobStorage, Storage e StorageV2. |
> | `--sku` | Questo determina il modello di replica e le prestazioni dell'account di archiviazione. Le opzioni includono Premium_LRS, Standard_GRS, Standard_LRS, Standard_RAGRS e Standard_ZRS. |
> | `--access-tier` | Il **livello di accesso** viene usato solo per l'archiviazione BLOB, le opzioni disponibili sono Sporadico (Cold) e Frequente (Hot). Il **livello di accesso frequente** è ideale per i dati a cui si accede spesso e il **livello di accesso sporadico** è migliore per i dati a cui si accede di rado. Tenere presente che in questo modo viene impostato solo il valore _predefinito_. Quando si crea un BLOB, è possibile impostare un valore diverso per i dati. |
    
Usare la tabella precedente per creare una riga di comando in Cloud Shell a destra per creare l'account.
- Usare un nome univoco, ad esempio simile a "photostore", con le proprie iniziali e un numero casuale. Se non è univoco, verrà visualizzato un errore.
- Normalmente si crea un nuovo gruppo di risorse in cui inserire le risorse dell'app. In questo caso viene usato il gruppo di risorse del sandbox.
- Inserire "Standard_LRS" per **sku** in modo da usare l'archiviazione standard con replica locale, idonea per questo esempio.
- Usare il **livello di accesso** sporadico (Cold).

### <a name="selecting-a-location"></a>Scelta di una località
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a>Comando di esempio

```bash
az storage account create \
        --name <name> \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \ 
        --access-tier Cold
```

> [!TIP]
> Per altre informazioni sulle opzioni relative all'account di archiviazione, vedere il modulo **Creare un account di archiviazione di Azure** in cui vengono esaminate in dettaglio.

La distribuzione dell'account richiederà qualche minuto. Mentre Azure esegue questa operazione, è possibile esplorare le API che verranno usate con questo account.