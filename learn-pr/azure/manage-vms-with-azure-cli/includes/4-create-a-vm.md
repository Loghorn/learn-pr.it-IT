L'interfaccia della riga di comando di Azure include il comando `vm` per lavorare con le macchine virtuali in Azure. È possibile fornire vari sottocomandi per eseguire attività specifiche. Quelli più comuni includono:

| Sottocomando | DESCRIZIONE |
|-------------|-------------|
| `create`    | Creare una nuova macchina virtuale |
| `deallocate` | Deallocare una macchina virtuale |
| `delete` | Eliminare una macchina virtuale |
| `list` | Elencare le macchine virtuali create nella sottoscrizione |
| `open-port` | Aprire una porta di rete specifica per il traffico in ingresso |
| `restart` | Riavviare una macchina virtuale |
| `show` | Ottenere i dettagli di una macchina virtuale |
| `start` | Avviare una macchina virtuale arrestata |
| `stop` | Arrestare una macchina virtuale in esecuzione |
| `update` | Aggiornare una proprietà di una macchina virtuale |

> [!NOTE]
> Per un elenco completo dei comandi, è possibile controllare la [documentazione di riferimento dell'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).

Si inizia dal primo: `az vm create`. Questo comando viene usato per creare una macchina virtuale in un gruppo di risorse. Esistono vari parametri che è possibile passare per configurare tutti gli aspetti della nuova macchina virtuale. I tre parametri da fornire sono:

| Parametro | DESCRIZIONE |
|-----------|-------------|
| `resource-group` | Il gruppo di risorse che conterrà la macchina virtuale |
| `name` | Il nome della macchina virtuale deve essere univoco all'interno del gruppo di risorse |
| `image` | L'immagine del sistema operativo da usare per creare la macchina virtuale |

È anche utile aggiungere il flag `--verbose` per visualizzare lo stato di avanzamento della creazione della macchina virtuale. 

## <a name="create-a-linux-virtual-machine"></a>Creare una macchina virtuale Linux

Verrà ora illustrato come creare una nuova macchina virtuale Linux. Eseguire il comando seguente in Azure Cloud Shell:

```azurecli
az vm create --resource-group ExerciseResources --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --verbose 
```

Questo comando crea una nuova macchina virtuale Linux **Debian** chiamata `SampleVM`. Si noti che lo strumento dell'interfaccia della riga di comando di Azure è bloccato durante la creazione della macchina virtuale. Se si preferisce non attendere, è possibile usare l'opzione `--no-wait` per indicare allo strumento dell'interfaccia della riga di comando di Azure di restituire subito un risultato, ad esempio se si esegue il comando in uno script. In seguito nello script, usare il comando `azure vm wait --name [vm-name]` per attendere la creazione della macchina virtuale.

Se si esaminano le risposte dettagliate, si vedrà anche come viene usato il nome `SampleVM` per denominare diverse dipendenze per la macchina virtuale.

```
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

È possibile eseguire l'override di questi nomi di risorse generati automaticamente usando parametri facoltativi per `vm create`, ad esempio `--vnet-name` e `--public-ip-address-dns-name`.

Si noti che viene specificato il nome dell'account amministratore "aldis" tramite il flag `admin-username`. Se lo si omette, il comando `vm create` userà il _nome dell'utente corrente_. Dato che le regole per i nomi degli account sono diverse per ogni sistema operativo, è consigliabile specificare un nome. Nomi comuni, ad esempio "root" e "admin", non sono consentiti per la maggior parte delle immagini.

Si usa anche il flag `generate-ssh-keys`. Questo parametro viene usato per le distribuzioni di Linux e crea una coppia di chiavi di sicurezza che permettono di usare lo strumento `ssh` per accedere da remoto alla macchina virtuale. I due file vengono inseriti nella cartella `.ssh` nella macchina e nella macchina virtuale. Se si dispone già di una chiave SSH denominata `id_rsa` nella cartella di destinazione, questa verrà usata e non ne verrà generata una nuova.

Dopo aver creato la macchina virtuale, si otterrà una risposta JSON che include lo stato corrente della macchina virtuale e i relativi indirizzi IP pubblici e privati assegnati da Azure:

```json
{
  "fqdns": "",
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1A-D9-74",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "168.61.54.62",
  "resourceGroup": "ExerciseResources",
  "zones": ""
}
```

> [!NOTE]
> Si noti che la macchina virtuale è stata creata nella posizione **eastus**. Per impostazione predefinita, la macchina virtuale viene creata nella posizione identificata dall'area di appartenenza. Tuttavia, in alcuni casi potrebbe essere necessario associare la macchina virtuale a un'area esistente, anche se si trova in un'altra parte del mondo. A tale scopo, è possibile specificare il parametro `--location` dell'opzione come parte del comando `az vm create`.