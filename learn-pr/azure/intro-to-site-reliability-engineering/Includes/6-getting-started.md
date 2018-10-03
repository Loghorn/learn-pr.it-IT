Nell'ultima unità di questo modulo si parlerà su come esplorare SRE (Site Reliability Engineering) partendo da qui. 

## <a name="reading-and-watching"></a>Lettura e visione di video

Per informazioni più dettagliate su SRE, è consigliabile leggere i tre manuali pubblicati sull'argomento

1. [_Site Reliability Engineering: How Google Runs Production Systems_](http://shop.oreilly.com/product/0636920041528.do) (Site Reliability Engineering: come Google esegue i sistemi di produzione, " noto anche con il titolo "The SRE Book")
1. [_The Site Reliability Workbook: Practical Ways to Implement SRE_](http://shop.oreilly.com/product/0636920132448.do) (Esercizi sull'affidabilità del sito: modi pratici per implementare SRE, noto anche con il titolo "The SRE Workbook")
1. [_Seeking SRE: Conversations About Running Production Systems at Scale_](http://shop.oreilly.com/product/0636920063964.do) (Alla ricerca di SRE: conversazioni sull'esecuzione di sistemi di produzione su larga scala)

Una veloce rivelazione: il principale autore di questo modulo è il curatore e redattore del terzo manuale

Ognuno di questi manuale offre informazioni importanti:

- The SRE Book: spiega nel dettaglio in che modo Google ha implementato SRE nel corso degli anni.

- The SRE Workbook: questo manuale complementare a The SRE Book non contiene solo informazioni dettagliate su SRE in Google e in pochi altri ambienti, ma offre informazioni anche su come e perché implementare SRE.

- Seeking SRE: offre un'immagine più completa del mondo SRE oltre le sue origini, includendo informazioni su come è stato implementato in altri ambienti.

È consigliabile leggere tutti i tre manuali con occhio critico. Non tutto quanto viene descritto in questi manuali potrà essere applicato all'organizzazione. È importante cercare attentamente le informazioni che sicuramente andranno ad aggiungeranno valore alla propria organizzazione. Considerare quali parti della cultura e dei valori aziendali possono supportare le attività SRE descritte e quali invece potrebbero rappresentare un problema.

Se anziché leggere si preferiscono esempi concreti, è possibile guardare l'intervento [Keys to SRE](https://www.usenix.org/conference/srecon14/technical-sessions/presentation/keys-sre) (Chiavi su SRE) di Ben Treynor, registrato in occasione della conferenza SREcon14. Treynor offre una spiegazione molto convincente sulla sua idea di SRE, per lo meno nel contesto Google. Può essere molto utile guardare altri interventi su SRE di questa [serie di conferenze](https://www.usenix.org/conferences/byname/925).

## <a name="talk-to-other-interested-people"></a>Confrontarsi con altre persone interessate

Leggere informazioni su SRE è importante, ma può essere più interessante confrontarsi con altri colleghi. Parlare delle difficoltà, dei successi e dei fallimenti che si incontrano con SRE può essere fondamentale per approfondire più aspetti su tale argomento. 

Per parlare di SRE, si organizzano numerosi meetup e conferenze. Le [conferenze SREcon](https://www.usenix.org/conferences/byname/925), distribuite a livello globale e disponibili in USENIX, sono probabilmente l'evento più rilevante (rivelazione: il principale autore di questo modulo è uno dei cofondatore di SREcon).

SRE è un contenuto sempre più trattato durante le conferenze, ad esempio [Velocity](https://conferences.oreilly.com/velocity), [LISA](https://www.usenix.org/conferences/byname/5) e le conferenze locali di DevOps [DevOps Days](https://www.devopsdays.org). Ove possibile, cercare questo e altri contenuti d'interesse nell'oggetto.

## <a name="first-steps-at-work"></a>Primi passi

Se si vuole iniziare a capire cosa accadrebbe adottando SRE nell'ambiente, è importante ricordare che SRE non è una soluzione drastica e definitiva.  È possibile iniziare ad adottare i principi e le pratiche SRE poco per volta.

Mikey Dickerson, ingegnere SRE noto per il lavoro svolto in quello che sarebbe divenuto lo United States Digital Service (ha salvato le sorti del sito Healthcare.gov), in omaggio alla scala dei bisogni di Maslow, ha proposto una gerarchia di livelli di affidabilità, citata nella sezione [Practices](https://landing.google.com/sre/book/chapters/part3.html) (Pratiche) del primo manuale SRE.

Questa gerarchia propone un monitoraggio iniziale nell'ambiente per verificarne funzionalità e affidabilità. Si tratta del primo passo per poter adottare SRE anche nell'ambiente. Non è possibile stabilire se qualcosa è affidabile, o se migliora o peggiora la situazione, se non si può misurare.

Dopo aver creato una piattaforma di monitoraggio affidabile, il passaggio successivo è scegliere un servizio attivo e iniziare a discutere su quelli che sono gli indicatori e gli obiettivi sul livello del servizio. Iniziare in modo semplice. Creare indicatori e obiettivi sul livello del servizio scelto, implementarli nel sistema di monitoraggio e osservare cosa accade quando si inizia ad analizzare l'affidabilità usando SRE. È un ottimo punto di partenza.
