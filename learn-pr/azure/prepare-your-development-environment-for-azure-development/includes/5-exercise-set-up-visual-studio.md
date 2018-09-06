In questo esercizio verrà installato Visual Studio nel computer Windows o nel macOS in uso. In Windows sarà necessario installare il carico di lavoro Sviluppo di Azure. In Visual Studio per Mac, il flusso di lavoro di Servizi connessi predefinito consentirà di creare app per Servizio app di Azure. Al termine, sarà tutto pronto per iniziare a creare le applicazioni e pubblicarle in Azure.

## <a name="exercise-steps"></a>Passaggi dell'esercizio

Esistono piccole differenze per l'installazione di Visual Studio tra Windows e macOS. Le sezioni seguenti descrivono tali differenze.

### <a name="windows"></a>Windows

1. Scaricare il programma di installazione di Visual Studio da https://visualstudio.microsoft.com/downloads/.
2. Eseguire il programma di installazione. Si aprirà la finestra Carichi di lavoro.
3. Scegliere il carico di lavoro **Sviluppo di Azure**.

    Lo screenshot seguente mostra il carico di lavoro per il programma di installazione di Visual Studio selezionato per consentire lo sviluppo di Azure all'interno di Visual Studio.

    ![Screenshot del programma di installazione di Visual Studio con il carico di lavoro di sviluppo di Azure evidenziato.](../media/5-select-azure-workload.png)

4. (Facoltativo) Installare il carico di lavoro Sviluppo ASP.NET e Web per prepararsi per la creazione di applicazioni Web per Azure.
5. Fare clic su **Installa** e attendere il completamento dell'installazione di Visual Studio.
6. Al termine dell'installazione aprire Visual Studio.
7. Passare al menu Visualizza in Visual Studio e assicurarsi di avere l'opzione **Cloud Explorer**.

    Lo screenshot seguente mostra l'opzione di menu Cloud Explorer che sarà presente se è installato il carico di lavoro Sviluppo di Azure.

    ![Screenshot del menu Visualizza di Visual Studio con l'opzione di menu Cloud Explorer evidenziata.](../media/5-verify-cloud-explorer.png)

### <a name="macos"></a>macOS

1. Passare a https://visualstudio.microsoft.com/ e scaricare il programma di installazione di Visual Studio per Mac.
2. Fare clic sul file VisualStudioInstaller.dmg per montare il programma di installazione e quindi eseguirlo facendo doppio clic sul logo.
3. Accettare le condizioni di licenza e privacy quando vengono visualizzate.
4. Il programma di installazione richiederà i componenti da installare. I componenti di Azure fanno già parte di Visual Studio per Mac, ma è consigliabile installare la piattaforma **.NET Core** per sviluppare esperienze Web per Azure.

    Lo screenshot seguente mostra la piattaforma .NET Core necessaria per aggiungere funzionalità di sviluppo di Azure a Visual Studio per Mac.

    ![Screenshot del programma di installazione di Visual Studio per Mac con l'opzione per la piattaforma .NET Core evidenziata.](../media/5-vsmac-install-net-core.png)

5. Fare clic su **Installa e aggiorna** quando si è soddisfatti delle selezioni e attendere il completamento del programma di installazione.
6. Se viene chiesto di elevare le autorizzazioni necessarie, usare le credenziali di amministratore per eseguire questa operazione.
7. Una volta completato il programma di installazione, avviare Visual Studio per Mac.
