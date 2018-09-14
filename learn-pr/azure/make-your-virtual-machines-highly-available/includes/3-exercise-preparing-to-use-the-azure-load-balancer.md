In questo esercizio si creerà un servizio di bilanciamento del carico, una rete virtuale e più macchine virtuali usando il portale di Azure.

Si supponga che si lavora per Woodgrove Bank, una startup che sta per avviare servizi bancari online. In questo settore è altamente competitivo, pertanto è necessario garantire almeno il 99,99% di disponibilità del servizio. Si è stabilito che soddisfano questo obiettivo con un pool di tre macchine virtuali di Azure Load Balancer.

## <a name="create-a-public-load-balancer"></a>Creare un servizio di bilanciamento del carico pubblico

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Nella barra laterale, fare clic su **crea una risorsa**. Quindi, nella **New** pannello, fare clic su **Networking**e quindi fare clic su **Load Balancer**.

1. Nel **crea bilanciamento del carico** pannello, immettere o selezionare le informazioni seguenti:
    - Nome: **woodgrove-LB**
    - Tipo: **pubblico**
    - SKU: **Basic**
    - Indirizzo IP pubblico: selezionare **Crea nuovo**. Nella casella di testo, digitare **woodgrove-LB-ip**. Lasciare l'assegnazione come **dinamica**.
    - Gruppo di risorse: selezionare **Usa esistente** e scegliere <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>.
    - Posizione: Selezionare un'area vicino a te.

1. Fare clic su **Crea**.

1. Attendere che il servizio di bilanciamento del carico ha distribuito prima di continuare con l'esercizio.

## <a name="create-a-virtual-network"></a>Creare una rete virtuale

1. Nel menu a sinistra, fare clic su **crea una risorsa**. Nel **New** pannello, fare clic su **Networking**e quindi fare clic su **rete virtuale**.

1. Nel **crea rete virtuale** pannello, immettere o selezionare le informazioni seguenti:
    - Name: **woodgrove-VNET**
    - Spazio degli indirizzi: **172.20.0.0/16**
    - Gruppo di risorse: selezionare **Usa esistente**, quindi selezionare <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>.
    - Subnet: **backendSubnet**
    - Spazio degli indirizzi: **172.20.0.0/24**
    - Protezione DDoS: **base**
    - Endpoint di servizio: **disabilitato**

1. Fare clic su **Crea**.

1. Attendere fino a quando non è distribuito nella rete virtuale prima di continuare con l'esercizio.

## <a name="create-a-vm-template"></a>Creare un modello di macchina virtuale

Iniziare definendo le informazioni di base sulla macchina virtuale:

1. Nel portale di Azure, nel menu a sinistra, fare clic su **macchine virtuali**, quindi fare clic su **crea macchina virtuale**.

1. Nel **Compute** pannello, nella **consigliato** fare clic su **Windows Server**.

1. Nel pannello **Windows Server** fare clic su **Windows Server 2016 Datacenter**.

1. Nel pannello **Windows Server 2016 Datacenter** fare clic su **Crea**.

1. Nel **nozioni di base** pannello nella **nome** , digitare **woodgrove SVR01**.

1. Nel **nomeutente** e **caselle Password**, digitare un nome sicuro e una password per un account di amministratore su questo server.

1. Nella casella **Sottoscrizione** selezionare la sottoscrizione di Azure.

1. Sotto **gruppo di risorse**, selezionare **Usa esistente**. Nell'elenco, selezionare **woodgrove-RG**.

1. Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.

1. Fare clic su **OK**.

Scegli una dimensione per la macchina virtuale e quindi configurare le impostazioni:

1. Nel **Scegli una dimensione** pannello Seleziona un **Standard** SKU, ad esempio **D2s_v3**. Fare clic su **Seleziona**.

1. Nel **le impostazioni** pannello, fare clic su **set di disponibilità**.

1. Nel **modificare set di disponibilità** pannello, fare clic su **Crea nuovo**.

