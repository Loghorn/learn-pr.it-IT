Se si desidera comprendere cosa può andare storto con la gestione dei segreti di configurazione di un'applicazione, cercare la storia di Steve lo sviluppatore senior.

Steve aveva lavorato per alcune settimane presso un'azienda di distribuzione di alimenti per animali domestici. Stava esplorando i dettagli dell'app Web dell'azienda &mdash;, un'app Web .NET Core che usava un database Azure SQL per archiviare le informazioni sugli ordini e API di terze parti per la fatturazione con carta di credito e la mappatura degli indirizzi dei clienti &mdash; quando ha incollato accidentalmente la stringa di connessione per il database degli ordini in un forum pubblico.

Giorni dopo, la contabilità ha notato che l'azienda stava consegnando un sacco di cibo per animali domestici che nessuno aveva pagato. Qualcuno aveva usato la stringa di connessione per accedere al database, aveva decodificato lo schema ed effettuato gli ordini senza passare attraverso il sito Web.

Dopo essersi reso conto del suo errore, Steve cambiò frettolosamente la password del database per bloccare l'aggressore, violando il sito Web. Effettuò l'accesso direttamente al server delle applicazioni e cambiò la configurazione delle app invece di distribuirle nuovamente, ma il server continuava a mostrare richieste non riuscite.

Steve aveva dimenticato che più istanze dell'app funzionavano su server diversi, ma ne aveva cambiato la configurazione solo per uno. Era necessaria una ridistribuzione completa, che ha causato altri 30 minuti di inattività.

La perdita di una stringa di connessione al database, di una chiave API o di una password di servizio può essere catastrofica. Il furto o la cancellazione di dati, danni finanziari, tempi di inattività delle applicazioni e danni irreparabili ai beni aziendali e alla reputazione sono tutti potenziali risultati. Sfortunatamente, i valori segreti spesso devono essere implementati simultaneamente in più luoghi e modificati in momenti non opportuni. Ed è necessario archiviarli *in una posizione*! Vediamo come possiamo rendere tutto ciò sicuro con Azure KeyVault.

## <a name="learning-objectives"></a>Obiettivi di apprendimento
> [!div class="checklist"]
> * Comprendere i tipi di informazioni che possono essere archiviati in Azure KeyVault
> * Creare un archivio di Azure Key Vault
> * Eseguire l'autenticazione con Azure KeyVault
> * Segreti di accesso in un Azure KeyVault