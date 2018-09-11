Supponiamo che sia stato chiesto di creare un sistema in Azure e di fare una stima dei costi che il sistema avrebbe nei 12 mesi successivi. Si sa già che i prezzi di Azure sono pienamente trasparenti e che ogni mese vengono fatturati solo i servizi usati. Come si può ricavare tale stima senza distribuire ed eseguire i servizi o senza determinare manualmente i prezzi di ogni servizio dalle pagine sui prezzi dei servizi di Azure? 

## <a name="introducing-the-azure-pricing-calculator"></a>Introduzione al calcolo dei prezzi di Azure

Per agevolare i clienti nella stima dei costi, Microsoft ha sviluppato il **calcolatore prezzi di Azure**. Questo calcolatore è uno strumento gratuito basato sul Web in cui si inseriscono i servizi di Azure e si modificano le proprietà e le opzioni dei servizi e il calcolatore restituisce i costi per ciascun servizio e il costo totale per la stima completa.

In un'altra finestra o scheda del browser passare al [calcolatore prezzi di Azure](https://azure.microsoft.com/pricing/calculator/). Nella pagina del calcolatore prezzi sono presenti tre schede:

1. **Prodotti.** In questa scheda si esegue la maggior parte delle attività. Questa scheda contiene l'elenco di tutti i servizi di Azure ed è qui che si aggiungono o si rimuovono i servizi per formulare la stima.
2. **Stime.** Questa scheda include tutte le stime salvate in precedenza. Questo processo verrà esaminato più avanti.
3. **DOMANDE FREQUENTI.** Questa scheda contiene le risposte ad alcune domande frequenti.

Iniziamo con la scheda **Prodotti**. A sinistra è presente l'elenco completo delle categorie dei servizi. Se si fa clic su una qualsiasi di queste categorie, vengono visualizzati i servizi corrispondenti. È disponibile anche una casella di ricerca che consente di cercare il servizio desiderato tra tutti i servizi. Se si fa clic sul servizio, questo viene aggiunto alla stima. È possibile aggiungere un solo servizio o tanti servizi in base alle esigenze, inclusi i multipli dello stesso servizio (ad esempio più macchine virtuali). 

Dopo aver aggiunto i servizi, occorre determinarne i prezzi. Scorrendo la pagina verso il basso vengono visualizzati dettagli personalizzabili sul servizio che riguardano la determinazione del prezzo. Nelle macchine virtuali ad esempio è possibile selezionare dettagli come la regione, il sistema operativo e la dimensione di istanza. Tutte informazioni che influiranno sul piano tariffario per la macchina virtuale. Verrà visualizzato un subtotale per il servizio. Se si continua a scorrere la pagina verso il basso, si vedrà il totale completo per tutti i servizi inclusi nella stima. Insieme al totale saranno visualizzati i pulsanti per esportare, salvare e condividere la stima.

## <a name="estimate-a-solution"></a>Stimare una soluzione

Partendo da questo scenario originale immaginiamo che il sistema sia destinato a essere eseguito in due macchine virtuali di Azure e a connettersi a un'istanza di database SQL di Azure. Si intende includere anche un firewall di livello 7 in modo da garantire le funzionalità avanzate di bilanciamento del carico seguenti:

![Diagramma dell'architettura di sistema](../images/estimate-costs-architecture.png)

È possibile usare il calcolatore prezzi di Azure per determinare il costo della soluzione ed esportare la stima per condividerla con il team.

> [!NOTE]
> Assicurarsi che nel calcolatore non siano presenti elementi elencati nella stima. Se nella stima sono presenti elementi, fare clic sull'icona del cestino per ogni elemento per reimpostare la stima.

Nel calcolatore prezzi di Azure, nella scheda **Prodotti** selezionare i servizi seguenti per aggiungerli alla stima:

- Macchine virtuali
- database SQL di Azure
- Gateway applicazione

È possibile configurare i dettagli di ognuno, nella scheda **Stime** o ottenere una stima generale dei costi. Usare la regione **Stati Uniti occidentali** per tutte le risorse.

* **Macchine virtuali.** Si tratta di un'applicazione ASP.NET, quindi sarà necessario usare una macchina virtuale con **sistema operativo Windows**. Questa applicazione non richiede una potenza di calcolo eccessiva, quindi selezionare la dimensione di istanza **D2v3**. Saranno necessarie due macchine virtuali che verranno eseguite continuamente (730 ore al mese). Verrà usata l'archiviazione Premium in unità SSD per queste macchine virtuali e occorrerà un solo disco per macchina virtuale di dimensioni **E10**, per un totale di due dischi. 

* **Database SQL.** Per il database si eseguirà il provisioning di un **singolo tipo di database** usando il **modello basato su vCore**. Si vuole usare un database di quarta generazione per Utilizzo generico con 4 vCore. Saranno necessari 32 GB di spazio di archiviazione e sarà possibile mantenere una media di 16 GB di spazio di archiviazione. I criteri di conservazione saranno otto settimane, 12 mesi e cinque anni. 

* **Gateway applicazione.** Per il gateway applicazione si userà il livello Web application firewall, per garantire un livello di protezione per l'ambiente. Verranno usate solo due istanze e le dimensioni medie, poiché il carico non sarà elevato. Si prevede di elaborare 1 TB di dati al mese.

Esaminando la stima, si noterà un costo riepilogativo per ogni servizio che è stato aggiunto e un totale completo per l'intera stima. In questo caso la stima deve essere di circa **$1.400,00 al mese**. È possibile provare a impostare alcune opzioni per vedere la stima aumentare o diminuire.

## <a name="share-and-save-your-estimate"></a>Condividere e salvare la stima

La stima per la soluzione è a questo punto pronta. È possibile salvarla, per poterci tornare in un secondo momento e apportare eventuali modifiche, esportarla in Excel per un'ulteriore analisi e condividerla tramite un URL. 

Per esportare la stima, fare clic su `Export` nella parte inferiore della stima. La stima verrà scaricata in formato Excel (con estensione **xlsx**) e includerà tutti i servizi aggiunti.

È possibile condividere il foglio di calcolo di Excel o fare clic sul pulsante `Share` del calcolatore. Verrà proposto un URL che potrà essere usato per condividere la stima. Tutti gli utenti con questo collegamento saranno in grado di accedere alla stima, rendendo così più semplice la condivisione con il team.

Se si è connessi con l'account di Azure, è possibile salvare la stima, per poterci tornare in un secondo momento. Proseguire e fare clic sul pulsante **Salva**. Se si è connessi, verrà visualizzata una notifica che indica che la stima è stata salvata. Se non si è connessi, verrà visualizzato un messaggio per accedere e salvare la stima. Dopo avere salvato la stima, tornare all'inizio della pagina e selezionare la scheda **Stime**. La stima sarà visibile in questa scheda. È quindi possibile selezionarla per riaprirla o eliminarla se non serve più.

## <a name="summary"></a>Summary

È stata eseguita una stima dei costi per un set di servizi di Azure senza alcuna spesa aggiuntiva. Non è stato creato nulla e si dispone di una stima completa condivisibile su cui si possono effettuare ulteriori analisi o modifiche in futuro. È possibile usare la stima non solo per creare stime per sistemi in cui si conoscono i servizi specifici che si prevede di usare, ma anche per confrontare i servizi e l'impatto che hanno sui costi complessivi. Un esempio è Microsoft SQL Server in una macchina virtuale rispetto a Database SQL di Azure. Vediamo a questo punto come ottenere informazioni dettagliate sui costi di servizi che sono già stati distribuiti.