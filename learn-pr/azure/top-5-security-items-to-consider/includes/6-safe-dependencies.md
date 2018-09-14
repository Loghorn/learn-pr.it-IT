Gran parte di codice presente nelle moderne applicazioni sono le librerie e dipendenze scelte dall'utente: lo sviluppatore. Si tratta di una pratica comune che consente di risparmiare tempo e denaro. Tuttavia, lo svantaggio è che è responsabili di questo codice, ora anche se altri utenti lo abbia scritto, in quanto è stato usato nel progetto. Se un ricercatore (o peggio ancora, un pirata informatico) consente di individuare una vulnerabilità in una di queste librerie di terze parti 3rd, quindi lo stesso problema probabilmente anche sarà presente nell'app.

Utilizzo dei componenti con vulnerabilità note è un enorme problema del settore. Sia così problematico che ha apportato il [elenco dei primi 10 OWASP](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) delle peggiori vulnerabilità dell'applicazione web, mantenuto #9 per diversi anni.

## <a name="track-known-security-vulnerabilities"></a>Tenere traccia delle vulnerabilità di sicurezza note

Il problema che sono disponibili è sapere quando viene rilevato un problema. Mantenimento dei nostri librerie e dipendenze naturalmente aiuterà a aggiornati (4 nel nostro elenco!), ma è una buona idea per tenere traccia di identificate le vulnerabilità che potrebbero influire sull'applicazione.

> [!IMPORTANT]
> Quando un sistema dispone di una vulnerabilità nota, è molto più probabile che anche codice exploit disponibili, gli utenti possono utilizzare per attaccare i sistemi. Se un exploit resi pubblico, è essenziale che qualsiasi sistemi interessati vengono aggiornati immediatamente.

**Mitre** è un'organizzazione no profit che gestisce il [elenco Common Vulnerabilities and Exposures](https://cve.mitre.org). Questo elenco è un set pubblicamente disponibili per la ricerca delle vulnerabilità note per la sicurezza informatica nell'App, librerie e Framework. **Se una libreria o componente si trova nel database di CVE, è causa di vulnerabilità note**.

I problemi vengono inviati dalla community di sicurezza quando viene rilevato un problema di sicurezza in un componente o prodotto. Ogni problema pubblicato viene assegnato un ID e contiene la data individuata, una descrizione della vulnerabilità e i riferimenti a soluzioni alternative pubblicate o fornitore istruzioni sul problema.

### <a name="how-to-verify-if-you-have-known-vulnerabilities-in-your-3rd-party-components"></a>Come verificare se si presentano vulnerabilità note in componenti 3rd party

È possibile inserire attività giornaliera nel tuo telefono per accedere e verificare questo elenco, ma per fortuna, sono disponibili molti strumenti per consentono di verificare se le dipendenze sono vulnerabili. È possibile eseguire questi strumenti per la codebase, o meglio ancora, aggiungerli alla pipeline di integrazione continua/recapito Continuo per cercare automaticamente i problemi come parte del processo di sviluppo.

- [Il controllo delle dipendenze OWASP](https://www.owasp.org/index.php/OWASP_Dependency_Check), che ha un [plug-in Jenkins](https://wiki.jenkins.io/display/JENKINS/OWASP+Dependency-Check+Plugin)
- [OWASP SonarQube](https://www.owasp.org/index.php/OWASP_SonarQube_Project)
- [Portatili](https://snyk.io), che è gratuita per i repository open source in GitHub
- [Black Duck](https://www.blackducksoftware.com) usata da molte aziende
- [RubySec](https://rubysec.com) un database di avviso solo per Ruby
- [Retire.js](https://github.com/retirejs/retire.js/) uno strumento per la verifica se le librerie JavaScript non sono aggiornate; può costituire un plug-in per diversi strumenti, tra cui [Burp Suite](https://www.portswigger.net)

Alcuni strumenti creati appositamente per analisi statica del codice possono essere utilizzati anche per questo.

- [Guardia Roslyn](https://dotnet-security-guard.github.io)
- [Puma analisi](https://pumascan.com)
- [PT Application Inspector](https://www.ptsecurity.com/ww-en/products/ai/)
- [Apache Maven Dependency Plugin](http://maven.apache.org/plugins/maven-dependency-plugin/)
- [Cancella origine](https://www.sourceclear.com)
- [Sonatype](https://ossindex.sonatype.org)
- [Piattaforma di sicurezza di nodo](https://nodesecurity.io)
- [WhiteSource](https://www.whitesourcesoftware.com/what-is-whitesource/)
- [Hdiv](https://hdivsecurity.com)
- [E molto altro ancora...](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)

Per altre informazioni sui rischi correlati all'utilizzo di componenti vulnerabili visitare il [pagina OWASP](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities) dedicato a questo argomento.

## <a name="summary"></a>Riepilogo

Quando si usa le librerie o altri componenti di terze parti 3rd come parte dell'applicazione che anche utente si assume i rischi possono avere. Il modo migliore per ridurre questo rischio è garantire che si usa solo componenti che non dispongono di alcuna vulnerabilità note associate.
