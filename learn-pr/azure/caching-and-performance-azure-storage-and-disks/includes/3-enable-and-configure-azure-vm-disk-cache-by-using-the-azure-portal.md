È possibile configurare le impostazioni della cache del disco della macchina virtuale con uno qualsiasi degli strumenti seguenti:

- Portale di Azure
- Modelli di Resource Manager
- Interfaccia della riga di comando di Azure
- Azure PowerShell

Nel prossimo esercizio si userà il portale per creare una macchina virtuale e configurare la memorizzazione nella cache nei relativi dischi. Ecco alcune informazioni da tenere presenti. 

Quando si effettua il provisioning di una nuova macchina virtuale tramite il portale di Azure, non è possibile modificare la configurazione di memorizzazione nella cache predefinita per il disco del sistema operativo dalle operazioni di lettura/scrittura finché la macchina virtuale non viene distribuita.

Quando si aggiunge un disco dati a una macchina virtuale esistente, è possibile configurare l'opzione di memorizzazione nella cache prima che il disco venga distribuito alla macchina virtuale.

La modifica dell'impostazione della cache di un disco di Azure scollega e ricollega il disco di destinazione. Se si tratta del disco del sistema operativo, la macchina virtuale viene riavviata. Arrestare tutte le applicazioni e i servizi che potrebbero essere interessati da questa interruzione prima di modificare l'impostazione della cache del disco.

Verrà ora creata una macchina virtuale e si modificheranno le impostazioni della cache tramite il portale di Azure.
