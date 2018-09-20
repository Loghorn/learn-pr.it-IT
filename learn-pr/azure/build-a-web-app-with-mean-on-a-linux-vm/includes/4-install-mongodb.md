Un database è necessario nella maggior parte delle applicazioni. MongoDB, che rappresenta la lettera "M" nello stack MEAN, è una delle soluzioni di database NoSQL più note ed è gratuita e open source. Un database NoSQL non richiede che i dati siano strutturati in un modo predefinito, come accade invece con un database relazionale come SQL Server o MySQL.

MongoDB archivia i dati in documenti in un formato simile a JSON che non richiedono strutture dei dati rigide. Per interagire con MongoDB si usano quindi query e comandi inviati in formato JSON (JavaScript Object Notation).

## <a name="mongodb-versions"></a>Versioni di MongoDB

Sono disponibili due versioni di MongoDB:

- MongoDB Community Server
- MongoDB Enterprise Server

In questo modulo verrà usato MongoDB Community Server, disponibile nella versione 3.6 al momento della stesura di questo articolo, per l'archivio dati dell'applicazione Web.

## <a name="how-to-install-mongodb"></a>Come installare MongoDB

MongoDB può essere installato su Linux, macOS e Windows. Per questo modulo, verrà installato nella macchina virtuale Ubuntu Linux, ma lo strumento di gestione pacchetti consigliato varia in base al sistema operativo e alla distribuzione. Il processo di installazione per tutti i sistemi operativi è ben documentato nella [documentazione per l'installazione di MongoDB](https://docs.mongodb.com/manual/administration/install-community/).

Per eseguire l'installazione nella macchina virtuale Ubuntu, verrà usato lo strumento di gestione pacchetti **apt-get**.