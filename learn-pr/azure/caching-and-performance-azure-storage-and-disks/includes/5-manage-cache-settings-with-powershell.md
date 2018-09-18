La creazione di script di amministrazione rappresenta un potente strumento per ottimizzare il flusso di lavoro. È possibile automatizzare attività comuni e ripetitive. Dopo che uno script è stato verificato, verrà eseguito in modo coerente, con la probabilità di ridurre gli errori. Nell'esercizio precedente è stata creata una macchina virtuale, è stato aggiunto un disco dati e sono state modificate le impostazioni della cache, il tutto tramite il portale di Azure. Come fare per ripetere queste attività tra più macchine virtuali in molte aree? È possibile usare Azure PowerShell.

## <a name="what-is-azure-powershell"></a>Che cos'è Azure PowerShell?
Azure PowerShell è un modulo che viene aggiunto a PowerShell per consentire di connettersi alla sottoscrizione di Azure e gestire le risorse. Per il funzionamento di Azure PowerShell è necessario PowerShell. PowerShell fornisce servizi come la finestra della shell, l'analisi dei comandi e così via. Azure PowerShell aggiunge i comandi specifici di Azure.

Ad esempio, Azure PowerShell fornisce il comando **New-AzureRmVM**, che crea una macchina virtuale all'interno della sottoscrizione di Azure. Per usarlo, è necessario avviare l'applicazione PowerShell e quindi eseguire un comando simile all'esempio:

```powershell
New-AzureRmVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Azure PowerShell è disponibile in due modalità: all'interno di un browser tramite Azure Cloud Shell o con un'installazione locale in Windows, Mac o Linux.

## <a name="what-are-powershell-cmdlets"></a>Che cosa sono i cmdlet di PowerShell?

Un comando di PowerShell viene denominato **cmdlet** (pronunciato "command-let"). Un cmdlet è un comando che manipola una singola funzione. Il termine **cmdlet** è usato per indicare un "piccolo comando". Per convenzione, gli autori di cmdlet sono invitati a mantenere i cmdlet semplici e dedicati a un unico scopo.

Il prodotto PowerShell di base viene fornito con cmdlet utilizzabili per funzionalità quali sessioni e processi in background. È possibile aggiungere moduli all'installazione di PowerShell per ottenere cmdlet in grado di manipolare altre funzionalità. Esistono ad esempio moduli di terze parti per l'uso di connessioni FTP, l'amministrazione del sistema operativo, l'accesso al file system e così via.

I cmdlet seguono una convenzione di denominazione verbo-sostantivo, ad esempio **Get-Process**, **Format-Table** e **Start-Service**. Esiste inoltre una convenzione per la scelta del verbo: "get" per il recupero di dati, "set" per l'inserimento o l'aggiornamento di dati, "format" per la formattazione di dati, "out" per l'indirizzamento dell'output a una destinazione e così via.

Gli autori di cmdlet sono invitati a includere un file della Guida per ciascun cmdlet. Il cmdlet **Get-Help** consente di visualizzare il file della Guida per qualsiasi cmdlet:

```
Get-Help <cmdlet-name> -detailed
```
## <a name="what-is-azurerm"></a>Che cos'è AzureRM?

**AzureRM** è il nome formale per il modulo di Azure PowerShell che offre un set di cmdlet per l'uso delle funzionalità di Azure (**RM** nel nome è l'acronimo di **Resource Manager**). Contiene centinaia di cmdlet che consentono di controllare praticamente ogni aspetto di ogni risorsa di Azure. È possibile lavorare con i gruppi di risorse, l'archiviazione, le macchine virtuali, Azure Active Directory, i contenitori, Machine Learning e così via.

## <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>Cmdlet di PowerShell per la gestione della memorizzazione nella cache del disco di Azure

Azure PowerShell offre cmdlet specifici per la gestione di macchine virtuali e dischi. 

La tabella seguente elenca alcuni dei cmdlet che verranno usati nell'esercizio successivo.

|Comando  |Descrizione  |
|---------|---------|
|`Get-AzureRmVM`     |  Ottiene le proprietà di una macchina virtuale.       |        $myVM
|`Update-AzureRmVM`     |  Aggiorna lo stato di una macchina virtuale di Azure.       |        
|`New-AzureRmDiskConfig`     |  Crea un oggetto disco configurabile.       |        
|`Add-AzureRmVMDataDisk`     |  Aggiunge un disco dati a una macchina virtuale.   |      


Questi cmdlet verranno usati nel prossimo esercizio per gestire la memorizzazione nella cache nella macchina virtuale.
