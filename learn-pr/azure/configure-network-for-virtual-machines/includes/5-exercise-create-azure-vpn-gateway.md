<span data-ttu-id="faa1c-101">Garantire la connessione di client o siti del proprio ambiente in Azure con tunnel crittografati sulla rete Internet pubblica.</span><span class="sxs-lookup"><span data-stu-id="faa1c-101">You want to ensure that you can connect clients or sites within your environment into Azure using encrypted tunnels across the public internet.</span></span> <span data-ttu-id="faa1c-102">In questa unità verrà creato un gateway VPN da punto a sito e quindi si stabilirà la connessione a tale gateway da un computer client.</span><span class="sxs-lookup"><span data-stu-id="faa1c-102">In this unit, you'll create a point-to-site VPN gateway, and then connect to that gateway from a client computer.</span></span> <span data-ttu-id="faa1c-103">Per la sicurezza si useranno connessioni con autenticazione del certificato nativa di Azure.</span><span class="sxs-lookup"><span data-stu-id="faa1c-103">You'll use native Azure certificate authentication connections for security.</span></span>

<span data-ttu-id="faa1c-104">La procedura è la seguente:</span><span class="sxs-lookup"><span data-stu-id="faa1c-104">You will carry out the following process:</span></span>

1. <span data-ttu-id="faa1c-105">Creare un gateway VPN di tipo RouteBased.</span><span class="sxs-lookup"><span data-stu-id="faa1c-105">Create a RouteBased VPN gateway.</span></span>

1. <span data-ttu-id="faa1c-106">Caricare la chiave pubblica per un certificato radice ai fini dell'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="faa1c-106">Upload the public key for a root certificate for authentication purposes.</span></span>

1. <span data-ttu-id="faa1c-107">Generare un certificato client dal certificato radice e quindi installare il certificato client in ogni computer client che si connetterà alla rete virtuale ai fini dell'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="faa1c-107">Generate a client certificate from the root certificate, and then install the client certificate on each client computer that will connect to the virtual network for authentication purposes.</span></span>

1. <span data-ttu-id="faa1c-108">Creare i file di configurazione del client VPN con le informazioni necessarie per la connessione del client alla rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="faa1c-108">Create VPN client configuration files, which contain the necessary information for the client to connect to the virtual network.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="faa1c-109">Prima di iniziare</span><span class="sxs-lookup"><span data-stu-id="faa1c-109">Before you begin</span></span>
<!---TODO: These should be prerequisites in the first unit and on the index.yml--->

<span data-ttu-id="faa1c-110">Per completare il modulo è necessario quanto segue:</span><span class="sxs-lookup"><span data-stu-id="faa1c-110">To complete this module, you must have:</span></span>

- <span data-ttu-id="faa1c-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="faa1c-111">Azure PowerShell</span></span>

- <span data-ttu-id="faa1c-112">Cartella denominata C:\cert</span><span class="sxs-lookup"><span data-stu-id="faa1c-112">A folder called C:\cert</span></span>

<span data-ttu-id="faa1c-113">Per installare Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="faa1c-113">To install Azure PowerShell:</span></span>

1. <span data-ttu-id="faa1c-114">Fare clic con il pulsante destro del mouse sul pulsante Windows e scegliere **Windows PowerShell (amministratore)**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-114">Right-click the Windows button and click **PowerShell (Admin)**.</span></span>

1. <span data-ttu-id="faa1c-115">Nella finestra del messaggio **Controllo dell'account utente** fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-115">In the **User Account Control** message box, click **Yes**.</span></span>

1. <span data-ttu-id="faa1c-116">Nella finestra di PowerShell digitare il comando seguente e premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="faa1c-116">In the PowerShell window, type the following command and press Enter:</span></span>

    ```PowerShell
    Import-Module AzureRM
    ```

1. <span data-ttu-id="faa1c-117">Nella richiesta di sicurezza digitare A e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="faa1c-117">At the security prompt, type A and press Enter.</span></span>

## <a name="sign-in-and-set-variables"></a><span data-ttu-id="faa1c-118">Accedere e impostare le variabili</span><span class="sxs-lookup"><span data-stu-id="faa1c-118">Sign in and set variables</span></span>

