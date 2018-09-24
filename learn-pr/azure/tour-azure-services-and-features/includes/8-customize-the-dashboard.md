Qui di seguito viene descritto come creare e modificare alcuni dashboard usando il portale di Azure e modificando direttamente il file con estensione json sottostante.

## <a name="what-is-a-dashboard"></a>Che cos'è un dashboard?

Un _dashboard_ è una raccolta personalizzabile di riquadri di interfaccia utente visualizzata nel portale di Azure. È possibile aggiungere, rimuovere e posizionare i riquadri per creare la visualizzazione esatta desiderata e quindi salvarla come dashboard. Sono supportati più dashboard ed è possibile passare tra un dashboard e l'altro a seconda delle esigenze. È anche possibile condividere i dashboard con altri membri del team.

I dashboard offrono una notevole flessibilità in termini di modalità di gestione di Azure. Ad esempio, è possibile creare dashboard per ruoli specifici all'interno dell'organizzazione, quindi usare il controllo degli accessi in base al ruolo per specificare quali utenti possono accedere ai dashboard. L'amministratore del database avrebbe quindi accesso a un dashboard che contiene le visualizzazioni del servizio di Database SQL, mentre l'amministratore di Azure Active Directory avrebbe accesso alle visualizzazioni degli utenti e dei gruppi di Azure AD. È anche possibile personalizzare il portale tra gli ambienti di produzione e sviluppo all'interno del portale, creando un dashboard specifico per ogni ambiente che si gestisce.

I dashboard vengono archiviati come file JSON (JavaScript Object Notation). Ciò significa che possono essere caricati e scaricati in altri computer o essere condivisi con i membri della directory di Azure. Azure archivia i dashboard all'interno di gruppi di risorse, proprio come le macchine virtuali o gli account di archiviazione che è possibile gestire all'interno del portale.

> [!TIP]
> Poiché i dashboard sono file JSON, è possibile anche [personalizzarli a livello di programmazione](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically), trasformandoli quindi in validi strumenti di amministrazione. Alcuni tipi di riquadri possono anche essere basati su query, in modo da essere aggiornati automaticamente quando i dati di origine cambiano.

## <a name="explore-the-default-dashboard"></a>Esplorare il dashboard predefinito

Il dashboard predefinito è denominato "Dashboard". Quando si accede al portale, viene visualizzato questo dashboard contenente cinque Web part.

![Web part predefinite](../media/8-dashboard-default-webparts.png)

Le Web part predefinite sono

1. Tutte le risorse

1. Introduzione ad Azure

1. Guide introduttive ed esercitazioni

1. Marketplace

1. Integrità dei servizi

## <a name="creating-and-managing-dashboards"></a>Creazione e gestione dei dashboard

Nella parte superiore del dashboard sono presenti i controlli che consentono di creare, caricare, scaricare, modificare e condividere un dashboard. Sono presenti anche i controlli per visualizzare un dashboard a schermo intero, clonarlo o eliminarlo.

![Personalizzare i controlli del dashboard](../media/8-customise-dashboard-controls.png)

## <a name="select-dashboard"></a>Selezionare un dashboard

All'estrema sinistra della barra degli strumenti è presente il controllo a discesa **Dashboard**. Facendo clic su questo controllo è possibile selezionare i dashboard che sono già stati definiti per l'account. Grazie a questo controllo è facile definire più dashboard per scopi diversi e quindi spostarsi tra l'uno e l'altro, a seconda delle azioni che si intende eseguire al momento.

Tutti i dashboard sono inizialmente privati, ossia sono accessibili solo a chi li ha creati. Per rendere un dashboard disponibile agli altri utenti in un'azienda, è necessario condividerlo. Questa opzione verrà esaminata a breve.

## <a name="create-a-new-dashboard"></a>Creare un nuovo dashboard

Per creare un nuovo dashboard, fare clic su **Nuovo dashboard**. Verrà visualizzata l'area di lavoro del dashboard priva di riquadri. È quindi possibile aggiungere, rimuovere e regolare i riquadri come si preferisce. Dopo aver finito di personalizzare il dashboard, fare clic su **Fatto** per salvare e passare a tale dashboard.

## <a name="upload-and-download"></a>Caricare e scaricare

