In questo esercizio si creerà un'applicazione console .NET Core e un account di archiviazione di Azure.

## <a name="create-a-net-core-console-application"></a>Creare un'applicazione console .NET Core

Per creare l'app, si userà Visual Studio 2017.

1. In Visual Studio 2017 selezionare l'opzione di menu **File** > **Nuovo** > **Progetto**.
1. Nella categoria **Visual C# - .NET Core** selezionare **App console (.NET Core)**, immettere un nome per l'app e selezionare **OK**.
  ![Nuova app](..\media-draft\0-new-console-app.png)

## <a name="create-an-azure-storage-account"></a>Creare un account di archiviazione di Azure

Per connettersi a un account di archiviazione di Azure, prima di tutto è necessario creare un account di archiviazione cui connettersi.

1. In alto a sinistra nel portale di Azure selezionare "Crea una risorsa".
1. Nel riquadro di selezione visualizzato selezionare "Archiviazione".
1. Sul lato destro del riquadro selezionare "Account di archiviazione: BLOB, File, Tabelle, Code".
  ![Selezione nel portale](..\media-draft\1-portal-storage-select.png)
1. È necessario immettere i dettagli dell'account di archiviazione. Immettere un nome per l'account di archiviazione.
1. Un gruppo di risorse è un contenitore logico per le risorse. Per la selezione del gruppo di risorse, selezionare **Crea nuovo** e immettere un nome per il gruppo di risorse.
1. Lasciare tutte le altre opzioni impostate sui valori predefiniti e fare clic su **Crea**.
  ![Dettagli nel portale](..\media-draft\2-portal-storage-details.png).

Viene ora effettuato il provisioning del gruppo di risorse. Al termine, l'account di archiviazione viene creato nel gruppo di risorse.
È stata anche creata l'applicazione ed è ora possibile configurarla e connetterla all'account di archiviazione.
