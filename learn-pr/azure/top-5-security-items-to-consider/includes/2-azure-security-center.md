Uno dei principali problemi a livello di sicurezza è la possibilità di controllare tutte le aree che è necessario proteggere e trovare le vulnerabilità prima che vengano individuate dai pirati informatici. Azure offre un servizio, denominato Centro sicurezza di Azure, che semplifica tutto questo.

## <a name="what-is-azure-security-center"></a>Informazioni sul Centro sicurezza di Azure

Il Centro sicurezza di Azure è un servizio di monitoraggio che offre protezione dalle minacce in tutti i servizi, sia in Azure che in locale. Il Centro sicurezza di Azure può:

- Offrire raccomandazioni sulla sicurezza in base alle configurazioni, alle risorse e alle reti specifiche.
- Monitorare le impostazioni di sicurezza nei carichi di lavoro locali e cloud e applicare automaticamente la sicurezza necessaria ai nuovi servizi che vengono portati online.
- Monitorare continuamente tutti i servizi ed eseguire valutazioni automatiche della sicurezza per identificare potenziali vulnerabilità prima che vengano sfruttate.
- Usare l'apprendimento automatico per rilevare il malware e bloccarne l'installazione nei servizi e nelle macchine virtuali. È anche possibile inserire le applicazioni nell'elenco elementi consentiti per assicurarsi che possano essere eseguite solo le app convalidate.
- Analizzare e identificare i potenziali attacchi in ingresso e consentire l'analisi delle minacce e delle eventuali attività successive alla violazione che potrebbero essersi verificate.
- Offrire il controllo di accesso JIT per le porte e ridurre così la superficie di attacco garantendo che la rete consenta solo il traffico necessario.