I pulsanti **Carica** e **Scarica** consentono di scaricare il dashboard corrente come file con estensione json, personalizzarlo e quindi distribuirlo e caricarlo o farlo caricare da un altro utente di nuovo nel portale di Azure, in sostituzione del dashboard corrente.

Se si fa clic su **Scarica**, il dashboard corrente viene scaricato nella cartella dei download predefinita. Quando si apre il file scaricato, viene visualizzato il codice JSON.

![Codice JSON del dashboard](../media/8-dashboard-json-code.png)

È possibile modificare il codice manualmente, ad esempio si possono modificare le dimensioni dei riquadri, e successivamente caricarlo in Azure usando il pulsante **Carica**.

### <a name="edit-a-dashboard"></a>Modificare un dashboard

Sebbene sia possibile modificare un dashboard scaricando il file JSON, modificando i valori nel file e caricandolo nuovamente in Azure, questo approccio non è intuitivo per la progettazione di un'interfaccia utente. Per usare l'interfaccia utente grafica per configurare il dashboard corrente, è possibile accedere alla modalità di modifica in diversi modi:

1. Fare clic sul pulsante **Modifica**.
1. Fare clic con il pulsante destro del mouse sul dashboard e scegliere **Modifica**. 
1. Passare il mouse su un riquadro del dashboard. Verrà visualizzato un menu `...` nell'angolo in alto a destra con le opzioni di modifica.

Il dashboard passerà alla modalità di modifica.

![Modificare il dashboard](../media/8-edit-dashboard.png)

Sul lato sinistro è visualizzata la Raccolta riquadri con una serie di riquadri possibili. È possibile filtrare la Raccolta riquadri per categoria e tipo di risorsa:

![Raccolta riquadri](../media/8-tile-gallery.png)

Per aggiungere un riquadro basta selezionare quello desiderato nell'elenco a sinistra e trascinarlo nell'area di lavoro. È quindi possibile posizionare ogni riquadro, ridimensionarlo o modificare i dati in esso visualizzati.

> [!TIP]
> Una funzionalità interessante ma nota a pochi consente di inserire nel dashboard elementi dei pannelli figlio. È sufficiente passare il mouse sull'elemento e cercare il menu di modifica dei riquadri `...`, che contiene un'opzione "Aggiungi al dashboard" che consente di inserire rapidamente un riquadro da un servizio all'interno del dashboard.

L'area di lavoro in modalità di modifica è suddivisa in sezioni quadrate. Ogni riquadro deve occupare almeno un quadrato e i riquadri si allineano al set di divisori di riquadri più grande e più vicino. Tutti i riquadri che si sovrappongono vengono spostati. Quando si crea un riquadro di dimensioni più piccole, i riquadri circostanti si riallineano rispetto ad esso.

#### <a name="change-tile-sizes"></a>Modificare le dimensioni dei riquadri

Alcuni riquadri hanno dimensioni definite che possono essere modificate solo a livello di programmazione. I riquadri con un angolo inferiore destro grigio invece possono essere modificati trascinando l'indicatore angolare.

![Riquadro ridimensionabile](../media/8-resizable-tile.png)

In alternativa, è possibile fare clic con il pulsante destro del mouse sul menu di scelta rapida e specificare le dimensioni desiderate.

![Dimensioni del riquadro](../media/8-tile-size.png)

Per creare il dashboard, trascinare i riquadri dalla Raccolta riquadri nell'area di lavoro e quindi disporli nell'ordine desiderato.

#### <a name="change-tile-settings"></a>Modificare le impostazioni dei riquadri

Alcuni riquadri dispongono di impostazioni modificabili. Ad esempio, quando viene trascinato nell'area di lavoro, il riquadro Orologio visualizza il riquadro **Modifica orologio**. È quindi possibile impostare il fuso orario e anche scegliere se visualizzare l'ora in formato di 12 o 24 ore.

