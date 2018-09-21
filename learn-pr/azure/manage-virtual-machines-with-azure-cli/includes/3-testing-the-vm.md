<span data-ttu-id="09ae7-101">Quando si crea una macchina virtuale, vengono assegnati un indirizzo IP pubblico che è raggiungibile tramite Internet e un indirizzo IP privato usato all'interno del data center di Azure.</span><span class="sxs-lookup"><span data-stu-id="09ae7-101">When you create a virtual machine, it gets assigned a public IP address that is reachable over the Internet, and a private IP address used within the Azure data center.</span></span> <span data-ttu-id="09ae7-102">Entrambi questi valori vengono restituiti nel blocco JSON dal comando `create`:</span><span class="sxs-lookup"><span data-stu-id="09ae7-102">You get both of those values in the returning JSON block from the `create` command:</span></span>

```json
{
   ...
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
   ...
}
```

## <a name="connecting-to-the-vm-with-ssh"></a><span data-ttu-id="09ae7-103">Connessione alla macchina virtuale con SSH</span><span class="sxs-lookup"><span data-stu-id="09ae7-103">Connecting to the VM with SSH</span></span>

<span data-ttu-id="09ae7-104">Con l'indirizzo IP pubblico si può testare rapidamente il funzionamento della macchina virtuale Linux usando lo strumento Secure Shell (`ssh`).</span><span class="sxs-lookup"><span data-stu-id="09ae7-104">With the public IP address we can quickly test that the Linux VM is up and running using the Secure Shell (`ssh`) tool.</span></span> <span data-ttu-id="09ae7-105">Tenere presente che il nome dell'amministratore è impostato su `aldis` e pertanto è necessario specificare questo nome.</span><span class="sxs-lookup"><span data-stu-id="09ae7-105">Remember that we set our admin name to `aldis`, so we have to specify that.</span></span> <span data-ttu-id="09ae7-106">Assicurarsi di usare l'indirizzo IP pubblico dalla _propria_ istanza di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="09ae7-106">Make sure to use the public IP address from _your_ running instance.</span></span>

```azurecli
ssh <public-ip-address> -l aldis
```

> [!NOTE]
> <span data-ttu-id="09ae7-107">La password non è necessaria poiché è stata generata una coppia di chiavi SSH come parte della creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="09ae7-107">We don't need a password because we generated an SSH key pair as part of the VM creation.</span></span> <span data-ttu-id="09ae7-108">La prima volta che si lancia la macchina virtuale, verrà visualizzata una richiesta di conferma dell'autenticità dell'host.</span><span class="sxs-lookup"><span data-stu-id="09ae7-108">The first time you shell into the VM, it will give you a prompt about the authenticity of the host.</span></span> 
> 
> <span data-ttu-id="09ae7-109">Questo avviene perché stiamo raggiungendo direttamente un indirizzo IP anziché un nome host.</span><span class="sxs-lookup"><span data-stu-id="09ae7-109">This is because we are hitting an IP address directly instead of a host name.</span></span> <span data-ttu-id="09ae7-110">Rispondendo "Sì" verrà salvato l'indirizzo IP come un host valido per la connessione e verrà consentito alla connessione di procedere.</span><span class="sxs-lookup"><span data-stu-id="09ae7-110">Answering "yes" will save the IP as a valid host for connection and allow the connection to proceed.</span></span>

```output
The authenticity of host '40.83.165.85 (40.83.165.85)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '40.83.165.85' (RSA) to the list of known hosts.
```

<span data-ttu-id="09ae7-111">Verrà quindi visualizzata una shell remota in cui è possibile immettere i comandi di Linux.</span><span class="sxs-lookup"><span data-stu-id="09ae7-111">Then you'll be presented with a remote shell where you can enter Linux commands.</span></span>

```output
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

<span data-ttu-id="09ae7-112">Provare alcuni comandi per fare pratica e al termine disconnettersi dalla shell (digitare `logout` o `exit` nella shell).</span><span class="sxs-lookup"><span data-stu-id="09ae7-112">Try a few commands as practice and when you are finished, sign out of the shell (type `logout` or `exit` in the shell).</span></span>