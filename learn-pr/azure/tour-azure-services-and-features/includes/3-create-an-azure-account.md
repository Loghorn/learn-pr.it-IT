Con un account Azure gratuito è possibile compilare, testare e distribuire applicazioni aziendali, creare esperienze personalizzate per il Web e per i dispositivi mobili e ricavare informazioni dettagliate dai dati grazie al Machine Learning e a potenti strumenti di analisi.

## <a name="what-is-an-azure-account"></a>Che cos'è un account Azure?

Un _account Azure_ è associato a un'identità specifica e contiene informazioni come:

- Nome, indirizzo di posta elettronica e preferenze di contatto
- Informazioni di fatturazione, ad esempio una carta di credito

L' account Azure serve per accedere al portale di Azure o all'interfaccia della riga di comando di Azure. Ogni account Azure è associato a una o più _sottoscrizioni_.

## <a name="what-is-an-azure-subscription"></a>Cos'è una sottoscrizione di Azure?

Una _sottoscrizione di Azure_ è un contenitore logico usato per il provisioning delle risorse in Microsoft Azure. Contiene i dettagli relativi a tutte le risorse, ad esempio macchine virtuali, database e così via.

La fatturazione avviene a livello di sottoscrizione &mdash; su base mensile viene generata una fattura per ogni sottoscrizione di Azure. È possibile impostare limiti di spesa su ogni sottoscrizione per evitare brutte sorprese alla fine del mese.

## <a name="what-is-an-azure-ad-tenant"></a>Che cos'è un tenant di Azure AD?

Azure AD (Azure Active Directory) è un provider di identità moderno che supporta più protocolli di autenticazione per la protezione di applicazioni e servizi nel cloud. _Non_ corrisponde a Active Directory di Windows, il cui scopo principale è proteggere i desktop e i server Windows. Azure AD si basa invece sugli standard dell'autenticazione basata sul Web, come OpenID e OAuth.

Gli utenti, le applicazioni e altre entità registrate in Azure AD non sono tutti raggruppati in un singolo servizio globale. Azure AD è infatti suddiviso in _tenant_ separati. Un tenant è un'istanza isolata e dedicata del servizio Azure Active Directory, di proprietà e gestito da un'organizzazione.

Quando si parla di tenant di Azure AD, non esiste una definizione concreta di "organizzazione" &mdash; i tenant possono appartenere a singoli utenti, team, aziende o qualsiasi altro gruppo di persone. I tenant sono normalmente associati alle aziende. Se ci si iscrive ad Azure con un indirizzo e-mail non associato a un tenant esistente, il processo di iscrizione guiderà l'utente nella creazione di un tenant di proprietà.

> [!NOTE]
> L'indirizzo e-mail che si usa per accedere ad Azure può essere associato a più di un tenant. Questo si può notare se si ha un proprio account Azure e si usa il sandbox di Azure di Microsoft Learn per completare gli esercizi. Nel portale di Azure è possibile visualizzare solo le risorse appartenenti a un unico tenant alla volta. Per cambiare il tenant di cui si stanno visualizzando le risorse, selezionare l'icona **a forma di libro e filtro** nella parte superiore del portale e scegliere un tenant diverso nella sezione **Cambia directory**.

I tenant e le sottoscrizioni di Azure AD hanno una relazione di trust molti-a-uno, vale a dire che un tenant può essere associato a più sottoscrizioni di Azure, ma ogni sottoscrizione è associata a un solo tenant. Questa struttura consente alle organizzazioni di gestire più sottoscrizioni e di impostare regole di sicurezza per tutte le risorse in esse contenute.

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