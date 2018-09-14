In questa breve esercitazione si apprenderà come eseguire la migrazione di un disco rigido virtuale esistente, non gestito a un disco rigido virtuale gestito. 

## <a name="sign-in-to-azure"></a>Accedere ad Azure

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

## <a name="migrate-our-disks-to-managed-disks"></a>Migrare i dischi a dischi gestiti

1. Nel portale di Azure, nel riquadro di spostamento a sinistra, selezionare **macchine virtuali**.

1. Nell'elenco delle macchine virtuali, selezionare la macchina virtuale, **MailSenderVM**.

1. Nel **MailSenderVM** riquadro, di sotto **impostazioni**, selezionare **dischi**.

1. Nel **MailSenderVM - Disks** pagina, selezionare **eseguire la migrazione a managed disks**.

1. Nel **eseguire la migrazione a managed disks** pagina, selezionare **Migrate**. Azure arresta la macchina virtuale, viene eseguita la migrazione dei dischi e quindi riavvia la macchina virtuale. L'esecuzione può richiedere alcuni minuti.

I dischi di migrazione a managed disks in questo esercizio. Usando dischi gestiti, non devi configurare account di archiviazione per tali dischi poiché Azure li gestisce automaticamente.
