Resta solo da provare a installare un server Web nella macchina virtuale. Uno dei pacchetti più semplici da installare è `nginx`.

## <a name="install-nginx-web-server"></a>Installare il server Web NGINX

1. Individuare l'indirizzo IP pubblico della macchina virtuale di Linux. È possibile usare il comando `vm list-ip-addresses` per cercarlo.

1. Aprire quindi una connessione `ssh` alla macchina come fatto in precedenza durante il test. Tenere presente che è necessario specificare il nome amministratore ("**aldis**").

1. Nella shell presentata, eseguire il comando seguente per installare il server Web `nginx`.

```bash
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. Chiudere la Secure Shell.

```bash
exit
```

## <a name="retrieve-our-default-page"></a>Recuperare la pagina predefinita

1. In Azure Cloud Shell usare `curl` per leggere la pagina predefinita del server Web di Linux. In alternativa, è possibile aprire una nuova scheda del browser e navigare all'indirizzo IP pubblico.

```azurecli
curl 40.83.165.85
```

Non verrà completata perché la macchina virtuale Linux non espone la porta 80 (`http`) attraverso il firewall incorporato. Per fortuna, l'interfaccia della riga comando di Azure dispone di un comando apposito: `vm open-port`. 

1. Per aprire la porta 80 in Cloud Shell, digitare quanto segue:

```azurecli
az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM
```

L'aggiunta della regola della rete e l'apertura della porta nel firewall richiederanno alcuni minuti. Provare `curl` di nuovo. Questa volta dovrebbe restituire i dati. È possibile vedere la pagina anche in un browser.

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx on Debian!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx on Debian!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working on Debian. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a></p>

<p>
      Please use the <tt>reportbug</tt> tool to report bugs in the
      nginx package with Debian. However, check <a
      href="http://bugs.debian.org/cgi-bin/pkgreport.cgi?ordering=normal;archive=0;src=nginx;repeatmerged=0">existing
      bug reports</a> before reporting a new bug.
</p>

<p><em>Thank you for using debian and nginx.</em></p>


</body>
</html>
```
