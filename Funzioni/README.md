# Funzioni
In questa sezione verranno salvate le funzioni che, di volta in volta, andrò a scrivere in ambito lavorativo o a seguito della risoluzione di un problema in ambito docenza.

- **Funzioni DAX**
  - [DataDictionary](./DAX/DataDictionary.md) Come creare un DataDictionary del modello semantico direttamente all'interno della dashboard
- **Funzioni PowerQuery**
  - **Date**
    - [fxCreateCalendar(DataInizio,DataFine)](./PowerQuery/fxCreateCalendar.md) Data una data iniziale ed una finale, crea un calendario completo.
    - [fxNumbersToDate(gg,mm,aa,"data")](./PowerQuery/fxNumbersToDate.md) Dati tre numeri, la funzione restituisce una data "gg/mm/aaaa". In caso uno dei campi abbia del testo o null o altro tipo di dato, la funzione restituisce "1/1/1900" o "errore" a discrezione dell'utente
  - **Utility**
    - [fxDifferentColumnType(Tabella, "campo")](./PowerQuery/fxDifferentColumnTypes.md) Quando una tabella ha una o più colonne con valori diversi, la funzione crea una nuova colonna con il tipo dati esplicitato per ogni cella.
    - [fxRemoveRowsColumnsBlank](./PowerQuery/fxRemoveRowsColumnsBlank.md) Nel caso in cui, in una tabella, ci siano righe o colonne vuote, si passa in input tale tabella e si ottiene una tabella senza colonne o righe vuote in output
    - [fxCleaningTextFields](./PowerQuery/fxCleaningTextFields.md) A fronte di una tabella ed una lista di campi testuali, elimina spazi intermedi e finali e caratteri non utilizzabili
  - **AS400**:
    - [fxFromAS400FieldsToTable](./PowerQuery/fxFromAS400FieldsToTable.md) Se viene stampato in PDF l'elenco dei campi di un file AS400 e lo si vuole riprodurre in una tabella Excel 
- **Funzioni Excel**
  - *Info Generali*
    - [Formato Numeri](./Excel/FormatoNumeri.md) Alcuni tips sui formati dei numeri
  - *Utility*
    - [ConfrontaDueTabelle](./Excel/ConfrontaDueTabelle.md) Verifica le differenze tra due tabelle o intervalli
    - [TrovaOccorrenze](./Excel/TrovaOccorrenze.md) Serve per trovare quante occorrenze ci sono in un elenco di alcune parole o frasi
  - *Finanziarie*
    - [SommatoriaCapitale](./Excel/SommatoriaCapitale.md) Funzione ricorsiva che somma il capitale per un finanziamento
    - [SommatoriaInteressi](./Excel/SommatoriaInteressi.md) Funzione ricorsiva che somma gli interessi per un finanziamento
    - [CalcoloTassiFinanziari](./Excel/CalcoloTassiFinanziari.md) Calcolo di TAN, TAEG, IRR
    - [NettoCliente](./Excel/NettoCliente.md) Netto Cliente
    - [CalcoloInteressi](./Excel/CalcoloInteressi.md) Interessi
    