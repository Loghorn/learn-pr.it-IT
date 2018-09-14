Si supponga che si esegue un'attività in linea e si esegue la pubblicazione del database in Azure. Per impostazione predefinita, un database di Server SQL di Azure sarà accessibile a qualsiasi applicazione di Azure o un servizio in esecuzione all'interno di Azure. Anche se si consentono solo i servizi all'interno di Azure per la connessione, è consigliabile limitare le connessioni al database ancora a servizi e applicazioni approvate.

In questa unità, esamineremo come limitare l'accesso al database SQL di Azure tramite gli intervalli di indirizzi IP.

## <a name="overview"></a>Panoramica

Un nuovo database SQL di Azure, per impostazione predefinita, consentirà solo servizi di Azure per la connessione. Questa configurazione non significa che un servizio che esegue la connessione può ottenere l'accesso al contenuto del database, solo che è possibile provare a eseguire l'autenticazione sul database. Richiedere un servizio per l'autenticazione è più sicuro di senza restrizioni accesso a internet pubblico. È comunque necessario limitare l'accesso al database solo le applicazioni e servizi che ne hanno necessità.

Ad esempio, potrebbe essere un'applicazione ASP.NET Core comunicando con un database SQL di Azure. È l'applicazione ASP.NET Core che deve accedere al database Azure SQL Server. Si esaminerà come si sarebbe limita l'accesso alla rete al database dall'applicazione web.

## <a name="restricting-network-access-to-the-database"></a>Limitare l'accesso alla rete per il database

L'accesso di rete al database, consentendo solo l'accesso da un intervallo di indirizzi IP specifico.

![Screenshot del portale di Azure che mostra il server di creazione della regola firewall con una configurazione della restrizione IP descritta aggiunta.](../media-draft/2-setting-ip-address-ranges-on-firewall.png)

1. Per creare una regola firewall del server, immettere un **il nome della regola**, il **IP iniziale** indirizzo e il **indirizzo IP finale** indirizzo.
1. Quindi fare clic su **salvare** per registrare le modifiche.

Solo gli indirizzi IP elencati nelle regole create avranno accesso al database.

## <a name="locking-down-access-at-the-database-level"></a>Blocco dell'accesso a livello di database

Si supponga che si esegue un gruppo di failover di Server SQL di Azure. Con un gruppo di failover, i server secondari sono solitamente disponibile in aree diverse. Se il server principale viene portato offline, le regole firewall del server potrebbero non più valide. Regole firewall del server vengono configurate per ogni server che ospita il database. Configurazione di una regola del firewall a livello di database assicura che le regole di replicano i database di backup.

Per creare una regola del firewall del database, connettersi al database usando SQL Server Management Studio o SQL Operations Studio e creare una nuova query. Si creerà una regola del database usando la convenzione seguente, dove si passano nel nome della regola, l'indirizzo IP iniziale e l'indirizzo IP finale.

```sql
EXECUTE sp_set_database_firewall_rule N'<Rule Name>', '<From IP Address>', '<To IP Address>'
```

Per limitare l'accesso al database dall'intervallo di indirizzi IP 10.21.2.33 – 10.21.2.54, ad esempio, si userà una regola simile per il codice SQL seguente:

```sql
EXECUTE sp_set_database_firewall_rule N'Web Apps Firewall Rule', '10.21.2.33', '10.21.2.54'
```

## <a name="enabling-transparent-data-encryption-tde"></a>Enabling Transparent Data Encryption (TDE)

### <a name="what-is-transparent-data-encryption"></a>Che cos'è Transparent Data Encryption?

Transparent Data Encryption (TDE) esegue la crittografia in tempo reale e la decrittografia dei database, i file di backup e i file di log.

Quando vengono creati i nuovi database SQL di Azure, hanno TDE abilitata per impostazione predefinita.

È importante verificare che la crittografia dei dati non è stato disattivato e database di Azure SQL Server meno recenti potrebbero non essere abilitata la funzionalità TDE.

Per verificare e abilitare Transparent Data Encryption:

1. Selezionare il database nel portale.
1. Selezionare l'opzione 'Transparent data encryption'.
1. Nell'opzione di crittografia dei dati, selezionare 'On'.
1. Fare clic su 'Salva'.

