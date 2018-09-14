La semplicità e la velocità della distribuzione di contenitori in Istanze di contenitore di Azure offre una piattaforma interessante per l'esecuzione di attività eseguite una sola volta come la compilazione, il test e il rendering di immagini in un'istanza del contenitore.

Con un criterio di riavvio configurabile, è possibile specificare l'arresto dei contenitori al completamento dei processi. Poiché le istanze del contenitore vengono fatturate al secondo, vengono addebitate solo le risorse di calcolo usate mentre il contenitore che esegue l'attività è in esecuzione.

## <a name="container-restart-policies"></a>Criteri di riavvio del contenitore

Quando si crea un contenitore in Istanze di contenitore di Azure, è possibile specificare una delle tre impostazioni dei criteri di riavvio:

| Criterio di riavvio   | Descrizione |
| ---------------- | :---------- |
| `Always` | I contenitori nel gruppo contenitore vengono sempre riavviati. Questa è l'impostazione **predefinita** applicata quando non si specifica alcun criterio di riavvio al momento della creazione del contenitore. |
| `Never` | I contenitori nel gruppo contenitore non vengono riavviati mai. I contenitori vengono eseguiti al massimo una volta. |
| `OnFailure` | I contenitori nel gruppo contenitore vengono riavviati solo quando il processo eseguito nel contenitore ha esito negativo, ovvero quando termina con un codice di uscita diverso da zero. I contenitori vengono eseguiti almeno una volta. |

Nell'unità di precedente di questo modulo è stato creato un contenitore senza un criterio di riavvio specifico. Per impostazione predefinita, a questo contenitore è stato applicato il criterio di riavvio *Always*. Poiché il carico di lavoro nel contenitore presuppone un'esecuzione prolungata (server Web), questo criterio ha senso.

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

## <a name="summary"></a>Riepilogo

In questa unità è stata creata un'istanza di contenitore con il criterio di riavvio *OnFailure*. Questa configurazione funziona per i contenitori che eseguono attività di breve durata.

Nell'unità successiva sarà possibile impostare variabili di ambiente in Istanze di contenitore di Azure.
