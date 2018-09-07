È stata creata e distribuita un'applicazione Web completa usando la funzionalità App Web del servizio app di Azure.

Il servizio app semplifica la gestione e il controllo dell'app Web rispetto alle opzioni di hosting tradizionali. Le app Web consentono di ridurre il tempo e il lavoro dedicati all'esecuzione e alla gestione dell'app Web e offrono funzionalità cloud avanzate, ad esempio la scalabilità automatica e l'integrazione di Git.

## <a name="clean-up-resources"></a>Pulire le risorse

Quando si è terminato di usare questa applicazione, è possibile eliminare tutte le risorse create durante l'esercitazione seguendo questa procedura.

Un gruppo di risorse consente di associare tutte le altre risorse che vengono create e correlate all'app Web. Pertanto, la pulizia in Azure consente di eliminare il gruppo di risorse e, di conseguenza, tutte le risorse create in tale gruppo non vengono più visualizzate.

Qui di seguito verrà esaminato come eliminare un gruppo di risorse usando il portale di Azure:

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Individuare e selezionare la voce di menu **Gruppi di risorse** a sinistra del dashboard. Nel pannello **Gruppi di risorse** sono elencati tutti i gruppi di risorse creati dall'utente nel portale di Azure.

1. Individuare e selezionare il gruppo di risorse creato nell'Unità #2. Nel portale viene visualizzato il pannello dell'app Web.

    ![Gruppi di risorse](../media-draft/8-resource-groups.png)

1. Nel portale di Azure viene visualizzato il pannello **Gruppo di risorse**. È possibile visualizzare un elenco di tutte le risorse create durante questo modulo in questo gruppo di risorse.

    ![Pannello Gruppo di risorse](../media-draft/8-resource-group-blade.png)

1. Individuare e fare clic sul collegamento **Elimina gruppo di risorse** nella parte superiore del pannello.

1. Azure verifica che si desideri eliminare questo gruppo di risorse richiedendo di digitarne il nome. Per continuare, digitare il nome del gruppo di risorse.

    ![Conferma dell'eliminazione di un gruppo di risorse](../media-draft/8-resource-group-delete.png)

1. Individuare e fare clic sul pulsante **Elimina** nella parte inferiore della finestra di conferma.

1. Azure impiega alcuni secondi per eliminare il gruppo di risorse e tutte le risorse correlate.
