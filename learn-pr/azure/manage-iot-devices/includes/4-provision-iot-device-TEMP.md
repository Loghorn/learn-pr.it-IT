Ora che si sono acquisite le conoscenze necessarie su come l'hub IoT di Azure raccoglie di dati dai dispositivi IoT, ad esempio Raspberry Pi, è possibile procedere al provisioning di altri dispositivi IoT per consentire loro di inviare i dati. La configurazione dei dispositivi IoT in un progetto di piccole dimensioni (da 1 a 5 dispositivi) può essere eseguita per un dispositivo alla volta. Per le distribuzioni di dimensioni più elevate è consigliabile scegliere un metodo di provisioning più efficiente.

Il Servizio Device Provisioning in hub IoT consente ai clienti di configurare il provisioning automatico dei dispositivi nell'hub IoT di Azure e offre la scalabilità del cloud a quello che in precedenza era un processo lungo e da eseguire per un dispositivo alla volta. Il processo è stato progettato in base alle problematiche della catena di approvvigionamento e offre l'infrastruttura necessaria per il provisioning di milioni di dispositivi in modo sicuro e scalabile.

Il provisioning di dispositivi automatico con il servizio Device Provisioning ora supporta tutti i protocolli supportati dall'hub IoT, tra cui HTTP, AMQP, MQTT AMQP over WebSocket e MQTT over WebSocket. Questa versione corrisponde inoltre al supporto dei linguaggi dell'SDK esteso sia per il lato dispositivo che per il lato client.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Collegare il servizio Device Provisioning a un hub IoT

Il passaggio successivo consiste nel collegare il servizio Device Provisioning e l'hub IoT in modo che il servizio Device Provisioning in hub IoT possa registrare i dispositivi nell'hub. Il servizio può eseguire il provisioning dei dispositivi solo negli hub IoT che sono stati collegati al servizio Device Provisioning. Seguire questa procedura.

1.  Nella pagina **Tutte le risorse** nel portale di Azure fare clic sull'istanza del servizio Device Provisioning creata in precedenza.

2.  Nella pagina Servizio Device Provisioning fare clic su **Hub IoT collegati**.

3.  Fare clic su **Aggiungi**.

4.  Nella pagina **Aggiungi collegamento all'hub IoT** fornire le informazioni seguenti e fare clic su **Salva**:

    - **Sottoscrizione**: assicurarsi di selezionare la sottoscrizione che contiene l'hub IoT. È possibile aggiungere un collegamento all'hub IoT presente in un'altra sottoscrizione.

    - **Hub IoT:**  scegliere il nome dell'hub IoT che si vuole collegare con la nuova istanza del servizio Device Provisioning.

    - **Criteri di accesso:** selezionare **iothubowner** come credenziali da usare per stabilire il collegamento con l'hub IoT.

