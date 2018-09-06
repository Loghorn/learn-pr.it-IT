AngularJS è un framework per la creazione di applicazioni Web dinamiche chiare e concise. Per il linguaggio del modello di contenuto si usa HTML, che viene però esteso con la sintassi di data binding. L'uso di AngularJS per il data binding e l'inserimento delle dipendenze contribuisce a eliminare buona parte del codice che altrimenti sarebbe necessario scrivere per gestire gli aggiornamenti del contenuto. L'aggiornamento del contenuto dell'utente viene eseguito all'interno del browser, consentendo così di abbinare AngularJS a qualsiasi tecnologia di hosting sul lato server.

## <a name="what-must-be-installed"></a>Che cosa è necessario installare?

Essendo un framework JavaScript front-end, AngularJS deve semplicemente essere reso disponibile ai client che accedono all'applicazione. Si può fare in diversi modi.

1. Fare riferimento ad AngularJS tramite una rete per la distribuzione di contenuti (CDN).
1. Servire AngularJS dal contenuto ospitato come pacchetto Node.js usando npm come strumento di gestione pacchetti.
1. Servire AngularJS dal contenuto ospitato come pacchetto Bower.

Per semplicità, per questo modulo useremo AngularJS direttamente da una rete CDN. La dipendenza del file di script viene passata alla rete CDN invece di essere gestita direttamente nel contenuto del server, cosa che può anche aumentare la velocità di download in base alla velocità e alla distribuzione geografica della rete CDN usata per una determinata risorsa.

> [!Note]
> AngularJS è il predecessore di Angular, una riscrittura completa della piattaforma di applicazioni Web. Nonostante molti dei concetti siano simili tra le due versioni, si tratta di due progetti distinti. Angular sarà disponibile nelle versioni 2.x.y e successive, mentre AngularJS lo sarà nelle versioni che terminano con 1.x.y. AngularJS viene ancora comunemente usato per gli scenari di applicazioni Web.

## <a name="how-to-install-angularjs-via-cdn"></a>Come installare AngularJS tramite una rete CDN

    You don't really _install_ AngularJS; you just add a reference to the JavaScript file via a script tag in you HTML page. Since AngularJS is critical to any functionality of our web application, we include it within the `<head>` tag like this (as compared to later loading of JavaScript files toward the end of the `<body>` tag).

    ```html
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.2/angular.min.js"></script>
    ```
