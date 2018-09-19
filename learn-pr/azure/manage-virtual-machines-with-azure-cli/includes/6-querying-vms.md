<span data-ttu-id="bcf3d-101">Ora che è stata creata una macchina virtuale, si possono recuperare informazioni su di essa tramite altri comandi.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-101">Now that a virtual machine has been created, we can get information about it through other commands.</span></span>

<span data-ttu-id="bcf3d-102">Iniziare con `vm list`.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-102">Let's start with `vm list`.</span></span>

```azurecli
az vm list
```

<span data-ttu-id="bcf3d-103">Questo comando restituirà _tutte_ le macchine virtuali definite nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-103">This command will return _all_ virtual machines defined in this subscription.</span></span> <span data-ttu-id="bcf3d-104">È possibile filtrare l'output in base a un gruppo di risorse specifico tramite il parametro `--resource-group`.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-104">The output can be filtered to a specific resource group through the `--resource-group` parameter.</span></span> 

## <a name="output-types"></a><span data-ttu-id="bcf3d-105">Tipi di output</span><span class="sxs-lookup"><span data-stu-id="bcf3d-105">Output types</span></span>
<span data-ttu-id="bcf3d-106">Si noti che il tipo di risposta predefinito per tutti i comandi finora eseguiti è JSON.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-106">Notice that the default response type for all the commands we've done so far is JSON.</span></span> <span data-ttu-id="bcf3d-107">È ottimale per gli script, ma per la maggior parte delle persone è più difficile da leggere.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-107">This is great for scripting - but most people find it harder to read.</span></span> <span data-ttu-id="bcf3d-108">È possibile modificare lo stile dell'output per qualsiasi risposta tramite il flag `--output`.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-108">You can change the output style for any response through the `--output` flag.</span></span> <span data-ttu-id="bcf3d-109">Provare ad esempio il comando seguente in Azure Cloud Shell per visualizzare un diverso stile dell'output.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-109">For example, try the following command in Azure Cloud Shell to see the different output style.</span></span>

```azurecli
az vm list --output table
```

<span data-ttu-id="bcf3d-110">Oltre a `table`, è possibile specificare `json` (impostazione predefinita), `jsonc` (codice JSON colorato) o `tsv` (valori delimitati da tabulazioni).</span><span class="sxs-lookup"><span data-stu-id="bcf3d-110">Along with `table`, you can specify `json` (the default), `jsonc` (colorized JSON), or `tsv` (Table-Separated Values).</span></span> <span data-ttu-id="bcf3d-111">Provare alcune varianti con il comando precedente per verificare la differenza.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-111">Try a few variations with the above command to see the difference.</span></span>

## <a name="getting-the-ip-address"></a><span data-ttu-id="bcf3d-112">Recupero dell'indirizzo IP</span><span class="sxs-lookup"><span data-stu-id="bcf3d-112">Getting the IP address</span></span>

<span data-ttu-id="bcf3d-113">Un altro comando utile è `vm list-ip-addresses`, che elencherà gli indirizzi IP pubblico e privato di una VM.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-113">Another useful command is `vm list-ip-addresses`, which will list the public and private IP addresses for a VM.</span></span> <span data-ttu-id="bcf3d-114">Se vengono modificati o non si sono acquisiti durante la creazione, è possibile recuperarli in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-114">If they change, or you didn't capture them during creation, you can retrieve them at any time.</span></span>

```azurecli
az vm list-ip-addresses -n SampleVM -o table
```

<span data-ttu-id="bcf3d-115">Il comando restituisce:</span><span class="sxs-lookup"><span data-stu-id="bcf3d-115">This returns:</span></span>

```
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
SampleVM          168.61.54.62         10.0.0.4
```

> [!TIP]
> <span data-ttu-id="bcf3d-116">Si noti che per il flag `--output` si sta usando la sintassi abbreviata `-o`.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-116">Notice that we are using a shorthand syntax for the `--output` flag as `-o`.</span></span> <span data-ttu-id="bcf3d-117">La maggior parte dei parametri per i comandi dell'interfaccia della riga di comando di Azure può essere abbreviata in un singolo trattino e una lettera.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-117">Most parameters to Azure CLI commands can be shortened to a single dash and letter.</span></span> <span data-ttu-id="bcf3d-118">Ad esempio, è possibile abbreviare `--name` in `-n` e `--resource-group` in `-g`.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-118">For example, `--name` can be shortened to `-n` and `--resource-group` to `-g`.</span></span> <span data-ttu-id="bcf3d-119">L'abbreviazione è comoda per la digitazione, ma negli script è consigliabile usare il nome completo dell'opzione per maggiore chiarezza.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-119">This is handy for typing, but we recommend using the full option name in scripts for clarity.</span></span> <span data-ttu-id="bcf3d-120">Per informazioni dettagliate su ogni comando, vedere la documentazione.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-120">Check the documentation for details on each command.</span></span>

