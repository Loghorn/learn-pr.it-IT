### <a name="exercise-1-create-a-custom-vision-service-project"></a>Esercizio 1: Creare un progetto del Servizio visione artificiale personalizzato

Il primo passaggio per la creazione del modello di classificazione delle immagini con il Servizio visione artificiale personalizzato consiste nel creare un progetto. In questo esercizio si userà il portale del Servizio visione artificiale personalizzato per creare un progetto del Servizio visione artificiale personalizzato.

1. Aprire il [portale del Servizio visione artificiale personalizzato](https://www.customvision.ai/) nel browser. Fare clic su **Accedi**. 
 
    ![Accesso al portale del Servizio visione artificiale personalizzato](../images/portal-sign-in.png)

    _Accesso al portale del Servizio visione artificiale personalizzato_

1. Se viene chiesto di accedere, usare le credenziali per l'account Microsoft. Se viene chiesto se consentire all'app di accedere alle proprie informazioni, fare clic su **Sì** e, se richiesto, accettare le condizioni del servizio.

1. Fare clic su **Nuovo progetto** per creare un nuovo progetto.
  
    ![Creazione di un progetto del Servizio visione artificiale personalizzato](../images/portal-click-new-project.png)

    _Creazione di un progetto del Servizio visione artificiale personalizzato_

1. Nella finestra "Nuovo progetto" assegnare al progetto il nome "Artworks", verificare che come dominio sia selezionato **Generale** e fare clic su **Crea progetto**.

    > Un dominio ottimizza un modello per tipi specifici di immagine. Ad esempio, se l'obiettivo è classificare immagini di cibi in base al tipo di alimento che contengono o all'esoticità dei piatti, può essere utile selezionare il dominio Food (Cibo). Per scenari che non corrispondono ad alcuno dei domini disponibili o in caso di dubbi su quale dominio scegliere, selezionare il dominio Generale.

    ![Creazione di un progetto del Servizio visione artificiale personalizzato](../images/portal-create-project.png)

    _Creazione di un progetto del Servizio visione artificiale personalizzato_

Il passaggio seguente consiste nel caricare immagini nel progetto e assegnare tag alle immagini per classificarle.