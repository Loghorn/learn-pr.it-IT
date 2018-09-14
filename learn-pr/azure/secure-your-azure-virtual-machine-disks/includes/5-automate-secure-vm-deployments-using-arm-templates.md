Si supponga che l'azienda voglia distribuire numerosi server come parte della transizione al cloud. I dischi delle macchine virtuali devono essere crittografati durante la distribuzione, per evitare periodi di vulnerabilità dei dischi. Si desidera automatizzare questo processo e necessario modificare i modelli di Azure Resource Manager per abilitare automaticamente la crittografia.

In questo caso, esamineremo come utilizzare un modello Azure Resource Manager per abilitare automaticamente la crittografia per le nuove macchine virtuali Windows.

## <a name="what-are-azure-resource-manager-templates"></a>Quali sono i modelli Azure Resource Manager?

Questi modelli sono file JSON usati per definire una risorsa da distribuire in Azure, ad esempio una macchina virtuale. È possibile scriverli da zero e, per alcune risorse di Azure, incluse le macchine virtuali, è possibile usare il portale di Azure per generarli. Sarà necessario immettere le informazioni necessarie per una distribuzione di macchina virtuale manuale, ma anziché distribuire la macchina virtuale in Azure, si salverà il modello.

In GitHub sono disponibili modelli di esempio.

## <a name="using-github-templates"></a>Utilizzo di modelli di GitHub

GitHub è un modello per abilitare la crittografia denominato il **abilitare la crittografia in un ARM di VM Windows in esecuzione**. È anche disponibile nel repository dei [Modelli di avvio rapido di Azure](https://github.com/Azure/azure-quickstart-templates). La pagina Leggimi per il modello fornisce un **Distribuisci in Azure** che consente di aprire quindi il modello nel portale di Azure.

Il modello consente di distribuire una macchina virtuale Windows Server con la crittografia già abilitata. Prima di usare il modello, è necessario assicurarsi che tutti i prerequisiti di crittografia siano soddisfatti. È necessario anche le informazioni di configurazione fornito dallo script dei prerequisiti, ad esempio Azure Active Directory Client ID e il segreto del Client Azure Active Directory.

Per creare una nuova macchina virtuale, immettere le informazioni necessarie nel modello. Avviare quindi la distribuzione facendo clic su **Acquista** (i costi sono in genere i normali addebiti di calcolo di Azure).

## <a name="deploy-an-encrypted-vm-by-using-a-template"></a>Distribuire una macchina virtuale crittografata tramite un modello

I passaggi principali per la distribuzione di una macchina virtuale crittografata tramite un modello sono:

1. Accedere ed eseguire il modello **Enable encryption on a running Windows VM ARM** (Abilitare la crittografia in una macchina virtuale che esegue Windows tramite Azure Resource Manager) in GitHub.

1. Aggiungere i dettagli necessari nella pagina di configurazione dello script.

1. Distribuire una nuova macchina virtuale tramite lo script facendo clic su **Acquista**.

1. Usare il portale di Azure per verificare lo stato di crittografia del disco.
