Si supponga di lavorare per un'azienda di vendita all'ingrosso che sta eseguendo il passaggio al cloud. Attualmente si sta usando un ambiente ibrido, costituito da server Windows locali, macchine virtuali di Azure (VM) e Azure Active Directory (AD). L'azienda ha sviluppato un'infrastruttura business-to-business (B2B) interna personalizzata che supporta la gestione sicura degli ordini ai fornitori. Alcuni fornitori usano i server Linux e l'azienda esegue alcuni server Linux in Azure per supportare questi fornitori.

I criteri di sicurezza richiedono la crittografia dei dati tramite chiavi di crittografia che l'azienda è responsabile di gestire.

Il team di amministrazione usa già PowerShell per la gestione dei server in locale. Si distribuiscono e si testano numerose macchine virtuali di Azure e si vuole usare modelli ARM per automatizzare questo processo.

In questo modulo verranno esaminati i tipi di protezione disponibili per i dischi delle macchine virtuali, in modo che sia possibile stabilire se Crittografia dischi di Azure (ADE) rappresenta la scelta migliore per un determinato scenario. Si abiliterà quindi Crittografia dischi di Azure nei dischi delle macchine virtuali esistenti per abilitare questo servizio nelle nuove distribuzioni di macchine virtuali.


## <a name="learning-objectives"></a>Obiettivi di apprendimento

Contenuto del modulo:

- Determinare il metodo di crittografia più adatto per la macchina virtuale
- Crittografare i dischi delle macchine virtuali esistenti usando il portale di Azure
- Crittografare i dischi delle macchine virtuali esistenti usando PowerShell
- Modificare i modelli ARM per automatizzare la crittografia dei dischi nelle nuove macchine virtuali
