In questo modulo è stata attivata la protezione della configurazione segreta di un'applicazione in Azure Key Vault. Il codice dell'app esegue l'autenticazione nell'insieme di credenziali con un'identità gestita e carica automaticamente all'avvio i segreti dall'insieme di credenziali nel sistema di configurazione di ASP.NET Core.

## <a name="clean-up"></a>Eseguire la pulizia
<!---TODO: Update for sandbox?--->

Per eseguire la pulizia della sottoscrizione di Azure, eseguire in Azure Cloud Shell il comando seguente che consente di eliminare il gruppo di risorse contenente tutte le risorse create in questo modulo.

```console
az group delete --name keyvault-exercise-group
```

Per eseguire la pulizia dell'archiviazione Cloud Shell, eliminare la directory `KeyVaultDemoApp`.

## <a name="next-steps"></a>Passaggi successivi

Se si trattasse di una vera app, che cosa succederebbe dopo?

- Verrebbero inseriti tutti i segreti delle app negli insiemi di credenziali! Non vi è più alcun motivo per disporne nei file di configurazione.
- L'applicazione continuerebbe a essere usata. L'ambiente di produzione è completamente configurato, quindi per le future implementazioni non è necessario ripetere tutte le impostazioni.
- Per supportare lo sviluppo, creare un ambiente di sviluppo che contenga segreti con lo stesso nome, ma con valori diversi. Concedere le autorizzazioni per il team di sviluppo e configurare il nome dell'insieme di credenziali nel file di configurazione dell'ambiente di sviluppo dell'app. Quando gli sviluppatori eseguiranno l'app in locale, `AddAzureKeyVault` rileverà automaticamente le installazioni locali di Visual Studio e l'interfaccia della riga di comando di Azure e userà le credenziali di Azure configurate in tali applicazioni per l'iscrizione e l'insieme di credenziali di accesso.
- Creare altri ambienti per scopi quali, ad esempio, test di autorizzazione utente.
- Insiemi di credenziali separati tra sottoscrizioni diverse e/o gruppi di risorse per isolarli.
- Concedere l'accesso ad altri insiemi di credenziali dell'ambiente alle persone appropriate.

## <a name="further-reading"></a>Altre informazioni

- [Documentazione su Key Vault](https://docs.microsoft.com/azure/key-vault/)
- [Informazioni su AddAzureKeyVault e le relative opzioni avanzate](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x)
- [Questa esercitazione](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application) illustra l'uso di `KeyVaultClient`, incluse le informazioni sull'autenticazione manuale ad Azure Active Directory tramite un segreto client anziché un'identità gestita.
- [Identità gestite per la documentazione del servizio token delle risorse di Azure](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol), per implementare personalmente il flusso di lavoro dell'autenticazione.