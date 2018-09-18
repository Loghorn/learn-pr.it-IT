Una buona parte del codice presente nelle applicazioni moderne è costituito dalle librerie e dalle dipendenze scelte dallo sviluppatore. È una pratica comune che consente di risparmiare tempo e denaro. Ma c'è un rovescio della medaglia: lo sviluppatore che usa questo codice nel suo progetto ne è infatti interamente responsabile, anche se è stato scritto da altri. Se un ricercatore (o peggio, un pirata informatico) individua una vulnerabilità in una di queste librerie di terze parti, la stessa vulnerabilità sarà probabilmente presente anche nell'app sviluppata con questa libreria.

L'uso di componenti con vulnerabilità note rappresenta un problema enorme nel settore IT. Lo è a tal punto che da diversi anni occupa la nona posizione nella [Top 10 OWASP](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) delle peggiori vulnerabilità delle applicazioni Web.

## <a name="track-known-security-vulnerabilities"></a>Tenere traccia delle vulnerabilità della sicurezza note

Il nocciolo del problema è sapere quando viene individuata una vulnerabilità. Mantenere aggiornate le librerie e le dipendenze (posizione numero 4 nella Top 10) è sicuramente utile, ma è sempre consigliabile tenere traccia delle vulnerabilità identificate che potrebbero avere un impatto negativo sull'applicazione.

> [!IMPORTANT]
> Quando un sistema ha una vulnerabilità nota, aumentano anche le probabilità che siano disponibili exploit, ossia codice che persone malintenzionate possono usare per attaccare il sistema. Se un exploit viene reso pubblico, è fondamentale che i sistemi interessati vengano aggiornati immediatamente.

**Mitre** è un'organizzazione no profit che gestisce il [Common Vulnerabilities and Exposures (CVE)](https://cve.mitre.org), ossia l'elenco delle vulnerabilità ed esposizioni comuni. Si tratta di un elenco, consultabile pubblicamente, delle vulnerabilità a livello di sicurezza informatica in app, librerie e framework. **Se si trova una libreria o un componente nel database CVE, significa che contiene vulnerabilità note**.

Quando in un prodotto o un componente viene rilevata una falla nella sicurezza, il problema viene inviato alla community dedicata alla sicurezza. Ogni problema pubblicato è identificato da un ID e contiene la data dell'individuazione, una descrizione della vulnerabilità e riferimenti a soluzioni alternative pubblicate o istruzioni del fornitore relative al problema.

### <a name="how-to-verify-if-you-have-known-vulnerabilities-in-your-3rd-party-components"></a>Come verificare se i componenti di terze parti in uso contengono vulnerabilità

Si potrebbe impostare un'attività giornaliera sul telefono per controllare questo elenco, ma fortunatamente esistono diversi strumenti che consentono di verificare se le dipendenze in uso sono vulnerabili. È possibile eseguire questi strumenti sulla codebase o, meglio ancora, aggiungerli alla pipeline CI/CD per verificare automaticamente la presenza di problemi durante il processo di sviluppo.

- [OWASP Dependency Check](https://www.owasp.org/index.php/OWASP_Dependency_Check), che dispone di un [plug-in Jenkins](https://wiki.jenkins.io/display/JENKINS/OWASP+Dependency-Check+Plugin)
- [OWASP SonarQube](https://www.owasp.org/index.php/OWASP_SonarQube_Project)
- [Synk](https://snyk.io), gratuito per i repository open source in GitHub
- [Black Duck](https://www.blackducksoftware.com), usato da numerose aziende
- [RubySec](https://rubysec.com), un database di consulenza riservato a Ruby
- [Retire.js](https://github.com/retirejs/retire.js/), uno strumento che consente di verificare se le librerie JavaScript in uso non sono aggiornate e può essere usato come plug-in per diversi strumenti, incluso [Burp Suite](https://www.portswigger.net)

Possono essere usati allo scopo anche alcuni strumenti sviluppati specificamente per l'analisi di codice statico.

- [Roslyn Security Guard](https://dotnet-security-guard.github.io)
- [Puma Scan](https://pumascan.com)
- [PT Application Inspector](https://www.ptsecurity.com/ww-en/products/ai/)
- [Apache Maven Dependency Plugin](http://maven.apache.org/plugins/maven-dependency-plugin/)
- [Source Clear](https://www.sourceclear.com)
- [Sonatype](https://ossindex.sonatype.org)
- [Node Security Platform](https://nodesecurity.io)
- [WhiteSource](https://www.whitesourcesoftware.com/what-is-whitesource/)
- [Hdiv](https://hdivsecurity.com)
- [E molti altri ancora...](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)

Per altre informazioni sui rischi impliciti nell'uso di componenti vulnerabili, visitare la [pagina di OWASP](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities) dedicata a questo argomento.

## <a name="summary"></a>Riepilogo

Quando si usano librerie o altri componenti di terze parti per lo sviluppo della propria applicazione, si accettano anche gli eventuali rischi che possono comportare. Il modo migliore per ridurre questi rischi è assicurarsi di usare solo componenti a cui non siano associate vulnerabilità note.
