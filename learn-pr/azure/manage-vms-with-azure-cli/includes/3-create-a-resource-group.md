Il nostro obiettivo consiste nel creare una nuova macchina virtuale di Azure. È necessario fornire diversi tipi di informazioni per identificare il percorso della risorsa, il sistema operativo da usare e la configurazione hardware necessaria per la macchina virtuale. Iniziare con un **gruppo di risorse**.

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Azure usa _gruppi di risorse_ per raggruppare le risorse correlate, ad esempio macchine virtuali e database. Il gruppo di risorse identifica anche un percorso specifico (denominato "area") che deciderà quale data center di risorsa è inserito.

Poiché è un esperimento, iniziare creando un nuovo gruppo di risorse denominato `ExerciseResources` e posizionarlo nell'area `eastus`.

<!-- TODO: replace with free ed-tier -->

Digitare il comando dell'interfaccia della riga di comando di Azure seguente in Azure Cloud Shell per creare il gruppo di risorse nella sottoscrizione.

```azurecli
az group create --name ExerciseResources --location eastus
```

Verrà restituito un blocco JSON che indica che il gruppo di risorse è stato creato. Dovrebbe essere simile a:

```json
{
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources",
  "location": "eastus",
  "managedBy": null,
  "name": "ExerciseResources",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Si noti che viene restituito l'identificatore univoco, il percorso e il nome della sottoscrizione come parte della risposta. È possibile usare questi elementi per verificare che il gruppo sia stato creato nella sottoscrizione e posizione appropriate.

Con un gruppo di risorse disponibile, creare una nuova macchina virtuale contenuta in esso.