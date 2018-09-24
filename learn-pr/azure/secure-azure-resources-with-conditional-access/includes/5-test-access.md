Negli esercizi precedenti sono stati creati una directory, un utente e un gruppo e quindi è stata creata una regola di accesso condizionale che richiede Azure Multi-Factor Authentication quando si accede al portale di Azure. Verrà quindi testato se è possibile accedere alle risorse.

## <a name="test-access-to-resources"></a>Testare l'accesso alle risorse

Gli utenti eseguiranno l'accesso a tutte le applicazioni SaaS tramite il portale delle app personali, quindi è questo che verrà testato.

1. Aprire una finestra del browser InPrivate.

1. Passare a https://myapps.microsoft.com.

1. Accedere usando le credenziali dell'utente creato nell'unità 3.

   * Si noti che si esegue l'accesso al portale senza Multi-Factor Authentication.

1. Nella stessa finestra del browser passare a https://portal.azure.com.

   * Si noti che verrà richiesto di fornire altre informazioni per mantenere sicuro l'account. Questa interruzione è dovuta all'avvio di Azure Multi-Factor Authentication a causa dei criteri di accesso condizionale creati. A questo punto è possibile interrompere e chiudere la finestra del browser.