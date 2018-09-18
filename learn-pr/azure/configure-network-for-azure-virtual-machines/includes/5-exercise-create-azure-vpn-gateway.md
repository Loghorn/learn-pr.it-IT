Garantire la connessione di client o siti del proprio ambiente in Azure con tunnel crittografati sulla rete Internet pubblica. In questa unità verrà creato un gateway VPN da punto a sito e quindi si stabilirà la connessione a tale gateway da un computer client. Per la sicurezza si useranno connessioni con autenticazione del certificato nativa di Azure.

La procedura è la seguente:

1. Creare un gateway VPN di tipo RouteBased.

1. Caricare la chiave pubblica per un certificato radice ai fini dell'autenticazione.

1. Generare un certificato client dal certificato radice e quindi installare il certificato client in ogni computer client che si connetterà alla rete virtuale ai fini dell'autenticazione.

1. Creare i file di configurazione del client VPN con le informazioni necessarie per la connessione del client alla rete virtuale.

## <a name="before-you-begin"></a>Prima di iniziare
<!---TODO: These should be prerequisites in the first unit and on the index.yml--->

Per completare il modulo è necessario quanto segue:

- Azure PowerShell

- Cartella denominata C:\cert

Per installare Azure PowerShell:

1. Fare clic con il pulsante destro del mouse sul pulsante Windows e scegliere **Windows PowerShell (amministratore)**.

1. Nella finestra del messaggio **Controllo dell'account utente** fare clic su **Sì**.

1. Nella finestra di PowerShell digitare il comando seguente e premere INVIO:

    ```PowerShell
    Import-Module AzureRM
    ```

1. Nella richiesta di sicurezza digitare A e premere INVIO.

## <a name="sign-in-and-set-variables"></a>Accedere e impostare le variabili

Per accedere e impostare le variabili, eseguire questa procedura:

1. Per connettersi ad Azure, digitare il comando seguente e premere INVIO:

    ```PowerShell
    Connect-AzureRmAccount
    ```

1. Ottenere un elenco delle sottoscrizioni di Azure.

    ```PowerShell
    Get-AzureRmSubscription
    ```
1. Specificare la sottoscrizione da usare.

    ```PowerShell
    Select-AzureRmSubscription -SubscriptionName "{Name of your subscription}"
    ```

    Sostituire "Nome della sottoscrizione" con il nome della propria sottoscrizione.

1. Immettere le variabili seguenti e premere INVIO dopo ognuna.

    ```PowerShell
    $VNetName  = "VNetData"
    $FESubName = "FrontEnd"
    $BESubName = "Backend"
    $GWSubName = "GatewaySubnet"
    $VNetPrefix1 = "192.168.0.0/16"
    $VNetPrefix2 = "10.254.0.0/16"
    $FESubPrefix = "192.168.1.0/24"
    $BESubPrefix = "10.254.1.0/24"
    $GWSubPrefix = "192.168.200.0/26"
    $VPNClientAddressPool = "172.16.201.0/24"
    $RG = "TestRG"
    $Location = "East US"
    $GWName = "VNetDataGW"
    $GWIPName = "VNetDataGWPIP"
    $GWIPconfName = "gwipconf"
    ```

## <a name="configure-a-virtual-network"></a>Configurare una rete virtuale

1. Creare un gruppo di risorse.

    ```PowerShell
    New-AzureRmResourceGroup -Name $RG -Location $Location
    ```

1. Creare le configurazioni delle subnet per la rete virtuale. Queste sono denominate **FrontEnd, BackEnd** e **GatewaySubnet**. Tutte queste subnet sono presenti nel prefisso di rete virtuale.

    ```PowerShell
    $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
    $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
    $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
    ```

1. Creare la rete virtuale usando i valori delle subnet e un server DNS statico.

    > [!IMPORTANT]
    > Ignorare il messaggio di avviso e attendere il completamento del comando.

    ```PowerShell
    New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
    ```

1. A questo punto, specificare le variabili per la rete appena creata.

    ```PowerShell
    $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    ```

1. Richiedere un indirizzo IP pubblico assegnato dinamicamente.

    ```PowerShell
    $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
    $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
    ```

## <a name="create-the-vpn-gateway"></a>Creare il gateway VPN

Durante la creazione del gateway VPN:

- GatewayType deve essere Vpn
- VpnType deve essere RouteBased

> [!NOTE]
> Si noti che il completamento di questa parte dell'esercizio può richiedere fino a 45 minuti, a seconda del valore di GatewaySku.

1. Per creare il gateway VPN, eseguire questo comando e premere INVIO.

    ```PowerShell
    New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
    -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
    -VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
    ```

1. Attendere che venga visualizzato l'output del comando.

