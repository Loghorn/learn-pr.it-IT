AngularJS è un framework per la creazione di applicazioni Web dinamiche chiare e concise. Per il linguaggio del modello di contenuto si usa HTML, che viene però esteso con la sintassi di data binding. L'uso di AngularJS per il data binding e l'inserimento delle dipendenze contribuisce a eliminare buona parte del codice che altrimenti sarebbe necessario scrivere per gestire gli aggiornamenti del contenuto. L'aggiornamento del contenuto dell'utente viene eseguito all'interno del browser, consentendo così di abbinare AngularJS a qualsiasi tecnologia di hosting sul lato server.

## <a name="angularjs-information"></a>Informazioni su Angular.js

Essendo un framework JavaScript front-end, AngularJS deve semplicemente essere reso disponibile ai client che accedono all'applicazione. Si può fare in diversi modi.

- Fare riferimento ad AngularJS tramite una rete per la distribuzione di contenuti (CDN).
- Servire AngularJS dal contenuto ospitato come pacchetto Node.js usando npm come strumento di gestione pacchetti.
- Servire AngularJS dal contenuto ospitato come pacchetto Bower.

Per semplicità, per questo modulo verrà usato AngularJS direttamente da una rete CDN. Questo approccio passa la dipendenza del file di script alla rete CDN, invece di gestirlo direttamente nel contenuto del server. Consente anche di migliorare le velocità di download in base alla velocità e alla distribuzione geografica della rete CDN usata per una rete specifica.

> [!NOTE]
> AngularJS è il predecessore di Angular, una riscrittura completa della piattaforma di applicazioni Web. Nonostante molti concetti siano simili tra le due versioni, si tratta di due progetti distinti. Angular sarà disponibile nelle versioni 2.x.y e successive, mentre AngularJS lo sarà nelle versioni che terminano con 1.x.y. AngularJS viene ancora comunemente usato per gli scenari di applicazioni Web.

## <a name="how-to-install-angularjs-via-cdn"></a>Come installare AngularJS tramite una rete CDN

Non viene effettivamente eseguita l'_installazione_ di AngularJS. Viene semplicemente aggiunto un riferimento al file JavaScript tramite un tag di script nella pagina HTML. Poiché AngularJS è essenziale per qualsiasi funzionalità dell'applicazione Web, viene incluso nel tag `<head>` in questo modo, rispetto al caricamento successivo di file JavaScript verso la fine del tag `<body>`.

```html
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.2/angular.min.js"></script>
```