## <a name="getting-vm-details"></a><span data-ttu-id="bcf3d-121">Recupero dei dettagli della VM</span><span class="sxs-lookup"><span data-stu-id="bcf3d-121">Getting VM details</span></span>

<span data-ttu-id="bcf3d-122">È possibile ottenere informazioni più dettagliate su una macchina virtuale specifica in base al nome o all'ID usando il comando `vm show`.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-122">We can get more detailed information about a specific virtual machine by name or ID using the `vm show` command.</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM
```

<span data-ttu-id="bcf3d-123">Questo comando restituirà un blocco JSON piuttosto esteso con tutti i tipi di informazioni sulla VM, tra cui i dispositivi di archiviazione collegati, le interfacce di rete e tutti gli ID oggetto per le risorse a cui la VM è connessa.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-123">This will return a fairly large JSON block with all sorts of information about the VM, including attached storage devices, network interfaces, and all of the object IDs for resources that the VM is connected to.</span></span> <span data-ttu-id="bcf3d-124">Anche in questo caso si può passare a un formato di tabella, ma in tale formato vengono omessi quasi tutti i dati interessanti.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-124">Again, we could change to a table format, but that omits almost all of the interesting data.</span></span> <span data-ttu-id="bcf3d-125">Si può invece usare un linguaggio di query predefinito per JSON denominato [JMESPath](http://jmespath.org/).</span><span class="sxs-lookup"><span data-stu-id="bcf3d-125">Instead, we can turn to a built-in query language for JSON called [JMESPath](http://jmespath.org/).</span></span>

## <a name="adding-filters-to-queries-with-jmespath"></a><span data-ttu-id="bcf3d-126">Aggiunta di filtri alle query con JMESPath</span><span class="sxs-lookup"><span data-stu-id="bcf3d-126">Adding filters to queries with JMESPath</span></span>

<span data-ttu-id="bcf3d-127">JMESPath è un linguaggio di query standard del settore basato su oggetti JSON.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-127">JMESPath is an industry-standard query language built around JSON objects.</span></span> <span data-ttu-id="bcf3d-128">La query più semplice consiste nello specificare un _identificatore_ che seleziona una chiave nell'oggetto JSON.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-128">The simplest query is to specify an _identifier_ that selects a key in the JSON object.</span></span>

<span data-ttu-id="bcf3d-129">Ad esempio, nel caso dell'oggetto seguente:</span><span class="sxs-lookup"><span data-stu-id="bcf3d-129">For example, given the object:</span></span>

```json
{
  "people": [
    {
      "name": "Fred",
      "age": 28
    },
    {
      "name": "Barney",
      "age": 25
    },
    {
      "name": "Wilma",
      "age": 27
    }
  ]
}
```

<span data-ttu-id="bcf3d-130">È possibile usare la query `people` per restituire la matrice di valori per la matrice `people`.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-130">We can use the query `people` to return the array of values for the `people` array.</span></span> <span data-ttu-id="bcf3d-131">Se si vuole restituire _una_ sola persona, si può usare un indicizzatore.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-131">If we just want _one_ of the people, we can use an indexer.</span></span> <span data-ttu-id="bcf3d-132">`people[1]`, ad esempio, restituirà:</span><span class="sxs-lookup"><span data-stu-id="bcf3d-132">For example, `people[1]` would return:</span></span>

```json
{
    "name": "Barney",
    "age": 25
}
```

<span data-ttu-id="bcf3d-133">È anche possibile aggiungere qualificatori specifici che restituiscano un subset degli oggetti in base ad alcuni criteri specificati.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-133">We can also add specific qualifiers that would return a subset of the objects based on some criteria.</span></span> <span data-ttu-id="bcf3d-134">Ad esempio, l'aggiunta del qualificatore `people[?age > '25']` restituirebbe:</span><span class="sxs-lookup"><span data-stu-id="bcf3d-134">For example, adding the qualifier `people[?age > '25']` would return:</span></span>

```json
[
  {
    "name": "Fred",
    "age": 28
  },
  {
    "name": "Wilma",
    "age": 27
  }
]
```

<span data-ttu-id="bcf3d-135">È infine possibile limitare i risultati aggiungendo una selezione come `people[?age > '25'].[name]`, che restituisce solo i nomi:</span><span class="sxs-lookup"><span data-stu-id="bcf3d-135">Finally, we can constrain the results by adding a select: `people[?age > '25'].[name]` that returns just the names:</span></span>

```json
[
  [
    "Fred"
  ],
  [
    "Wilma"
  ]
]
```

<span data-ttu-id="bcf3d-136">JMESPath offre diverse altre interessanti funzionalità di query.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-136">JMESQuery has several other interesting query features.</span></span> <span data-ttu-id="bcf3d-137">Quando si ha tempo, vedere l'[esercitazione online](http://jmespath.org/tutorial.html) disponibile nel sito [JMESPath.org](http://jmespath.org/).</span><span class="sxs-lookup"><span data-stu-id="bcf3d-137">When you have time, check out the [online tutorial](http://jmespath.org/tutorial.html) available on the [JMESPath.org](http://jmespath.org/) site.</span></span>

## <a name="filtering-our-azure-cli-queries"></a><span data-ttu-id="bcf3d-138">Applicazione di filtri alle query dell'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="bcf3d-138">Filtering our Azure CLI queries</span></span>

<span data-ttu-id="bcf3d-139">Con una conoscenza di base delle query JMES, è possibile aggiungere filtri ai dati restituiti da query come il comando `vm show`.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-139">With a basic understanding of JMES queries, we can add filers to the data being returned by queries like the `vm show` command.</span></span> <span data-ttu-id="bcf3d-140">Ad esempio, si può recuperare il nome utente amministratore:</span><span class="sxs-lookup"><span data-stu-id="bcf3d-140">For example, we can retrieve the admin user name:</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --query "osProfile.adminUsername"
```

