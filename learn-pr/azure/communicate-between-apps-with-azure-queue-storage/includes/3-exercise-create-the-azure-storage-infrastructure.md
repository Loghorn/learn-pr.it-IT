Si è notato che i picchi di traffico possono sovraccaricare il livello intermedio. Per risolvere questo problema, si è deciso di aggiungere una coda tra il front-end e il livello intermedio nell'applicazione di caricamento degli articoli.

Il primo passaggio della procedura di creazione di una coda è creare l'account di archiviazione di Azure in cui verranno archiviati i dati.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-storage-account-with-the-azure-cli"></a>Creare un account di archiviazione usando l'interfaccia della riga di comando di Azure

> [!TIP] 
> In genere, si potrebbe avviare un nuovo progetto mediante la creazione di un _gruppo di risorse_ per contenere tutte le risorse associate. In questo caso, verrà usato l'ambiente Sandbox di Azure che fornisce un gruppo di risorse denominato <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>.

1. In Cloud Shell a destra selezionare Bash se è possibile scegliere un'opzione.

1. Usare il `az storage account create` comando per creare l'account di archiviazione. È necessario specificare alcuni parametri:

| Parametro | Valore |
|-----------|-------|
| `--name`  | Imposta il nome. Tenere presente che gli account di archiviazione usano il nome per generare un URL pubblico, quindi deve essere univoco. Inoltre, il nome dell'account deve avere una lunghezza compresa tra 3 e 24 caratteri e può contenere solo numeri e lettere minuscole. È consigliabile usare il prefisso **articles** con un numero casuale come suffisso ma è possibile usare ciò che si vuole. |
| `-g`        | Fornisce il **gruppo di risorse**, usare <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn> come valore. |
| `--kind`    | Imposta il **tipo di Account di archiviazione** -utilizzare _archiviazione v2_ per creare un account per utilizzo generico V2. |
| `-l`        | Imposta il **posizione** indipendenti del proprietario del gruppo di risorse. È facoltativo, ma è possibile usarlo per posizionare la coda in un'area diversa rispetto al gruppo di risorse. |
| `--sku`     | Imposta il **tipo di replica e archiviazione**, per impostazione predefinita _Standard_RAGRS_. Verrà quindi usato _Standard_LRS_, che è ridondante solo a livello locale all'interno del data center. |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Di seguito è riportato un esempio di riga di comando che usa i parametri indicati sopra. Assicurarsi di modificare la `--name` e `--location` parametri se si decide di questo comando Copia/Incolla.

```azurecli
az storage account create --name [unique-name] -g <rgn>[Sandbox resource group name]</rgn> --kind StorageV2 -l [location-name] --sku Standard_LRS
```