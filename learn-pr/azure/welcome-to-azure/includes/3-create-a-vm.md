Come professionista IT, è probabile che si abbia esperienza in un'area specifica, ad esempio come amministratore dell'archiviazione o esperto di virtualizzazione o delle procedure più recenti per la sicurezza. Gli studenti potrebbero essere ancora alla ricerca dell'area di maggiore interesse.

::: zone pivot="windows-cloud"

Indipendentemente dal ruolo, la maggior parte delle persone inizia l'esperienza con il cloud creando una macchina virtuale. Qui verranno descritte le procedure per attivare una macchina virtuale che esegue Windows Server 2016.

::: zone-end

::: zone pivot="linux-cloud"

Indipendentemente dal ruolo, la maggior parte delle persone inizia l'esperienza con il cloud creando una macchina virtuale. Qui verranno descritte le procedure per attivare una macchina virtuale che esegue Ubuntu 16.04.

::: zone-end

Esistono molti modi per creare una macchina virtuale in Azure. Qui verrà descritto come attivare una macchina virtuale Windows o Linux usando un terminale interattivo denominato Cloud Shell. Chi lavora quotidianamente dal terminale sa che spesso rappresenta il modo più rapido per svolgere varie operazioni.

::: zone pivot="windows-cloud"

> [!TIP]
> Si preferisce Linux o si vuole provare qualcosa di nuovo? Selezionare **Linux** nella parte superiore di questa pagina per eseguire una macchina virtuale Linux.

::: zone-end

::: zone pivot="linux-cloud"

> [!TIP]
> Si preferisce Windows o si vuole provare qualcosa di nuovo? Selezionare **Windows** nella parte superiore di questa pagina per eseguire una macchina virtuale Windows Server.

::: zone-end

Verranno presentati alcuni termini di base e poi si procederà all'attivazione ed esecuzione della prima macchina virtuale.

## <a name="what-is-a-virtual-machine"></a>Che cos'è una macchina virtuale?

Una macchina virtuale è un'emulazione software di un computer fisico. Poiché le macchine virtuali esistono come software, bastano pochi minuti per generare decine, centinaia o persino migliaia di macchine virtuali di Azure e per eliminarle quando non servono più. Il costo è conveniente e la fatturazione viene calcolata al minuto, quindi si paga solo per le risorse di calcolo usate e fino a quando le si usa. Esistono inoltre molti modi per configurare le macchine virtuali in modo da soddisfare esigenze specifiche.

::: zone pivot="windows-cloud"

Uno snapshot di una macchina virtuale in esecuzione viene chiamato _immagine_. Azure offre immagini per Windows e per varie versioni di Linux. È anche possibile creare immagini preconfigurate personalizzate per velocizzare le distribuzioni. Qui verranno descritte le procedure per attivare una macchina virtuale con Windows Server 2016 fornita da Microsoft.

::: zone-end

::: zone pivot="linux-cloud"

Uno snapshot di una macchina virtuale in esecuzione viene chiamato _immagine_. Azure offre immagini per Windows e per varie versioni di Linux. È anche possibile creare immagini preconfigurate personalizzate per velocizzare le distribuzioni. Qui verranno descritte le procedure per attivare una macchina virtuale con Ubuntu 16.04 fornita da Canonical.

::: zone-end

## <a name="what-defines-a-virtual-machine-on-azure"></a>Cosa definisce una macchina virtuale in Azure?

Una macchina virtuale è definita da numerosi fattori, tra cui dimensioni e posizione. Prima di attivare la macchina virtuale, verranno illustrati brevemente gli elementi coinvolti.