<span data-ttu-id="bcf3d-141">È possibile ottenere la dimensione assegnata alla VM:</span><span class="sxs-lookup"><span data-stu-id="bcf3d-141">We can get the size assigned to our VM:</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --query hardwareProfile.vmSize
```

<span data-ttu-id="bcf3d-142">Per recuperare tutti gli ID delle interfacce di rete, invece, si può usare la query seguente:</span><span class="sxs-lookup"><span data-stu-id="bcf3d-142">Or to retrieve all the IDs for your network interfaces, you can use the query:</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --query "networkProfile.networkInterfaces[].id"
```

<span data-ttu-id="bcf3d-143">Questa tecnica di query funziona con qualsiasi comando dell'interfaccia della riga di comando di Azure e può essere usata per estrarre dati specifici nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-143">This query technique will work with any Azure CLI command and can be used to pull specific bits of data out on the command line.</span></span> <span data-ttu-id="bcf3d-144">È utile anche per gli script. È ad esempio possibile estrarre un valore dall'account Azure e archiviarlo in una variabile di ambiente o di script.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-144">It is useful for scripting as well - for example, you can pull a value out of your Azure account and store it in an environment or script variable.</span></span> <span data-ttu-id="bcf3d-145">Se si decide di usarla in questo modo, un flag utile da aggiungere è il parametro `--output tsv`, abbreviabile in `-o tsv`.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-145">If you decide to use it this way, a useful flag to add is the `--output tsv` parameter (which can be shortened to `-o tsv`).</span></span> <span data-ttu-id="bcf3d-146">In questo modo, i risultati vengono restituiti in valori delimitati da tabulazioni e includono solo i valori di dati effettivi con tabulazioni come separatori.</span><span class="sxs-lookup"><span data-stu-id="bcf3d-146">This will return the results in tab-separated values that only include the actual data values with tab separators.</span></span>

<span data-ttu-id="bcf3d-147">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bcf3d-147">As an example,</span></span>

```azurecli
az vm show --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --query "networkProfile.networkInterfaces[].id" -o tsv
```

<span data-ttu-id="bcf3d-148">Restituisce il testo: `/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`</span><span class="sxs-lookup"><span data-stu-id="bcf3d-148">returns the text: `/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`</span></span>
