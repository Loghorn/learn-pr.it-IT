È possibile configurare le impostazioni della cache del disco di macchina virtuale con uno qualsiasi degli strumenti seguenti:

- Portale di Azure
- Modelli di Resource Manager
- Interfaccia della riga di comando di Azure
- Azure PowerShell

Nel prossimo esercizio, si userà il portale per creare una macchina virtuale e configurare caching sui relativi dischi. Ecco alcune informazioni da tenere presenti.

Quando si effettua il provisioning di una nuova VM usando il portale di Azure, è possibile modificare il valore predefinito di memorizzazione nella cache di configurazione per il disco del sistema operativo da lettura/scrittura fino a quando non viene distribuita la macchina virtuale.

Quando si aggiunge un disco dati a una VM esistente, è possibile configurare l'opzione di memorizzazione nella cache prima che la distribuzione del disco alla macchina virtuale.

Modifica dell'impostazione della cache di un disco di Azure, scollega e ricollega il disco di destinazione. Se il disco del sistema operativo, la VM viene riavviata. Arrestare tutte le applicazioni/i servizi che potrebbero essere interessati da questa interruzione prima di modificare l'impostazione della cache del disco.

È possibile creare una macchina virtuale e modificare le impostazioni della cache usando il portale di Azure.
