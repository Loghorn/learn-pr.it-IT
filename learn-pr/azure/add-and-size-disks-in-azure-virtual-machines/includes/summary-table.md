
La tabella seguente illustra quali dischi sono disponibili come dischi gestiti e non gestiti.

|Tipo di disco  |Non gestito  | Gestita  | Account di archiviazione per non gestito|
|---------|---------|---------|---------|
|[Unità disco rigido standard](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)      |   Sì      |  Sì       |  account di archiviazione generale       |
|[Standard SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)    |   no      |   Sì      |  Non applicabile       |
|[Unità SSD Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)    |    Sì     |   Sì      |     Account di archiviazione Premium    |

Ecco un riepilogo dei compromessi apportate quando si seleziona un tipo di disco per le macchine virtuali.

|Tipo di disco  |IOPS relativo *  | Costo relativo  | Scenari|
|---------|---------|---------|---------|
|[Unità disco rigido standard](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)      |   più basso      |  più basso       |  Utilizzo per i carichi di lavoro di IOPs a basso che richiedono prestazioni coerenti e sviluppo conveniente e test       |
|[Standard SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)    |   Medium|   Medium      |  Utilizzo per i carichi di lavoro di IOPs a basso che richiedono prestazioni coerenti e sviluppo conveniente e test |
|[Unità SSD Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)    |    più alto     |   più alto      |     Utilizzo per la produzione e sensibili alle prestazioni dei carichi di lavoro in cui sono cruciali a bassa latenza e velocità effettiva elevata    |

* IOPS - operazioni di Input/output al secondo.