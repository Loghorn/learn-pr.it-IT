Molti sviluppatori decidono quali framework e librerie usare per compilare il software principalmente in base alle funzionalità e alle preferenze personali. La scelta del framework è invece una decisione importante, non solo dal punto di vista della progettazione e della funzionalità, ma anche della _sicurezza_. Scegliere un framework con funzionalità di sicurezza moderne e mantenerlo aggiornato è uno dei modi migliori per garantire la sicurezza delle proprie app.

## <a name="choose-your-framework-carefully"></a>Scegliere il framework con attenzione

Per quanto riguarda la sicurezza, il fattore più importante nella scelta del framework è il livello di supporto. I framework migliori sono dotati di dispositivi di sicurezza consolidati e sono supportati da una vasta community che li testa e li migliora continuamente. Nessun software è completamente privo di bug o totalmente sicuro, ma quando viene identificata una vulnerabilità è importante avere la certezza che verrà presto risolta o quanto meno verrà trovata una soluzione alternativa.

Spesso "ben supportato" è sinonimo di "moderno". I framework meno recenti tendono a essere sostituiti o a perdere gradualmente popolarità. Anche se si ha una notevole esperienza con un framework meno recente o numerose app scritte con tale framework, è comunque più vantaggioso scegliere una libreria moderna che contenga le funzionalità di cui si ha bisogno. I framework moderni si basano in genere sulle lezioni apprese dalle versioni precedenti, quindi sceglierli per le nuove app significa ridurre la superficie di attacco. Ci sarà un'app in meno di cui preoccuparsi in caso venisse individuata una vulnerabilità nel framework precedente con cui sono scritte le applicazioni legacy.

<!-- TODO: add link; Should we be pointing to other modules? -->
<!--
For more information on secure design and reducing threat surface, please see [Design For Security in Azure](../../design-for-security-in-azure/index.yml).
-->

## <a name="keep-your-framework-updated"></a>Mantenere aggiornato il framework

Per i framework di sviluppo del software, come Java Spring e .NET Core, vengono rilasciati regolarmente aggiornamenti e nuove versioni. Questi aggiornamenti includono nuove funzionalità, rimozioni di vecchie funzionalità e, spesso, correzioni o miglioramenti per la sicurezza. Se non si aggiorna il framework in uso, si crea quello che si chiama "debito tecnico". Più si lascia che il framework diventi obsoleto e più sarà difficile e rischioso aggiornare il codice alla versione più recente. Inoltre, come può accadere con la scelta del framework iniziale, mantenere versioni precedenti del framework può esporre a minacce per la sicurezza che sono state invece risolte nelle versioni più recenti del framework.

Ad esempio, dal 2016-2017 sono state individuate [oltre 30 vulnerabilità](https://www.cvedetails.com/product/6117/Apache-Struts.html?vendor_id=45) nel framework Apache Struts. Queste vulnerabilità sono state rapidamente neutralizzate dal team di sviluppo, ma alcune aziende non hanno applicato le patch, [pagandone il prezzo in forma di violazioni dei dati](https://www.zdnet.com/article/equifax-confirms-apache-struts-flaw-it-failed-to-patch-was-to-blame-for-data-breach/). **Assicurarsi quindi di mantenere i framework e le librerie sempre aggiornati**.

### <a name="how-do-i-update-my-framework"></a>Come si aggiorna il framework?

Alcuni framework, come Java o .NET, richiedono un'installazione e vengono in genere rilasciati con una cadenza nota. È consigliabile attendere il rilascio di nuove versioni e pianificare la creazione di un ramo del codice da testare al momento del rilascio. Ad esempio, .NET Core ha una [pagina di note sulla versione](https://github.com/dotnet/core/tree/master/release-notes) che è possibile controllare per trovare le ultime versioni disponibili.

Le librerie più specializzate, come i framework JavaScript o i componenti .NET, possono essere aggiornate tramite uno strumento di gestione pacchetti. **NPM** e **Bower** sono le opzioni più comuni per i progetti Web e sono supportate dalla maggior parte degli ambienti di sviluppo integrato (IDE) o dagli strumenti di compilazione. In .NET si usa **NuGet** per gestire le dipendenze dei componenti. Analogamente all'aggiornamento del framework, la creazione di un ramo del codice, l'aggiornamento dei componenti e il testing costituiscono una buona tecnica per convalidare una nuova versione di una dipendenza.

> [!NOTE]
> Lo strumento da riga di comando `dotnet` ha le opzioni `add package` e `remove package` per aggiungere e rimuovere pacchetti NuGet, ma non ha un comando `update package` corrispondente. È però possibile eseguire `dotnet add package <package-name>` nel pacchetto per _aggiornare_ automaticamente il pacchetto all'ultima versione. È un modo facile di aggiornare le dipendenze senza dover aprire l'IDE.

## <a name="take-advantage-of-built-in-security"></a>Sfruttare le funzionalità di sicurezza predefinite

È sempre opportuno verificare cos'hanno da offrire le funzionalità di sicurezza dei framework. Non implementare **mai** le proprie funzionalità di sicurezza se esiste già una tecnica o una funzionalità standard integrata. Usare inoltre algoritmi e flussi di lavoro consolidati, in quanto spesso sono già stati esaminati da numerosi esperti, criticati e quindi rafforzati, pertanto sono certamente affidabili e sicuri.

Il framework .NET Core ha innumerevoli funzionalità di sicurezza, come le seguenti.
* [Autenticazione e gestione delle identità](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/index?view=aspnetcore-2.1)
* [Autorizzazione](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/index?view=aspnetcore-2.1)
* [Protezione dei dati](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/index?view=aspnetcore-2.1)
* [Configurazione sicura](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/configuration/index?view=aspnetcore-2.1)
* [API di estendibilità della sicurezza](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/extensibility/index?view=aspnetcore-2.1)

Ognuna di queste funzionalità è stata scritta da esperti nello specifico campo e quindi sottoposta a test ripetuti per verificare che funzioni nel modo previsto e in nessun altro modo. Altri framework offrono funzionalità simili. Rivolgersi al fornitore del framework per conoscere le funzionalità offerte in ogni categoria.

> [!WARNING]
> Scrivere personalmente il codice di sicurezza invece di usare quelli forniti dal framework non solo è una perdita di tempo, ma è anche meno sicuro.


## <a name="azure-security-center"></a>Centro sicurezza di Azure

Quando si usa Azure per ospitare le proprie applicazioni Web, il Centro sicurezza invia un avviso se i framework in uso non sono aggiornati usando la scheda Raccomandazioni.  Consultare periodicamente questa scheda per verificare la presenza di avvisi relativi alle app.

![Avviso che raccomanda un aggiornamento del framework nel Centro sicurezza di Azure.](../media-draft/ASCFramework.png)


## <a name="summary"></a>Riepilogo

Quando possibile, scegliere un framework moderno per la compilazione delle app, usare sempre le funzionalità di sicurezza integrate e assicurarsi di mantenere il framework aggiornato. Queste semplici regole aiuteranno a creare solide basi per la compilazione delle applicazioni.
