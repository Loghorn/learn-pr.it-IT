Quando si crea una macchina virtuale, vengono assegnati un indirizzo IP pubblico che è raggiungibile tramite Internet e un indirizzo IP privato usato all'interno del data center di Azure. Entrambi questi valori vengono restituiti nel blocco JSON dal comando `create`:

```json
{
   ...
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
   ...
}
```

## <a name="connecting-to-the-vm-with-ssh"></a>Connessione alla macchina virtuale con SSH

Con l'indirizzo IP pubblico si può testare rapidamente il funzionamento della macchina virtuale Linux usando lo strumento Secure Shell (`ssh`). Tenere presente che il nome dell'amministratore è impostato su `aldis` e pertanto è necessario specificare questo nome. Assicurarsi di usare l'indirizzo IP pubblico dalla _propria_ istanza di esecuzione.

```azurecli
ssh <public-ip-address> -l aldis
```

> [!NOTE]
> La password non è necessaria poiché è stata generata una coppia di chiavi SSH come parte della creazione della macchina virtuale. La prima volta che si lancia la macchina virtuale, verrà visualizzata una richiesta di conferma dell'autenticità dell'host. 
> 
> Questo avviene perché stiamo raggiungendo direttamente un indirizzo IP anziché un nome host. Rispondendo "Sì" verrà salvato l'indirizzo IP come un host valido per la connessione e verrà consentito alla connessione di procedere.

```output
The authenticity of host '40.83.165.85 (40.83.165.85)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '40.83.165.85' (RSA) to the list of known hosts.
```

Verrà quindi visualizzata una shell remota in cui è possibile immettere i comandi di Linux.

```output
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

Provare alcuni comandi per fare pratica e al termine disconnettersi dalla shell (digitare `logout` o `exit` nella shell).