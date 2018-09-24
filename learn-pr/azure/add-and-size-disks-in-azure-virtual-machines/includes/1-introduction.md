Si lavora come progettista di sistemi per uno studio legale. Lo studio ha chiesto di eseguire la migrazione dei sistemi critici in Azure. Le operazioni includono il database dei casi archiviati, attualmente ospitato da un server SQL locale e a cui si accede da un'applicazione desktop. Il server SQL esegue anche alcuni servizi interni personalizzati per la manutenzione del database. Si è deciso che una soluzione basata su macchine virtuali (VM) di Azure consentirà di ospitare il server SQL e continuare a usare i servizi personalizzati. Si creerà un disco rigido virtuale di Azure basato sui contenuti del server locale esistente per agevolare la migrazione.

In questo modulo si apprenderà come progettare la configurazione ottimale dei dischi per le macchine virtuali create in Azure.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Creare una macchina virtuale (VM)
- Configurare e collegare dischi rigidi virtuali (VHD) a una macchina virtuale esistente
- Determinare se sono necessari dischi Premium
- Ridimensionare i dischi per una macchina virtuale

## <a name="prerequisites"></a>Prerequisiti  

Nessuno