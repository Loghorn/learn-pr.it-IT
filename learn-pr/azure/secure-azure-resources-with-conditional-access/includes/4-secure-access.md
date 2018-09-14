Nell'esercizio precedente abbiamo abilitato licenze di valutazione, creato una directory, creato un utente e creato un gruppo per testare la soluzione. In questa unità, si creerà la regola di accesso condizionale per richiedere Azure multi-Factor Authentication per il portale di Azure.

## <a name="enable-conditional-access-based-multi-factor-authentication"></a>Abilitare l'autenticazione a più fattori basata sull'accesso condizionale

L'accesso condizionale consente agli amministratori di configurare quando deve verificarsi o meno un evento. È possibile usare più regole in parallelo per concedere o negare l'accesso alle risorse. Ecco la regola che è necessario creare:

**Quando si accede al portale di Azure: Richiedi autenticazione a più fattori**

I passaggi seguenti spiegano in maniera dettagliata come creare una regola di accesso condizionale per richiedere agli utenti di eseguire l'autenticazione a più fattori quando accedono al portale di Azure.

1. Passare ad **Azure Active Directory** > **Accesso condizionale**.

1. Fare clic su **Nuovo criterio**.

1. Denominare il criterio **Richiedi autenticazione a più fattori per il portale di Azure**.

1. In **Assegnazioni** > **Utenti e gruppi** selezionare **Utenti e gruppi**. Selezionare il gruppo appena creato **CA-MFA-AzurePortal**. Fare clic su **Fatto**.

1. In **App cloud** > **Selezionare le app**, selezionare **Microsoft Azure Management**.

1. In **Controlli di accesso** > **Concedi** selezionare **Richiedi autenticazione a più fattori**.

1. Impostare **Abilita criterio** su **Sì**.

1. Fare clic su **Crea**.

## <a name="summary"></a>Riepilogo

In questa unità, è stato descritto come creare una regola di accesso condizionale. La regola richiede l'autenticazione a più fattori per l'accesso al portale di Azure.