## <a name="create-a-secure-connection-to-the-server"></a>Creare una connessione protetta al server

Le applicazioni devono connettersi ai database in modo sicuro. Usare una stringa di connessione con il corretto livello di sicurezza per creare una connessione sicura. Queste connessioni devono essere crittografati in modo da ridurre la probabilità di un attacco man-in-the-middle.

Esaminiamo come ottenere la stringa di connessione per un database.

Tramite il portale, passare a SQL Server. Selezionare il database che si desidera ottenere l'accesso a.

Selezionare il *Mostra stringhe di connessione Database* opzione.

A questo punto, tra le opzioni disponibili, selezionare la scheda corrispondente il linguaggio di programmazione e copiare la stringa di connessione visualizzato. È possibile completare la password, come viene mantenuta segreta e non visualizzati qui.

![Screenshot del portale di Azure che mostra la sezione di stringhe di connessione di database con ADO.NET scelto e il campo del valore fornito evidenziato.](../media-draft/2-viewing-connection-strings.png)

È importante proteggere la stringa di connessione da parte dei clienti esterni. Le stringhe di connessione devono essere archiviate in Azure Key Vault, non nel progetto, il controllo delle versioni o sistemi di integrazione continua.

### <a name="what-is-the-azure-key-vault"></a>Che cos'è Azure Key Vault?

Azure Key Vault è uno strumento utilizzato per archiviare in modo sicuro le credenziali e altre chiavi e segreti. Questi segreti possono essere protette da software o hardware.

## <a name="open-the-correct-ports-for-server-access"></a>Aprire le porte appropriate per accedere al server

Il database di Azure consente comunicazioni in uscita sulla porta 1433. Se non hai accesso a questa porta, comunicare con gli amministratori di rete per consentire il traffico di rete.

## <a name="restrict-server-access-with-azure-virtual-networks"></a>Limitare l'accesso al server con reti virtuali di Azure

### <a name="what-is-a-virtual-network"></a>Informazioni sulla rete virtuale

Una rete virtuale è una rete isolata logicamente creata all'interno della rete di Azure. È possibile usare una rete virtuale per controllare quali le risorse di Azure possono connettersi ad altre risorse.

Si supponga che si sta eseguendo un'applicazione web che si connette a un database. Si userà le subnet per isolare le diverse parti della rete. Una subnet è una parte di una rete basata su un intervallo di indirizzi IP.

Per configurare queste subnet, viene creata una rete virtuale e quindi suddividere la rete in subnet. L'applicazione web operano su una subnet e il database in un'altra subnet. Ogni subnet avrà una proprio regole per la comunicazione da e verso l'altra rete. Queste regole consentono di limitare l'accesso dal database all'applicazione web.

### <a name="what-is-a-network-security-group"></a>Che cos'è un gruppo di sicurezza di rete?

Un gruppo di sicurezza di rete definisce regole che consentono o rifiutano il traffico di rete da e verso gli indirizzi di origine e destinazione. Ogni subnet avrà un gruppo di sicurezza di rete assegnato a esso.

Il diagramma seguente illustra un esempio dei raggruppamenti che vengono creati. La subnet web consente l'accesso a internet, ma solo per le connessioni HTTP. Subnet del database consente solo l'accesso dalla subnet di web. Configurazione di rete virtuale consente di aggiungere le restrizioni sul modo in cui servizi sono accessibili e agisce come un firewall che protegga i servizi ospitati.

