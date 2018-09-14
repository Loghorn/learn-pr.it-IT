Gli account di archiviazione consentono di creare un gruppo di regole di gestione dei dati e applicarle a tutti contemporaneamente a un gruppo di BLOB di Azure, file di Azure, le code di Azure e le tabelle di Azure. 

Se si è provato a ottenere lo stesso risultato senza account di archiviazione, sarebbe noiosa e soggetta a errori. Ad esempio, quali sono le probabilità che è possibile applicare correttamente l'oggetto ruleset stesso esatto a migliaia di BLOB?

Al contrario, si acquisiscono le regole nelle impostazioni per un account di archiviazione e tali regole vengono applicate automaticamente a ogni servizio dati nell'account.