Come amministratori di rete presso una società di analisi di dati, si è responsabili per la connessione alle macchine virtuali in Azure e di verificare la corretta configurazione. Per effettuare tale operazione, usare un client RDP (Remote Desktop Protocol) per la connessione all'interfaccia utente del desktop di Windows della macchina virtuale.

## <a name="what-is-the-remote-desktop-protocol"></a>Cos'è Remote Desktop Protocol (RDP)?

Remote Desktop Protocol (RDP) fornisce la connettività remota all'interfaccia utente dei computer basati su Windows. Il protocollo RDP consente di accedere a un computer fisico remoto o virtuale di Windows e controllare il computer come dalla console.

> [!Note]
> Windows Hyper-V in Windows Server 2016 e Windows 10 usa RDP per connettersi alle macchine virtuali in esecuzione su hypervisor.

Una connessione RDP consente di svolgere la maggior parte delle operazioni che è possibile eseguire dalla console di un computer fisico, ad eccezione di alcune funzioni di alimentazione e relative all'hardware.

Una connessione RDP richiede un client RDP. Microsoft fornisce client RDP per i sistemi operativi seguenti:

* Windows
* iOS
* MacOS
* Android

Sono disponibili anche client Linux open source, ad esempio Remmina che consentono di connettersi a un PC Windows da una distribuzione Ubuntu.

Windows 10 include un client RDP.

![Client RDP Windows](../media-drafts/4-rdp-client.PNG)

## <a name="what-functionality-does-an-rdp-connection-support"></a>Quali funzionalità sono supportate da una connessione RDP?

Con una connessione RDP, è possibile interagire con l'interfaccia utente. Tuttavia, è anche possibile connettersi ad altri servizi nel computer remoto. Tali servizi includono:

* Connessioni alle stampanti che consentono al computer remoto di stampare sul dispositivo di stampa locale.
* Riproduzione audio, che consente di riprodurre l'audio nel computer locale o nel dispositivo remoto.
* Registrazione audio, che consente di registrare l'audio dal computer locale e indirizzare l'audio al dispositivo remoto.
* Porte, che è possibile reindirizzare alle porte nel computer locale.
* Le unità, incluse le unità di rete mappate, in cui le unità verranno visualizzate come connesse al computer remoto.
* Dispositivi di acquisizione video, ad esempio webcam integrata.
* Altri dispositivi Plug and Play supportati.

Non tutte queste funzionalità sono supportate in Azure, poiché sono presenti restrizioni relative a quali risorse fisiche sono disponibili su tale piattaforma. Ad esempio, solo le stampanti software possono essere reindirizzate attraverso una connessione RDP da una macchina virtuale ospitata in Azure ai dispositivi di stampa del client di connessione.

## <a name="configure-network-settings-for-rdp-access-to-virtual-machines"></a>Configurare le impostazioni di rete per l'accesso RDP alle macchine virtuali

È possibile effettuare le connessioni alle macchine virtuali di Azure in tre modi:

1. Direttamente all'indirizzo IP pubblico della macchina virtuale.
2. Tramite una connessione VPN (rete privata virtuale).
3. Tramite una connessione ExpressRoute

Un indirizzo IP pubblico può essere allocato in modo definitivo o dinamica. È anche necessario assicurarsi di avere accesso all'indirizzo pubblico della macchina virtuale sulla porta 3389 (RDP). Questa disposizione può richiedere la negoziazione con il team che gestisce la sicurezza del firewall dell'organizzazione , configurando una regola all'indirizzo IP pubblico della macchina virtuale sulla porta 3389.

Gli indirizzi IP pubblici dinamici vengono riallocati ogni volta che la macchina virtuale viene riavviata. Gli indirizzi statici sono persistenti, ma hanno costi maggiori.

Per connettersi tramite una VPN, la rete locale deve avere una connessione VPN attiva a Microsoft Azure.

L'approccio con collegamento ExpressRoute gestisce la connettività alla macchina virtuale. Questo approccio offre anche la latenza più bassa e la connessione di larghezza di banda più elevata.

> [!Note]
> Indipendentemente dall'opzione usata per connettersi ad Azure, è necessario assicurarsi che la macchina virtuale sia accessibile tramite RDP, in genere sulla porta predefinita 3389. È possibile configurare la macchina virtuale in modo che sia accessibile solo dal proprio indirizzo IP client. La connessione a una macchina virtuale tramite la porta 3389 su un indirizzo IP pubblico è consigliata solo per gli ambienti di test. Negli ambienti di produzione, usare l'opzione 2 o 3.

## <a name="how-do-you-connect-to-a-vm-in-azure-using-rdp"></a>In che modo è possibile connettersi a una macchina virtuale in Azure tramite RDP?

La connessione a una macchina virtuale in Azure tramite RDP è un processo semplice. Nel portale di Azure, accedere alle proprietà della macchina virtuale e nella parte superiore, fare clic su **Connetti**. Questa azione scarica un file .RDP preconfigurato che verrà quindi aperto da Windows nel client RDP. È possibile scegliere di connettersi tramite l'indirizzo IP pubblico della macchina virtuale nel file RDP. In alternativa, se ci si connette tramite VPN o ExpressRoute, è possibile selezionare l'indirizzo IP interno. È anche possibile selezionare il numero di porta per la connessione.

Se si usa un indirizzo IP pubblico statico per la macchina virtuale, è possibile salvare il file .RDP sul desktop. Se si usano indirizzi IP dinamici, il file .RDP rimane valido solo mentre la macchina virtuale è in esecuzione. Se si arresta e si riavvia la macchina virtuale, è necessario scaricare un altro file .RDP.

> [!Note]
> È anche possibile immettere l'indirizzo IP pubblico della macchina virtuale nel client RDP di Windows e fare clic su **Connetti**.

Quando ci si connette, in genere verranno visualizzati due avvisi. Si tratta di:

* Avviso editore: a causa del file .RDP che non è stato firmato pubblicamente.
* Avviso certificato: causato dal certificato del computer non considerato attendibile.

Negli ambienti di test, è possibile ignorare questi avvisi. Negli ambienti di produzione il file .RDP può essere firmato usando RDPSIGN.EXE e il certificato del computer inserito nell'archivio del client **Autorità di certificazione radice attendibili**.

## <a name="summary"></a>Riepilogo

Adesso è possibile connettersi a una macchina virtuale Windows tramite RDP. Nell'esercizio seguente verrà usato RDP per connettersi a una macchina virtuale, quindi verrà installato un ruolo del server nel computer.