![Modificare l'orologio](../media/8-edit-clock.png)

Per le aziende multinazionali o intercontinentali, è possibile aggiungere orologi, ciascuno con un fuso orario diverso.

#### <a name="accepting-your-edits"></a>Accettare le modifiche

Dopo avere disposto i riquadri secondo le proprie esigenze, fare clic su **Fatto** oppure fare clic con il pulsante destro del mouse e selezionare **Fatto**.

## <a name="edit-a-dashboard-by-changing-the-json-file"></a>Modificare un dashboard modificando il file con estensione json

È possibile modificare un dashboard modificando il file con estensione json. Questo approccio offre più opzioni per modificare le impostazioni, ma non è possibile vedere le modifiche fino a quando non si carica di nuovo il file in Azure.

![Impostazioni JSON](../media/8-json-code.png)

Nell'esempio precedente, per modificare le dimensioni del riquadro, modificare le variabili **colSpan** e **rowSpan**, quindi salvare il file e caricarlo di nuovo in Azure. È anche possibile distribuire il file ad altri utenti.

## <a name="reset-a-dashboard"></a>Reimpostare lo stato predefinito di un dashboard

È possibile reimpostare un dashboard sullo stile predefinito. In modalità di modifica fare clic con il pulsante destro del mouse e selezionare **Ripristina valori predefiniti**. Verrà visualizzata una finestra di dialogo che chiederà di confermare che si intende reimpostare il dashboard.

## <a name="share-or-unshare-a-dashboard"></a>Condividere o annullare la condivisione di un dashboard

Un dashboard appena creato è privato e visibile solo all'account che li ha creati. Per renderlo visibile ad altri utenti, è necessario condividerlo. Come per qualsiasi altra risorsa di Azure, è tuttavia necessario specificare un gruppo di risorse (o usare un gruppo di risorse esistente) in cui memorizzare il dashboard condiviso. Se non si dispone di un gruppo di risorse esistente, Azure creerà un gruppo di risorse *dashboard* nel percorso che si specifica. Se si dispone di gruppi di risorse esistenti, è possibile specificarne uno in cui archiviare i dashboard.

![Condivisione e controllo dell'accesso 1](../media/8-share-dashboards-default.png)

Dopo avere condiviso il modello, viene visualizzato un secondo pannello **Condivisione e controllo dell'accesso**.

![Condivisione e controllo dell'accesso 2](../media/8-share-dashboards-access-control.png)

È quindi possibile fare clic su **Gestisci utenti** per specificare gli utenti che hanno accesso a tale dashboard.

### <a name="switching-to-a-shared-dashboard"></a>Passaggio a un dashboard condiviso

Per passare a un dashboard condiviso, fare clic sull'elenco dei dashboard, quindi su **Sfoglia tutti i dashboard**.

![Sfogliare tutti i dashboard](../media/8-browse-dashboards.png)

Verrà a questo punto visualizzato il pannello **Tutti i dashboard**, con i nomi di tutti i dashboard condivisi visualizzati. È sufficiente fare clic su un dashboard per visualizzarlo nel portale di Azure.

![Dashboard condivisi](../media/8-select-shared-dashboard.png)

## <a name="display-a-dashboard-as-a-full-screen"></a>Visualizzare un dashboard in modalità schermo intero

Se si vuole visualizzare il dashboard alle massime dimensioni, fare clic sul pulsante **Schermo intero** per visualizzare il dashboard corrente senza i menu del browser. Se sono presenti riquadri oltre i limiti dello schermo, vengono visualizzate le barre di scorrimento a destra e in basso.

Dopo avere completato le azioni necessarie nella modalità schermo intero, premere ESC o fare clic su **Chiudi la visualizzazione schermo intero** accanto al nome del dashboard in cima alla schermata.

## <a name="clone-a-dashboard"></a>Clonare un dashboard

Quando si clona un dashboard, viene creata una copia istantanea denominata "Duplicato di \<nome del dashboard>", che diventa automaticamente il dashboard corrente. La clonazione è anche un modo semplice per creare dashboard prima di condividerli. Se ad esempio si dispone di un dashboard che è quasi come lo si vorrebbe esattamente, è sufficiente clonarlo, apportarvi le modifiche necessarie e quindi condividerlo.

## <a name="delete-a-dashboard"></a>Eliminare un dashboard

L'eliminazione di un dashboard ne determina la rimozione dall'elenco dei dashboard disponibili. Viene chiesto di confermare che si intende eliminare il dashboard e non sono disponibili funzionalità per il ripristino di un dashboard che è stato eliminato.

È possibile provare alcune di queste opzioni tramite la creazione di un nuovo dashboard.
