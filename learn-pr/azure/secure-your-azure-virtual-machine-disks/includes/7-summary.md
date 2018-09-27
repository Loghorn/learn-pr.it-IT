Per proteggere i dischi delle macchine virtuali di Azure, è possibile usare Crittografia del servizio di archiviazione e Crittografia dischi di Azure incluse in Azure. L'uso combinato di queste tecnologie consente di accedere alla crittografia a 256 bit, nell'ambito di un approccio di difesa più approfondito per la protezione dei dischi delle macchine virtuali di Azure. Per abilitare la Crittografia dischi di Azure, è necessario soddisfare i prerequisiti di Crittografia dischi di Azure. Lo script di configurazione dei prerequisiti di Crittografia dischi di Azure consente di automatizzare questo processo. Quando si abilita la crittografia in nuove macchine virtuali, è possibile usare un modello Azure Resource Manager. Ciò garantisce che i dati vengano crittografati al momento della distribuzione, eliminando qualsiasi vulnerabilità.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Risoluzione dei problemi relativi alla crittografia del disco di Azure VM](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-tsg)
- [Come crittografare una macchina virtuale Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/encrypt-disks)
- [Panoramica della Crittografia dischi di Azure](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)
- [Quali distribuzioni di Linux sono supportate da Crittografia dischi di Azure?](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-faq#bkmk_LinuxOSSupport)
- [Modelli di Resource Manager su GitHub](https://github.com/Azure/azure-quickstart-templates)