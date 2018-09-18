Si supponga di avere un'applicazione per il team di vendita di un'azienda globale. Ogni membro del team ha un telefono cellulare in cui verrà installata l'app. Un servizio Web ospitato in Azure implementa la logica di business per l'applicazione e archivia le informazioni nel database SQL di Azure. C'è un'istanza del servizio Web per ogni area geografica. Sono stati identificati gli scopi seguenti per l'invio di messaggi tra l'app per dispositivi mobili e il servizio Web:

- I messaggi relativi a singole vendite devono essere inviati solo all'istanza del servizio Web nell'area dell'utente.
- I messaggi relativi alle prestazioni di vendita devono essere inviati a tutte le istanze del servizio Web.

Si è deciso di implementare una coda del bus di servizio per il primo caso d'uso e l'argomento del bus di servizio per il secondo caso d'uso.

In questo esercizio si creerà uno spazio dei nomi del bus di servizio, che conterrà sia una coda che un argomento con sottoscrizioni.

## <a name="create-a-service-bus-namespace"></a>Creare uno spazio dei nomi del bus di servizio

Nel bus di servizio di Azure uno spazio dei nomi è un contenitore, con un nome di dominio completo univoco, per code, argomenti e inoltri. È necessario iniziare creando lo spazio dei nomi.

Ogni spazio dei nomi include anche le chiavi di crittografia di firma di accesso condiviso primaria e secondaria. Un componente mittente o destinatario deve fornire queste chiavi quando si connette per ottenere l'accesso agli oggetti nello spazio dei nomi.

Per creare uno spazio dei nomi del bus di servizio usando il portale di Azure, seguire questa procedura:

1. In un browser passare al [portale di Azure](https://portal.azure.com/) e accedere con le proprie credenziali dell'account di Azure.

1. Nel riquadro di spostamento a sinistra fare clic su **Tutti i servizi**.

1. Nel pannello **Tutti i servizi** scorrere verso il basso fino alla sezione **INTEGRAZIONE** e quindi fare clic su **Bus di servizio**.

    ![Creare uno spazio dei nomi del bus di servizio](../media-draft/3-create-namespace-1.png)

1. In alto a sinistra nel pannello **Bus di servizio** fare clic su **Aggiungi**.

1. Nella casella di testo **Nome** digitare un nome univoco per lo spazio dei nomi. Ad esempio "salesteamapp" + *iniziali del nome* + *data corrente*.

1. Nell'elenco a discesa **Piano tariffario** selezionare **Standard**.

1. Nell'elenco a discesa **Sottoscrizione** selezionare la sottoscrizione in uso.

1. In **Gruppo di risorse** selezionare **Crea nuovo** e quindi digitare **SalesTeamAppRG**.

1. Nell'elenco a discesa **Località** selezionare una località nelle vicinanze e quindi fare clic su **Crea**. Azure creerà il nuovo spazio dei nomi del bus di servizio.

    ![Creare uno spazio dei nomi del bus di servizio](../media-draft/3-create-namespace-2.png)

## <a name="create-a-service-bus-queue"></a>Creare una coda del bus di servizio

Dopo aver creato uno spazio dei nomi, è possibile creare una coda per i messaggi relativi alle singole vendite. A questo scopo, seguire questa procedura:

1. Nel pannello **Bus di servizio** fare clic su **Aggiorna**. Verrà visualizzato lo spazio dei nomi appena creato.

1. Fare clic sullo spazio dei nomi appena creato.

1. In alto a sinistra nel pannello dello spazio dei nomi fare clic su **Coda**.

1. Nel pannello **Crea coda**, nella casella di testo **Nome** digitare **salesmessages** e quindi fare clic su **Crea**. Azure creerà la coda nello spazio dei nomi.

    ![Creazione di una coda](../media-draft/3-create-queue.png)

## <a name="create-a-service-bus-topic-and-subscriptions"></a>Creare un argomento del bus di servizio e le sottoscrizioni

Si vuole creare anche un argomento che verrà usato per i messaggi relativi alle prestazioni di vendita. Più istanze del servizio Web per la logica di business eseguiranno la sottoscrizione di questo argomento da paesi diversi. Ogni messaggio verrà recapitato a più istanze.

Seguire questa procedura:

1. Nel pannello **Spazio dei nomi del bus di servizio** fare clic su **+ Argomento**.

1. Nel pannello **Crea argomento**, nella casella di testo **Nome** digitare **salesperformancemessages** e quindi fare clic su **Crea**. Azure creerà l'argomento nello spazio dei nomi.

    ![Creazione di un argomento](../media-draft/3-create-topic.png)

1. Quando l'argomento è stato creato, nel pannello **Spazio dei nomi del bus di servizio** fare clic su **Argomenti** in **Entità**.

1. Nell'elenco di argomenti fare clic su **salesperformancemessages** e quindi fare clic su **+ Sottoscrizione**.

1. Nella casella di testo **Nome** digitare **Americas** e quindi fare clic su **Crea**.

1. Fare clic su **+ Sottoscrizione**.

1. Nella casella di testo **Nome** digitare **EuropeAndAfrica** e quindi fare clic su **Crea**.

È stata creata l'infrastruttura necessaria per usare il bus di servizio per aumentare la resilienza dell'applicazione distribuita per la forza vendita. Sono stati creati una coda per i messaggi relativi alle singole vendite e un argomento per i messaggi relativi alle prestazioni di vendita. L'argomento include più sottoscrizioni perché i messaggi inviati all'argomento possono essere recapitati a più servizi Web di destinazione in tutto il mondo.
