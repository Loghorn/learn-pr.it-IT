Si decide di distribuire Azure AD e usare i criteri di accesso condizionale in modo che Azure richieda Multi-Factor Authentication quando qualcuno accede al portale di Azure. È necessario creare una directory e avere a disposizione licenze temporanee.

## <a name="launch-lab-and-sign-in-to-the-azure-portal"></a>Avviare il lab e accedere al portale di Azure

1. Per avviare il lab, fare clic sul collegamento **Launch Lab** (Avvia lab) riportato sopra.

> [!NOTE]
> Dopo l'avvio del lab, il nome utente e la password necessari per eseguire l'accesso sono disponibili nella scheda **Risorse** accanto alle istruzioni.

## <a name="create-a-directory"></a>Creare una directory

Si creerà una nuova directory per First Up Consultants in cui è possibile eseguire test senza temere conseguenze per gli utenti di produzione.

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

2. Nel riquadro di navigazione a sinistra fare clic su **Crea una risorsa** > **Identità** > **Azure Active Directory**.

3. Nel pannello **Crea directory** specificare i valori seguenti per **Nome organizzazione** e **Nome di dominio iniziale**:

   1. Nome organizzazione: `First Up Consultants`.
   2. Nome di dominio iniziale: `firstupconsultants<XXXXXXX>` in cui <XXXXXXX> è il numero che segue il nome utente nella scheda **Risorse** nella parte superiore della finestra delle istruzioni.

4. Attendere che la directory venga creata. Fare clic sul collegamento per passare alla nuova directory oppure fare clic su **Filtro per directory e sottoscrizione** nella parte superiore della finestra e quindi scegliere la directory appena creata.

## <a name="get-trial-licenses"></a>Ottenere le licenze per la versione di valutazione

Per usare funzionalità come l'accesso condizionale e l'autenticazione a più fattori è necessaria almeno una licenza di valutazione. La procedura seguente illustra come abilitare una licenza di valutazione:

1. Nel riquadro **Panoramica** di Azure AD fare clic sul collegamento per **avviare una versione di valutazione gratuita**.

1. Sotto la voce **Azure AD Premium P2** fare clic su **Versione di valutazione gratuita**, quindi fare clic su **Attiva**.

## <a name="create-a-test-user"></a>Creare un utente di test

Per eseguire il test è necessario un utente. Isabella Simonsen, un altro membro del team, ha offerto il proprio aiuto. Dato che avrà bisogno di un account nella directory, verrà esaminata la procedura di creazione del suo account.

1. Passare a **Azure Active Directory** > **Utenti**.

1. Fare clic su **Nuovo utente**.

1. Creare un utente denominato **Isabella Simonsen** con questo nome utente:

   `Isabella@firstupconsultants<XXXXXXX>.onmicrosoft.com`

   Fare in modo che il dominio dopo il simbolo @ corrisponda al dominio creato nella sezione *Creare una directory* sopra riportata.

1. Selezionare la casella per **visualizzare la password** per l'utente. Prendere nota della password in modo da usarla in un secondo tempo durante il test.

1. Fare clic su **Crea**.

## <a name="create-a-pilot-group"></a>Creare un gruppo pilota

Il criterio creato verrà assegnato a un gruppo di utenti, ma è necessario creare un gruppo per questo criterio. I passaggi seguenti consentono di creare un gruppo di sicurezza per la distribuzione pilota.

1. Passare a **Azure Active Directory** > **Gruppi**.

1. Fare clic su **Nuovo gruppo**.

1. Tipo di gruppo **Sicurezza**.

1. Nome del gruppo **CA-MFA-AzurePortal**.

1. Tipo di appartenenza **Assegnato**.

1. Selezionare l'utente creato nel passaggio precedente e scegliere **Seleziona**.

1. Fare clic su **Crea**.

In questa unità si è appreso come creare una directory con licenza di valutazione, un utente di test e un gruppo pilota nel portale di Azure.