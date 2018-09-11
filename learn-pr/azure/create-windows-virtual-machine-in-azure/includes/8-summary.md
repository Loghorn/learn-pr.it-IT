In questo modulo si è appreso come creare una macchina virtuale Windows usando il portale di Azure. Ci si è quindi connessi all'indirizzo IP pubblico della macchina virtuale e questa è stata gestita tramite RDP. Si è osservato come RDP in Azure offra un'esperienza simile per accedere interattivamente a un computer fisico.

Si è inoltre appreso che mentre RDP consente di interagire con il sistema operativo e con il software della macchina virtuale, il portale permette di configurare l'hardware virtuale e la connettività. È anche possibile usare PowerShell o l'interfaccia della riga di comando di Azure, se si preferisce un ambiente gestibile tramite script o riga di comando.

## <a name="clean-up-the-resources"></a>Pulire le risorse

Sono previsti addebiti per le macchine virtuali in esecuzione e per l'archiviazione in base alla quantità di spazio usato. Arrestare e deallocare sempre le macchine virtuali quando non sono in uso. Quando le risorse non sono più necessarie, è una buona idea eliminarle. Per rimuovere tutte le risorse create, è possibile eliminarle una per volta oppure eliminare l'intero gruppo di risorse.

1. Accedere al portale di Azure.

1. Nel menu a sinistra selezionare **Tutti i servizi**.

1. Selezionare **Gruppi di risorse**.

1. Trovare il gruppo di risorse creato nel primo esercizio. Fare clic sui puntini di sospensione (...) sul lato destro della visualizzazione elenco.

1. Selezionare **Elimina gruppo di risorse**.

1. Nella schermata successiva immettere il nome del gruppo di risorse per confermare l'eliminazione.

1. Fare clic su **Elimina**.
