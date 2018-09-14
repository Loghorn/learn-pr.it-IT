Molti sviluppatori di prendere in considerazione i Framework e librerie che utilizzano per compilare il proprio software con a decidere principalmente applicando una funzionalità o preferenze personali. Tuttavia, il framework scelto è una decisione importante, non solo da una prospettiva di progettazione e le funzionalità ma anche da un _sicurezza_ prospettiva. Scelta di un framework con funzionalità di sicurezza moderne e mantenere aggiornati è uno dei modi migliori per garantire la sicure delle app.

## <a name="choose-your-framework-carefully"></a>Scegliere con attenzione il tuo framework

Il fattore più importante riguardo alla sicurezza quando si sceglie un framework è supportato come è. Migliori Framework secondo le istruzioni di accordi di sicurezza e sono supportate da community di grandi dimensioni che migliorare e il framework di test. Software non è al 100% privo di errori o totalmente sicura, ma quando viene identificata una vulnerabilità, vogliamo essere certi che verranno chiuse o corrispondere a una soluzione alternativa indicata rapidamente.

Spesso "ben" supportate"è sinonimo con"modern". Versioni precedenti tendono a essere sostituito o alla fine la dissolvenza in entrata popolarità. Anche se si ha esperienza significativa o o molte App scritte in un framework precedente, sarà preferibile disattivare la scelta di una libreria moderni con le funzionalità necessarie. Framework moderni tendono su cui basare gli aspetti appresi dalle iterazioni precedenti che rende loro scelta per le app nuove una forma di riduzione della superficie di minaccia. Si avrà un'unica meno app preoccuparsi se individuazione di una vulnerabilità in framework precedente le applicazioni legacy scritte in.

<!-- TODO: add link; Should we be pointing to other modules? -->
<!--
For more information on secure design and reducing threat surface, please see [Design For Security in Azure](../../design-for-security-in-azure/index.yml).
-->

## <a name="keep-your-framework-updated"></a>Mantenere il tuo framework aggiornato

Framework di sviluppo software, ad esempio Spring Java e .NET Core versione regolarmente gli aggiornamenti e nuove versioni. Questi aggiornamenti includono nuove funzionalità, la rimozione della funzionalità precedente e spesso sicurezza correzioni o miglioramenti. Quando si consente nostri Framework diventino obsolete, crea "debito tecnico". L'ulteriore obsolete otteniamo, il più difficile e più rischioso verrà per portare il nostro codice fino alla versione più recente. Inoltre, in modo analogo la scelta iniziale framework, rimanere in versioni precedenti del framework open è fino a ulteriori minacce alla sicurezza che sono stati risolti nelle versioni più recenti di framework.

Ad esempio, da 2016-2017 [vulnerabilità oltre 30](https://www.cvedetails.com/product/6117/Apache-Struts.html?vendor_id=45) sono stati trovati in framework Apache Struts. Questi sono stati risolti rapidamente dal team di sviluppo, ma alcune aziende non applicano le patch e [pagato il prezzo sotto forma di una violazione dei dati](https://www.zdnet.com/article/equifax-confirms-apache-struts-flaw-it-failed-to-patch-was-to-blame-for-data-breach/). **Assicurarsi di mantenere aggiornati i Framework e librerie**.

### <a name="how-do-i-update-my-framework"></a>Come si aggiorna il mio framework?

Alcuni Framework, come Java o .NET, richiede l'installazione e tendono a cadenza note di rilascio. È una buona idea per tenere sotto controllo le nuove versioni e prevede di apportare un ramo del codice per provare il servizio quando viene rilasciato. Ad esempio, .NET Core gestisce un [relativa alle note sulla versione](https://github.com/dotnet/core/tree/master/release-notes) che è possibile archiviare per trovare le versioni più recenti disponibili.

Più specializzato, ad esempio i framework JavaScript librerie o componenti .NET possono essere aggiornati tramite Gestione pacchetti. **NPM** e **Bower** sono le opzioni più comuni per i progetti web e sono supportati per la maggior parte degli ambienti di sviluppo integrato o gli strumenti di compilazione. In .NET, utilizziamo **NuGet** per gestire le dipendenze di componente. Molto, ad esempio aggiornare il framework core, creando rami del codice, l'aggiornamento dei componenti e il test è una tecnica valida per convalidare una nuova versione di una dipendenza.

> [!NOTE]
> Il `dotnet` strumento della riga di comando contiene un `add package` e `remove package` l'opzione per aggiungere o rimuovere NuGet pacchetti ma non ha un corrispondente `update package` comando. Tuttavia, si scopre è possibile eseguire `dotnet add package <package-name>` nel progetto e verrà automaticamente _aggiornare_ il pacchetto alla versione più recente. Questo è un modo semplice per aggiornare le dipendenze senza dover aprire l'IDE.

## <a name="take-advantage-of-built-in-security"></a>Sfruttare i vantaggi di sicurezza predefinite

Verificare sempre le funzionalità di sicurezza dell'offerta di Framework. **Mai** implementare per la sicurezza se è presente una tecnica standard o una funzionalità integrata. Inoltre, si basano su algoritmi collaudati disponibili e i flussi di lavoro perché questi hanno spesso analizzati da numerosi esperti, criticato e potenziate in modo che si possono avere la certezza che siano affidabili e sicure.

Il framework .NET Core ha molte funzionalità di sicurezza, ecco alcuni core a partire da posizioni la documentazione.
* [Autenticazione-gestione delle identità](https://docs.microsoft.com/aspnet/core/security/authentication/index?view=aspnetcore-2.1)
* [Autorizzazione](https://docs.microsoft.com/aspnet/core/security/authorization/index?view=aspnetcore-2.1)
* [Protezione dei dati](https://docs.microsoft.com/aspnet/core/security/data-protection/index?view=aspnetcore-2.1)
* [Configurazione protetta](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/index?view=aspnetcore-2.1)
* [API di estendibilità della protezione](https://docs.microsoft.com/aspnet/core/security/data-protection/extensibility/index?view=aspnetcore-2.1)

Ognuna di queste funzionalità è stata scritta da esperti nei rispettivi campi e quindi battered con i test per assicurarsi che funzioni come previsto e solo come previsto. Altri Framework offrono funzionalità simili - controllo con il fornitore che offre il framework per trovare le informazioni in ogni categoria.

> [!WARNING]
> Scrivere i propri controlli di sicurezza, anziché usare quelle fornite dai framework, è non solo sprecare tempo, ma risulta meno sicuro.


## <a name="azure-security-center"></a>Centro sicurezza di Azure

Quando si usa Azure per ospitare le applicazioni web il Centro sicurezza avviserà l'utente se sono il Framework non aggiornata come parte della scheda consigli.  Non dimenticare di si cerca di tanto in tanto per verificare se esistono eventuali avvisi relativi alle app.

![Centro sicurezza di Azure in cui si consiglia un framework di eseguire l'aggiornamento.](../media-draft/ASCFramework.png)


## <a name="summary"></a>Riepilogo

Se possibile, scegliere un moderno framework per compilare le app sempre usare le funzionalità di sicurezza incorporati e assicurarsi che mantenere aggiornato. Queste semplici regole consente di verificare che l'applicazione venga avviata su solide basi.