<span data-ttu-id="faa1c-119">Per accedere e impostare le variabili, eseguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="faa1c-119">To sign in and set variables, carry out the following steps:</span></span>

1. <span data-ttu-id="faa1c-120">Per connettersi ad Azure, digitare il comando seguente e premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="faa1c-120">Connect to Azure by entering the following command and pressing Enter:</span></span>

    ```PowerShell
    Connect-AzureRmAccount
    ```

1. <span data-ttu-id="faa1c-121">Ottenere un elenco delle sottoscrizioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="faa1c-121">Get a list of your Azure subscriptions.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
1. <span data-ttu-id="faa1c-122">Specificare la sottoscrizione da usare.</span><span class="sxs-lookup"><span data-stu-id="faa1c-122">Specify the subscription that you want to use.</span></span>

    ```PowerShell
    Select-AzureRmSubscription -SubscriptionName "{Name of your subscription}"
    ```

    <span data-ttu-id="faa1c-123">Sostituire "Nome della sottoscrizione" con il nome della propria sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="faa1c-123">Replace "Name of subscription" with your subscription name.</span></span>

1. <span data-ttu-id="faa1c-124">Immettere le variabili seguenti e premere INVIO dopo ognuna.</span><span class="sxs-lookup"><span data-stu-id="faa1c-124">Enter the following variables and press Enter after each one.</span></span>

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

## <a name="configure-a-virtual-network"></a><span data-ttu-id="faa1c-125">Configurare una rete virtuale</span><span class="sxs-lookup"><span data-stu-id="faa1c-125">Configure a virtual network</span></span>

