È necessario ridimensionare le macchine virtuali in base al carico di lavoro previsto. Una macchina virtuale senza la corretta quantità di memoria o CPU non riesce a funzionare in condizioni di carico o può risultare troppo lenta e quindi poco efficiente. 

## <a name="pre-defined-vm-sizes"></a>Dimensioni di VM predefinite

Quando si crea una macchina virtuale, è possibile specificare un valore per le _dimensioni di macchina virtuale_ in base al quale verrà determinata la quantità di risorse di calcolo da dedicare alla macchina virtuale. Tra queste risorse sono incluse la CPU, la GPU e la memoria che vengono rese disponibili da Azure per la macchina virtuale.

Azure definisce un set di dimensioni di macchina virtuale predefinite per Linux e Windows che è possibile scegliere in base all'utilizzo previsto. 

| Tipo | Dimensioni | Descrizione |
|------|-------|-------------|
| Utilizzo generico   | Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7 | Rapporto equilibrato tra CPU e memoria. Soluzione ideale per sviluppo/test e soluzioni di dati e applicazioni medio-piccole. |
| Ottimizzate per il calcolo | Fs, F | Rapporto elevato tra CPU e memoria. Soluzione idonea per applicazioni con livelli medi di traffico, appliance di rete e processi batch. |
| Ottimizzate per la memoria  | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Rapporto elevato tra memoria e core. Soluzione ideale per database relazionali, cache medio-grandi e analisi in memoria. |
| Ottimizzate per l'archiviazione | Ls | I/O e velocità effettiva del disco elevati. Soluzione ideale per Big Data, database SQL e NoSQL. |
| Ottimizzate per la GPU | NV, NC | Macchine virtuali specializzate ottimizzate per livelli intensivi di rendering della grafica ed editing di video. |
| Prestazioni elevate | H, A8-11 | Le macchine virtuali con CPU più potenti, con interfacce di rete ad alta velocità effettiva opzionali (RDMA). | 

Le dimensioni disponibili variano in base all'area in cui si crea la macchina virtuale. È possibile ottenere un elenco delle dimensioni disponibili tramite il comando `vm list-sizes`. Provare a digitare quanto segue in Azure Cloud Shell:

```azurecli
az vm list-sizes --location eastus --output table
```

Ecco una risposta abbreviata per `eastus`:

```
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          2048  Standard_B1ms                         1           1047552                    4096
                 2          1024  Standard_B1s                          1           1047552                    2048
                 4          8192  Standard_B2ms                         2           1047552                   16384
                 4          4096  Standard_B2s                          2           1047552                    8192
                 8         16384  Standard_B4ms                         4           1047552                   32768
                16         32768  Standard_B8ms                         8           1047552                   65536
                 4          3584  Standard_DS1_v2                       1           1047552                    7168
                 8          7168  Standard_DS2_v2                       2           1047552                   14336
                16         14336  Standard_DS3_v2                       4           1047552                   28672
                32         28672  Standard_DS4_v2                       8           1047552                   57344
                64         57344  Standard_DS5_v2                      16           1047552                  114688
        ....
                64       3891200  Standard_M128-32ms                  128           1047552                 4096000
                64       3891200  Standard_M128-64ms                  128           1047552                 4096000
                64       3891200  Standard_M128ms                     128           1047552                 4096000
                64       2048000  Standard_M128s                      128           1047552                 4096000
                64       1024000  Standard_M64                         64           1047552                 8192000
                64       1792000  Standard_M64m                        64           1047552                 8192000
                64       2048000  Standard_M128                       128           1047552                16384000
                64       3891200  Standard_M128m                      128           1047552                16384000
```

## <a name="specifying-a-size-during-vm-creation"></a>Specifica delle dimensioni durante la creazione della macchina virtuale

Al momento della creazione della macchina virtuale non è stata specificata una dimensione. Azure ha pertanto selezionato automaticamente una dimensione predefinita per utilizzo generico, ovvero `Standard_DS1_v2`. È tuttavia possibile specificare la dimensione come parte del comando `vm create` usando il parametro `--size`. Per creare una macchina virtuale con 16 core, ad esempio, è possibile usare il comando seguente:

```azurecli
az vm create --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM2 \
  --image Debian --admin-username aldis --generate-ssh-keys --verbose \
  --size "Standard_DS5_v2"
```

> [!WARNING]
> Il livello di sottoscrizione [applica i limiti](https://docs.microsoft.com/azure/azure-subscription-service-limits) al numero di risorse che è possibile creare e alle dimensioni totali di queste risorse. Ad esempio, il limite massimo è di **20 CPU virtuali** per la sottoscrizione con pagamento in base al consumo e di solo **4 vCPU** per il livello gratuito. Quando il limite viene superato, l'interfaccia della riga di comando di Azure visualizza l'errore **Quota superata**. Se viene visualizzato questo errore nella sottoscrizione a pagamento, è possibile richiedere l'aumento dei limiti associati alla sottoscrizione a pagamento (fino a 10.000 vCPU) inviando una [richiesta online gratuita](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

## <a name="resizing-an-existing-vm"></a>Ridimensionamento di una macchina virtuale esistente
È anche possibile ridimensionare una macchina virtuale esistente se si verifica una variazione del carico di lavoro o se non è stata definita la dimensione corretta in fase di creazione. Prima di richiedere un ridimensionamento, è necessario verificare se la dimensione desiderata è disponibile nel cluster di cui fa parte la macchina virtuale. A questo scopo, usare il comando `vm list-vm-resize-options`:

```azurecli
az vm list-vm-resize-options --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --output table
```

Verrà restituito l'elenco di tutte le possibili configurazioni di dimensione che sono disponibili nel gruppo di risorse. Se la dimensione desiderata non è disponibile nel cluster, ma _è_ disponibile nell'area, è possibile [deallocare la macchina virtuale](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-deallocate). Questo comando arresta la macchina virtuale in esecuzione e la rimuove dal cluster corrente senza che si verifichi alcuna perdita di risorse. È quindi possibile ridimensionarla, creando nuovamente la macchina virtuale in un altro cluster in cui è disponibile la configurazione di dimensione desiderata.

> [!NOTE]
> L'ambiente sandbox di Azure è limitato ad alcune dimensioni di macchina virtuale. La maggior parte delle possibilità non è consentita nella sottoscrizione di Microsoft Learn gratuita.

Per ridimensionare una macchina virtuale si usa il comando `vm resize`. Se ad esempio si riscontra che la macchina virtuale è sottostimata per l'attività che deve eseguire, la si può incrementare di alcuni livelli, fino a un livello DS3_v2 in cui ha 4 vCore e 14 GB di memoria. Digitare questo comando in Cloud Shell:

```azurecli
az vm resize --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --size Standard_DS3_v2
```

Il comando impiegherà alcuni minuti per ridurre le risorse della macchina virtuale e al termine dell'esecuzione restituirà una nuova configurazione JSON.