### <a name="exercise-7-delete-the-resource-group"></a>Esercizio 7: Eliminare il gruppo di risorse

In questo esercizio si elimineranno il gruppo di risorse che contiene il bot e tutte le risorse associate. L'eliminazione del gruppo di risorse comporta l'eliminazione di tutti gli elementi al suo interno e impedisce l'addebito di ulteriori costi per il gruppo. I gruppi di risorse che vengono eliminati non possono essere ripristinati, dunque prima di eliminarli assicurarsi di non averne più bisogno.

1. Tornare al pannello del gruppo di risorse "factbot-rg". Quindi fare clic su **Elimina gruppo di risorse** nella parte superiore del pannello.

    ![Eliminazione del gruppo di risorse](../images/delete-resource-group.png)

    _Eliminazione del gruppo di risorse_

1. Per motivi di sicurezza, è necessario digitare il nome del gruppo di risorse. Una volta eliminato, infatti, un gruppo di risorse non può essere recuperato. Digitare il nome del gruppo di risorse. Fare clic sul pulsante **Elimina** per rimuovere tutte le tracce di questo lab dalla sottoscrizione di Azure.

Dopo qualche minuto, il gruppo di risorse e tutte le risorse che contiene verranno eliminati. Quando si fa clic su **Elimina** la fatturazione si interrompe, quindi il tempo necessario per eliminare le risorse non viene addebitato. Analogamente, la fatturazione non inizia finché le risorse non sono state distribuite correttamente e completamente.

### <a name="summary"></a>Riepilogo

Molto altro che è possibile eseguire per sfruttare la potenza del servizio Azure Bot incorporando[dialoghi](http://aihelpwebsite.com/Blog/EntryId/9/Introduction-To-Using-Dialogs-With-The-Microsoft-Bot-Framework), [FormFlow](https://blogs.msdn.microsoft.com/uk_faculty_connection/2016/07/14/building-a-microsoft-bot-using-microsoft-bot-framework-using-formflow/) e [Language Understanding Intelligent Service (LUIS)](https://docs.botframework.com/en-us/node/builder/guides/understanding-natural-language/). Con queste e altre funzionalità, è possibile creare bot sofisticati che rispondono alle domande e ai comandi degli utenti e interagiscono in modo fluido, colloquiale e non lineare. Per altre informazioni e per idee su come iniziare, vedere [What is Microsoft Bot Framework Overview](https://blogs.msdn.microsoft.com/uk_faculty_connection/2016/04/05/what-is-microsoft-bot-framework-overview/) (Panoramica su Microsoft Bot Framework).