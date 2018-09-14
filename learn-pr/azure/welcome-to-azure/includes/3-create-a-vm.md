Come un professionista IT, probabile che hanno esperienza in un'area specifica. Si supponga di essere un amministratore di archiviazione o esperto di virtualizzazione o forse si è concentrati sulle procedure di protezione più recenti. Se sei uno studente, si potrebbe comunque essere esplorazione cosa di maggior interesse.

::: zone pivot="windows-cloud"

Indipendentemente dal proprio ruolo, la maggior parte delle persone inizia con il cloud mediante la creazione di una macchina virtuale. Qui offriremo rapidamente una macchina virtuale che esegue Windows Server 2016.

::: zone-end

::: zone pivot="linux-cloud"

Indipendentemente dal proprio ruolo, la maggior parte delle persone inizia con il cloud mediante la creazione di una macchina virtuale. Qui verrà visualizzata una macchina virtuale che esegue Ubuntu 16.04.

::: zone-end

Esistono molti modi per creare una macchina virtuale in Azure. In questo caso, verrà visualizzata una macchina virtuale Windows o Linux usando un terminale interattivo chiamato Cloud Shell. Se si lavora dal terminale su base giornaliera, si conosce che ciò è spesso il modo più rapido per svolgere il lavoro.

::: zone pivot="windows-cloud"

> [!TIP]
> Preferisce Linux o per provare qualcosa di nuovo? Selezionare **Linux** dall'inizio di questa pagina per l'esecuzione di una macchina virtuale Linux.

::: zone-end

::: zone pivot="linux-cloud"

> [!TIP]
> Preferisce Windows o per provare qualcosa di nuovo? Selezionare **Windows** dall'inizio di questa pagina per l'esecuzione di una macchina virtuale Windows Server.

::: zone-end

È possibile esaminare alcuni termini di base e ottenere la prima macchina virtuale sia operativa.

## <a name="what-is-a-virtual-machine"></a>Che cos'è una macchina virtuale?

Una macchina virtuale o macchina virtuale, è un'emulazione software di un computer fisico. Poiché le macchine virtuali esistono come software, decine, centinaia o anche migliaia di macchine virtuali di Azure può essere generati in pochi minuti, quindi eliminato quando non servono più. Con la fatturazione al minuto a basso costo, si paga solo per le risorse di calcolo che usi, fintanto che li si stia usando. Inoltre, esistono diversi modi per configurare le macchine virtuali per soddisfare le esigenze.

::: zone pivot="windows-cloud"

Viene chiamato uno snapshot di una macchina virtuale in esecuzione un' _immagine_. Azure offre immagini per diverse versioni di Linux e Windows. È anche possibile creare le proprie immagini preconfigurate per eseguire le distribuzioni più velocemente. In questo caso verrà visualizzata una VM Windows Server 2016, fornito da Microsoft.

::: zone-end

::: zone pivot="linux-cloud"

Viene chiamato uno snapshot di una macchina virtuale in esecuzione un' _immagine_. Azure offre immagini per diverse versioni di Linux e Windows. È anche possibile creare le proprie immagini preconfigurate per eseguire le distribuzioni più velocemente. In questo caso verrà visualizzata una VM Ubuntu 16.04, fornito da Canonical.

::: zone-end

## <a name="what-defines-a-virtual-machine-on-azure"></a>Definisce una macchina virtuale in Azure?

Una macchina virtuale è definita da numerosi fattori, tra cui la posizione e dimensioni. Prima di connettere la macchina virtuale, questo articolo verranno brevemente illustrati gli elementi coinvolti.

:::row:::
    :::column:::
        **Dimensione**
    :::column-end:::
    ::: estensione della colonna = "3"::: una macchina virtuale _dimensioni_ definisce la velocità del processore, quantità di memoria, quantità iniziale di archiviazione e larghezza di banda di rete previsto. Alcune dimensioni includono anche hardware specializzato, ad esempio GPU per il rendering di grafiche complesse e modifica di video.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Area**
    :::column-end:::
    ::: estensione della colonna = "3"::: Azure è costituita da data center distribuiti in tutto il mondo. Oggetto _regione_ è un set di data center di Azure in una posizione geografica specifica. Ogni risorsa di Azure, tra cui macchine virtuali, viene assegnata un'area. Stati Uniti orientali ed Europa settentrionale sono esempi di aree.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Rete**
    :::column-end:::
    ::: estensione della colonna = "3"::: un' _rete virtuale_ è una rete isolata logicamente in Azure. Ogni macchina virtuale in Azure è associata a una rete virtuale. Azure offre i firewall a livello cloud per le reti virtuali denominate _gruppi di sicurezza di rete_.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Gruppi di risorse**
    :::column-end:::
    ::: estensione della colonna = "3"::: le macchine virtuali e altre risorse cloud sono raggruppati in contenitori logici denominati _gruppi di risorse_. Gruppi vengono in genere usati per organizzare set di risorse che sono distribuite insieme come parte di un'applicazione o servizio. Fare riferimento a un gruppo di risorse in base al nome.
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a>Che cos'è Azure Cloud Shell?

Azure Cloud Shell è un'esperienza da riga di comando basata su browser per la gestione e sviluppo di risorse di Azure. Cloud Shell può essere paragonato a una console interattiva che si esegue nel cloud.

