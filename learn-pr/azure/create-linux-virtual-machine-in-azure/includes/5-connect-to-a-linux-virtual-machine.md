Dopo aver creato una macchina virtuale Linux in Azure, il passaggio successivo consiste nel configurarla per le attività da spostare in Azure.

Se non è stata configurata una VPN da sito a sito in Azure, le macchine virtuali di Azure non saranno accessibili dalla rete locale. Se si sta iniziando a usare Azure, è improbabile che sia presente una VPN da sito a sito funzionante. Come è possibile quindi connettersi alla macchina virtuale?

## <a name="azure-vm-ip-addresses"></a>Indirizzi IP della VM di Azure

Come si è visto poco fa, le VM di Azure comunicano in una rete virtuale. Possono anche avere un indirizzo IP pubblico facoltativo assegnato. Con un IP pubblico è possibile interagire con la macchina virtuale tramite Internet. In alternativa, è possibile configurare una rete privata virtuale (VPN, Virtual Private Network) che connette la rete locale ad Azure, permettendo di connettersi in modo sicuro alla macchina virtuale senza esporre un indirizzo IP pubblico. Questo approccio è descritto in un altro modulo ed è completamente documentato, se si è interessati a esplorare tale opzione.

Un aspetto da tenere presente per gli indirizzi IP pubblici in Azure è che spesso vengono allocati in modo dinamico. Ciò significa che l'indirizzo IP può cambiare nel corso del tempo - per le macchine virtuali, ciò si verifica quando la VM viene riavviata. È possibile pagare una tariffa aggiuntiva per assegnare indirizzi statici, se ci si vuole connettere direttamente a un indirizzo IP invece che a un nome ed è necessario fare in modo che l'indirizzo IP non cambi.

## <a name="connect-to-an-azure-linux-vm-with-ssh"></a>Connettersi a una VM Linux di Azure con SSH

La connessione a una macchina virtuale in Azure usando SSH è semplice. Nel portale di Azure, aprire le proprietà della macchina virtuale e nella parte superiore, fare clic su **Connect**. Verranno visualizzati gli indirizzi IP assegnati alla macchina virtuale insieme a tutti i dettagli di accesso per SSH. 

![Screenshot del portale di Azure che illustra la connessione a un pannello della macchina virtuale configurata per connettersi tramite SSH alla VM Linux appena creato.](../media/5-connect-ssh.png)

Con questi dettagli è possibile usare il comando seguente per accedere alla macchina virtuale Linux:

```bash
ssh jim@137.117.101.249
```

La prima volta che ci si connette, SSH richiederà l'autenticazione con un host sconosciuto. SSH informa che non è stata mai stabilita una connessione a questo server. Se è vero, quindi è perfettamente normale, è possibile rispondere con **Sì** per salvare l'impronta digitale del server nel file di host note:

```output
The authenticity of host '137.117.101.249 (137.117.101.249)' can't be established.
ECDSA key fingerprint is SHA256:w1h08h4ie1iMq7ibIVSQM/PhcXFV7O7EEhjEqhPYMWY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '137.117.101.249' (ECDSA) to the list of known hosts.
```

## <a name="transferring-files-to-the-vm"></a>Trasferimento di file nella macchina virtuale

Un'altra attività comune consiste nel copiare i dati o i file locali da un server a un altro. In questo caso, verranno copiati i dati del sito Web in una macchina virtuale di Azure. Con macchine virtuali Linux e SSH è possibile usare il comando `scp`. Il comando è simile alla copia dei file locali con `cp`, ma sarà necessario specificare l'utente remoto e l'host nel comando.

Ad esempio, per copiare `somefile.txt` dalla directory corrente in `~/folder` in un computer Linux con l'indirizzo IP `192.168.1.25`, è possibile digitare:

```bash
scp somefile.txt someuser@192.168.1.25:~/folder/
```

Verrà eseguita l'autenticazione come `someuser` nel sistema remoto richiedendo una password o usando la chiave privata SSH. Tale utente dovrà avere le autorizzazioni di scrittura per `~/folder/` nel server di destinazione.

> [!WARNING]
> `scp` eseguirà le copie dei file locali se la riga di comando non è corretta. Il componente mancante più comune è la cartella di destinazione.

Provare a usare SSH per connettersi a una macchina virtuale Linux in esecuzione.