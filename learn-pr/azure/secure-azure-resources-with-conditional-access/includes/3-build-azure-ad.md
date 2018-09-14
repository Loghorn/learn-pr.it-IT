Si decide di distribuire Azure AD e usare i criteri di accesso condizionale in modo che Azure richieda Multi-Factor Authentication quando qualcuno accede al portale di Azure. È necessario creare una directory e avere a disposizione licenze temporanee.

## <a name="create-a-directory"></a>Creare una directory
Viene creata una nuova directory per First Up Consultants in cui è possibile eseguire test senza temere conseguenze per gli utenti di produzione.

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Nel riquadro di spostamento a sinistra fare clic su **Crea una risorsa** > **Identità** > **Azure Active Directory**.

1. Nel pannello **Crea directory** specificare i valori seguenti per **Nome organizzazione** e **Nome di dominio iniziale**:

   1. NOME ORGANIZZAZIONE: `First Up Consultants`.
   1. NOME DI DOMINIO INIZIALE: `firstupconsultants<XXXX>.onmicrosoft.com`.

1. Attendere che la directory venga creata. Fare clic sul collegamento per passare alla nuova directory oppure fare clic sui **Directory e sottoscrizione filtro** nella parte superiore della finestra e quindi scegliere la directory appena creata.

## <a name="get-trial-licenses"></a>Ottenere le licenze di valutazione

Per usare funzionalità come l'accesso condizionale e Multi-Factor Authentication, è necessaria almeno una licenza di valutazione. La procedura seguente illustra come abilitare una licenza di valutazione:

1. Nel riquadro **Panoramica** di Azure AD fare clic sul collegamento per **avviare una versione di valutazione gratuita**.

1. Sotto la voce **Azure AD Premium P2** fare clic su **Versione di valutazione gratuita**, quindi fare clic su **Attiva**.

## <a name="create-a-test-user"></a>Creare un utente di test

Per eseguire il test è necessario un utente. Isabella Simonsen, un altro membro del team, ha offerto il proprio aiuto. Avrà bisogno di un account nella directory, quindi verrà esaminata la procedura di creazione del suo account.

1. Passare a **Azure Active Directory** > **Utenti** > **Tutti gli utenti**.

1. Fare clic su **Nuovo utente**.

1. Creare un utente denominato **Isabella Simonsen** con questo nome utente:

   `Isabella@firstupconsultants<XXXX>.onmicrosoft.com`

   Corrisponde al dominio dopo il @ con il dominio è stato creato nel *creare una directory* sezione precedente.

1. Selezionare la casella per **visualizzare la password** per l'utente. Prendere nota della password in modo da usarla in un secondo tempo durante il test.

1. Fare clic su **Crea**.

## <a name="create-a-pilot-group"></a>Creare un gruppo pilota

Il criterio creato verrà assegnato a un gruppo di utenti, ma è necessario creare un gruppo per questo criterio. I passaggi seguenti consentono di creare un gruppo di sicurezza per la distribuzione pilota.

1. Passare a **Azure Active Directory** > **Gruppi** > **Tutti i gruppi**.

1. Fare clic su **Nuovo gruppo**.

1. Tipo di gruppo **Sicurezza**.

1. Nome del gruppo **CA-MFA-AzurePortal**.

1. Tipo di appartenenza **Assegnato**.

1. Selezionare l'utente creato nel passaggio precedente e scegliere **seleziona**.

1. Fare clic su **Crea**.

## <a name="summary"></a>Riepilogo

In questa unità si è appreso come creare una directory con licenza di valutazione, un utente di test e un gruppo pilota nel portale di Azure.
