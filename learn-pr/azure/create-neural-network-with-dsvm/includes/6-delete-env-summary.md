### <a name="exercise-6-delete-the-data-science-vm"></a>Esercizio 6: Eliminare la Data Science VM

In questo esercizio si eliminerà il gruppo di risorse creato nell'esercizio 1 durante la creazione della Data Science VM. L'eliminazione del gruppo di risorse comporta l'eliminazione di tutti gli elementi al suo interno e impedisce l'addebito di ulteriori costi per il gruppo. I gruppi di risorse che vengono eliminati non possono essere ripristinati, dunque prima di eliminarli assicurarsi di non averne più bisogno. È comunque **importante non lasciare questo gruppo di risorse distribuito più del necessario** perché una Data Science VM è moderatamente costosa.

1. Tornare al pannello per il gruppo di risorse creato nell'esercizio 1. Fare quindi clic sul pulsante **Elimina gruppo di risorse** nella parte superiore del pannello.

    ![Eliminazione del gruppo di risorse](../images/delete-resource-group.png)

    _Eliminazione del gruppo di risorse_

1. Per motivi di sicurezza, è necessario digitare il nome del gruppo di risorse. Una volta eliminato, infatti, un gruppo di risorse non può essere recuperato. Digitare il nome del gruppo di risorse. Fare clic sul pulsante **Elimina** per rimuovere tutte le tracce di questo lab dalla sottoscrizione di Azure.

Dopo qualche minuto, il gruppo di risorse e tutte le risorse che contiene verranno eliminati. Quando si fa clic su **Elimina** la fatturazione si interrompe, quindi il tempo necessario per eliminare le risorse non viene addebitato. Analogamente, la fatturazione non inizia finché le risorse non sono state distribuite correttamente e completamente.

### <a name="summary"></a>Riepilogo

I passaggi descritti in questo lab potrebbero essere generalizzati per eseguire altri tipi di attività di classificazione delle immagini. Ad esempio, si potrebbe eseguire il training dello stesso modello di TensorFlow per riconoscere le immagini di gatti o per identificare parti difettose prodotte da una catena di montaggio. La classificazione delle immagini è attualmente uno degli usi più diffusi del Machine Learning, la cui utilità è destinata ad aumentare nel corso del tempo. Ora che è disponibile una base di partenza, provare a creare alcuni modelli di classificazione delle immagini personalizzati per sperimentare e trovare idee utili.