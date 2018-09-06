In questa unità si creerà un Registro contenitori di Azure usando l'interfaccia della riga di comando di Azure.

## <a name="create-an-azure-container-registry"></a>Creare un Registro contenitori di Azure

Prima di creare il Registro contenitori di Azure, è necessario un *gruppo di risorse* in cui eseguirne la distribuzione. Un gruppo di risorse è una raccolta logica in cui vengono distribuite e gestite tutte le risorse di Azure.

Creare un gruppo di risorse con il comando `az group create`. Nell'esempio seguente viene creato un gruppo di risorse denominato *myResourceGroup* nell'area *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Dopo aver creato il gruppo di risorse, creare un Registro contenitori di Azure con il comando `az acr create`. Il nome del registro contenitori deve essere univoco in Azure e contenere da 5 a 50 caratteri alfanumerici. Sostituire `<acrName>` con un nome univoco per il registro.

Per questo esempio viene distribuito uno SKU del registro Premium. Lo SKU Premium è obbligatorio per la replica geografica. Per altre informazioni sugli SKU del Registro contenitori di Azure, vedere [SKU del Registro contenitori di Azure](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus)

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Premium
```

Di seguito è riportato l'output di esempio per una nuova istanza di Registro contenitori di Azure.

```console
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-15T19:19:07.042178+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myACR0007",
  "location": "eastus",
  "loginServer": "myacr.azurecr.io",
  "name": "myACR",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Nel resto del modulo viene usato `<acrName>` come segnaposto per il nome del registro contenitori scelto in questo passaggio.

## <a name="summary"></a>Riepilogo

In questa unità è stato creato un Registro contenitori di Azure usando l'interfaccia della riga di comando di Azure. Nella prossima unità si userà Registro contenitori di Azure per creare un'immagine del contenitore e archiviarla nel registro contenitori.