1. <span data-ttu-id="faa1c-126">Creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="faa1c-126">Create a resource group.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name $RG -Location $Location
    ```

1. <span data-ttu-id="faa1c-127">Creare le configurazioni delle subnet per la rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="faa1c-127">Create subnet configurations for the virtual network.</span></span> <span data-ttu-id="faa1c-128">Queste sono denominate **FrontEnd, BackEnd** e **GatewaySubnet**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-128">These have the name **FrontEnd, BackEnd**, and **GatewaySubnet**.</span></span> <span data-ttu-id="faa1c-129">Tutte queste subnet sono presenti nel prefisso di rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="faa1c-129">All of these subnets exist within the virtual network prefix.</span></span>

    ```PowerShell
    $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
    $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
    $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
    ```

1. <span data-ttu-id="faa1c-130">Creare la rete virtuale usando i valori delle subnet e un server DNS statico.</span><span class="sxs-lookup"><span data-stu-id="faa1c-130">Create the virtual network using the subnet values and a static DNS server.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="faa1c-131">Ignorare il messaggio di avviso e attendere il completamento del comando.</span><span class="sxs-lookup"><span data-stu-id="faa1c-131">Ignore the warning message, and then wait for the command to finish.</span></span>

    ```PowerShell
    New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
    ```

1. <span data-ttu-id="faa1c-132">A questo punto, specificare le variabili per la rete appena creata.</span><span class="sxs-lookup"><span data-stu-id="faa1c-132">Now specify the variables for this network that you have just created.</span></span>

    ```PowerShell
    $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    ```

1. <span data-ttu-id="faa1c-133">Richiedere un indirizzo IP pubblico assegnato dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="faa1c-133">Request a dynamically assigned public IP address.</span></span>

    ```PowerShell
    $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
    $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
    ```

## <a name="create-the-vpn-gateway"></a><span data-ttu-id="faa1c-134">Creare il gateway VPN</span><span class="sxs-lookup"><span data-stu-id="faa1c-134">Create the VPN gateway</span></span>

<span data-ttu-id="faa1c-135">Durante la creazione del gateway VPN:</span><span class="sxs-lookup"><span data-stu-id="faa1c-135">When creating this VPN gateway:</span></span>

- <span data-ttu-id="faa1c-136">GatewayType deve essere Vpn</span><span class="sxs-lookup"><span data-stu-id="faa1c-136">GatewayType must be Vpn</span></span>
- <span data-ttu-id="faa1c-137">VpnType deve essere RouteBased</span><span class="sxs-lookup"><span data-stu-id="faa1c-137">VpnType must be RouteBased</span></span>

> [!NOTE]
> <span data-ttu-id="faa1c-138">Si noti che il completamento di questa parte dell'esercizio può richiedere fino a 45 minuti, a seconda del valore di GatewaySku.</span><span class="sxs-lookup"><span data-stu-id="faa1c-138">Note that this part of the exercise can take up to 45 minutes to complete, depending on the value of GatewaySku.</span></span>

1. <span data-ttu-id="faa1c-139">Per creare il gateway VPN, eseguire questo comando e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="faa1c-139">To create the VPN gateway, run the following command and press Enter.</span></span>

    ```PowerShell
    New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
    -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
    -VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
    ```

1. <span data-ttu-id="faa1c-140">Attendere che venga visualizzato l'output del comando.</span><span class="sxs-lookup"><span data-stu-id="faa1c-140">Wait for the command output to appear.</span></span>

## <a name="add-the-vpn-client-address-pool"></a><span data-ttu-id="faa1c-141">Aggiungere il pool di indirizzi client VPN</span><span class="sxs-lookup"><span data-stu-id="faa1c-141">Add the VPN client address pool</span></span>

1. <span data-ttu-id="faa1c-142">Eseguire il comando seguente e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="faa1c-142">Run the following command and press Enter.</span></span>

    ```PowerShell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
    ```

1. <span data-ttu-id="faa1c-143">Attendere che venga visualizzato l'output del comando.</span><span class="sxs-lookup"><span data-stu-id="faa1c-143">Wait for the command output to appear.</span></span>

## <a name="generate-a-client-certificate"></a><span data-ttu-id="faa1c-144">Generare un certificato client</span><span class="sxs-lookup"><span data-stu-id="faa1c-144">Generate a client certificate</span></span>

1. <span data-ttu-id="faa1c-145">Creare un certificato radice autofirmato.</span><span class="sxs-lookup"><span data-stu-id="faa1c-145">Create the self-signed root certificate.</span></span>

    ```PowerShell
    $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
    -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
    ```

1. <span data-ttu-id="faa1c-146">Generare un certificato client.</span><span class="sxs-lookup"><span data-stu-id="faa1c-146">Generate a client certificate.</span></span>

    ```PowerShell
    New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
    -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
    ```

1. <span data-ttu-id="faa1c-147">Esportare la chiave pubblica del certificato radice.</span><span class="sxs-lookup"><span data-stu-id="faa1c-147">Export the root certificate public key.</span></span> <span data-ttu-id="faa1c-148">Nel computer client digitare il comando seguente e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="faa1c-148">On your client computer, type the following command and press Enter.</span></span>

    ```PowerShell
    certmgr
    ```

1. <span data-ttu-id="faa1c-149">Passare a Personale/Certificati.</span><span class="sxs-lookup"><span data-stu-id="faa1c-149">Navigate to Personal/Certificates.</span></span> <span data-ttu-id="faa1c-150">Fare clic con il pulsante destro del mouse su P2SRootCert, scegliere **Tutte le attività** e quindi selezionare **Esporta**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-150">Right-click P2SRootCert, click **All tasks**, and then select **Export**.</span></span>

1. <span data-ttu-id="faa1c-151">In Esportazione guidata certificati fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-151">In the Certificate Export Wizard, click **Next**.</span></span>

1. <span data-ttu-id="faa1c-152">Assicurarsi che l'opzione **No, non esportare la chiave privata** sia selezionata e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-152">Ensure that **No, do not export the private key** is selected, and then click **Next**.</span></span>

1. <span data-ttu-id="faa1c-153">Nella pagina **Formato file di esportazione** assicurarsi che l'opzione **Codificato Base 64 X.509 (.CER)** sia selezionata e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-153">On the **Export File Format** page, ensure that **Base-64 encoded X.509 (.CER)** is selected, and then click **Next**.</span></span>

1. <span data-ttu-id="faa1c-154">Nella pagina **File da esportare** immettere **C:\cert\P2SRootCert.cer** in **Nome file** e quindi fare clic su Avanti.</span><span class="sxs-lookup"><span data-stu-id="faa1c-154">In the **File to Export** page, under **File name**, enter **C:\cert\P2SRootCert.cer**, and then click Next.</span></span>

1. <span data-ttu-id="faa1c-155">Nella pagina **Completamento dell'Esportazione guidata certificati** fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-155">On the **Completing the Certificate Export Wizard** page, click **Finish**.</span></span>

1. <span data-ttu-id="faa1c-156">Nella finestra di messaggio **Esportazione guidata certificati** fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-156">On the **Certificate Export Wizard** message box, click **OK**.</span></span>

## <a name="upload-the-root-certificate-public-key-information"></a><span data-ttu-id="faa1c-157">Caricare le informazioni sulla chiave pubblica del certificato radice</span><span class="sxs-lookup"><span data-stu-id="faa1c-157">Upload the root certificate public key information</span></span>

1. <span data-ttu-id="faa1c-158">Nella finestra di PowerShell eseguire il comando seguente per dichiarare una variabile per il nome del certificato:</span><span class="sxs-lookup"><span data-stu-id="faa1c-158">In the PowerShell window, execute the following command to declare a variable for the certificate name:</span></span>

    ```PowerShell
    $P2SRootCertName = "P2SRootCert.cer"
    ```

1. <span data-ttu-id="faa1c-159">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="faa1c-159">Execute the following command:</span></span>

    ```PowerShell
    $filePathForCert = "C:\cert\P2SRootCert.cer"
    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
    $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
    $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
    ```

1. <span data-ttu-id="faa1c-160">A questo punto, caricare il certificato in Azure.</span><span class="sxs-lookup"><span data-stu-id="faa1c-160">Now upload the certificate to Azure.</span></span> <span data-ttu-id="faa1c-161">Azure lo riconosce come un certificato radice attendibile.</span><span class="sxs-lookup"><span data-stu-id="faa1c-161">Azure then recognizes it as a trusted root certificate.</span></span>

    ```PowerShell
    Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNetDataGW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
    ```

## <a name="configure-the-native-vpn-client"></a><span data-ttu-id="faa1c-162">Configurare il client VPN nativo</span><span class="sxs-lookup"><span data-stu-id="faa1c-162">Configure the native VPN client</span></span>

1. <span data-ttu-id="faa1c-163">Eseguire il comando seguente per creare i file di configurazione del client VPN in formato ZIP.</span><span class="sxs-lookup"><span data-stu-id="faa1c-163">Execute the following command to create VPN client configuration files in .ZIP format.</span></span>

    ```PowerShell
    $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNetDataGW" -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. <span data-ttu-id="faa1c-164">Copiare l'URL restituito nell'output del comando e incollarlo nel browser.</span><span class="sxs-lookup"><span data-stu-id="faa1c-164">Copy the URL returned in the output from this command and paste it into your browser.</span></span> <span data-ttu-id="faa1c-165">Il browser avvierà il download di un file ZIP.</span><span class="sxs-lookup"><span data-stu-id="faa1c-165">Your browser should start downloading a .ZIP file.</span></span> <span data-ttu-id="faa1c-166">Decomprimere il file e salvarlo nel percorso desiderato.</span><span class="sxs-lookup"><span data-stu-id="faa1c-166">Unzip it and put it in a suitable location.</span></span>

