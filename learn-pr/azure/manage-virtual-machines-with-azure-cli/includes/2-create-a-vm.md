Iniziamo con l'attività più ovvia, ovvero la creazione di una macchina virtuale di Azure.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="logins-subscriptions-and-resource-groups"></a>Account di accesso, sottoscrizioni e gruppi di risorse

L'ambiente di lavoro è Azure Cloud Shell. Una volta attivato l'ambiente sandbox, viene stabilita una connessione ad Azure con una sottoscrizione gratuita gestita da Microsoft Learn. Non è necessario accedere ad Azure in modo autonomo né selezionare una sottoscrizione, perché le operazioni vengono eseguite in modo automatico. In genere l'utente creerebbe anche un _gruppo di risorse_ in cui includere le nuove risorse. In questo modulo il gruppo di risorse viene creato automaticamente nell'ambiente sandbox di Azure al fine di eseguire tutti i comandi.

## <a name="create-a-linux-vm-with-the-azure-cli"></a>Creare una macchina virtuale Linux con l'interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure include il comando `vm` per usare le macchine virtuali in Azure. È possibile fornire vari sottocomandi per eseguire attività specifiche. Quelli più comuni includono:

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

> [!div class="mx-tableFixed"]
> | Parametro | Descrizione |
> |-----------|-------------|
> | `resource-group` | Il gruppo di risorse proprietario della macchina virtuale. Usare **<rgn>[gruppo di risorse sandbox]</rgn>**. |
> | `name` | Il nome della macchina virtuale deve essere univoco all'interno del gruppo di risorse. |
> | `image` | L'immagine del sistema operativo da usare per creare la macchina virtuale. |
> | `location` | L'area in cui inserire la macchina virtuale. In genere tale area è vicina all'utente della macchina virtuale. In questo esercizio scegliere una località nelle vicinanze nell'elenco seguente. |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

È anche utile aggiungere il flag `--verbose` per visualizzare lo stato di avanzamento della creazione della macchina virtuale. 

## <a name="create-a-linux-virtual-machine"></a>Creare una macchina virtuale Linux

Verrà ora illustrato come creare una nuova macchina virtuale Linux. Eseguire questo comando in Azure Cloud Shell per creare una macchina Linux Debian nell'area "West US". Modificare la località se quella scelta non è nelle vicinanze.

```azurecli
az vm create --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --location westus --verbose 
```

[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


Questo comando crea una nuova macchina virtuale Linux **Debian** chiamata `SampleVM`. Si noti che lo strumento dell'interfaccia della riga di comando di Azure rimane in attesa durante la creazione della macchina virtuale. È possibile aggiungere l'opzione `--no-wait` per indicare allo strumento dell'interfaccia della riga di comando di Azure di rispondere immediatamente in modo che Azure prosegua con la creazione della macchina virtuale in background. Questo comportamento è utile se si esegue il comando in uno script. Nello script usare in seguito il comando `azure vm wait --name [vm-name]` per attendere il completamento della creazione della macchina virtuale.

Se si esaminano le risposte dettagliate, si vedrà anche come viene usato il nome `SampleVM` per denominare diverse dipendenze per la macchina virtuale.

```output
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

È possibile eseguire l'override di questi nomi di risorse generati automaticamente usando parametri facoltativi per `vm create`, ad esempio `--vnet-name` e `--public-ip-address-dns-name`.

Il nome dell'account amministratore viene specificato tramite il flag `admin-username` in modo che sia **"aldis"**. Se lo si omette, il comando `vm create` userà il _nome dell'utente corrente_. Dato che le regole per i nomi degli account sono diverse per ogni sistema operativo, è consigliabile specificare un nome. 

> [!NOTE]
> Nomi comuni, ad esempio "root" e "admin", non sono consentiti per la maggior parte delle immagini.

Si usa anche il flag `generate-ssh-keys`. Questo parametro viene usato per le distribuzioni di Linux e crea una coppia di chiavi di sicurezza che permettono di usare lo strumento `ssh` per accedere da remoto alla macchina virtuale. I due file vengono inseriti nella cartella `.ssh` nella macchina e nella macchina virtuale. Se si dispone già di una chiave SSH denominata `id_rsa` nella cartella di destinazione, questa verrà usata e non ne verrà generata una nuova.

Dopo aver creato la macchina virtuale, si otterrà una risposta JSON che include lo stato corrente della macchina virtuale e i relativi indirizzi IP pubblici e privati assegnati da Azure:

```json
{
  "fqdns": "",
  "id": "/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "westus",
  "macAddress": "00-0D-3A-58-F8-45",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
  "resourceGroup": "2568d0d0-efe3-4d04-a08f-df7f009f822a",
  "zones": ""
}
```
