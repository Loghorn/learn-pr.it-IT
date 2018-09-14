Quando si crea una macchina virtuale, viene assegnato un indirizzo IP pubblico che è raggiungibile tramite Internet e un indirizzo IP privato usato all'interno di data center di Azure. È possibile verificare rapidamente che la VM Linux sia attiva e in esecuzione usando lo strumento `ssh`. Tenere presente che il nome dell'amministratore è impostato su `aldis` e, pertanto, è necessario specificarlo.

```azurecli
ssh 168.61.54.62 -l aldis
```

> [!NOTE]
> Non è necessaria una password poiché è stata generata una coppia di chiavi SSH come parte della creazione della macchina virtuale. La prima volta che si lancia la macchina virtuale, verrà visualizzata una richiesta di conferma dell'autenticità dell'host. 
> 
> Questo avviene perché stiamo raggiungendo direttamente un indirizzo IP anziché un nome host. Rispondendo "Sì" verrà salvato l'indirizzo IP come un host valido per la connessione e verrà consentito alla connessione di procedere.

```
The authenticity of host '168.61.54.62 (168.61.54.62)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '168.61.54.62' (RSA) to the list of known hosts.
```

Quindi verrà mostrata una shell remota in cui è possibile immettere i comandi di Linux.

```
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

Provare alcuni comandi per comprenderne il funzionamento e al termine, disconnettersi dall'account (tipo `logout` o `exit` nella shell).