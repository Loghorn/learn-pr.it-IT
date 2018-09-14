La creazione di script di amministrazione rappresenta un potente strumento per ottimizzare il flusso di lavoro. È possibile automatizzare attività comuni e ripetitive. Dopo aver verificato uno script, verrà eseguita in modo coerente, che verranno probabilmente ridurre gli errori. Nell'esercizio precedente, abbiamo creato una macchina virtuale, aggiungere un disco dati e modificato le impostazioni della cache, tutto tramite il portale di Azure. Cosa accade se è stato necessario ripetere queste attività in più macchine virtuali, in molte aree? È possibile usare Azure PowerShell.

## <a name="what-is-azure-powershell"></a>Informazioni su Azure PowerShell.

Azure PowerShell è un modulo che si aggiunta a Windows PowerShell per consentire a è connettersi alla sottoscrizione di Azure e gestire le risorse. Azure PowerShell richiede Windows PowerShell alla funzione. Windows PowerShell fornisce servizi come la finestra della shell, comandi di analisi e così via. Azure PowerShell aggiunge i comandi specifici di Azure.

Ad esempio, Azure PowerShell fornisce il **New-AzureRmVM** comando che crea una macchina virtuale per l'utente all'interno della sottoscrizione di Azure. Per usarlo, si potrebbe avviare l'applicazione di PowerShell e quindi eseguire un comando simile all'esempio:

```powershell
New-AzureRmVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Azure PowerShell sono disponibili due modi: in un browser tramite Azure Cloud Shell o con un'installazione locale in Windows, Mac o Linux.

## <a name="what-are-powershell-cmdlets"></a>Che cosa sono i cmdlet di PowerShell?

Un comando di PowerShell viene denominato **cmdlet** (pronunciato "command-let"). Un cmdlet è un comando che manipola una singola funzione. Il termine **cmdlet** deve implica di "piccole dimensioni comando." Per convenzione, gli autori di cmdlet sono invitati a mantenere i cmdlet semplici e dedicati a un unico scopo.

Il prodotto PowerShell di base viene fornito con cmdlet utilizzabili per funzionalità quali sessioni e processi in background. È possibile aggiungere moduli all'installazione di PowerShell per ottenere cmdlet in grado di manipolare altre funzionalità. Esistono ad esempio moduli di terze parti per l'uso di connessioni FTP, l'amministrazione del sistema operativo, l'accesso al file system e così via.

I cmdlet seguono una convenzione di denominazione verbo-sostantivo, ad esempio **Get-Process**, **Format-Table** e **Start-Service**. È inoltre disponibile una convenzione per la scelta di verbo: "get" recuperare i dati, "set" per inserire o aggiornare dati, "format" per formattare i dati, "out" all'output diretto a una destinazione e così via.

Gli autori di cmdlet sono invitati a includere un file della Guida per ciascun cmdlet. Il cmdlet **Get-Help** consente di visualizzare il file della Guida per qualsiasi cmdlet:

```powershell
Get-Help <cmdlet-name> -detailed
```

## <a name="what-is-azurerm"></a>Che cos'è AzureRM?

**AzureRM** è il nome formale per il modulo Azure PowerShell che include un set di cmdlet per lavorare con le funzionalità di Azure (la **RM** il nome è l'acronimo **Resource Manager**). Contiene centinaia di cmdlet che consentono di controllare praticamente ogni aspetto di ogni risorsa di Azure. È possibile lavorare con i gruppi di risorse, l'archiviazione, le macchine virtuali, Azure Active Directory, i contenitori, Machine Learning e così via.

## <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>Cmdlet di PowerShell per la gestione dei dischi di Azure caching

Azure PowerShell sono disponibili cmdlet specifici per gestire le macchine virtuali e dischi.

Nella tabella seguente sono elencati alcuni dei cmdlet che verrà usato nell'esercizio successivo:

|Comando  |Descrizione  |
|---------|---------|
|`Get-AzureRmVM`     |  Ottiene le proprietà di una macchina virtuale.       |
|`Update-AzureRmVM`     |  Aggiorna lo stato della macchina virtuale di Azure.       |
|`New-AzureRmDiskConfig`     |  Crea un oggetto disco configurabile.       |
|`Add-AzureRmVMDataDisk`     |  Aggiunge un disco dati a una macchina virtuale.   |

È possibile usare questi cmdlet nel prossimo esercizio per gestire la memorizzazione nella cache nella VM.
