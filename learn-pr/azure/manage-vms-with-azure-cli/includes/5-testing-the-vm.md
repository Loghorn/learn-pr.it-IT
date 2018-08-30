<span data-ttu-id="bc9a5-101">Quando si crea una macchina virtuale, viene assegnato un indirizzo IP pubblico che è raggiungibile tramite Internet e un indirizzo IP privato usato all'interno di data center di Azure.</span><span class="sxs-lookup"><span data-stu-id="bc9a5-101">When you create a virtual machine, it gets assigned a public IP address that is reachable over the Internet, and a private IP address used within the Azure data center.</span></span> <span data-ttu-id="bc9a5-102">È possibile verificare rapidamente che la VM Linux sia attiva e in esecuzione usando lo strumento `ssh`.</span><span class="sxs-lookup"><span data-stu-id="bc9a5-102">We can quickly test that the Linux VM is up and running using the `ssh` tool.</span></span> <span data-ttu-id="bc9a5-103">Tenere presente che il nome dell'amministratore è impostato su `aldis` e, pertanto, è necessario specificarlo.</span><span class="sxs-lookup"><span data-stu-id="bc9a5-103">Remember that we set our admin name to `aldis`, so we have to specify that.</span></span>

```azurecli
ssh 168.61.54.62 -l aldis
```

> [!NOTE]
> <span data-ttu-id="bc9a5-104">Non è necessaria una password poiché è stata generata una coppia di chiavi SSH come parte della creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bc9a5-104">We don't need a password because we generated an SSH key pair as part of the VM creation.</span></span> <span data-ttu-id="bc9a5-105">La prima volta che si lancia la macchina virtuale, verrà visualizzata una richiesta di conferma dell'autenticità dell'host.</span><span class="sxs-lookup"><span data-stu-id="bc9a5-105">The first time you shell into the VM, it will give you a prompt about the authenticity of the host.</span></span> 
> 
> <span data-ttu-id="bc9a5-106">Questo avviene perché stiamo raggiungendo direttamente un indirizzo IP anziché un nome host.</span><span class="sxs-lookup"><span data-stu-id="bc9a5-106">This is because we are hitting an IP address directly instead of a host name.</span></span> <span data-ttu-id="bc9a5-107">Rispondendo "Sì" verrà salvato l'indirizzo IP come un host valido per la connessione e verrà consentito alla connessione di procedere.</span><span class="sxs-lookup"><span data-stu-id="bc9a5-107">Answering "yes" will save the IP as a valid host for connection and allow the connection to proceed.</span></span>

```
The authenticity of host '168.61.54.62 (168.61.54.62)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '168.61.54.62' (RSA) to the list of known hosts.
```

<span data-ttu-id="bc9a5-108">Quindi verrà mostrata una shell remota in cui è possibile immettere i comandi di Linux.</span><span class="sxs-lookup"><span data-stu-id="bc9a5-108">Then you'll be presented with a remote shell where you can enter Linux commands.</span></span>

```
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

<span data-ttu-id="bc9a5-109">Provare alcuni comandi per comprenderne il funzionamento e al termine, disconnettersi dall'account (tipo `logout` o `exit` nella shell).</span><span class="sxs-lookup"><span data-stu-id="bc9a5-109">Try a few commands as practice and when you are finished, sign out of your account (type `logout` or `exit` in the shell).</span></span>