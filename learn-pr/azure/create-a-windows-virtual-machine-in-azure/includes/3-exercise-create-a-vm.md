Si consideri uno scenario alternativo. In questo scenario l'organizzazione è un istituto di istruzione che usa macchine virtuali Windows per creare rapidamente ambienti di test in cui gli studenti installano app Web, configurano domini ed esplorano i servizi e le funzionalità di Windows senza alterare i computer dell'istituto di istruzione. Gli insegnanti possono connettersi a queste macchine virtuali tramite il protocollo RDP e controllare i progressi degli studenti. Vediamo come si potrebbe creare qualcosa di simile con Azure.

## <a name="create-a-windows-vm"></a>Creare una macchina virtuale Windows

Per creare una macchina virtuale Windows, seguire questa procedura:

1. Accedere ad Azure tramite il [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Fare clic su **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.

1. Nella **barra di ricerca** immettere **Windows Server 2016 Datacenter** e quindi fare clic sul collegamento con lo stesso titolo.

### <a name="configure-the-vm-settings"></a>Configurare le impostazioni della macchina virtuale

1. In **Informazioni di base**, nel campo **Nome** immettere un nome per la macchina virtuale, ad esempio "StudentVM".

1. Nel campo **Tipo di disco della macchina virtuale** fare clic sul menu a discesa per visualizzare le opzioni. Assicurarsi che sia selezionata l'opzione **Unità SSD**.

1. Nel campo **NomeUtente** immettere un nome utente idoneo da usare per accedere alla macchina virtuale.

1. Nel campo **Password** immettere una password di almeno 12 caratteri. La password deve contenere caratteri maiuscoli e minuscoli, numeri e caratteri speciali.

1. In **Gruppo di risorse** fare clic su **Crea nuovo**. Assegnare un nome al gruppo di risorse, ad esempio "myTestRG".

1. Selezionare un percorso adatto per la macchina virtuale da creare e quindi fare clic su **OK**.

### <a name="select-the-vm-image-size-and-options"></a>Selezionare le dimensioni dell'immagine e le opzioni della macchina virtuale

1. Nella pagina **Scegli una dimensione** fare clic sull'immagine **B1s** e quindi fare clic su **Seleziona**.

   > [!Note] 
   > L'immagine B1 dispone solo di 1 GB di RAM e sarà soggetta a errori di memoria al primo accesso. È tuttavia possibile ridimensionarla in un secondo momento in un esercizio di laboratorio successivo.

1. Nella pagina **Impostazioni**, in **Selezionare le porte in ingresso pubbliche** selezionare **Non sono state trovate porte in ingresso pubbliche**. Configureremo l'accesso RDP in un secondo momento.

### <a name="finish-configuring-the-vm-and-create-the-image"></a>Completare la configurazione della macchina virtuale e creare l'immagine

1. Scorrere la pagina fino in fondo e fare clic su **OK**.

1. In **Crea** controllare le impostazioni precedentemente configurate. Nella parte inferiore della pagina fare clic su **Crea**. Il dashboard di Azure mostrerà la macchina virtuale da distribuire. Questa operazione può richiedere alcuni minuti.

## <a name="summary"></a>Riepilogo

È stata a questo punto creata una macchina virtuale Windows Server idonea alle esigenze degli studenti e a cui si può accedere dalla rete dell'istituto di istruzione. Nell'unità successiva si esaminerà come connettersi alla macchina virtuale tramite RDP e come gestirla.