1. <span data-ttu-id="faa1c-167">Nella cartella estratta passare alla cartella WindowsAmd64 (per i computer Windows a 64 bit) o alla cartella WindowsZX86 (per i computer a 32 bit).</span><span class="sxs-lookup"><span data-stu-id="faa1c-167">In the extracted folder, navigate to either the WindowsAmd64 folder (for 64-bit Windows computers) or the WindowsZX86 folder (for 32-bit computers).</span></span>

1. <span data-ttu-id="faa1c-168">Fare doppio clic sul file VpnClientSetupxxxxx.exe, a seconda dell'architettura in uso.</span><span class="sxs-lookup"><span data-stu-id="faa1c-168">Double-click on the VpnClientSetupxxxxx.exe file, depending on your architecture.</span></span>

1. <span data-ttu-id="faa1c-169">Nella schermata **PC protetto da Windows** fare clic su **Altre informazioni** e quindi su **Esegui comunque**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-169">In the **Windows protected your PC** screen, click **More info**, and then click **Run anyway**.</span></span>

1. <span data-ttu-id="faa1c-170">Nella finestra di dialogo **Controllo dell'account utente** fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-170">In the **User Account Control** dialog box, click **Yes**.</span></span>

1. <span data-ttu-id="faa1c-171">Nella finestra di dialogo **VNetData** fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-171">In the **VNetData** dialog box, click **Yes**.</span></span>

