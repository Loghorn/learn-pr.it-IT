Uno dei principali problemi con la sicurezza è la possibilità di visualizzare tutte le aree che è necessario per proteggere e per individuare le vulnerabilità prima di eseguire operazioni di pirati informatici. Azure offre un servizio che semplifica molto questa operazione è denominata Centro sicurezza di Azure.

## <a name="what-is-azure-security-center"></a>Che cos'è il Centro sicurezza di Azure?

Centro sicurezza di Azure (ASC) è un servizio di monitoraggio che fornisce protezione dalle minacce in tutti i servizi sia in Azure e in locale. È possibile:

- Fornire raccomandazioni sulla sicurezza basate sulle configurazioni, risorse e reti.
- Monitorare le impostazioni di sicurezza locali e cloud i carichi di lavoro e applicare automaticamente protezione necessari ai nuovi servizi non appena si verificano in linea.
- In modo continuo monitorare tutti i servizi ed eseguire valutazioni della sicurezza automatico per identificare le potenziali vulnerabilità prima che vengano sfruttate.
- Usare machine learning per rilevare e bloccare il malware viene installato nei servizi e macchine virtuali. È anche possibile elenco elementi consentiti le applicazioni per assicurarsi che siano consentite solo le app che la convalida da eseguire.
- Analizzare e identificare potenziali attacchi in ingresso e consentono di analizzare le minacce e le attività post violazione che sono stati generati.
- Controllo di accesso Just-In-Time per le porte, riducendo la superficie di attacco assicurandosi solo la rete consenta il traffico desiderato.

Centro sicurezza di AZURE fa parte di [Center for Internet Security](https://www.cisecurity.org/cis-benchmarks/) raccomandazioni (CIS).

## <a name="activating-azure-security-center"></a>Attivazione di Centro sicurezza di Azure

Centro sicurezza di Azure fornisce la gestione unificata della sicurezza e protezione avanzata dalle minacce per carichi di lavoro cloud ibridi ed è offerto in due livelli: gratuito e Standard. Il livello gratuito fornisce i criteri di sicurezza, valutazione e consigli, mentre il livello Standard offre un solido set di funzionalità, tra cui intelligence per le minacce.

Considerati i vantaggi del Centro sicurezza di AZURE, il team di sicurezza della società ha deciso che può essere attivata per tutte le sottoscrizioni presso la sede. È stato ottenuto un messaggio di posta elettronica questa mattina per attivarlo per le applicazioni: quindi vediamo come procedere.

1. Aprire il [portale di Azure](https://portal.azure.com?azure-portal=true) e selezionare **Centro sicurezza di Azure** dal menu a sinistra, se non è visibile, è possibile selezionare **tutti i servizi** e individuare  **Il Centro sicurezza** nella sezione sicurezza, come illustrato di seguito.

![Aprire Centro di sicurezza di Azure](../media-draft/ASC-Menu.png)

2. Se è mai stato aperto Centro sicurezza di AZURE, il pannello inizierà il giorno le **introduttiva** voce che potrebbe venire richiesto di aggiornare la sottoscrizione. Ignorare temporaneamente, selezionare **Skip** nella parte inferiore della pagina e quindi selezionare **Panoramica**.
    - Verrà visualizzata l'immagine di sicurezza di big data"" in tutti gli elementi disponibili nella sottoscrizione.
    - Produce una considerevole quantità di informazioni eccezionali, che è possibile esplorare.

3. Successivamente, selezionare **Coverage**, nella sezione "Criteri e conformità". Verrà visualizzato ciò che gli elementi di sottoscrizione vengono trattati o non coperto da ACS. Qui è possibile disabilitare nel servizio contenitore di AZURE per potere accedere a tutte le sottoscrizioni. Provare a passare tra le tre aree di copertura: "Non coperto", "Copertura Basic" e "Copertura Standard".

4. Le sottoscrizioni che non vengono analizzate avrà un prompt dei comandi per l'attivazione di ACS. È possibile premere il pulsante "Aggiorna ora" per abilitare ACS per tutte le risorse nella sottoscrizione.

![Eseguire l'aggiornamento di Code Coverage](../media-draft/Upgrade-Now.png)

### <a name="free-vs-standard-pricing-tier"></a>Visual Studio gratuito. Piano tariffario Standard

Sebbene sia possibile usare un livello di sottoscrizione gratuita di Azure con Centro sicurezza di AZURE, è limitata alle valutazioni e le raccomandazioni di solo le risorse di Azure. Per sfruttare veramente Centro sicurezza di AZURE, è necessario eseguire l'aggiornamento a una sottoscrizione di livello Standard, come illustrato in precedenza. È possibile aggiornare la sottoscrizione tramite il pulsante "Aggiorna ora" nel **Coverage** pannello come indicato in precedenza. È anche possibile passare per il **introduttiva** pannello nel menu di Centro sicurezza di AZURE che illustra la modifica del livello di abbonamento. I prezzi e le funzionalità possono variare in base alla regione, è possibile ottenere una panoramica completa [pagina dei prezzi](https://azure.microsoft.com/en-us/pricing/details/security-center/). 

> [!NOTE]
> Per aggiornare una sottoscrizione al livello Standard, è necessario disporre del ruolo di proprietario della sottoscrizione, collaboratore alla sottoscrizione o amministratore della sicurezza.

> [!IMPORTANT]
> Al termine del periodo di valutazione di 60 giorni, costi ASC **$15/nodo al mese** e verranno fatturate al proprio account.

## <a name="turning-off-azure-security-center"></a>Attivare o disattivare il Centro sicurezza di Azure

Per i sistemi di produzione, sicuramente è consigliabile mantenere attivata in modo che è possibile monitorare tutte le risorse per le minacce Centro sicurezza di Azure. Tuttavia, se sono solo un esempio con Centro sicurezza di AZURE e averla attivata, probabilmente ci si vuole disabilitare in modo da garantire che non vengono fatturate. È possibile farlo ora.

1. Aprire il [portale di Azure](https://portal.azure.com?azure-portal=true) e selezionare **Centro sicurezza di Azure** dal menu a sinistra, se non è visibile, è possibile selezionare **tutti i servizi** e individuare  **Il Centro sicurezza** nella sezione sicurezza, come illustrato di seguito.

![Aprire Centro di sicurezza di Azure](../media-draft/ASC-Menu.png)

2. Selezionare **criteri di sicurezza** dal menu a sinistra.

3. Successivamente, selezionare **modificare le impostazioni >** accanto alla sottoscrizione per cui si desidera eseguire il downgrade di Centro sicurezza di AZURE.

4. Nella schermata successiva selezionare "Piano tariffario" dal menu a sinistra.

5. Verrà visualizzata una nuova pagina che è simile a quella riportata di seguito. Fare clic sulla casella a sinistra con la dicitura "Gratuito (per le risorse di Azure solo)".

![Piano tariffario](../media-draft/Pricing-Tier.png)

6. Fare clic sul pulsante "Salva" nella parte superiore della schermata.

È stato ora effettuato il downgrade la sottoscrizione per il livello gratuito del Centro sicurezza di Azure.

## <a name="summary"></a>Riepilogo

<!-- TODO: need link to module --> Complimenti, il passaggio primo (e più importante) eseguite per protezione di applicazioni, dati e rete. <!--If you want to learn more about Azure Security Center, you can go through the **Protect your resources with Azure Security Center** learning module.-->
