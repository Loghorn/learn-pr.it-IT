Per definire l'immagine in base alla quale creare la macchina virtuale si è usato `Debian`. Azure offre diverse immagini di macchina virtuale standard che è possibile usare per creare una macchina virtuale. 

## <a name="listing-images"></a>Elenco di immagini

È possibile ottenere un elenco delle immagini di macchina virtuale disponibili usando il comando seguente. 

```azurecli
az vm image list --output table
```

L'output del comando include le immagini più diffuse che fanno parte di un elenco offline integrato nell'interfaccia della riga di comando di Azure. In Azure Marketplace sono tuttavia disponibili _centinaia_ di opzioni di immagine. 

## <a name="getting-all-images"></a>Elenco di tutte le immagini

È possibile ottenere un elenco completo aggiungendo il flag `--all` al comando. Poiché l'elenco delle immagini nel Marketplace è molto ampio, è utile filtrarlo con le opzioni `--publisher`, `--sku` o `–-offer`.

Si provi ad esempio a usare il comando seguente per visualizzare _tutte_ le immagini di Wordpress disponibili:

```azurecli
az vm image list --sku Wordpress --output table --all
```

Oppure si può provare questo comando per visualizzare tutte le immagini Microsoft:

```azurecli
az vm image list --publisher Microsoft --output table --all
```

## <a name="location-specific-images"></a>Immagini specifiche dell'area

Alcune immagini sono disponibili solo in determinate aree. Provare ad aggiungere il flag `--location [location]` al comando per limitare l'ambito dei risultati alle immagini disponibili nell'area in cui si vuole creare la macchina virtuale. Digitare ad esempio quanto segue in Azure Cloud Shell per ottenere l'elenco delle immagini disponibili nell'area `eastus`.

```azurecli
az vm image list --location eastus --output table
```

Provare a controllare alcune delle immagini presenti in altre posizioni disponibili nell'ambiente sandbox di Azure:

[!include[](../../../includes/azure-sandbox-regions-note.md)]

> [!TIP]
> Queste sono le immagini standard offerte da Azure. Tenere presente che è anche possibile [creare e caricare immagini personalizzate](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) per creare macchine virtuali in base a configurazioni univoche oppure versioni o distribuzioni meno comuni di un sistema operativo.

La finestra di comando a questo punto è probabilmente completa. È possibile digitare `clear` per cancellare la schermata se lo si vuole.