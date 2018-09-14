Il server deve risorse sufficienti per gestire la domanda giornaliera. Una tipica strategia consiste nel scegliere una dimensione di VM in fase di creazione che è sufficiente per i carichi di lavoro tipici e quindi ridimensionarlo al mutare delle esigenze.

Nello scenario aziendale di giocattoli, questa strategia potrebbe essere utile per gestire le risorse per la crescita medio termine. È possibile aumentare le dimensioni della VM per gestire la richiesta di aggiunta man mano che aumenta l'azienda.

## <a name="what-is-virtual-machine-size"></a>Che cos'è di dimensioni della macchina virtuale?

Il _dimensioni_ di una macchina virtuale è una misura del relativo della CPU, memoria, disco e larghezza di banda di rete previsto. Sono disponibili in un numero predeterminato di dimensioni delle macchine virtuali. Ad esempio, il **Standard_F32s_v2** dimensioni con 32 CPU virtuali, 64 GiB di memoria, un disco SSD locale GiB 256 e 14.000 Mbps di larghezza di banda di rete previsto.

Quando si crea una nuova macchina virtuale in Azure, è necessario scegliere una dimensione. Dimensioni maggiori hanno costi maggiori. L'obiettivo consiste nello scegliere una dimensione in grado di gestire il carico di lavoro non si configurano più energia maggiore del necessario.

## <a name="what-is-virtual-machine-type"></a>Che cos'è il tipo di macchina virtuale?

Il _tipo_ di una macchina virtuale è il carico di lavoro per cui è stata ottimizzata la macchina virtuale. Ad esempio, alcune macchine virtuali sono destinati a attività a elevato utilizzo della CPU, ad esempio un server web di hosting. Altri sono destinati i processi incentrati su archiviazione simile all'esecuzione di un database.

Esistono _tipi_ che corrispondono a ciascun componente hardware di core in un computer moderni: **compute**, **memoria**, **archiviazione**e  **GPU**. È inoltre disponibile un' **generico** digitare se è necessaria una combinazione bilanciata delle risorse. Nella tabella seguente sono elencati i tipi e le dimensioni di VM che fanno parte di ogni tipo, insieme a una breve descrizione del carico di lavoro di destinazione.

|Tipo|Dimensioni|Descrizione|
|---|---|---|
|Utilizzo generico|B, Ds_v3, D_v3, some DS_v2, some D_v2, A_v2|Le macchine per utilizzo generico hanno un rapporto CPU-memoria equilibrato. Per utilizzo generico macchine sono ideali per test o sviluppo server, database medio-piccoli o un server web con bassa a livelli medi di traffico.|
|Ottimizzato per il calcolo|Fs_v2, Fs, F|Ottimizzate per il calcolo macchine virtuali hanno un rapporto tra CPU e memoria superiore rispetto alle macchine per utilizzo generico, per le attività che richiedono potenza di elaborazione aggiuntivi, ad esempio i server applicazioni, i dispositivi di rete o server web con traffico medio.|
|Ottimizzato per la memoria|Es_v3, E_v3, M, GS, G, alcuni DS_v2, alcuni D_v2|Le macchine virtuali ottimizzate per la memoria hanno un rapporto elevato tra memoria e CPU. Queste macchine sono ideali per i server di database relazionali, i server che richiedono o eseguono una grande quantità di memorizzazione nella cache o i server che eseguono analitica in memoria.|
|Ottimizzato per l'archiviazione|Ls|Queste macchine virtuali sono configurate per la velocità effettiva del disco elevata e le operazioni dei / o in base alle proprie dei big data, SQL e i database NoSQL.|
|GPU|NV, NC, NC_v2, NC_v3, ND|Le macchine virtuali GPU sono specializzate per le attività, ad esempio di rendering della grafica robusto alloggiamento o modifica di video, insieme a training del modello e l'inferenza dei (serie ND) con apprendimento profondo. È possibile scegliere unica o più GPU per tali macchine.|
|High Performance Computing (HPC)|H|CPU più potenti più rapidamente, sono disponibili in queste macchine virtuali. È anche possibile aggiungere interfacce di rete e velocità effettiva elevata (RDMA).|

