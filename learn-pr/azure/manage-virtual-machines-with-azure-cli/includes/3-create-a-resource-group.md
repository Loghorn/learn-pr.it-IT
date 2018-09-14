Il nostro obiettivo consiste nel creare una nuova macchina virtuale di Azure. È necessario fornire diversi tipi di informazioni per identificare il percorso della risorsa, il sistema operativo da usare e la configurazione hardware necessaria per la macchina virtuale. Iniziare con un **gruppo di risorse**.

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Azure usa _gruppi di risorse_ per raggruppare le risorse correlate, ad esempio macchine virtuali e database. Il gruppo di risorse identifica anche un percorso specifico (denominato "area") che deciderà quale data center di risorsa è inserito.

> [!NOTE]
> Sandbox di Azure fornisce un gruppo di risorse creato in precedenza denominato <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>. Non devi eseguire questi passaggi. Tuttavia, quando si crea il _proprio_ risorse per i progetti reali, questi saranno i comandi da eseguire. Sandbox di Azure non consente di creare gruppi di risorse direttamente.

Ad esempio, è possibile digitare il comando di Azure in Azure Cloud Shell creare un gruppo di risorse nel **Stati Uniti orientali** area. Sostituirà **[gruppo di risorse]** con un nome valido che sia univoco nella sottoscrizione attiva.

```azurecli
az group create --name [resource-group] --location eastus
```

Verrà così restituito un blocco JSON che indica che il gruppo di risorse creato.

```json
{
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/<resourcegroup>",
  "location": "eastus",
  "managedBy": null,
  "name": "<resourcegroup>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Si noti che viene restituito l'identificatore univoco, il percorso e il nome della sottoscrizione come parte della risposta. È possibile usare questi elementi per verificare che il gruppo sia stato creato nella sottoscrizione e posizione appropriate.

Ora che abbiamo imparato a creare un gruppo di risorse, è possibile creare una nuova macchina virtuale.