## <a name="add-the-vpn-client-address-pool"></a>Aggiungere il pool di indirizzi client VPN

1. Eseguire il comando seguente e premere INVIO.

    ```PowerShell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
    ```

1. Attendere che venga visualizzato l'output del comando.

## <a name="generate-a-client-certificate"></a>Generare un certificato client

1. Creare un certificato radice autofirmato.

    ```PowerShell
    $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
    -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
    ```

1. Generare un certificato client.

    ```PowerShell
    New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
    -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
    ```

1. Esportare la chiave pubblica del certificato radice. Nel computer client digitare il comando seguente e premere INVIO.

    ```PowerShell
    certmgr
    ```

1. Passare a Personale/Certificati. Fare clic con il pulsante destro del mouse su P2SRootCert, scegliere **Tutte le attività** e quindi selezionare **Esporta**.

1. In Esportazione guidata certificati fare clic su **Avanti**.

1. Assicurarsi che l'opzione **No, non esportare la chiave privata** sia selezionata e quindi fare clic su **Avanti**.

1. Nella pagina **Formato file di esportazione** assicurarsi che l'opzione **Codificato Base 64 X.509 (.CER)** sia selezionata e quindi fare clic su **Avanti**.

1. Nella pagina **File da esportare** immettere **C:\cert\P2SRootCert.cer** in **Nome file** e quindi fare clic su Avanti.

1. Nella pagina **Completamento dell'Esportazione guidata certificati** fare clic su **Fine**.

1. Nella finestra di messaggio **Esportazione guidata certificati** fare clic su **OK**.

## <a name="upload-the-root-certificate-public-key-information"></a>Caricare le informazioni sulla chiave pubblica del certificato radice

1. Nella finestra di PowerShell eseguire il comando seguente per dichiarare una variabile per il nome del certificato:

    ```PowerShell
    $P2SRootCertName = "P2SRootCert.cer"
    ```

1. Eseguire il comando seguente:

    ```PowerShell
    $filePathForCert = "C:\cert\P2SRootCert.cer"
    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
    $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
    $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
    ```

1. A questo punto, caricare il certificato in Azure. Azure lo riconosce come un certificato radice attendibile.

    ```PowerShell
    Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNetDataGW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
    ```

## <a name="configure-the-native-vpn-client"></a>Configurare il client VPN nativo

1. Eseguire il comando seguente per creare i file di configurazione del client VPN in formato ZIP.

    ```PowerShell
    $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNetDataGW" -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. Copiare l'URL restituito nell'output del comando e incollarlo nel browser. Il browser avvierà il download di un file ZIP. Decomprimere il file e salvarlo nel percorso desiderato.

1. Nella cartella estratta passare alla cartella WindowsAmd64 (per i computer Windows a 64 bit) o alla cartella WindowsZX86 (per i computer a 32 bit).

1. Fare doppio clic sul file VpnClientSetupxxxxx.exe, a seconda dell'architettura in uso.

1. Nella schermata **PC protetto da Windows** fare clic su **Altre informazioni** e quindi su **Esegui comunque**.

1. Nella finestra di dialogo **Controllo dell'account utente** fare clic su **Sì**.

1. Nella finestra di dialogo **VNetData** fare clic su **Sì**.

## <a name="connect-to-azure"></a>Connettersi ad Azure

1. Premere il tasto Windows, digitare **Impostazioni** e premere INVIO.

1. Nella finestra **Impostazioni** fare clic su **Rete e Internet**.

1. Nel riquadro a sinistra fare clic su **VPN**.

1. Nel riquadro a destra fare clic su **VNetData** e quindi su **Connetti**.

1. Nella finestra VNetData fare clic su **Connetti**.

1. Nella finestra VNetData successiva fare clic su **Continua**.

1. Nella finestra del messaggio **Controllo dell'account utente** fare clic su **Sì**.

> [!NOTE]
> Se questa procedura non funziona, potrebbe essere necessario riavviare il computer.

## <a name="verify-your-connection"></a>Verificare la connessione

1. Premere il tasto Windows, digitare **cmd** e premere INVIO. Verrà visualizzata la finestra del **prompt dei comandi**.

1. Digitare `IPCONFIG /ALL` e premere INVIO.

1. Copiare l'indirizzo IP visualizzato in Scheda PPP VNetData o prendere nota dell'indirizzo.

1. Confermare che l'indirizzo IP sia compreso nell'**intervallo VPNClientAddressPool di 172.16.201.0/24**.

1. La connessione al gateway VPN di Azure è riuscita.

## <a name="summary"></a>Riepilogo

È stato configurato un gateway VPN per stabilire una connessione client crittografata a una rete virtuale in Azure. Questo approccio è ideale per i computer client e le connessioni da sito a sito di dimensioni ridotte.