Il Centro sicurezza di Azure fa parte delle raccomandazioni del [Center for Internet Security](https://www.cisecurity.org/cis-benchmarks/) (CSI).

## <a name="activating-azure-security-center"></a>Attivazione del Centro sicurezza di Azure

Il Centro sicurezza di Azure fornisce la gestione unificata della sicurezza e la protezione avanzata dalle minacce per carichi di lavoro cloud ibridi ed è disponibile in due livelli: Gratuito e Standard. Il livello gratuito fornisce criteri di sicurezza, valutazioni e raccomandazioni, mentre il livello Standard offre un solido set di funzionalità, tra cui intelligence per le minacce.

Considerati i vantaggi del Centro sicurezza di Azure, il team responsabile della sicurezza dell'azienda ha deciso di attivarlo per tutte le sottoscrizioni dell'ufficio. È stato comunicato tramite posta elettronica che si deve attivare il Centro sicurezza di Azure per le proprie applicazioni. Di seguito verrà illustrato come eseguire questa operazione.

> [!IMPORTANT]
> Il Centro sicurezza di Azure non è supportato nell'ambiente sandbox di Azure gratuito. È possibile eseguire questi passaggi nella propria sottoscrizione o semplicemente leggere la procedura per capire come attivare il Centro sicurezza di Azure.

1. Aprire il [portale di Azure](https://portal.azure.com?azure-portal=true) e selezionare **Centro sicurezza di Azure** nel menu a sinistra. Se non è visualizzato in tale posizione, è possibile selezionare **Tutti i servizi** e trovare **Centro sicurezza** nella sezione Sicurezza, come illustrato di seguito.

![Aprire il Centro sicurezza di Azure](../media/2-ASC-Menu.png)

2. Se non si è mai aperto il Centro sicurezza di Azure, verrà visualizzato il pannello **Attività iniziali**, in cui potrebbe essere richiesto di aggiornare la sottoscrizione. Per il momento ignorare la richiesta e selezionare **Ignora** nella parte inferiore della pagina e quindi **Panoramica**.
    - Verrà così visualizzato un quadro complessivo della sicurezza relativo a tutti gli elementi disponibili nella sottoscrizione,
    - contenente molte informazioni utili che è possibile esplorare.

3. Selezionare quindi **Copertura** in "Criteri e conformità". Verranno così visualizzati gli elementi della sottoscrizione coperti o non coperti dal Centro sicurezza di Azure. È possibile attivare il Centro sicurezza di Azure per qualsiasi sottoscrizione a cui si ha accesso. Provare a spostarsi tra le tre aree di copertura: "Senza copertura", "Copertura Basic" e "Copertura Standard".

4. Per le sottoscrizioni senza copertura verrà richiesto di attivare il Centro sicurezza di Azure. È possibile fare clic sul pulsante "Aggiorna ora" per abilitare il Centro sicurezza di Azure per tutte le risorse nella sottoscrizione.

![Aggiornare la copertura](../media/2-Upgrade-Now.png)

### <a name="free-vs-standard-pricing-tier"></a>Piano tariffario Gratuito e Standard

Anche se è possibile usare il Centro sicurezza di Azure con una sottoscrizione di Azure di livello gratuito, il servizio in questo caso è limitato a valutazioni e raccomandazioni per le sole risorse di Azure. Per sfruttare realmente il Centro sicurezza di Azure, sarà necessario eseguire l'aggiornamento a una sottoscrizione di livello Standard come descritto sopra. È possibile aggiornare la sottoscrizione tramite il pulsante "Aggiorna ora" nel pannello **Copertura** come indicato sopra. È anche possibile passare tramite il menu del Centro sicurezza di Azure al pannello **Attività iniziali**, in cui verrà illustrato come modificare il livello della sottoscrizione. I prezzi e le funzionalità possono variare in base all'area. Per una panoramica completa, vedere la [pagina dei prezzi](https://azure.microsoft.com/pricing/details/security-center/). 

> [!NOTE]
> Per aggiornare una sottoscrizione al livello Standard, è necessario avere il ruolo di proprietario o collaboratore della sottoscrizione oppure di amministratore della sicurezza.

> [!IMPORTANT]
> Al termine del periodo di valutazione di 60 giorni, il prezzo applicato per il Centro sicurezza di Azure è di **$ 15 per nodo al mese** e viene fatturato all'account.

## <a name="turning-off-azure-security-center"></a>Disattivazione del Centro sicurezza di Azure

Per i sistemi di produzione, sarà certamente opportuno mantenere attivato il Centro sicurezza di Azure affinché possa monitorare la presenza di minacce in tutte le risorse. Se si è attivato il Centro sicurezza di Azure solo per provare, invece, è probabilmente opportuno disabilitarlo per evitare addebiti. Di seguito viene illustrato come.

1. Aprire il [portale di Azure](https://portal.azure.com?azure-portal=true) e selezionare **Centro sicurezza di Azure** nel menu a sinistra. Se non è visualizzato in tale posizione, è possibile selezionare **Tutti i servizi** e trovare **Centro sicurezza** nella sezione Sicurezza, come illustrato di seguito.

![Aprire il Centro sicurezza di Azure](../media/2-ASC-Menu.png)

2. Selezionare **Criteri di sicurezza** nel menu a sinistra.

3. Selezionare quindi **Modifica le impostazioni >** accanto alla sottoscrizione per cui si vuole effettuare il downgrade del Centro sicurezza di Azure.

4. Nella schermata successiva selezionare "Piano tariffario" nel menu a sinistra.

5. Verrà visualizzata una nuova pagina simile all'immagine seguente. Fare clic sulla casella "Gratuito (solo per le risorse di Azure)" a sinistra.

![Piano tariffario](../media/2-Pricing-Tier.png)

6. Fare clic sul pulsante **Salva** nella parte superiore della schermata.

È stato così effettuato il downgrade della sottoscrizione al livello gratuito del Centro sicurezza di Azure.

Il primo e più importante passo per proteggere l'applicazione, i dati e la rete è stato compiuto.