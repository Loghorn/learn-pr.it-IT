In questo modulo sono state esaminate le decisioni che è necessario prendere prima di creare una macchina virtuale. Queste decisioni includono aspetti come le dimensioni della VM, i tipi di dischi usati, l'immagine del sistema operativo selezionata e i tipi di risorse create.

Sono state esaminate anche le opzioni per creare e gestire le macchine virtuali in Azure. È stato illustrato quanto sia facile creare e gestire le VM usando il portale e quando usare i modelli di Resource Manager, PowerShell, l'interfaccia della riga di comando di Azure e Azure Client SDK.

Sono stati infine esaminati i servizi e le estensioni disponibili per amministrare più facilmente le VM.

## <a name="clean-up-your-resources"></a>Pulire le risorse

Tenere presente che le macchine virtuali continuano a comportare un addebito mensile finché si ha hardware riservato (anche se il sistema operativo viene arrestato) e si usa spazio per i dischi. È possibile arrestare la VM nel portale per interrompere la fatturazione per i servizi di calcolo, ma lo spazio di archiviazione continuerà a essere fatturato. Se si creano risorse a scopo di test, è possibile rimuoverle tutte facilmente eliminando il **gruppo di risorse** di cui fanno parte.

> [!TIP]
> Assicurarsi di inserire tutte le risorse di test nello stesso gruppo di risorse per poterle gestire facilmente.

Per eliminare un gruppo di risorse, individuarlo nel pannello **Gruppi di risorse** facendo clic su **Gruppi di risorse** nella barra laterale. Fare quindi clic sul menu con i puntini di sospensione `...` accanto al gruppo di risorse e selezionare **Elimina gruppo di risorse** dal menu popup, come mostrato di seguito.

![Eliminare un gruppo di risorse](../media-draft/7-delete-rgs.png)
