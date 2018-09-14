Quando si usa Hub eventi, è fondamentale monitorare l'hub per assicurarsi che funzioni come previsto.

Continuando con l'esempio relativo al settore bancario, si supponga di aver distribuito Hub eventi di Azure e di aver configurato le applicazioni mittente e ricevitore. Le applicazioni sono pronte per il test della soluzione di elaborazione dei pagamenti. L'applicazione mittente raccoglie i dati relativi alla carta di credito del cliente, mentre l'applicazione ricevitore verifica la validità della carta di credito. A causa della natura riservata dell'attività aziendale, è essenziale che l'elaborazione dei pagamenti sia efficace e affidabile, anche quando risulta temporaneamente non disponibile.

È necessario valutare l'hub eventi verificando che l'elaborazione dei dati avvenga nel modo previsto. Grazie alle metriche disponibili in Hub eventi è possibile assicurarsi che il sistema funzioni correttamente.

## <a name="how-do-you-use-the-azure-portal-to-view-your-event-hub-activity"></a>Come usare il portale di Azure per visualizzare l'attività dell'hub eventi

Nella pagina Panoramica del portale di Azure per l'hub eventi sono visualizzati i conteggi dei messaggi. Questi conteggi dei messaggi rappresentano i dati (eventi) ricevuti e inviati dall'hub eventi. È possibile scegliere la scala cronologica per la visualizzazione di questi eventi.

![Visualizzare i messaggi dell'hub eventi](../media-draft/6-view-messages.png)

## <a name="how-can-you-test-event-hub-resilience"></a>Come testare la resilienza dell'hub eventi

Hub eventi di Azure continua a ricevere messaggi dall'applicazione mittente anche quando non è disponibile. I messaggi ricevuti durante questo periodo vengono trasmessi non appena l'hub diventa disponibile.

Per testare questa funzionalità, è possibile usare il portale di Azure per disabilitare l'hub eventi.

Quando si abilita nuovamente l'hub eventi, è possibile eseguire nuovamente l'applicazione ricevitore e usare le metriche di Hub eventi per lo spazio dei nomi per controllare se tutti i messaggi del mittente sono stati trasmessi e ricevuti correttamente.

Altre metriche utili disponibili in Hub eventi includono:

- Richieste limitate: numero di richieste che sono state limitate perché è stato superato il limite per l'uso delle unità elaborate.
- Connessioni attive: numero di connessioni attive in uno spazio dei nomi o un hub eventi.
- Byte in ingresso/in uscita: numero di byte inviati al/ricevuti dal servizio Hub eventi di Azure in un periodo specificato.

## <a name="summary"></a>Riepilogo

Il portale di Azure fornisce conteggi dei messaggi e altre metriche che è possibile usare come controllo di integrità per gli hub eventi.
