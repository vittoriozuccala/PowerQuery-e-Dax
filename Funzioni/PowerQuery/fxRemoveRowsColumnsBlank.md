# fxRemoveRowsColumnsBlank

## Descrizione
Questa funziona, data una tabella di input, scansiona tutte le colonne e le righe.
Se trova colonne o righe completamente *null* (la stringa con uno spazio singolo è considerata *blank*) le elimina e restituisce una tabella senza righe o colonne completamente vuote. 

## Input
- Tabella: una tabella

## Esempio



## Funzione

```sql
(Tabella as table) =>
let
    Source = Tabella,
    ReplaceBlanks = Table.ReplaceValue(Source," ",null,Replacer.ReplaceValue,Table.ColumnNames(Source)),
    AnnullaColonneVuote = Table.FromColumns( List.Select( Table.ToColumns( ReplaceBlanks ), each List.NonNullCount(_)  >0) ),
    AnnullaRigheVuote = Table.FromRows( List.Select( Table.ToRows( AnnullaColonneVuote ), each List.NonNullCount(_)  >0) )
    
in
    AnnullaRigheVuote

```