Con un account Azure gratuito è possibile compilare, testare e distribuire applicazioni aziendali, creare esperienze personalizzate per il Web e per i dispositivi mobili e ricavare informazioni dettagliate dai dati grazie al Machine Learning e a potenti strumenti di analisi.

## <a name="what-is-an-azure-account"></a>Che cos'è un account Azure?

Un _account Azure_ è associato a un'identità specifica e contiene informazioni come:

- Nome, indirizzo di posta elettronica e preferenze di contatto
- Informazioni di fatturazione, ad esempio una carta di credito

Un account Azure è associato a una o più _sottoscrizioni_.

## <a name="what-is-an-azure-subscription"></a>Informazioni sulla sottoscrizione di Azure

Una _sottoscrizione di Azure_ è un contenitore logico usato per il provisioning delle risorse in Microsoft Azure. Contiene i dettagli relativi a tutte le risorse, ad esempio macchine virtuali, database e così via. Ha anche una relazione di trust con un singolo _tenant_ di Azure AD che viene usata per autenticare utenti e ruoli per le risorse contenute nella sottoscrizione.

La fatturazione avviene al livello della sottoscrizione. È possibile impostare limiti di spesa su ogni sottoscrizione per evitare brutte sorprese alla fine del mese. 

## <a name="what-is-an-azure-ad-tenant"></a>Che cos'è un tenant di Azure AD?

Azure AD (Azure Active Directory) è un provider di identità moderno che supporta più protocolli di autenticazione per la protezione di applicazioni e servizi nel cloud. _Non_ corrisponde a Windows Active Directory, il cui scopo principale è proteggere i desktop e i server Windows. Azure AD si basa invece sugli standard dell'autenticazione basata sul Web, come OpenID e OAuth.

Un singolo tenant rappresenta un'organizzazione logica e consente a più identità si accedere a e utilizzare risorse protette da tale tenant. Una sottoscrizione di Azure ha sempre una relazione di trust con un _singolo_ tenant di Azure AD, ma _più_ sottoscrizioni possono condividere un singolo tenant. Questa struttura consente all'organizzazione di gestire più sottoscrizioni e di impostare regole di sicurezza per tutte le risorse in esse contenute.

Ecco una semplice rappresentazione di account, sottoscrizioni, tenant e risorse.

![Diagramma che mostra l'interazione tra account, tenant, sottoscrizioni e risorse](../media/3-azure-ad-tenant.png)

Si noti che ogni tenant di Azure AD ha un _proprietario dell'account_, che corrisponde all'account Azure originale responsabile della fatturazione. È possibile aggiungere altri utenti al tenant e persino invitare guest di altri tenant di Azure AD ad accedere alle risorse contenute nelle sottoscrizioni.

## <a name="azure-account-types"></a>Tipi di account Azure

Azure offre diversi tipi di account adatti ai diversi tipi di clienti. Gli account usati più di frequente sono:

- Gratuito
- Pagamento in base al consumo
- Contratto Enterprise

### <a name="azure-free-account"></a>Account Azure gratuito

Un account Azure gratuito include **€ 170 di credito** da spendere nei primi 30 giorni, l'accesso gratuito ai prodotti di Azure più diffusi per 12 mesi e l'accesso a più di 25 prodotti che sono sempre gratuiti. Questo account rappresenta un'ottima soluzione per i nuovi utenti che vogliono iniziare a usare Azure. Per configurare un account gratuito sono necessari un numero di telefono, una carta di credito e un account Microsoft.

> [!NOTE]
> I dati della carta di credito vengono usati esclusivamente a scopo di verifica dell'identità. Non verrà addebitato alcun servizio finché non si esegue l'aggiornamento.

### <a name="azure-pay-as-you-go-account"></a>Account Azure con pagamento in base al consumo

Un account con pagamento in base al consumo (PAYG) prevede la fatturazione mensile per i servizi usati. Questo tipo di account è adatto a un'ampia gamma di utenti, dai singoli utenti alle aziende di piccole dimensioni fino alle organizzazioni molto grandi.

### <a name="azure-enterprise-agreement"></a>Contratto Enterprise per Azure

Un contratto Enterprise offre la flessibilità necessaria per acquistare servizi cloud e licenze software in base a un unico contratto con sconti per nuove licenze e Software Assurance. È destinato alle organizzazioni di livello aziendale.

## <a name="summary"></a>Riepilogo

Singoli utenti, aziende di piccole dimensioni o grandi organizzazioni, devono tutti disporre di un account per poter usare i servizi di Azure. La procedura standard è quella di iniziare con un account gratuito per poter valutare i servizi di Azure. Al termine del periodo di valutazione, si convertirà l'account gratuito in un account con pagamento in base al consumo.