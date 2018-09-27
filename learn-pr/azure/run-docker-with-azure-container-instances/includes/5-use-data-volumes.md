Per impostazione predefinita, Istanze di contenitore di Azure è senza stato. Se il contenitore si blocca o si arresta, lo stato viene perso. Per rendere persistente lo stato oltre la durata del contenitore, è necessario montare un volume da un archivio esterno.

In questo caso, si monterà una condivisione file di Azure in un'istanza di contenitore di Azure per l'archiviazione e il recupero dei dati.

## <a name="create-an-azure-file-share"></a>Creare una condivisione file di Azure

Eseguire lo script seguente per creare un account di archiviazione. Il nome dell'account di archiviazione deve essere globalmente univoco, quindi lo script aggiunge un valore casuale alla stringa di base:

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --sku Standard_LRS \
    --location eastus
```

Eseguire il comando seguente per inserire la stringa di connessione dell'account di archiviazione nella variabile di ambiente *AZURE_STORAGE_CONNECTION_STRING*. Questa variabile di ambiente viene riconosciuta dall'interfaccia della riga di comando di Azure e può essere usata in operazioni correlate all'archiviazione:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string --resource-group <rgn>[sandbox resource group name]</rgn> --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv)
```

Creare una condivisione file nell'account di archiviazione eseguendo il comando `az storage share create`. L'esempio seguente crea una condivisione denominata *aci-share-demo*:

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a>Ottenere le credenziali di archiviazione

Per montare una condivisione file di Azure come volume in Istanze di contenitore di Azure, sono necessari tre valori: il nome dell'account di archiviazione, il nome della condivisione e la chiave di accesso all'account di archiviazione.

Se si usa lo script precedente, il nome dell'account di archiviazione viene creato con un valore casuale alla fine. Per eseguire una query sulla stringa finale (inclusa la parte casuale), usare i comandi seguenti:

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group <rgn>[sandbox resource group name]</rgn> --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

Il nome della condivisione è già noto (aci-share-demo), quindi non resta che la chiave dell'account di archiviazione, che può essere trovata usando il comando seguente:

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group <rgn>[sandbox resource group name]</rgn> --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Distribuire contenitori e montare volumi

Per montare una condivisione file di Azure come volume in un contenitore, specificare il punto di montaggio della condivisione e del volume quando si crea il contenitore:

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --location eastus \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

Dopo aver creato il contenitore, ottenere l'indirizzo IP pubblico:

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --query ipAddress.ip \
    --output tsv
```

Aprire un browser e passare all'indirizzo IP del contenitore. Verrà visualizzato un modulo semplice. Immettere il testo e fare clic su **Invia**. Questa azione consente di creare un file nella condivisione di File di Azure che contiene il testo immesso.

![Demo della condivisione file di Istanze di contenitore di Azure](../media/5-files-ui.png)

Per convalidare, è possibile passare alla condivisione file nel portale di Azure e scaricare il file.

1. Ottenere il nome del file

    ```azurecli
    az storage file list -s aci-share-demo -o table
    ```

1. Scaricarlo e assicurarsi di sostituire l'elemento `<filename>` qui sotto.

    ```azurecli
    az storage file download -s aci-share-demo -n <filename>
    ```
    
![Esempio di file di testo con l'applicazione demo Contents](../media/5-sample-text.png)

Se i file e i dati archiviati nella condivisione di File di Azure avevano un valore qualsiasi, tale condivisione potrebbe essere rimontata in una nuova istanza di contenitore per fornire i dati con stato.