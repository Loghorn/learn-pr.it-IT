Si supponga che i partner commerciali della società abbiano criteri di sicurezza che richiedono di proteggere i dati commerciali con la crittografia avanzata. Si usa un'applicazione B2B che viene eseguita nei server Windows e archivia i dati nel disco dati dei server. In questa fase di transizione al cloud è necessario dimostrare ai partner commerciali che i dati archiviati nelle macchine virtuali di Azure non sono accessibili a utenti, dispositivi o applicazioni non autorizzate. È necessario stabilire una strategia per l'implementazione della crittografia dei dati B2B.

I requisiti di controllo prevedono che le chiavi di crittografia deve essere gestito internamente e non da terze parti. Un'altra preoccupazione è legata alla necessità di mantenere le prestazioni e la gestibilità dei server basati su Azure. Prima di implementare la crittografia, è quindi opportuno assicurarsi che non si verificherà un calo delle prestazioni.

## <a name="what-is-encryption"></a>Che cos'è la crittografia?

La crittografia implica la conversione di informazioni importanti in qualcosa che non sembra importante, ad esempio una sequenza casuale di lettere e numeri. Il processo di crittografia utilizza qualche forma di **chiave** come parte dell'algoritmo che crea i dati crittografati. È anche necessaria una chiave per eseguire la decrittografia. Le chiavi possono essere  **_simmetrica_**, in cui la stessa chiave viene usata per la crittografia e decrittografia, o  **_asimmetrica_**, in cui sono chiavi diverse utilizzato. Un esempio di quest'ultimo è il **pubblica-privata** utilizzate nei certificati digitali coppie di chiavi.

### <a name="symmetric-encryption"></a>Crittografia simmetrica

Gli algoritmi che usano le chiavi simmetriche, ad esempio Advanced Encryption Standard (AES), sono in genere più veloci rispetto agli algoritmi a chiave pubblica e vengono spesso usati per la protezione di archivi dati di grandi dimensioni. Poiché è presente solo una chiave, devono essere applicate procedure per impedire che la chiave diventi nota pubblicamente.

### <a name="asymmetric-encryption"></a>Crittografia asimmetrica

Con gli algoritmi asimmetrici, solo il membro chiave privata della coppia deve restare privato e sicuro. Come suggerisce il nome, la chiave pubblica può essere resa disponibile a chiunque senza compromettere i dati crittografati. Lo svantaggio degli algoritmi a chiave pubblica, tuttavia, è che sono molto più lenti rispetto agli algoritmi simmetrici e non possono essere usati per crittografare grandi quantità di dati.

## <a name="key-management"></a>Gestione delle chiavi

In Azure, le chiavi di crittografia che possono essere gestite da Microsoft o al cliente. Spesso la domanda di chiavi gestite dal cliente proviene da organizzazioni che devono dimostrare la conformità a HIPAA o ad altre normative. Tale conformità può richiedere che venga registrato l'accesso alle chiavi e che le normali modifiche alle chiavi vengano apportate e registrate.

## <a name="azure-disk-encryption-technologies"></a>Tecnologie di crittografia dischi di Azure

Le principali tecnologie di protezione dei dischi basate sulla crittografia per le macchine virtuali di Azure sono:

- Crittografia del servizio di archiviazione
- Crittografia dischi di Azure

### <a name="storage-service-encryption"></a>Crittografia del servizio di archiviazione

Azure Storage Service Encryption (SSE) è un servizio di crittografia incorporato in Azure che usato per proteggere i dati inattivi. La piattaforma di archiviazione di Azure crittografa automaticamente i dati prima di essere archiviato a diversi servizi di archiviazione, tra cui Azure Managed Disks. La crittografia è abilitata per impostazione predefinita tramite la crittografia AES a 256 bit e viene gestita dall'amministratore dell'account di archiviazione.

### <a name="azure-disk-encryption"></a>Crittografia dischi di Azure