:::row:::
    :::column:::
        **Dimensioni**
    :::column-end:::
    :::column span="3"::: Le _dimensioni_ di una macchina virtuale ne definiscono la velocità del processore, la quantità di memoria, la quantità iniziale di spazio di archiviazione e la larghezza di banda di rete prevista. Alcune dimensioni includono anche hardware specialistico, come GPU per carichi intensivi di rendering della grafica e modifica di video.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Area**
    :::column-end:::
    :::column span="3"::: Azure è costituito da data center distribuiti in tutto il mondo. Un'_area_ è un set di data center di Azure in un'area geografica denominata. A ogni risorsa di Azure, incluse le macchine virtuali, viene assegnata un'area. Stati Uniti orientali ed Europa settentrionale sono esempi di aree.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Rete**
    :::column-end:::
    :::column span="3"::: Una _rete virtuale_ è una rete isolata logicamente in Azure. Ogni macchina virtuale in Azure è associata a una rete virtuale. Azure offre firewall a livello di cloud per le reti virtuali denominati _gruppi di sicurezza di rete_.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Gruppi di risorse**
    :::column-end:::
    :::column span="3"::: Le macchine virtuali e altre risorse cloud sono raggruppate in contenitori logici denominati _gruppi di risorse_. I gruppi vengono in genere usati per organizzare set di risorse distribuite insieme come parte di un'applicazione o un servizio. Si fa riferimento a un gruppo di risorse con il nome.
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a>Che cos'è Azure Cloud Shell?

Azure Cloud Shell è un'esperienza di riga di comando basata su browser per la gestione e lo sviluppo di risorse di Azure. Cloud Shell può essere paragonato a una console interattiva eseguita nel cloud.

Cloud Shell offre due esperienze tra cui scegliere: Bash e PowerShell. Entrambe includono l'interfaccia della riga di comando di Azure.

È possibile usare qualsiasi interfaccia di gestione di Azure, tra cui il portale di Azure, l'interfaccia della riga di comando di Azure e Azure PowerShell, per gestire qualsiasi tipo di macchina virtuale. Ai fini dell'apprendimento, si userà l'interfaccia della riga di comando di Azure per creare e gestire una macchina virtuale Windows o Linux.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="creating-resources-in-azure"></a>Creazione di risorse in Azure

In genere, per prima cosa si crea un _gruppo di risorse_ per contenere tutto ciò che è necessario creare. In questo modo è possibile gestire le macchine virtuali, i dischi, le interfacce di rete e gli altri elementi che costituiscono la soluzione come un'unità. È possibile usare l'interfaccia della riga di comando di Azure per creare un gruppo di risorse con il comando `az group create`. Sono necessari un parametro `--name` per assegnare un nome univoco al gruppo nella sottoscrizione e un parametro `--location` per indicare ad Azure l'area del mondo in cui si vogliono posizionare le risorse per impostazione predefinita.

Poiché si sta usando l'ambiente sandbox di Azure gratuito, non è necessario eseguire questo passaggio, ma si userà il gruppo di risorse creato in precedenza, **<rgn>[nome gruppo di risorse]</rgn>**.

## <a name="choosing-a-location"></a>Scelta di una località

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

::: zone pivot="windows-cloud"

## <a name="create-a-windows-vm"></a>Creare una macchina virtuale Windows

Si vedrà ora come preparare ed eseguire una macchina virtuale Windows.

1. In Cloud Shell, a destra della pagina, eseguire il comando `az vm create` seguente per creare la macchina virtuale. Il comando crea la macchina virtuale nella località "Stati Uniti orientali", ma è possibile scegliere una qualsiasi delle località elencate in precedenza ed è consigliabile sceglierne una vicina alla propria posizione.

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --location eastus \
      --admin-username azureuser
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    Anche se è possibile specificare la password di amministratore di Windows tramite il comando, qui verrà chiesto di immetterne una. Scegliere una password contenente almeno 12 caratteri con una combinazione di lettere maiuscole e minuscole, numeri e simboli. Non usare una password usata in altre posizioni.

    Per l'attivazione della macchina virtuale saranno necessari da quattro a cinque minuti. Confrontare questo tempo con quello che sarebbe necessario per acquistare, installare in rack e configurare un sistema nel proprio data center. La differenza è notevole.

Durante l'attesa è possibile esaminare il comando appena eseguito.

