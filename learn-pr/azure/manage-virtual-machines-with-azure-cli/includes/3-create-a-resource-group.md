Il nostro obiettivo consiste nel creare una nuova macchina virtuale di Azure. È necessario fornire diversi tipi di informazioni per identificare il percorso della risorsa, il sistema operativo da usare e la configurazione hardware necessaria per la macchina virtuale. Iniziare con un **gruppo di risorse**.

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Azure usa _gruppi di risorse_ per raggruppare le risorse correlate, ad esempio macchine virtuali e database. Il gruppo di risorse identifica anche un percorso specifico, denominato "area", che deciderà quale data center di risorsa è inserito.

> [!NOTE]
> Il sandbox di Azure mette a disposizione un gruppo di risorse create precedentemente denominato <rgn>[nome gruppo di risorse sandbox]</rgn>. Non è necessario eseguire questi passaggi. Tuttavia, questi sono i comandi da eseguire per creare le _proprie_ risorse per i progetti reali. Sandbox di Azure non consente di creare direttamente gruppi di risorse.

Come esempio, digitare il seguente comando dell'interfaccia della riga di comando di Azure in Azure Cloud Shell per creare il gruppo di risorse nella regione **Stati Uniti orientali**. Sostituire **[resource-group]** con un nome valido che sia univoco nella sottoscrizione attiva.

```azurecli
az group create --name [resource-group] --location eastus
```

Verrà restituito un blocco JSON che indica che il gruppo di risorse è stato creato.

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

Si noti che viene restituito l'identificatore univoco, la località e il nome della sottoscrizione come parte della risposta. È possibile usare questi elementi per verificare che il gruppo sia stato creato nella sottoscrizione e località appropriate.

Ora che si sa creare un gruppo di risorse, è possibile creare una nuova macchina virtuale.