Azure crittografia dischi di viene gestita dal proprietario della macchina virtuale. Controlla la crittografia di Windows e i dischi inclusi nel controllo della macchina virtuale Linux, usando **BitLocker** nelle macchine virtuali Windows e **DM-Crypt** nelle macchine virtuali Linux. Crittografia unità BitLocker è una funzionalità di protezione dei dati che si integra con il sistema operativo e gli indirizzi i rischi di furto dei dati o dalla perdita, furto o rimozione inappropriata delle autorizzazioni di computer. Analogamente, DM-Crypt crittografa i dati inattivi per Linux prima che vengano scritti nella risorsa di archiviazione.

Crittografia dischi di Azure assicura che tutti i dati nei dischi delle macchine virtuali vengano crittografati nella risorsa di archiviazione di Azure quando sono inattivi ed è obbligatorio per le macchine virtuali di cui viene eseguito un backup nell'insieme di credenziali di ripristino.

Con ADE, le macchine virtuali avviate con chiavi controllate dai clienti e i criteri. Crittografia dischi di Azure è integrata con Azure Key Vault per la gestione delle chiavi e dei segreti di crittografia dei dischi.

> [!NOTE] 
> ADE non supporta la crittografia delle macchine virtuali di livello base e non è possibile usare un locale del servizio KMS (Key Management) con ADE.

## <a name="when-to-use-encryption"></a>Quando usare la crittografia?

I dati del computer sono a rischio quando sono in transito (trasmessi tramite Internet o un'altra rete) e quando sono inattivi (salvati in un dispositivo di archiviazione). Lo scenario dei dati inattivi è quello più preoccupante quando si proteggono i dati nei dischi delle macchine virtuali di Azure. Un utente potrebbe, ad esempio, scaricare il file disco rigido virtuale (VHD) associato a una macchina virtuale di Azure e salvarlo sul portatile. Se il disco rigido virtuale non è crittografato, il contenuto del disco rigido virtuale è potenzialmente accessibile a chiunque possa montare il file VHD nel proprio computer.

Per i dischi del sistema operativo, i dati come le password vengono crittografati automaticamente, quindi anche se il disco rigido virtuale non è crittografato, non è facile accedere a tali informazioni. Anche le applicazioni possono crittografare automaticamente i dati. Anche con tali protezioni, tuttavia, se un utente malintenzionato dovesse ottenere l'accesso a un disco dati e il disco non fosse crittografato, potrebbe riuscire a sfruttare eventuali punti di debolezza noti nella protezione dei dati dell'applicazione. Con la crittografia dei dischi, tali exploit non sono invece possibili.

Strumentazione gestione Windows (SSE, Storage Service Encryption) fa parte di Azure, e non vi sarà alcun impatto significativo sulle prestazioni del disco della macchina virtuale i/o quando si usa la crittografia lato server. I dischi gestiti con la crittografia SSE sono ora il valore predefinito e non deve essere presente alcun motivo per modificarlo. Crittografia dischi di Azure usa gli strumenti del sistema operativo della macchina virtuale (BitLocker e DM-Crypt) e quindi la macchina virtuale stessa deve effettuare alcune operazioni quando viene eseguita la crittografia o la decrittografia nei dischi della macchina virtuale. L'impatto di questa attività aggiuntiva della CPU della macchina virtuale è in genere trascurabile, tranne che in determinate situazioni. Ad esempio, se si dispone di un'applicazione a elevato utilizzo di CPU, potrebbe esserci un case serata tranquilla lontano dal disco del sistema operativo non crittografato per ottimizzare le prestazioni. In una situazione come questa è possibile archiviare i dati applicazione in un disco dati crittografato separato e ottenere così le prestazioni necessarie senza compromettere la sicurezza.

Azure offre due tecnologie di crittografia complementari usate per proteggere i dischi delle macchine virtuali di Azure. Queste tecnologie, SSE e ADE, crittografare nei diversi livelli e per scopi diversi. Entrambi usano la crittografia AES a 256 bit. L'uso di entrambe le tecnologie offre una protezione avanzata contro l'accesso non autorizzato alla risorsa di archiviazione di Azure e a dischi rigidi virtuali specifici.
