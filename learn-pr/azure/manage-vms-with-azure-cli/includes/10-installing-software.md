Resta solo da provare a installare un server Web nella macchina virtuale. Uno dei pacchetti più semplici da installare è `nginx`.

1. Individuare l'indirizzo IP pubblico della macchina virtuale di Linux. È possibile usare il comando `vm list-ip-addresses` per cercarlo.

1. Aprire quindi una connessione `ssh` alla macchina come fatto in precedenza durante il test. Tenere presente che è necessario specificare il nome amministratore ("**aldis**").

1. Nella shell presentata, eseguire il comando seguente per installare il server Web `nginx`.

```azurecli
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. In Azure Cloud Shell usare `curl` per leggere la pagina predefinita del server Web di Linux. In alternativa, è possibile aprire una nuova scheda del browser e navigare all'indirizzo IP pubblico.

```azurecli
curl 168.61.54.62
```

Non verrà completata perché la macchina virtuale Linux non espone la porta 80 (`http`) attraverso il firewall incorporato. Per fortuna, l'interfaccia della riga comando di Azure dispone di un comando apposito: `vm open-port`. 

1. Per aprire la porta 80 in Cloud Shell, digitare quanto segue:

```
az vm open-port --port 80 --resource-group ExerciseResources --name SampleVM
```

Infine, provare `curl` nuovamente. Questa volta dovrebbe restituire i dati. È possibile vedere la pagina anche in un browser.
