In questa unità le immagini di famosi dipinti di Picasso, Pollock e Rembrandt vengono aggiunte al progetto Artworks. È possibile contrassegnare le immagini in modo che Servizio visione artificiale personalizzato sia in grado di distinguere un artista da un altro.

1. Nel progetto **Artworks** creato selezionare il segno più (**+**) a destra di **Tags** (Tag) nel pannello laterale.

     ![Selezione del pulsante di aggiunta tag](../media/2-add-tags.png)

1. Viene visualizzata una finestra di dialogo denominata **Name the tag** (Assegna un nome al tag). Digitare *painting* nel campo nome e selezionare **Save** (Salva). In questo modo viene creato il tag *painting* nell'elenco di tag. È possibile aggiungere altri tag. 

1. Ripetere il passaggio 2 per aggiungere i tag con i valori *Picasso*, *Pollock* e *Rembrandt*. Al termine dell'operazione, l'elenco di tag avrà un aspetto simile al seguente.

    ![Selezione del pulsante di aggiunta tag](../media/2-tag-list.png)

    Come si può notare, il numero di immagini del progetto contrassegnate con ognuno di questi tag è pari a zero. Aggiungere alcune immagini al progetto e assegnare i tag.

1. Scaricare [cvs-resources.zip](https://github.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/raw/master/cvs-resources.zip) contenente risorse immagine per il modulo e decomprimerlo nel computer locale. 

1. Nel portale selezionare **Add images** (Aggiungi immagini) per aggiungere le immagini al progetto.

    ![Aggiunta di immagini al progetto Artworks](../media/2-portal-click-add-images.png)

1. Nella cartella **cvs-resources** scaricata in locale nel passaggio 4 passare alla cartella "Artists\Picasso".

1. Selezionare tutti i file in "Artists\Picasso" e quindi selezionare **Open** (Apri).

    ![Selezione di un'immagine](../media/2-fe-browse-picasso-01.png)

1. Viene visualizzata la finestra di dialogo **Image upload** (Caricamento immagine) in cui sono presenti anteprime di tutte le immagini da caricare. Selezionare il campo **My Tags** (Tag personali) per aprire un elenco a discesa dei tag che è possibile assegnare a tali immagini. 

    ![Aggiunta di un tag "painting" alle immagini](../media/2-upload-picasso-tags.png)

1. Selezionare i tag *painting* e *Picasso* e quindi selezionare **Upload 7 files** (Carica 7 file) per completare il caricamento. 

1. Verificare che le immagini caricate siano presenti nel progetto Artworks e che l'elenco di tag sia stato aggiornato per indicare che sette immagini siano state contrassegnate con *Picasso* e *painting*.

    ![Immagini caricate](../media/2-portal-tagged-01.png)

1. Con sette immagini di Picasso, il Servizio visione artificiale personalizzato è in grado di identificare in modo abbastanza corretto i dipinti di Picasso. Se si esegue il training del modello a questo punto, tuttavia, il modello sarà in grado di riconoscere solo a cosa assomiglia un Picasso, ma non di identificare i dipinti di altri artisti. Il passaggio successivo consiste nel caricare alcuni dipinti di altri artisti. 

1. Selezionare **Add images** (Aggiungi immagini) e selezionare tutte le immagini contenute nella cartella "Artists\Rembrandt" nelle risorse del modulo. Contrassegnare le immagini con le etichette "painting" e "Rembrandt" (non "Picasso") e quindi selezionare **Upload 6 files** (Carica 6 file) per caricarle nel progetto.

    ![Immagini caricate](../media/2-upload-rembrandt.png)

1. Verificare che le immagini di Rembrandt siano visualizzate accanto a quelle di Picasso nel progetto e che "Rembrandt" sia incluso nell'elenco di tag.

    ![Immagini di Picasso e Rembrandt](../media/2-portal-tagged-02.png)

1. Si aggiungeranno ora i dipinti dell'enigmatico artista Jackson Pollock per permettere al Servizio visione artificiale personalizzato di riconoscere anche i suoi dipinti. Selezionare tutte le immagini contenute nella cartella "Artists\Pollock" nelle risorse del modulo, aggiungere alle immagini i tag con i termini "painting" e "Pollock" e quindi caricare le immagini nel progetto.

Dopo aver caricato le immagini con tag, il passaggio successivo consiste nell'eseguire il training del modello con queste immagini in modo che sia in grado di distinguere tra i dipinti di Picasso, Rembrandt e Pollock e di determinare se un dipinto è opera di uno di questi celebri artisti.