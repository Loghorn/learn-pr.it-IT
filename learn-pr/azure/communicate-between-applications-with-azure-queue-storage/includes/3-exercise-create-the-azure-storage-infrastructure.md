Si è notato che i picchi di traffico possono sovraccaricare il livello intermedio. Per risolvere questo problema, si è deciso di aggiungere una coda tra il front-end e il livello intermedio nell'applicazione di caricamento degli articoli.

Il primo passaggio della procedura di creazione di una coda è creare l'account di archiviazione di Azure in cui verranno archiviati i dati.

## <a name="create-a-storage-account-with-the-azure-cli"></a>Creare un account di archiviazione usando l'interfaccia della riga di comando di Azure

Per iniziare, creare un gruppo di risorse di Azure in cui inserire l'account di archiviazione.

1. In Cloud Shell a destra selezionare Bash se è possibile scegliere un'opzione.

1. Usare il comando `az group create` dell'interfaccia della riga di comando di Azure per creare un nuovo gruppo di risorse. Assegnare al gruppo il nome **ExerciseResources** e posizionarlo in una località vicina. 
    - L'esempio seguente usa "eastus" come località.

    ```azurecli
    az group create -n ExerciseResources --location eastus
    ```
        
1. Ora usare il comando `az storage account create` per creare l'account di archiviazione effettivo. È necessario specificare alcuni parametri:

| Parametro | Valore |
|-----------|-------|
| `--name`  | Imposta il nome. Tenere presente che gli account di archiviazione usano il nome per generare un URL pubblico, quindi deve essere univoco. Inoltre, il nome dell'account deve avere una lunghezza compresa tra 3 e 24 caratteri e può contenere solo numeri e lettere minuscole. È consigliabile usare il prefisso **articles** con un numero casuale come suffisso ma è possibile usare ciò che si vuole. |
| `-g`        | Specifica il gruppo di risorse, usare **ExerciseResources** come valore. |
| `--kind`    | Imposta il tipo di account di archiviazione, usare **StorageV2** per creare un account V2 per utilizzo generico. |
| `-l`        | Imposta la posizione in modo indipendente dal proprietario del gruppo di risorse. È facoltativo, ma è possibile usarlo per posizionare la coda in un'area diversa rispetto al gruppo di risorse. |
| `--sku`     | Per impostazione predefinita il tipo di account è **Standard_RAGRS**, che però è eccessivo per questa dimostrazione. Verrà quindi usato **Standard_LRS**, che è ridondante solo a livello locale all'interno del data center. |

Di seguito è riportato un esempio di riga di comando che usa i parametri indicati sopra. Assicurarsi di modificare il parametro `--name` in modo che sia univoco se si decide di copiare e incollare questo comando.

```azurecli
az storage account create --name <name> -g ExerciseResources --kind StorageV2 -l eastus --sku Standard_LRS
```