# Dark Factory - Idee future

Proposte fuori dalla spec corrente, da valutare dopo la prima implementazione.

## Pipeline

- **Pool invece di wave.** Merge appena un ticket è verde e avvio immediato del prossimo ticket pronto; wave-test quando il pool si svuota o ogni `parallel` merge. "Wave" resta il nome del batch tra due wave-test, bisect invariato. Evita che un ticket lento tenga fermo il merge degli altri.
- **Touch hint nei ticket.** Riga `**Touches:**` con i path probabili; l'engine evita nella stessa wave ticket con path in comune quando ha alternative pronte. Meno conflitti e bisect.
- **Riassunto dei merge per il worker.** L'engine mantiene `runs/<run>/merged.md` con id, titolo e `summary` di ogni ticket mergiato, nel prompt di implement e review.
- **Coda di spec.** `df run a b c` esegue più spec in sequenza, un lock per volta.

## Holdout

- **Baseline negativa.** A inizio run i replay della spec nuova girano su `main` e devono essere rossi. Un replay verde prima dell'implementazione non testa nulla: issue per holdout-gen (scenario vuoto). Controllo deterministico contro scenari banali.
- **Scenario negativo per AC.** Almeno un caso di errore o edge per ogni AC, come regola dell'oracle holdout.

## Config e controlli

- **Validazione di `df.yaml` sul principio 4.** `df run` rifiuta la config se worker e giudice di uno stesso ciclo (implement/review, implement/verify, tickets/oracle, holdout-gen/oracle) usano lo stesso model.
- **Stima di budget al gate.** AC x costo storico per AC da `df stats`, mostrata in `df accept` quando esistono dati.

## CLI

- **`df run --until <nodo>`.** Ferma al confine del nodo indicato con `paused`; `df resume` prosegue. Permette di vedere scomposizione e costo prima di spendere il budget.
- **`df replay <scenario>`.** Avvio tool e un solo replay contro il branch, evidenze in locale. Per capire un'escalation al mattino.
- **Notifica a fine run.** `notify: <comando>` in `df.yaml`, eseguito a `run.end` con il path del report.
