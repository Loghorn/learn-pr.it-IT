Ora che è disponibile una macchina virtuale Windows in Azure, il passaggio successivo consiste nell'aggiungere applicazioni e dati in queste tali macchine virtuali per elaborare i video sul traffico. 

Tuttavia, se non è stata configurata una VPN da sito a sito in Azure, le macchine virtuali di Azure non saranno accessibili dalla rete locale. Se si è agli inizi con l'uso di Azure, è improbabile che sia disponibile una rete VPN da sito a sito funzionante. Allora in che modo è possibile trasferire file alle macchine virtuali di Azure? Un modo semplice prevede l'uso della funzionalità Connessioni Desktop remoto di Azure per condividere le unità locali con le nuove macchine virtuali di Azure.

Ora che è disponibile una nuova macchina virtuale Windows, è necessario installarvi il software personalizzato. È possibile eseguire questa operazione in due modi:

- Remote Desktop Protocol (RDP)
- Script personalizzati
- Immagini di macchine virtuali personalizzate (con il software preinstallato)

Verrà ora esaminato l'approccio più semplice per le macchine virtuali Windows: Desktop remoto.

## <a name="what-is-the-remote-desktop-protocol"></a>Cos'è Remote Desktop Protocol (RDP)?

Remote Desktop Protocol (RDP) fornisce connettività remota all'interfaccia utente dei computer basati su Windows. Il protocollo RDP consente di accedere a un computer fisico remoto o virtuale di Windows e controllare il computer come dalla console. Una connessione RDP consente di svolgere la maggior parte delle operazioni che è possibile eseguire dalla console di un computer fisico, ad eccezione di alcune funzioni di alimentazione e relative all'hardware.

Una connessione RDP richiede un client RDP. Microsoft fornisce client RDP per i sistemi operativi seguenti:

- Windows (predefinito)
- MacOS
- iOS
- Android

Lo screenshot seguente illustra il client Remote Desktop Protocol in Windows 10.

![Screenshot dell'interfaccia utente del client Remote Desktop Protocol.](../media/4-rdp-client.png)

Sono disponibili anche client Linux open source, ad esempio Remmina, che consentono di connettersi a un PC Windows da una distribuzione Ubuntu.

## <a name="connecting-to-an-azure-vm"></a>Connessione a una macchina virtuale di Azure

Come si è visto poco fa, le macchine virtuali di Azure comunicano su una rete virtuale. Possono anche avere un indirizzo IP pubblico facoltativo assegnato. Con un IP pubblico, è possibile comunicare con la macchina virtuale tramite Internet. In alternativa, è possibile configurare una rete privata virtuale (VPN) che connette la rete locale ad Azure, che consente la connessione sicura alla macchina virtuale senza esporre un indirizzo IP pubblico. Questo approccio è descritto in un altro modulo ed è completamente documentato se si è interessati a esplorare tale opzione.

Un aspetto da tenere presente per gli indirizzi IP pubblici in Azure è che spesso vengono allocati in modo dinamico. Ciò significa che l'indirizzo IP può cambiare nel tempo e per le macchine virtuali ciò avviene al riavvio. È possibile pagare una tariffa aggiuntiva per assegnare indirizzi statici, se ci si vuole connettere direttamente a un indirizzo IP invece che a un nome ed è necessario fare in modo che l'indirizzo IP non cambi.

### <a name="how-do-you-connect-to-a-vm-in-azure-using-rdp"></a>In che modo è possibile connettersi a una macchina virtuale in Azure tramite RDP?

La connessione a una macchina virtuale in Azure tramite RDP è un processo semplice. Nel portale di Azure passare alle proprietà della macchina virtuale e nella parte superiore fare clic su **Connetti**. Verranno visualizzati gli indirizzi IP assegnati alla macchina virtuale e verrà offerta la possibilità di scaricare un file **RDP** preconfigurato che verrà aperto da Windows nel client RDP. È possibile scegliere di connettersi tramite l'indirizzo IP pubblico della macchina virtuale nel file RDP. In alternativa, se ci si connette tramite VPN o ExpressRoute, è possibile selezionare l'indirizzo IP interno. È anche possibile selezionare il numero di porta per la connessione.

Se si usa un indirizzo IP pubblico statico per la macchina virtuale, è possibile salvare il file **RDP** sul desktop. Se si usano indirizzi IP dinamici, il file **RDP** rimane valido solo mentre la macchina virtuale è in esecuzione. Se si arresta e si riavvia la macchina virtuale, è necessario scaricare un altro file **RDP**.

> [!TIP]
> È anche possibile immettere l'indirizzo IP pubblico della macchina virtuale nel client RDP di Windows e fare clic su **Connetti**.

Quando ci si connette, in genere verranno visualizzati due avvisi. Si tratta di:

-**Avviso editore**: generato dal file **RDP** non firmato pubblicamente.
- **Avviso certificato**: generato dal certificato del computer non considerato attendibile.

Negli ambienti di test è possibile ignorare questi avvisi. Negli ambienti di produzione il file **RDP** può essere firmato usando **RDPSIGN.EXE** e il certificato del computer inserito nell'archivio del client **Autorità di certificazione radice attendibili**.

Si proverà ora a usare RDP per connettersi alla macchina virtuale.