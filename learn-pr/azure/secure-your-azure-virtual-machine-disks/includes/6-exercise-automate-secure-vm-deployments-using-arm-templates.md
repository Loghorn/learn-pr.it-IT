La società bancaria gestisce i dati sensibili dei clienti e richiede che i dischi siano sempre crittografati, anche durante la distribuzione dei dischi delle macchine virtuali. Il compito assegnato è quindi quello di automatizzare una distribuzione sicura delle macchine virtuali per salvaguardare i dati aziendali.

In questa unità si userà un modello di Azure Resource Manager (ARM) per abilitare automaticamente la crittografia per le nuove macchine virtuali Windows.

## <a name="configure-and-deploy-a-new-vm-using-an-arm-template"></a>Configurare e distribuire una nuova macchina virtuale con un modello ARM

1. Passare al [modello di Resource Manager in GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) e quindi fare clic su **Distribuisci in Azure**.
1. Nel pannello **Modello di avvio rapido di Azure** del portale di Azure selezionare **Usa esistente** in **Gruppo di risorse** e nell'elenco selezionare **moneyapprg**.
1. Nella sezione **IMPOSTAZIONI** immettere le informazioni seguenti:

   - Nome macchina virtuale: **moneyappsvr02**
   - **Nome utente amministratore**: uguale a quello usato nell'esercizio precedente
   - **Password amministratore**: uguale a quella usata nell'esercizio precedente
   - **Nuovo nome account di archiviazione**: immettere un nome univoco
   - **Dimensioni macchina virtuale**: sostituire con le stesse dimensioni usate nell'esercizio precedente, ad esempio **Standard_B1s** (dal momento che si usa la stessa area di Azure, assicurarsi che tali dimensioni siano disponibili nell'area corrente)
   - **Nome rete virtuale**: **moneyapprg-vnet**
   - **Nome subnet**: **predefinito**
   - **AAD Client Id** (ID client AAD): copiarlo dalle informazioni incollate negli Appunti
   - **AAD Client Secret** (Segreto client AAD): copiarlo dalle informazioni incollate negli Appunti
   - **Nome insieme di credenziali delle chiavi**: **moneyappkv**
   - **Gruppo di risorse dell'insieme di credenziali delle chiavi**: **moneyapprg**
   - **Key Encryption Key URL** (URL della chiave di crittografia chiavi): copiarlo dalle informazioni incollate negli Appunti
   - Selezionare la casella di controllo **Accetto le condizioni riportate sopra** e quindi fare clic su **Acquista**.
1. Il completamento della distribuzione richiede 5-10 minuti.

## <a name="verify-encryption-status-of-new-vm"></a>Verificare lo stato della crittografia della nuova macchina virtuale

1. Nella barra laterale del portale di Azure fare clic su **Macchine virtuali**.

1. Nel pannello **Macchine virtuali** fare clic su **moneyappsvr02**.

1. Nel pannello **Macchina virtuale** fare clic su **Dischi** in **IMPOSTAZIONI**.

1. Nel pannello **Dischi** notare che lo stato di crittografia dei dischi del sistema operativo è **Abilitato**.
