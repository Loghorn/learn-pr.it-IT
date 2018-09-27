Si supponga che i partner commerciali della società abbiano criteri di sicurezza che richiedono di proteggere i dati commerciali con la crittografia avanzata. Si usa un'applicazione B2B che viene eseguita nei server Windows e archivia i dati nel disco dati dei server. In questa fase di transizione al cloud è necessario dimostrare ai partner commerciali che i dati archiviati nelle macchine virtuali di Azure non sono accessibili a utenti, dispositivi o applicazioni non autorizzate. È necessario stabilire una strategia per l'implementazione della crittografia dei dati B2B.

I requisiti di controllo prevedono che le chiavi di crittografia siano gestite internamente e non da terze parti. Un'altra preoccupazione è legata alla necessità di mantenere le prestazioni e la gestibilità dei server basati su Azure. Prima di implementare la crittografia, è quindi opportuno assicurarsi che non si verificherà un calo delle prestazioni.

## <a name="what-is-encryption"></a>Che cos'è la crittografia?

La crittografia implica la conversione di informazioni importanti in qualcosa che non sembra importante, ad esempio una sequenza casuale di lettere e numeri. Il processo di crittografia usa qualche tipo di **chiave** come parte dell'algoritmo che crea i dati crittografati. Una chiave è necessaria anche per eseguire la decrittografia. Le chiavi possono essere **_simmetriche_**, se la stessa chiave viene usata per la crittografia e la decrittografia, o **_asimmetriche_**, se vengono usate chiavi diverse. Un esempio del secondo caso sono le coppie di chiavi **pubbliche-private** usate nei certificati digitali.

### <a name="symmetric-encryption"></a>Crittografia simmetrica

Gli algoritmi che usano le chiavi simmetriche, ad esempio Advanced Encryption Standard (AES), sono in genere più veloci rispetto agli algoritmi a chiave pubblica e vengono spesso usati per la protezione di archivi dati di grandi dimensioni. Poiché è presente solo una chiave, devono essere applicate procedure per impedire che la chiave diventi nota pubblicamente.

### <a name="asymmetric-encryption"></a>Crittografia asimmetrica

Con gli algoritmi asimmetrici, solo il membro chiave privata della coppia deve restare privato e sicuro. Come suggerisce il nome, la chiave pubblica può essere resa disponibile a chiunque senza compromettere i dati crittografati. Lo svantaggio degli algoritmi a chiave pubblica, tuttavia, è che sono molto più lenti rispetto agli algoritmi simmetrici e non possono essere usati per crittografare grandi quantità di dati.

## <a name="key-management"></a>Gestione delle chiavi

In Azure le chiavi di crittografia possono essere gestite da Microsoft oppure dal cliente. Spesso la richiesta di chiavi gestite dal cliente proviene da organizzazioni che devono dimostrare la conformità a HIPAA o ad altre normative. Tale conformità può richiedere che venga registrato l'accesso alle chiavi e che le normali modifiche alle chiavi vengano apportate e registrate.

## <a name="azure-disk-encryption-technologies"></a>Tecnologie di crittografia dischi di Azure

Le principali tecnologie di protezione dei dischi basate sulla crittografia per le macchine virtuali di Azure sono:

- Crittografia del servizio di archiviazione
- Crittografia dischi di Azure

### <a name="storage-service-encryption"></a>Crittografia del servizio di archiviazione

Crittografia del servizio di archiviazione di Azure è un servizio di crittografia incorporato in Azure usato per proteggere i dati inattivi. La piattaforma di archiviazione Azure crittografa automaticamente i dati prima che vengano archiviati in diversi servizi di archiviazione, tra cui Azure Managed Disks. La crittografia è abilitata per impostazione predefinita tramite la crittografia AES a 256 bit e viene gestita dall'amministratore account di archiviazione.

### <a name="azure-disk-encryption"></a>Crittografia dischi di Azure

