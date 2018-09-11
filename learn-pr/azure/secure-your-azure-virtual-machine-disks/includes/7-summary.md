Per proteggere i dischi delle macchine virtuali di Azure, è possibile usare la crittografia del servizio di archiviazione e Crittografia dischi di Azure incluse in Azure. L'uso combinato di queste tecnologie consente di accedere alla crittografia a 256 bit, nell'ambito di un approccio di difesa più approfondito per la protezione dei dischi delle macchine virtuali di Azure. Per abilitare la crittografia dei dischi, è necessario soddisfare i prerequisiti di Crittografia dischi di Azure. Lo script di configurazione dei prerequisiti di Crittografia dischi di Azure consente di automatizzare questo processo. Quando si abilita la crittografia in nuove macchine virtuali, è possibile usare un modello ARM per garantire che i dati vengano crittografati al momento della distribuzione, senza lasciare vulnerabilità.

## <a name="cleanup"></a>Eseguire la pulizia
<!---TODO: Do we need to include cleanup for the free education tier?--->

L'esecuzione di macchine virtuali di Azure comporta l'addebito di costi per la sottoscrizione. È quindi consigliabile rimuovere le risorse non necessarie per evitare addebiti non necessari. Il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse. Verranno così eliminate anche tutte le risorse nel gruppo. Dopo aver completato questo modulo, eseguire il cmdlet seguente di Azure PowerShell:

   ```powershell
   Remove-AzureRmResourceGroup -Name moneyapprg
   ```

Quando viene richiesto di confermare l'eliminazione, rispondere **Sì**. Il completamento di questo comando può richiedere alcuni minuti mentre vengono eliminate le risorse. 

Se viene visualizzato un messaggio di eliminazione non riuscita, può essere necessario rimuovere i blocchi nel Key Vault prima di poter eliminare il gruppo di risorse.
