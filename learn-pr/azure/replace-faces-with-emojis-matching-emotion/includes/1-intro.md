TheMojifier è un Slack _barra_ comando che sostituisce i visi di persone nelle immagini con emoji corrispondenza relativi emozioni, come segue:

![Immagine di esempio](/media-drafts/example-mojify-image.png)

È progettato per funzionare da Slack come un comando personalizzato. È possibile assegnare il comando desiderato, ma per questo modulo, chiamiamo `mojify`.

Per eseguire il comando, digitare `/mojify <image to mojify>`:

![Immagine di esempio](/media-drafts/9.slack-type-mojify.png)

Verrà quindi il Mojifier:

  1.  Il calcolo delle emozioni di persone nell'immagine
  2.  Corrispondenza emozioni per emoji
  3.  Sostituisce i volti nell'immagine con emoji
  4.  Pubblica l'immagine aggiornata in Slack come risposta

Scritto con TypeScript e tecnologie di Azure diversi tra cui la Mojifier [funzioni di Azure](https://azure.microsoft.com/services/functions/) e [servizi cognitivi di Azure](https://azure.microsoft.com/services/cognitive-services/). Si userà queste per verificare la propria versione di _TheMojifier_. 

> [!NOTE] 
> È disponibile in tutto il codice per Mojifier [GitHub](https://github.com/microsoftdocs/mslearn-the-mojifier).

## <a name="tools-youll-use"></a>Strumenti necessari

### <a name="azure-cognitive-services"></a>Servizi cognitivi di Azure

Servizi cognitivi di Azure sono un set di API ad alto livello, è possibile usare per aggiungere rapidamente funzionalità avanzate di intelligenza artificiale (AI) in un'app. Se si sa come eseguire una richiesta HTTP, quindi sarà possibile usare servizi cognitivi.

### <a name="azure-functions"></a>Funzioni di Azure

Azure consente di funzioni di includere frammenti di codice in grado di rispondere a eventi o le richieste HTTP. Azure gestisce automaticamente i problemi di scalabilità, e si paga solo per le risorse usate. Come con qualsiasi parte del Microsoft Learn, si affronterà tutti i costi per l'utente nell'ambiente di apprendimento.