![Rete virtuale per un'applicazione web con un database connesso](../media-draft/2-virtualnetwork-overview.png)

L'esempio seguente si presuppone che si usa macchine virtuali che fungono da host web e si connettono al database. Esaminiamo come creare una rete virtuale per configurare questa infrastruttura pianificata.

1. Dal portale di Azure, selezionare la **crea una risorsa** collegamento.
1. Da Azure Marketplace, selezionare **Networking** > **rete virtuale**. Se richiede di selezionare un modello di distribuzione, selezionare **Resource Manager**
1. Fare clic sul pulsante **Create** (Crea).
1. Immettere il **nome** per la rete virtuale.
1. Fornire un' **spazio degli indirizzi** che può essere utilizzato. Uno spazio indirizzi è un modo per la struttura di un intervallo di indirizzi IP. In questo esempio, `172.16.0.0/16` fa riferimento a un intervallo di indirizzi da 172.16.0.0 a 172.16.255.255.

   Verrà generate indicazioni per uno spazio di indirizzi che non sia già in uso. In cui viene rilevato un indirizzo IP da questo intervallo, verrà definita come si trova in questa subnet.

1. Selezionare di Azure **sottoscrizione**.
1. Selezionare o creare una nuova **gruppo di risorse**.
1. Immettere il **nome** della subnet che verrà creato.
1. Fornire un' **intervallo di indirizzi**.
1. Scegliere il **Create** pulsante per creare la rete virtuale.

![Screenshot del portale di Azure con la creazione di un pannello della rete virtuale con la configurazione descritta.](../media-draft/2-create-virtual-network-settings.png)

Dopo aver creata la rete virtuale, si riceverà una notifica. Fare clic sui **Vai alla risorsa** pulsante nella notifica.

Verrà ora visualizzata per il pannello rete virtuale. Selezionare il **le impostazioni** > **subnet** sezione di configurazione. A questo punto, è necessario che la subnet creata durante la configurazione di rete iniziale, denominata web_subnet.

È necessario creare un'altra subnet che rappresenterà gli indirizzi IP per i server di database.

1. Fare clic sui __+ Subnet__ per avviare il processo di aggiunta una nuova subnet per il database.

1. Immettere un **nome** per la subnet. Nell'esempio precedente, abbiamo lo ha chiamato database_subnet.

1. Rivedere le **intervallo di indirizzi (blocco CIDR)**. Si noterà che l'intervallo di indirizzi viene popolato con un intervallo che non è in conflitto con altre subnet nel sistema.

1. **Gruppo di sicurezza di rete** è un'impostazione di core che è necessario applicare alla subnet. Per il momento, lasciare vuota questa impostazione. In un secondo momento verrà creare il gruppo di sicurezza di rete e tornare indietro e impostare questo valore.

1. Scegliere il **OK** consente di salvare la subnet.

![Screenshot del portale di Azure che mostra il pannello subnet Add con la configurazione descritta.](../media-draft/2-create-database-subnet.png)

## <a name="creating-a-network-security-group"></a>Creazione di un gruppo di sicurezza di rete

Il processo del gruppo di sicurezza di rete è agire come un firewall, e controlla il flusso del traffico da e verso una subnet. È possibile creare due gruppi di sicurezza di rete. Una subnet dell'applicazione web e l'altra è per la subnet di database.

1. Selezionare **crea una risorsa** e selezionare **Networking** > **gruppo di sicurezza di rete**. Se viene richiesto di selezionare un modello di distribuzione, selezionare Resource Manager.
1. Fornire una **nome** per il gruppo di sicurezza di rete.
1. Selezionare la sottoscrizione di Azure, gruppo di risorse e posizione desiderata.
1. Fare clic sul pulsante **Create** (Crea).

![Screenshot del portale di Azure che mostra il gruppo di sicurezza web scelto per la subnet di web.](../media-draft/2-define-nsg-for-web-subnet.png)

Dopo aver creato il gruppo di sicurezza di rete, quindi è ora possibile configurare le regole di traffico in ingresso e in uscita per il gruppo di sicurezza.

1. Selezionare il nuovo gruppo di sicurezza di rete.
1. Nel **Panoramica** schermo, si noterà l'elenco di regole in ingresso e in uscita che sono stati creati. Predefinite sono le regole già create utilizzati per l'accesso di Azure interno.

    > [!NOTE]
    > Anche se non è possibile eliminare queste regole, è possibile creare regole aggiuntive con una priorità più alta che avrà la precedenza su queste regole.

![Screenshot del portale di Azure che mostra le regole di sicurezza preconfigurati in ingresso e in uscita di un gruppo di sicurezza di rete.](../media-draft/2-view-web-apps-security-group-rules.png)

1. Per creare nuove regole, selezionare la **regole di sicurezza in ingresso** sezione per il gruppo di sicurezza di rete.
1. Fare clic su **Add** . A questo punto, è possibile configurare i dettagli per le regole di sicurezza di rete.

    > [!NOTE]
    > Per impostazione predefinita, si noterà la vista avanzata per configurare la regola, ma facendo il **base** pulsante, sarà possibile selezionare il protocollo si vuole consentire.

1. Si vuole consentire l'accesso ai servizi HTTP da internet. Per filtrare per il protocollo HTTP, si selezionano **Tag del servizio** come la **origine** valore.
1. È quindi necessario verificare i **tag del servizio di origine** è impostata su **Internet**.
1. Set di valori per gli intervalli di porta `80,443` per rappresentare le porte usate per accedere a questo servizio. (Porta 80 viene usata per il protocollo HTTP, e la porta 443 viene usata per l'accesso HTTPS).
1. Selezionare **TCP** per il **protocollo**.
1. Impostare **azione** al **consentire**.
1. Assegnare alla regola un **priorità** pari a `100`.

    > [!NOTE]
    > Minore sarà il numero, più importante è la regola.

![Screenshot del portale di Azure che Mostra pannello della regola di sicurezza in ingresso Add con configurazione specificata e il tag di servizio di origine (Internet) e porta di destinazione gli intervalli di campi (80,443) evidenziati.](../media-draft/2-add-inbound-security-rule-for-web-nsg.png)

A questo punto è possibile configurare le impostazioni di gruppo di sicurezza di rete per il database. Per il database, si decide di configurare una singola regola in ingresso che consenta richieste SQL dall'intervallo IP della subnet applicazioni web e quindi negargli altro traffico in ingresso e in uscita. In questo modo sarà possibile controllare l'accesso al database e assicurarsi che solo le richieste di database ottenere tramite il sistema dal sito Web, e che nessun altro accesso al database è consentito.

1. Fare clic sui **crea una risorsa** per creare un'altra **Networking** > **gruppo di sicurezza di rete** che usa il modello di distribuzione Resource Manager.

    Questa fase si creerà uno per il database.

1. Fornire una **nome** per il gruppo di sicurezza di rete.
1. Selezionare la sottoscrizione di Azure, gruppo di risorse e posizione desiderata.
1. Scegliere il **Create** pulsante per creare il nuovo gruppo di sicurezza di rete.

    ![Screenshot del portale di Azure che mostra il pannello Crea rete sicurezza gruppo con una configurazione di esempio.](../media-draft/2-create-database-network-security-group.png)

    È necessario attendere fino a quando non viene creato il gruppo di sicurezza di rete.

1. Al termine, selezionare la **Vai alla risorsa** opzione della notifica per iniziare a configurare le regole per il gruppo di sicurezza di rete è stato creato.

Per il gruppo di sicurezza di rete, l'obiettivo principale è di consentire le richieste di database web dell'applicazione solo dalla subnet. È necessario configurare le richieste TCP in ingresso sulla porta 1433 dall'intervallo IP della subnet web.

1. Nel **regole di sicurezza in ingresso** impostazioni, selezionare la **Add** pulsante per creare una nuova regola. In questo caso, si desidera solo consentire l'accesso dalla subnet dell'applicazione web.
1. Si creerà una regola in ingresso che consente di impostare il **origine** al **gli indirizzi IP**.
1. Quando la **origine indirizzi IP/intervalli CIDR** vengono visualizzati, immettere l'intervallo CIDR da una subnet web che è stata creata in precedenza.
1. Per il **intervalli di porte di destinazione**, immettere `1433` per indicare solo l'accesso al Server SQL di Azure.
1. Impostare il **Protocol** al **TCP** per limitare ulteriormente le connessioni in ingresso.
1. Si desidera **Allow** accedere **azione**.
1. Assegnargli un **priorità** di `100`.
1. **Nome** la regola di sicurezza in modo appropriato.
1. Fare clic su **Add** per aggiungere la regola.

![Screenshot del portale di Azure che Mostra pannello della regola di sicurezza in ingresso Aggiungi la configurazione descritta.](../media-draft/2-create-inbound-rule-for-db-security-group.png)

Le regole di sicurezza di base sono ora disponibili per limitare l'accesso al sistema di database. Ciò che resta è per assegnare i gruppi di sicurezza di rete alle subnet.

1. Aprire la rete virtuale creata in precedenza. Selezionare il **le impostazioni** > **subnet** sezione.
1. Selezionare la subnet web creata in precedenza.
1. Selezionare il **gruppo di sicurezza di rete** opzione e il gruppo di sicurezza di rete creato in modo specifico per il web.
1. Fare clic su **salvare** e il gruppo di sicurezza di rete verrà applicato a subnet.

    ![Screenshot del portale di Azure che illustra un gruppo di sicurezza di rete applicato per la subnet web](../media-draft/2-define-nsg-for-web-subnet.png)

Si desidera ripetere lo stesso processo per la subnet di database.

1. Ritornare alla subnet.
1. Selezionare la subnet di database
1. Impostare relativi **gruppo di sicurezza di rete** al gruppo di sicurezza di rete per il database.
1. È consigliabile fare clic su **Salva** per salvare le modifiche.

    ![Screenshot del portale di Azure che illustra un gruppo di sicurezza di rete applicato scelto per la subnet di database.](../media-draft/2-define-nsg-for-db-subnet.png)

Ora che è stato configurato l'accesso, si tratta dell'applicazione le reti virtuali e subnet per il server di database e server web.

Iniziamo con il server di database.

1. Selezionare il database.
1. Il database, selezionare la **firewall e reti virtuali** impostazione di configurazione.

A sinistra, si noterà i dettagli sulla configurazione. È presente un numero di impostazioni in gioco.

1. In primo luogo, attivare **consentire l'accesso ai servizi di Azure** al **OFF**. Questo modo si garantisce che siano abilitati solo i servizi che si desidera utilizzare.

    Si noterà anche che mostra un indirizzo IP del Client. L'indirizzo IP del Client è l'indirizzo IP del computer in uso la connessione al database del Server SQL di Azure. Si sarebbe possibile aggiungere un indirizzo IP del Client all'elenco Nome regola. Ciò è utile se si desidera connettersi al server SQL Server Management Studio o SQL Operations Studio. Aggiungerà una regola che indica quali indirizzi IP può connettersi. È non possibile questa operazione in questo caso, ma verrà impostato configurata la rete virtuale.

1. Fare clic su **Aggiungi rete virtuale esistente**, e verrà visualizzata una schermata di opzioni per immettere i dettagli per la nuova regola.

1. Immettere il **nome** della regola da utilizzare.
1. Selezionare la sottoscrizione che erano in uso.
1. In particolare, selezionare la rete virtuale che si sta utilizzando.
1. Selezionare la subnet con le regole di gruppo di sicurezza di rete appropriata per l'accesso al database.
1. Dopo aver configurato tutte le impostazioni, selezionare la **abilitare** pulsante. Il database verrà quindi applicata alla subnet all'interno della rete virtuale.

Dopo aver configurata la subnet di database, si eseguirà una procedura simile con il server web. Indipendentemente dal modo in cui è stato configurato l'applicazione, è una questione di garantire che le applicazioni web di usano la subnet per accesso web.

Se si dispone di una singola macchina virtuale o un servizio di bilanciamento del carico con le macchine virtuali in un set di scalabilità, assicurarsi che utilizzano la subnet web hanno accesso al database. Quando si creano risorse, ad esempio le macchine virtuali, assicurarsi che le subnet e della rete virtuale siano configurate per controllare le informazioni da inserire in e out di tali servizi.

![Screenshot del portale di Azure che illustra il passaggio del pannello delle impostazioni di creazione di una nuova macchina virtuale in cui i valori di rete e subnet virtuali sono stati impostati.](../media-draft/2-configure-virtual-machine-with-subnet.png)

Una volta applicate alle subnet per il database e le macchine virtuali che eseguono le app web, la configurazione appropriata sarà posto. Quindi è possibile accedere più forte tra le App e database.

Sicurezza di rete è il primo punto di base di protezione. Assicurarsi che solo le applicazioni e servizi che devono connettersi al database di connettersi al database di sistema verrà rendere più sicura.
