Ora che è disponibile un'app, è necessario un account di archiviazione di Azure.

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a>Usare l'interfaccia della riga di comando di Azure per creare un account di archiviazione di Azure

Si userà il comando `az storage account create` per creare un nuovo account di archiviazione. Per controllare la configurazione dell'account di archiviazione, sono disponibili diversi parametri.

> [!div class="mx-tableFixed"]
> | Opzione | Descrizione |
> |--------|-------------|
> | `--name` | **Nome dell'account di archiviazione**. Il nome verrà usato per generare l'URL pubblico necessario per accedere ai dati nell'account. Deve essere univoco in tutti i nomi di account di archiviazione esistenti in Azure. Deve avere una lunghezza compresa tra 3 e 24 caratteri e può contenere solo caratteri minuscoli e numeri. |
> | `--resource-group` | Usare **<rgn>[nome gruppo di risorse sandbox]</rgn>** per inserire l'account di archiviazione nell'ambiente sandbox gratuito. |
> | `--location` | Selezionare una località nelle vicinanze (vedere più avanti). |
> | `--kind` | Questo determina il _tipo_ di account di archiviazione. Le opzioni sono `BlobStorage`, `Storage` e `StorageV2`. |
> | `--sku` | Questo determina il modello di replica e le prestazioni dell'account di archiviazione. Le opzioni sono `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS` e `Standard_ZRS`. |
> | `--access-tier` | Il **livello di accesso** viene usato solo per l'archiviazione BLOB. Le opzioni disponibili sono [`Cool` \| `Hot`]. Il **livello di accesso frequente** è ideale per i dati a cui si accede spesso e il **livello di accesso sporadico** è più efficiente per i dati a cui si accede di rado. Tenere presente che in questo modo viene impostato solo il valore _predefinito_. Quando si crea un BLOB, è possibile impostare un valore diverso per i dati. |
    
Usare la tabella precedente per comporre, sul lato destro di Cloud Shell, una riga di comando che consenta di creare l'account.
- Usare un nome univoco. È consigliabile un nome simile a "photostore" con le proprie iniziali e un numero casuale. Se il nome non è univoco, verrà visualizzato un errore.
- In genere, si crea un nuovo gruppo di risorse in cui inserire le risorse dell'app. In questo caso, tuttavia, usare il gruppo di risorse dell'ambiente sandbox "**<rgn>[nome gruppo di risorse sandbox]</rgn>**".
- Per **sku** usare "Standard_LRS". In questo modo verrà usata l'archiviazione standard con replica locale, idonea per questo esempio.
- Per il **livello di accesso** usare "Cool".

### <a name="selecting-a-location"></a>Selezione di una località
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a>Comando di esempio

```azurecli
az storage account create \
        --name <name> \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \
        --access-tier Cool
```

> [!TIP]
> Per altre informazioni sulle opzioni relative all'account di archiviazione, vedere il modulo **Creare un account di archiviazione di Azure**, in cui tali opzioni vengono esaminate in dettaglio.

La distribuzione dell'account richiederà qualche minuto. Mentre Azure esegue questa operazione, è possibile esplorare le API che verranno usate con questo account.
