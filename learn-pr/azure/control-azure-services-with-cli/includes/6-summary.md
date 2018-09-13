L'interfaccia della riga di comando di Azure è una buona scelta per gli utenti senza conoscenze della riga di comando o della creazione di script. La sintassi semplice e la compatibilità multipiattaforma consentono di ridurre il rischio di errori durante l'esecuzione di attività regolari e ripetitive. In questo modulo sono stati usati i comandi dell'interfaccia della riga di comando di Azure per creare un gruppo di risorse e per distribuire un'app Web con un piccolo set di comandi. Questi comandi potrebbero essere combinati in uno script della shell come parte della soluzione di automazione. 

## <a name="cleanup"></a>Eseguire la pulizia

L'esecuzione di app Web comporta costi per la sottoscrizione. Rimuovere le risorse non necessarie per evitare addebiti non necessari. Il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse. Verranno così eliminate anche tutte le risorse nel gruppo. Dopo aver completato questo modulo, eseguire il comando seguente di Azure:

    ```azurecli
    az group delete --resource-group popupResGroup
    ```

Quando viene richiesto di confermare l'eliminazione, rispondere **Sì**. Il completamento di questo comando può richiedere alcuni minuti mentre vengono eliminate le risorse. 