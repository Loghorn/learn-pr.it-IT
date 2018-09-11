Prima di poter creare una macchina virtuale Linux in Azure, è necessario riflettere sull'accesso remoto. Occorre essere in grado di accedere al server Web Linux per configurare il software ed eseguire la manutenzione. L'approccio predefinito per amministrare le macchine virtuali Linux ospitate in Azure è SSH.

## <a name="what-is-ssh"></a>Che cos'è SSH?

SSH (Secure Shell) è un protocollo per connessioni crittografate che consente accessi protetti su connessioni non sicure. SSH consente di connettersi a una shell del terminale da una posizione remota tramite una connessione di rete.

Esistono due approcci che è possibile usare per autenticare una connessione SSH: **nome utente/password** o una **coppia di chiavi SSH**. 

> [!TIP]
> Sebbene SSH fornisca una connessione crittografata, se si usano password con le connessioni SSH la macchina virtuale rimane vulnerabile agli attacchi di forza bruta o di individuazione delle password. Un metodo più sicuro e preferito per la connessione a una macchina virtuale Linux tramite SSH è con una coppia di chiavi pubblica e privata, dette anche chiavi SSH.

Con una coppia di chiavi SSH, è possibile accedere a macchine virtuali di Azure basate su Linux senza una password. Si tratta di un approccio più sicuro, se si intende accedere solo da pochi computer. Se è necessario essere in grado di accedere alla macchina virtuale Linux da svariate posizioni, una combinazione di nome utente e password potrebbe essere un approccio migliore. Esistono due parti di una coppia di chiavi SSH: una chiave pubblica e una chiave privata.

* La chiave pubblica viene posizionata nella macchina virtuale Linux o in qualsiasi altro servizio che si vuole usare con la crittografia a chiave pubblica. Questa chiave può essere condivisa con chiunque.

* La chiave privata è quella che si presenta alla macchina virtuale Linux quando ci si connette a SSH, per verificare la propria identità. Considerare questa chiave un'informazione riservata e proteggerla come nel caso di una password o altri dati privati.

È possibile usare la stessa singola coppia di chiavi pubblica e privata per accedere a più macchine virtuali e servizi di Azure.

## <a name="create-the-ssh-key-pair"></a>Creare la coppia di chiavi SSH

In Linux, Windows 10 e MacOS è possibile usare il comando `ssh-keygen` predefinito per generare i file di chiave pubblica e privata SSH. 

> [!TIP]
> Windows 10 include un client SSH con Fall Creators Update. Le versioni precedenti di Windows richiedono software aggiuntivo. Se si prevede di usare Windows con SSH, [vedere la documentazione per i dettagli completi](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows). In alternativa, è possibile installare il sottosistema Linux per Windows e ottenere la stessa funzionalità.

> [!NOTE]
> Si userà Azure Cloud Shell che archivierà le chiavi generate in Azure nell'account di archiviazione privato. È anche possibile digitare questi comandi direttamente nella shell locale se si preferisce. Sarà necessario adattare le istruzioni in tutto il modulo a una sessione locale, se si adotta questo approccio.

Ecco il comando minimo necessario per generare la coppia di chiavi per una macchina virtuale di Azure. Verrà creata una coppia di chiavi pubblica-privata RSA SSH-2 con una lunghezza di 2048 bit (la lunghezza minima). 

Digitare questo comando in Cloud Shell.

```bash
ssh-keygen -t rsa -b 2048
```

Lo strumento richiederà i nomi per i file e una passphrase facoltativa. Accettare i valori predefiniti. Verranno creati due file, `id_rsa` e `id_rsa.pub`, nella directory `~/.ssh`. I file vengono sovrascritti se esistono. Sono disponibili diverse opzioni che è possibile usare per specificare il nome del file o una passphrase per evitare la richiesta.

### <a name="private-key-passphrase"></a>Passphrase per la chiave privata

È anche possibile specificare una passphrase facoltativamente durante la generazione della chiave privata. Si tratta di una password che è necessario immettere quando si usa la chiave. Questa passphrase viene usata per accedere al file della chiave SSH privata e non è la password dell'account utente. 

Quando si aggiunge una passphrase alla chiave SSH, la chiave privata viene crittografata con AES a 128 bit, in modo che sia inutilizzabile senza la passphrase per decrittografarla. 

> [!IMPORTANT]
> È **vivamente** consigliabile aggiungere una passphrase. Se un utente malintenzionato ruba una chiave privata priva di passphrase, può usarla per accedere ai server che hanno la chiave pubblica corrispondente. Se una chiave privata è protetta da una passphrase, non può essere usata da utenti malintenzionati e rappresenta un livello di sicurezza aggiuntivo per l'infrastruttura in Azure.

Ecco un esempio che mostra come impostare la passphrase. Non è necessario eseguire questo comando (sebbene sia possibile).

```bash
ssh-keygen -t rsa -b 4096 \
    -C "azureuser@myserver" \
    -f ~/.ssh/mykeys/myprivatekey \
    -N someReallySecurePhraseYouWillRemember
```

| Parametro | Risultato |
|-----------|--------------|
| `-t` | Tipo di chiave da creare. Deve essere **rsa**. |
| `-b` | Numero di bit nella chiave. La lunghezza minima è 2048, quella massima 4096. |
| `-C` | Commento facoltativo da aggiungere alla chiave pubblica che può essere usato per facilitarne l'identificazione. Si tratta in genere di un indirizzo di posta elettronica, ma è testo semplice ed è possibile usare il metodo di identificazione preferito. |
| `-f` | Percorso e nome del file della chiave privata. Un file di chiave pubblica corrispondente con estensione **pub** viene generato nella stessa directory. La directory deve esistere. |
| `-N` | Passphrase usata per crittografare la chiave privata. |

## <a name="use-the-ssh-key-pair-with-an-azure-linux-vm"></a>Usare la coppia di chiavi SSH con una macchina virtuale Linux di Azure

Dopo aver generato la coppia di chiavi, è possibile usarla con una macchina virtuale Linux in Azure. È possibile fornire la chiave pubblica durante la creazione della macchina virtuale o aggiungerla dopo aver creato la macchina virtuale. 

È possibile visualizzare i contenuti del file in Azure Cloud Shell con il comando seguente.

```bash
cat ~/.ssh/id_rsa.pub
```

Sarà una singola riga simile a:

```output
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXGX/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

Copiare questo valore in modo da poterlo usare nel prossimo esercizio.

### <a name="use-the-ssh-key-when-creating-a-linux-vm"></a>Usare la chiave SSH durante la creazione di una macchina virtuale Linux

Per applicare la chiave SSH durante la creazione di una nuova macchina virtuale Linux, sarà necessario copiare il contenuto della chiave pubblica e specificarlo nel portale di Azure _oppure_ specificare il file di chiave pubblica per il comando dell'interfaccia della riga di comando di Azure o di Azure PowerShell. Si userà questo approccio durante la creazione della macchina virtuale Linux.

### <a name="add-the-ssh-key-to-an-existing-linux-vm"></a>Aggiungere la chiave SSH a una macchina virtuale Linux esistente

Se è già stata creata una macchina virtuale, è possibile installare la chiave pubblica nella macchina virtuale Linux con il comando `ssh-copy-id`. Una volta autorizzata per SSH, la chiave concede l'accesso al server senza una password.

Passare il file di chiave pubblica e il nome utente da associare alla chiave.

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```
Ora che è disponibile la chiave pubblica è possibile passare al portale di Azure e creare una macchina virtuale Linux.

