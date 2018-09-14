In questo caso, si aggiungerà un criterio di rimozione per Cache Redis di Azure.

## <a name="set-an-eviction-policy"></a>Impostare un criterio di rimozione

Per impostare un criterio di rimozione in Azure, è sufficiente usare un menu di scelta rapida nel portale di Azure.

1. Aprire la Cache Redis di Azure nel portale di Azure.

1. Selezionare il **impostazioni avanzate** pannello.

1. Usare la **maxmemory-policy** dal menu a discesa e selezionare **allkeys-random**.

1. Fare clic su **Salva**. 

A questo punto, se si esauriscono memoria, Cache Redis di Azure selezionerà una chiave casuale da eliminare per liberare spazio per i nuovi dati.