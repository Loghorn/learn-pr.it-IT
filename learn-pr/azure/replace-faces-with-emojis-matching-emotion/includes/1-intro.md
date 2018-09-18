Mojifier è un comando _slash_ di Slack che sostituisce i visi delle persone nelle immagini con emoji corrispondenti alle loro emozioni, in questo modo:

![Immagine di esempio](/media-drafts/example-mojify-image.png)

È progettato per essere usato da Slack come comando personalizzato ed è possibile assegnare al comando il nome desiderato. In questo documento è stato chiamato `mojify`.

Per eseguire il comando, digitare `/mojify <image to mojify>`, in questo modo:

![Immagine di esempio](/media-drafts/9.slack-type-mojify.png)

Il comando mojifier:

1.  Calcola l'emozione di qualsiasi persona presente nell'immagine.
2.  Associa le emozioni a emoji.
3.  Sostituisce i visi con emoji.
4.  Ripubblica l'immagine in Twitter come risposta.

Viene scritto tramite TypeScript e diverse tecnologie di Azure, tra cui [Funzioni di Azure](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai) e [Servizi cognitivi di Azure](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

In questa esercitazione viene descritto come è stato realizzato Mojifier e come creare un comando di Slack personalizzato usando tecnologie di Azure.

> TODO, qual è la posizione?
> Tutto il codice per Mojifier è disponibile in [GitHub](https://github.com/jawache/mojifier)

# <a name="requirements"></a>Requisiti

Per creare il comando mojifier, è necessario usare diversi servizi di Azure.

## <a name="azure-cognitive-services"></a>Servizi cognitivi di Azure

Servizi cognitivi di Azure è un set di API di alto livello che può essere usato per aggiungere rapidamente funzionalità di intelligenza artificiale avanzate nell'applicazione. Se è possibile effettuare una richiesta HTTP, è possibile usare Servizi cognitivi.

[Altre informazioni](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

## <a name="azure-functions"></a>Funzioni di Azure

Servizio potente come App per la logica. A volte è necessario scrivere logica di business sfruttando l'espressività completa di un linguaggio di programmazione. Funzioni di Azure è una tecnologia che permette di ospitare frammenti di codice in grado di rispondere a eventi o a richieste HTTP, mentre Azure gestisce tutti i problemi di scalabilità e permette di pagare solo per le risorse effettivamente usate.

[Altre informazioni](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai)