## <a name="clusters"></a>Cluster

L'hardware del server fisico in aree di Azure sia raggruppato in cluster. Ogni cluster può supportare diverse dimensioni di macchina virtuale diversa in base all'hardware fisico.

Quando si crea una macchina virtuale e sceglie una dimensione specifica, il provisioning della macchina virtuale a un cluster di hardware appropriato per tale dimensione. Anche se è possibile ridimensionare le macchine virtuali dopo la creazione, le opzioni di ridimensionamento possono essere limitate dal cluster hardware scelto per le dimensioni iniziali.

## <a name="what-is-vertical-scaling"></a>Che cos'è il ridimensionamento verticale?

_Il ridimensionamento verticale_ è il processo di modifica il _dimensioni_ di una macchina virtuale. È possibile _aumentare le prestazioni_ scegliendo una dimensione più potente per gestire un aumento della domanda oppure _ridurle_ allocare un numero di risorse e ridurre i costi. La figura seguente mostra un esempio di modifica delle dimensioni di una macchina virtuale.

![Un'illustrazione che mostra la scalabilità verticale e scalabilità verso il basso di una macchina virtuale per modificare le funzionalità delle prestazioni.](../media/2-ScaleUpDown.png)

È possibile ridimensionare una VM usando il portale di Azure, Azure PowerShell o l'interfaccia CLI di Azure.

### <a name="resize-in-the-portal"></a>Ridimensiona nel portale

Nel portale di Azure, è possibile ridimensionare una macchina virtuale selezionando la macchina virtuale, fare clic sui **dimensioni** voce e selezionare una voce dalle **Scegli una dimensione** pannello. 

Se la macchina virtuale è in esecuzione al momento, le dimensioni disponibili che è possibile selezionare variano in base le dimensioni disponibili nella propria area. Viene visualizzata solo ridimensionare opzioni compatibile con stesso cluster hardware che la macchina virtuale è attualmente in esecuzione; è talvolta definito una *dimensioni famiglia*. Se si sceglie una nuova dimensione mentre è in esecuzione la macchina virtuale, la macchina virtuale verrà riavviata automaticamente per applicare le nuove dimensioni.

Se la dimensione desiderata non è visibile nel portale quando la macchina virtuale è in esecuzione, è possibile arrestare la macchina virtuale per visualizzare altre opzioni. Quando la macchina è il **arrestata (deallocata)** lo stato, sarà possibile selezionare le dimensioni da altri componenti hardware nella stessa area.

### <a name="resize-with-powershell"></a>Ridimensionare con PowerShell

È possibile usare PowerShell per eseguire la scalabilità verticale in modo interattivo o tramite script. Gli script sono buone per scenari complessi; ad esempio, se è necessario ridimensionare le macchine virtuali diverse in una sola volta. Possono inoltre essere utile se è necessario eseguire il ridimensionamento durante orari non lavorativi per evitare interruzioni dell'utente.

Il cmdlet seguente elenca le dimensioni VM della stessa famiglia di dimensioni come hardware corrente:

```PowerShell
Get-AzureRmVMSize -ResourceGroupName "myResourceGroup" -VMName "MyVM"
```

Se viene visualizzata la dimensione desiderata, usare il cmdlet seguente per modificare le dimensioni di macchina virtuale:

```PowerShell
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "MyVM"
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
```

Se la dimensione desiderata non viene visualizzata con la macchina in esecuzione, usare i comandi seguenti per deallocare la macchina virtuale, ridimensionare la macchina e riavviare il computer:

```PowerShell
Stop-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "MyVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "MyVM"
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "MyVM"
```

Macchine virtuali in Azure può essere ridimensionate in base alle esigenze per migliorare le prestazioni o ridurre i costi. Esecuzione manuale, il ridimensionamento con il portale o uno script, è utile per la crescita del business graduale handle o per sapere quando su una modifica della domanda anticipatamente. Nello scenario aziendale di giocattoli, cui è stato possibile aumentare le prestazioni prima di un giorno festivo per gestire il picco della domanda, quindi ridurle in un secondo momento.
