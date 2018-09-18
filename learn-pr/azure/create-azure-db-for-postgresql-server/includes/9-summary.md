In questo modulo sono state acquisite informazioni sull'offerta Database di Azure per PostgreSQL. È stato illustrato come creare un database di Azure per PostgreSQL usando il portale di Azure, nonché l'interfaccia della riga di comando di Azure.

Sono state esaminate le opzioni di configurazione disponibili per configurare una regola del firewall a livello del server PostgreSQL e imporre le connessioni SSL. Infine, è stato mostrato come connettersi a server di database di Azure per PostgreSQL tramite _psql_ in Azure Cloud Shell

## <a name="clean-up"></a>Eseguire la pulizia
<!---TODO: Update for sandbox?--->

A questo punto, rimane solo da eseguire la pulizia delle risorse create nei lab. Tenere presente che il server genererà un addebito mensile anche per le risorse che non vengono usate. Se si creano risorse a scopo di test, è possibile rimuoverle tutte facilmente eliminando il gruppo di risorse di cui fanno parte.

> [!NOTE]
> Ricordare che un gruppo di risorse è una raccolta di risorse che condividono il ciclo di vita, le autorizzazioni e i criteri. Queste azioni elimineranno tutte le risorse allocate in questo gruppo di risorse, inclusi tutti i dati eventualmente aggiunti ai database nel server creato.

- Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

- Nel menu a sinistra selezionare **Gruppi di risorse** per visualizzare tutti i gruppi di risorse attualmente in uso per l'account.

- Fare clic sul nome del gruppo di risorse creato e quindi sul pulsante **Elimina gruppo di risorse** nella barra degli strumenti nella parte superiore.

- Verrà visualizzata una finestra di dialogo con il titolo **Eliminare `<resource_group_name>`** in cui viene richiesto di digitare il nome del gruppo di risorse per confermarne l'eliminazione. Confermare il nome del gruppo di risorse e fare clic sul pulsante Elimina nella parte inferiore.
