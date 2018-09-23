In questa unità si creerà un registro contenitori di Azure usando l'interfaccia della riga di comando di Azure.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]
 
## <a name="create-an-azure-container-registry"></a>Creare un registro contenitori di Azure

Si lavorerà in un ambiente sandbox gratuito, pertanto non occorre creare un gruppo di risorse. Creare un registro contenitori di Azure con il comando `az acr create`. Il nome del registro contenitori deve essere univoco in Azure e contenere tra 5 e 50 caratteri alfanumerici. Sostituire `<acrName>` con un nome univoco da assegnare al registro.

Per questo esempio viene distribuito uno SKU del registro Premium. Lo SKU Premium è obbligatorio per la replica geografica. Immettere il comando seguente nell'editor di Cloud Shell.

```azurecli
az acr create --resource-group <rgn>[Sandbox resource group name]</rgn> --name <acrName> --sku Premium
```

Di seguito è riportato l'output di esempio per un nuovo registro contenitori di Azure:

```output
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

In questa unità è stato creato un registro contenitori di Azure usando l'interfaccia della riga di comando di Azure.