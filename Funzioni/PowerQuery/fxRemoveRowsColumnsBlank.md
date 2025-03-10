# fxRemoveRowsColumnsBlank

## Descrizione
Questa funziona, data una tabella di input, scansiona tutte le colonne e le righe.
Se trova colonne o righe completamente *null* (la stringa con uno spazio singolo è considerata *blank*) le elimina e restituisce una tabella senza righe o colonne completamente vuote. 

## Input
- Tabella: una tabella con righe o colonne vuote

## Output
- Tabella: una tabella senza righe o colonne vuote

## Esempio
A fronte di un input come questo:
| Data      |	Separatore  | Città         | SecondoSeparatore | Provincia |
| --------- | ------------- | ------------- | ----------------- | --------- |
| Compendio |               | 	            |                   |           |
|  	        |	            |               |                   |           |
|  	        |	            |               |                   |           |
| 5/4/2025  |               | Collegno      |                   | Torino    |
| 3/5/2025  |               Villanova d'Asti|                   | Asti      |

Dopo aver applicato la funzione `= fxRemoveRowsColumnsBlank(Source)` si ottiene:

| Data          | Città	            | Provincia |
| ------------- | ----------------- | --------- |
| Compendio     |                   |           | 
| 5/4/2025      | Collegno	        | Torino    |
| 3/5/2025      | Villanova d'Asti	| Asti      |



## Funzione

```sql
(Tabella as table) =>
// La funzione, a fronte di una tabella, verifica se ci sono righe o colonne completamente vuote.
// In tal caso le elimina e restituisce una tabella senza tali colonne/righe
// 
// INPUT => Una tabella
// OUTPUT => Una tabella senza colonne e righe vuote
let
    Source = Tabella,
    Colonne = Table.ColumnNames(Tabella),
    NumeroColonne = List.Count(Colonne)+1,
    Combina = List.Zip({
        Colonne,
        List.Repeat({Text.Trim},List.Count(Colonne)), // Si prevede la possibilità di riportare a null eventuali stringhe vuote come " " o "  " etc.
        List.Repeat({type text},List.Count(Colonne))
    }), 
    ReplaceStrings = Table.TransformColumns(Source,Combina),
    ReplaceBlanks = Table.ReplaceValue(
        ReplaceStrings,
        "",
        null,                                   // Si riporta a null le stringhe vuote
        Replacer.ReplaceValue,
        Table.ColumnNames(Source)),
    TogliColonneVuote = List.Select( 
            Table.ToColumns( ReplaceBlanks ), 
            each List.NonNullCount(_)  >0),
    ListaColonneEliminate = List.Transform(Table.ToColumns( ReplaceBlanks ), each List.NonNullCount(_) > 0),
    TitoliFinali = List.Last( 
        List.Generate(
            () => [x = 0, y = {}],
            each [x] < NumeroColonne,
            each [x = [x]+1, y = if ListaColonneEliminate{[x]} then [y] & {Colonne{[x]}} else [y]],
            each [y]
        )),
    AnnullaColonneVuote = Table.FromColumns( TogliColonneVuote, TitoliFinali ),
    AnnullaRigheVuote = Table.FromRows( 
        List.Select( 
            Table.ToRows( AnnullaColonneVuote ), 
            each List.NonNullCount(_)  >0),
        TitoliFinali )
in
    AnnullaRigheVuote
```