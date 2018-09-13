In questa unità si integrerà la libreria client di Archiviazione di Azure nell'applicazione console .NET Core.

## <a name="add-the-azure-storage-nuget-package"></a>Aggiungere il pacchetto NuGet per Archiviazione di Azure

1. Fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**
  ![Gestisci pacchetti NuGet]/' (..\media-draft\5-manage-nuget-packages.png)

1. Viene visualizzata una pagina che mostra i pacchetti NuGet attualmente installati. Fare clic sull'opzione **Sfoglia** e digitare **Storage** nel campo di ricerca. Nei risultati della ricerca viene visualizzato il pacchetto **WindowsAzure.Storage**.

1. Selezionare il pacchetto **WindowsAzure.Storage**. Viene visualizzato un riquadro a destra che per impostazione predefinita indica la versione più recente. Al momento della redazione di questo documento, la versione più recente è la 9.3.0. Fare clic sul pulsante **Installa** per aggiungere un riferimento al pacchetto al progetto.
  ![Ricerca del pacchetto](..\media-draft\5-find-package.png)

1. Visual Studio visualizza il riquadro **Output** mentre vengono ripristinati i pacchetti. Saranno necessari alcuni secondi, a seconda della velocità di rete. Viene visualizzata la finestra di dialogo **Anteprima modifiche** che mostra i pacchetti che stanno per essere aggiunti al progetto. Fare clic su **OK**.
  ![Anteprima modifiche](..\media-draft\5-preview-changes.png)

1. Viene visualizzata un'altra finestra di dialogo che chiede se accettare le condizioni di licenza. Fare clic su **Accetto**.
  ![Licenza](..\media-draft\5-licence.png)

Dopo pochi secondi, vengono mostrate altre attività nel riquadro dell'output in Visual Studio e il pacchetto viene aggiunto al progetto.
È possibile verificare che il pacchetto sia stato aggiunto correttamente espandendo l'opzione **Dipendenze** nel progetto, quindi l'opzione **NuGet**. Nell'elenco sarà visualizzato un riferimento a **WindowsAzure.Storage**.

![Controllo del pacchetto](..\media-draft\5-package-check.png)
