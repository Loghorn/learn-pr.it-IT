La società bancaria gestisce i dati sensibili dei clienti e richiede che i dischi siano sempre crittografati, anche durante la distribuzione dei dischi delle macchine virtuali. Il compito assegnato è quindi quello di automatizzare una distribuzione sicura delle macchine virtuali per salvaguardare i dati aziendali.

In questa unità, si userà un modello Azure Resource Manager per abilitare automaticamente la crittografia per le nuove macchine virtuali Windows.

## <a name="configure-and-deploy-a-new-vm-using-an-azure-resource-manager-template"></a>Configurare e distribuire una nuova macchina virtuale usando un modello di Azure Resource Manager

1. Passare al [modello di Resource Manager in GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) e quindi fare clic su **Distribuisci in Azure**.
1. Nel portale di Azure, nelle **modello di avvio rapido** pannello, in **gruppo di risorse**, selezionare **Usa esistente**. Nell'elenco, selezionare **moneyapprg**.
1. Nella sezione **IMPOSTAZIONI** immettere le informazioni seguenti:

   - Nome macchina virtuale: **moneyappsvr02**
   - **Admin Username**: identico a quello usato nell'esercizio precedente.
   - **Password amministratore**: identico a quello usato nell'esercizio precedente.
   - **New Storage Account Name**: immettere un nome univoco.
   - **Dimensioni della macchina virtuale**: sostituire con le stesse dimensioni che è usato nell'esercizio precedente, ad esempio **Standard_B1s** (come si usa la stessa area di Azure, assicurando che le dimensioni sono disponibile nell'area corrente).
   - **Nome rete virtuale**: **moneyapprg-vnet**
   - **Nome subnet**: **predefinito**
   - **ID Client AAD**: copia dalle informazioni incollato nel blocco note.
   - **Segreto Client AAD**: copia dalle informazioni incollato nel blocco note.
   - **Nome insieme di credenziali delle chiavi**: **moneyappkv**
   - **Gruppo di risorse dell'insieme di credenziali delle chiavi**: **moneyapprg**
   - **Chiave di crittografia chiave URL**: copia dalle informazioni incollato nel blocco note.
1. Selezionare la casella di controllo **Accetto le condizioni riportate sopra** e quindi fare clic su **Acquista**.

La distribuzione potrebbe richiedere circa 5-10 minuti.

## <a name="verify-encryption-status-of-new-vm"></a>Verificare lo stato della crittografia della nuova macchina virtuale

1. Nella barra laterale del portale di Azure fare clic su **Macchine virtuali**.

1. Nel pannello **Macchine virtuali** fare clic su **moneyappsvr02**.

1. Nel pannello **Macchina virtuale** fare clic su **Dischi** in **IMPOSTAZIONI**.

1. Nel pannello **Dischi** notare che lo stato di crittografia dei dischi del sistema operativo è **Abilitato**.
