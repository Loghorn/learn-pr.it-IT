In questo modulo è stata completata la creazione di uno spazio dei nomi, una coda e un argomento del bus di servizio nella sottoscrizione mediante il portale di Azure e sono stati inviati e ricevuti messaggi tramite la coda e l'argomento.

Le code e gli argomenti del bus di servizio sono strumenti eccellenti che consentono di aumentare la resilienza delle comunicazioni in un'applicazione distribuita. Fungendo da posizione di archiviazione temporanea, rendono superflua la comunicazione diretta tra i componenti e gestiscono senza problemi i picchi della domanda. È consigliabile usarli quando è presente un componente che può comunicare con un altro componente in una configurazione ad accoppiamento debole.

## <a name="clean-up"></a>Eseguire la pulizia

Le code e gli argomenti del bus di servizio nella sottoscrizione di Azure comportano un costo, anche se è probabile che si tratti di un costo ridotto se sono presenti pochi messaggi di piccole dimensioni. Il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse creato durante il primo esercizio. Verranno eliminati anche tutti gli argomenti, tutte le code, tutti gli spazi dei nomi e tutte le altre risorse del gruppo. Al termine del modulo, seguire questa procedura:

1. Nel **portale di Azure** fare clic su **Gruppi di risorse** nel riquadro di spostamento a sinistra.
1. Nell'elenco dei gruppi di risorse fare clic su **SalesTeamRG**.
1. Nel pannello **Gruppo di risorse** fare clic su **Elimina gruppo di risorse**.
1. Nella casella di testo **DIGITARE IL NOME DEL GRUPPO DI RISORSE** digitare **SalesTeamAppRG** e quindi fare clic su **Elimina**. Azure rimuove il gruppo di risorse e tutte le rispettive risorse.