## <a name="connect-to-azure"></a><span data-ttu-id="faa1c-172">Connettersi ad Azure</span><span class="sxs-lookup"><span data-stu-id="faa1c-172">Connect to Azure</span></span>

1. <span data-ttu-id="faa1c-173">Premere il tasto Windows, digitare **Impostazioni** e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="faa1c-173">Press the Windows key, type **Settings** and press Enter.</span></span>

1. <span data-ttu-id="faa1c-174">Nella finestra **Impostazioni** fare clic su **Rete e Internet**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-174">In the **Settings** window, click **Network and Internet**.</span></span>

1. <span data-ttu-id="faa1c-175">Nel riquadro a sinistra fare clic su **VPN**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-175">In the left-hand pane, click **VPN**.</span></span>

1. <span data-ttu-id="faa1c-176">Nel riquadro a destra fare clic su **VNetData** e quindi su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-176">In the right-hand pane, click **VNetData**, and then click **Connect**.</span></span>

1. <span data-ttu-id="faa1c-177">Nella finestra VNetData fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-177">In the VNetData window, click **Connect**.</span></span>

1. <span data-ttu-id="faa1c-178">Nella finestra VNetData successiva fare clic su **Continua**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-178">In the next VNetData window, click **Continue**.</span></span>

1. <span data-ttu-id="faa1c-179">Nella finestra del messaggio **Controllo dell'account utente** fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-179">In the **User Account Control** message box, click **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="faa1c-180">Se questa procedura non funziona, potrebbe essere necessario riavviare il computer.</span><span class="sxs-lookup"><span data-stu-id="faa1c-180">If these steps do not work, you may need to restart your computer.</span></span>

## <a name="verify-your-connection"></a><span data-ttu-id="faa1c-181">Verificare la connessione</span><span class="sxs-lookup"><span data-stu-id="faa1c-181">Verify your connection</span></span>

1. <span data-ttu-id="faa1c-182">Premere il tasto Windows, digitare **cmd** e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="faa1c-182">Press the Windows key, type **cmd** and press Enter.</span></span> <span data-ttu-id="faa1c-183">Verrà visualizzata la finestra del **prompt dei comandi**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-183">The **Command Prompt** window appears.</span></span>

1. <span data-ttu-id="faa1c-184">Digitare `IPCONFIG /ALL` e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="faa1c-184">Type `IPCONFIG /ALL` and press Enter.</span></span>

1. <span data-ttu-id="faa1c-185">Copiare l'indirizzo IP visualizzato in Scheda PPP VNetData o prendere nota dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="faa1c-185">Copy the IP address under PPP adapter VNetData, or write it down.</span></span>

1. <span data-ttu-id="faa1c-186">Confermare che l'indirizzo IP sia compreso nell'**intervallo VPNClientAddressPool di 172.16.201.0/24**.</span><span class="sxs-lookup"><span data-stu-id="faa1c-186">Confirm that IP address is in the **VPNClientAddressPool range of 172.16.201.0/24**.</span></span>

1. <span data-ttu-id="faa1c-187">La connessione al gateway VPN di Azure è riuscita.</span><span class="sxs-lookup"><span data-stu-id="faa1c-187">You have successfully made a connection to the Azure VPN gateway.</span></span>

## <a name="summary"></a><span data-ttu-id="faa1c-188">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="faa1c-188">Summary</span></span>

<span data-ttu-id="faa1c-189">È stato configurato un gateway VPN per stabilire una connessione client crittografata a una rete virtuale in Azure.</span><span class="sxs-lookup"><span data-stu-id="faa1c-189">You just set up a VPN gateway, allowing you to make an encrypted client connection to a virtual network in Azure.</span></span> <span data-ttu-id="faa1c-190">Questo approccio è ideale per i computer client e le connessioni da sito a sito di dimensioni ridotte.</span><span class="sxs-lookup"><span data-stu-id="faa1c-190">This approach is great with client computers and smaller site-to-site connections.</span></span>