Cloud Shell offre due esperienze di scegliere tra: Bash e PowerShell. Entrambi includono l'accesso per il comando di Azure, l'interfaccia della riga di comando di Azure.

È possibile utilizzare qualsiasi interfaccia di gestione di Azure, tra cui il portale di Azure CLI di Azure e Azure PowerShell, per gestire qualsiasi tipo di macchina virtuale. Ai fini dell'apprendimento, in questo caso si userà la CLI di Azure per creare e gestire VM Linux o Windows.

::: zone pivot="windows-cloud"

## <a name="create-a-windows-vm"></a>Creare una macchina virtuale Windows

[!include[](../../../includes/azure-sandbox-activate.md)]

È possibile ottenere la macchina virtuale Windows in esecuzione.

<!--

TODO: Omitted for sandbox. Possibly re-insert later for non-sandbox workflow.

1. From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```
-->

1. Da Cloud Shell sul lato destro della pagina, eseguire il `az vm create` comando per creare la macchina virtuale. Si consiglia di che modificare la password su uno in un secondo momento si ricorderà illustrata di seguito.

      > [!NOTE]
    > Scegliere una password contenente almeno 8 caratteri con una combinazione di maiuscole e lettere minuscole, numeri e simboli.

    ```azurecli
    az vm create \
      --name myWindowsVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --admin-username azureuser \
      --admin-password "Password1234&"
    ```

    > [!TIP]
    > È possibile usare la **copia** per copiare ogni comando. Per incollare il codice, fare clic con il pulsante destro sulla nuova riga nella finestra della Cloud Shell e selezionare **incollare**.

    La macchina virtuale richiederà da quattro a cinque minuti per avviarsi. Confronta con il tempo che necessario per acquistare, installare in rack e configurare un sistema nel data center. Una differenza.

Durante l'attesa, è opportuno esaminare i comandi che sono stati eseguiti.

* La VM è denominata **myWindowsVM**. Questo nome identifica la macchina virtuale in Azure. Diventa anche il nome host interno della macchina virtuale o nome del computer.
* Il gruppo di risorse o un contenitore logico della VM è denominata  **<rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>**.
* **Win2016Datacenter** specifica l'immagine di macchina virtuale di Windows Server 2016.
* **Standard_DS2_v2** indicano le dimensioni della macchina virtuale. Questa dimensione dispone di due CPU e 7 GB di memoria virtuale.
* Il nome utente e la password per potersi connettere alla macchina virtuale in un secondo momento. Ad esempio, è possibile connettersi tramite Desktop remoto o gestione remota Windows per usare e configurare il sistema.

Per impostazione predefinita, Azure assegna un indirizzo IP pubblico alla macchina virtuale. È possibile configurare una macchina virtuale per essere accessibile da Internet o solo dalla rete interna.

È anche possibile leggere questo breve video su alcune delle opzioni che disponibili per creare e gestire le macchine virtuali.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

Quando la macchina virtuale è pronta, noterete che le relative informazioni. Ecco un esempio.

```console
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myWindowsVM",
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

[!include[](../../../includes/azure-sandbox-activate.md)]

È possibile ottenere la VM Linux in esecuzione.

<!--

TODO: Omitted for sandbox. Possibly re-insert later for non-sandbox workflow.
 
1. From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```
-->

1. Da Cloud Shell sul lato destro della pagina, eseguire il `az vm create` comando per creare la macchina virtuale.

    ```azurecli
    az vm create \
      --name myLinuxVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image UbuntuLTS \
      --size Standard_DS2_v2 \
      --generate-ssh-keys
    ```

    > [!TIP]
    > È possibile usare la **copia** per copiare ogni comando. Per incollare il codice, fare clic con il pulsante destro sulla nuova riga nella finestra della Cloud Shell e selezionare **incollare**.

    La macchina virtuale richiederà circa due minuti per avviarsi. Confronta con il tempo che necessario per acquistare, installare in rack e configurare un sistema nel data center. Una differenza.

Durante l'attesa, è opportuno esaminare i comandi che sono stati eseguiti.

* La VM è denominata **myLinuxVM**. Questo nome identifica la macchina virtuale in Azure. Diventa anche il nome host interno della macchina virtuale o nome del computer.
* Il gruppo di risorse o un contenitore logico della VM è denominata  **<rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>**.
* **UbuntuLTS** specifica l'immagine di macchina virtuale di Ubuntu 16.04 LTS.
* **Standard_DS2_v2** indicano le dimensioni della macchina virtuale. Questa dimensione dispone di due CPU e 7 GB di memoria virtuale.
* Il `--generate-ssh-keys` opzione Crea una coppia di chiavi SSH per consentire di accedere alla macchina virtuale.

Per impostazione predefinita, Azure assegna un indirizzo IP pubblico alla macchina virtuale. È possibile configurare una macchina virtuale per essere accessibile da Internet o solo dalla rete interna.

È anche possibile leggere questo breve video su alcune delle opzioni che disponibili per creare e gestire le macchine virtuali.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

Quando la macchina virtuale è pronta, noterete che le relative informazioni. Ecco un esempio.

```console
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myLinuxVM",
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

Con pochi concetti disposizione, si è in grado di creare una macchina virtuale in Azure in pochi minuti. Molti di questi concetti, ad esempio le dimensioni della macchina virtuale e regole del firewall sono già probabilmente familiari per l'utente.

Successivamente, si verrà installare un server web nella VM e configurare il server web per gestire un sito web di base.