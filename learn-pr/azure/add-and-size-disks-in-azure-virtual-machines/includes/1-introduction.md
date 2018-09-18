Si lavora come progettista di sistemi per uno studio legale. Lo studio ha richiesto di eseguire la migrazione dei sistemi principali in Azure. I sistemi includono il database dei casi archiviati, attualmente ospitato da un server SQL locale e a cui viene eseguito l'accesso da un'applicazione desktop. Il server SQL esegue anche alcuni servizi interni personalizzati per la manutenzione del database. Si è deciso che una soluzione basata su macchine virtuali (VM) di Azure consentirà di ospitare il server SQL e continuare a usare i servizi personalizzati. Si creerà un disco rigido virtuale di Azure basato sui contenuti del server locale esistente per agevolare la migrazione.

In questo modulo si apprenderà come progettare la configurazione ottimale dei dischi per le macchine virtuali create in Azure. Verranno creati dischi rigidi virtuali tramite il portale di Azure e tali dischi verranno aggiunti alle macchine virtuali per creare risorse di calcolo completamente funzionali.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Creare una macchina virtuale nel portale di Azure
- Creare dischi dati e dischi del sistema operativo nelle macchine virtuali di Azure
- Scegliere tra dischi Standard e Premium e creare i dischi scelti
- Scegliere tra dischi gestiti e non gestiti e creare i dischi scelti