1. Nel **Crea nuovo** pannello nella **nome** , digitare **woodgrove-AS**e quindi fare clic su **OK**.

1. Nel **le impostazioni** pannello, in **Network Security Group**, fare clic su **avanzate**e quindi fare clic su **(nuovo) woodgrove-SVR01-nsg**.

1. Nel **Crea gruppo di sicurezza di rete** pannello, nella **nome** modificare il nome da **woodgrove-NSG**e quindi fare clic su **OK**.

1. Nel **le impostazioni** pannello, fare clic su **OK**.

Salvare le impostazioni in un modello, in modo che è possibile distribuire facilmente più macchine virtuali.

1. Nel **Create** pannello, fare clic su **Scarica modello e parametri**.

1. Nel **modello** pannello, fare clic su **aggiungere alla libreria**.

1. Nel **Salva modello** pannello, nel **nome** e **descrizione** caselle digitare **woodgrove-server-template**. Fare quindi clic su **Salva**.

> [!NOTE]
> Se è necessario trovare questo modello, fare clic su **tutti i servizi** nel menu a sinistra, digitare **modello** nella casella del filtro e quindi fare clic su **modelli (anteprima)**.

## <a name="use-the-template-to-provision-the-first-vm"></a>Usare il modello per effettuare il provisioning della prima VM

1. Nel **modello** pannello, fare clic su **Distribuisci**.

1. Nel **distribuzione personalizzata** blade, sotto **gruppo di risorse**, selezionare **Usa esistente**. Nell'elenco, selezionare **woodgrove-RG**.

1. Nel **distribuzione personalizzata** pannello nella **password amministratore** , digitare la stessa password utilizzata in precedenza.

1. Nel **distribuzione personalizzata** blade, selezionare la **dichiaro di accettare i termini e condizioni** casella di controllo e quindi fare clic su **acquisto** (il costo è il calcolo di Azure regolare applicato un addebito, che a seconda della VM di livello di prezzo).

1. Attendere il completamento della distribuzione della macchina virtuale prima di continuare con l'esercizio. Si tratta in modo da essere certi che il modello sia configurato correttamente prima di usarla per eseguire il provisioning di macchine virtuali aggiuntive e che siano state create tutte le risorse associate.

## <a name="use-the-template-to-provision-two-additional-vms"></a>Usare il modello per effettuare il provisioning di due VM aggiuntive

1. Nel portale di Azure, nelle **modello** pannello, fare clic su **Distribuisci**.

1. Nel **distribuzione personalizzata** blade, sotto **gruppo di risorse**, selezionare **Usa esistente**. Nell'elenco, selezionare **woodgrove-RG**.

1. Nel **distribuzione personalizzata** pannello, nel **nome macchina virtuale** modificare il nome da **woodgrove SVR02**.

1. Nel **distribuzione personalizzata** pannello, nel **nome di interfaccia di rete** modificare il nome da **woodgrovesvr02222**.

1. Nel **distribuzione personalizzata** pannello nella **password amministratore** , digitare la stessa password utilizzata in precedenza.

1. Nel **distribuzione personalizzata** pannello, nella **nome indirizzo Ip pubblico** modificare il nome da **woodgrove-SVR02-ip**.

1. Nel **distribuzione personalizzata** blade, selezionare la **dichiaro di accettare i termini e condizioni** casella di controllo e quindi fare clic su **acquisto** (il costo è il calcolo di Azure regolare applicato un addebito, che a seconda della VM di livello di prezzo).

1. Ripetere i passaggi da 1 a 7, usando le informazioni seguenti:
    - Nome della macchina virtuale: **woodgrove SVR03**
    - Nome di interfaccia di rete: **woodgrovesvr03333**
    - Nome dell'indirizzo IP pubblico: **woodgrove-SVRr03-ip**

1. Attendere che le macchine virtuali sono distribuite prima di continuare con l'esercizio.

È ora disponibile un servizio di bilanciamento del carico pubblico pronto per la configurazione, e tre macchine virtuali pronto da usare con questo servizio di bilanciamento del carico.