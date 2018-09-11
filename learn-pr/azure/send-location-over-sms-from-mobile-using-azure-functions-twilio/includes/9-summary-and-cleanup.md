È stata creata un'app per dispositivi mobili multipiattaforma usando Xamarin ed è stata creata una funzione di Azure con un'associazione a Twilio.

## <a name="clean-up-resources"></a>Pulire le risorse

Dopo avere usato questa applicazione di Funzioni di Azure, è possibile eliminare nel portale di Azure tutte le risorse create durante l'esercitazione.

1. In Visual Studio selezionare *Visualizza->Cloud Explorer*.

2. Nell'elenco a discesa nella parte superiore del pannello selezionare *Gruppi di risorse*.

3. Espandere la sottoscrizione usata per creare il gruppo di risorse. Fare clic con il pulsante destro del mouse sul gruppo di risorse "ImHere" e selezionare *Apri nel portale*.

    ![Aprire il gruppo di risorse nel portale dalla finestra di Cloud Explorer](../media-drafts/9-open-resource-group-in-portal.png)

4. Accedere al portale di Azure nel browser, se necessario.

5. Nel portale verrà aperto il gruppo di risorse "ImHere". Fare clic sul pulsante **Elimina gruppo di risorse**.

    ![Eliminare il gruppo di risorse](../media-drafts/9-delete-resource-group.png)

6. Immettere il nome del gruppo di risorse di cui confermare l'eliminazione e fare clic su **Elimina**.

    ![Immettere il nome del gruppo di risorse di cui confermare l'eliminazione](../media-drafts/9-confirm-delete-resource-group.png)

## <a name="summary"></a>Riepilogo

Questa esercitazione ha descritto come:
> [!div class="checklist"]
> * Creare un'app Xamarin.Forms multipiattaforma che usa Xamarin.Essentials.
> * Creare un'interfaccia utente multipiattaforma usando XAML con la logica dell'applicazione in un elemento ViewModel, nonché associare le proprietà nel ViewModel all'interfaccia utente.
> * Rilevare la posizione dell'utente.
> * Creare una funzione di Azure con un trigger HTTP ed eseguirla in locale.
> * Chiamare una funzione di Azure da un'app per dispositivi mobili, passando i dati come JSON.
> * Associare una funzione di Azure a Twilio per inviare un messaggio SMS.
> * Pubblicare una funzione di Azure in Azure.
