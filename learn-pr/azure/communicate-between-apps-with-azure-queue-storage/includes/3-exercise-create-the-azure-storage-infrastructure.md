Si è notato che i picchi di traffico possono sovraccaricare il livello intermedio. Per risolvere questo problema, si è deciso di aggiungere una coda tra il front-end e il livello intermedio nell'applicazione di caricamento degli articoli.

Il primo passaggio della procedura di creazione di una coda è creare l'account di archiviazione di Azure in cui verranno archiviati i dati.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-storage-account-with-the-azure-cli"></a>Creare un account di archiviazione tramite l'interfaccia della riga di comando di Azure

> [!TIP] 
> In genere, si potrebbe avviare un nuovo progetto creando un _gruppo di risorse_ in cui includere tutte le risorse associate. In questo caso, verrà usato l'ambiente sandbox di Azure, che fornisce un gruppo di risorse denominato <rgn>[Nome gruppo di risorse sandbox]</rgn>.

Usare il comando `az storage account create` per creare l'account di archiviazione. È possibile digitare il comando nella finestra di Cloud Shell a destra.

Il comando richiede diversi parametri:

| Parametro | Valore |
|-----------|-------|
| `--name`  | Imposta il nome. Tenere presente che gli account di archiviazione usano il nome per generare un URL pubblico, quindi deve essere univoco. Inoltre, il nome dell'account deve avere una lunghezza compresa tra 3 e 24 caratteri e può contenere solo numeri e lettere minuscole. È consigliabile usare il prefisso **articles** con un numero casuale come suffisso, ma anche è possibile scegliere il nome desiderato. |
| `-g`        | Imposta il **gruppo di risorse**. Usare _<rgn>[Nome gruppo di risorse sandbox]</rgn>_ come valore. |
| `--kind`    | Imposta il _tipo di account di archiviazione_. Usare **StorageV2** per creare un account V2 per utilizzo generico. |
| `--sku`     | Imposta il **tipo di replica e archiviazione**, per impostazione predefinita _Standard_RAGRS_. Usare _Standard_LRS_, che indica la ridondanza solo a livello locale all'interno del data center. |
| `-l`        | Imposta la **località** indipendentemente dal proprietario del gruppo di risorse. È facoltativo, ma può essere usato per collocare la coda in un'area diversa da quella del gruppo di risorse. Scegliere una località vicina tra quelle presenti nell'elenco seguente di aree disponibili nell'ambiente sandbox. |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Ecco una riga di comando di esempio che usa i parametri indicati sopra. Assicurarsi di modificare il parametro `--name`.

```azurecli
az storage account create --name [unique-name] -g <rgn>[Sandbox resource group name]</rgn> --kind StorageV2 --sku Standard_LRS
```

<!-- Paste tip-->
[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
