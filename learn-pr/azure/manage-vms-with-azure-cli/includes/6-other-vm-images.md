Per definire l'immagine in base alla quale creare la macchina virtuale si è usato `Debian`. Azure offre diverse immagini di macchina virtuale standard che è possibile usare per creare una macchina virtuale. 

È possibile ottenere un elenco delle immagini disponibili tramite il comando `az vm image list --output table`. L'output del comando include le immagini più diffuse che fanno parte di un elenco offline integrato nell'interfaccia della riga di comando di Azure. In Azure Marketplace sono tuttavia disponibili _centinaia_ di opzioni di immagine. 

> [!TIP]
> È possibile ottenere un elenco completo aggiungendo il flag `--all` al comando. Poiché l'elenco delle immagini in Marketplace è molto ampio, è utile filtrare l'elenco con le opzioni `--publisher` o `–-offer`.

Alcune immagini sono disponibili solo in determinate aree. Provare ad aggiungere il flag `--location [location]` al comando per limitare l'ambito dei risultati alle immagini disponibili nell'area in cui si vuole creare la macchina virtuale. Ad esempio, digitare quanto segue in Azure Cloud Shell per ottenere l'elenco delle immagini disponibili nell'area `eastus`.

```azurecli
az vm image list --location eastus --output table
```

> [!NOTE]
> Queste sono le immagini standard fornite da Azure. Tenere presente che è anche possibile [creare e caricare immagini personalizzate](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) per creare macchine virtuali in base a configurazioni univoche oppure versioni o distribuzioni meno comuni di un sistema operativo.