* Il nome della macchina virtuale è **myVM**. Questo nome identifica la macchina virtuale in Azure. Diventa anche il nome host interno, o nome computer, della macchina virtuale.
* Il gruppo di risorse, ovvero il contenitore logico della macchina virtuale, è denominato **<rgn>[nome gruppo di risorse sandbox]</rgn>**.
* **Win2016Datacenter** specifica l'immagine della macchina virtuale Windows Server 2016.
* **Standard_DS2_v2** indica le dimensioni della macchina virtuale. Queste dimensioni includono due CPU virtuali e 7 GB di memoria.
* Il nome utente e la password consentono di connettersi alla macchina virtuale in un secondo momento. Ad esempio, è possibile connettersi tramite Desktop remoto o Gestione remota Windows per usare e configurare il sistema.

Per impostazione predefinita, Azure assegna un indirizzo IP pubblico alla macchina virtuale. È possibile configurare una macchina virtuale in modo che sia accessibile da Internet o solo dalla rete interna.

È anche possibile guardare questo breve video su alcune delle opzioni disponibili per creare e gestire le macchine virtuali.

#### <a name="options-to-create-and-manage-vms"></a>Opzioni per creare e gestire le macchine virtuali

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

Quando la macchina virtuale è pronta vengono visualizzate informazioni su di essa. Ecco un esempio.

```json
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1E-1B-3B",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "104.211.9.245",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

::: zone-end

::: zone pivot="linux-cloud"

## <a name="create-a-linux-vm"></a>Creare una macchina virtuale Linux

Si vedrà ora come preparare ed eseguire una macchina virtuale Linux.

1. In Cloud Shell, a destra della pagina, eseguire il comando `az vm create` per creare la macchina virtuale. Il comando seguente crea la macchina virtuale nella località "Stati Uniti orientali", ma è possibile scegliere una qualsiasi delle località elencate in precedenza ed è consigliabile sceglierne una vicina alla propria posizione.

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --image UbuntuLTS \
      --location eastus \
      --size Standard_DS2_v2 \
      --generate-ssh-keys
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    Per l'attivazione della macchina virtuale saranno necessari un paio di minuti circa. Confrontare questo tempo con quello che sarebbe necessario per acquistare, installare in rack e configurare un sistema nel proprio data center. La differenza è notevole.

Durante l'attesa è possibile esaminare il comando appena eseguito.

* Il nome della macchina virtuale è **myVM**. Questo nome identifica la macchina virtuale in Azure. Diventa anche il nome host interno, o nome computer, della macchina virtuale.
* Il gruppo di risorse, ovvero il contenitore logico della macchina virtuale, è denominato **<rgn>[nome gruppo di risorse sandbox]</rgn>**.
* **UbuntuLTS** specifica l'immagine della macchina virtuale Ubuntu 16.04 LTS.
* **Standard_DS2_v2** indica le dimensioni della macchina virtuale. Queste dimensioni includono due CPU virtuali e 7 GB di memoria.
* L'opzione `--generate-ssh-keys` crea una coppia di chiavi SSH per consentire l'accesso alla macchina virtuale.

Per impostazione predefinita, Azure assegna un indirizzo IP pubblico alla macchina virtuale. È possibile configurare una macchina virtuale in modo che sia accessibile da Internet o solo dalla rete interna.

È anche possibile guardare questo breve video su alcune delle opzioni disponibili per creare e gestire le macchine virtuali.

#### <a name="options-to-create-and-manage-vms"></a>Opzioni per creare e gestire le macchine virtuali

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

Quando la macchina virtuale è pronta vengono visualizzate informazioni su di essa. Ecco un esempio.

```json
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

::: zone-end

## <a name="summary"></a>Riepilogo

È sufficiente conoscere alcuni concetti per riuscire ad attivare una macchina virtuale in Azure in pochi minuti. Molti di questi concetti, ad esempio le dimensioni della macchina virtuale e le regole del firewall, sono probabilmente già noti.

Successivamente si vedrà come installare un server Web nella macchina virtuale e configurare il server Web per gestire un sito Web di base.