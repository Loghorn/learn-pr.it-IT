Ora che è stata creata una macchina virtuale, si possono recuperare informazioni su di essa tramite altri comandi.

Iniziare con `vm list`.

```azurecli
az vm list
```

Questo comando restituirà _tutte_ le macchine virtuali definite nella sottoscrizione. È possibile filtrare l'output in base a un gruppo di risorse specifico tramite il parametro `--resource-group`. 

## <a name="output-types"></a>Tipi di output
Si noti che il tipo di risposta predefinito per tutti i comandi finora eseguiti è JSON. È ottimale per gli script, ma per la maggior parte delle persone è più difficile da leggere. È possibile modificare lo stile dell'output per qualsiasi risposta tramite il flag `--output`. Provare ad esempio il comando seguente in Azure Cloud Shell per visualizzare un diverso stile dell'output.

```azurecli
az vm list --output table
```

Oltre a `table`, è possibile specificare `json` (impostazione predefinita), `jsonc` (codice JSON colorato) o `tsv` (valori delimitati da tabulazioni). Provare alcune varianti con il comando precedente per verificare la differenza.

## <a name="getting-the-ip-address"></a>Recupero dell'indirizzo IP

Un altro comando utile è `vm list-ip-addresses`, che elencherà gli indirizzi IP pubblico e privato di una VM. Se vengono modificati o non si sono acquisiti durante la creazione, è possibile recuperarli in qualsiasi momento.

```azurecli
az vm list-ip-addresses -n SampleVM -o table
```

Il comando restituisce:

```
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
SampleVM          168.61.54.62         10.0.0.4
```

> [!NOTE]
> Si noti che per il flag `--output` si sta usando la sintassi abbreviata `-o`. La maggior parte dei parametri per i comandi dell'interfaccia della riga di comando di Azure può essere abbreviata in un singolo trattino e una lettera. Ad esempio, è possibile abbreviare `--name` in `-n` e `--resource-group` in `-g`. L'abbreviazione è comoda per la digitazione, ma negli script è consigliabile usare il nome completo dell'opzione per maggiore chiarezza. Per informazioni dettagliate su ogni comando, vedere la documentazione.

## <a name="getting-vm-details"></a>Recupero dei dettagli della VM

È possibile ottenere informazioni più dettagliate su una macchina virtuale specifica in base al nome o all'ID usando il comando `vm show`.

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM
```

Questo comando restituirà un blocco JSON piuttosto esteso con tutti i tipi di informazioni sulla VM, tra cui i dispositivi di archiviazione collegati, le interfacce di rete e tutti gli ID oggetto per le risorse a cui la VM è connessa. Anche in questo caso si può passare a un formato di tabella, ma in tale formato vengono omessi quasi tutti i dati interessanti. Si può invece usare un linguaggio di query predefinito per JSON denominato [JMESPath](http://jmespath.org/).

## <a name="adding-filters-to-queries-with-jmespath"></a>Aggiunta di filtri alle query con JMESPath

JMESPath è un linguaggio di query standard del settore basato su oggetti JSON. La query più semplice consiste nello specificare un _identificatore_ che seleziona una chiave nell'oggetto JSON.

Ad esempio, nel caso dell'oggetto seguente:

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

È possibile usare la query `people` per restituire la matrice di valori per la matrice `people`. Se si vuole restituire _una_ sola persona, si può usare un indicizzatore. `people[1]`, ad esempio, restituirà:

```json
{
    "name": "Barney",
    "age": 25
}
```

È anche possibile aggiungere qualificatori specifici che restituiscano un subset degli oggetti in base ad alcuni criteri specificati. Ad esempio, l'aggiunta del qualificatore `people[?age > '25']` restituirebbe:

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

È infine possibile limitare i risultati aggiungendo una selezione come `people[?age > '25'].[name]`, che restituisce solo i nomi:

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

JMESPath offre diverse altre interessanti funzionalità di query. Quando si ha tempo, vedere l'[esercitazione online](http://jmespath.org/tutorial.html) disponibile nel sito [JMESPath.org](http://jmespath.org/).

## <a name="filtering-our-azure-cli-queries"></a>Applicazione di filtri alle query dell'interfaccia della riga di comando di Azure

Con una conoscenza di base delle query JMES, è possibile aggiungere filtri ai dati restituiti da query come il comando `vm show`. Ad esempio, si può recuperare il nome utente amministratore:

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "osProfile.adminUsername"
```

È possibile ottenere la dimensione assegnata alla VM:

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query hardwareProfile.vmSize
```

Per recuperare tutti gli ID delle interfacce di rete, invece, si può usare la query seguente:

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "networkProfile.networkInterfaces[].id"
```

Questa tecnica di query funziona con qualsiasi comando dell'interfaccia della riga di comando di Azure e può essere usata per estrarre dati specifici nella riga di comando. È utile anche per gli script. È ad esempio possibile estrarre un valore dall'account Azure e archiviarlo in una variabile di ambiente o di script. Se si decide di usarla in questo modo, un flag utile da aggiungere è il parametro `--output tsv`, abbreviabile in `-o tsv`. In questo modo, i risultati vengono restituiti in valori delimitati da tabulazioni e includono solo i valori di dati effettivi con tabulazioni come separatori.

Ad esempio:

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "networkProfile.networkInterfaces[].id" -o tsv
```

Restituisce il testo: `/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`
