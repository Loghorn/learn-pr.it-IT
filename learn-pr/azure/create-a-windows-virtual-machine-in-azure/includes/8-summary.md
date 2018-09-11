In questo modulo si è appreso come creare una macchina virtuale Windows usando il portale di Azure. Ci si è quindi connessi all'indirizzo IP pubblico di tale macchina virtuale e si è gestita la macchina tramite RDP. Si è visto come RDP in Azure offre un'esperienza simile all'accesso interattivo a un computer fisico.

Si è inoltre appreso che, mentre RDP consente di interagire con il sistema operativo e con il software della macchina virtuale, il portale permette di configurare l'hardware virtuale e la connettività. Si sarebbe anche potuto usare PowerShell o l'interfaccia della riga di comando di Azure, nel caso in cui si preferisca un ambiente gestibile tramite script o riga di comando.

## <a name="clean-up-the-resources"></a>Pulire le risorse

Si incorre in un addebito per le macchine virtuali in esecuzione e per l'archiviazione in base alla quantità di spazio utilizzato. Arrestare e deallocare sempre le macchine virtuali quando non sono in uso. Quando le risorse non sono più necessarie, è una buona idea eliminarle. Per rimuovere tutte le risorse create, è possibile eliminarle una alla volta o eliminare l'intero gruppo.

1. Accedere al portale di Azure.

1. Nel menu a sinistra selezionare **Tutti i servizi**.

1. Selezionare **Gruppi di risorse**.

1. Trovare il gruppo di risorse creato nel primo esercizio. Fare clic sui puntini di sospensione (...) sul lato destro della visualizzazione elenco.

1. Selezionare **Elimina gruppo di risorse**.

1. Nella schermata successiva immettere il nome del gruppo di risorse per confermare l'eliminazione.

1. Fare clic su **Elimina**.
