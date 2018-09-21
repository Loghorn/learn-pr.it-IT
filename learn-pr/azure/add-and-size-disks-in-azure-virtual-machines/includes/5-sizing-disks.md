Azure archivia le immagini dei dischi rigidi virtuali come BLOB di pagine in un account di archiviazione di Azure. Con i dischi gestiti, Azure si occupa di gestire la risorsa di archiviazione per conto dell'utente: uno dei motivi migliori per scegliere i dischi gestiti.

Quando viene creata, la VM sceglie una dimensione per il disco del sistema operativo. La dimensione specifica si basa sull'immagine selezionata. In Linux è spesso di circa 30 GB, in Windows di circa 127 GB.

È possibile aggiungere dischi dati per offrire spazio di archiviazione aggiuntivo, ma è anche possibile espandere un disco esistente, ad esempio se un'applicazione legacy non può suddividere i dati in unità o si esegue la migrazione dell'unità di un computer fisico ad Azure ed è necessaria un'unità del sistema operativo di dimensioni maggiori.

> [!NOTE]
> È possibile ridimensionare un disco solo per _aumentarne_ le dimensioni. La riduzione dei dischi gestiti non è attualmente supportata.

La modifica delle dimensioni del disco può anche cambiare il livello del disco (ad esempio, da P10 a P20). Questa condizione può risultare utile per gli aggiornamenti delle prestazioni, ma comporta costi più elevati passando ai livelli premium.

## <a name="vm-size-vs-disk-size"></a>Dimensioni della VM e dimensioni del disco

La dimensione di VM scelta in fase di creazione determina il numero di risorse che può allocare. Per l'archiviazione, le dimensioni consentono di controllare il numero di dischi che è possibile aggiungere alla VM e le dimensioni massime di ogni disco. 

Come indicato nell'unità precedente, alcune dimensioni di VM supportano solo unità di archiviazione Standard, con limiti per le prestazioni I/O.

Se si ritiene di aver bisogno di più spazio di archiviazione rispetto a ciò che consentono le dimensioni della VM, è possibile modificare queste ultime. Questo argomento è illustrato nel modulo **Introduzione alle macchine virtuali di Azure**.

## <a name="expanding-a-disk-using-the-azure-cli"></a>Espansione di un disco con l'interfaccia della riga di comando di Azure

> [!WARNING]
> Assicurarsi sempre di eseguire il backup dei dati prima di eseguire operazioni di ridimensionamento dei dischi.

Non è possibile eseguire operazioni sui dischi rigidi virtuali quando la macchina virtuale è in esecuzione. Il primo passaggio è quello di arrestare e deallocare la VM con `az vm deallocate` fornendo il nome della VM e del gruppo di risorse.

La deallocazione di una VM, a differenza del suo semplice _arresto_, rilascia le risorse di calcolo associate e consente ad Azure di apportare modifiche di configurazione all'hardware virtualizzato.

```azurecli
az vm deallocate --resource-group <resource-group-name> --name <vm-name>
```

Successivamente, per ridimensionare un disco, usare `az disk update`, passando il nome del disco, il nome del gruppo di risorse e le nuove dimensioni richieste. Quando si espande un disco gestito, si esegue il mapping delle dimensioni specificate alle dimensioni del disco gestito più vicino.

```azurecli
az disk update \
    --resource-group <resource-group-name> \
    --name <disk-name> \
    --size-gb 200
```

Infine, si avvia nuovamente la VM con `az vm start`:

```azurecli
az vm start --resource-group <resource-group-name> --name <vm-name>
```

## <a name="expanding-a-disk-using-the-azure-portal"></a>Espansione di un disco con il portale di Azure

L'espansione di un disco con il portale di Azure è ancora più semplice.

1. Arrestare la VM usando il pulsante **Arresta** sulla barra degli strumenti nella vista **Panoramica** della VM.

1. Fare clic su **Dischi** nella sezione **Impostazioni**.

1. Selezionare il disco dati che si intende ridimensionare.

    ![Screenshot che mostra la sezione dei dischi di una VM con evidenziato il disco rigido virtuale da modificare](../media/5-portal-disks.png)

1. Nei dettagli del disco digitare una dimensione _maggiore_ rispetto alla dimensione corrente. Qui è anche possibile passare dalla versione Premium alla versione Standard (o viceversa). Queste impostazioni regolano le prestazioni in base a quanto riportato nella sezione delle operazioni di I/O al secondo stimate.

    ![Screenshot che mostra la schermata di modifica del disco rigido virtuale con evidenziato il campo della nuova dimensione](../media/5-resize-disk.png)

1. Fare clic su **Salva** per salvare le modifiche.

1. Riavviare la VM.


### <a name="expanding-the-partition"></a>Espansione della partizione

Esattamente come per l'aggiunta di un nuovo disco dati, un disco espanso non aggiunge spazio utilizzabile fino a quando non si espandono la partizione e il file system. Questa operazione deve essere eseguita mediante gli strumenti del sistema operativo disponibili per la VM. 

In Windows usiamo lo strumento di gestione dischi o lo strumento della riga di comando `diskpart`.

In Linux usiamo `parted` e `resize2fs`.

Proviamo alcuni di questi comandi.