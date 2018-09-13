I contenitori sono un modo moderno per distribuire le applicazioni e i processi di calcolo. Quando si usano i contenitori, le applicazioni e tutte le dipendenze vengono organizzate in pacchetti nelle cosiddette *immagini del contenitore*. Le immagini dei contenitori sono estremamente portabili grazie a un registro immagini dei contenitori. È possibile creare un'immagine del contenitore nel sistema di sviluppo e quindi eseguire un'istanza di tale immagine in un data center di Azure avendo la certezza che funzionerà senza ulteriori modifiche.

## <a name="container-efficiencies"></a>Efficienze dei contenitori

I contenitori e le immagini dei contenitori vengono compilati per poter usare in modo efficiente le risorse host, ad esempio spazio su disco, memoria e CPU. Grazie a queste efficienze, i contenitori vengono avviati velocemente. In alcuni casi, l'avvio di una nuova istanza di un contenitore è quasi immediata. Ciò consente non solo il rapido provisioning delle applicazioni, ma anche un nuovo modello di operazioni di elaborazione e scalabilità on demand.

Si immagini questo scenario: si esegue un servizio di elaborazione batch che in alcuni casi rileva un picco elevato nella domanda. Usando i contenitori, è possibile compilare un sistema che reagisce all'aumento della domanda effettuando rapidamente il provisioning di nuove istanze di contenitore per soddisfare tale aumento. Si tratta di un sistema avanzato e non facile da realizzare con le macchine virtuali tradizionali.

Oltre all'avvio veloce, con i contenitori è possibile ottenere l'"iperdensità". Ciò significa che è possibile eseguire più applicazioni e processi con meno risorse fisiche o virtuali.

## <a name="use-cases"></a>Casi d'uso

I contenitori non sono solo una piattaforma ideale per l'esecuzione di un carico di lavoro tradizionale, ad esempio i server Web, ma offrono anche nuove opportunità, come l'elaborazione batch con possibilità di burst, applicazioni compilate con un'architettura moderna e distribuita e qualsiasi elemento richieda la scalabilità on demand.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Preparare un ambiente di sviluppo del contenitore locale.
- Apprendere le operazioni di base di Docker.
- Eseguire, elencare ed eliminare i contenitori.
- Creare un'immagine personalizzata del contenitore.
- Eseguire il push delle immagini dei contenitori in un registro contenitori pubblico ed eseguire i contenitori da queste immagini.
