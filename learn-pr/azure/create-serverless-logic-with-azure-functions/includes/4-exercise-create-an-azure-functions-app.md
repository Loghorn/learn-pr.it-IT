In questo esercizio verrà creata un'app per le funzioni di Azure.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true) con l'account di Azure.
1. Selezionare il pulsante **Crea una risorsa** visualizzato nell'angolo superiore sinistro del portale di Azure, quindi selezionare **Calcolo > App per le funzioni** per aprire il pannello *Crea* dell'app per le funzioni.
  ![Screenshot del portale di Azure in cui è evidenziata la selezione *Crea una risorsa* seguita da * Calcolo* e * App per le funzioni*](../images/4-create-function-app-blade.png)
1. Scegliere un nome app univoco globale. Questo nome sarà l'URL di base del servizio. Ad esempio, è possibile assegnare all'app il nome **escalator-functions-xxxxxxx**, sostituendo la x con le iniziali e l'anno di nascita dell'utente. Se il nome non è univoco globale, è possibile provare un'altra combinazione. I caratteri validi sono a-z, 0-9 e -.
1. Selezionare la sottoscrizione di Azure in cui ospitare l'app per le funzioni.
1. Creare un nuovo gruppo di risorse denominato **escalator-functions-group**. L'impiego di un gruppo per contenere tutte le risorse usate in questo modulo semplificherà le successive operazioni di pulizia.
1. Selezionare **Windows** come sistema operativo.
1. Per il **Piano di hosting**, selezionare **Piano a consumo** in modo da poter usufruire dei vantaggi delle funzionalità di Azure senza server.
1. Selezionare la località geografica più vicina all'utente (o ai clienti).
1. Creare un nuovo account di archiviazione e denominarlo **escalator functions**.
1. Verificare che Azure Application Insights sia **abilitato** e selezionare l'area più vicina all'utente (o ai clienti).
Al termine, la configurazione sarà simile alla configurazione rappresentata nello screenshot seguente.

  ![Screenshot della schermata di configurazione *Crea* dell'app per le funzioni, con tutti i campi configurati in base alle istruzioni precedenti.](../images/4-create-function-app-settings.png)

1. Selezionare **Crea**. Il processo di distribuzione richiederà alcuni minuti. L'utente riceverà una notifica al completamento dell'operazione.

## <a name="verify-your-azure-function-app"></a>Verificare l'app per le funzioni di Azure

1. Nel menu a sinistra del portale di Azure selezionare **Gruppi di risorse**. **escalator-functions-group** sarà visualizzato nell'elenco dei gruppi disponibili.
  ![Screenshot della schermata Gruppi di risorse nel portale di Azure con il gruppo di risorse **escalator-functions-group** visualizzato.](../images/4-resource-group.png)
1. Selezionare **escalator-functions-group**. Sarà visualizzato un elenco di risorse simile al seguente.
  ![Schermata di tutte le risorse contenute nel gruppo **escalator-functions-group**, comprese le voci relative a piano di servizio app, account di archiviazione, Application Insights e servizio app](../images/4-resource-list.png)

L'elemento con l'icona a forma di fulmine, elencato come servizio app, costituisce la nuova app per le funzioni. 
