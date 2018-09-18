In questo esercizio si creeranno un servizio di bilanciamento del carico, una rete virtuale e più macchine virtuali usando il portale di Azure.

Si supponga di lavorare per Woodgrove Bank, una startup che sta per lanciare servizi di banking online. Trattandosi di un settore estremamente competitivo, è necessario garantire una disponibilità minima dei servizi del 99,99%. È stato determinato che un'istanza di Azure Load Balancer con un pool di tre macchine virtuali consentirà di raggiungere questo obiettivo.

## <a name="create-a-public-load-balancer"></a>Creare un servizio di bilanciamento del carico pubblico

1. In un browser passare al [portale di Azure](https://portal.azure.com/?azure-portal=true) e accedere al proprio account.

1. Nella barra laterale fare clic su **Crea una risorsa** e quindi nel pannello **Nuovo** fare clic su **Rete** e infine su **Servizio di bilanciamento del carico**.

1. Nel pannello Crea servizio di bilanciamento del carico immettere o selezionare le informazioni seguenti.
    - Nome: **woodgrove-LB**
    - Tipo: **Pubblico**
    - SKU: **Basic**
    - Indirizzo IP pubblico: selezionare **Crea nuovo**, digitare **woodgrove-LB-ip** nella casella di testo e lasciare l'assegnazione di tipo **Dinamico**
    - Gruppo di risorse: selezionare **Crea nuovo** e digitare **woodgrove-RG** nella casella
    - Località: selezionare un'area nelle vicinanze

1. Fare clic su **Crea**.

1. Prima di continuare con l'esercizio attendere il completamento della distribuzione del servizio di bilanciamento del carico.

## <a name="create-a-virtual-network"></a>Creare una rete virtuale

1. Nel menu a sinistra fare clic su **Crea una risorsa** e quindi nel pannello **Nuovo** fare clic su **Rete** e infine su **Rete virtuale**.

1. Nel pannello **Crea rete virtuale** immettere o selezionare le informazioni seguenti.
    - Nome: **woodgrove-VNET**
    - Spazio indirizzi: **172.20.0.0/16**
    - Gruppo di risorse: selezionare **Usa esistente** e quindi **woodgrove-RG**
    - Subnet: **backendSubnet**
    - Spazio indirizzi: **172.20.0.0/24**
    - Protezione DDoS: **Basic**
    - Endpoint servizio: **Disabilitato**

1. Fare clic su **Crea**.

1. Prima di continuare con l'esercizio attendere il completamento della distribuzione della rete virtuale.

## <a name="create-a-vm-template"></a>Creare un modello di VM

Per iniziare, definire le informazioni di base della VM:

1. Nel portale di Azure fare clic su **Macchine virtuali** nel menu a sinistra e quindi su **Crea macchina virtuale**.

1. Nel pannello Calcolo fare clic su **Windows Server** nella sezione **Consigliati**.

1. Nel pannello **Windows Server** fare clic su **Windows Server 2016 Datacenter**.

1. Nel pannello **Windows Server 2016 Datacenter** fare clic su **Crea**.

1. Nel pannello **Informazioni di base** digitare **woodgrove-SVR01** nella casella **Nome**.

1. Nelle caselle **Nome utente** e **Password** digitare un nome e una password sicuri per un account amministratore nel server.

1. Nella casella **Sottoscrizione** selezionare la sottoscrizione di Azure.

1. In **Gruppo di risorse** selezionare **Usa esistente** e quindi **woodgrove-RG** nell'elenco.

1. Nell'elenco a discesa **Località** selezionare un'area nelle vicinanze.

1. Fare clic su **OK**.

Scegliere una dimensione per la VM e configurare le impostazioni:

1. Nel pannello **Scegli una dimensione** selezionare uno SKU **Standard**, ad esempio **D2s_v3**, e quindi fare clic su **Seleziona**.

1. Nel pannello **Impostazioni** fare clic su **Set di disponibilità**.

1. Nel pannello **Modifica set di disponibilità** fare clic su **Crea nuovo**.

1. Nel pannello **Crea nuovo** digitare **woodgrove-AS** nella casella **Nome** e quindi fare clic su **OK**.

1. Nel pannello **Impostazioni** fare clic su **Avanzate** in **Gruppo di sicurezza di rete** e quindi su **(nuovo) woodgrove-SVR01-nsg**.

1. Nel pannello **Crea gruppo di sicurezza di rete** modificare il nome in **woodgrove-NSG** nella casella **Nome** e quindi fare clic su **OK**.

1. Nel pannello **Impostazioni** fare clic su **OK**.

Salvare le impostazioni in un modello, per poter distribuire facilmente più VM.

1. Nel pannello **Crea** fare clic su **Scarica modello e parametri**.

1. Nel pannello **Modello** fare clic su **Aggiungi elementi al Catalogo multimediale**.

1. Nel pannello **Salva modello** digitare **woodgrove-server-template** nelle caselle **Nome** e **Descrizione** e quindi fare clic su **Salva**.

> [!NOTE]
> Per trovare questo modello, fare clic su **Tutti i servizi** nel menu a sinistra, digitare **modelli** nella casella del filtro e quindi fare clic su **Modelli (ANTEPRIMA)**.

## <a name="use-the-template-to-provision-the-first-vm"></a>Usare il modello per effettuare il provisioning della prima VM

1. Nel pannello **Modello** fare clic su **Distribuisci**.

1. Nel pannello **Distribuzione personalizzata** selezionare **Usa esistente** in **Gruppo di risorse** e quindi **woodgrove-RG** nell'elenco.

1. Nel pannello **Distribuzione personalizzata** digitare la stessa password usata in precedenza nella casella **Password amministratore**.

1. Nel pannello **Distribuzione personalizzata** selezionare la casella di controllo **Accetto le condizioni riportate sopra** e quindi fare clic su **Acquista**. Verranno addebitati i normali costi di calcolo di Azure, in base al piano tariffario della VM.

1. Prima di continuare con l'esercizio attendere il completamento della distribuzione della VM, per assicurarsi che il modello sia configurato correttamente prima di usarlo per il provisioning di altre VM e che siano state create tutte le risorse associate.

## <a name="use-the-template-to-provision-two-additional-vms"></a>Usare il modello per effettuare il provisioning di altre due VM

1. Nel portale di Azure fare clic su **Distribuisci** nel pannello **Modello**.

1. Nel pannello **Distribuzione personalizzata** selezionare **Usa esistente** in **Gruppo di risorse** e quindi **woodgrove-RG** nell'elenco.

1. Nel pannello **Distribuzione personalizzata** modificare il nome in **woodgrove-SVR02** nella casella **Nome macchina virtuale**.

1. Nel pannello **Distribuzione personalizzata** modificare il nome in **woodgrovesvr02222** nella casella **Network Interface Name** (Nome interfaccia di rete).

1. Nel pannello **Distribuzione personalizzata** digitare la stessa password usata in precedenza nella casella **Password amministratore**.

1. Nel pannello **Distribuzione personalizzata** modificare il nome in **woodgrove-SVR02-ip** nella casella **Nome indirizzo IP pubblico**.

1. Nel pannello **Distribuzione personalizzata** selezionare la casella di controllo **Accetto le condizioni riportate sopra** e quindi fare clic su **Acquista**. Verranno addebitati i normali costi di calcolo di Azure, in base al piano tariffario della VM.

1. Ripetere i passaggi da 1 a 7 usando le informazioni seguenti.
    - Nome macchina virtuale: **woodgrove-SVR03**
    - Nome interfaccia di rete: **woodgrovesvr03333**
    - Nome indirizzo IP pubblico: **woodgrove-SVRr03-ip**

1. Prima di continuare con l'esercizio attendere il completamento della distribuzione delle VM.

Sono ora disponibili un servizio di bilanciamento del carico pubblico pronto per la configurazione e tre VM pronte per essere usate con tale servizio.