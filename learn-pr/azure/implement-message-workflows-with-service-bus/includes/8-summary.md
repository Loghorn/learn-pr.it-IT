In questo modulo è stata completata la creazione di uno spazio dei nomi, una coda e un argomento del bus di servizio nella sottoscrizione mediante il portale di Azure e sono stati inviati e ricevuti messaggi tramite la coda e l'argomento.

Le code e gli argomenti del bus di servizio sono strumenti eccellenti che consentono di aumentare la resilienza delle comunicazioni in un'applicazione distribuita. Fungendo da posizione di archiviazione temporanea, rendono superflua la comunicazione diretta tra i componenti e gestiscono senza problemi i picchi della domanda. È consigliabile usarli quando è presente un componente che può comunicare con un altro componente in una configurazione ad accoppiamento debole.

[!include[](../../../includes/azure-sandbox-cleanup.md)]