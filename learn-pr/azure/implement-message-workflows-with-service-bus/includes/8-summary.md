In questo modulo, creato uno spazio dei nomi del Bus di servizio, una coda e un argomento nella sottoscrizione usando il portale di Azure e si messaggi inviati e ricevuti tramite la coda e argomento.

Le code e gli argomenti del bus di servizio sono strumenti eccellenti che consentono di aumentare la resilienza delle comunicazioni in un'applicazione distribuita. Fungendo da posizione di archiviazione temporanea, rendono superflua la comunicazione diretta tra i componenti e gestiscono senza problemi i picchi della domanda. È consigliabile usarli ogni volta che si dispone di un componente in grado di comunicare con un altro componente in una configurazione a regime.

[!include[](../../../includes/azure-sandbox-cleanup.md)]