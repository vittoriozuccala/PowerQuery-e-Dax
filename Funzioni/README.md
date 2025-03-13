# Funzioni
In questa sezione verranno salvate le funzioni che, di volta in volta, andrò a scrivere in ambito lavorativo o a seguito della risoluzione di un problema in ambito docenza.


- **Funzioni PowerQuery**
  - **Date**
    - [fxCreateCalendar(DataInizio,DataFine)](./PowerQuery/fxCreateCalendar.md) Data una data iniziale ed una finale, crea un calendario completo.
    - [fxNumbersToDate(gg,mm,aa,"data")](./PowerQuery/fxNumbersToDate.md) Dati tre numeri, la funzione restituisce una data "gg/mm/aaaa". In caso uno dei campi abbia del testo o null o altro tipo di dato, la funzione restituisce "1/1/1900" o "errore" a discrezione dell'utente
  - **Utility**
    - [fxDifferentColumnType(Tabella, "campo")](./PowerQuery/fxDifferentColumnTypes.md) Quando una tabella ha una o più colonne con valori diversi, la funzione crea una nuova colonna con il tipo dati esplicitato per ogni cella.
    - [fxRemoveRowsColumnsBlank](./PowerQuery/fxRemoveRowsColumnsBlank.md) Nel caso in cui, in una tabella, ci siano righe o colonne vuote, si passa in input tale tabella e si ottiene una tabella senza colonne o righe vuote in output