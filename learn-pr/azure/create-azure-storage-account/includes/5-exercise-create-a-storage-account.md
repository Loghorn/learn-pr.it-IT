In questa unità si userà il portale di Azure per creare un account di archiviazione appropriato per un'app Web fittizia di previsioni per il surf nella California del sud.

Il sito di previsioni per il surf consente agli utenti di caricare foto e video delle condizioni delle spiagge locali. I visualizzatori useranno il contenuto per scegliere la spiaggia con le condizioni migliori per il surf. Ecco l'elenco degli obiettivi di progettazione e funzionalità:

- Il contenuto video deve caricarsi rapidamente.
- Il sito deve essere in grado di gestire i picchi imprevisti nei volumi di caricamento.
- Il contenuto non aggiornato deve essere rimosso man mano che cambiano le condizioni per il surf, in modo che il sito mostri sempre le condizioni attuali.

Per soddisfare questi requisiti, si opta per memorizzare nel buffer il contenuto caricato in una coda di Azure per l'elaborazione, per poi spostarlo in un BLOB di Azure per l'archiviazione. A questo scopo è necessario un account di archiviazione in grado di contenere sia le code che i BLOB fornendo al contempo un accesso con bassa latenza al contenuto.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="use-the-azure-portal-to-create-a-storage-account"></a>Usare il portale di Azure per creare un account

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.

1. In alto a sinistra nel portale di Azure selezionare **Crea una risorsa**.

1. Nel pannello di selezione visualizzato selezionare **Archiviazione**.

