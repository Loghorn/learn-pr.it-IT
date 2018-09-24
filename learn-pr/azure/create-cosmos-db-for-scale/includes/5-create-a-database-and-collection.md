Dopo avere appreso il modo in cui le unità richiesta vengono usate per determinare la velocità effettiva del database e come la chiave di partizione crea la strategia di scale-out per il database, si è pronti per creare il database e la raccolta. I valori della velocità effettiva e della chiave di partizione devono essere impostati durante la creazione della raccolta, quindi è opportuno avere chiari tali concetti prima di creare un database.

## <a name="creating-your-database-and-collection"></a>Creare il database e la raccolta

1. Nel portale di Azure selezionare **Esplora dati** dalla risorsa di Cosmos DB e fare clic sul pulsante **Nuova raccolta** nella barra degli strumenti.
    
    L'area **Aggiungi raccolta** viene visualizzata all'estrema destra. Potrebbe essere necessario scorrere verso destra per visualizzarlo.

    ![Esplora dati nel portale di Azure: pannello Aggiungi raccolta](../media/5-azure-cosmosdb-data-explorer.png)

1. Nella pagina **Aggiungi raccolta** immettere le impostazioni per la nuova raccolta.

    Impostazione | Valore consigliato | DESCRIZIONE
    --------|-----------------|-------------
    ID database      | Prodotti         | Immettere *Products* come nome del nuovo database. I nomi dei database devono avere una lunghezza compresa tra 1 e 255 caratteri e non possono contenere /, \\, #, ? o spazi finali.
    ID raccolta    | Clothing  | Immettere *Clothing* come nome della nuova raccolta. Gli ID delle raccolte prevedono gli stessi requisiti relativi ai caratteri dei nomi di database.
    Capacità di archiviazione | Illimitato     | Usare il valore predefinito **Illimitato**. Questo valore è la capacità di archiviazione del database e consente al database di ridimensionarsi in base alle esigenze.
    Chiave di partizione    | productId        | L'ID prodotto è una buona chiave di partizione per uno scenario di vendita online. Molte domande si basano infatti sull'ID prodotto.
    Velocità effettiva       |1000 UR        | Modificare la velocità effettiva in 1000 unità richiesta al secondo (UR/sec). 1000 è il valore di UR/s minimo che è possibile impostare per abilitare la scalabilità automatica.
    
    Per il momento, non selezionare l'opzione **Provision database throughput** (Velocità effettiva di provisioning del database) e non aggiungere chiavi univoche alla raccolta.
    
1. Fare clic su **OK**. In Esplora dati verranno visualizzati il nuovo database e la nuova raccolta.

    ![Esplora dati nel portale di Azure, con il nuovo database e la nuova raccolta](../media/5-azure-cosmos-db-new-collection.png)

## <a name="summary"></a>Riepilogo

In questa unità, è stata usata la conoscenza delle chiavi di partizione e delle unità richiesta per creare un database e una raccolta con impostazioni di velocità effettiva e ridimensionamento adeguate alle esigenze aziendali.