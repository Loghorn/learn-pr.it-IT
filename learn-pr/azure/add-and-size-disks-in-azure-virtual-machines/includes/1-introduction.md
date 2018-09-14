Si è l'architetto di sistema per uno studio legale. L'azienda ha richiesto di eseguire la migrazione di sistemi di chiavi in Azure. Sistemi includono il database di cronologie dei case, attualmente ospitato da un server SQL in locale e accedervi da un'applicazione desktop. SQL server esegue anche alcuni servizi interni personalizzate per eseguire la manutenzione del database. Si è deciso che una soluzione basata su macchine virtuali di Azure (VM) consentirà di ospitare SQL server e continuare a usare i servizi personalizzati. Si creerà un disco rigido virtuale Azure basato sul contenuto del server locale esistente per agevolare la migrazione.

In questo modulo, verrà illustrato come progettare la configurazione ottimale dei dischi per le macchine virtuali create in Azure. È possibile creare dischi rigidi virtuali tramite il portale di Azure e aggiungerli alle macchine virtuali per creare le risorse di calcolo completamente funzionale.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Creare una macchina virtuale nel portale di Azure
- Creare dischi e dischi dati di sistema operativo nelle VM di Azure
- Scegliere tra e creare dischi standard e premium
- Scegliere tra e creare non gestite e i dischi gestiti