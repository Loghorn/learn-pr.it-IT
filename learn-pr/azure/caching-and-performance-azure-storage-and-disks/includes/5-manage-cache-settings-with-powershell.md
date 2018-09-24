La creazione di script di amministrazione rappresenta un potente strumento per ottimizzare il flusso di lavoro. È possibile automatizzare attività comuni e ripetitive. Dopo che uno script è stato verificato, verrà eseguito in modo coerente, con la probabilità di ridurre gli errori. Nell'esercizio precedente è stata creata una macchina virtuale, è stato aggiunto un disco dati e sono state modificate le impostazioni della cache, il tutto tramite il portale di Azure. Come fare per ripetere queste attività su più macchine virtuali in molte aree? È possibile farlo con Azure PowerShell.

> [!TIP]
> Azure PowerShell è illustrato in dettaglio nel modulo **Automatizzare le attività di Azure con PowerShell**. Assicurarsi di consultare tale modulo per altri dettagli sull'installazione, la configurazione e l'uso di PowerShell.

## <a name="what-is-azure-powershell"></a>Che cos'è Azure PowerShell?

Azure PowerShell è uno strumento da riga di comando multipiattaforma per connettersi alla sottoscrizione di Azure e gestire le risorse. È la combinazione di due elementi, ovvero **PowerShell**, che fornisce il supporto dello strumento da riga di comando, e **AzureRM** che fornisce i comandi (detti "cmdlet") utilizzabili con Azure. 

Azure PowerShell include cmdlet per modificare la maggior parte degli aspetti delle risorse di Azure. È possibile lavorare con i gruppi di risorse, l'archiviazione, le macchine virtuali, Azure Active Directory, i contenitori, Machine Learning e così via. Tutti questi dettagli sono oggetto di altri moduli di apprendimento.

### <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>Cmdlet di PowerShell per la gestione della memorizzazione nella cache del disco di Azure

Azure PowerShell offre cmdlet specifici per la gestione di macchine virtuali e dischi.

|Comando  | Descrizione |
|---------|-------------|
| `Get-AzureRmVM`         | Ottiene le proprietà di una macchina virtuale.       |
| `Update-AzureRmVM`      | Aggiorna lo stato di una macchina virtuale di Azure.  |
| `New-AzureRmDiskConfig` | Crea un oggetto disco configurabile.             |
| `Add-AzureRmVMDataDisk` | Aggiunge un disco dati a una macchina virtuale.          |

Con questi cmdlet è possibile eseguire tutte le attività eseguite nel portale di Azure. Ora verranno provati con la macchina virtuale dell'esercizio.
