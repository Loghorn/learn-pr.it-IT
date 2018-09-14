La semplicità e rapidità di distribuzione di contenitori in istanze di contenitore di Azure fornisce una piattaforma interessante per l'esecuzione di attività eseguire una sola volta, ad esempio compilazione, test e il rendering delle immagini.

Con un criterio di riavvio configurabile, è possibile specificare l'arresto dei contenitori al completamento dei processi. Poiché le istanze di contenitore vengono fatturate al secondo, ti viene addebitata solo per le risorse di calcolo usate mentre il contenitore è in esecuzione che l'attività è in esecuzione.

## <a name="container-restart-policies"></a>Criteri di riavvio del contenitore

Istanze di contenitore di Azure sono disponibili tre opzioni di criteri di riavvio:

| Criterio di riavvio   | Descrizione |
| ---------------- | :---------- |
| `Always` | I contenitori nel gruppo contenitore vengono sempre riavviati. Questo criterio ha senso per attività a esecuzione prolungata, ad esempio un server web. Questa è l'impostazione **predefinita** applicata quando non si specifica alcun criterio di riavvio al momento della creazione del contenitore. |
| `Never` | I contenitori nel gruppo contenitore non vengono riavviati mai. I contenitori vengono eseguiti al massimo una volta. |
| `OnFailure` | I contenitori nel gruppo contenitore vengono riavviati solo quando il processo eseguito nel contenitore ha esito negativo, ovvero quando termina con un codice di uscita diverso da zero. I contenitori vengono eseguiti almeno una volta. Questo criterio funziona bene per i contenitori che eseguono le attività di breve durate. |

## <a name="run-to-completion"></a>Eseguire fino al completamento

Per visualizzare i criteri di riavvio in azione, creare un'istanza del contenitore dall'immagine *microsoft/aci-wordcount* e specificare il criterio di riavvio *OnFailure*. Questo contenitore di esempio esegue uno script di Python che analizza il testo Amleto di Shakespeare, scrive le 10 parole più comuni in STDOUT ed esce.

Eseguire il contenitore di esempio con il comando `az container create` seguente:

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Istanze di contenitore di Azure avvia il contenitore e lo interrompe quando la sua applicazione, o lo script in questo caso, esce. Quando Istanze di contenitore di Azure arresta un contenitore i cui criteri di riavvio sono *Never* o *OnFailure*, lo stato del contenitore viene impostato su **Terminato**.

È possibile controllare lo stato del contenitore usando il comando `az container show`:

```azurecli
az container show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

Quando lo stato del contenitore di esempio mostra **Terminato**, è possibile visualizzare l'output dell'attività visualizzando i log dei contenitori. Eseguire il comando **az container logs** per visualizzare l'output dello script:

```azurecli
az container logs --resource-group <rgn>[Sandbox resource group name]</rgn> --name mycontainer-restart-demo
```

Output:

```json
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```