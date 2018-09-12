È frequente l'esecuzione di alcune logiche a un intervallo impostato. Si supponga di essere un proprietario di un blog e di notare che i sottoscrittori non leggono i post più recenti. Si decide che l'azione più efficace consiste nell'inviare un messaggio di posta elettronica una volta a settimana per ricordare ai componenti di controllare il blog. La logica viene implementata usando una funzione di Azure con un _trigger timer_ per richiamare la funzione ogni settimana.

## <a name="what-is-a-timer-trigger"></a>Cos è un trigger timer?

Un trigger timer è un trigger che esegue una funzione a un intervallo costante. Per creare un trigger timer, è necessario specificare due tipi di informazioni.

1. Un *nome del parametro timestamp*, che è semplicemente un identificatore per accedere al trigger nel codice.
2. Una *Pianificazione*, ovvero un'*espressione CRON* che imposta l'intervallo del timer.

## <a name="what-is-a-cron-expression"></a>Cos'è un espressione CRON?

Un'*espressione CRON* è una stringa costituita da sei campi che rappresentano un insieme di tempi.

L'ordine dei sei campi in Azure è: `{second} {minute} {hour} {day} {month} {day of the week}`.

Ad esempio, un'*espressione CRON* per creare un trigger che viene eseguita ogni cinque minuti potrebbe essere:

```log
0 */5 * * * *
```

Inizialmente, questa stringa può generare confusione. Torneremo indietro ad analizzare questi concetti quando avremo approfondito le *espressioni CRON*.

Per creare un'*espressione CRON*, è necessario avere una conoscenza di base di alcuni caratteri speciali.

| Carattere speciale | Significato | Esempio |
| ------------- | ------------- | ------------- |
| *      | Consente di selezionare ogni valore in un campo | Un asterisco "*" nel campo del giorno della settimana campo significa *ogni* giorno. |
| ,      | Separa gli elementi in un elenco | Una virgola "1,3" nel campo del giorno della settimana significa semplicemente lunedì (giorno 1) e mercoledì (giorno 3). |
| -      | Specifica un intervallo | Un trattino "10-12" nel campo dell'ora indica un intervallo che include le ore 10, 11 e 12. |
| /      | Specifica un incremento | Una barra "*/10" nel campo minuti indica un incremento ogni 10 minuti. |

Ora torniamo all'esempio dell'espressione CRON originale. Proviamo a capirla meglio suddividendola campo per campo.

```log
0 */5 * * * *
```

Il **primo campo** rappresenta i secondi. Questo campo supporta i valori tra 0 e 59. Poiché il campo contiene il valore zero, viene selezionato il primo valore possibile, ovvero un secondo.

Il **secondo campo** rappresenta i minuti. Il valore "*/5" contiene due caratteri speciali. Per prima cosa, l'asterisco (\*) significa "seleziona ogni valore nel campo." Poiché questo campo rappresenta i minuti, i valori possibili sono da 0 a 59. Il secondo carattere speciale è la barra (/), che rappresenta un incremento. Quando si combinano insieme questi caratteri, ovvero per tutti i valori tra 0 e 59 è necessario selezionare ogni quinto valore. Un modo più semplice per segnalare che è semplicemente "ogni cinque minuti."

I **rimanenti quattro campi** rappresentano l'ora, il giorno, il mese e il giorno della settimana. Un asterisco per questi campi indica di selezionare ogni possibile valore. In questo esempio, selezioniamo "ogni ora di ogni giorno del mese."

Quando si inseriscono tutti i campi contemporaneamente, l'espressione viene letta come "al primo secondo, di ogni cinque minuti di ogni ora, di ogni giorno, di ogni mese".

## <a name="how-to-create-a-timer-trigger"></a>Come creare un trigger timer

È possibile creare un trigger timer interamente all'interno del portale di Azure. All'interno della funzione di Azure, selezionare **trigger timer** dall'elenco dei tipi di trigger predefiniti. Immettere la logica che si desidera eseguire. Specificare un **Nome del parametro Timestamp** e l'**espressione CRON**.

## <a name="summary"></a>Riepilogo

Creare un trigger timer per richiamare una funzione in una pianificazione coerente. Per definire la pianificazione di un trigger timer, creiamo un'*espressione CRON*, ovvero una stringa che rappresenta un insieme di tempi.
