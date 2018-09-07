Per impostazione predefinita, Istanze di contenitore di Azure è senza stato. Se il contenitore si blocca o si arresta, lo stato viene perso. Per rendere persistente lo stato oltre la durata del contenitore, è necessario montare un volume da un archivio esterno.

In questa unità, una condivisione file di Azure verrà montata su un'Istanza di contenitore di Azure per il recupero e l'archiviazione dei dati.

## <a name="create-an-azure-file-share"></a>Creare una condivisione file di Azure

Prima di usare una condivisione file di Azure con Istanze di contenitore di Azure è necessario creare la condivisione. Eseguire lo script seguente per creare un account di archiviazione. Il nome dell'account di archiviazione deve essere globalmente univoco, quindi lo script aggiunge un valore casuale alla stringa di base.

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --location eastus --sku Standard_LRS
```

Eseguire il comando seguente per inserire la stringa di connessione dell'account di archiviazione nella variabile di ambiente *AZURE_STORAGE_CONNECTION_STRING*. Questa variabile di ambiente viene riconosciuta dall'interfaccia della riga di comando di Azure e può essere usata in operazioni correlate all'archiviazione.

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`
```

Creare la condivisione file mediante il comando `az storage share create`. L'esempio seguente crea una condivisione denominata *aci-share-demo*.

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a>Ottenere le credenziali di archiviazione

Per montare una condivisione file di Azure come volume in Istanze di contenitore di Azure sono necessari tre valori: il nome dell'account di archiviazione, il nome della condivisione e la chiave di accesso alle risorse di archiviazione.

Se si usa lo script precedente, il nome dell'account di archiviazione viene creato con un valore casuale alla fine. Per eseguire una query sulla stringa finale (inclusa la parte casuale), usare i comandi seguenti:

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

Il nome della condivisione è già noto (aci-share-demo), quindi resta da trovare solo la chiave dell'account di archiviazione, che può essere recuperata tramite il comando seguente:

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Distribuire contenitori e montare volumi

Per montare una condivisione file di Azure come volume in un contenitore, specificare la condivisione e il punto di montaggio del volume quando si crea il contenitore.

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

Dopo aver creato il contenitore, ottenere l'indirizzo IP pubblico.

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-files --query ipAddress.ip -o tsv
```

Aprire un browser e passare all'indirizzo IP del contenitore. Verrà visualizzato un modulo semplice. Immettere il testo e fare clic su **Invia**. Questa azione consente di creare un file nella condivisione file di Azure. Il corpo del file sarà il testo immesso.

![Demo della condivisione file di Istanze di contenitore di Azure.](../media-draft/files-ui.png)

Per convalidare, è possibile passare alla condivisione file nel portale di Azure e scaricare il file.

![Esempio di file di testo con applicazione della demo contenuto.](../media-draft/sample-text.png)

Se i file e i dati archiviati nella condivisione file di Azure avevano un valore qualsiasi, tale condivisione potrebbe rimontata in una nuova istanza di contenitore per fornire i dati con stati.


## <a name="summary"></a>Riepilogo

In questa unità sono stati creati una condivisione file di Azure e un contenitore e la condivisione file è stata montata in tale contenitore. La condivisione è stata quindi usata per archiviare i dati dell'applicazione.

Nell'unità successiva verranno trattate alcune operazioni comuni di risoluzione dei problemi delle istanze di contenitore.