1. Sul lato destro del riquadro selezionare **Account di archiviazione: BLOB, File, Tabelle, Code**.

    ![Screenshot del portale di Azure che illustra il pannello Crea una risorsa con la categoria Archiviazione e l'opzione Account di archiviazione evidenziate.](..\media\5-portal-storage-select.png)

### <a name="configure-the-basic-options"></a>Configurare le opzioni di base

[!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

In **DETTAGLI PROGETTO**:

1. Scegliere la **sottoscrizione** appropriata.

1. Selezionare il gruppo di risorse esistente ("**<rgn>[nome gruppo di risorse sandbox]</rgn>**") nell'elenco a discesa.

    > [!NOTE]
    > Questo gruppo di risorse gratuito è stato reso disponibile da Microsoft nell'ambito dell'esperienza di apprendimento. Quando si crea un account per un'applicazione reale, è consigliabile creare un nuovo gruppo di risorse nella sottoscrizione per contenere tutte le risorse per l'app.

In **DETTAGLI ISTANZA**:

1. Immettere un **nome dell'account di archiviazione**. Il nome verrà usato per generare l'URL pubblico necessario per accedere ai dati nell'account. Questo nome deve essere univoco in tutti i nomi di account di archiviazione esistenti in Azure. I nomi devono avere una lunghezza compresa tra 3 e 24 caratteri e possono contenere solo lettere minuscole e numeri.

1. Selezionare dall'elenco precedente una **località** nelle vicinanze.

1. Lasciare _Resource Manager_ come **modello di distribuzione**. Questo è il modello preferito per tutte le distribuzioni di risorse in Azure e consente di raggruppare tutte le risorse correlate per l'app in un _gruppo di risorse_ per una gestione più semplice.

1. Selezionare _Standard_ per l'opzione **Prestazioni**. In tal modo viene stabilito il tipo di archiviazione su disco usato per contenere i dati nell'account di archiviazione. L'archiviazione Standard usa i tradizionali dischi rigidi, quella Premium usa le unità SSD per un accesso più rapido. Tenere tuttavia presente che Premium supporta i _BLOB di pagine_. Sono necessari _BLOB in blocchi_ per i video e una coda per la memorizzazione nel buffer ed entrambi sono disponibili solo nell'opzione _Standard_.

1. Selezionare _Archiviazione v2 (utilizzo generico v2)_ per il **tipo di account**. In questo modo è possibile accedere alle funzionalità e ai prezzi più recenti. In particolare, per gli account di archiviazione BLOB sono disponibili più opzioni con questo tipo di account. Sono infatti necessari sia BLOB che una coda, quindi l'opzione _Archiviazione BLOB_ non è adatta. La scelta di un account di tipo _Archiviazione (utilizzo generico v1)_ non porterebbe alcun vantaggio a questa applicazione, poiché limiterebbe le funzionalità accessibili senza presumibilmente ridurre il costo del carico di lavoro previsto.

1. Selezionare _Archiviazione con ridondanza locale_ per l'opzione **Replica**. I dati negli account di archiviazione di Azure vengono sempre replicati per assicurarne la disponibilità elevata. Questa opzione consente di scegliere dopo quanto la replica viene eseguita per soddisfare i requisiti di durabilità. In questo caso, le immagini e i video diventano rapidamente obsoleti e vengono rimossi dal sito. Non vale quindi la pena di pagare di più per la ridondanza globale. Nel caso in cui un evento catastrofico causi la perdita di dati, è possibile riavviare il sito con contenuti aggiornati caricati dagli utenti.

1. Impostare il **livello di accesso** su _Accesso frequente_. Questa impostazione viene usata solo per l'archivio BLOB. Il **livello di accesso frequente** è ideale per i dati a cui si accede spesso e il **livello di accesso sporadico** è migliore per i dati a cui si accede di rado. Tenere presente che in questo modo viene impostato solo il valore _predefinito_. Quando si crea un BLOB, è possibile impostare un valore diverso per i dati. In questo caso, si vuole che i video vengano caricati rapidamente, quindi si userà l'opzione con prestazioni elevate per i BLOB.

Lo screenshot seguente mostra le impostazioni compilate per la scheda **Informazioni di base**. Si noti che il gruppo di risorse, la sottoscrizione e il nome avranno valori diversi.

![Screenshot di un pannello Crea account di archiviazione con la scheda **Informazioni di base** selezionata.](../media/5-create-storage-account-basics.png)

### <a name="configure-the-advanced-options"></a>Configurare le opzioni avanzate

1. Fare clic sul pulsante **Next: Advanced >** (Avanti: Avanzate >) per passare alla scheda **Avanzate** o selezionare la scheda **Avanzate** nella parte superiore della schermata.

1. Impostare **Trasferimento sicuro obbligatorio** su _Abilitato_. L'impostazione **Trasferimento sicuro obbligatorio** determina se **HTTP** può essere usato per le API REST usate per accedere ai dati nell'account di archiviazione. Impostando questa opzione su _Abilitato_, tutti i client dovranno usare SSL (**HTTPS**). È quasi sempre opportuno impostarla su _Abilitato_ perché usare HTTPS in rete è la procedura consigliata.

    > [!WARNING]
    > Se questa opzione è abilitata, applicherà alcune restrizioni aggiuntive. Le connessioni del servizio File di Azure senza crittografia avranno esito negativo, inclusi gli scenari che usano SMB 2.1 o 3.0 in Linux. Poiché Archiviazione di Azure non supporta SSL per i nomi di dominio personalizzati, l'opzione non può essere usata con un nome di dominio personalizzato.

1. Impostare l'opzione **Reti virtuali** su _Nessuna_. Questa opzione consente di isolare l'account di archiviazione in una rete virtuale di Azure. Si vuole usare l'accesso a Internet pubblico. Il contenuto è esposto al pubblico ed è necessario consentire l'accesso da client pubblici.

1. Lasciare l'opzione **Data Lake Storage Gen2** impostata su _Disattivato_. Questa opzione è per le applicazioni Big Data che non sono rilevanti per questo modulo.

Lo screenshot seguente mostra le impostazioni compilate per la scheda **Avanzate**.

![Screenshot di un pannello Crea account di archiviazione con la scheda **Avanzate** selezionata.](../media/5-create-storage-account-advanced.png)

### <a name="create"></a>Creare

1. Volendo è possibile esplorare le impostazioni di **Tag**. Questa funzionalità consente di associare coppie chiave/valore all'account per la categorizzazione ed è disponibile per qualsiasi risorsa di Azure.

1. Fare clic su **Rivedi e crea** per rivedere le impostazioni. Verrà eseguita una veloce convalida delle opzioni per verificare che tutti i campi obbligatori siano selezionati. Se sono presenti problemi, verranno segnalati qui. Dopo aver rivisto le impostazioni, fare clic su **Crea** per effettuare il provisioning dell'account di archiviazione.

La distribuzione dell'account richiederà qualche minuto. Mentre Azure esegue questa operazione, è possibile esplorare le API che verranno usate con questo account.

### <a name="verify"></a>Verificare

1. Selezionare il collegamento **Account di archiviazione** nella barra laterale sinistra.

1. Individuare il nuovo account di archiviazione nell'elenco per verificare che la creazione sia riuscita.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

Quando si usa la propria sottoscrizione, è possibile eseguire i passaggi seguenti nel portale di Azure per eliminare il gruppo di risorse e tutte le risorse associate.

1. Selezionare il collegamento **Gruppo di risorse** nella barra laterale sinistra.

1. Individuare il gruppo di risorse creato nell'elenco.

1. Fare clic con il pulsante destro del mouse sulla voce del gruppo di risorse e scegliere **Elimina gruppo di risorse** dal menu di scelta rapida. È anche possibile fare clic sull'elemento menu "..." a destra della voce per aprire lo stesso menu di scelta rapida.

1. Digitare il nome del gruppo di risorse nel campo di conferma.

1. Fare clic sul pulsante **Elimina**. Questa operazione può richiedere alcuni minuti.

È stato creato un account di archiviazione con le impostazioni più appropriate per i propri requisiti aziendali. Ad esempio, potrebbe essere stato selezionato il data center Stati Uniti occidentali perché i clienti risiedevano principalmente nella California del sud. Il flusso descritto è un flusso tipico: prima si analizzano i dati e gli obiettivi e poi si configurano le opzioni dell'account di archiviazione più appropriate.