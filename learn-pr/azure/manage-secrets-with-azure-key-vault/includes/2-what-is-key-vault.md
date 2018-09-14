Azure Key Vault è un *archivio segreti*, ossia un servizio cloud centralizzato per l'archiviazione dei segreti delle applicazioni. Key Vault consente di controllare i segreti delle applicazioni tenerli in un'unica posizione centrale e fornendo accesso protetto, controllo delle autorizzazioni e la registrazione di accesso.

I vantaggi principali dell'uso di Key Vault sono:

- Rischio ridotto di divulgazione accidentale di segreti mediante l'archiviazione sicura, fuori dalla configurazione e dal controllo del codice sorgente, e l'eliminazione di scenari in cui i segreti vengono copiati in file o incollati in messaggi di posta elettronica o chat
- Accesso ai segreti limitato con criteri di accesso su misura per applicazioni e utenti che hanno necessità di usarle
- Archiviazione dei segreti centralizzata, in modo che più utenti e istanze di applicazioni possano accedere ai valori dei segreti che possono essere aggiornati in un unico posto
- Accesso a registrazione e monitoraggio per comprendere meglio come e quando i segreti sono accessibili

I segreti vengono archiviati in *insiemi di credenziali*, che sono risorse di Azure con configurazione e criteri di sicurezza propri che possono essere creati con qualsiasi strumento di gestione di Azure come il portale di Azure o l'interfaccia della riga di comando di Azure. L'accesso ai segreti e la gestione degli insiemi di credenziali avvengono tramite l'API REST, supportata anche da tutti gli strumenti di gestione di Azure nonché da librerie client disponibili per i linguaggi più diffusi. Ogni insieme di credenziali ha un URL univoco in cui è ospitata l'API relativa.

> [!IMPORTANT]
> **Key Vault è progettato per archiviare i segreti della configurazione delle applicazioni server.** Non è destinato ad archiviare i dati appartenenti agli utenti dell'app e non dovrebbe essere usato sul lato client di un'app. Ciò si riflette nelle caratteristiche di prestazioni, nell'API e nel modello di costi.
>
> I dati utente dovranno essere archiviati altrove, ad esempio in un database SQL di Azure con Transparent Data Encryption o in un account di archiviazione con la crittografia del servizio di archiviazione. I segreti usati dall'applicazione per accedere a tali archivi dati possono essere conservati in Key Vault.

## <a name="what-is-a-secret-in-key-vault"></a>Informazioni sui segreti in Key Vault

In Key Vault, un segreto è una coppia di stringhe nome-valore. I nomi dei segreti devono avere una lunghezza compresa tra 1 e 127 caratteri, contenere solo caratteri alfanumerici e trattini ed essere univoci all'interno di un insieme di credenziali. Il valore di un segreto può essere qualsiasi stringa UTF-8 di dimensioni fino a 25 KB.

> [!TIP]
> I nomi dei segreti non devono necessariamente essere considerati a propria volta segreti. Possono essere archiviati nella configurazione dell'app, se necessario per l'implementazione. Lo stesso vale per gli URL e i nomi degli insiemi di credenziali.

> [!NOTE]
> Oltre alle stringhe, Key Vault supporta altri due tipi di segreti, ossia *chiavi* e *certificati*, e offre funzionalità utili specifiche per i relativi casi d'uso. Questo modulo non illustra tali funzionalità e si concentra sui segreti stringa, come le password e le stringhe di connessione.