Crittografia dischi di Azure viene gestita dal proprietario della macchina virtuale. Controlla la crittografia dei dischi controllati dalle macchine virtuali Windows e Linux, usando **BitLocker** nelle macchine virtuali Windows e **DM-Crypt** nelle macchine virtuali Linux. Crittografia unità BitLocker è una funzionalità di protezione dei dati che si integra con il sistema operativo e risponde alle minacce di furto o esposizione dei dati da computer persi, rubati o per i quali sono state rimosse le autorizzazioni in modo inappropriato. Analogamente, DM-Crypt crittografa i dati inattivi per Linux prima che vengano scritti nella risorsa di archiviazione.

Crittografia dischi di Azure assicura che tutti i dati nei dischi delle macchine virtuali vengano crittografati nella risorsa di archiviazione di Azure quando sono inattivi ed è obbligatoria per le macchine virtuali di cui viene eseguito un backup nell'insieme di credenziali di ripristino.

Con Crittografia dischi di Azure, le macchine virtuali vengono avviate con chiavi e criteri controllati dai clienti. Crittografia dischi di Azure è integrata con Azure Key Vault per la gestione delle chiavi e dei segreti di crittografia dei dischi.

> [!NOTE] 
> Crittografia dischi di Azure non supporta la crittografia delle macchine virtuali del livello Basic e non consente di usare un servizio di gestione delle chiavi locale.

## <a name="when-to-use-encryption"></a>Quando usare la crittografia?

I dati del computer sono a rischio quando sono in transito (trasmessi tramite Internet o un'altra rete) e quando sono inattivi (salvati in un dispositivo di archiviazione). Lo scenario dei dati inattivi è quello più preoccupante quando si proteggono i dati nei dischi delle macchine virtuali di Azure. Un utente potrebbe, ad esempio, scaricare il file disco rigido virtuale (VHD) associato a una macchina virtuale di Azure e salvarlo sul portatile. Se il disco rigido virtuale non è crittografato, il contenuto del disco rigido virtuale è potenzialmente accessibile a chiunque possa montare il file VHD nel proprio computer.

Per i dischi del sistema operativo, i dati come le password vengono crittografati automaticamente, quindi anche se il disco rigido virtuale non è crittografato, non è facile accedere a tali informazioni. Anche le applicazioni possono crittografare automaticamente i dati. Anche con tali protezioni, tuttavia, se un utente malintenzionato dovesse ottenere l'accesso a un disco dati e il disco non fosse crittografato, potrebbe riuscire a sfruttare eventuali punti di debolezza noti nella protezione dei dati dell'applicazione. Con la crittografia dei dischi, tali exploit non sono invece possibili.

La crittografia del servizio di archiviazione fa parte di Azure e, quando viene usata, non vi sarà alcun impatto significativo sulle prestazioni di I/O del disco della macchina virtuale. I dischi gestiti con la crittografia del servizio di archiviazione sono ora l'impostazione predefinita e non dovrebbe esserci motivo per modificarli. Crittografia dischi di Azure usa gli strumenti del sistema operativo della macchina virtuale (BitLocker e DM-Crypt) e quindi la macchina virtuale stessa deve effettuare alcune operazioni quando viene eseguita la crittografia o la decrittografia nei dischi della macchina virtuale. L'impatto di questa attività aggiuntiva della CPU della macchina virtuale è in genere trascurabile, tranne che in determinate situazioni. Se ad esempio si ha un'applicazione con utilizzo elevato di CPU, potrebbe essere necessario lasciare non crittografato il disco del sistema operativo per ottimizzare le prestazioni. In una situazione come questa è possibile archiviare i dati applicazione in un disco dati crittografato separato e ottenere così le prestazioni necessarie senza compromettere la sicurezza.

Azure offre due tecnologie di crittografia complementari usate per proteggere i dischi delle macchine virtuali di Azure. Queste tecnologie, Crittografia del servizio di archiviazione e Crittografia dischi di Azure, applicano la crittografia a livelli diversi e per scopi diversi. Entrambe usano la crittografia AES a 256 bit. L'uso di entrambe le tecnologie offre una protezione avanzata contro l'accesso non autorizzato alla risorsa di archiviazione di Azure e a dischi rigidi virtuali specifici.
