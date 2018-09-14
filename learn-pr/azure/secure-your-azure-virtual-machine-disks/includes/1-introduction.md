Si supponga di lavorare per un'azienda di vendita all'ingrosso che sta eseguendo il passaggio al cloud. Attualmente, è usare un ambiente ibrido, costituito da server Windows locali e macchine virtuali di Azure (VM) Azure Active Directory. L'azienda ha sviluppato un'infrastruttura business-to-business (B2B) interna personalizzata che supporta la gestione sicura degli ordini ai fornitori. Alcuni fornitori usano i server Linux e l'azienda esegue alcuni server Linux in Azure per supportare questi fornitori.

I criteri di sicurezza richiedono la crittografia dei dati tramite chiavi di crittografia che l'azienda è responsabile di gestire.

Il team di amministrazione usa già PowerShell per la gestione dei server in locale. Si sarà distribuire e testare più macchine virtuali di Azure e prevede di usare i modelli di Azure Resource Manager per automatizzare questo processo.

In questo modulo verranno esaminati i tipi di protezione disponibili per i dischi delle macchine virtuali, in modo che sia possibile stabilire se Crittografia dischi di Azure (ADE) rappresenta la scelta migliore per un determinato scenario. Si verrà quindi abilitare ADE nei dischi di macchina virtuale esistenti e usare modelli per abilitare ADE per nuove distribuzioni di macchine Virtuali.


## <a name="learning-objectives"></a>Obiettivi di apprendimento

Contenuto del modulo:

- Determinare il metodo di crittografia più adatto per la macchina virtuale
- Crittografare i dischi delle macchine virtuali esistenti usando il portale di Azure
- Crittografare i dischi delle macchine virtuali esistenti usando PowerShell
- Modificare i modelli di Azure Resource Manager per automatizzare la crittografia del disco nelle nuove macchine virtuali
