Come procedura consigliata, l'inserimento di un registro contenitori in ogni area in cui vengono eseguite le immagini consente l'esecuzione di operazioni in posizioni di rete vicine e quindi di trasferimenti di livelli di immagine più veloci e affidabili. La replica geografica consente al registro contenitori di Azure di fungere da singolo registro in modo da servire più aree con registri regionali multimaster.

Un registro con replica geografica è caratterizzato dai vantaggi seguenti:

* I nomi di registro/immagine/tag singoli possono essere usati in più aree
* Accesso al registro in una posizione di rete vicina da distribuzioni regionali
* Niente corrispettivi aggiuntivi per il traffico in uscita perché il pull delle immagini viene eseguito da un registro replicato in locale nella stessa area dell'host del contenitore
* Gestione unica di un registro in più aree

## <a name="replicate-image-to-multiple-locations"></a>Replica di immagini in più posizioni

Usare il comando `az acr replication create` per replicare le immagini del contenitore in un'altra area. In questo esempio viene creata una replica per l'area `japaneast`. Aggiornare `<acrName>` con il nome del registro contenitori.

```azurecli
az acr replication create --registry <acrName> --location japaneast
```

L'output dovrebbe essere simile al seguente:

```console
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myACR0007/replications/japaneast",
  "location": "japaneast",
  "name": "japaneast",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "status": {
    "displayStatus": "Syncing",
    "message": null,
    "timestamp": "2018-08-15T20:22:09.275792+00:00"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries/replications"
}
```

Per visualizzare un elenco di tutte le repliche dell'immagine del contenitore, usare il comando `az acr replication list`. Aggiornare `<acrName>` con il nome del registro contenitori.

```azurecli
az acr replication list --registry <acrName> --output table
```

L'output dovrebbe essere simile al seguente:

```console
NAME       LOCATION    PROVISIONING STATE    STATUS
---------  ----------  --------------------  --------
japaneast  japaneast   Succeeded             Ready
eastus     eastus      Succeeded             Ready
```

Anche se in questo esercizio non è stato usato il portale di Azure, è utile vedere l'esperienza del portale.

Nel portale di Azure selezionare `Replications` per un registro contenitori di Azure per visualizzare una mappa che illustra in dettaglio le repliche correnti. Le immagini del contenitore possono essere replicate in altre aree geografiche selezionando le aree sulla mappa.

![Mappa di replica dei contenitori come visualizzata nel portale di Azure](../media/replication-map.png)

## <a name="clean-up"></a>Eseguire la pulizia

Questa è l'ultima unità del modulo di apprendimento sul Registro contenitori di Azure. A questo punto è possibile pulire le risorse create eliminando il gruppo di risorse. A questo scopo, usare il comando `az group delete`.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a>Riepilogo

In questo modulo si è eseguita la replica di un'immagine del contenitore in più aree di Azure tramite l'interfaccia della riga di comando di Azure.