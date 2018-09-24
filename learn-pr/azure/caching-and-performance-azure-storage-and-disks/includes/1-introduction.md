L'infrastruttura di database di un'azienda è basata su macchine virtuali di SQL Server in esecuzione in Azure. I risultati dell'azienda sono soddisfacenti e richiedono un'espansione della struttura, sempre tenendo sotto controllo i costi. Alcune operazioni di database comportano molte attività di lettura dei dati esistenti. Le regolari esecuzioni delle attività di fatturazione e reporting sono operazioni con intensa attività di scrittura. È indispensabile trovare un modo per ottimizzare l'infrastruttura per gestire tutti i tipi di operazioni. Prima di investire in miglioramenti dell'infrastruttura, si decide di prendere in considerazione innanzi tutto le opzioni di memorizzazione nella cache del disco della macchina virtuale.

La memorizzazione nella cache è un approccio comune per velocizzare le risorse di calcolo. Azure supporta una gamma di tecnologie di memorizzazione nella cache per ottimizzare l'accesso ai dati dall'intero panorama applicativo di Azure, incluse opzioni di memorizzazione nella cache specifiche per l'archiviazione di Azure e dischi usati dalle macchine virtuali di Azure (VM).

Si esamineranno ora le opzioni di memorizzazione nella cache del disco disponibili in Azure e si apprenderà come gestire la memorizzazione nella cache del disco tramite il portale e PowerShell.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Descrivere le principali considerazioni sulle prestazioni del disco in Azure
- Descrivere gli effetti della memorizzazione nella cache sulle prestazioni del disco in Azure
- Abilitare e gestire le impostazioni della cache con il portale di Azure
- Abilitare e gestire le impostazioni della cache con PowerShell

## <a name="prerequisites"></a>Prerequisiti  

Nessuno