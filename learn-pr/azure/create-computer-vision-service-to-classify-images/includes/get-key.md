## <a name="get-an-access-key"></a><span data-ttu-id="63368-101">Ottenere una chiave di accesso</span><span class="sxs-lookup"><span data-stu-id="63368-101">Get an access key</span></span>

<span data-ttu-id="63368-102">Se non è già stato fatto, eseguire il comando seguente in Azure Cloud Shell per archiviare la chiave di accesso API in una variabile denominata `key`.</span><span class="sxs-lookup"><span data-stu-id="63368-102">If you haven't done so already, run the following command in Azure Cloud Shell to store the API access key in a variable called `key`.</span></span> <span data-ttu-id="63368-103">Questa variabile verrà usata in chiamate successive.</span><span class="sxs-lookup"><span data-stu-id="63368-103">We'll use this variable in subsequent calls.</span></span>

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[sandbox resource group name]</rgn> \
--query key1 -o tsv)
```