![Collegare il nome dell'hub da associare a DPS nel portale](../media/ee6e78754a1d39d86de71fb6872723f3.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Impostare criteri di allocazione per il servizio Device Provisioning

I criteri di allocazione rappresentano un'impostazione del servizio Device Provisioning in hub IoT che determina la modalità di assegnazione dei dispositivi a un hub IoT. Sono supportati tre criteri di allocazione:

1. **Lowest latency** (Latenza minima): il provisioning dei dispositivi viene eseguito in un hub IoT in base all'hub con latenza minima per il dispositivo.

2. **Distribuzione ponderata in modo uniforme**: gli hub IoT collegati hanno le stesse probabilità di ricevere dispositivi sottoposti a provisioning. Questa è l'impostazione predefinita. Se si esegue il provisioning dei dispositivi per un solo hub IoT, è possibile mantenere questa impostazione.

3. **Static configuration via the enrollment list** (Configurazione statica tramite l'elenco di registrazione): la specifica dell'hub IoT desiderato nell'elenco di registrazione ha priorità rispetto ai criteri di allocazione a livello del servizio Device Provisioning.

Per impostare i criteri di allocazione, nella pagina Device Provisioning Service (Servizio Device Provisioning) fare clic su **Manage allocation policy** (Gestisci criteri di allocazione). Verificare che i criteri di allocazione siano impostati su **Evenly weighted distribution** (Distribuzione ponderata uniforme) (impostazione predefinita). Al termine, se si apportano modifiche, fare clic su **Save** (Salva).

![Gestire i criteri di allocazione](../media/0c5fa5193156f17b4f5d64aab65a414d.png)

## <a name="enroll-the-device"></a>Registrare il dispositivo

Questo passaggio prevede l'aggiunta di elementi di sicurezza esclusivi del dispositivo al servizio Device Provisioning. Questi elementi di sicurezza si basano sul [meccanismo di attestazione](https://docs.microsoft.com/azure/iot-dps/concepts-device#attestation-mechanism) del dispositivo, come indicato di seguito:

Per i dispositivi basati su TPM sono necessari:

- *Chiave di verifica dell'autenticità*, che è univoca per ogni chip TPM o simulazione e che deve essere richiesta al produttore del chip TPM. Per altre informazioni leggere [Informazioni sulla chiave di verifica dell'autenticità del TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation#terminology).

- *ID di registrazione*, usato per identificare in modo univoco un dispositivo nell'ambito o nello spazio dei nomi. L'ID può coincidere o meno con l'ID dispositivo. L'ID è obbligatorio per ogni dispositivo. Per i dispositivi basati su TPM, l'ID di registrazione può essere derivato dal TPM stesso, ad esempio un hash SHA-256 della chiave di verifica dell'autenticità del TPM.

![Informazioni di registrazione per TPM nel portale](../media/11db90b7128e1cf222a4da45de7cbac8.png)

Per i dispositivi basati su X.509 sono necessari:

- [Certificato emesso nel chip X.509](https://docs.microsoft.com/windows/desktop/SecCertEnroll/about-x-509-public-key-certificates) o nella simulazione, sotto forma di file con estensione *pem* o *cer*. Per la registrazione singola è necessario usare il *certificato del firmatario* per dispositivo per il sistema X.509, mentre per i gruppi di registrazione è necessario usare il *certificato radice*.

   ![Aggiungere una registrazione singola per l'attestazione X.509 nel portale](../media/8d56752f453f27e55dd15b7c894ae406.png)

È possibile registrare il dispositivo nel servizio Device Provisioning in due modi:

- **Enrollment Groups** (Gruppi di registrazione) Rappresenta un gruppo di dispositivi che condividono un meccanismo di attestazione specifico. È consigliabile usare un gruppo di registrazione per un numero elevato di dispositivi che condividono una configurazione iniziale desiderata o per i dispositivi destinati allo stesso tenant. Per altre informazioni sull'attestazione dell'identità per i gruppi di registrazione, vedere [Sicurezza](https://docs.microsoft.com/azure/iot-dps/concepts-security#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

   ![Aggiungere una registrazione di gruppo per l'attestazione X.509 nel portale](../media/4a9d9ea822887c70f1ff1e4b64b138f1.png)

- **Registrazioni singole** Rappresenta una voce per un singolo dispositivo che può eseguire la registrazione per il servizio Device Provisioning. Le registrazioni singole possono usare certificati X.509 o token di firma di accesso condiviso (in un TPM reale o virtuale) come meccanismo di attestazione. È consigliabile usare le registrazioni singole per i dispositivi che richiedono configurazioni iniziali univoche e per i dispositivi che possono usare solo token di firma di accesso condiviso tramite TPM o TPM virtuale come meccanismo di attestazione. In caso di registrazione individuale è possibile specificare l'ID dispositivo dell'hub IoT desiderato.

Ora si registra il dispositivo nell'istanza del servizio Device Provisioning, usando gli elementi di sicurezza necessari in base al meccanismo di attestazione del dispositivo:

1. Accedere al portale di Azure, fare clic sul pulsante **Tutte le risorse** nel menu a sinistra e aprire il servizio Device Provisioning.

2. Nel pannello di riepilogo del servizio Device Provisioning selezionare **Gestisci registrazioni**. Selezionare la scheda **Individual Enrollments** (Registrazioni individuali) o **Enrollment Groups** (Gruppi di registrazione) in base alla configurazione del dispositivo. Fare clic sul pulsante **Aggiungi** in alto. Selezionare **TPM** o **X.509** come *meccanismo* di attestazione dell'identità e immettere gli elementi di sicurezza appropriati come descritto in precedenza. È possibile immettere un nuovo **ID di dispositivo hub IoT**. Al termine, fare clic sul pulsante **Salva**.

3. Quando la registrazione è completata, il dispositivo verrà visualizzato nel portale come riportato di seguito:

![Successful TPM enrollment in the portal (Registrazione TPM nel portale riuscita)](../media/cb277b2e5bc21cd02669775d536e89c0.png)

Dopo la registrazione, il servizio di provisioning attende che il dispositivo venga avviato per potersi connettere successivamente. Quando il dispositivo viene avviato per la prima volta, la libreria SDK del client interagisce con il chip per estrarre gli elementi di sicurezza dal dispositivo e verifica la registrazione con il servizio Device Provisioning.

## <a name="start-the-iot-device"></a>Avviare il dispositivo IoT

Il dispositivo IoT può essere un dispositivo reale o un dispositivo simulato. Dato che il dispositivo IoT a questo punto è stato registrato in un'istanza del servizio Device Provisioning, il dispositivo può ora essere avviato e chiamare il servizio di provisioning per ottenere il riconoscimento tramite il meccanismo di attestazione. Dopo il riconoscimento nel servizio di provisioning, il dispositivo verrà assegnato a un hub IoT.

Sono inclusi esempi di dispositivo simulato, che usano sia l'attestazione TPM che X.509, per C, Java, C\#, Node.js e Python. Ad esempio, un dispositivo simulato che usa TPM e [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) seguirà il processo descritto nella sezione [Simulare la sequenza del primo avvio per il dispositivo](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device#simulate-first-boot-sequence-for-the-device). Lo stesso dispositivo con l'attestazione con certificato X.509 farebbe riferimento a questa sezione per la [sequenza di avvio](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509#simulate-first-boot-sequence-for-the-device).

Vedere la [guida pratica per MXChip Iot DevKit](https://docs.microsoft.com/azure/iot-dps/how-to-connect-mxchip-iot-devkit) come esempio per un dispositivo reale.

Avviare il dispositivo per consentire all'applicazione client del dispositivo di eseguire la registrazione nel servizio Device Provisioning.

## <a name="verify-the-device-is-registered"></a>Verificare che il dispositivo sia registrato

In seguito all'avvio del dispositivo dovrebbero verificarsi le azioni seguenti:

1. Il dispositivo invia una richiesta di registrazione al servizio Device Provisioning.

2. Per i dispositivi TPM il servizio Device Provisioning invia di nuovo una richiesta di registrazione a cui risponde il dispositivo.

3. Al termine della registrazione il servizio Device Provisioning invia l'URI dell'hub IoT, l'ID dispositivo e la chiave crittografata di nuovo al dispositivo.

4. L'applicazione client dell'hub IoT nel dispositivo si connette quindi all'hub.

5. Se la connessione all'hub riesce, il dispositivo dovrebbe essere visualizzato nel riquadro di esplorazione **Dispositivi IoT** dell'hub IoT.

![Connessione all'hub nel portale riuscita](../media/12ea6da6eef9bf96be6bd80aa1721173.png)

<!--Reference links

-   <https://docs.microsoft.com/azure/iot-dps/tutorial-set-up-cloud>

-   <https://docs.microsoft.com/azure/iot-dps/tutorial-provision-device-to-hub>-->