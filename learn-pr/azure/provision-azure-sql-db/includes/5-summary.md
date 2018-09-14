Ora che il database SQL di Azure è operativo, è possibile connetterlo al proprio strumento di gestione di SQL Server preferito per popolarlo con dati reali.

Inizialmente è stato valutato se eseguire il database in locale o nel cloud. Con il database SQL di Azure è possibile configurare alcune opzioni di base per disporre di un database SQL completamente funzionale da connettere alle app.

Non vi sono infrastrutture o patch software da gestire. È quindi possibile concentrarsi sul prototipo di app di logistica per i trasporti, invece di preoccuparsi dell'amministrazione del database. Il prototipo non verrà usato solo a scopo dimostrativo. Il database SQL di Azure fornisce funzionalità di sicurezza e prestazioni a livello di produzione.

Tenere presente che ogni server logico SQL di Azure contiene uno o più database. Il database SQL di Azure offre due modelli di prezzi, DTU e vCore, che consentono di bilanciare costi e prestazioni tra tutti i database.

Se si sta iniziando o si vuole un'opzione di acquisto semplice e preconfigurata, scegliere il modello DTU. Se si vuole maggiore controllo sulle risorse di calcolo e archiviazione create e per cui vengono effettuati pagamenti, scegliere il modello vCore.

Azure Cloud Shell rende più semplice iniziare a lavorare con i database. Da Cloud Shell è possibile accedere all'interfaccia della riga di comando di Azure, che consente di ottenere informazioni sulle risorse di Azure. Cloud Shell fornisce anche molte altre utilità comuni, come `sqlcmd`, che consentono di iniziare subito a lavorare con il nuovo database.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>Risorse aggiuntive

La documentazione fornisce molte altre informazioni, inclusi esercitazioni ed esempi. Ecco alcuni collegamenti relativi agli argomenti descritti in questo documento:

- [Documentazione sul database SQL di Azure](https://docs.microsoft.com/azure/sql-database/)
- [Modelli di acquisto e risorse del database SQL di Azure](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)
- [Server logici del database SQL di Azure e relativa gestione](https://docs.microsoft.com/azure/sql-database/sql-database-logical-servers)
- [Regole firewall per il database SQL di Azure e SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)

Per altre informazioni su Cloud Shell, vedere [Panoramica di Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

Per altre informazioni sull'utilità `sqlcmd`, vedere [Utilità sqlcmd](https://docs.microsoft.com/sql/tools/sqlcmd-utility?view=sql-server-2017).
