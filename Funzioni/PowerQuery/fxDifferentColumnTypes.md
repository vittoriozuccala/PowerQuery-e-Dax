# fxDifferentColumnTypes

## Descrizione
La funzione crea una nuova colonna in una tabella della quale analizza una o più colonne.
Per ciascuna colonna verifica, cella per cella, il tipo di dato contenuto e lo restituisce nella nuova colonna.

## Input
- Tabella: è la tabella dalla quale partire
- Colonne: è una lista di colonne delle quali prelevare il tipo dati

## Esempio

Partendo dalla seguente tabella:

```
= #table(
      {"Dato","Secondo Dato"},
      {
        {#date(2025,3,6),"Torino"},
        {null,null},
        {"Esempio",12},
        {22,66}
      }
)
```
Applicando la funzione `= fxDifferentColumnTypes(Source,{"Dato","Secondo Dato"})` si ottiene il seguente risultato:

| Dato	        | Secondo Dato	    | Tipo Dato	    | Tipo Secondo Dato |
| ------------- | ----------------- | ------------- | ----------------- |
| 06/03/2025	| Torino	        | Date.Type	    | Text.Type         |
| null	        | null	            | Null.Type	    | Null.Type         |
| Esempio	    | 12	            | Text.Type	    | Number.Type       |
| 22	        | 66	            | Number.Type	| Number.Type       |



## Funzione
```sql
// La funzione restituisce una tabella con una lista di colonne con il tipo dato per ciascuna cella
(Tabella as table, Colonne as list ) =>
let
    Risultato = List.Accumulate(
        Colonne,
        Tabella,
        (outTable, nextColumn) => 
        let 
            ColumnName = "Tipo " & nextColumn,
            AddTipoDato = Table.AddColumn(
                outTable,
                ColumnName,
                each Value.FromText(Value.Type(Record.Field(_,nextColumn)))
            ),
            TipoDatoToTableType = Table.TransformColumns(
                AddTipoDato, 
                {
                {
                    ColumnName, 
                    each type table [TipoDato = _]
                }
                }),
            TableSchemaOfTipoDato = Table.TransformColumns(
            TipoDatoToTableType,
            {{
                ColumnName,
                each Type.TableSchema(_)
            }}
            ),
            Result = Table.TransformColumns(TableSchemaOfTipoDato,{{ColumnName, each _[TypeName]{0},type text}})
    in
        Result
        )
in Risultato
```

