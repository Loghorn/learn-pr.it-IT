## <a name="get-an-access-key"></a>Ottenere una chiave di accesso

Se non è già stato fatto, eseguire il comando seguente in Azure Cloud Shell per archiviare la chiave di accesso API in una variabile denominata `key`. Questa variabile verrà usata in chiamate successive.

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[sandbox resource group name]</rgn> \
--query key1 -o tsv)
```
