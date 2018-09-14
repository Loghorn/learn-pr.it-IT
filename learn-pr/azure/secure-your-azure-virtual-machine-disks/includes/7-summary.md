Per proteggere i dischi delle macchine virtuali di Azure, è possibile usare la crittografia del servizio di archiviazione e Crittografia dischi di Azure incluse in Azure. Queste tecnologie interagiscono per fornire crittografia a 256 bit, come parte di un approccio di difesa in profondità per la protezione dei dischi delle macchine Virtuali di Azure. È necessario completare i prerequisiti di crittografia dischi di Azure per abilitare la crittografia del disco. Lo script di configurazione dei prerequisiti di crittografia dischi di Azure è possibile automatizzare questo processo. Quando si abilita la crittografia in nuove macchine virtuali, è possibile usare un modello Azure Resource Manager. Ciò garantisce che i dati vengono crittografati al momento della distribuzione, non lasciando alcuna vulnerabilità.

## <a name="clean-up"></a>Eseguire la pulizia
<!---TODO: Update for sandbox?---> Esecuzione di macchine virtuali di Azure e l'archiviazione associata, comporta costi per la sottoscrizione. È quindi consigliabile rimuovere le risorse non necessarie per evitare addebiti non necessari. Il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse. Verranno così eliminate anche tutte le risorse nel gruppo. Dopo aver completato questo modulo, eseguire il cmdlet seguente di Azure PowerShell:

   ```powershell
   Remove-AzureRmResourceGroup -Name moneyapprg
   ```

Quando viene chiesto di confermare l'eliminazione, rispondere **Sì**. Il completamento di questo comando può richiedere alcuni minuti mentre vengono eliminate le risorse. 

Se viene visualizzato un messaggio di eliminazione non riuscita, occorre rimuovere i blocchi su key vault prima di poter eliminare il gruppo di risorse.
